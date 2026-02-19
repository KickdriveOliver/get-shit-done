---
agent: 'agent'
description: "GSD: Systematic debugging with persistent state"
---

# GSD: Debug

You are executing the GSD **debug** workflow. This provides systematic debugging using the scientific method.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/diagnose-issues.md`
2. Read `.planning/STATE.md` for current state

## Initialize

```bash
node .claude/get-shit-done/bin/gsd-tools.cjs state load
```

## Workflow

1. **Check active sessions** — Look for existing debug sessions in `.planning/debug/`
2. **Gather symptoms** — What's broken? Error messages, steps to reproduce, expected vs actual
3. **Form hypothesis** — Based on symptoms, what could cause this?
4. **Investigate** — Read relevant code, run diagnostic commands
5. **Test hypothesis** — Make targeted changes to verify
6. **Fix or iterate** — If hypothesis confirmed, fix it. If not, form new hypothesis
7. **Document** — Write debug session results

## Debug Session Files

Debug sessions live in `.planning/debug/` with:
- Session ID and timestamp
- Symptoms documented
- Hypotheses tested (confirmed/rejected)
- Root cause identified
- Fix applied

## Important

- Follow scientific method: observe → hypothesize → test → conclude
- Don't shotgun-debug (changing random things and hoping)
- Document each hypothesis and its outcome
- Commit fixes with: `fix: description of what was fixed`
