# Work queue — executor inbox

Append-only. One commit per line.

**Format:** `<repo>|<sha>|<commit-date>|<subject>`

The bootstrap script seeds this with all commits from the 4 source repos
(chorus + dev-console + bonfire + mosaicat) in chronological order.

The executor role reads lines past `state/bookmarks.md::executor-last-sha`
and processes each one.

<!-- Bootstrap will append commit lines below this line -->
