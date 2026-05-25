# Audit queue — reviewer inbox

Append-only. One lesson per line.

**Format:** `<repo>|<sha>|<lesson-path>`

The executor role appends each new lesson here after writing it.

The reviewer role reads lines past `state/bookmarks.md::reviewer-last-sha`
and audits each lesson — updating the lesson's frontmatter `verdict:` field.

<!-- Executor will append lesson lines below this line -->
