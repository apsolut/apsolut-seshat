---
id: 03-audits/000
date:
tags:
status: active | done | archived
---
# [one line — audit / review title, e.g. after 10 code changes or quarterly]

## scope

[What was reviewed? e.g. plans since last audit, specific area of code, active decisions]

## changes since last

[Key code/docs changes, new features, refactors. Link to commits or notes if useful.]

## review of active plans / decisions

- [ ] Plan 02-xxx: status? still relevant? links updated? overcomplicated?
- [ ] Decision 05-yyy: still holds? needs revisit?

## slop & hygiene check

- Dead code left behind?
- Over-abstracted or brittle code?
- Comments/code changed without reason?
- Comprehension: can I still explain the key parts?

## synthesis

[Recurring themes, contradictions, what the last period taught us. Feed this into 06-knowledge/ and 05-decisions/. Combat comprehension debt.]

## links to long-term memory

- [[05-decisions/]]
- [[06-knowledge/]]
- [[04-library/]]

## actions

- prune / archive old plans
- update knowledge
- create new decision if needed
- next audit trigger (e.g. after another 10 changes or major refactor)

<!--
03-audits is the layer for long-term memory maintenance. Use it to "rescan" the project state after changes, synthesize recent work, review the plan/ and decisions/ layers, and keep the durable memory (05-decisions + 06-knowledge) fresh.

Subfolders encouraged: e.g. audits/2026-07/, audits/refactors/, audits/by-feature/.

This replaces the need for external rescans — the audits themselves become the record of evolution.
-->