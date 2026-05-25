---
sha: ed1f2a0087c8feb37cb4c0670a416ea0c3a8fe32
repo: chorus
date: 2026-05-22
subject: "feat(inject-loop): skeleton + smoke Case 1 happy path (v0.5 #7)"
extracted_at: 2026-05-25T16:40:38Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — PATH-shadow your mocks for external commands; function overrides don't cross exec boundaries

## What happened
The new `lib/inject-loop.sh` shells out to `tmux`. To test it, the smoke creates a mock `tmux` script in a temp dir, prepends that dir to `$PATH`, then calls `inject-loop.sh`. The subprocess invocation finds the mock first. An earlier discarded approach defined `tmux()` as a bash function — but that override does NOT survive an `exec` or a sourced sub-shell, so the real tmux was being called.

## Transferable lesson
When you need to mock an external command that a tested script invokes via subprocess, the only reliable mechanism is PATH-shadowing: drop a fake script in a temp dir, `export PATH="$tmpdir:$PATH"`, and run the system under test. Function definitions in shell don't propagate across exec or external-command invocations; aliases definitely don't. PATH is the only universal hook.

## Surface area
Next time you need to fake `tmux`, `curl`, `git`, etc. inside a shell test, write the mock as a real executable file in a fresh tmp dir and prepend that dir to PATH — don't redefine the command as a shell function.
