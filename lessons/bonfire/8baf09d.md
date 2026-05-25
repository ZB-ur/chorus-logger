---
sha: 8baf09d91306926a28d2e660585f6b5abbfa2b80
repo: bonfire
date: 2026-04-10
subject: "docs: add subagent-protocol reference"
extracted_at: 2026-05-25T21:38:52Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — In a parent-spawning-subagent architecture: parent is sole writer to shared state; never leak the parent's preferred answer into the subagent's prompt

## What happened
The subagent-protocol explicitly forbids two anti-patterns: "Do not pass the parent's preferred answer to independent agents" (taints the "independent" verdict) and "Parent skill is the only writer to truth surface and bundle notes" (avoids race conditions, ensures atomic boundaries). Subagents return STRUCTURED JSON DELTAS, never finished markdown — keeps the parent in charge of how output appears. Per-agent constraints, tool permissions, and output-capture flow are tabulated.

## Transferable lesson
Two write-architecture rules for any parent-spawning-subagent system. (1) Single-writer to shared state: parent reads from subagents, parent writes to the durable record. Multi-writer subagents create race conditions and lose the atomic boundary where validation should run. (2) Don't leak your hypothesis into the prompt of any agent whose job is "give an independent view." If you tell the critic "I think this is good," you get back "yes, I confirm" — independence requires withholding. Same shape for code review, design review, security audit.

## Surface area
Next time you spawn an "independent reviewer" subagent, strip your own conclusions from the prompt — and make the parent the sole writer to any shared artifact the subagent touches.
