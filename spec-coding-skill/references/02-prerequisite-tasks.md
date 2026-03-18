# Phase 02 — Prerequisite Tasks

This phase is **optional**. Evaluate each trigger below and produce the corresponding document if the condition is met. Multiple documents may be needed.

## Trigger Conditions

| Condition | Document to produce |
|---|---|
| Requirement modifies existing code | `inspect.md` |
| Requirement introduces a new technology or third-party library | `research.md` |
| Requirement involves performance issues or optimization | `profiling.md` |
| Requirement is a bug fix | `diagnosis.md` |

## inspect.md

**When:** any modification to existing code.

**Contents:**
- Directory and file structure overview
- Key classes, modules, and their responsibilities
- Call graph of affected components
- Reusable functions and patterns already present
- Risks and areas requiring care during modification

## research.md

**When:** adopting a new framework, library, or technology.

**Contents:**
- Technology options considered (at least two alternatives)
- Pros and cons of each option
- Selected option and rationale
- Integration approach and known limitations
- Relevant documentation links

## profiling.md

**When:** requirement involves performance degradation, optimization, or scalability.

**Contents:**
- Current performance baseline (measured, not assumed)
- Identified bottlenecks and their locations
- Root cause analysis
- Proposed optimization strategies
- Expected improvement targets

## diagnosis.md

**When:** task is a bug fix.

**Contents:**
- Bug description and reproduction steps
- Affected code paths
- Root cause analysis
- Proposed fix approach
- Regression risk assessment
