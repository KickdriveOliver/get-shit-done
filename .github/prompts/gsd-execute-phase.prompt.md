---
agent: 'agent'
description: "GSD: Execute all plans in a phase sequentially"
---

# GSD: Execute Phase

You are executing the GSD **execute-phase** workflow. This runs all plans for a phase.

## Flags

- `--gaps-only` — Execute only gap closure plans (plans with `gap_closure: true` in frontmatter). Use after verify-work creates fix plans.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/execute-phase.md`
2. Read the execute-plan workflow: `.claude/get-shit-done/workflows/execute-plan.md`
3. Read `.planning/STATE.md` for current state

## Initialize

Run in terminal:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs init execute-phase <PHASE_NUMBER>
```

Parse JSON for: phase_dir, plans, incomplete_plans, commit_docs.

Also get plan inventory:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs phase-plan-index <PHASE_NUMBER>
```

## Git Behavior

Read `.planning/config.json`. If `git.enabled` is `false` (or missing), **skip all git operations** (commits, branch creation, history checks). Just write files, track progress in STATE.md, and continue.

## Workflow

1. **Discover plans** — Find all PLAN.md files in the phase directory
2. **Skip completed** — Skip plans that already have a SUMMARY.md
3. **Execute each plan sequentially:**
   - Read the PLAN.md file
   - Execute each `<task>` in order
   - Run verification steps after each task
   - If `git.enabled`: make atomic git commits per task: `feat(XX-YY): description`
   - If `git.enabled` is `false`: record task completion in STATE.md instead
   - Create SUMMARY.md when plan is complete
4. **Update state** — Update STATE.md with completed work
5. **Verify phase** — Check that phase goals were achieved

**Creates:** `{phase}-{N}-SUMMARY.md`, `{phase}-VERIFICATION.md`

## Task Execution Pattern

For each task in a plan:
1. Read the task's `<action>` instructions
2. Implement the code changes
3. Run the task's `<verify>` command
4. If verify passes → commit with atomic message (**only if `git.enabled` is `true`**; otherwise just continue)
5. If verify fails → debug and fix before moving on

## Git Commit Convention

**Skip this section entirely if `git.enabled` is `false` in `.planning/config.json`.**

```
feat(XX-YY): description    # Feature implementation
fix(XX-YY): description     # Bug fix
docs(XX-YY): description    # Documentation
refactor(XX-YY): description # Code refactoring
```

Where XX = phase number, YY = plan number.

## Important for VS Code Copilot

- Execute plans **one at a time, sequentially** (no parallel subagent spawning)
- Keep context focused — read only the current plan's files
- If `git.enabled`: commit after each task, not after all tasks. If `git.enabled` is `false`: skip commits entirely
- If context gets large, suggest the user start a new chat for the next plan
- After all plans complete, suggest running `gsd-verify-work` next
