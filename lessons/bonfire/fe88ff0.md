---
sha: fe88ff039d45d7ee5d166fec9de066c75cd204f5
repo: bonfire
date: 2026-04-10
subject: "feat: implement delta-parser with per-agent JSON schema validation"
extracted_at: 2026-05-25T21:11:04Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Encode validation constraints in schema keys by convention (`_min_length`, `_enum`, `_required_when_X`); document the conventions so they're not folklore

## What happened
`validateDelta(agentName, delta)` reads `schema.delta_schemas[agentName]` and applies constraints by parsing key suffixes: `<field>_min_length` becomes an array-length check, `verdict_enum` an allowed-values check, `conflict_type_required_when_rejected` a conditional-required check, `conflict_type_from_reentry_routes` a cross-reference into another schema section. Adding a new "min length 3" constraint = adding `{"reasons_min_length": 3}` to the schema — zero code change IF the convention is supported.

## Transferable lesson
Schema-driven validation by key-naming convention compresses N hand-written validators into one generic loop, but is invisible to a reader who doesn't know the conventions. The trade is acceptable only when (a) the conventions are documented (in the schema file's comments OR a CONVENTIONS.md) AND (b) the validator's source explicitly lists which conventions it supports. Add a convention by extending the validator, NOT by guessing — silent silence-on-typo is the typical failure ("min_lenght" never matches the suffix and the field is silently unconstrained).

## Surface area
Next time you encode per-field rules in a JSON schema, document the key-suffix conventions you support (`_min_length`, `_max`, `_enum`, `_required_when_X`), and have the validator throw on unknown suffixes rather than silently ignoring them.
