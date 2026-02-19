---
agent: 'agent'
description: "GSD: Check project progress and route to next action"
---

# GSD: Progress

You are executing the GSD **progress** workflow. This shows where the project stands and what to do next.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/progress.md`
2. Read `.planning/STATE.md`
3. Read `.planning/ROADMAP.md`

## Initialize

Run in terminal:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs progress
```

Also check roadmap analysis:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs roadmap analyze
```

## Workflow

1. **Load state** — Read STATE.md and ROADMAP.md
2. **Summarize progress** — Which phases are done, in progress, remaining
3. **Show recent work** — Last completed tasks and summaries
4. **Route to next action** — Based on current state:
   - Phase has plans but no execution → suggest `gsd-execute-phase`
   - Phase needs planning → suggest `gsd-plan-phase`
   - Phase needs discussion → suggest `gsd-discuss-phase`
   - All phases done → suggest `gsd-complete-milestone`

## Output Format

Present a clear, concise progress summary:
- Overall completion percentage
- Current phase and its status
- What was last completed
- Recommended next action with the appropriate prompt file name
