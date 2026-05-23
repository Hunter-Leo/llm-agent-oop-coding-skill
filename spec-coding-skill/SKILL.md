---
name: spec-coding-skill
description: "Guides LLM agents through large-scale coding tasks using a spec-driven, phase-by-phase methodology with round-based iterative execution. Covers requirement definition, planning, algorithm design, implementation, deviation handling, and round-to-round issue tracking. Includes OOP/SOLID principles, language-specific coding standards, and a dual-layer state machine for flow decisions. Keywords: spec-driven development, round-based execution, iterative development, deviation protocol, state machine, coding methodology, software engineering workflow, requirements-to-code pipeline, project blueprint."
triggers:
  - spec-driven
  - coding task
  - development workflow
  - requirements
  - implementation plan
  - coding standards
  - software project
  - phase-by-phase
  - OOP
  - SOLID
  - round
  - iteration
  - deviation
  - state machine
  - iterative
  - iterative development
metadata:
  version: "1.2"
---

# Spec Coding Skill

A spec-driven methodology for LLM agents to define, plan, and implement coding work with quality and consistency. Uses **round-based iterative execution** — when problems arise during implementation, they are logged and resolved in subsequent rounds rather than silently deviating from the plan. Includes automatic **task sizing** — trivial fixes skip document ceremony, while complex features get the full structured flow.

## When Activated

1. **Read [00-agent-execution.md](references/constitution/00-agent-execution.md) first** — these are global rules that apply to every phase and take precedence over all other instructions
2. **Check if `.dev/blueprint.md` exists** (project already has requirements):
   - **If yes**: run the [Session Bootstrap](references/execution/00-start-and-resume.md#session-bootstrap-new-agent-session-on-existing-project) to present project status, then resume the active requirement
   - **If no**: proceed with the new requirement
3. **Run Task Sizing** — see [00-agent-execution.md § Task Sizing](references/constitution/00-agent-execution.md#task-sizing). Determines whether to:
   - **XS/S**: implement directly with minimal or no documents
   - **M/L/XL**: enter the standard Phase 01–07 flow
4. **Start Phase 01** (if sized M+) — follow [01-initialization.md](references/phases/00-initialization.md) to create the requirement definition document
5. **Work through phases in order** — consult the reference file for each phase as you enter it
6. **After Phase 07 (Execution)**, perform a **Round Review**: check `issues.md`. If open issues remain, re-enter Phase 01 for Round N+1 (incremental update based on accumulated issues). If no issues, mark done.

Execution is **round-based**: problems found during a round are captured in `issues.md` and resolved in the next round, not silently fixed mid-flight. See [01-round-mechanism.md](references/execution/01-round-mechanism.md) for the state machine and decision matrix.

All generated documents go under `.dev/[NNN]-[req-name]/` in the project root.

## Blueprint Layer (above all phases)

The Blueprint is a project-level document that tracks every requirement, its current phase, status, and dependencies. It provides the bird's-eye view missing from per-requirement documents.

```
                 ┌──────────────────────────────┐
                 │     .dev/blueprint.md         │  ← created at project start
                 │  (all requirements + status)  │     updated at each phase transition
                 └──────────────────────────────┘
                              │
                              ▼
Phase 01 — Initialization    │  (registers new req in blueprint)
              ↓              │
Phase 02 — Prerequisite Tasks     (optional)
Phase 03 — Algorithm Design       (optional)
              ↓              │
Phase 04 — Implementation Plan    │  (advances req phase in blueprint)
              ↓              │
Phase 05 — Task Planning          │
              ↓              │
Phase 06 — Create start-and-resume.md
              ↓              │
Phase 07 — Execution         │  (updates req status in blueprint)
              │              │
              ▼              │
     Round Review            │  (check issues.md)
         │                   │
    ┌────┴────────┐          │
    │             │          │
  open          no open     │
  issues        issues      │
    │             │          │
    ▼             ▼          │
 Phase 01*     ✅ Done      │
 (Round N+1)   (terminal)   │
    │                       │
    └──→ back to Phase 01 ──┘
```

Blueprint management rules: [09-blueprint-management.md](references/phases/05-blueprint-management.md)

## Phase Overview

```
              ┌──────────────────────────┐
              │    Task Sizing           │  ← mandatory first step
              │  XS → direct implement   │     (see 00-agent-execution.md)
              │  S  → minimal init.md    │
              │  M+ → Phase 01-07 below  │
              └──────────────────────────┘
                          │ (M+ only)
                          ▼
Phase 01 — Initialization              (required for M+)
              ↓
Phase 02 — Prerequisite Tasks          (optional, LLM judges)
Phase 03 — Algorithm Design            (optional, LLM judges)
              ↓
Phase 04 — Implementation Plan         (required for M+)
              ↓
Phase 05 — Task Planning               (required for M+)
              ↓
Phase 06 — Create start-and-resume.md  (required for M+)
              ↓
Phase 07 — Execution                   (required for M+)
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

Phases 02 and 03 are independent — both, one, or neither may be needed.

## Phase Reference

| Phase | Output | Reference |
|---|---|---|
| Blueprint layer | `.dev/blueprint.md` | [09-blueprint-management.md](references/phases/05-blueprint-management.md) |
| 01 Initialization | `init.md` | [01-initialization.md](references/phases/00-initialization.md) |
| 02 Prerequisite Tasks | `inspect.md` / `research.md` / `profiling.md` / `diagnosis.md` | [02-prerequisite-tasks.md](references/phases/01-prerequisite-tasks.md) |
| 03 Algorithm Design | `algorithm-design.md` | [03-algorithm-design.md](references/phases/02-algorithm-design.md) |
| 04 Implementation Plan | `plan.md` | [04-implementation-plan.md](references/phases/03-implementation-plan.md) |
| 05 Task Planning | `tasks.md` | [05-task-planning.md](references/phases/04-task-planning.md) |
| 06 Create start-and-resume.md | `start-and-resume.md` | [00-start-and-resume.md](references/execution/00-start-and-resume.md) § Step 0 |
| 07 Execution | code | [00-start-and-resume.md](references/execution/00-start-and-resume.md) |
| Round Review | `issues.md` + Round History + N+1 loop | [01-round-mechanism.md](references/execution/01-round-mechanism.md) |

## Coding Reference

| Topic | Reference |
|---|---|
| OOP & SOLID Principles | [01-oop-principles.md](references/constitution/01-oop-principles.md) |
| Coding Standards (all languages) | [02-coding-standards.md](references/constitution/02-coding-standards.md) |
| Git Workflow | [03-git-workflow.md](references/constitution/03-git-workflow.md) |
| Round Mechanism | [01-round-mechanism.md](references/execution/01-round-mechanism.md) |
| Reader's Guide | [README.md](references/README.md) |

## Core Rules

- Never skip a required phase
- Read existing code before modifying anything
- Write and pass unit tests before moving to the next task
- Minimize changes when the project is `live`; breaking changes are allowed for `pre-launch` + new modules
- Never hardcode secrets; use environment variables
- All identifiers, comments, and docs must be in English
- **Maintain the blueprint** — update `.dev/blueprint.md` at every phase transition for any requirement
- **Start with references/README.md** — check the reader's guide to find the right reference file for the current task
- **Round-based execution** — after Phase 07, check `issues.md`. If open issues remain, offer user to start Round N+1. Do not silently start new rounds.
- **Phase 07 entry** — re-read `execution/00-start-and-resume.md` and `execution/01-round-mechanism.md` before entering the execution loop
- **Follow the Deviation Protocol** — when implementation contradicts the plan, do not silently modify course. Stop, log to `issues.md`, present options to the user, and act on their decision. See `execution/00-start-and-resume.md § Deviation Protocol`.
