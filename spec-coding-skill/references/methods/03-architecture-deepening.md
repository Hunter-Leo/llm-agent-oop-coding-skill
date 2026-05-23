---
name: architecture-deepening
description: "Systematic identification and refactoring of shallow modules — code that delegates without adding abstraction value. Use during Phase 02 Prerequisite Tasks (inspect.md) when modifying existing code, especially in codebases with thin service/repository layers, facade-heavy patterns, or excessive pass-through wrappers. Do NOT use for greenfield projects."
tags: [architecture, refactoring, shallow-modules, deletion-test, phase-02, inspect]
---

# Architecture Deepening

Find shallow modules and propose deepening refactors. Adapted from the improve-codebase-architecture methodology.

## When to Use

Codebase patterns that suggest shallow modules:
- Modules whose public methods are one-line delegations
- Classes that add no state or behavior
- Layers of indirection without abstraction benefit
- Pass-through wrappers (service → repository patterns where service just delegates)

Do **not** use for greenfield projects (nothing to inspect) or one-time scripts.

## Glossary

Use these terms exactly in every suggestion:

| Term | Meaning |
|------|---------|
| **Module** | Anything with an interface and an implementation (function, class, package) |
| **Interface** | Everything a caller must know to use the module correctly (types, invariants, error modes, ordering, config) |
| **Depth** | Leverage at the interface — a module is **deep** when much behavior sits behind a small interface; **shallow** when interface is nearly as complex as implementation |
| **Seam** | A place where you can alter behavior without editing in that place |
| **Adapter** | A concrete thing that satisfies an interface at a seam |

## Deletion Test

For each candidate module, ask: "If I delete this module and inline its calls, does the codebase become simpler or more complex?"

- **Simpler** → the module was shallow (pass-through, hiding nothing). Consider removing or merging.
- **More complex** → the module was earning its keep (hiding real complexity). Keep it, it's doing its job.

## Process

### 1. Explore

Walk the codebase and note where you experience friction:
- Where does understanding one concept require bouncing between many small modules?
- Where are interfaces nearly as complex as implementations?
- Where have pure functions been extracted just for testability, but real bugs hide in how they're called?
- Where do tightly-coupled modules leak across their seams?
- Which parts are hard to test through their current interface?

### 2. Present Candidates

For each candidate, document:
- **Files** — which files/modules are involved
- **Problem** — why the current structure causes friction (apply deletion test)
- **Solution** — proposed deepening: inline, merge with caller, extract real abstraction, split deeper
- **Benefits** — in terms of locality (fix concentrated in one place) and leverage (more behavior per interface unit)

### 3. Grilling Loop

For the selected candidate, stress-test the proposal:
- "What does this proposed seam actually hide?"
- "If we inline this module, where does the complexity go?"
- "Does this deepening make testing easier or harder?"

Integrate with methods/00-term-grilling-and-adr.md:
- **Naming a deepened module after a new concept?** Add the term to CONTEXT.md
- **User rejects the candidate with a load-bearing reason?** Offer an ADR

## Output

- Extended `inspect.md` section with findings
- ADR files for each deepening decision
- Optional: updated CONTEXT.md with new architectural terms
