# spec-coding-skill

A spec-driven skill for LLM agents to plan and implement large-scale coding tasks with structure, consistency, and quality.

## What It Does

Guides an agent through six phases — from requirement definition to code execution — using a disciplined, document-first workflow. Each phase produces a concrete artifact that the next phase builds on.

```
Phase 01 — Initialization              (required)
              ↓
Phase 02 — Prerequisite Tasks          (optional)
Phase 03 — Algorithm Design            (optional)
              ↓
Phase 04 — Implementation Plan         (required)
              ↓
Phase 05 — Task Planning               (required)
              ↓
Phase 06 — Create start-and-resume.md  (required)
              ↓
Phase 07 — Execution                   (required)
```

## Usage

Activate this skill when you need to:

- Start a new software project or feature from scratch
- Refactor or extend existing code in a structured way
- Implement a complex feature that spans multiple files or modules
- Ensure coding work follows OOP principles and language-specific standards

### Getting Started

Tell your agent:

> Use the spec-coding-skill to implement [your requirement].

The agent will:

1. Ask about document language and whether to proceed interactively
2. Create `.dev/[NNN]-[req-name]/init.md` with requirement definition
3. Produce planning documents under `.dev/[NNN]-[req-name]/generated/`
4. Execute tasks one by one, running tests after each

### Project Structure

```
.dev/
├── TODO.md                          # Cross-requirement backlog
└── 001-your-requirement/
    ├── init.md                      # Requirement definition
    └── generated/
        ├── plan.md                  # Technical approach
        ├── tasks.md                 # Task list with status
        ├── start-and-resume.md      # Resumption guide
        └── inspect.md / research.md / ...  # Optional prereqs
```

## Reference Files

| File | Purpose |
|---|---|
| `SKILL.md` | Entry point — phase overview and core rules |
| `references/00-agent-execution.md` | Agent behavior rules (interaction, file discipline, self-check) |
| `references/01-initialization.md` | Phase 01: creating `init.md` |
| `references/02-prerequisite-tasks.md` | Phase 02: inspect / research / profiling / diagnosis |
| `references/03-algorithm-design.md` | Phase 03: algorithm design for complex logic |
| `references/04-implementation-plan.md` | Phase 04: creating `plan.md` |
| `references/05-task-planning.md` | Phase 05: creating `tasks.md` |
| `references/06-start-and-resume.md` | Phase 06: execution loop, git workflow, resumption |
| `references/07-oop-principles.md` | OOP & SOLID principles with examples |
| `references/08-coding-standards.md` | Coding standards: universal + Python / TypeScript / Java |

## Installation

Install using [skills](https://github.com/vercel-labs/skills):

```bash
npx skills add https://github.com/Hunter-Leo/llm-agent-oop-coding-skill --skill spec-coding-skill -a kiro-cli -g -y
```

## Requirements

- A LLM agent with file read/write and shell execution capabilities (e.g. Kiro, Claude Code)
- Git initialized in the project root
