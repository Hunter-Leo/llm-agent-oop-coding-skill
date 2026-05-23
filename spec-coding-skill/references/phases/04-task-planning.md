# Phase 05 — Task Planning

Produce the current round's `tasks.md` after its `plan.md` is complete. This document drives execution in Phase 07.

**Entry:** `plan.md` exists at `.dev/[NNN]-[req-name]/generated/rounds/round-NNN/plan.md`.
**Exit:** `tasks.md` created at `.dev/[NNN]-[req-name]/generated/rounds/round-NNN/tasks.md` with all tasks defined.

> **Per-round:** Each round has its own `tasks.md` under `rounds/round-NNN/`. For Round N+1, read `issues.md` and create a new `tasks.md` with tasks addressing open issues. Previous round's tasks.md stays as a historical record under its round directory.

Before writing `tasks.md`:
1. Read `plan.md` in full — tasks must map directly to the implementation path defined there
2. Read `init.md § Constitution` — ensure each implementation task has a corresponding test task as required by the constitution

## Output

`.dev/[NNN]-[req-name]/generated/rounds/round-NNN/tasks.md`

## tasks.md Structure

### Status Table

```markdown
| ID    | Type | Task Name                     | Status      | Priority | Deps     | Notes |
|-------|------|-------------------------------|-------------|----------|----------|-------|
| T-001 | feat | Implement user repository     | done        | P0       | -        |       |
| T-002 | test | Unit tests for user repo      | not-started | P0       | T-001    |       |
| T-003 | feat | Implement login endpoint      | in-progress | P0       | T-001    |       |
```

**Status values:** `not-started` · `in-progress` · `done` · `blocked`

**ID format:** `T-001`, `T-002`, `T-003`, … (sequentially numbered, no gaps)

**Type values:**

| Type | When to Use |
|------|-------------|
| `feat` | New feature, API endpoint, module, or component |
| `test` | Unit tests, integration tests, E2E tests |
| `refactor` | Code restructuring without behavior change |
| `fix` | Bug fix in existing code |
| `docs` | Documentation, comments, docstrings |
| `config` | Build config, CI/CD, tooling, dependencies |
| `db` | Database migration, schema change, seed data |
| `ui` | UI component, styling, layout |

**Priority values:** `P0` blocker · `P1` core · `P2` nice-to-have

**Task naming convention:** `<imperative verb> <noun>` — e.g. "Implement user repository", not "User repository implementation"

### Dependency Rules

- Add dependencies in the `Deps` column using `T-XXX` IDs
- A task with unmet dependencies must stay `not-started` until all deps reach `done`
- Circular dependencies are not allowed — if you find one, split the tasks or redesign

### Task Detail Block

For each task, add a detail block below the table:

```markdown
#### T-001 — Implement X

**Goal:** What this task achieves.

**Requirements:**
- Specific requirement 1
- Specific requirement 2

**Acceptance Criteria:**
- Unit tests pass
- Criterion 2

**References:** `plan.md § Implementation Path`, `src/module_a/service.py`

**Implementation Summary:** *(filled in when task reaches `done`)*
What was actually done. Note any deviation from plan.md.
```

## Task Granularity Rules

- Each task should be completable in one focused session
- Implementation task and its unit tests are **separate tasks** (T-001 implements, T-002 tests)
- A task that modifies more than two files is probably too large — split it
- Tasks must respect the dependency order defined in `plan.md`

---

## Out-of-Scope Items

If you discover a bug or new requirement while working on a task, do **not** add it to `tasks.md`. Log it to `.dev/TODO.md` instead — see [00-agent-execution.md](../constitution/00-agent-execution.md) for the format and rules.

## After Completing Phase 05

Update `.dev/blueprint.md`: advance this requirement's Phase to `05 Tasks` and Status to `✅ done` for this phase.
