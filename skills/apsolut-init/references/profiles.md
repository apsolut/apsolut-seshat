# Profile manifests

Canonical layout per profile. Source of truth for what `apsolut-init` creates.

## bare

For weekend projects, < 10 entries total. No folders.

```
.apsolut/
├── TASKS.md
├── DECISIONS.md
└── FIRES.md
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
└── fires/000-template.md
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
└── fires/000-template.md
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
└── fires/000-template.md
```

## All profiles also create

```
project/
├── .apsolut/       (see above)
├── artifacts/      (empty; populated on demand)
├── README.md       (if missing)
├── CLAUDE.md       (if missing)
└── .gitignore      (created or appended)
```

## Promotion path

```
bare ──split files into folders──> minimal
minimal ──add notes/docs/services/ops──> standard
standard ──split notes/ + ops/ into 6──> full
```

Splitting is mechanical: grep entries by frontmatter, write each slice into a new folder file.
