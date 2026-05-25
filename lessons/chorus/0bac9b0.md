---
sha: 0bac9b012c1a89a5fef64b94b5c5f12a13a2a24c
repo: chorus
date: 2026-05-25
subject: "docs(runs): v0.6 build evidence final HEAD sha post-v0.2-build tag (v0.6 scope)"
extracted_at: 2026-05-25T18:14:17Z
verdict: keep
verdict_at: 2026-05-25T18:14:30Z
verdict_reason: null
---

# Lesson — Reference future tags with `(pending)` placeholders, patch forward after tagging — never tag-then-rewrite-history

## What happened
The v0.6 build-evidence doc was committed at Task 15 with a "Pending Task 16 reference" section. Task 16 then created the immutable `v0.2-build` annotated tag. A follow-up doc-only commit (this one) filled the placeholder in past tense and renamed the section to "Task 16 tag reference." The tag was NOT moved; the doc patch landed on main as a normal forward commit.

## Transferable lesson
When a document needs to reference a SHA or version that doesn't exist until later, write the doc now with an obvious `(pending)` placeholder and patch it forward as a separate commit after the tag/snapshot is created. The opposite — waiting to commit the doc until the tag exists — leaves the build evidence in a draft state during the most failure-prone phase. The opposite-opposite — moving the tag to "include the doc" — breaks every cross-reference that already cited the original SHA.

## Surface area
Next time you reference a tag/SHA that doesn't yet exist, commit the document NOW with `(pending — filled at Task N)` markers, then patch forward later. Never move tags to include trailing docs.
