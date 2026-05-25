---
sha: dff1b20803bd4ad7a7a0a6fe1cd5806c2f8f97aa
repo: bonfire
date: 2026-04-10
subject: "test: add freeze maturity gates, annotate, supersede, discard tests"
extracted_at: 2026-05-25T20:42:05Z
verdict: keep
verdict_at: 2026-05-25T20:42:30Z
verdict_reason: null
---

# Lesson — For a per-category state machine, write ≥1 test per (category × transition); spell out compound invariants ("supersede produces a SUPERSEDED+FROZEN pair") explicitly

## What happened
Nine tests, deliberately structured: category-specific freeze gates for `retained_goal`, `confirmed_fact`, `high_impact_risk` (different gates per category); annotate-whitelist enforcement on FROZEN entries; supersede producing a PAIR of events (old marked SUPERSEDED, new marked FROZEN); discard requiring rationale. Coverage is not "does freeze work" — it's "does freeze do the right thing for each category, and does each multi-event operation emit the right event set."

## Transferable lesson
State machines with per-type rules need per-type tests. "freeze works" passes if any one category's freeze succeeds — but masks bugs where category B's gate is missing. Enumerate (category × transition) and write at least one test per cell. Same with compound operations that emit multiple events: name the invariant explicitly ("supersede produces SUPERSEDED+FROZEN pair") rather than just "supersede works," and assert on the full event set.

## Surface area
Next time you test a state machine with per-type/per-category rules, walk the matrix and write one test per cell; for any operation that emits multiple events, assert on the FULL event set, not just the last event.
