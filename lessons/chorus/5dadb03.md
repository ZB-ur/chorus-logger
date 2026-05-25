---
sha: 5dadb0367d58e927e85e65a18e5931336abe4173
repo: chorus
date: 2026-05-25
subject: "docs(runs): v0.6 build evidence + chorus-self prereq 3/5 closure (v0.6 scope)"
extracted_at: 2026-05-25T18:11:39Z
verdict: keep
verdict_at: 2026-05-25T18:12:00Z
verdict_reason: null
---

# Lesson — Sometimes a prereq closes by being "dissolved" — verify at outcome level (counts of runs, observed side-effects), not at code level

## What happened
v0.6 Mode B closed two of the five chorus-self prereqs: #2 (cron-vs-tick dissolved — Mode B has no fixed interval, so the original interval-vs-tick overlap problem vanishes by redesign) and #5 (idle no-op semantics — verified at OUTCOME level: zero ponger runs across a 5-minute pause window, not "function returns 0"). Combined with v0.5's #7, the prereq tally is 3 of 5 closed; the remaining two gate v1.0.

## Transferable lesson
Closing a prereq comes in three flavors: shipped a fix, observed the property at runtime, or redesigned so the prereq no longer applies ("dissolved"). All three are valid closures, but the third is the easiest to fool yourself on — make sure the new design genuinely makes the question moot, not just hidden. And: verify behavioral prereqs (idle, atomic, eventually-consistent) by counting runtime side-effects (logs, file writes, message counts) — not by inspecting that a function returns the right code. Code-level checks miss timing and ordering issues.

## Surface area
Next time you "close" a roadmap prereq via redesign, write one sentence explaining why the new design makes the question moot. Verify behavioral prereqs by counting side-effects over a stated window.
