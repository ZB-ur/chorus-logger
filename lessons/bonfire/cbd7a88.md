---
sha: cbd7a8899933ce7b70795d915798b3342a76d8c9
repo: bonfire
date: 2026-04-10
subject: "feat: add CLI entry point with subcommand routing and route command"
extracted_at: 2026-05-25T20:11:09Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Bin entry script is a dispatcher only — every subcommand lives in its own bin/lib/<name>.cjs module

## What happened
`bin/bonfire-tools.cjs` is the CLI entry point. Its body does nothing except parse the first argv as a subcommand name, then dispatch to a module in `bin/lib/`. Each subcommand (`route`, `init`, `archive`, …) is a separate `.cjs` file with a default exported function. The entry script stays under ~90 lines and is easy to test as routing-only.

## Transferable lesson
A CLI entry point with embedded business logic grows into an unsearchable monster — every subcommand's bug fix touches the entry script, blame becomes meaningless, and the test surface widens unnecessarily. Keep `bin/cli.<ext>` as a single switch statement (or table lookup) that imports per-subcommand modules. The entry script's only logic: argv parsing, subcommand dispatch, top-level error formatting. Each subcommand has its own file, its own tests, its own changelog rights.

## Surface area
Next time you add a subcommand to a CLI, do not add to the entry script — create `bin/lib/<subcommand>.<ext>` and register it via the dispatcher's table.
