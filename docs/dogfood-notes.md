# Dogfood notes — chorus runtime under real workload

This is the friction log. Append observations as they happen; **do not
delete entries even if later resolved**. The whole point of this file is
to be input for chorus v0.7 brainstorm — preserved friction trace is more
valuable than tidy retrospective.

## Format per entry

```
## YYYY-MM-DD HH:MM — <one-line summary>

**Triggered by:** <what was running / what conductor did>
**Observed:** <what chorus did or failed to do>
**Expected:** <what would have been better>
**Severity:** <low | medium | high — affecting dogfood vs blocking>
**Surface area in chorus:** <which role / mechanism / spec section is relevant>
```

## Entries

<!-- New entries below; newest at TOP -->

## 2026-05-26 07:06 — ScheduleWakeup socket failure → role 永久死，无 retry

**Triggered by:** executor 完成第 24 batch（commit `02fcfe5 extract: 5 lessons (bonfire:5)`）后调用 ScheduleWakeup 安排下一次唤醒。
**Observed:** jsonl 44a99012 末三条:
```
07:05:22  assistant: "Pushed 02fcfe5. 120 lessons total. Rescheduling fallback."
07:06:27  synthetic: "API Error: The socket connection was closed unexpectedly."
07:06:27  system:    turn_duration 207018ms (3.5 min hung)
```
claude PID 76128 之后仍存活但完全无活动 — 不会重试、不会写 BLOCKED file、不会触发任何 watchdog。reviewer 在 13:55 CST（dark wake 期间）同样死法：`API Error: Unable to connect to API (FailedToOpenSocket)`。
**Expected:** ScheduleWakeup 失败应：(1) backoff retry 至少 3 次 (2) 仍失败则写 BLOCKED-schedule-wakeup-failed.md (3) 外部 watchdog 检测到死 role 后重新 chorus-start 该 role
**Severity:** high — 这是 chorus 单点故障，**任何一次 RPC 抖动都会永久杀掉 role**。比 5h quota cliff 严重得多（quota 当时只 < 30%）。
**Surface area in chorus:** `/loop` skill / Mode B 设计 / ScheduleWakeup 调用点（无包裹）/ 整个 keep-alive 机制；F7 lineage 第 4 个外部边界失败案例。

## 2026-05-26 07:06 — executor 死因：**纯 transient 网络/API 故障**（lid 当时仍开）

**Triggered by:** executor 调 ScheduleWakeup 的瞬间。**用户确认 07:05 时 lid 仍开**，08:28 才合盖。故 lid-close / dark-wake 假设对 executor 07:05 死亡**不适用**。
**Observed:** socket hung 3:27 min 后回 "socket connection closed unexpectedly"。pmset 07:06-07:07 的事件（caffeinate ClientDied / coreaudiod 释放 sleep-prevention 断言）只是常规闲置-prevention 调整，**不会导致网络中断**，时间上的重合是巧合。
**Expected:** chorus 不能假设 ScheduleWakeup RPC 总成功 — 即使 lid 开 + 系统正常运行也会发生 transient 失败。
**Severity:** **upgrade to high** — 比合盖场景更糟：意味着即使正常运行 chorus role 也随时可能 RPC 抖一下就死。reviewer 13:55 才是 dark-wake 场景（合盖后 5h27m）。两件事**根因相同（无 retry），触发场景不同**。
**Surface area in chorus:** 与 entry "ScheduleWakeup socket failure → role 永久死，无 retry" 同；这条只是把"为什么 07:05 偏偏死"从 pmset 假设修正为"任何时刻都可能发生的 transient 失败"。

## 2026-05-26 08:28+ — `chorus-status` "alive" verdict 不可信（僵尸 claude PID）

**Triggered by:** 4 个 claude PID（76128/78636/80570/82606）从 07:06/13:55 死后一直存活至 20:46+ 我重连。chorus-status 持续报告 `alive(<PID>)`。
**Observed:** verdict 只看 PID 存活，不看实际工作；last-emit 字段反而是真信号（59666s / 44315s ≈ 死亡时间）但 verdict 没用它。
**Expected:** verdict = `alive AND last-emit < 5min` 才算 OK；否则 `zombie`。或在 role 每次 /loop tick 顶部 `touch state/heartbeats/<role>.beat`，verdict 用 heartbeat mtime。
**Severity:** high — 用户/operator 看不出 role 死了，发现得越晚损失越大。
**Surface area in chorus:** `bin/chorus-status.sh` verdict 逻辑；role 缺 heartbeat artifact。

## 2026-05-26 (all night) — 无 log 目录，post-mortem 必须挖 jsonl

**Triggered by:** chorus-start.sh 启动 4 roles 时未 mkdir `state/logs/`。role stdout 全部丢失。
**Observed:** 想找 conductor-listener cron 是否曾 fire — `~/AiCoding/chorus/state/logs/` 不存在；只能用 `~/.claude/projects/.../*.jsonl` 反推。conductor-listener 是否 fire 过完全无证。
**Expected:** chorus-start 应 `mkdir -p state/logs/` 并把每个 role 的 stdout/stderr 重定向到 `state/logs/<role>.log`。
**Severity:** medium — 不阻塞，但 v0.7 brainstorm 的输入质量直接受限。
**Surface area in chorus:** `bin/chorus-start.sh`；role 启动后 IO 重定向。

## 2026-05-26 (all night) — mosaicat 一个 commit 都没处理（FIFO 串行调度饿死）

**Triggered by:** executor.md 队列是 4 个 repo 按顺序拼接 — chorus → dev-console → bonfire → mosaicat。
**Observed:** 7.5h 处理 chorus 57 + dev-console 20 + bonfire 43 = 120 / 697 (17%)。mosaicat 在队尾，**永远饿死**。bookmarks: mosaicat (pending) 0 processed。
**Expected:** 多 repo 应轮询（round-robin）或并行 — 5 commit chorus → 5 commit mosaicat → 5 commit dev-console …
**Severity:** medium — dogfood 范围内的输出不完整；transferable lesson 来源偏向"先到的"repo。
**Surface area in chorus:** compounding domain 的 executor role-prompt 设计；inbox 调度策略。

## 2026-05-26 (all night) — 30+ orphan monitor-inbox.sh bash 进程堆积

**Triggered by:** Monitor tool 每 ~28 min 重新 spawn 一个 `monitor-inbox.sh` 子进程，旧的不被回收。
**Observed:** `ps aux | grep monitor-inbox` 输出 25+ 个不同时间戳的孤儿（reviewer 17 个 + executor 8 个 + zsh wrapper 8 个）。每个 ≈ 2MB RSS，累积 ~50MB。
**Expected:** Monitor 父进程在 spawn 新 child 前应 SIGTERM 旧 child；或 child 自带 "exit on parent gone"。
**Severity:** low — 资源泄露但未致命；symptom of v0.6 Monitor lifecycle 设计漏洞。
**Surface area in chorus:** `lib/monitor-inbox.sh` 父-子进程关系；v0.6 Mode B spec 没显式约束。

## 2026-05-26 — verdict 91% keep 偏宽松（需要 summarizer 数据再验证）

**Triggered by:** 120 lessons → 109 keep / 11 skip。
**Observed:** reviewer 5 条 verdict 规则触发顺序：trivial / duplicate / project-noise / needs-conductor-judgment / otherwise keep。"otherwise keep" 兜底过宽，11 个 skip 全是 trivial（pattern repetition）— **没有一个 duplicate 或 project-noise**。
**Expected:** reviewer 应有更主动的"already-said-elsewhere"维度（跨 repo 跨 batch 去重）。
**Severity:** low — 数据可后期降噪（summarizer 聚类时归并）；但越早 filter 越省 reviewer 算力。
**Surface area in chorus:** `domains/compounding/role-prompts/reviewer.md` verdict 规则；可能 v0.7 需要"全局 lesson 记忆"。
