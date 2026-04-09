# Phase 04 — Implementation Plan

Produce `plan.md` after all prerequisite and algorithm design documents are complete.

Before writing `plan.md`:
1. Read all completed prerequisite documents (`inspect.md`, `research.md`, `algorithm-design.md` — whichever exist)
2. Read `init.md § Constitution` — the plan must respect the coding constraints defined there
3. Read [07-oop-principles.md](07-oop-principles.md) — all class and module design must follow OOP & SOLID principles

## Output

`.dev/[NNN]-[req-name]/generated/plan.md`

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
- Class and module design must follow OOP & SOLID principles — see [07-oop-principles.md](07-oop-principles.md)
- Integration points between modules
- Error handling strategy
- Data flow between components
- Reference to `algorithm-design.md` if Phase 03 was completed

### Out of Scope

Explicitly list what this plan does **not** cover, to prevent scope creep.

## Design Compliance Review

After writing `plan.md`, review it against the following principles and fix any violations inline before proceeding to Phase 05. Do not defer violations to the execution phase.

**SOLID Principles:**
- [ ] **SRP** — each class/module has one responsibility; no class does two unrelated things
- [ ] **OCP** — new behavior is added by extension (new classes/functions), not by modifying existing ones
- [ ] **LSP** — subclasses can substitute their parent without breaking callers
- [ ] **ISP** — interfaces are fine-grained; no implementation is forced to implement unused methods
- [ ] **DIP** — high-level modules depend on abstractions, not concrete implementations

**Constitution:**
- [ ] All design decisions comply with `init.md § Constitution`
- [ ] No hardcoded secrets, environment-specific values, or magic numbers planned
- [ ] No planned duplication of existing logic (DRY)
- [ ] No planned `if/elif` chains that must be edited for every new case — replace with polymorphism

If any check fails, revise the relevant section of `plan.md` before proceeding to Phase 05.
