---
sha: ab0fa884adac15f76b9155022cc89b19a660f5f1
repo: bonfire
date: 2026-04-10
subject: "feat: implement state machine with step transitions, reentry, run management"
extracted_at: 2026-05-25T21:11:04Z
verdict: keep
verdict_at: 2026-05-25T21:11:30Z
verdict_reason: null
---

# Lesson — Bump `updated_at` on every state write; drive step ordering from a schema constant, not hardcoded sequences

## What happened
`saveState(root, state)` always sets `state.updated_at = timestamp()` before the writeAtomic — no caller has to remember. Step ordering is driven by `schema.step_order[pipelineStage]` from the loaded JSON schema (`stepsForPipeline()`/`pipelineOrder()` helpers) rather than literal arrays in code. Adding a new pipeline stage = one schema edit + the corresponding step entries; no code change.

## Transferable lesson
Two paired habits for state-machine code. (1) Mutating `save` helpers should always stamp an `updated_at` field before write — gives free staleness monitoring (`now - updated_at > threshold` flags stuck states) and free debug logs (when did this stage last move?). (2) Drive step orderings, allowed transitions, and gate names from a schema; hardcoded sequences in code force a multi-file diff for every flow addition. Schema-driven flows also enable schema-integrity tests (see 6f600a6 lesson).

## Surface area
Next time you write a `saveX(state)` helper, stamp `updated_at` inside it — never trust the caller. Next time you encode "the order of stages" inline in code, ask whether it belongs in the schema instead.
