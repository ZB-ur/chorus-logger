# chorus-logger

An open compounding-engineering ledger: continuously extract transferable
lessons from commits across multiple personal repos, audit them, and
publish daily digests. Powered by [chorus](https://github.com/) runtime
v0.6.

## What this is

Compounding-engineering ledger inspired by Boris Cherny's pattern, scaled
to multi-repo dogfood:

- Each commit (historical + new) is mined for a **transferable lesson**
- Lessons are audited for quality (transferable vs project-noise)
- Daily 22:00 digest aggregates the day's keepers

Source repos (initial scope, 697 commits backfill):

| Repo | Commits | Theme |
|------|---------|-------|
| chorus | 56 | Multi-session AI orchestration runtime |
| dev-console | 20 | A0 pattern (dual-session adversarial) origin |
| bonfire | 174 | (TBD — described in lessons over time) |
| mosaicat | 447 | (TBD — described in lessons over time) |

## Architecture

4 chorus roles running against this repo as their working directory:

- **executor** (Mode B): processes new sha from `inboxes/executor.md` → writes `lessons/<repo>/<sha>.md`
- **reviewer** (Mode B): audits each lesson → labels verdict (keep / skip / needs-input)
- **summarizer** (Mode A, daily 22:00): aggregates day's lessons → `digests/<date>.md`
- **conductor-listener** (Mode A, every 15 min): polls GitHub Issues labeled `conductor-input` → routes to relevant inbox → closes issue

See [`docs/architecture.md`](docs/architecture.md) for details.

## How conductor (human) interacts

Two channels:

| When | Channel | How |
|------|---------|-----|
| At desk | local files via chorus | Read `lessons/` / `digests/` directly, write to `docs/dogfood-notes.md` |
| Remote / mobile | GitHub Issues | Use [`conductor-input` issue templates](.github/ISSUE_TEMPLATE/) to leave verdicts, adjust scope, or log friction |

Issue-based input is the **MVP of phase 4 LUI** (planned for chorus v1.0+). Dogfooding it now to surface what works.

## Status

| Phase | Status |
|-------|--------|
| Backfill (697 commits → lessons) | pending — runs once chorus-start lands |
| Steady-state (process new commits) | pending |
| Sanitization | none — repo is open by design |

See [`docs/dogfood-notes.md`](docs/dogfood-notes.md) for live friction log.

## License

CC0 / public domain for lessons. Source code (none yet in this repo) MIT.
