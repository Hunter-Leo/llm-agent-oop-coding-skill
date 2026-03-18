---
name: spec-coding-skill
description: Guides LLM agents through large-scale coding tasks using a spec-driven, phase-by-phase methodology covering requirement definition, planning, algorithm design, and implementation with OOP principles and language-specific coding standards. Use when starting a new software project, implementing a complex feature, refactoring existing code, or when you need a disciplined step-by-step approach to any non-trivial coding task.
metadata:
  version: "1.0"
---

# Spec Coding Skill

A spec-driven methodology for LLM agents to define, plan, and implement large-scale coding work with quality and consistency.

## Phase Overview

```
Phase 01 — Initialization        (required)
              ↓
Phase 02 — Prerequisite Tasks    (optional, LLM judges)
Phase 03 — Algorithm Design      (optional, LLM judges)
              ↓
Phase 04 — Implementation Plan   (required)
              ↓
Phase 05 — Task Planning         (required)
              ↓
Phase 06 — Execution             (required)
```

Phases 02 and 03 are independent and may both, one, or neither be needed.

## Phases

| Phase | Document | Reference |
|---|---|---|
| 01 Initialization | `init.md` | [01-initialization.md](references/01-initialization.md) |
| 02 Prerequisite Tasks | `inspect.md` / `research.md` / `profiling.md` / `diagnosis.md` | [02-prerequisite-tasks.md](references/02-prerequisite-tasks.md) |
| 03 Algorithm Design | `algorithm-design.md` | [03-algorithm-design.md](references/03-algorithm-design.md) |
| 04 Implementation Plan | `plan.md` | [04-implementation-plan.md](references/04-implementation-plan.md) |
| 05 Task Planning | `tasks.md` | [05-task-planning.md](references/05-task-planning.md) |
| 06 Execution | `start-and-resume.md` + code | [06-start-and-resume.md](references/06-start-and-resume.md) |

## Coding Guidelines

| Topic | Reference |
|---|---|
| Agent Execution Requirements | [00-agent-execution.md](references/00-agent-execution.md) |
| OOP & SOLID Principles | [07-oop-principles.md](references/07-oop-principles.md) |
| Coding Standards (all languages) | [08-coding-standards.md](references/08-coding-standards.md) |

## Core Rules

- Never skip a required phase
- Read existing code before modifying anything
- Write and pass unit tests before moving to the next task
- Minimize changes when the project is `live`; breaking changes are allowed for `pre-launch` + new modules (see [06-start-and-resume.md](references/06-start-and-resume.md))
- Never hardcode secrets; use environment variables
- All identifiers, comments, and docs must be in English
