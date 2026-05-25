---
sha: acedff4581422c77bc3e2d2609befef5c56b467a
repo: chorus
date: 2026-05-20
subject: "docs: retrospective F6 + post-tag end-to-end verification"
extracted_at: 2026-05-25T16:09:45Z
verdict: keep
verdict_at: 2026-05-25T16:11:25Z
verdict_reason: null
---

# Lesson — Re-verify "fixed" symptoms from a cold fresh state before tagging

## What happened
After tagging v0, the conductor re-booted from the v0 tag on a fresh tmux server and reproduced the exact F3 Task-9 first-boot symptom that commit 328229b's "12s settle" claimed to fix. The "empirically sufficient" 12s was based on a single observation during active build; from cold start it still fails. Open Question #4 was reopened; an operational workaround documented.

## Transferable lesson
"Empirically sufficient" with N=1 is a guess wearing a lab coat. Any fix for a timing/flakiness bug must be re-verified at least once from a truly cold state (fresh process tree, fresh tmux server, fresh shell) — not from the warm dev state where it was discovered. The warm state often hides the original failure mode.

## Surface area
Next time you fix a flaky-boot or timing bug and want to tag a release, kill every related daemon and re-launch from scratch in a new shell before signing off.
