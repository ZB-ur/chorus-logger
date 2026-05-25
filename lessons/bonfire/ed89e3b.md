---
sha: ed89e3b798e750d678208ce7ec340623c6d14c85
repo: bonfire
date: 2026-04-10
subject: "Add Plan 1: Foundation implementation plan"
extracted_at: 2026-05-25T19:41:56Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Foundation plans should include stub modules for downstream plans; the stubs commit to interfaces before they're implemented

## What happened
Bonfire Plan 1 covers 12 tasks for the foundation layer (plugin manifest, schema, CLI entry, shared utilities, init/archive commands, golden test case) AND ships "stub modules for Plan 2-3." The stubs aren't placeholders — they fix the import paths, function signatures, and minimum contract that Plan 2 and Plan 3 will implement against, so Plan 1's tests can wire against a known interface and Plan 2-3 don't get to redesign at integration time.

## Transferable lesson
Multi-phase plans frequently fall into "we'll figure out the interface when we get to that plan" — and then Phase 2's implementer redesigns the interface, breaking Phase 1's wiring. The fix: in the foundation phase, ship empty/stub modules that lock in the imports, function signatures, and minimum return shapes the later phases will fill in. The stubs are the contract; Phase 2/3 fill in bodies, never edit signatures.

## Surface area
Next time you write a multi-phase plan, list the modules each future phase will own and stub them out (empty functions with the eventual signature) in the foundation commit — so the foundation's tests can already import from them.
