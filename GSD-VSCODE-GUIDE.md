# GSD for VS Code + GitHub Copilot — Setup & Usage Guide

> [!WARNING] 
> This is not the original [GSD](https://github.com/glittercowboy/get-shit-done) repo, but an experimental feature, built for personal use with VS Code / GitHub Copilot Pro on Windows. 

This guide explains how to use **GSD (Get Shit Done)** within **VS Code with GitHub Copilot (Agent Mode)** on **Windows**, and how it differs from the original [GSD on Claude Code](https://github.com/glittercowboy/get-shit-done) experience.

---

## Installation (Windows + VS Code)

### Prerequisites

- **VS Code** with **GitHub Copilot** (Pro/Business/Enterprise with Agent mode)

### Step 1: Install Node.js (v16.7+)

- Install **Node.js** (v16.7+). E.g. get a pre-built Windows Installer .msi from [Download Node.js®](https://nodejs.org/en/download). 

Note: The Node.js installer has a "Tools for Native Modules - Automatically install the necessary tools" checkbox option. This is not needed if you are only using Node.js to run GSD.

- Confirm that `node` and `npm` can be run from a `cmd` terminal prompt in your VS Code workspace, e.g. 

```cmd
E:\dev\test>node -v
v24.14.0

E:\dev\test>npm -v
11.9.0
```

### Step 2: Install GSD Locally in your VS Code workspace

In the workspace where you want to use GSD-VSCODE[^1], run the following command from a `cmd` terminal window:

```cmd
E:\dev\test>npx get-shit-done-cc@1.20.4 --claude --local
Need to install the following packages:
get-shit-done-cc@1.20.4
Ok to proceed? (y) y
```

This creates a `.claude/` directory in your project with:
```
.claude/
├── commands/gsd/        # Command definitions (slash commands for Claude Code)
├── get-shit-done/       # Core system
│   ├── bin/             # gsd-tools.cjs CLI
│   ├── workflows/       # Step-by-step process definitions
│   ├── templates/       # Document templates
│   └── references/      # Guidelines and patterns
├── agents/              # Agent definitions
└── settings.json        # Claude Code settings (not used by Copilot)
```

[^1]: The original GSD `README.md` uses `npx get-shit-done-cc@latest` for installation, but this VSCODE variant intentionally freezes to a specific GSD version, since the copilot prompt files are generated/updated in a manual step and only infrequently.

### Step 3: Add Copilot Integration Files

Copy the `.github/prompts` folder and the `.github/copilot-instructions.md` file from this repository to your project:

```
.github/
├── copilot-instructions.md    # Custom instructions for Copilot
└── prompts/                   # Prompt files (= GSD commands)
    ├── gsd-new-project.prompt.md
    ├── gsd-discuss-phase.prompt.md
    ├── gsd-plan-phase.prompt.md
    ├── gsd-execute-phase.prompt.md
    ├── gsd-verify-work.prompt.md
    ├── gsd-progress.prompt.md
    ├── gsd-quick.prompt.md
    ├── gsd-debug.prompt.md
    ├── gsd-map-codebase.prompt.md
    ├── gsd-pause-work.prompt.md
    ├── gsd-resume-work.prompt.md
    ├── gsd-health.prompt.md
    └── gsd-help.prompt.md
```

### Step 4: Verify

In Copilot Chat (Agent mode), type `/` and you should see now the `gsd-*` prompt files listed.

---

## How to Use GSD in VS Code / Copilot

### Invoking Commands

| Claude Code (original) | VS Code / Copilot (adapted) |
|---|---|
| `/gsd:new-project` | Type `/gsd-new-project` in Chat |
| `/gsd:plan-phase 3` | Type `/gsd-plan-phase` then say "Phase 3" |
| `/gsd:execute-phase 1` | Type `/gsd-execute-phase` then say "Phase 1" |
| `/gsd:quick` | Type `/gsd-quick` then describe the task |
| `/gsd:progress` | Type `/gsd-progress` |

### Typical Workflow

```
1. /gsd-map-codebase      → Optional: Analyze existing codebase, if this is not a new project created from scratch
2. /gsd-new-project        → Initialize project (questions, research, requirements, roadmap)
3. /gsd-discuss-phase       → Shape how phase 1 should be built (optional but recommended)
4. /gsd-plan-phase          → Create detailed execution plans for phase 1
5. /gsd-execute-phase       → Execute the plans.
6. /gsd-verify-work         → Test that it actually works
7. Repeat 3-6 for each phase
```

### Using gsd-tools.cjs

The CLI helper works identically on Windows. Run from the VS Code terminal:

```cmd
# Check project init state
node .claude/get-shit-done/bin/gsd-tools.cjs init new-project

# Load project state
node .claude/get-shit-done/bin/gsd-tools.cjs state load

# Check progress
node .claude/get-shit-done/bin/gsd-tools.cjs progress

# Validate consistency
node .claude/get-shit-done/bin/gsd-tools.cjs validate consistency

# Analyze roadmap
node .claude/get-shit-done/bin/gsd-tools.cjs roadmap analyze
```

---

## Key Differences: Claude Code vs VS Code / Copilot

### 1. No Subagent Spawning

| Claude Code | VS Code / Copilot |
|---|---|
| Spawns parallel subagents via `Task` tool | Single agent context — everything runs sequentially |
| 4 parallel researchers investigate domain | One research pass, done sequentially |
| Multiple executors run plans simultaneously | Plans executed one at a time in order |
| Each subagent gets fresh 200k context | Same context window throughout the session |

**Impact:** Execution is slower but still follows the same methodology. For large phases, consider starting a **new Copilot chat session** for each plan to get fresh context.

### 2. Slash Commands via Prompt Files

| Claude Code | VS Code / Copilot |
|---|---|
| `/gsd:new-project` (slash command) | `/gsd-new-project` (prompt file) |
| Commands auto-load frontmatter tools | Prompt files provide instructions that Copilot follows |
| `$ARGUMENTS` passed inline | User provides arguments in the chat message |

### 3. No Session Hooks

| Claude Code | VS Code / Copilot |
|---|---|
| `SessionStart` hook checks for updates | No hooks — update manually with `npx get-shit-done-cc@latest` |
| Statusline shows model + context usage | Not available in Copilot — use `/gsd-progress` instead |

### 4. No Agent Definitions

| Claude Code | VS Code / Copilot |
|---|---|
| `agents/gsd-executor.md` defines specialized agents | Copilot has one agent — follows workflow instructions directly |
| Agents have tool restrictions (Read, Write, Bash...) | Copilot uses all available tools automatically |
| Agent model can be configured (Opus, Sonnet, Haiku) | Model is whatever Copilot is configured to use |

### 5. File References

| Claude Code | VS Code / Copilot |
|---|---|
| `@~/.claude/get-shit-done/...` (global install) | `.claude/get-shit-done/...` (local relative paths) |
| `@.planning/STATE.md` auto-loads content | Copilot reads files when instructed by the prompt |

### 6. Tool Names

| Claude Code Tool | VS Code / Copilot Equivalent |
|---|---|
| `Read` | `read_file` |
| `Write` | `create_file` |
| `Edit` | `replace_string_in_file` |
| `Bash` | `run_in_terminal` |
| `Glob` | `file_search` |
| `Grep` | `grep_search` |
| `Task` | `runSubagent` (limited) |
| `AskUserQuestion` | `ask_questions` |
| `WebFetch` | `fetch_webpage` (if available) |

### 7. Context Management

| Claude Code | VS Code / Copilot |
|---|---|
| 200k token context per subagent | Shared context within one chat session |
| Context stays at ~30-40% during orchestration | Context grows as you work — start new chats for fresh context |
| Explicit context budget management | Use shorter sessions and `/gsd-pause-work` / `/gsd-resume-work` |

---

## What Works Identically

Despite the differences, the **core GSD methodology** is fully compatible:

- **`.planning/` directory structure** — Same files, same templates, same format
- **gsd-tools.cjs CLI** — All commands work on Windows
- **Git integration** — Atomic commits, branch strategies, conventional commit format
- **Templates** — PROJECT.md, REQUIREMENTS.md, ROADMAP.md, STATE.md, PLAN.md, SUMMARY.md
- **Workflows** — The `.md` workflow files are readable instructions that any AI can follow
- **Phase lifecycle** — discuss → plan → execute → verify → repeat

---

## Disabling Git Integration in GSD for VS Code

> [!WARNING] 
> These options are *not* supported in the original [GSD](https://github.com/glittercowboy/get-shit-done) release.

GSD-VSCODE can operate without any git commands. Set `git.enabled` to `false` in `.planning/config.json`:

```json
{
  "git": {
    "enabled": false,
    "auto_commit": false,
    "check_history": false
  }
}
```

When `git.enabled` is `false`:
- No `git add`, `git commit`, `git push`, `git log`, `git diff`, `git status`, or `git branch` commands will be run
- All "commit per task" workflow steps are skipped automatically
- Progress is tracked in `STATE.md` instead of commit history
- The planning/execution/verification workflow remains fully functional

This is useful when you manage commits manually, work outside a git repo, or prefer to commit on your own terms.

---

## Tips for Best Results

1. **Start new chat sessions often** — Context rot is real. Start a fresh chat for each plan execution.
2. **Use `/gsd-pause-work` before ending** — Creates a handoff file so you can resume cleanly.
3. **Reference workflow files explicitly** — Tell Copilot "read `.claude/get-shit-done/workflows/new-project.md` and follow it step by step."
4. **One plan per session** — For execute-phase, run one PLAN.md per chat session for best quality.
5. **Use gsd-tools.cjs for state operations** — The CLI is deterministic and faster than having Copilot parse files manually.
6. **Keep plans small** — Since there's no fresh-context-per-subagent, smaller plans = better results.

---

## Quick Reference Card

| Action | Command |
|---|---|
| Initialize project | `/gsd-new-project` |
| Discuss phase preferences | `/gsd-discuss-phase` → "Phase 1" |
| Plan a phase | `/gsd-plan-phase` → "Phase 1" |
| Execute plans | `/gsd-execute-phase` → "Phase 1" |
| Verify results | `/gsd-verify-work` → "Phase 1" |
| Check progress | `/gsd-progress` |
| Quick ad-hoc task | `/gsd-quick` → describe task |
| Debug an issue | `/gsd-debug` → describe problem |
| Map existing code | `/gsd-map-codebase` |
| Pause session | `/gsd-pause-work` |
| Resume session | `/gsd-resume-work` |
| Check planning health | `/gsd-health` |
| Show all commands | `/gsd-help` |

---

## File Structure After Setup

```
your-project/
├── .claude/
│   ├── get-shit-done/          # GSD core system
│   │   ├── bin/gsd-tools.cjs    # CLI helper
│   │   ├── workflows/          # Process definitions
│   │   ├── templates/          # Document templates
│   │   └── references/         # Guidelines
│   ├── commands/gsd/           # Command definitions
│   └── agents/                 # Agent definitions
├── .github/
│   ├── copilot-instructions.md # Copilot custom instructions
│   └── prompts/                # Prompt files for GSD commands
       └── gsd-*.prompt.md     # 13 prompt files
├── .planning/                  # Created by GSD during project init
│   ├── PROJECT.md
│   ├── config.json
│   ├── REQUIREMENTS.md
│   ├── ROADMAP.md
│   ├── STATE.md
│   ├── research/
│   └── phase-N-*/              # Per-phase work
└── ...your code...
```
