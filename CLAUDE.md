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
    ├── README.md                   # Reader's guide by category
    ├── constitution/               # Must-read rules
    │   ├── 00-agent-execution.md   # Global agent behavior rules (highest precedence)
    │   ├── 01-oop-principles.md    # OOP & SOLID principles
    │   ├── 02-coding-standards.md  # Coding standards: universal + Python/TS/Java
    │   └── 03-git-workflow.md      # Git: branch, commit, PR/merge, tagging
    ├── execution/                  # Round entry docs
    │   ├── 00-start-and-resume.md  # Execution loop + Deviation Protocol + Round History
    │   └── 01-round-mechanism.md   # State machine + decision matrix + issues.md format
    ├── phases/                     # Phase workflow
    │   ├── 00-initialization.md    # Phase 01: create init.md
    │   ├── 01-prerequisite-tasks.md# Phase 02: inspect / research / profiling / diagnosis
    │   ├── 02-algorithm-design.md  # Phase 03: algorithm design
    │   ├── 03-implementation-plan.md # Phase 04: create plan.md with Method Selection
    │   ├── 04-task-planning.md     # Phase 05: create tasks.md
    │   └── 05-blueprint-management.md # Blueprint layer: project-level req roadmap
    └── methods/                    # Trigger-driven extended methods
        ├── 00-term-grilling-and-adr.md  # Term alignment + ADR
        ├── 01-vertical-slice-tdd.md     # RED-GREEN-REFACTOR per behavior
        ├── 02-dual-axis-review.md       # Standards + Spec parallel review
        ├── 03-architecture-deepening.md # Shallow module detection
        └── 04-structured-debugging.md   # Six-phase bug diagnosis
```

### Phase Flow (Round-Based)

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
              │
              ▼
     Round Review                      (check issues.md)
         │
    ┌────┴────────┐
    │             │
  open          no open
  issues        issues
    │             │
    ▼             ▼
 Phase 01*     ✅ Done
 (Round N+1)
    │
    └──→ back to Phase 01-07
         (next round)
```

### Generated Output Convention

```
.dev/
├── TODO.md                          # Cross-requirement backlog
└── 001-your-requirement/
    ├── init.md                      # Requirement definition
    ├── issues.md                    # Cross-round issue log
    └── generated/
        ├── start-and-resume.md      # Shared execution rules + Round History
        └── rounds/
            ├── round-001/
            │   ├── plan.md          # Round 1 plan
            │   └── tasks.md         # Round 1 tasks
            └── round-002/           # Current round
                ├── plan.md
                └── tasks.md
```

## Key Design Decisions

- `references/constitution/00-agent-execution.md` takes precedence over all other reference files — always read it first
- `references/execution/00-start-and-resume.md` is the authority for execution: it defines the execution loop, Deviation Protocol, and Round History
- `references/execution/01-round-mechanism.md` defines the dual-layer state machine for round-based flow decisions
- The round model uses **per-round documents**: each round has its own `plan.md`, `tasks.md`, and `issues.md` under `generated/rounds/round-NNN/`. `start-and-resume.md` stays shared at the `generated/` level.
- Phases 02 and 03 are independent of each other — both, one, or neither may apply
- `start-and-resume.md` must exist before any task execution begins (mandatory gate)
- `methods/` contains trigger-driven extended methodologies — evaluated at phase entry via Method Selection, not optional suggestions

## When Modifying This Skill

- The skill's own rules apply: read files before editing, one file at a time, targeted edits over full rewrites
- All identifiers, comments, and documentation must be in English
- `SKILL.md` is the user-facing entry point — keep it in sync with `references/` content
- If adding a new reference file, update the directory tree in `SKILL.md`, `README.md`, and `CLAUDE.md`
