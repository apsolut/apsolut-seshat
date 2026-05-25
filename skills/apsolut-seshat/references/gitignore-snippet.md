# Lines to append to project `.gitignore`

```gitignore
# apsolut: scratch binaries — keep history-bearing ones, ignore the ephemeral
artifacts/tasks/
artifacts/inbox/

# apsolut: keep the screenshots folder, ignore the screenshots themselves
.apsolut/screenshots/*
!.apsolut/screenshots/.gitkeep
```

## What to keep in git vs ignore

| Path                       | Status     | Why                                                       |
|----------------------------|------------|-----------------------------------------------------------|
| `artifacts/fires/`         | keep       | documents what broken UI/data looked like                 |
| `artifacts/decisions/`     | keep       | architecture diagrams, design exports                     |
| `artifacts/runbooks/`      | keep       | recovery screenshots, dashboards                          |
| `artifacts/tasks/`         | ignore     | throwaway scratch from CC sessions                        |
| `artifacts/inbox/`         | ignore     | raw captures (voice, bookmarks)                           |
| `.apsolut/` (markdown)     | keep all   | markdown only, intentionally curated                      |
| `.apsolut/screenshots/`    | keep dir   | tracked folder so collaborators see it exists             |
| `.apsolut/screenshots/*`   | ignore     | ephemeral bug screenshots — `git add -f` the ones worth keeping (e.g. into a fire entry) |
