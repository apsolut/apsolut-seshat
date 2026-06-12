# What's next — pick up and run the next task

Pick the next task from the vault, set up the branch, do the work, move it through the lifecycle.
Do NOT preload the vault. One task file is the entire briefing.

## Pass 1: Detect layout (glob only, no file reading)

- `.apsolut/tasks/next/` exists → **folder mode** (minimal/standard/full)
- `.apsolut/TASKS.md` exists, no `tasks/` → **bare mode**
- `.apsolut/03-plan/` exists → **davinci mode**
- None of the above → tell the user to run `/apsolut-seshat` first. Stop.

## Pass 2: Pick the task

**folder mode:**
1. List `tasks/next/*.md`, exclude `000-template.md`
2. If `$ARGUMENTS` names a task (number or slug fragment), pick that one; otherwise pick the lowest number
3. If `next/` is empty: report it, point at the active plan (`/apsolut-plan`), stop
4. If `doing/` already has a file, mention it — ask whether to resume that instead

**bare mode:** read `TASKS.md`, pick the first unchecked item (or the `$ARGUMENTS` match).

**davinci mode:** grep `03-plan/` frontmatter for `status: active`, open the newest match, pick its first unchecked step.

## Pass 3: Brief

Read the ONE task file. Echo a 4-line brief: what / why / accept / size.
Fetch `refs` files only if listed — never more than the task names.

## Pass 4: Start

**folder mode only:**
1. If the working tree is dirty, stop and ask
2. Create the branch from `branch:` frontmatter (fallback: `feat/<number>-<slug>`)
3. `git mv` the file `next/` → `doing/`, update its `id:` to `tasks/doing/<number>`

**bare/davinci:** no file move — work directly against the checklist item.

## Pass 5: Do the work

Execute against the spec. The `accept` list is the definition of done — verify each
condition (run the tests it names) before claiming completion.

## Pass 6: Close out

**folder mode:** when every accept condition passes:
1. `git mv` the file `doing/` → `done/`, update `id:`, append one outcome line
   (`> done <date> — <what shipped, one line>`)
2. Suggest the commit (task file move + the work itself)

**bare mode:** check the item off in `TASKS.md` with a one-line outcome.
**davinci mode:** check the step off in the plan; if it was the last step, set the plan `status: done` and suggest pruning per the garden rule.

If accept conditions DON'T pass and you're blocked: leave the file in `doing/`, write a one-line blocker note under `## blocked` in the task file, report honestly.
