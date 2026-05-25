---
sha: 90f5edced4203223899e913a3ed07ba5b6130d3e
repo: bonfire
date: 2026-04-10
subject: "docs: add bonfire-d-critique agent definition"
extracted_at: 2026-05-25T22:07:54Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Adversarial agents need a required-minimum challenges count (≥1) — otherwise the LLM defaults to politely approving everything

## What happened
`bonfire-d-critique` is an "independent requirements critic." Its rules include: "You must include at least 1 challenge. If you find nothing to challenge, you are not looking hard enough." The delta schema enforces this at parse time: `challenges` is a required field, length ≥ 1. The agent has no Write permission — output is JSON deltas only, parsed and applied by the parent skill.

## Transferable lesson
LLMs default to agreeable when asked to review work — they're trained to be helpful, not adversarial. Without an enforced minimum, a "critic" agent reliably produces "looks good to me" reviews. The fix is a structural floor: `challenges_min_length: 1` in the schema, enforced by validation. Pair with a clear instruction: "If you find nothing to challenge, you are not looking hard enough" — frames empty output as the agent's failure, not as acceptance. Same trick for red-team prompts, audit prompts, devil's-advocate prompts.

## Surface area
Next time you ask an LLM to critique/audit/red-team, require ≥1 finding in the output schema and add "if you find nothing, you're not looking hard enough" to the rules.
