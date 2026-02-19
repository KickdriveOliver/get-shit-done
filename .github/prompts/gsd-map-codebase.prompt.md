---
agent: 'agent'
description: "GSD: Analyze existing codebase to produce structured documentation"
---

# GSD: Map Codebase

You are executing the GSD **map-codebase** workflow. This analyzes an existing codebase to produce structured documentation.

## Setup

1. Read the workflow: `.claude/get-shit-done/workflows/map-codebase.md`
2. Read codebase templates: `.claude/get-shit-done/templates/codebase/`
3. Check for `.planning/STATE.md` (may not exist yet)

## Workflow

Analyze the codebase across these focus areas and write documents to `.planning/codebase/`:

1. **Stack & Dependencies** — Languages, frameworks, package managers, versions
2. **Architecture** — Directory structure, module organization, patterns used
3. **API Surface** — Endpoints, routes, public interfaces
4. **Data Layer** — Databases, schemas, models, migrations
5. **Build & Deploy** — Build tools, CI/CD, scripts, environments
6. **Testing** — Test framework, coverage, test patterns
7. **Conventions** — Naming, code style, commit patterns, documentation

**Creates:** `.planning/codebase/` with 7 structured documents

## When to Use

- Before `/gsd:new-project` on brownfield codebases
- After significant changes to refresh understanding
- Onboarding to unfamiliar code
- Before major refactoring

## Important for VS Code Copilot

Since there are no parallel subagents, analyze each focus area sequentially. Use workspace search and file reading efficiently — don't try to read every file, sample key files in each area.
