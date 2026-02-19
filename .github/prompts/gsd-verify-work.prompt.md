---
agent: 'agent'
description: "GSD: Validate built features through conversational UAT testing"
---

# GSD: Verify Work

You are executing the GSD **verify-work** workflow. This confirms that built features actually work from the user's perspective.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/verify-work.md`
2. Read the UAT template: `.claude/get-shit-done/templates/UAT.md`
3. Read `.planning/STATE.md` for current state
4. Read `.planning/ROADMAP.md` for phase details

## Workflow

1. **Identify phase** — Use the phase number provided, or detect from recent work
2. **Extract testable deliverables** — What should the user be able to do now?
3. **Walk through one test at a time:**
   - Present what to test: "Can you [specific action]?"
   - Wait for user response: pass / fail / describe what's wrong
4. **If test fails:**
   - Diagnose the root cause automatically
   - Create fix plans ready for re-execution
5. **Track results** — Write `{phase}-UAT.md` with all test outcomes

**Creates:** `{phase}-UAT.md`, fix plans if issues found

## Test Presentation Style

- One test at a time — don't overwhelm
- Plain language — "Can you log in with email?" not "Verify authentication flow"
- Specific steps — Tell the user exactly what to try
- Binary outcome — Pass or fail, with details on failure

## After Verification

- If all pass → Phase complete, suggest `gsd-progress` or next phase
- If failures → Fix plans created, suggest `gsd-execute-phase` with `--gaps-only`
