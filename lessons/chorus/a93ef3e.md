---
sha: a93ef3e3c62f288a019f4ea1bf91c285a8e127a7
repo: chorus
date: 2026-05-20
subject: "docs: tag-naming convention + F6 dead-code wording + backlog #8"
extracted_at: 2026-05-25T16:09:45Z
verdict: keep
verdict_at: 2026-05-25T16:11:25Z
verdict_reason: null
---

# Lesson — A feature that requires a workaround on every run is dead code, not a papercut

## What happened
Cycle-2 reviewer reframed F6: start.sh's `/loop` send-keys branch fires on every boot but its Enter is never interpreted as submit, so the operator types Enter manually anyway. The earlier "minor papercut" wording was replaced with "buggy feature masked by an operator workaround = effectively dead code." Also adopted: a tag-naming convention (no bare `v<MAJOR>`; all release tags semver-triplet) grandfathering existing v0/v0-build as one-time exceptions.

## Transferable lesson
If the user has to do the same manual step every time after your "automated" code runs, the code isn't doing the job — it's noise that hides the bug. Call it dead code in the doc so the next maintainer doesn't preserve it out of misplaced respect. For naming conventions adopted late, grandfather existing names rather than mass-renaming — destructive rewrites cost more than the inconsistency saves.

## Surface area
Next time you label a recurring-workaround situation as a "papercut," ask if the underlying code path is ever the one that actually completes the job. If not, file it as dead code.
