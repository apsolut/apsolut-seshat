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

## All profiles also create

```
project/
├── .apsolut/       (see above — always includes screenshots/ and files/)
├── README.md       (if missing)
├── CLAUDE.md       (if missing)
└── .gitignore      (created or appended)
```

`.apsolut/screenshots/` and `.apsolut/files/` are the binary exceptions inside the otherwise markdown-only `.apsolut/` vault:

- **`screenshots/`** — hot path. Drop bug/UI screenshots, reference them inline in conversation.
- **`files/`** — cold storage. PDFs, audio, exports, anything else. User adds subfolders (`pdfs/`, `docs/`, `audio/`) on demand.

Both folders are tracked in git; contents are gitignored. `git add -f` the keepers. **No top-level `artifacts/` is created** — that older convention was folded into `.apsolut/files/`.

## Promotion path

```
bare ──split files into folders──> minimal
minimal ──add notes/docs/services/ops──> standard
standard ──split notes/ + ops/ into 6──> full
```

Splitting is mechanical: grep entries by frontmatter, write each slice into a new folder file.
