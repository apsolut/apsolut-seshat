# Lines to append to project `.gitignore`

```gitignore
# apsolut: scratch binaries — keep history-bearing ones, ignore the ephemeral
artifacts/tasks/
artifacts/inbox/
```

## What to keep in git vs ignore

| Path                       | Status   | Why                                      |
|----------------------------|----------|------------------------------------------|
| `artifacts/fires/`         | keep     | documents what broken UI/data looked like |
| `artifacts/decisions/`     | keep     | architecture diagrams, design exports    |
| `artifacts/runbooks/`      | keep     | recovery screenshots, dashboards          |
| `artifacts/tasks/`         | ignore   | throwaway scratch from CC sessions       |
| `artifacts/inbox/`         | ignore   | raw captures (voice, bookmarks)          |
| `.apsolut/`                | keep all | markdown only, intentionally curated     |
