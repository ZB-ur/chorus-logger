# chorus-logger Architecture

## Data flow

```
┌────────────────────────────────────────────────────────────────────────┐
│ chorus runtime (~/AiCoding/chorus)                                     │
│                                                                        │
│  ┌────────────┐  ┌────────────┐  ┌──────────────┐  ┌────────────────┐ │
│  │  executor  │  │  reviewer  │  │  summarizer  │  │ conductor-     │ │
│  │  Mode B    │  │  Mode B    │  │  Mode A      │  │ listener Mode A│ │
│  │  (inbox)   │  │  (lesson)  │  │  (22:00 cron)│  │  (15min cron)  │ │
│  └─────┬──────┘  └──────┬─────┘  └──────┬───────┘  └────────┬───────┘ │
│        │                │               │                   │         │
│        │ cwd            │ cwd           │ cwd               │ cwd     │
└────────┼────────────────┼───────────────┼───────────────────┼─────────┘
         │                │               │                   │
         ▼                ▼               ▼                   ▼
┌────────────────────────────────────────────────────────────────────────┐
│ chorus-logger (~/AiCoding/chorus-logger ↔ github.com/ZB-ur/chorus-logger) │
│                                                                        │
│  inboxes/executor.md ──→ executor reads                                │
│       ▲                       │ writes lessons/                        │
│       │                       ▼                                        │
│  bootstrap.sh             lessons/<repo>/<sha>.md ──→ reviewer audits  │
│       │                       │  reviewer writes verdict frontmatter   │
│       │                       ▼                                        │
│  inboxes/reviewer.md ←── executor appends sha                          │
│                               │                                        │
│                               ▼                                        │
│                          summarizer reads day's keepers                │
│                               │                                        │
│                               ▼                                        │
│                          digests/<date>.md                             │
│                                                                        │
│  state/bookmarks.md (per-repo last-processed sha)                      │
│  state/monitor-*-last-emit.txt (chorus runtime heartbeat)              │
│                                                                        │
│  .github/ISSUE_TEMPLATE/* ──→ conductor-listener polls + routes        │
└────────────────────────────────────────────────────────────────────────┘
```

## Why split between chorus and chorus-logger?

| Concern | chorus | chorus-logger |
|---------|--------|---------------|
| Runtime engine | ✅ | — |
| spec / plan / critic / build evidence | ✅ (intentionally local) | — |
| Role prompts | ✅ (`domains/compounding/`) | — |
| Inboxes + lessons + digests | — | ✅ (published) |
| Conductor input via Issues | — | ✅ |
| Public visibility | private (personal audit trail) | **public** (by design) |

Roles run with `cwd: ~/AiCoding/chorus-logger`, so their git operations affect chorus-logger, not chorus itself.

## Role-level detail

### executor (Mode B)

- Wake on `inboxes/executor.md` mtime change
- Read unprocessed lines (past `state/bookmarks.md`)
- For each `<repo>|<sha>|<date>|<subject>`:
  - `git -C ~/AiCoding/<repo> show <sha>` to read diff + message
  - Extract transferable lesson (≤200 字 markdown, 3 sections)
  - Write `lessons/<repo>/<sha>.md` with frontmatter (verdict: pending)
- Update `state/bookmarks.md`
- Append sha + lesson-path to `inboxes/reviewer.md`
- `git add . && git commit && git push origin main`

### reviewer (Mode B)

- Wake on `inboxes/reviewer.md` mtime change
- Read unprocessed lines (past per-role bookmark)
- For each `<repo>|<sha>|<lesson-path>`:
  - Read the lesson markdown
  - Audit: transferable? duplicate? needs-conductor judgment?
  - Update lesson's frontmatter `verdict:` to `keep` / `skip` / `needs-input`
  - If `needs-input`, the conductor will see it in next digest's "needs input" section
- Commit + push

### summarizer (Mode A, daily 22:00)

- Read all lessons with `verdict: keep` modified since yesterday's digest
- Cluster by emerging theme (typically 3-5 clusters)
- Top 5 highlights with one-line quotes
- "Needs conductor input" section lists open issues + needs-input lessons
- Write `digests/<YYYY-MM-DD>.md`
- Commit + push

### conductor-listener (Mode A, every 15 min)

- `gh issue list --label conductor-input --state open`
- For each issue:
  - Parse frontmatter (type, target)
  - Route to appropriate inbox or apply state change
  - Comment on issue with action taken
  - Close issue
- If no issues: no-op (idle-aware)

## Inbox formats

### inboxes/executor.md

```
# Work queue — append-only; one commit per line
# Format: <repo>|<sha>|<commit-date>|<subject-truncated-60-chars>

chorus|0824cbf...|2026-05-20|"feat: README, CLAUDE.md, .gitignore"
chorus|71f4c9e...|2026-05-20|"feat: roles.yaml + hello-chorus toy domain"
...
```

### inboxes/reviewer.md

```
# Audit queue — append-only; one lesson per line
# Format: <repo>|<sha>|<lesson-path>

chorus|0824cbf...|lessons/chorus/0824cbf.md
chorus|71f4c9e...|lessons/chorus/71f4c9e.md
...
```

### state/bookmarks.md

Markdown table; per-repo + per-role last processed.

## What chorus-logger does NOT do

- Run chorus runtime (that's in `~/AiCoding/chorus`)
- Modify source repos (chorus / dev-console / bonfire / mosaicat) — strictly read-only via `git -C <path> show`
- Sanitize content for public exposure — open by design
