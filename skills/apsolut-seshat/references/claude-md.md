# CLAUDE.md — {{PROJECT_NAME}}

{{ONE_LINE_DESCRIPTION}}

**Stack:** {{TECH_STACK}}

## Pointers

- Current tasks: `.apsolut/tasks/next/` (or `.apsolut/TASKS.md` for bare profile)
- Decisions: `.apsolut/decisions/` (or `.apsolut/DECISIONS.md`)
- Services: `.apsolut/services/` — read before adding API calls
- Ops: `.apsolut/ops/` — read before deploy/debug/setup, when broken, or before touching user data (slice by `kind:`)
- Fires: `.apsolut/fires/` (or `.apsolut/FIRES.md`) — read before changing areas that broke before
- Screenshots: `.apsolut/screenshots/` — user drops bug/UI screenshots here and references them inline (e.g. "see .apsolut/screenshots/login-broken.png"). Folder tracked, contents gitignored.

## Commands

```bash
# Add the dev/test/build commands relevant to this project
```

Linter handles code style — don't enforce style in prompts.

## Don'ts

- Don't read `.apsolut/notes/` — raw + exploration + draft plans, not for CC
- Don't preload files — fetch only what the current task needs
- Don't put binaries (PDFs, audio, exports) in `.apsolut/` — use `../artifacts/` at project root. The one exception is `.apsolut/screenshots/`, which is the user's drop zone for inline image references.
