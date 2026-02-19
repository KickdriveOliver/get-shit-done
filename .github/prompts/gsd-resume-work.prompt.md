---
agent: 'agent'
description: "GSD: Resume work from a previous session"
---

# GSD: Resume Work

You are executing the GSD **resume-work** workflow. This restores context from a previous session.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/resume-project.md`
2. Check for `.continue-here.md` at project root
3. Read `.planning/STATE.md`

## Workflow

1. **Check for handoff** — Read `.continue-here.md` if it exists
2. **Load state** — Read STATE.md for full project context
3. **Detect incomplete work** — Find plans without summaries
4. **Present status:**
   - Overall progress
   - Where work was paused
   - What's next
5. **Route to next action:**
   - If mid-execution → suggest `gsd-execute-phase`
   - If phase needs planning → suggest `gsd-plan-phase`
   - If needs discussion → suggest `gsd-discuss-phase`

## Important

- Read ALL context before suggesting actions
- Check CONTEXT.md exists before suggesting plan (may need discuss first)
- Clean up `.continue-here.md` after successfully resuming
