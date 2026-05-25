# Lines to append to project `.gitignore`

```gitignore
# apsolut: keep the drop-zone folders, ignore their contents
.apsolut/screenshots/*
!.apsolut/screenshots/.gitkeep
.apsolut/files/*
!.apsolut/files/.gitkeep
```

## What to keep in git vs ignore

| Path                       | Status     | Why                                                       |
|----------------------------|------------|-----------------------------------------------------------|
| `.apsolut/` (markdown)     | keep all   | markdown only, intentionally curated                      |
| `.apsolut/screenshots/`    | keep dir   | tracked folder so collaborators see it exists             |
| `.apsolut/screenshots/*`   | ignore     | ephemeral bug/UI screenshots — drop, reference inline, discard |
| `.apsolut/files/`          | keep dir   | tracked folder so collaborators see it exists             |
| `.apsolut/files/*`         | ignore     | PDFs, audio, exports — most are throwaway                 |

If a binary matters long-term (evidence for a fire entry, a signed contract PDF, a design export referenced from `decisions/`), force-add it with `git add -f path/to/file`.

## Legacy: top-level `artifacts/`

Older versions of this scaffold created `project/artifacts/` at the project root for binaries. That has been folded into `.apsolut/files/`. If you already have a project-root `artifacts/`, it still works — but new projects shouldn't get one. Move contents into `.apsolut/files/` when convenient.
