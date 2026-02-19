# GSD (Get Shit Done) — VS Code Copilot Integration

You are operating with the **GSD (Get Shit Done)** system — a meta-prompting, context engineering, and spec-driven development framework. Originally built for Claude Code, it has been adapted for VS Code with GitHub Copilot Agent mode.

## Git Behavior Override

**Before any git operation**, check `.planning/config.json` → `git.enabled`.

If `git.enabled` is `false` (or the `git` key is missing):
- **Do NOT run** any git commands (`git add`, `git commit`, `git push`, `git log`, `git diff`, `git status`, `git branch`, etc.)
- **Do NOT suggest** commits or reference commit history
- **Skip** all "commit per task" and "atomic commit" instructions in workflows and prompt files
- Simply save files and move to the next task
- Track completed tasks in `STATE.md` instead of relying on commit history
- When a workflow step says "git commit", mark the task as done in `STATE.md` and continue

If `git.enabled` is `true` (default when present):
- Follow all git instructions in workflows as written

## System Overview

GSD solves **context rot** — the quality degradation that happens as AI fills its context window. It does this through structured workflows, persistent state, and fresh-context execution patterns.

## How GSD Works in VS Code / Copilot

### File Structure

All GSD resources are installed locally at `.claude/get-shit-done/`:
- **Workflows**: `.claude/get-shit-done/workflows/` — Step-by-step process definitions
- **Templates**: `.claude/get-shit-done/templates/` — Document templates
- **References**: `.claude/get-shit-done/references/` — Guidelines and patterns
- **Tools**: `.claude/get-shit-done/bin/gsd-tools.cjs` — CLI helper for state management

### Project State (`.planning/` directory)

| File | Purpose |
|------|---------|
| `PROJECT.md` | Project vision, always loaded |
| `config.json` | Workflow settings (mode, depth, model profiles) |
| `REQUIREMENTS.md` | Scoped v1/v2 requirements with REQ-IDs |
| `ROADMAP.md` | Phase breakdown with completion status |
| `STATE.md` | Decisions, blockers, position — memory across sessions |
| `research/` | Domain research results |
| Phase directories | Plans, summaries, context, verification per phase |

### Available GSD Commands (via Prompt Files)

Use `/` in Copilot Chat to invoke these prompt files from `.github/prompts/`:

| Prompt File | Equivalent Claude Code Command | Purpose |
|---|---|---|
| `gsd-new-project.prompt.md` | `/gsd:new-project` | Initialize project |
| `gsd-discuss-phase.prompt.md` | `/gsd:discuss-phase` | Capture implementation preferences |
| `gsd-plan-phase.prompt.md` | `/gsd:plan-phase` | Research + plan + verify |
| `gsd-execute-phase.prompt.md` | `/gsd:execute-phase` | Execute plans |
| `gsd-verify-work.prompt.md` | `/gsd:verify-work` | User acceptance testing |
| `gsd-progress.prompt.md` | `/gsd:progress` | Check progress |
| `gsd-quick.prompt.md` | `/gsd:quick` | Ad-hoc task with GSD guarantees |
| `gsd-debug.prompt.md` | `/gsd:debug` | Systematic debugging |
| `gsd-map-codebase.prompt.md` | `/gsd:map-codebase` | Analyze existing codebase |
| `gsd-pause-work.prompt.md` | `/gsd:pause-work` | Create handoff for session break |
| `gsd-resume-work.prompt.md` | `/gsd:resume-work` | Resume from previous session |
| `gsd-health.prompt.md` | `/gsd:health` | Validate planning directory |
| `gsd-help.prompt.md` | `/gsd:help` | Show command reference |

### Key Differences from Claude Code

1. **No subagent spawning**: VS Code Copilot cannot spawn parallel subagents like Claude Code's `Task` tool. Execute plans sequentially within the same context.
2. **No slash commands**: Instead of `/gsd:command`, use prompt files via `/gsd-command` in Copilot Chat.
3. **No hooks**: Session hooks (update checks, statusline) don't apply in VS Code.
4. **Manual workflow**: Read workflow files and follow them step-by-step rather than having them auto-execute via frontmatter routing.

### Using gsd-tools.cjs

The GSD CLI helper provides deterministic operations. Run via terminal:

```bash
node .claude/get-shit-done/bin/gsd-tools.cjs <command> [args]
```

Key commands:
- `init new-project` — Initialize project config
- `init execute-phase <N>` — Load phase execution context
- `phase-plan-index <N>` — Get plan inventory for a phase
- `state load` — Load current project state
- `progress` — Get progress data
- `validate consistency` — Check phase numbering and sync

### Git Convention

**First check `.planning/config.json` → `git.enabled`.** If `false`, skip all git operations below.

When git is enabled, GSD uses atomic commits per task:
```
feat(08-02): implement user registration endpoint
docs(08-02): complete phase 2 plan
```

Format: `type(phase): description`

### When Following GSD Workflows

1. Always read the referenced workflow file before executing
2. Read `STATE.md` before any operation for current project context
3. Follow the workflow steps in order — don't skip validation gates
4. Create documents using the templates in `.claude/get-shit-done/templates/`
5. Update `STATE.md` after completing significant work
6. Make atomic git commits per task (**only if `git.enabled` is `true` in `.planning/config.json`**)
