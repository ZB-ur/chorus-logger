---
sha: 07ceb4fd34964f4d2ce2f8afed0a8111b9afa562
repo: chorus
date: 2026-05-22
subject: "fix(smoke-quota-watcher): drop misleading unguarded [PASS] in A2 case (v0.5 #9)"
extracted_at: 2026-05-25T16:40:38Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Never print "[PASS]" from a code path that isn't actually asserting anything

## What happened
A smoke test case had two sub-runs. The first mocked `fetch_usage` entirely — which bypassed the A2 assertion that the test claimed to cover — and still printed `[PASS]`. Quality review caught that the [PASS] was unguarded: no assertion fired before it. Fix dropped the misleading sub-run entirely; the second sub-run (urlopen-patched, with real rc/flag assertions) was sufficient on its own.

## Transferable lesson
A success marker without an assertion behind it is worse than no test at all — it tells the next maintainer (and any regression scanner) that a code path is verified when nothing is. The rule: every `[PASS]` / `OK` / green check must be immediately preceded by an assertion that could have failed. If you can mock the thing away and still print PASS, your test is decorative.

## Surface area
Next time you write a print "PASS" line, look up — if there's no `assert`/expectation in the last few lines of the same function, delete the print.
