---
agent: 'agent'
description: "GSD: Validate planning directory health and optionally repair issues"
---

# GSD: Health Check

You are executing the GSD **health** workflow. This validates `.planning/` directory integrity and reports actionable issues.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/health.md`

## Initialize

Run the health validation:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs validate health
```

If user requested repairs (`--repair` flag provided):
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs validate health --repair
```

## Workflow

Parse JSON output for:
- `status`: "healthy" | "degraded" | "broken"
- `errors[]`: Critical issues (code, message, fix, repairable)
- `warnings[]`: Non-critical issues
- `info[]`: Informational notes
- `repairable_count`: Number of auto-fixable issues
- `repairs_performed[]`: Actions taken if --repair was used

## Output Format

Present results in this format:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
 GSD Health Check
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Status: HEALTHY | DEGRADED | BROKEN
Errors: N | Warnings: N | Info: N
```

**If errors exist:**
- List each error with its code, message, and suggested fix
- Indicate which errors are auto-repairable

**If repairs were performed:**
- Show each repair action taken
- Confirm success or failure

## Checks Performed

- Missing required files (PROJECT.md, ROADMAP.md, STATE.md, config.json)
- Invalid config.json structure
- Inconsistent state (STATE.md vs ROADMAP.md)
- Orphaned plans (PLAN.md without phase entry in ROADMAP)
- Missing phase directories
- Summary files without corresponding plans

## When to Use

- After merging GSD updates
- When workflows behave unexpectedly
- After manual edits to `.planning/` files
- Periodically to ensure project health
