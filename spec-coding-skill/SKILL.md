---
name: spec-coding-skill
description: "Guides LLM agents through large-scale coding tasks using a spec-driven, phase-by-phase methodology with round-based iterative execution. Covers requirement definition, planning, algorithm design, implementation, deviation handling, and round-to-round issue tracking. Includes OOP/SOLID principles, language-specific coding standards, a dual-layer state machine for flow decisions, and trigger-driven extended methodologies for TDD, structured debugging, architecture deepening, terminology alignment, and dual-axis review. Keywords: spec-driven development, round-based execution, iterative development, deviation protocol, state machine, coding methodology, software engineering workflow, requirements-to-code pipeline, project blueprint, TDD, debugging, architecture review, method selection."
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
  - tdd
  - debugging
  - architecture review
  - code review
  - bug diagnosis
  - term alignment
  - ADR
metadata:
  version: "1.4"
---

# Spec Coding Skill

A spec-driven methodology for LLM agents to define, plan, and implement coding work with quality and consistency. Uses **round-based iterative execution** — when problems arise during implementation, they are logged and resolved in subsequent rounds rather than silently deviating from the plan. Includes automatic **task sizing** — trivial fixes skip document ceremony, while complex features get the full structured flow.

## STOP — Mandatory Startup Sequence

> **Do each step in order. Do not skip any step. Each step is a hard gate — the next step depends on information from the previous one.**

### Step 1 — Read Global Agent Rules (DO NOT SKIP)

**MUST** read [00-agent-execution.md](references/constitution/00-agent-execution.md) before doing anything else. This file defines rules that override all phase-specific instructions: Task Sizing, Phase Transition Re-read Discipline, Self-Check, Deviation Protocol, and Round-based Execution Model. **If you skip this file, you will miss mandatory rules and violate the process.**

### Step 2 — Check Project State

Check if `.dev/blueprint.md` exists:

| Blueprint exists? | Action |
|---|---|
| **Yes** | Run the [Session Bootstrap](references/execution/00-start-and-resume.md#session-bootstrap-new-agent-session-on-existing-project). Present full project status, then resume the active requirement. |
| **No** | Proceed to Step 3 (new requirement). |

### Step 3 — Task Sizing (MANDATORY)

Run [Task Sizing](references/constitution/00-agent-execution.md#task-sizing) to determine workflow depth. Announce the size to the user before proceeding.

| Size | Workflow |
|------|----------|
| **XS** (1 file, trivial) | Brief `init.md` with inline plan → implement → test → commit |
| **S** (1-3 files, well-defined) | Lightweight `init.md` (Spec + Requirements + Plan) → implement → test → commit |
| **M** (multi-module) | Full Phase 01–07 flow |
| **L/XL** (cross-requirement) | Full flow + Blueprint layer + Phase 02 mandatory |

### Step 4 — Phase Transition Rules (applies to ALL phases)

Before entering ANY phase, you MUST:

1. **Re-read the phase reference file** — see [Phase Transition Re-read Discipline](references/constitution/00-agent-execution.md#phase-transition-re-read-discipline) for the exact file per phase. Never enter a phase cold.
2. **Run Method Selection** — evaluate every method tagged for that phase against trigger conditions. Apply if triggers match; provide specific factual justification if they don't. See [Method Selection](#method-selection--trigger-driven) below. Generic "not needed" is invalid.

### Step 5 — Enter Phase 01

Follow [00-initialization.md](references/phases/00-initialization.md). XS/S: Pre-flight Checks then lightweight init.md. M+: full init.md with Spec, Requirements, Action Items, Constitution.

### Step 6 — Work Through Phases in Order

Proceed through Phase 02–07. At each phase entry, repeat Step 4 (re-read phase doc + run Method Selection). After Phase 07, perform Round Review (Step 7).

### Step 7 — Round Review (MANDATORY after Phase 07)

Check `issues.md`. Open issues → re-enter Phase 01 for Round N+1. No open issues → done.

Execution is **round-based**: problems found during a round are captured in `issues.md` and resolved in the next round, not silently fixed mid-flight. See [01-round-mechanism.md](references/execution/01-round-mechanism.md) for the state machine and decision matrix.

All generated documents go under `.dev/[NNN]-[req-name]/` in the project root.

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

Phases 02 and 03 are **independent** (parallel, not sequential) — both, one, or neither may be needed.

## Phase Reference

| Phase | Output | Reference |
|---|---|---|
| Blueprint layer | `.dev/blueprint.md` | [05-blueprint-management.md](references/phases/05-blueprint-management.md) |
| 01 Initialization | `init.md` | [00-initialization.md](references/phases/00-initialization.md) |
| 02 Prerequisite Tasks | `inspect.md` / `research.md` / `profiling.md` / `diagnosis.md` | [01-prerequisite-tasks.md](references/phases/01-prerequisite-tasks.md) |
| 03 Algorithm Design | `algorithm-design.md` | [02-algorithm-design.md](references/phases/02-algorithm-design.md) |
| 04 Implementation Plan | `plan.md` | [03-implementation-plan.md](references/phases/03-implementation-plan.md) |
| 05 Task Planning | `tasks.md` | [04-task-planning.md](references/phases/04-task-planning.md) |
| 06 Create start-and-resume.md | `start-and-resume.md` | [00-start-and-resume.md](references/execution/00-start-and-resume.md) § Step 0 |
| 07 Execution | code | [00-start-and-resume.md](references/execution/00-start-and-resume.md) |
| Round Review | `issues.md` + Round History + N+1 loop | [01-round-mechanism.md](references/execution/01-round-mechanism.md) |

> **Blueprint**: `.dev/blueprint.md` tracks all requirements, phases, and dependencies. Updated at every phase transition. See [05-blueprint-management.md](references/phases/05-blueprint-management.md).

## Coding Reference

| Topic | Reference |
|---|---|
| OOP & SOLID Principles | [01-oop-principles.md](references/constitution/01-oop-principles.md) |
| Coding Standards (all languages) | [02-coding-standards.md](references/constitution/02-coding-standards.md) |
| Git Workflow | [03-git-workflow.md](references/constitution/03-git-workflow.md) |
| Round Mechanism | [01-round-mechanism.md](references/execution/01-round-mechanism.md) |
| Reader's Guide | [README.md](references/README.md) |

## Methods

| Method | Phase | When to Use | Reference |
|--------|-------|-------------|-----------|
| Term Grilling + ADR | 01 / 02 / 04 | Fuzzy terminology, hard-to-reverse decisions | [00-term-grilling-and-adr.md](references/methods/00-term-grilling-and-adr.md) |
| Vertical Slice TDD | 07 | Tasks with clear public interfaces | [01-vertical-slice-tdd.md](references/methods/01-vertical-slice-tdd.md) |
| Dual-Axis Review | Round Complete / PR | Significant code reviews | [02-dual-axis-review.md](references/methods/02-dual-axis-review.md) |
| Architecture Deepening | 02 | Modifying code with shallow modules | [03-architecture-deepening.md](references/methods/03-architecture-deepening.md) |
| Structured Debugging | 02 | Non-trivial bug fixes | [04-structured-debugging.md](references/methods/04-structured-debugging.md) |

### Method Selection — Trigger-Driven

Methods are gate-checked by trigger conditions, not optional suggestions. At each phase entry, the agent **MUST**:

1. Identify all methods tagged for that phase (see the table above)
2. Read each method's `## When to Use` / `## Do Not Use` sections from its standalone file
3. Evaluate trigger conditions against current context
4. Apply if triggers match; produce a specific factual justification if they do not

The Method Selection table is produced as a planning artifact at every applicable phase entry. Skipping without justification is not permitted. The trigger conditions are defined in each method's documentation — the phase docs do not duplicate them.

## Core Rules

Rules unique to this skill. See `00-agent-execution.md` for: `§ File Reading Discipline` (read before edit), `§ Self-Check` (tests/quality), `§ Phase Transition Re-read Discipline` (always re-read phase doc before starting), `§ Round-based Execution Model` (issues.md gating), `§ Design Principle Compliance` (OOP/SOLID).

- Minimize changes when project `live`; breaking changes OK for `pre-launch` + new modules
- All identifiers, comments, docs in English
- **Maintain the blueprint** — update `.dev/blueprint.md` at every phase transition
- **Start with references/README.md** — reader's guide before diving into phase docs
- **Run Method Selection at every phase entry** — evaluate tagged methods against trigger conditions. Apply if triggers match; justify if they don't.
- **⚠ Follow the Deviation Protocol** — when implementation differs from plan.md: stop, assess severity, log to `issues.md`, present options to user. **Never silently modify plan.md, add unplanned files, or change interfaces without logging the deviation.** See `execution/00-start-and-resume.md § Deviation Protocol`.
