# Phase 06 — Create start-and-resume.md / Phase 07 — Execution

## Step 0 — Phase 06: Create start-and-resume.md (mandatory gate)

**Do not begin the execution loop until this file exists.**

Create `.dev/[NNN]-[req-name]/generated/start-and-resume.md`:

```markdown
# Start and Resume Guide — [NNN]-[req-name]

## Quick Start
1. Read `init.md` — requirement scope
2. Read `plan.md` — technical approach
3. Read `tasks.md` — find the next `not-started` task
4. Review the standards sections below before writing any code

## Resuming After Interruption
1. Open `tasks.md` and find the first task not in `done`
2. If a task is `in-progress`, read its Notes for context before continuing
3. If a task is `blocked`, read the Notes and address the blocker first
4. Review the standards sections below before continuing

## Key Documents
- Requirement: `.dev/[NNN]-[req-name]/init.md`
- Plan: `.dev/[NNN]-[req-name]/generated/plan.md`
- Tasks: `.dev/[NNN]-[req-name]/generated/tasks.md`

---

## Constitution
<!-- Copy the full # Constitution section from init.md here -->

---

## OOP & SOLID Principles (applicable rules)
<!-- Copy the relevant rules from references/07-oop-principles.md that apply to this requirement -->

---

## Coding Standards (applicable rules)
<!-- Copy the relevant sections from references/08-coding-standards.md for the languages used in this requirement -->

---

## Git Workflow

Branch: `<type>/[NNN]-[req-name]`

Commit format:
```
[NNN] T-XXX <type>: <imperative summary ≤ 72 chars>
```
Types: `feat` · `fix` · `refactor` · `test` · `docs` · `chore`
```

---

## Step 0.5 — Execution Mode Recommendation

Before starting the Execution Loop, recommend the best execution mode for this requirement:

**Step 1 — Read OMC execution skills in real time:**
- Scan `~/.claude/skills/` and OMC plugin cache directory
- Find all Tier 4 execution-class skills (ralph, team, ultrawork, ultraqa, autopilot, etc.)
- Read each skill's `Use_When` / `Do_Not_Use_When` descriptions

**Step 2 — Analyze `tasks.md`:**
- Total task count, type distribution (feature / test / UI / architecture / etc.)
- Dependency structure (sequential chain vs. parallelizable)

**Step 3 — Match against `Use_When` conditions** and recommend the best-fit execution mode to the user.

**Step 4 — If a multi-agent mode is recommended (e.g. `/team`):**

a. Read OMC agents directory in real time to get available role types (e.g. `~/.claude/plugins/cache/omc/.../agents/`)

b. Map `tasks.md` task types to roles. Naming convention:
   ```
   <omc-role-type>-<index>
   ```
   Examples: `executor-1`, `executor-2`, `designer-1`, `test-engineer-1`
   **Forbidden:** `worker-1`, `agent-2`, or any non-semantic names.

c. Present the team configuration to the user:
   > Based on `tasks.md` analysis (N tasks: X feature + Y test + Z UI), I recommend `/team`:
   > - `executor-1`, `executor-2` — T-001 ~ T-004
   > - `test-engineer-1` — T-005 ~ T-006
   > - `designer-1` — T-007
   >
   > Each worker will read `start-and-resume.md` on startup for Constitution and coding standards.
   >
   > Confirm this configuration, or tell me what to adjust?

d. After user confirmation, **agent directly invokes** the chosen execution mode, passing the team configuration and the path to this `start-and-resume.md` as worker context.

> Rules: Agent role types are read from OMC in real time — never hardcoded. When OMC adds new execution modes, this step picks them up automatically on next run.

---

## Execution Loop

Repeat for each task in `tasks.md` — **never skip a step, never batch tasks**:

```
1.  Mark task as `in-progress` in tasks.md
2.  Read plan.md and relevant generated docs to confirm the approach for this task
3.  Read all affected existing source files (in sections if large)
4.  Read start-and-resume.md § Constitution, § OOP & SOLID Principles, and § Coding Standards
      before writing any code — all rules are already copied there for this requirement
5.  Implement the minimal change needed
6.  Verify code against Constitution:
      [ ] All function/method parameters and return types annotated
      [ ] Every new file has a module-level docstring
      [ ] Every new class has a class-level docstring
      [ ] Every new public function has a full docstring (Args / Returns / Raises)
      [ ] No hardcoded secrets or environment-specific values
      [ ] No linting errors introduced
7.  Run existing tests — must pass (no regressions)
8.  Read [08-coding-standards.md](08-coding-standards.md) § Testing to confirm test file naming and coverage requirements, then write unit tests:
      [ ] Normal cases covered
      [ ] Edge cases covered
      [ ] Error / exception cases covered
9.  Run new tests — all must pass before continuing
10. Update tasks.md:
      a. Set status to `done`
      b. Write implementation summary in Notes (include any deviation from plan.md)
11. Commit following [Git Workflow](#git-workflow):
      git commit -m "[NNN] T-XXX <type>: <imperative summary ≤ 72 chars>"
```

**Mandatory flow: check docs → implement → verify → test → update tasks.md → commit**

---

## Mid-Execution New Tasks

If a new task is discovered during execution (e.g. a dependency that was missed in planning):

1. **Stop** the current task at a clean checkpoint
2. **Add the new task** to `tasks.md` (status: `not-started`, with a Notes entry explaining why it was added)
3. **Resume** the current task or switch to the new task based on dependency order

Do not execute an unplanned task without first recording it in `tasks.md`.

---

## tasks.md Update Rules

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

---

## Handling Blockers

When a task cannot proceed:

1. Record in `tasks.md` Notes: what was attempted, what failed, what is needed
2. Set task status to `blocked`
3. Ask the user for guidance before continuing

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

Format:

```
[NNN] T-XXX <type>: <short summary in imperative mood>

<optional body: what and why, not how>
```

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

## File Reading and Writing Discipline

- Read files in sections if large — never load an entire large file at once
- Write one file at a time; do not exceed ~200 lines in a single write
- Do not rewrite an entire file when only a small change is needed — use targeted edits
