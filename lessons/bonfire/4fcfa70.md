---
sha: 4fcfa700012924f494ffd80d8f8078c46961f21a
repo: bonfire
date: 2026-04-10
subject: "docs: add bonfire-g-red and bonfire-g-blue agent definitions"
extracted_at: 2026-05-25T22:07:54Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Adversarial-pair agents need role purity rules; force blue team to pick one of N named responses per attack — never "we'll think about it later"

## What happened
G-Red attacks the retained path, G-Blue defends it. G-Red's rules include "Do NOT propose mitigations — that is G-Blue's job" (role purity). G-Blue must respond to every attack with exactly ONE of four named options: Mitigate (new constraint), Accept risk (align with explanation), Monitor (propose verification), Explicitly state residual risk (for unmitigable items). "Do NOT dismiss attacks without evidence."

## Transferable lesson
When you split work across an adversarial pair (red/blue, attack/defend, proposer/critic), each side will naturally drift into the other's job — red-teamers start proposing fixes, blue-teamers start handwaving attacks away. Two reinforcements: (1) role-purity rules ("you do NOT do X — that's the other agent's job"); (2) a closed-set response template ("for each attack, pick one of: Mitigate, Accept-with-rationale, Monitor, Residual-Risk"). Without (2), "we'll think about it" becomes the default and risks accumulate unmitigated.

## Surface area
Next time you design an adversarial pair (red/blue review, audit/response), give each side an explicit "you do NOT do X" rule, and require the responder to pick one of a small set of named responses per attack — never an open-ended "TBD."
