---
agent: 'agent'
description: "GSD: Show all available commands and usage guide"
---

# GSD: Help

Display the GSD command reference for VS Code / Copilot.

## Read and display the help content:

Read the file `.claude/get-shit-done/workflows/help.md` and display its contents.

Then add this **VS Code / Copilot Usage Note**:

---

## Using GSD in VS Code with GitHub Copilot

In VS Code, GSD commands are available as **prompt files**. Use them by typing `#` in Copilot Chat and selecting the prompt:

| Prompt File | Purpose |
|---|---|
| `#gsd-new-project` | Initialize project |
| `#gsd-discuss-phase` | Capture implementation preferences |
| `#gsd-plan-phase` | Research + plan + verify |
| `#gsd-execute-phase` | Execute plans |
| `#gsd-verify-work` | User acceptance testing |
| `#gsd-progress` | Check progress |
| `#gsd-quick` | Ad-hoc task with GSD guarantees |
| `#gsd-debug` | Systematic debugging |
| `#gsd-map-codebase` | Analyze existing codebase |
| `#gsd-pause-work` | Create handoff for session break |
| `#gsd-resume-work` | Resume from previous session |
| `#gsd-health` | Validate planning directory |

### Key Differences from Claude Code

1. **No subagents** — Plans execute sequentially in one context
2. **No slash commands** — Use `#prompt-name` instead of `/gsd:command`
3. **gsd-tools.cjs** — Still works via terminal: `node .claude/get-shit-done/bin/gsd-tools.cjs <command>`
4. **Manual workflow** — Follow workflow steps in the referenced `.md` files

### Typical Workflow

```
#gsd-new-project → #gsd-discuss-phase → #gsd-plan-phase → #gsd-execute-phase → #gsd-verify-work → repeat
```
