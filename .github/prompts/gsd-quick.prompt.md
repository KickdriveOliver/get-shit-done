---
agent: 'agent'
description: "GSD: Execute a quick ad-hoc task with state tracking"
---

# GSD: Quick Task

You are executing the GSD **quick** workflow. This handles small, ad-hoc tasks with GSD guarantees (STATE.md tracking, optional atomic commits) while skipping optional agents.

## Flags

- `--full` — Enables plan-checking (max 2 iterations) and post-execution verification. Use when you want quality guarantees without full milestone ceremony.

## Git Behavior

Read `.planning/config.json`. If `git.enabled` is `false` (or missing), **skip all git operations**. Just write files and track progress in STATE.md.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/quick.md`
2. Read `.planning/STATE.md` for current state

## Initialize

```bash
node .claude/get-shit-done/bin/gsd-tools.cjs state load
```

## Workflow

1. **Get task description** — Ask the user what they want done (if not already described)
2. **Plan** — Create a quick plan in `.planning/quick/{NNN}-{slug}/PLAN.md`
3. **Execute** — Implement the task following the plan
4. **Commit** — If `git.enabled`: atomic git commit per task. If `false`: skip, continue to next step
5. **Summary** — Write SUMMARY.md in the task directory
6. **Update state** — Add to "Quick Tasks Completed" table in STATE.md

## When to Use Quick Mode

- Bug fixes
- Small features
- Config changes
- One-off tasks
- Anything that doesn't need research or multi-plan phasing

## Important

- Quick tasks live in `.planning/quick/`, separate from phase work
- They do NOT appear in ROADMAP.md
- They still get state tracking (and atomic commits if `git.enabled` is `true`)
- Each task gets its own numbered directory
