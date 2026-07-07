# apsolut-seshat

The `.apsolut/` convention — a markdown vault for project notes, decisions, ops, and fires.
Named for the Egyptian goddess of records, measurement, and architecture.

Works on weekend projects, scales to teams.

> Standalone — works on its own. Optional pairing: [apsolut-cortex](https://github.com/apsolut/apsolut-cortex) for Claude's cross-project memory.

## Get started

Install as a Claude Code plugin (two steps — add the repo as a marketplace, then install):

```bash
/plugin marketplace add apsolut/apsolut-seshat
/plugin install apsolut-seshat@apsolut-seshat
```

Then in any project:

```bash
/apsolut-seshat           # picks a profile interactively
/apsolut-seshat bare      # 3 files at .apsolut/ root, no folders
/apsolut-seshat standard  # 6 folders, the default for active projects
/apsolut-seshat davinci   # parallel track: 6 numbered folders for solo creative/research work
```

The vault comes with a command family — type `/apsolut` to see them all:

| Command | What it does |
|---------|-------------|
| `/apsolut-seshat` | Scaffold the vault (profiles above) |
| `/apsolut-next` | Pick up the next task: brief, branch, work it, move it through next → doing → done |
| `/apsolut-plan` | Show the active plan with per-task progress; offer to slice unsliced steps into tasks |
| `/apsolut-maintain` | Scan and fix broken `[[wiki-links]]`, orphans, stale tags, naming drift |

Open `.apsolut/` as an Obsidian vault for graph view, templates, and search.

## Pick a size

The vault scales with the project. Start small, add folders as content grows.
See [EXAMPLE.md](EXAMPLE.md) for what each profile looks like in use — lived-in trees with realistic entries, plus the capture→task flow end to end.

| Profile      | What you get                                                                                                                                  | When                                                              |
|--------------|-----------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| **bare**     | 3 files at `.apsolut/` root: `TASKS.md`, `DECISIONS.md`, `FIRES.md` — no folders                                                              | Weekend project, < 10 entries total                                |
| **minimal**  | folders: `tasks/`, `decisions/`, `fires/`                                                                                                      | Promote from bare when any one file > ~10 entries                  |
| **standard** *(default)* | + `notes/`, `docs/`, `services/`, `ops/`                                                                                          | Most active projects                                               |
| **full**     | + split `notes/` → `inbox/` + `explore/` + `plans/`; split `ops/` → `guides/` + `runbooks/` + `rules/`; add inbox subfolders                   | Team project with planning ceremonies + ongoing ops                |
| **davinci**  | 8 numbered folders: `01-ideas/` … `06-knowledge/`, plus `07-files/` + `08-screenshots/` for binaries. No fires, no un-numbered drop zones | Solo creative or research work — notebook/sketchbook mindset       |

Promote bare → minimal → standard → full as the project grows. Splitting is mechanical: take the one file, slice by frontmatter, write the slices into folders.

**davinci is a parallel track**, not on the promotion path. Pick it when you want flow-state notebook structure (ideas → plan → audits → decisions) instead of the operations-friendly bare→full layout. `04-library/` holds markdown excerpts; binaries get their own numbered folders — `07-files/` (PDFs, audio, exports) and `08-screenshots/` (images) — linked from anywhere. That's a deliberate divergence from bare→full's un-numbered `screenshots/`+`files/`: davinci keeps everything numbered so the vault reads as one ordered notebook. Each binary folder's `000-template.md` is a Claude-maintained manifest (the injection map — Claude reads the table to find one file rather than loading the folder). `08-screenshots/` and `07-files/` support arbitrary subfolders (bugs/, playwright/, debug/, client-foo/, etc.); automated/test screenshots stay out of the vault. `03-audits/` is for periodic reviews and synthesis after changes — ideal for agentic workflows (review slop, dead code, comprehension debt in IDE + audits, per shifts like Karpathy's). davinci has no `fires/` log.

The folder set shown below is the **standard** profile.

## Folders (standard)

| Folder | What goes here | Who reads it |
|--------|---------------|--------------|
| `notes/` | Capture pipeline — raw input, exploration, feature plans (slice by `stage:` frontmatter) | You |
| `tasks/` | Small, self-contained work for Claude Code | CC (one file per session) |
| `docs/` | Reference material — schemas, API, glossary, onboarding | CC on demand |
| `decisions/` | Why we built it that way | CC on demand |
| `ops/` | How-to + recovery + constraints (slice by `kind:` frontmatter) | CC before deploy/debug, on-call when broken, before touching user data |
| `services/` | External APIs — limits, costs, TOS | CC before adding API calls |
| `fires/` | What broke and why — see categories below | CC to avoid repeating mistakes |

## Flow

```
notes (raw → exploring → planned) → tasks → done
```

Plans live in `notes/` as `stage: planned`; they get their own `plans/` folder in the full profile (davinci: `02-plan/`). `tasks/` is the *output* of planning — self-contained specs sliced from a plan.

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
- Markdown only — except two drop zones inside `.apsolut/`:
  - `.apsolut/screenshots/` — hot path. Drop bug/UI/debug screenshots. **Subfolders are fully encouraged** (`bugs/`, `inspiration/`, `playwright/`, `debug/2026-07/`, `client-foo/`, or anything you invent). Reference inline. Force-add keepers.
  - `.apsolut/files/` — cold storage. PDFs, audio, exports, design assets, etc. — **add any subfolders you want** on demand. Use the manifest (davinci) or links so nothing gets bulk-loaded.
  - Both folders are tracked in git; contents are gitignored. `git add -f` the keepers.
- This vault is for things you write on purpose; if you also use apsolut-cortex, that's where Claude's auto-learned memory lives
- **The vault is a garden, not an archive.** When a decision is reached, prune the alternatives it killed — and any decision it supersedes. The decision's "options considered" keeps the *why-not*, and git keeps the history, so nothing is truly lost. Claude proposes the deletions; you confirm. This keeps the live vault to what's true *now*
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
ON DEMAND:        .apsolut/ops/gdpr.md (only if task touches user data — kind: constraint)
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
- Services: .apsolut/services/ (read before adding API calls)
- Ops: .apsolut/ops/ (read before deploy/debug/setup, when broken, or before touching user data — slice by `kind:`)
- Fires: .apsolut/fires/ (read before changing areas that broke before)

# Commands
- npm run dev, npm test, npm run lint
- Linter handles code style — don't enforce style in prompts

# Don'ts
- Don't read .apsolut/notes/
- Don't preload files — fetch only what the current task needs
- Don't sprinkle binaries across .apsolut/ markdown folders — they belong in .apsolut/screenshots/ (hot) or .apsolut/files/ (cold)
```

What does NOT belong in CLAUDE.md:
- Architecture explanations (put in `.apsolut/docs/`)
- Code style rules (put in ESLint/Prettier config)
- Full API documentation (put in `.apsolut/docs/`)
- Decision history (put in `.apsolut/decisions/`)
- Onboarding guides (put in `.apsolut/ops/` with `kind: howto`)

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

Run `/apsolut-maintain` in Claude Code to scan and fix:
- Broken `[[links]]` pointing to files that don't exist
- Orphaned files with no incoming links
- Plain text references that should be `[[wiki-links]]`
- Stale `#tags` used by only one file
- Naming drift from the `001-kebab-case.md` convention

The command works in 4 passes using grep — never loads all files at once.

For agentic workflows (mostly agent + review in IDE, per shifts like Karpathy's): 
- Write specs in `02-plan/` with clear "done when", verification steps (tests/criteria first), then let agents loop.
- After changes (e.g. 10 commits or refactors), create/update an audit in `03-audits/` (davinci) or synthesis to review for slop (overcomplication, dead code), comprehension debt, and hygiene.
- Leverage: give agents success criteria and watch them go; use our structure (plans as specs, audits for review) to stay in control.
- Always review in IDE + our tools (`/apsolut-maintain`, audits). Combat slop and maintain long-term memory via `03-audits/`, `05-decisions/`, `06-knowledge/`.

See the embedded Principal AI & Systems Architect role in the generated `CLAUDE.md` for full guidance on rigor, review, and modes.

## AI Operating Role

When working on this project or any project scaffolded with this boilerplate, the AI operates as the **Principal AI & Systems Architect (Unified Engine)**.

The full role definition (pillars, archetypes, audit/generation modes, algorithmic rigor, structural UI, AEO) is embedded at the top of the generated `CLAUDE.md` (from the template in `references/claude-md.md`).

This role takes precedence for all interactions with the codebase. Follow the operational modes: Audit & Optimization or Generation & Build, always preferring deterministic algorithmic solutions over LLM brute-force.
