---
sha: 33ceba62bced061b1e674aa75a5e0f9f006eaf2e
repo: chorus
date: 2026-05-22
subject: "feat(inject-loop): Stage 2 polling for Scheduled <hex> + Case 4 (v0.5 #7)"
extracted_at: 2026-05-25T16:43:09Z
verdict: keep
verdict_at: 2026-05-25T16:43:30Z
verdict_reason: null
---

# Lesson — When matching identifiers via regex, prefer `{N,}` over `{N}` to future-proof against format growth

## What happened
Stage 2 polls for `/Scheduled [a-f0-9]{8,}/` in the pane buffer. The choice of `{8,}` instead of `{8}` exactly was a cycle-3b decision: claude-code's job-id format could extend to 9, 10, or more hex chars in a future release; pinning to exactly 8 would silently start failing on the next bump. The lower bound stays as 8 so an obvious typo (e.g. `Scheduled 12`) is still rejected.

## Transferable lesson
External identifier formats (request IDs, job IDs, hashes, version strings) tend to grow over time as systems scale or add disambiguation. Matching with `{N}` exact creates a silent compatibility hazard at the next bump; `{N,}` keeps the floor while accepting growth. Same idea: prefer `^prefix-` over `^prefix-\d{6}$` if you only care that something starts with the prefix.

## Surface area
Next time you write a regex against an identifier from an external system, set a lower bound but leave the upper bound open unless the producer's spec explicitly fixes it.
