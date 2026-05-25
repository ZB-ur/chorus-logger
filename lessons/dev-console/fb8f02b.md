---
sha: fb8f02b6473499368de734a82d9ec3006df7adeb
repo: dev-console
date: 2026-05-18
subject: "feat: infer_stack via priority-ordered marker files"
extracted_at: 2026-05-25T18:45:36Z
verdict: keep
verdict_at: 2026-05-25T18:45:50Z
verdict_reason: null
---

# Lesson — When inferring "kind of thing" from sibling markers, concat in priority order — don't first-match away polyglot projects

## What happened
`infer_stack(directory)` checks for four marker files in priority order (Cargo.toml→Rust, pyproject.toml→Python, go.mod→Go, package.json→Node). If both pyproject.toml AND package.json exist, returns "Python+Node" — concatenation, not first match. The decision was a spec-correction recorded in the init commit ("按优先级顺序 concat — no 'first match' language"). Markers live in a `_STACK_MARKERS: list[tuple[str, str]]` constant ordered by priority.

## Transferable lesson
Inference from sibling markers (manifest files, config files, README sections) tempts you toward "first-match wins" because it's simpler — but real projects often hold multiple cohabiting markers (polyglot codebases, monorepos, mixed-build setups). First-match silently downgrades them; concat in priority order preserves the truth. Same principle for tech-stack badges, language detection in linters, framework auto-detect.

## Surface area
Next time you write a "what kind of project is this?" inference, return all matching labels joined by `+` rather than the first hit; reserve "first match wins" for cases where the markers are truly mutually exclusive.
