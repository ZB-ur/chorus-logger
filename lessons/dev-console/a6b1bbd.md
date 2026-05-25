---
sha: a6b1bbd69d99a862d04c5ab0e2876b2738b47bb0
repo: dev-console
date: 2026-05-18
subject: "docs: retrospective §7 cleanup outcome + §8 standing commitments"
extracted_at: 2026-05-25T19:41:56Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Archive real-data directories rather than deleting; pair every "only-if-X-happens" process step with a "do-by-date-Z regardless" deadline

## What happened
Two interleaved disciplines. (1) §7 cleanup: 3 dirs with real experiment data were ARCHIVED to `~/AiCoding/.archive/2026-04-detent/` (dotted-prefix skipped by the scanner); 4 dirs were DELETED only after confirming "0 user files" (one of them held only auto-generated `.claude/settings.local.json`). Post-cleanup smoke rose to 71.4% — still under 80% but honest. (2) §8 commits: the §6 reachability demo was made conditional on a triggering event, BUT also given an explicit deadline (2026-05-25) so the "condition never fires → demo never done" failure mode can't hide.

## Transferable lesson
Two paired habits. (1) Default to archive, not delete, for any directory that might hold real artifacts — dotted-prefix archive dirs satisfy "ignored by tooling" without losing data. Delete only what you've confirmed is empty or fully auto-generated. (2) Any conditional process step ("only do X if Y happens") needs a paired absolute deadline ("do X by date Z regardless"). Without it, the absence of trigger Y becomes a silent skip — and you never learn whether X actually works.

## Surface area
Next time you're cleaning up a directory tree, archive everything with bytes to a dotted-prefix archive dir and delete only confirmed-empty paths. Next time you write a conditional process step, add a "by date Z regardless" clause so non-triggering can't bypass it.
