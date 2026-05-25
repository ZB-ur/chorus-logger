---
sha: 8dfd24f78e8fac60df8e605cdf1f598a4b2c44fc
repo: chorus
date: 2026-05-24
subject: "docs(claude): cwd discipline - start claude from repo root, not subdirs"
extracted_at: 2026-05-25T17:13:16Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Tools with per-CWD state have hidden coupling between launch directory and feature access — document the discipline before users find out the hard way

## What happened
Memory scope in Claude Code is per-CWD: starting `claude` from `lib/`, `bin/`, `domains/<name>/`, or `state/` creates an orphaned memory pool with no access to project-level memories. Discovered while prepping a v0.6 brainstorm. Bumped from a casual fix to a committed CLAUDE.md rule because future Mode B role spawners will face the same question and the convention belongs in the project's central guide.

## Transferable lesson
Many tools key per-CWD state silently (claude memory, IDE workspaces, direnv, asdf versions, npm/yarn .node-version, git worktrees). Launching from a non-root subdir gives you an apparently-working tool with the wrong context. The hidden coupling shows up as "tool X seems to have forgotten everything" weeks later. Document the launch-from-where rule in the repo's primary contributor doc before any automation depends on the right answer.

## Surface area
Next time you depend on a per-CWD tool inside automation, fix the launch cwd at the top of the script (`cd "$REPO_ROOT"`) and document in CLAUDE.md / README that humans should do the same.
