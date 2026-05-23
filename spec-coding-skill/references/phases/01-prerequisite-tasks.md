# Phase 02 — Prerequisite Tasks

**Entry:** `init.md` exists for this requirement with Constitution defined (including OOP & SOLID rules from Phase 01), and analysis/investigation is needed before planning.
**Exit:** Required prerequisite documents created (`inspect.md`, `research.md`, `profiling.md`, `diagnosis.md`, or `ui-design.md` as needed). All OOP & SOLID considerations relevant to this phase are captured (see OOP paragraph below).

This phase is **optional**. Before deciding whether to skip it, complete the reasoning step below.

## Reasoning Step (required before skipping)

Read `init.md § Spec` and `§ Requirements` in full, then ask:

> "If I started writing `plan.md` right now, what information gaps would prevent me from making confident decisions?"

For each gap identified, produce a document that fills it. The common examples below are a starting point — not an exhaustive list. If the requirement has unique information needs, create an appropriately named document (e.g. `data-model.md`, `api-contract.md`, `ui-design.md`).

Only skip this phase if there are genuinely no information gaps.

---

## Method Selection

If Phase 02 is needed (information gaps exist), evaluate which methods apply before creating prerequisite documents.

**Methods tagged for Phase 02:**
- [Architecture Deepening](../methods/03-architecture-deepening.md) — Shallow module detection and refactoring
- [Structured Debugging](../methods/04-structured-debugging.md) — Six-phase bug diagnosis
- [Term Grilling + ADR](../methods/00-term-grilling-and-adr.md) — ADRs for decisions in prerequisite docs

**Evaluate:**

1. For each method, read its `## When to Use` / `## Do Not Use` sections
2. Check trigger conditions against the current requirement and codebase
3. Populate the table:

```
| Method | Phase | Triggers Present? | Apply? | Skip Justification |
|--------|-------|-------------------|--------|--------------------|
| Architecture Deepening | 02 | Yes/No | Yes/No | (if No: specific reason) |
| Structured Debugging | 02 | Yes/No | Yes/No | (if No: specific reason) |
| Term Grilling + ADR | 02 | Yes/No | Yes/No | (if No: specific reason) |
```

4. **If Yes**: integrate the method into the relevant prerequisite document:
   - **Architecture Deepening**: add findings to `inspect.md`
   - **Structured Debugging**: replaces the basic diagnosis template with the six-phase process
   - **Term Grilling + ADR**: create ADRs for hard-to-reverse decisions found in prerequisite docs
5. **If No**: record a specific justification. Generic "not needed" is invalid.
   Valid examples: "Greenfield project — no existing code to inspect" (Architecture Deepening), "Bug root cause immediately visible from the symptom" (Structured Debugging)

After Method Selection, proceed to the common documents below.

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

## inspect.md

**When:** any modification to existing code.

**Contents:**
- Directory and file structure overview
- Key classes, modules, and their responsibilities
- Call graph of affected components
- Reusable functions and patterns already present
- Risks and areas requiring care during modification

**OOP considerations during inspection:**
- Evaluate whether existing code follows SRP (classes doing one thing) and OCP (open for extension)
- Note any `if/elif` chains that violate OCP — these signal future refactoring needs
- Assess composition vs. inheritance patterns in the existing codebase

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

## After Completing Phase 02

If Phase 02 was executed, update `.dev/blueprint.md`: advance this requirement's Phase to `02 Prerequisite` and Status to `✅ done` for this phase.
