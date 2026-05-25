---
sha: ea466dcfa0be8fa6f46eaf40c00d9005a72229b4
repo: bonfire
date: 2026-04-10
subject: "docs: add stage-playbook and approval-gate references"
extracted_at: 2026-05-25T21:38:52Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — When an agent has authority to write structured records, document the (evidence-type → action) mapping explicitly — turn judgment into a lookup table

## What happened
The `approval-gate.md` reference doesn't just list "Stage A rules" — it spells out the evidence-type → truth-surface-action mapping: reality-checker-verified facts → `truth-propose confirmed_fact`; dubious user claims → `truth-propose challenged_claim`; confirmed user goals → `truth-propose retained_goal`; acceptance criteria → `truth-propose acceptance_semantic`. Rules also include "treat the user's description as low-reliability evidence" and "do not optimize for fewer questions."

## Transferable lesson
An agent (LLM, junior engineer, ops runbook follower) given write authority to a structured record needs more than principles — it needs a lookup table. "If you observe X, take action Y" beats "use judgment to record entries appropriately." The lookup is reviewable, auditable, and turns disagreement into "the table is wrong about X→Y" instead of "you wrote the entry wrong." Also: explicit anti-rules ("do not optimize for fewer questions") name the temptation rather than relying on the reader to resist it.

## Surface area
Next time you give an agent authority to write structured entries, ship a (observation → record-type → action) lookup table in the reference doc; name the anti-pattern temptations explicitly.
