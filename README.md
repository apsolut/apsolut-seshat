# .apsolut/

Project brain. Lives next to `.claude/`.

Open this folder as an Obsidian vault for graph view, templates, and search.

> Standalone — works on its own. Optional pairing: [apsolut-cortex](https://github.com/apsolut/apsolut-cortex) for Claude's cross-project memory.

## Folders

| Folder | What goes here | Who reads it |
|--------|---------------|--------------|
| `notes/` | Capture pipeline — raw input, exploration, feature plans (slice by `stage:` frontmatter) | You |
| `tasks/` | Small, self-contained work for Claude Code | CC (one file per session) |
| `docs/` | Reference material — schemas, API, glossary, onboarding | CC on demand |
| `decisions/` | Why we built it that way | CC on demand |
| `guides/` | How to do things — deploy, debug, setup | CC on demand |
| `runbooks/` | Ops recovery — when X breaks, do Y | CC + on-call when something is broken |
| `rules/` | Legal, GDPR, TOS — hard constraints | CC before touching user data |
| `services/` | External APIs — limits, costs, TOS | CC before adding API calls |
| `fires/` | What broke and why — see categories below | CC to avoid repeating mistakes |

## Flow

```
notes (raw → exploring → blueprinted) → tasks → done
```

## Fire categories

Log every incident, even near misses. Pick the category that fits:

| Category | What happened | Example |
|----------|--------------|---------|
| `outage` | App or feature stopped working | Auth provider down, DB pool exhausted |
| `data` | Wrong, lost, or leaked data | User saw another user's data, migration corrupted rows |
| `third-party` | External service broke your app | YouTube quota hit, Stripe webhook format changed |
| `cost-spike` | Unexpected bill | Infinite retry loop on Claude API, missing cache headers |
| `near-miss` | Caught before users noticed | Hardcoded dev key deployed, migration ran on prod |
| `performance` | Works but slow — users leave | Dashboard 12s with 500 records, N+1 query |
| `security` | Unauthorized access or exposure | API endpoint without auth, JWT secret in git |
| `deploy` | Release itself went wrong | Missing env var, migration out of order, rollback failed |

Near misses are the most valuable — free lessons without the damage.

## Conventions

- Files use `001-descriptive-name.md` numbering (per folder)
- Each folder has `000-template.md` — read it before creating new entries
- Markdown only — no binaries, screenshots, or tool dumps (those go in `artifacts/`)
- This vault is for things you write on purpose; if you also use apsolut-cortex, that's where Claude's auto-learned memory lives
- Use `[[wiki-links]]` for all cross-references (Obsidian graph + CC compatible)
- Use `tags:` with `#hashtags` for grouping related files across folders

## Obsidian setup

Open `.apsolut/` as a vault in Obsidian. Not the whole project — just this folder.

### What to do

- Add `.obsidian/` to your `.gitignore` (it's local preferences, not project data)
- Install **Templater** plugin — use `000-template.md` files as templates
- Install **Dataview** plugin — query frontmatter (e.g. "all bugs with status: new")
- Install **Kanban** plugin (optional) — visualize `tasks/` as a board

### Graph view

Graph view works automatically. It reads two things:

- `[[wiki-links]]` — every link becomes a connection in the graph
- `#tags` — every tag becomes a hub node with lines to all files using it

No config needed. Just use `[[brackets]]` in `from:` fields and body refs.

### What doesn't change

- CC reads raw markdown — it doesn't care about Obsidian
- `[[wiki-links]]` are just text to CC — it can still follow them by path
- The folder structure is the same
- The frontmatter is the same
- Nothing about context discipline changes

## Claude Code

**Golden rule: don't pollute context.**

`.apsolut/` and `.claude/` are separate systems with different jobs.
Claude Code's context window is expensive and finite — every token loaded
is a token not available for thinking. Respect the boundary.

### What goes where

| | `.claude/` | `.apsolut/` |
|---|---|---|
| **Purpose** | CC's runtime config | Your brain + pipeline |
| **CLAUDE.md** | Under 60 lines. Pointers only, never content. | N/A |
| **Skills** | CC-triggered skills (SKILL.md convention) | N/A — `.claude/` owns all skills |
| **Commands** | Slash commands for CC | N/A |
| **Agents** | Subagent definitions (isolated context each) | N/A |
| **Everything else** | Does not belong here | Lives here |

### What CC reads per session

CC picks up **one task file** from `tasks/next/` — that's the entire briefing.
Everything else is pulled **on demand** only when the task references it:

```
ALWAYS loaded:    .claude/CLAUDE.md (< 60 lines, pointers only)
PER SESSION:      .apsolut/tasks/next/001-whatever.md (self-contained)
ON DEMAND:        .apsolut/decisions/003.md (only if task says "see decisions/003")
ON DEMAND:        .apsolut/services/002.md (only if task touches that API)
ON DEMAND:        .apsolut/rules/gdpr.md (only if task touches user data)
NEVER:            .apsolut/notes/ (raw + exploration + draft plans — CC has no business here)
```

### CLAUDE.md stays lean

Don't dump architecture, style guides, or project history into CLAUDE.md.
It loads into **every single session** — bloat here costs you everywhere.

What belongs in CLAUDE.md:
```
# Pointers
- Current tasks: .apsolut/tasks/next/
- Decisions: .apsolut/decisions/
- Rules: .apsolut/rules/ (read before touching user data or external APIs)
- Services: .apsolut/services/ (read before adding API calls)
- Guides: .apsolut/guides/ (read before deploy, debug, or setup)
- Fires: .apsolut/fires/ (read before changing areas that broke before)

# Commands
- npm run dev, npm test, npm run lint
- Linter handles code style — don't enforce style in prompts

# Don'ts
- Don't read .apsolut/notes/
- Don't preload files — fetch only what the current task needs
- Don't put screenshots or binaries in .apsolut/ (use artifacts/)
```

What does NOT belong in CLAUDE.md:
- Architecture explanations (put in `.apsolut/docs/`)
- Code style rules (put in ESLint/Prettier config)
- Full API documentation (put in `.apsolut/docs/`)
- Decision history (put in `.apsolut/decisions/`)
- Onboarding guides (put in `.apsolut/guides/`)

### Context budget per session

```
CLAUDE.md           ~200 tokens (pointers only)
Task file           ~300 tokens (self-contained spec)
Referenced files     ~500 tokens (1-2 files, on demand)
Source code          CC greps/globs as needed
─────────────────────────────────
Total overhead       ~1000 tokens — the rest is for thinking
```

If CC needs to chase more than 2 references to understand a task,
the task isn't self-contained enough. Rewrite it.

### Maintenance

Run `/maintain` in Claude Code to scan and fix:
- Broken `[[links]]` pointing to files that don't exist
- Orphaned files with no incoming links
- Plain text references that should be `[[wiki-links]]`
- Stale `#tags` used by only one file
- Naming drift from the `001-kebab-case.md` convention

The command works in 4 passes using grep — never loads all files at once.
