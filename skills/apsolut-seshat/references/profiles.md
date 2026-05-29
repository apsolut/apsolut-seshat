# Profile manifests

Canonical layout per profile. Source of truth for what `/apsolut-seshat` creates.

## bare

For weekend projects, < 10 entries total. No folders.

```
.apsolut/
├── TASKS.md
├── DECISIONS.md
├── FIRES.md
├── screenshots/.gitkeep
└── files/.gitkeep
```

Promote to **minimal** when any one file crosses ~10 entries: split by section into a folder with one file per entry.

## minimal

Folders, but only the essentials. Three high-frequency loops.

```
.apsolut/
├── tasks/
│   ├── next/000-template.md
│   ├── doing/000-template.md
│   └── done/000-template.md
├── decisions/000-template.md
├── fires/000-template.md
├── screenshots/.gitkeep
└── files/.gitkeep
```

## standard *(default)*

Most active projects. Adds capture pipeline + reference + ops.

```
.apsolut/
├── notes/000-template.md       # capture pipeline (slice by `stage:` frontmatter)
├── tasks/
│   ├── next/000-template.md
│   ├── doing/000-template.md
│   └── done/000-template.md
├── decisions/000-template.md
├── docs/000-template.md
├── services/000-template.md
├── ops/000-template.md         # howto + recovery + constraint (slice by `kind:`)
├── fires/000-template.md
├── screenshots/.gitkeep        # drop bug screenshots here, reference inline
└── files/.gitkeep              # PDFs, audio, exports — user adds subfolders on demand
```

## full

For team projects with planning ceremonies and ongoing ops. Splits the merged folders back out.

```
.apsolut/
├── inbox/
│   ├── 000-template.md
│   ├── bookmarks/.gitkeep
│   ├── bugs/.gitkeep
│   ├── feedback/.gitkeep
│   ├── ideas/.gitkeep
│   ├── meetings/.gitkeep
│   └── voice/.gitkeep
├── explore/000-template.md
├── blueprints/000-template.md
├── tasks/
│   ├── next/000-template.md
│   ├── doing/000-template.md
│   └── done/000-template.md
├── decisions/000-template.md
├── docs/000-template.md
├── services/000-template.md
├── guides/000-template.md
├── runbooks/000-template.md
├── rules/000-template.md
├── fires/000-template.md
├── screenshots/.gitkeep
└── files/.gitkeep
```

## davinci *(alternate track)*

A different mental model from bare→full. The notebook/sketchbook profile — for solo creative or research work where thought flows from raw capture into structured plans and decisions. Standalone, not on the promotion path.

```
.apsolut/
├── 01-thinking/000-template.md     # raw capture, stream of consciousness
├── 02-ideas/000-template.md        # exploration, options, comparisons
├── 03-plan/000-template.md         # blueprints, broken-down plans
├── 04-library/000-template.md      # markdown reading-room — excerpts, transcripts, snippets
├── 05-decisions/000-template.md    # locked-in decisions and rationale
├── 06-knowledge/000-template.md    # reference: concepts, glossary, learned material
├── 07-files/000-template.md        # cold binaries: PDFs, audio, exports — 000-template.md is a manifest; subfolders on demand
└── 08-screenshots/000-template.md  # hot images — manifest + intent subfolders: inspiration/ (keep), bugs/ (ephemeral)
```

Differences from the bare→full track:
- **No fires/** — davinci is for creative/research work, not operations
- **Numbered folder prefixes** force a reading order: thinking → ideas → plan
- **`04-library/` is markdown only** — excerpts, transcripts, snippets you paste in
- **Binaries live in numbered folders, not the shared un-numbered zones** — `07-files/` (PDFs, audio, exports) and `08-screenshots/` (images), linked from `04-library/` or anywhere. This is a deliberate divergence: bare→full use un-numbered `screenshots/`+`files/`, but davinci keeps everything numbered so the vault stays an all-numbered notebook. `07-files/`/`08-screenshots/` contents are gitignored (the `000-template.md` stays tracked)
- **Each binary folder's `000-template.md` is a manifest** — a markdown table (one row per kept binary) that acts as the injection map: Claude reads the table to locate a file, then opens only that one instead of scanning the folder. `08-screenshots/` further splits by intent into `inspiration/` (keep) and `bugs/` (ephemeral) subfolders, created on demand. Automated/Playwright/test screenshots stay out of the vault entirely
- **File naming inside folders is the same** `001-kebab-case.md` convention as other profiles

Flow:
```
01-thinking ──promote──> 02-ideas ──commit──> 03-plan
                                  └─lock──> 05-decisions
04-library + 06-knowledge + 07-files + 08-screenshots are referenced from anywhere
```

## All profiles also create

```
project/
├── .apsolut/       (see above — binary drop zones vary by track, see below)
├── README.md       (if missing)
├── CLAUDE.md       (if missing)
└── .gitignore      (created or appended)
```

Every profile gets binary drop zones inside the otherwise markdown-only `.apsolut/` vault — but **where** depends on the track:

- **bare→full** use two un-numbered folders: `.apsolut/screenshots/` (hot path — bug/UI screenshots, referenced inline) and `.apsolut/files/` (cold storage — PDFs, audio, exports; user adds subfolders like `pdfs/`, `audio/` on demand).
- **davinci** keeps it numbered instead: `.apsolut/08-screenshots/` (hot) and `.apsolut/07-files/` (cold) — same roles, numbered to fit the notebook layout.

In both cases the folders are tracked in git, contents are gitignored, and you `git add -f` the keepers. **No top-level `artifacts/` is created** — that older convention was folded into the cold-storage folder.

## Promotion path

```
bare ──split files into folders──> minimal
minimal ──add notes/docs/services/ops──> standard
standard ──split notes/ + ops/ into 6──> full

davinci ──standalone, not on the bare→full path──
```

Splitting is mechanical: grep entries by frontmatter, write each slice into a new folder file.

davinci is a parallel track. Pick it for solo creative/research work; pick bare→full for projects that need ops and fires. Both tracks support binaries — bare→full via un-numbered `screenshots/`+`files/`, davinci via numbered `07-files/`+`08-screenshots/`.
