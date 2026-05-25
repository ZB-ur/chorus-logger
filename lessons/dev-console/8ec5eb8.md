---
sha: 8ec5eb81fcfbada8bc5f64fbf2c8cf972aff2f3b
repo: dev-console
date: 2026-05-18
subject: "feat: derive_status with boundary semantics"
extracted_at: 2026-05-25T18:43:09Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Test both sides of every numeric threshold; a one-sided test passes even if you wrote `<` instead of `<=`

## What happened
`derive_status` returns "active" when days ≤ 30, "dormant" when 31 ≤ days ≤ 90, "abandoned" when days ≥ 91. Tests cover BOTH sides of each boundary: 30 active, 31 dormant; 90 dormant, 91 abandoned — plus None=unknown and today=active. The pattern is deliberate: a test that only asserts "30 → active" passes whether the code says `<= 30` or `< 30` (the second is wrong but invisible).

## Transferable lesson
Threshold logic has two ways to be wrong: the constant (30 vs 31) and the comparison (`<` vs `<=`). A test at exactly the threshold + a test one step past the threshold catches both — neither alone does. Apply this to status buckets, rate-limit windows, retry-count cutoffs, age buckets, anything with a `<= N` or `>= N` in code. The extra test pair costs nothing.

## Surface area
Next time you write code with a numeric threshold, draft two tests per threshold (at the boundary, just past it) before the implementation — it forces you to think about whether the boundary itself is inside or outside the bucket.
