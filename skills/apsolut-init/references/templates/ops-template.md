---
id: ops/000
tags:
last-verified:
kind: howto | recovery | constraint
applies-to: [service, area, or "all"]
severity:                    # recovery only: minor | major | critical
trigger:                     # recovery only: symptom or alert name
---
# [one line — what this covers]

<!--
KIND values:

howto       → build-time procedure (deploy, debug, setup)
recovery    → ops-time recovery (when X breaks, do Y) — fill severity + trigger
constraint  → hard rule (legal, GDPR, TOS, security) — what you must / must not do
-->

## when you need this

[symptoms (recovery) | situation (howto) | scope (constraint)]

## prerequisites

- [tool, access, credential, env var, permission level]

## steps / decision tree / in practice

**howto** — numbered steps:
1. [step]
2. [step]

**recovery** — decision tree → recovery steps → verify:
1. Is X happening? → yes step 2, no fallback
2. [exact command]
3. ...
- verify: [check 1 — exact command or URL]

**constraint** — in practice:
- when building [X], you must [Y]
- never [Z]
- violations look like: [example]
- source: [link to law, TOS, or internal decision]

## if it fails (recovery only)

- escalation: [who to page, in what order]
- fallback: [how to degrade gracefully]
- rollback: [how to undo what you just tried]

## related

- [[decisions/]] — why it works this way
- [[services/]] — affected service (recovery)
- [[fires/]] — incidents this addresses (recovery)
