---
id: 08-screenshots/000
date:
tags:
---
# 08-screenshots — image manifest + hot drop zone

Drop an image, then add a row to the manifest below so it's findable without loading the folder.

**Subfolders are fully supported and encouraged.** Create whatever makes sense for your workflow (e.g. `inspiration/`, `bugs/`, `playwright/`, `debug/2026-07/`, `client-foo/`, `manual-screenshots/`). The only rule: put the binary in a subfolder, then add **one row** to the manifest so Claude can find it without ever scanning the folder.

Examples of common intents (feel free to invent your own):

- **`inspiration/`** — **keep.** Design/mood/reference shots, sketches, mockups you'll revisit. Force-add the keepers (`git add -f`).
- **`bugs/`** — **ephemeral.** "Look at this broken thing." Stays gitignored; delete once the fix lands and prune its manifest row.
- `playwright/` or `test-failures/` — for selected automated screenshots you want to keep in the vault.
- `debug/<date-or-session>/` — for ad-hoc debugging sessions.

**Not here:** automated/Playwright/test screenshots by default. Those are regenerated test output — they belong in the repo's test-artifacts directory (gitignored there), never in the vault. Only copy the ones that actually helped you debug into a subfolder here.

## manifest

| file | what it is | supports | keep? |
|------|-----------|----------|-------|
| `inspiration/hero-ref.png` | competitor hero layout I liked | [[01-ideas/003]] | keep |
| `bugs/login-500.png` | 500 on submit, prod | [[02-plan/004]] | ephemeral |

<!--
This manifest is the injection map. Claude reads THIS table to find the right image, then views only that one file — it never bulk-loads the folder, so a vault with 40 screenshots costs nothing until one is actually needed. The user drops the file; Claude adds the row when the file is mentioned. Prune `ephemeral` rows when the shot is deleted; keep `keep` rows after force-adding. Inspiration shots are pulled only for related design work; bug shots only when debugging that issue.
-->
