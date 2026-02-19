---
agent: 'agent'
description: "GSD: Create detailed execution plans for a roadmap phase"
---

# GSD: Plan Phase

You are executing the GSD **plan-phase** workflow. This creates executable task plans for a phase.

## Flags

- `--auto` — Automatic mode. Chains discuss → plan → execute without stopping
- `--research` — Force re-research even if RESEARCH.md exists
- `--skip-research` — Skip research, go straight to planning
- `--gaps` — Gap closure mode (reads VERIFICATION.md, skips research)
- `--skip-verify` — Skip verification loop

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/plan-phase.md`
2. Read `.planning/STATE.md` for current state
3. Read `.planning/ROADMAP.md` for phase details

## Initialize

Run in terminal:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs init execute-phase <PHASE_NUMBER>
```

## Workflow

**Default flow:** Research (if config says yes) → Plan → Verify → Done

1. **Parse phase** — Determine which phase to plan (auto-detect next unplanned if not specified)
2. **Research** (unless skipped) — Investigate how to implement this phase:
   - Read CONTEXT.md decisions (if exists)
   - Analyze stack, patterns, potential issues
   - Write `{phase}-RESEARCH.md`
3. **Plan** — Create 2-3 atomic task plans with XML structure:
   - Each plan = small enough for one focused execution session
   - Tasks use `<task>` XML with name, files, action, verify, done
   - Plans include wave assignments for parallel execution
   - Write `{phase}-{N}-PLAN.md` files
4. **Verify** — Check plans against requirements and phase goals
5. **Iterate** — If verification fails, fix and re-verify (max 3 iterations)

**Creates:** `{phase}-RESEARCH.md`, `{phase}-{N}-PLAN.md` files

## Plan Format

Each plan should follow the template at `.claude/get-shit-done/templates/` with:
- YAML frontmatter (phase, plan number, wave, autonomous flag)
- `<objective>` — What this plan achieves
- `<context>` — File references and dependencies
- `<tasks>` — Atomic tasks with XML structure
- `<verification>` — How to confirm it works
- `<output>` — Expected SUMMARY.md structure

## Important

- Plans must be small enough to execute without context degradation
- Each task should modify 1-3 files max
- Include verification commands (curl, test runs, etc.)
- After completion, suggest running `gsd-execute-phase` next

## Note for VS Code Copilot

Since Copilot doesn't spawn subagents, execute the research and planning steps sequentially in this same context. Keep the planning focused and avoid loading too many files at once.
