---
sha: bc84d3abd7e69cca075d4cbc0283b9c5e381d78b
repo: chorus
date: 2026-05-20
subject: "feat: bin/chorus-{start,stop}.sh"
extracted_at: 2026-05-25T15:38:30Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Track desired state and actual state in two separate artifacts

## What happened
`chorus-start.sh` parses `roles.yaml` (desired state — what should be running) and writes `state/pids` (actual state — what IS running, by PID captured right after launch). YAML parser tries PyYAML first, then regex fallback so the script runs without optional deps. tmux-kill-session was rejected upstream because it doesn't propagate to children — explicit `kill <pid>` only.

## Transferable lesson
For any system with "should be running" vs "is running" semantics, never fuse them into one file. Two files lets a health check diff them; one file silently drifts. Also: when you depend on a tool you don't fully trust to clean up children (tmux, docker, init systems), track child PIDs yourself and kill them directly.

## Surface area
Next time you wire up a process manager, ask: "where do I look to see what *should* be up, and where do I look to see what *is* up?" If it's the same place, you've lost the ability to detect drift.
