# Phase 05 — Task Planning

Produce `tasks.md` after `plan.md` is complete. This document drives all execution in Phase 06.

Before writing `tasks.md`:
1. Read `plan.md` in full — tasks must map directly to the implementation path defined there
2. Read `init.md § Constitution` — ensure each implementation task has a corresponding test task as required by the constitution

## Output

`.dev/[NNN]-[req-name]/generated/tasks.md`

## tasks.md Structure

### Status Table

```markdown
| ID    | Task Name              | Status      | Notes |
|-------|------------------------|-------------|-------|
| T-001 | Implement X            | not-started |       |
| T-002 | Unit tests for X       | not-started |       |
| T-003 | Implement Y            | not-started |       |
```

**Status values:** `not-started` · `in-progress` · `done` · `blocked`

**ID format:** `T-001`, `T-002`, `T-003`, …

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

If you discover a bug or new requirement while working on a task, do **not** add it to `tasks.md`. Log it to `.dev/TODO.md` instead — see [00-agent-execution.md](00-agent-execution.md) for the format and rules.
