---
name: apsolut-seshat
description: Scaffold the .apsolut/ vault into the current project. Pick a profile (bare/minimal/standard/full), create the files + folders + CLAUDE.md + artifacts/. Idempotent — audits and reports gaps if .apsolut/ already exists.
argument-hint: [bare|minimal|standard|full]
---

# apsolut-seshat

> Part of [apsolut-seshat](https://github.com/apsolut/apsolut-seshat). Source of truth lives in that repo.

Scaffold the `.apsolut/` vault for any project, sized by profile.

## Trigger

User invokes `/apsolut-seshat` (optionally with a profile: `/apsolut-seshat bare`, `minimal`, `standard`, `full`).

## Profiles

| Profile      | What you get                                                                                                                                                       | Use when                                                  |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| **bare**     | 3 files at `.apsolut/` root: `TASKS.md`, `DECISIONS.md`, `FIRES.md` — no folders                                                                                    | Weekend project, < 10 entries total                       |
| **minimal**  | folders: `tasks/`, `decisions/`, `fires/`                                                                                                                            | Promote from bare when any one file > ~10 entries         |
| **standard** *(default)* | minimal + `notes/`, `docs/`, `services/`, `ops/`                                                                                                       | Most active projects                                      |
| **full**     | standard + split `notes/` → `inbox/`+`explore/`+`blueprints/`, split `ops/` → `guides/`+`runbooks/`+`rules/`, inbox subfolders (bookmarks, bugs, feedback, ideas, meetings, voice) | Team project with planning ceremonies + ongoing ops       |

All profiles also create `artifacts/` at project root (sibling to `.apsolut/`).

See `references/profiles.md` for the canonical manifest.

## Instructions

Execute steps in order. Read templates from `references/` in this skill directory.

### Step 1 — Detect state

Check at project root:
- `.apsolut/` directory
- `.claude/` directory
- `README.md`, `CLAUDE.md`, `.gitignore`
- Project metadata: `package.json`, `Cargo.toml`, `pyproject.toml`, `go.mod`, `composer.json`

States:

| Detected                                                                | Action                                                                                                            |
|-------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| `.apsolut/` does not exist                                              | Proceed to Step 2                                                                                                 |
| `.apsolut/` exists matching current spec                                | **Audit mode** — compare against chosen profile, report gaps, ask before adding                                   |
| `.apsolut/brain/` exists (very old convention)                          | **Stop and warn.** Tell user to rename to `.apsolut-old/` and re-run init. Don't merge automatically              |
| `.apsolut/` has `inbox/` or `guides/` (pre-collapse layout)             | Tell user the spec moved on — they're on the old maximalist layout. Suggest splitting/merging by hand, or staying on it (it still works)             |

**Never overwrite existing files.**

### Step 2 — Pick a profile

If `$ARGUMENTS` contains a profile name, use it.
Otherwise recommend based on detection:

- No `package.json`/`Cargo.toml`, no `tests/`, no `docs/`, < 20 source files → **bare**
- Single-language repo with tests OR docs → **minimal**
- Has tests + docs + dependencies → **standard** (default)
- Monorepo, multiple feature areas, or CI workflows → **full**

Confirm with user. Then ask:
- `{{ONE_LINE_DESCRIPTION}}` — what does this project do?
- `{{ONE_LINE_GOAL}}` — current goal? (optional)

### Step 3 — Create `.apsolut/`

**bare profile:**
- Create `.apsolut/` directory
- Copy `references/templates/bare-tasks.md` → `.apsolut/TASKS.md`
- Copy `references/templates/bare-decisions.md` → `.apsolut/DECISIONS.md`
- Copy `references/templates/bare-fires.md` → `.apsolut/FIRES.md`
- Create `.apsolut/screenshots/.gitkeep` and `.apsolut/files/.gitkeep` (see [Step 3b](#step-3b--binary-drop-zones-all-profiles))
- No other subdirectories.

**minimal / standard / full profiles:**
- Create folders per `references/profiles.md`
- For `tasks/`, create three subfolders: `next/`, `doing/`, `done/` — each gets `000-template.md` from `references/templates/tasks-template.md`
- For each other leaf folder, copy `references/templates/<folder>-template.md` → `<folder>/000-template.md`
- Create `.apsolut/screenshots/.gitkeep` and `.apsolut/files/.gitkeep` (see [Step 3b](#step-3b--binary-drop-zones-all-profiles))
- **full only:** create the 6 inbox subfolders (bookmarks, bugs, feedback, ideas, meetings, voice) — each gets a `.gitkeep` and re-uses `inbox-template.md`

### Step 3b — Binary drop zones (all profiles)

Every profile gets two binary drop zones inside `.apsolut/`:

- **`screenshots/`** — hot path. User drops bug/UI screenshots and references them inline in conversation (e.g. "look at .apsolut/screenshots/login-broken.png").
- **`files/`** — cold storage. PDFs, audio, exports, anything else binary. No preset subfolders — user creates `pdfs/`, `docs/`, `audio/`, etc. on demand.

Both folders are tracked in git via `.gitkeep`; contents are gitignored — see [Step 5](#step-5--update-gitignore). This is the one exception to the "`.apsolut/` is markdown only" rule, and it replaces the older top-level `artifacts/` folder.

### Step 4 — (removed)

The old top-level `project/artifacts/` folder is no longer created. Binaries live inside `.apsolut/` (see Step 3b). If audit mode detects a legacy `project/artifacts/`, tell the user it still works but suggest moving its contents into `.apsolut/files/` (don't auto-move — they may have tooling that points at the old path).

### Step 5 — Update `.gitignore`

If `.gitignore` doesn't exist, create it. Append (if not already present):

```
# apsolut: keep the drop-zone folders, ignore their contents
.apsolut/screenshots/*
!.apsolut/screenshots/.gitkeep
.apsolut/files/*
!.apsolut/files/.gitkeep
```

Both drop zones are ephemeral by default. If a specific screenshot or file matters long-term (e.g. evidence for a fire entry, a signed contract PDF), force-add it with `git add -f`.

See `references/gitignore-snippet.md` for details.

### Step 6 — `CLAUDE.md`

If missing, create from `references/claude-md.md`. Substitute template variables.

If exists, skip. Note in the summary that user should verify it points to `.apsolut/`.

### Step 7 — `README.md`

If missing, create from `references/root-readme.md`. Substitute variables.

If exists, skip.

### Step 8 — Summary

```
## apsolut-seshat complete

Profile: <bare|minimal|standard|full>

### Created
- list of files/dirs

### Skipped (already existed)
- list

### Next steps
- bare:     append your first task in .apsolut/TASKS.md
- minimal+: add your first task in .apsolut/tasks/next/001-<name>.md
- Log near-misses in fires — they're tomorrow's outages
- Promote to next profile when files grow: `/apsolut-seshat <next-profile>`
```

## Audit mode (when `.apsolut/` already exists)

Compare existing layout against the chosen profile. Report:
- Files/folders present + match profile ✓
- Missing for this profile (offer to add)
- Present but not in this profile (likely from a richer profile — leave them)
- Missing `000-template.md` in any present folder (offer to add)
- Missing `.apsolut/screenshots/` or `.apsolut/files/` (offer to add — these are required in all profiles now)
- Legacy `project/artifacts/` at project root (note: convention moved binaries to `.apsolut/files/`. Suggest moving contents — don't auto-move, may break tooling that points at the old path)

Ask user before adding. **Never delete.**

## Template variables

| Variable                  | Source                                                        | Fallback           |
|---------------------------|---------------------------------------------------------------|--------------------|
| `{{PROJECT_NAME}}`        | package.json / Cargo.toml / pyproject.toml / dir name         | directory name     |
| `{{ONE_LINE_DESCRIPTION}}`| Ask user in Step 2                                            | "A new project"    |
| `{{ONE_LINE_GOAL}}`       | Ask user in Step 2                                            | ""                 |
| `{{TECH_STACK}}`          | Auto-detect (package.json → Next/React, Cargo → Rust, etc.)   | "Not detected"     |
| `{{DATE}}`                | Today (YYYY-MM-DD)                                            | today              |
