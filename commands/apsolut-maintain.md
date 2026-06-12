# Maintain .apsolut/ vault

Scan and fix links, tags, and references across the .apsolut/ vault.
Do NOT read every file into context. Work in passes.

## Pass 1: Inventory (grep only, no file reading)

1. Find all wiki-links: `grep -rn "\[\[" .apsolut/ --include="*.md"`
2. Find all tags: `grep -rn "tags:" .apsolut/ --include="*.md"`
3. Find all hash tags in body: `grep -rn "#[a-z]" .apsolut/ --include="*.md"`
4. List all actual files: `find .apsolut/ -name "*.md" -not -name "000-*" | sort`

## Pass 2: Detect problems

From the grep output (not file contents), identify:
- **Broken links**: [[target]] where target file doesn't exist
- **Orphans**: files with zero incoming links from other files
- **Missing links**: files sharing a #tag but not [[linked]] to each other
- **Stale tags**: #tags used by only one file (dead thread)
- **Plain text refs**: any `from:` or `see` without [[brackets]] (not Obsidian-compatible)
- **Naming drift**: files not following 001-kebab-case.md pattern

## Pass 3: Report (don't fix yet)

Print a summary table:
- X broken links
- X orphaned files
- X missing connections suggested
- X stale tags
- X plain text refs to convert
- X naming issues

Ask for approval before proceeding.

## Pass 4: Fix (only after approval)

For each issue, open ONLY that one file, fix it, close it.
One file at a time. Never hold more than one file in context.
Move to next file only after saving the current one.

Priority order: broken links first, then plain text refs, then orphans.
