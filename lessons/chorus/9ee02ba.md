---
sha: 9ee02baff6a8cb6c6e11fac88985fc09d24efdd4
repo: chorus
date: 2026-05-22
subject: "chore(quota-watcher): drop inert anthropic-beta header (v0.5 #9)"
extracted_at: 2026-05-25T16:40:38Z
verdict: keep
verdict_at: 2026-05-25T16:42:07Z
verdict_reason: null
---

# Lesson — Verify cargo headers empirically (vary the value, observe the response) before keeping them

## What happened
The quota-watcher request carried an `anthropic-beta: oauth-2025-04-20` header inherited from the original auth flow. Cycle-4-mid tested four distinct values (current, missing, `messages-*`, `oauth-2024-04-26`) and all returned identical HTTP 200 with identical body. The header was cargo. Dropped. Live verification happens at deploy via `launchctl kickstart` + reading `global/quota-status.md`.

## Transferable lesson
Headers, query params, and "just-in-case" config values accumulate in HTTP calls and never get pruned. The cheap probe: change the value (or remove it) and see if the response changes. If three or four perturbations all return identical results, the field is inert — delete it. Less surface area = less cargo for the next maintainer to wonder about.

## Surface area
Next time you inherit an HTTP call from older code, run a quick perturbation pass on every "optional" header/param — anything that doesn't change behavior gets dropped in the same PR.
