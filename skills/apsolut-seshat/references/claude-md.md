# CLAUDE.md — {{PROJECT_NAME}}

{{ONE_LINE_DESCRIPTION}}

**Stack:** {{TECH_STACK}}

## Pointers

- Current tasks: `.apsolut/tasks/next/` (or `.apsolut/TASKS.md` for bare profile)
- Decisions: `.apsolut/decisions/` (or `.apsolut/DECISIONS.md`)
- Services: `.apsolut/services/` — read before adding API calls
- Ops: `.apsolut/ops/` — read before deploy/debug/setup, when broken, or before touching user data (slice by `kind:`)
- Fires: `.apsolut/fires/` (or `.apsolut/FIRES.md`) — read before changing areas that broke before
- Screenshots: `.apsolut/screenshots/` (davinci: `.apsolut/08-screenshots/`) — user drops bug/UI screenshots here and references them inline (e.g. "see .apsolut/screenshots/login-broken.png"). Folder tracked, contents gitignored.
- Files: `.apsolut/files/` (davinci: `.apsolut/07-files/`) — cold storage for PDFs, audio, exports, any other binary. User may add subfolders (`pdfs/`, `docs/`, `audio/`). Folder tracked, contents gitignored.

## Commands

```bash
# Add the dev/test/build commands relevant to this project
```

Linter handles code style — don't enforce style in prompts.

## Don'ts

- Don't read `.apsolut/notes/` — raw + exploration + draft plans, not for CC
- Don't preload files — fetch only what the current task needs
- Don't sprinkle binaries across `.apsolut/` markdown folders — they belong in `.apsolut/screenshots/` (hot path) or `.apsolut/files/` (cold storage); on davinci, that's `.apsolut/08-screenshots/` and `.apsolut/07-files/`
