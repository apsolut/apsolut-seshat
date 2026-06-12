# What a vault looks like in use

The [README](README.md) explains the convention; `skills/apsolut-seshat/references/profiles.md` is the canonical scaffold. This file shows the part neither can: a vault that's been **lived in** for a few weeks.

One fictional project runs through bare→full: **mealdrop** — a meal-planning app that imports recipes from YouTube. The davinci example is a separate solo-research notebook, because that's what davinci is for.

---

## bare

Three files, no folders. Entries are sections inside each file.

```
.apsolut/
├── TASKS.md          # checklist — "import youtube playlist", "fix login redirect"
├── DECISIONS.md      # 4 entries — "chose SQLite", "no user accounts for MVP"…
├── FIRES.md          # 1 entry — "youtube quota hit on day 2 (near-miss)"
├── screenshots/      # login-broken.png  ← gitignored, referenced inline
└── files/            # empty until needed
```

When any one file passes ~10 entries, slice it into a folder → **minimal**.

## minimal

Same three concerns, now one file per entry.

```
.apsolut/
├── tasks/
│   ├── next/
│   │   ├── 003-paginate-recipe-list.md
│   │   └── 004-add-import-progress-bar.md
│   ├── doing/
│   │   └── 002-rate-limit-youtube-import.md
│   └── done/
│       └── 001-import-youtube-playlist.md
├── decisions/
│   ├── 001-chose-sqlite.md
│   └── 002-no-accounts-for-mvp.md
├── fires/
│   └── 001-youtube-quota-near-miss.md
├── screenshots/
└── files/
```

## standard *(default)*

Adds the capture pipeline (`notes/`), reference (`docs/`, `services/`), and ops.

```
.apsolut/
├── notes/
│   ├── 001-voice-memo-meal-prep-mode.md      # stage: raw
│   ├── 002-compare-recipe-parsers.md         # stage: exploring
│   └── 003-weekly-shopping-list.md           # stage: planned → sliced into tasks 005+006
├── tasks/
│   ├── next/
│   │   ├── 005-shopping-list-data-model.md   # from: [[notes/003-weekly-shopping-list]]
│   │   └── 006-shopping-list-ui.md
│   ├── doing/
│   │   └── 002-rate-limit-youtube-import.md
│   └── done/
│       └── 001-import-youtube-playlist.md
├── decisions/
│   ├── 001-chose-sqlite.md
│   └── 003-chose-redis-for-import-queue.md
├── docs/
│   └── 001-recipe-schema.md
├── services/
│   └── 001-youtube-data-api.md               # quota: 10k units/day — read before adding API calls
├── ops/
│   ├── 001-deploy-to-fly.md                  # kind: howto
│   ├── 002-import-queue-stuck.md             # kind: recovery
│   └── 003-gdpr-recipe-authors.md            # kind: constraint
├── fires/
│   ├── 001-youtube-quota-near-miss.md
│   └── 002-import-loop-cost-spike.md
├── screenshots/
└── files/
    └── pdfs/                                  # signed-api-tos.pdf — git add -f'd
```

## full

`notes/` splits into `inbox/` + `explore/` + `plans/`; `ops/` splits into `guides/` + `runbooks/` + `rules/`.

```
.apsolut/
├── inbox/
│   ├── bookmarks/001-hn-thread-recipe-ml.md
│   ├── bugs/001-safari-import-button-dead.md
│   ├── feedback/001-beta-user-wants-export.md
│   ├── ideas/001-meal-prep-mode.md
│   ├── meetings/001-2026-06-02-kickoff.md
│   └── voice/001-commute-braindump.md
├── explore/
│   └── 001-recipe-parser-options.md          # option A: regex, option B: LLM extraction
├── plans/
│   └── 001-shopping-list-feature.md          # goal, steps, done-when → tasks 005–008
├── tasks/
│   ├── next/   … doing/   … done/
├── decisions/
│   └── 004-llm-extraction-over-regex.md      # from: [[explore/001-recipe-parser-options]]
├── docs/
│   └── 001-recipe-schema.md
├── services/
│   └── 001-youtube-data-api.md
├── guides/
│   └── 001-local-dev-setup.md                # was ops/ kind: howto
├── runbooks/
│   └── 001-import-queue-stuck.md             # was ops/ kind: recovery
├── rules/
│   └── 001-gdpr-recipe-authors.md            # was ops/ kind: constraint
├── fires/
├── screenshots/
└── files/
```

## davinci *(parallel track)*

A different project — solo research into generative sound art. All numbered, no fires, binaries in numbered folders with manifests.

```
.apsolut/
├── 01-thinking/
│   └── 001-what-if-rain-data-drove-synths.md
├── 02-ideas/
│   └── 001-three-sonification-approaches.md
├── 03-plan/
│   └── 001-prototype-rain-to-midi.md         # status: active — steps, done-when
├── 04-library/
│   └── 001-xenakis-formalized-music-excerpts.md
├── 05-decisions/
│   └── 001-supercollider-over-max.md
├── 06-knowledge/
│   └── 001-midi-cc-cheatsheet.md
├── 07-files/
│   ├── 000-template.md                       # MANIFEST — table: one row per kept binary
│   └── field-recording-berlin-rain.wav       # git add -f'd, row in manifest
└── 08-screenshots/
    ├── 000-template.md                       # MANIFEST + intent subfolders
    ├── inspiration/ryoji-ikeda-datamatics.png
    └── bugs/                                  # ephemeral — empty right now, good
```

---

## The flow, end to end

How one idea moved through the standard-profile vault above:

**1. Captured raw** — `notes/003-weekly-shopping-list.md`:

```markdown
---
stage: raw
type: idea
status: active
---
# users keep asking for a shopping list from their meal plan
voice memo from the gym: "...just sum the ingredients across the week..."
```

**2. Explored, then committed** — same file, stage advanced `raw → exploring → planned`:

```markdown
---
stage: planned
status: promoted
---
## planned
- problem: meal plan exists, ingredients don't aggregate
- how: data model first, then UI
- tasks: [[tasks/next/005-shopping-list-data-model]], [[tasks/next/006-shopping-list-ui]]
- done when: weekly plan exports a deduplicated ingredient list
```

**3. Sliced into a task** — `tasks/next/005-shopping-list-data-model.md`, self-contained, ~300 tokens. This is the **only** vault file Claude Code reads for the session:

```markdown
---
from: "[[notes/003-weekly-shopping-list]]"
size: s
branch: feat/005-shopping-list-model
---
# Shopping list data model
## accept
- ingredients dedupe across recipes (2 onions + 1 onion = 3 onions)
- units normalize (500g + 1kg = 1.5kg)
- 6 tests passing in tests/shopping/
## refs
- [[docs/001-recipe-schema]] — ingredient shape
```

**4. A decision got locked along the way** — `decisions/003-chose-redis-for-import-queue.md` keeps the why-not (and the exploration note that produced it gets pruned — garden, not archive).

**5. Something broke; it's a fire now** — `fires/002-import-loop-cost-spike.md` (`category: cost-spike`) ends with prevention: a new task and a runbook update. Next time anyone touches the import queue, CC reads that fire first.

That's the whole loop: **capture → explore → plan → task → done**, with decisions and fires as the permanent record on the side.
