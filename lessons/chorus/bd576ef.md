---
sha: bd576efdf610297be55b106d7eb08515a1ca2421
repo: chorus
date: 2026-05-22
subject: "refactor(chorus-start): delete F3/F6 dead /loop block; wire inject-loop.sh (v0.5 #7)"
extracted_at: 2026-05-25T16:43:09Z
verdict: keep
verdict_at: 2026-05-25T16:43:30Z
verdict_reason: null
---

# Lesson — Delete dead code with a grep-checkable acceptance line; use soft-fail aggregation for multi-actor startup

## What happened
The old inline `sleep 12 + send-keys /loop + Enter` block (empirically dead per F3/F6) was deleted and replaced with a per-role call to `lib/inject-loop.sh`. Acceptance includes a grep-checkable line: `grep -E 'sleep[[:space:]]+12|send-keys.*\/loop' bin/chorus-start.sh` returns empty. Per-role injection uses soft-fail aggregation: one role's failure (exit 5 or 6) doesn't stop other roles from spawning; `state/pids` is written BEFORE inject so `chorus-stop.sh` can clean up regardless; start.sh's own exit reflects ANY_INJECT_FAILED.

## Transferable lesson
Two pairings worth remembering. (1) When deleting a code path, write a grep that confirms it stays gone — a one-line CI/review hook that catches accidental resurrection. (2) For startup of N independent actors, soft-fail aggregation (continue spawning others, capture cleanup state first, surface partial-failure via exit code) is almost always the right shape; hard-fail on the first error leaves orphaned half-started processes.

## Surface area
Next time you delete a multi-line dead block, add the grep acceptance to the commit body. Next time you spawn N independent things in a loop, record cleanup-info before each step and aggregate failures instead of returning early.
