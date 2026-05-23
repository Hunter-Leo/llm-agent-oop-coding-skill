# spec-coding-skill

A spec-driven skill for LLM agents to plan and implement large-scale coding tasks with structure, consistency, and quality. Uses **round-based iterative execution** — problems found during implementation are logged and resolved in subsequent rounds rather than silently deviating from the plan. Includes a project-level **Blueprint** for tracking all requirements, their phases, dependencies, and status at a glance.

## What It Does

Guides an agent through a structured, document-first workflow with round-based execution. Each round is a full pass through the spec-driven flow:

```
Task Sizing → Phase 01-05 → Phase 06 → Phase 07 → Review → (done or next round)
                                                                      |
                                                                      v
                                                              Phase 01* (Round N+1)
                                                              → back to Phase 02-07
```

If problems arise during execution, they are captured in `issues.md` and resolved in the next round — the agent never silently deviates from the plan.

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

1. Size the task (XS/S skip ceremony, M+ enter full flow)
2. Create `.dev/[NNN]-[req-name]/init.md` with requirement definition
3. Register the requirement in `.dev/blueprint.md`
4. Produce planning documents under `.dev/[NNN]-[req-name]/generated/`
5. Execute tasks in rounds, each round having its own `plan.md` and `tasks.md`
6. If issues arise, log them to `issues.md` and resolve in the next round

### Project Structure

```
.dev/
├── blueprint.md                     # Project-wide req overview (status, deps, milestones)
├── TODO.md                          # Cross-requirement backlog
└── 001-your-requirement/
    ├── init.md                      # Requirement definition
    ├── issues.md                    # Cross-round issue log
    └── generated/
        ├── start-and-resume.md      # Shared execution rules + Round History
        └── rounds/
            ├── round-001/
            │   ├── plan.md          # Round 1 technical approach
            │   └── tasks.md         # Round 1 task list
            └── round-002/           # Current round
                ├── plan.md
                └── tasks.md
```

## Reference Files

| File | Purpose |
|---|---|
| `SKILL.md` | Entry point — phase overview and core rules |
| `references/README.md` | Reader's guide — file navigation by category |
| `references/constitution/00-agent-execution.md` | Agent behavior rules (global, all phases) |
| `references/constitution/01-oop-principles.md` | OOP & SOLID principles |
| `references/constitution/02-coding-standards.md` | Coding standards: universal + language-specific |
| `references/constitution/03-git-workflow.md` | Git: branch, commit, PR/merge, tagging |
| `references/execution/00-start-and-resume.md` | Execution loop + Deviation Protocol + Round History |
| `references/execution/01-round-mechanism.md` | Round state machine + decision matrix + issues.md format |
| `references/phases/00-initialization.md` | Phase 01: creating `init.md` |
| `references/phases/01-prerequisite-tasks.md` | Phase 02: inspect / research / profiling / diagnosis |
| `references/phases/02-algorithm-design.md` | Phase 03: algorithm design |
| `references/phases/03-implementation-plan.md` | Phase 04: creating `plan.md` |
| `references/phases/04-task-planning.md` | Phase 05: creating `tasks.md` |
| `references/phases/05-blueprint-management.md` | Blueprint layer: project-level req roadmap |

## Installation

Install using [skills](https://github.com/vercel-labs/skills):

```bash
npx skills add https://github.com/Hunter-Leo/llm-agent-oop-coding-skill --skill spec-coding-skill -a kiro-cli -g -y
```

## Requirements

- A LLM agent with file read/write and shell execution capabilities (e.g. Kiro, Claude Code)
- Git initialized in the project root
