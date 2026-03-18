# Phase 06 — Execution

## start-and-resume.md

Before starting execution, produce `.dev/[NNN]-[req-name]/generated/start-and-resume.md`:

```markdown
# Start and Resume Guide

## Quick Start
1. Read `init.md` to understand the requirement scope and constitution
2. Read `plan.md` to understand the technical approach
3. Read `tasks.md` to find the next pending task

## Resuming After Interruption
1. Open `tasks.md` and find the first task not in `done`
2. If a task is `in-progress`, read its Notes for context before continuing
3. If a task is `blocked`, read the Notes and address the blocker first

## Key Documents
- Requirement definition: `.dev/[NNN]-[req-name]/init.md`
- Technical plan: `.dev/[NNN]-[req-name]/generated/plan.md`
- Task list: `.dev/[NNN]-[req-name]/generated/tasks.md`
```

---

## Execution Loop

Repeat for each task in `tasks.md`:

```
1. Mark task as `in-progress` in tasks.md
2. Read plan.md and relevant generated docs to confirm the approach
3. Read all affected existing source files (in sections if large)
4. Implement the minimal change needed
5. Run existing tests — ensure no regressions
6. Write new unit tests covering this task's logic
7. Run new tests — they must all pass before continuing
8. Update tasks.md:
   a. Set task status to `done`
   b. Record implementation summary in the task's Notes — note any deviation from plan.md
9. Commit: git commit following the Git Workflow rules
```

**The mandatory flow for every task: check docs → modify code → update tasks.md**

**Never skip a step. Never batch multiple tasks into one loop iteration.**

### Mid-Execution New Tasks

If a new task is discovered during execution (e.g. a dependency that was missed in planning):

1. **Stop** the current task at a clean checkpoint
2. **Add the new task** to `tasks.md` (status: `not-started`, with a Notes entry explaining why it was added)
3. **Resume** the current task or switch to the new task based on dependency order

Do not execute an unplanned task without first recording it in `tasks.md`.

### tasks.md Update Rules

Every update to `tasks.md` must include **two things**:

1. **Status change** — update the status column (`not-started` → `in-progress` → `done` / `blocked`)
2. **Implementation summary** — update the task's Notes with what was actually done, especially if the implementation deviated from `plan.md`

Example Notes entry for a completed task:

```
Implemented using Strategy pattern instead of planned if/elif chain.
Added UserScoreCache class not in original plan — required for performance.
All 12 tests pass.
```

---

## Requirement Complete

When all tasks in `tasks.md` reach `done`:

1. Notify the user:
   > ✅ All tasks complete for requirement [NNN]-[req-name].
   > Summary: X tasks completed, Y deviations from plan recorded in tasks.md.
2. Check `.dev/TODO.md` — if any items have `Source` matching this requirement, update their status to `pending` (confirm they are still relevant)
3. Suggest creating a pull request for branch `<type>/[NNN]-[req-name]`

---

## Breaking Changes Policy

Read `project_stage` from `init.md` before modifying existing code:

- **`pre-launch` + adding a new module**: breaking changes to existing structure are **allowed**. Prioritize a clean, unambiguous project and code structure over compatibility with the current state.
- **`live`** or **modifying an existing module**: breaking changes are **not allowed**. Make the smallest change that satisfies the requirement. Ask the user before any structural refactor.

---

## Reading Before Writing

Before modifying any existing file:

- Read the file (in sections if large — never load the entire file at once)
- Identify reusable functions — do not reimplement what already exists
- Understand the call graph: who calls this, what does this call
- Make the smallest change that satisfies the task requirement

See [00-agent-execution.md](00-agent-execution.md) for full file reading and writing discipline.

---

## Quality Checkpoints

See [00-agent-execution.md](00-agent-execution.md) for the full self-check checklist that must be completed before marking any task `done`.

---

## Handling Blockers

See [00-agent-execution.md](00-agent-execution.md) for the full blocker handling policy.

---

## Git Workflow

### Branch

Create a new branch for each requirement initialized in Phase 01:

```bash
git checkout -b <type>/[NNN]-[req-name]
# e.g. git checkout -b feat/001-user-auth
#      git checkout -b fix/002-payment-timeout
#      git checkout -b refactor/003-extract-base-processor
```

**Type values:** `feat` · `fix` · `refactor` · `test` · `docs` · `chore`

### Commit Messages

Follow Google Style commit messages. Format:

```
[NNN] T-XXX <type>: <short summary in imperative mood>

<optional body: what and why, not how>
```

**Type values:** `feat` · `fix` · `refactor` · `test` · `docs` · `chore`

**Examples:**

```
[001] T-001 feat: add UserRepository with CRUD operations

[001] T-002 test: add unit tests for UserRepository

[001] T-005 refactor: extract PaymentProcessor base class
```

**Rules:**
- Commit after each task reaches `done`
- Summary line ≤ 72 characters
- Use imperative mood: "add", "fix", "extract" — not "added", "fixing"
- One logical change per commit — do not batch multiple tasks into one commit

---

## File Writing Discipline

See [00-agent-execution.md](00-agent-execution.md) for file reading and writing discipline rules.
