---
sha: caf659aad939e71987e18fdbfb60c22b2723a65d
repo: chorus
date: 2026-05-24
subject: "docs(runs): v0.5 build evidence - cycle-6 re-audit verdict + new v0.6 candidates"
extracted_at: 2026-05-25T17:13:16Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — When supervision and outcome disagree, trust the outcome and investigate the supervisor

## What happened
Cycle-6 re-audit ran 26 independent verifications. Key finding: BLOCKED-task-11b's "partial failure" wasn't a mechanism failure — it was a supervision FALSE-POSITIVE. Inbox forensics showed pinger wrote `ping 2026-05-24T03:36:47Z` to ponger.md at T+47s even though inject-loop reported Stage 2 timeout at T+31s. The chorus mechanism (REPL + CronCreate + tick + inbox write) worked end-to-end; only the pane-scrape supervisor gave up early. Cycle-2's "leave claude running on inject-failure" preserved the success.

## Transferable lesson
Probe-based supervision (capture-pane, status endpoint poll, log tail) reports the prober's belief, not the world. When the prober says "failed" but the outcome (side-effect, downstream write, end-state) says "succeeded," the prober is wrong. Always cross-check supervision with out-of-band evidence — inbox writes, file mtimes, downstream signals — before declaring failure. Build supervisors that prefer false-positive timeouts over silent false-negatives (timeout is investigable, missed failure is not).

## Surface area
Next time you build a probe-based supervisor, design at least one independent out-of-band evidence path (file write, event log, side-effect timestamp) you can use to refute false-positive timeouts.
