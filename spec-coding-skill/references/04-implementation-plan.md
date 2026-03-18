# Phase 04 — Implementation Plan

Produce `plan.md` after all prerequisite and algorithm design documents are complete.

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
