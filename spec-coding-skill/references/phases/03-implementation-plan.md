# Phase 04 — Implementation Plan

Produce `plan.md` after all prerequisite and algorithm design documents are complete.

**Entry:** `init.md` exists, optional prerequisite docs (Phase 02/03) are complete if needed.
**Exit:** `plan.md` created at `.dev/[NNN]-[req-name]/generated/rounds/round-NNN/plan.md`, Design Compliance Review passed.

> **Incremental update (Round N+1):** Create a new `plan.md` under `rounds/round-(N+1)/`. Read `issues.md` and the previous round's `plan.md` for context, then write an updated plan that addresses open issues. Do not overwrite the previous round's plan.md — each round keeps its own.

## Method Selection

Before writing `plan.md`, evaluate which methods apply to this phase.

**Methods tagged for Phase 04 / plan output:**
- [Term Grilling + ADR](../methods/00-term-grilling-and-adr.md) — Plan stress test, ADRs for plan decisions

**Evaluate:**

1. For each method, read its `## When to Use` / `## Do Not Use` sections
2. Check trigger conditions against the current plan content
3. Populate the table:

```
| Method | Phase | Triggers Present? | Apply? | Skip Justification |
|--------|-------|-------------------|--------|--------------------|
| Term Grilling + ADR | 04 | Yes/No | Yes/No | (if No: specific reason) |
```

4. **If Yes**: run the Plan Stress Test after drafting `plan.md`, before the Design Compliance Review. Create ADRs for hard-to-reverse decisions.
5. **If No**: record a specific justification. Generic "not needed" is invalid.
   Valid examples: "Plan makes no significant technical decisions beyond established project patterns"

After Method Selection, proceed to the prerequisites below.

Before writing `plan.md`:
1. Read all completed prerequisite documents (`inspect.md`, `research.md`, `algorithm-design.md` — whichever exist)
2. Read `init.md § Constitution` — the plan must respect the coding constraints defined there
3. Read [../constitution/01-oop-principles.md](../constitution/01-oop-principles.md) — all class and module design must follow OOP & SOLID principles

## Output

`.dev/[NNN]-[req-name]/generated/rounds/round-NNN/plan.md`

## plan.md Structure

### Project Structure

Show the intended directory and file layout:

```
src/
├── module_a/
│   ├── __init__.py
│   └── service.py
└── module_b/
    └── repository.py
```

For modifications to existing code, show only the affected parts and mark new/changed files.

### Technology Decisions

- Language version and runtime
- Key libraries and frameworks (with version constraints)
- Rationale for each significant choice
- Reference to `research.md` if Phase 02 was completed

### Implementation Path

Ordered list of implementation steps at the module level:

1. Implement `X` (depends on nothing)
2. Implement `Y` (depends on `X`)
3. Integrate `X` and `Y` in `Z`

Each step should be independently testable.

### Key Technical Points

- Non-obvious design decisions and their reasoning
- Class and module design must follow OOP & SOLID principles — see [../constitution/01-oop-principles.md](../constitution/01-oop-principles.md)
- Integration points between modules
- Error handling strategy
- Data flow between components
- Reference to `algorithm-design.md` if Phase 03 was completed

### Out of Scope

Explicitly list what this plan does **not** cover, to prevent scope creep.

## Design Compliance Review

After writing `plan.md`, review it against the following principles. If any check fails, revise the relevant section of `plan.md` to address the violation — do not just mark the checkbox. Do not defer violations to the execution phase.

**SOLID Principles:**
- [ ] **SRP** — each class/module has one responsibility; no class does two unrelated things
- [ ] **OCP** — new behavior is added by extension (new classes/functions), not by modifying existing ones
- [ ] **LSP** — subclasses can substitute their parent without breaking callers
- [ ] **ISP** — interfaces are fine-grained; no implementation is forced to implement unused methods
- [ ] **DIP** — high-level modules depend on abstractions, not concrete implementations

**Constitution:**
- [ ] All design decisions comply with `init.md § Constitution`
- [ ] All design decisions are consistent with `algorithm-design.md` if Phase 03 was completed
- [ ] No hardcoded secrets, environment-specific values, or magic numbers planned
- [ ] No planned duplication of existing logic (DRY)
- [ ] No planned `if/elif` chains that must be edited for every new case — replace with polymorphism

If any check fails, revise the relevant section of `plan.md` before proceeding to Phase 05.

## After Completing Phase 04

Update `.dev/blueprint.md`: advance this requirement's Phase to `04 Plan` and Status to `✅ done` for this phase. See [05-blueprint-management.md](05-blueprint-management.md) for the full update rules.
