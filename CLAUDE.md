# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A spec-driven skill package (`spec-coding-skill`) for LLM agents. When activated, it guides an agent through a structured, phase-by-phase workflow — from requirement definition to code execution — producing concrete artifacts at each phase.

## Installation

```bash
npx skills add https://github.com/Hunter-Leo/llm-agent-oop-coding-skill --skill spec-coding-skill -a kiro-cli -g -y
```

## Architecture

The skill lives entirely under `spec-coding-skill/`:

```
spec-coding-skill/
├── SKILL.md                        # Entry point — phase overview and core rules
└── references/
    ├── 00-agent-execution.md       # Global agent behavior rules (highest precedence)
    ├── 01-initialization.md        # Phase 01: create init.md
    ├── 02-prerequisite-tasks.md    # Phase 02: inspect / research / profiling / diagnosis
    ├── 03-algorithm-design.md      # Phase 03: algorithm design
    ├── 04-implementation-plan.md   # Phase 04: create plan.md
    ├── 05-task-planning.md         # Phase 05: create tasks.md
    ├── 06-start-and-resume.md      # Phase 06+07: execution loop and resumption
    ├── 07-oop-principles.md        # OOP & SOLID principles with examples
    ├── 08-coding-standards.md      # Coding standards: universal + Python/TS/Java
    ├── 09-blueprint-management.md  # Blueprint layer: project-level req roadmap
    └── 10-git-workflow.md          # Git: branch, commit, PR/merge, tagging
```

### Phase Flow

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

### Generated Output Convention

When the skill is used on a project, all generated documents go under `.dev/[NNN]-[req-name]/`:

```
.dev/
├── TODO.md                          # Cross-requirement backlog
└── 001-your-requirement/
    ├── init.md                      # Requirement definition
    └── generated/
        ├── plan.md
        ├── tasks.md
        ├── start-and-resume.md
        └── inspect.md / research.md / ...
```

## Key Design Decisions

- `00-agent-execution.md` takes precedence over all other reference files — always read it first
- `06-start-and-resume.md` is self-contained: it inlines the Constitution, OOP principles, and coding standards so agents resuming mid-task don't need to re-read multiple files. Git workflow is extracted to `10-git-workflow.md` for shared access from XS/S and M+ flows.
- Phases 02 and 03 are independent of each other — both, one, or neither may apply
- `start-and-resume.md` must exist before any task execution begins (mandatory gate)

## When Modifying This Skill

- The skill's own rules apply: read files before editing, one file at a time, targeted edits over full rewrites
- All identifiers, comments, and documentation must be in English
- `SKILL.md` is the user-facing entry point — keep it in sync with `references/` content
- If adding a new phase or reference file, update the phase table in both `SKILL.md` and `README.md`
