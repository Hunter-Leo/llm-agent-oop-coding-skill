# Phase 02 — Prerequisite Tasks

This phase is **optional**. Before deciding whether to skip it, complete the reasoning step below.

## Reasoning Step (required before skipping)

Read `init.md § Spec` and `§ Requirements` in full, then ask:

> "If I started writing `plan.md` right now, what information gaps would prevent me from making confident decisions?"

For each gap identified, produce a document that fills it. The common examples below are a starting point — not an exhaustive list. If the requirement has unique information needs, create an appropriately named document (e.g. `data-model.md`, `api-contract.md`, `ui-design.md`).

Only skip this phase if there are genuinely no information gaps.

---

## Common Prerequisite Documents

| Scenario | Document |
|---|---|
| Requirement modifies existing code | `inspect.md` |
| Requirement introduces a new technology or third-party library | `research.md` |
| Requirement involves performance issues or optimization | `profiling.md` |
| Requirement is a bug fix | `diagnosis.md` |
| Requirement involves UI, prototyping, or web design | `ui-design.md` |
| Any other information gap | a descriptively named `.md` file |

## General Rule for Design Documents

Any prerequisite document that involves design (UI, flow, algorithm, architecture) **must** include:

1. **Diagrams** — ASCII diagrams illustrating structure, layout, or flow. Use these for:
   - UI wireframes and component hierarchy
   - Process/state flows and decision trees
   - Data structures and relationships
   - System or module interaction diagrams

2. **Use cases & examples** — at least two concrete scenarios with real inputs, steps, and expected outputs. Cover both a normal case and an edge/error case.

These are not optional — a design document without diagrams and examples is incomplete.

---

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

**When:** requirement is a bug fix.

**Contents:**
- Bug description and reproduction steps
- Affected code paths
- Root cause analysis
- Proposed fix approach
- Regression risk assessment

## ui-design.md

**When:** requirement involves UI design, prototyping, or web page creation.

**Contents:**
- User flows and interaction design
- Wireframes or layout descriptions (ASCII diagrams or structured descriptions)
- Component breakdown and hierarchy
- Design tokens: colors, typography, spacing
- Responsive behavior and breakpoints
- Accessibility requirements (WCAG level, keyboard navigation, ARIA)
- Reference designs or style guides if provided
