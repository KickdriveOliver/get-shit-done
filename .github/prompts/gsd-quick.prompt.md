---
mode: agent
description: "GSD: Execute a quick ad-hoc task with atomic commits and state tracking"
---

# GSD: Quick Task

You are executing the GSD **quick** workflow. This handles small, ad-hoc tasks with GSD guarantees (atomic commits, STATE.md tracking) while skipping optional agents.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/quick.md`
2. Read `.planning/STATE.md` for current state

## Initialize

```bash
node .claude/get-shit-done/bin/gsd-tools.js state load
```

## Workflow

1. **Get task description** — Ask the user what they want done (if not already described)
2. **Plan** — Create a quick plan in `.planning/quick/{NNN}-{slug}/PLAN.md`
3. **Execute** — Implement the task following the plan
4. **Commit** — Atomic git commit per task
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
- They still get atomic commits and state tracking
- Each task gets its own numbered directory
