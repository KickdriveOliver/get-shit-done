---
agent: 'agent'
description: "GSD: Initialize a new project — questioning → research → requirements → roadmap"
---

# GSD: New Project

You are executing the GSD **new-project** workflow. This initializes a new project through a structured flow.

## Flags

- `--auto` — Automatic mode. After config questions, runs research → requirements → roadmap without further interaction. Expects idea document via @ reference.

## Before Starting

1. Read the workflow: `.claude/get-shit-done/workflows/new-project.md`
2. Read the questioning guide: `.claude/get-shit-done/references/questioning.md`
3. Read the project template: `.claude/get-shit-done/templates/project.md`
4. Read the requirements template: `.claude/get-shit-done/templates/requirements.md`

## Initialize

Run in terminal:
```bash
node .claude/get-shit-done/bin/gsd-tools.cjs init new-project
```

Parse the JSON output for project state and config.

## Workflow

Follow the new-project workflow end-to-end:

1. **Setup** — Check if project already exists, init git if needed
2. **Brownfield detection** — If existing code found, offer to map codebase first
3. **Deep questioning** — Ask until you understand the idea completely (goals, constraints, tech, edge cases)
4. **Config** — Ask about mode (interactive/yolo), depth (quick/standard/comprehensive), research preference
5. **Research** (optional) — Investigate the domain, analyze stack, patterns, pitfalls
6. **Requirements** — Extract v1/v2/out-of-scope with REQ-IDs
7. **Roadmap** — Create phases mapped to requirements with success criteria
8. **State** — Initialize STATE.md with project memory

**Creates:** `.planning/PROJECT.md`, `config.json`, `research/`, `REQUIREMENTS.md`, `ROADMAP.md`, `STATE.md`

**After completion:** Tell the user to run the `gsd-plan-phase` prompt for phase 1.

## Important

- Ask questions in batches of 3-5, not one at a time
- Each question should gather NEW information (don't re-ask)
- Use the templates exactly — they have size limits that matter for quality
- Commit all `.planning/` files with: `docs: initialize GSD project`
