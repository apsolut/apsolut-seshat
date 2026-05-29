# Lines to append to project `.gitignore`

**bare / minimal / standard / full** — un-numbered drop zones:

```gitignore
# apsolut: keep the drop-zone folders, ignore their contents
.apsolut/screenshots/*
!.apsolut/screenshots/.gitkeep
.apsolut/files/*
!.apsolut/files/.gitkeep
```

**davinci** — numbered binary folders (keep each `000-template.md`):

```gitignore
# apsolut (davinci): keep the binary folders + their templates, ignore the binaries
.apsolut/07-files/*
!.apsolut/07-files/000-template.md
.apsolut/08-screenshots/*
!.apsolut/08-screenshots/000-template.md
```

## What to keep in git vs ignore

| Path                       | Status     | Why                                                       |
|----------------------------|------------|-----------------------------------------------------------|
| `.apsolut/` (markdown)     | keep all   | markdown only, intentionally curated                      |
| `.apsolut/screenshots/`    | keep dir   | tracked folder so collaborators see it exists             |
| `.apsolut/screenshots/*`   | ignore     | ephemeral bug/UI screenshots — drop, reference inline, discard |
| `.apsolut/files/`          | keep dir   | tracked folder so collaborators see it exists             |
| `.apsolut/files/*`         | ignore     | PDFs, audio, exports — most are throwaway                 |
| `.apsolut/07-files/000-template.md`, `.apsolut/08-screenshots/000-template.md` | keep | davinci's numbered binary folders — template stays tracked so the folder exists |
| `.apsolut/07-files/*`, `.apsolut/08-screenshots/*` | ignore | davinci binaries — same throwaway-by-default rule as `files/`+`screenshots/` |

If a binary matters long-term (evidence for a fire entry, a signed contract PDF, a design export referenced from `decisions/`), force-add it with `git add -f path/to/file`.

## Legacy: top-level `artifacts/`

Older versions of this scaffold created `project/artifacts/` at the project root for binaries. That has been folded into `.apsolut/files/`. If you already have a project-root `artifacts/`, it still works — but new projects shouldn't get one. Move contents into `.apsolut/files/` when convenient.
