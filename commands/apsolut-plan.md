# Where are we — show the active plan and its progress

Find the most recent active plan, cross-reference its tasks, report progress.
Do NOT read every file. Grep frontmatter first, open one plan file.

## Pass 1: Detect layout (glob only)

- `.apsolut/plans/` exists → **full**: plans live there
- `.apsolut/notes/` exists → **standard**: plans are notes with `stage: planned`
- `.apsolut/02-plan/` exists → **davinci**: plans live there
- `.apsolut/TASKS.md` only → **bare**: no plans layer — summarize unchecked TASKS.md items instead and stop

## Pass 2: Find the active plan (grep only)

- full: `grep -l "status: active" .apsolut/plans/*.md` — exclude `000-template.md`
- standard: `grep -l "stage: planned" .apsolut/notes/*.md` filtered to `status: active|promoted`
- davinci: `grep -l "status: active" .apsolut/02-plan/*.md`

Pick the highest-numbered match (newest). If `$ARGUMENTS` names a plan, use that one.
If nothing matches: report "no active plan", list the newest 3 exploring/idea-stage notes as candidates, stop.

## Pass 3: Read the ONE plan file

Extract: goal, steps, done-when, linked tasks (`[[tasks/...]]` refs).

## Pass 4: Cross-reference progress (glob only, don't open task files)

For each task the plan links, check which folder its file is in:
- `tasks/done/` → ✅
- `tasks/doing/` → 🔨
- `tasks/next/` → ⏳
- nowhere → ❌ not sliced yet

davinci: use the plan's own checkboxes instead.

## Pass 5: Report

```
## Plan: <title>           (<file path>)
Goal: <one line>
Done when: <criteria>

⏳ 005-shopping-list-data-model   (next)
🔨 006-shopping-list-ui           (doing)
✅ 004-recipe-schema-migration    (done)
❌ "export to PDF"                (step not sliced into a task yet)

Next move: /apsolut-next → 005
```

## Pass 6: Offer (don't do without approval)

- ❌ steps → offer to slice each into a `tasks/next/` file using the task template (self-contained, ~300 tokens, `from:` pointing back at the plan)
- All steps ✅ → offer to mark the plan done and prune per the garden rule
