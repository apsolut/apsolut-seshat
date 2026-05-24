---
id: runbooks/000
tags:
last-verified:
severity: minor | major | critical
trigger: [symptom or alert name]
---
# [what's broken — one line]

## when to use this

[symptoms that bring you here — what you'd see in logs, alerts, user reports]

## severity

[minor | major | critical — and what determines it]

## prerequisites

- [tool, access, or credential needed]
- [env var that must be set]
- [permission level required]

## decision tree

1. Is X happening? → yes go to step 2, no go to fallback
2. Check [thing] — if [condition], do [A]; else do [B]
3. ...

## recovery steps

1. [step — exact command if possible]
2. [step]
3. [step]

## verify it worked

- [check 1 — exact command or URL]
- [check 2]

## if recovery fails

- escalation: [who to page, in what order]
- fallback: [how to degrade gracefully]
- rollback: [how to undo what you just tried]

## related

- [[fires/]] — incidents this runbook addresses
- [[services/]] — the affected service
- [[decisions/]] — design choices that motivated this runbook
