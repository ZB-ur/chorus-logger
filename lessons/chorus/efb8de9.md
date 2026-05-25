---
sha: efb8de9d120ac2544c0ca926896eb0844c9a29b7
repo: chorus
date: 2026-05-24
subject: "fix(inject-loop): bump SCHEDULED_TIMEOUT default 30s -> 90s (v0.5 #7 cycle-5c)"
extracted_at: 2026-05-25T17:10:39Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — When bumping a timeout/retry/threshold, name the source incident in a code comment

## What happened
Default `SCHEDULED_TIMEOUT` bumped from 30 → 90 (≈1.9x margin over the observed 47s worst-case agent-planning latency). Stage 2 polls exit early on first regex match, so happy-path latency is unchanged; only the worst-case bound moves. A comment block in the script explicitly names `BLOCKED-task-11b + cycle-5c` so a future reader knows WHY 90 and not 60 or 120 — no magic numbers.

## Transferable lesson
Numeric constants that exist because of an incident are landmines without provenance. Every "we bumped this to N" deserves a one-line comment naming the incident/log/issue/PR that justifies N — otherwise the next maintainer will halve it to look reasonable. The same rule applies to retry counts, rate limits, batch sizes. Also: pick the bound based on observed worst-case + a stated margin (here ~1.9x), not a round number.

## Surface area
Next time you change a numeric default in code, add a comment referencing the incident or measurement that justifies the new value — and prefer a value tied to observed data plus a stated margin over a round number.
