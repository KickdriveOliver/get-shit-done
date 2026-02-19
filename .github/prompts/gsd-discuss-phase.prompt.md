---
agent: 'agent'
description: "GSD: Discuss phase implementation preferences before planning"
---

# GSD: Discuss Phase

You are executing the GSD **discuss-phase** workflow. This captures implementation decisions before planning begins.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/discuss-phase.md`
2. Read the context template: `.claude/get-shit-done/templates/context.md`
3. Read `.planning/STATE.md` for current project state
4. Read `.planning/ROADMAP.md` for phase details

## Phase Input

The user should specify a phase number (e.g., "1", "2"). If not provided, ask which phase to discuss.

## Workflow

1. **Validate** — Confirm the phase exists in the roadmap
2. **Check existing** — If CONTEXT.md exists for this phase, offer to update/view/skip
3. **Analyze phase** — Identify the domain and generate phase-specific gray areas:
   - Visual features → Layout, density, interactions, empty states
   - APIs/CLIs → Response format, flags, error handling
   - Content systems → Structure, tone, depth
   - Data/Organization → Grouping criteria, naming, exceptions
4. **Present gray areas** — Let user select which to discuss (multi-select)
5. **Deep-dive each area** — ~4 questions per area, then offer more or move to next
6. **Write CONTEXT.md** — Sections match the areas discussed

**Creates:** `.planning/{phase}-CONTEXT.md`

## Important

- This step is optional but highly recommended — it shapes what gets built
- Don't ask about things already decided in PROJECT.md or REQUIREMENTS.md
- The output feeds into research and planning — be specific and actionable
- After completion, suggest running `gsd-plan-phase` next
