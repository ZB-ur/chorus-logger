---
sha: 4aa4df1a3fc29f54d3ba40397644edb8d0062643
repo: chorus
date: 2026-05-22
subject: "test(inject-loop): smoke Case 3 (Stage 1 timeout) + per-case extra-flags support (v0.5 #7)"
extracted_at: 2026-05-25T16:43:09Z
verdict: keep
verdict_at: 2026-05-25T16:43:30Z
verdict_reason: null
---

# Lesson — Timeouts in production code need a flag override so tests can shrink them

## What happened
A new smoke case exercises the Stage 1 timeout path. Default `--prompt-timeout=30s` would have made the test take 30s; running it with `--prompt-timeout=2s` cuts that 15x without changing production semantics. The test harness `run_case` gained an optional 5th arg for per-case extra inject-loop flags so future cases can shorten timeouts independently.

## Transferable lesson
Hard-coded timeouts in production paths make tests slow and discourage adding negative cases. The cheap fix: expose every timeout as a CLI flag (or env var) and default it to the production value. Tests pass a small override; production is untouched. The same idea applies to retry counts, poll intervals, and any "wait up to N" knob.

## Surface area
Next time you write code with a fixed timeout/retry/sleep value, expose it as a flag with the production value as default — your future timeout-path test will thank you.
