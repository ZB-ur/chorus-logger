---
sha: ebeab9f4bb10cc831e53ea07e8900d6fc0bd0c07
repo: bonfire
date: 2026-04-10
subject: "docs: add complete reentry route table to code-playbook"
extracted_at: 2026-05-25T22:35:55Z
verdict: keep
verdict_at: 2026-05-25T22:36:10Z
verdict_reason: null
---

# Lesson — Duplicate the authoritative table into the doc that needs it, with a "from <source>" note — readers should never have to context-switch to a schema

## What happened
The `conflict_type → target stage` route table existed in `bonfire-v1.json` (authoritative), but the code-playbook reference referred to it abstractly. Fix: paste the full table into the playbook with a header "from `bonfire-v1.json` route table" — readers get the answer where they're already reading. The commit message names the friction explicitly: "forcing readers to look in multiple places."

## Transferable lesson
"Single source of truth" is a code/data principle, not a docs principle. When a mapping lives in a schema/config and is needed by readers of a separate doc, duplicate the table in the doc with an attribution line ("from <source>"). The duplication is intentional — readers stay in flow, and the attribution tells maintainers where to update first when the mapping changes. Same shape for error-code tables, status-code references, status enums, route maps.

## Surface area
Next time you write a doc that references a "see the schema for the full mapping," paste the mapping inline with a "from <source>" line — the small duplication cost beats the context-switch cost on every read.
