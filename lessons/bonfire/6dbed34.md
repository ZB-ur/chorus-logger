---
sha: 6dbed34fd83b007193fb1c9bc8c8575fac1e473f
repo: bonfire
date: 2026-04-10
subject: "feat: wire truth-surface and log commands to CLI router"
extracted_at: 2026-05-25T21:11:04Z
verdict: keep
verdict_at: 2026-05-25T21:11:30Z
verdict_reason: null
---

# Lesson — When N subcommands all dispatch to one module by action name, use a factory closure — never N near-identical wrappers

## What happened
Nine `truth-*` subcommands and three `log-*` subcommands all route to one module each (truth-surface.cjs, logger.cjs). Rather than write 12 near-identical wrapper functions, the CLI registers each with a factory call: `'truth-propose': () => truthCommand('propose')`. `truthCommand(action)` returns a closure that handles arg parsing, action dispatch, error formatting, and JSON exit consistently. One place to fix the arg convention, one place to fix the exit convention.

## Transferable lesson
A series of CLI subcommands `<prefix>-<verb>` that all delegate to the same module is a strong signal for a factory closure: `<prefix>Command(verb)`. Without the factory, the same arg-parse + error-format code copies into N wrappers; the day you decide to change the verb separator, you touch N files. With the factory, you change the closure body once. Same idea for HTTP handlers, RPC stubs, plugin entry points.

## Surface area
Next time you write a CLI dispatch table with `prefix-*` entries each calling the same module, replace the wrappers with a `prefixCommand(verb)` factory closure.
