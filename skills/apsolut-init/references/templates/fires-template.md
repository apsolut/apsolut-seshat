---
id: fires/000
date:
tags:
category: outage | data | third-party | cost-spike | near-miss | performance | security | deploy
severity: minor | major | critical
duration: [how long it lasted]
---

<!--
CATEGORIES — pick one:

outage        → app/feature stopped working, users blocked
data          → wrong data, lost data, leaked data, GDPR violation
third-party   → external service broke your app (API down, quota hit, format changed)
cost-spike    → unexpected bill from infinite loops, missing cache, overage
near-miss     → caught before users noticed — log it, it's tomorrow's outage
performance   → it works but slow — users leave, they don't complain
security      → unauthorized access, leaked secrets, missing auth checks
deploy        → release went wrong — env vars, migrations, rollbacks
-->

# [what broke — one line]

## timeline

- [HH:MM] noticed [symptom]
- [HH:MM] identified [root cause]
- [HH:MM] fixed by [action]
- [HH:MM] confirmed resolved

## root cause

[what actually went wrong — be specific]

## impact

[who was affected, how badly]

## fix

[what we did to stop the bleeding]

## prevention

[what we changed so this doesn't happen again]
- [ ] task created: [[tasks/next/]]
- [ ] runbook updated: [[runbooks/]]

## lesson

[one sentence a future dev should know]

<!-- For severity: major | critical — add a postmortem block below -->

## postmortem (major/critical only)

### contributing factors

[plural — not "root cause." what conditions made this possible?]

### 5 whys

1. why did X happen? → because Y
2. why did Y? → because Z
3. ...

### action items

- [ ] [owner] [due-date] [action]
- [ ] [owner] [due-date] [action]
