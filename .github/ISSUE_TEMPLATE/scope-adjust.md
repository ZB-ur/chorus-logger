---
name: Scope adjustment (conductor input)
about: Add or remove a repo from the work queue, or replay a range of commits
title: "[scope] <action> <repo>"
labels: ["conductor-input", "scope-adjust"]
assignees: []
---

<!--
Use this to:
- add a new repo to the work queue (e.g., post-bootstrap)
- skip a range of commits (e.g., "skip mosaicat before 2024")
- replay commits (e.g., "re-extract chorus commits with updated prompt")
- pause processing of a specific repo

The conductor-listener role polls this every 15 min, applies the change,
and closes the issue.
-->

---
type: scope-adjust
action: add-repo | remove-repo | skip-range | replay | pause-repo
target: <repo-name-or-path>
range: <optional, e.g., HEAD~50..HEAD>
---

## Reasoning

<why this adjustment>

## Effect

<what you expect to change in the work queue or processed lessons>
