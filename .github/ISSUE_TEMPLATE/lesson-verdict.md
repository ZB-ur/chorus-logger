---
name: Lesson verdict (conductor input)
about: Override a reviewer's verdict on a specific lesson, or provide judgment when reviewer marked "needs-input"
title: "[verdict] <repo>/<sha7> — <one-line summary>"
labels: ["conductor-input", "lesson-verdict"]
assignees: []
---

<!--
Use this when:
- reviewer set verdict=needs-input and you want to decide
- you disagree with a verdict reviewer assigned
- you want to demote a "keep" to "skip" or vice versa

The conductor-listener role polls this every 15 min, applies your verdict,
and closes the issue.
-->

---
type: lesson-verdict
target: lessons/<repo>/<sha>.md
verdict: keep | skip | needs-input | expand
---

## Reasoning

<your reasoning in 1-3 sentences>

## Additional notes (optional)

<longer context if helpful, e.g., link to related lessons>
