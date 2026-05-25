---
sha: 70a6fad242ff32a13d30db50e79998d6d0bb4c70
repo: chorus
date: 2026-05-22
subject: "feat(inject-loop): Stage 1 detect-then-settle probe (v0.5 #7)"
extracted_at: 2026-05-25T16:43:09Z
verdict: pending
verdict_at: null
verdict_reason: null
---

# Lesson — Wait for "content matches AND buffer is stable", not just "content matches"

## What happened
Replaced a single-shot `capture-pane` check with a polling loop (every 0.1s) that marks the prompt ready only when (a) the last visible line ends with `❯`, AND (b) the buffer has been unchanged for ≥ SETTLE seconds. This defends against mid-redraw Enter consumption (the F6 hypothesis from earlier cycles) — it is NOT a paint-timing fix that just waits longer. Converges in ~1.5s for a normally-painting prompt vs. the previous fixed 12s sleep that was empirically broken.

## Transferable lesson
When waiting for an interactive UI to be ready for input, the two failure modes are "not painted yet" and "still repainting." A content-match probe handles the first; a "buffer hasn't changed in N ms" stability check handles the second. Doing only one of them leaves the other failure mode wide open.

## Surface area
Next time you write a wait-for-prompt probe (tmux send-keys, expect-style automation, selenium, e2e), require both content match AND a brief unchanged window before sending input.
