---
sha: db8ed52ebee2e67ffe7ebff867984747017bc3d6
repo: dev-console
date: 2026-05-18
subject: "feat: latest_commit via git log subprocess (2s timeout)"
extracted_at: 2026-05-25T18:45:36Z
verdict: keep
verdict_at: 2026-05-25T18:45:50Z
verdict_reason: null
---

# Lesson — Shelling out to git from your code needs four defenses: timeout, FileNotFoundError, returncode, and parse fallback

## What happened
`latest_commit` runs `git -C <dir> log -1 --format=%ct%n%s` via `subprocess.run`. Four guards: (1) `timeout=2.0` — git can hang on a corrupted index or huge pack; (2) except `(TimeoutExpired, FileNotFoundError, OSError)` → None — git might not be on PATH; (3) `returncode != 0` → None — empty repo (no commits) returns non-zero; (4) `len(parts) != 2` → None, plus `int(parts[0])` wrapped in try — parse failures don't crash. Tests use a fresh `tempfile.TemporaryDirectory()` and `git init` with a fixed `GIT_COMMITTER_DATE` env var for determinism.

## Transferable lesson
"Just call git" is four bugs waiting: it can hang, be missing, fail nontrivially, and emit unexpected output. Every shell-out to a vendor tool needs all four guards or your code becomes the slowest+most-flaky part of the pipeline. For testing: never depend on the global git state — `git init` a tmp dir and set `GIT_COMMITTER_DATE` via env so the test is deterministic across machines and clocks.

## Surface area
Next time you `subprocess.run` against git, jq, ffmpeg, etc., set a timeout, catch the tool-missing exception, check returncode separately from output, and use a tmp-dir + fixed-env-var test fixture.
