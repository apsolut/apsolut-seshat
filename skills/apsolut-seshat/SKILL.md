---
name: apsolut-seshat
description: Scaffold the .apsolut/ vault into the current project. Pick a profile (bare/minimal/standard/full/davinci), create the files + folders + CLAUDE.md + binary drop zones. Idempotent — audits and reports gaps if .apsolut/ already exists.
argument-hint: [bare|minimal|standard|full|davinci]
---

# apsolut-seshat

> Part of [apsolut-seshat](https://github.com/apsolut/apsolut-seshat). Source of truth lives in that repo.

Scaffold the `.apsolut/` vault for any project, sized by profile.

## Trigger

User invokes `/apsolut-seshat` (optionally with a profile: `/apsolut-seshat bare`, `minimal`, `standard`, `full`, `davinci`).

## Profiles

| Profile      | What you get                                                                                                                                                       | Use when                                                  |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| **bare**     | 3 files at `.apsolut/` root: `TASKS.md`, `DECISIONS.md`, `FIRES.md` — no folders                                                                                    | Weekend project, < 10 entries total                       |
| **minimal**  | folders: `tasks/`, `decisions/`, `fires/`                                                                                                                            | Promote from bare when any one file > ~10 entries         |
| **standard** *(default)* | minimal + `notes/`, `docs/`, `services/`, `ops/`                                                                                                       | Most active projects                                      |
| **full**     | standard + split `notes/` → `inbox/`+`explore/`+`plans/`, split `ops/` → `guides/`+`runbooks/`+`rules/`, inbox subfolders (bookmarks, bugs, feedback, ideas, meetings, voice) | Team project with planning ceremonies + ongoing ops       |
| **davinci**  | 8 numbered folders: `01-thinking/` … `06-knowledge/`, plus `07-files/` + `08-screenshots/` for binaries. No fires. No un-numbered drop zones | Solo creative or research work (notebook/sketchbook mindset) |

davinci is a parallel track, not on the bare→full promotion path. It has no `fires/`, and instead of the shared un-numbered `screenshots/`+`files/` zones it keeps binaries in its own numbered folders — `07-files/` (cold) and `08-screenshots/` (hot) — so the vault stays an all-numbered notebook.

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
| `.apsolut/` has `blueprints/` (pre-rename)                              | Folder was renamed to `plans/` (stage `blueprinted` → `planned`). Suggest `git mv blueprints plans` — don't auto-rename. Old name still works        |

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

**davinci profile:**
- Create `.apsolut/` directory
- Create 8 numbered folders: `01-thinking/`, `02-ideas/`, `03-plan/`, `04-library/`, `05-decisions/`, `06-knowledge/`, `07-files/`, `08-screenshots/`
- For each folder, copy the matching `references/templates/davinci-<NN-name>.md` → `<folder>/000-template.md` (e.g. `04-library/` uses `davinci-04-library.md`, `07-files/` uses `davinci-07-files.md`, `08-screenshots/` uses `davinci-08-screenshots.md`)
- `07-files/` and `08-screenshots/` `000-template.md` are **manifests** — a markdown table the user/Claude fill in (one row per kept binary) so binaries are findable without loading the folder. `08-screenshots/` splits images by intent into `inspiration/` (keep) and `bugs/` (ephemeral) subfolders, created **on demand** (not pre-scaffolded). Automated/Playwright/test screenshots do **not** go here — they're regenerated test output.
- **Do not** create the un-numbered `screenshots/`/`files/` drop zones — davinci keeps binaries in numbered `07-files/`+`08-screenshots/` instead (Step 3b is skipped for davinci)
- **Do not** create `fires/` — davinci has no incident track
- Apply the davinci gitignore block in [Step 5](#step-5--update-gitignore)

### Step 3b — Binary drop zones (bare/minimal/standard/full only — skip for davinci)

The bare→full profiles get two un-numbered binary drop zones inside `.apsolut/`. davinci skips this step — it uses the numbered `07-files/`+`08-screenshots/` folders created in the davinci block above.

- **`screenshots/`** — hot path. User drops bug/UI screenshots and references them inline in conversation (e.g. "look at .apsolut/screenshots/login-broken.png").
- **`files/`** — cold storage. PDFs, audio, exports, anything else binary. No preset subfolders — user creates `pdfs/`, `docs/`, `audio/`, etc. on demand.

Both folders are tracked in git via `.gitkeep`; contents are gitignored — see [Step 5](#step-5--update-gitignore). This is the one exception to the "`.apsolut/` is markdown only" rule, and it replaces the older top-level `artifacts/` folder.

### Step 4 — (removed)

The old top-level `project/artifacts/` folder is no longer created. Binaries live inside `.apsolut/` (bare→full: see Step 3b; davinci: `07-files/`+`08-screenshots/`). If audit mode detects a legacy `project/artifacts/`, tell the user it still works but suggest moving its contents into the cold-storage folder (`.apsolut/files/` for bare→full, `.apsolut/07-files/` for davinci) — don't auto-move, they may have tooling that points at the old path.

### Step 5 — Update `.gitignore`

If `.gitignore` doesn't exist, create it. Append the block for the chosen track (if not already present):

**bare/minimal/standard/full:**

```
# apsolut: keep the drop-zone folders, ignore their contents
.apsolut/screenshots/*
!.apsolut/screenshots/.gitkeep
.apsolut/files/*
!.apsolut/files/.gitkeep
```

**davinci** (numbered binary folders; keep each `000-template.md`):

```
# apsolut (davinci): keep the binary folders + their templates, ignore the binaries
.apsolut/07-files/*
!.apsolut/07-files/000-template.md
.apsolut/08-screenshots/*
!.apsolut/08-screenshots/000-template.md
```

Binary contents are ephemeral by default. If a specific screenshot or file matters long-term (e.g. evidence for a fire entry, a signed contract PDF), force-add it with `git add -f`.

See `references/gitignore-snippet.md` for details.

### Step 6 — `CLAUDE.md`

If missing, create from `references/claude-md.md`. Substitute template variables.

**For davinci:** the template's Pointers section lists bare→full folder names (`tasks/next/`, `decisions/`, `ops/`, `fires/`…). Replace them with davinci's folders — `01-thinking/`, `02-ideas/`, `03-plan/`, `04-library/`, `05-decisions/`, `06-knowledge/`, and the binary zones `07-files/`/`08-screenshots/` — and drop the fires/ops/services/tasks pointers davinci doesn't have. Keep the binary-injection guidance: to find a binary, read the folder's `000-template.md` manifest (never bulk-load the folder); pull inspiration shots only for related design, bug shots only when debugging that issue.

If exists, skip. Note in the summary that user should verify it points to `.apsolut/`.

### Step 7 — `README.md`

If missing, create from `references/root-readme.md`. Substitute variables.

If exists, skip.

### Step 8 — Summary

```
## apsolut-seshat complete

Profile: <bare|minimal|standard|full|davinci>

### Created
- list of files/dirs

### Skipped (already existed)
- list

### Next steps
- bare:     append your first task in .apsolut/TASKS.md
- minimal+: add your first task in .apsolut/tasks/next/001-<name>.md
- davinci:  start in .apsolut/01-thinking/001-<name>.md and let it flow
- Log near-misses in fires — they're tomorrow's outages (bare→full only)
- Promote to next profile when files grow: `/apsolut-seshat <next-profile>` (bare→full track only; davinci is standalone)
```

## Audit mode (when `.apsolut/` already exists)

Compare existing layout against the chosen profile. Report:
- Files/folders present + match profile ✓
- Missing for this profile (offer to add)
- Present but not in this profile (likely from a richer profile — leave them)
- Missing `000-template.md` in any present folder (offer to add)
- Missing binary drop zones (offer to add): for bare/minimal/standard/full → `.apsolut/screenshots/` + `.apsolut/files/`; for davinci → `.apsolut/07-files/` + `.apsolut/08-screenshots/`
- Legacy `project/artifacts/` at project root (note: convention moved binaries into `.apsolut/` — `files/` for bare→full, `07-files/` for davinci. Suggest moving contents — don't auto-move, may break tooling that points at the old path)
- **davinci with un-numbered `screenshots/`/`files/`** (an older davinci setup): do **not** flag or "fix" these — some davinci projects deliberately added them. Treat as "present but not in profile — leave them."
- **davinci `07-files/`/`08-screenshots/` with a bare (pre-manifest) `000-template.md`**: offer to upgrade it to the manifest format (the table + `inspiration/`/`bugs/` intent convention). Don't force — existing binaries and rows are untouched.

Ask user before adding. **Never delete.**

## Template variables

| Variable                  | Source                                                        | Fallback           |
|---------------------------|---------------------------------------------------------------|--------------------|
| `{{PROJECT_NAME}}`        | package.json / Cargo.toml / pyproject.toml / dir name         | directory name     |
| `{{ONE_LINE_DESCRIPTION}}`| Ask user in Step 2                                            | "A new project"    |
| `{{ONE_LINE_GOAL}}`       | Ask user in Step 2                                            | ""                 |
| `{{TECH_STACK}}`          | Auto-detect (package.json → Next/React, Cargo → Rust, etc.)   | "Not detected"     |
| `{{DATE}}`                | Today (YYYY-MM-DD)                                            | today              |
