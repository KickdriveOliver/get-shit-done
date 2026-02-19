---
agent: 'agent'
description: "GSD: Create handoff file when pausing mid-session"
---

# GSD: Pause Work

You are executing the GSD **pause-work** workflow. This creates a context handoff file so work can be resumed later.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/pause-work.md`
2. Read `.planning/STATE.md`

## Workflow

1. **Detect current phase** — Find the most recently modified phase directory
2. **Gather state:**
   - What was being worked on
   - What's completed
   - What remains
   - Any active decisions or blockers
3. **Write `.continue-here.md`** — Handoff file with all context
4. **Update STATE.md** — Record the pause point
5. **Git commit** — If `git.enabled` is `true` in `.planning/config.json`: `wip: pause work on phase N`. If `false`: skip this step

**Creates:** `.continue-here.md` at project root

## Handoff File Contents

The `.continue-here.md` should contain enough context for a fresh session to continue seamlessly:
- Current position (phase, plan, task)
- What was just completed
- What's remaining
- Key decisions made
- Any blockers or issues
- Exact next step to take
