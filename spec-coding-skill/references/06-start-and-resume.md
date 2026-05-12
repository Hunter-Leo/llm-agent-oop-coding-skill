# Phase 06 — Create start-and-resume.md / Phase 07 — Execution

**Entry (Phase 06):** `tasks.md` exists at `.dev/[NNN]-[req-name]/generated/tasks.md`.
**Exit (Phase 06):** `start-and-resume.md` created, execution mode confirmed.
**Entry (Phase 07):** `start-and-resume.md` exists (mandatory gate).
**Exit (Phase 07):** All tasks in `tasks.md` reach `done`, `.dev/blueprint.md` updated.

## Step 0 — Phase 06: Create start-and-resume.md (mandatory gate)

**Do not begin the execution loop until this file exists.**

Create `.dev/[NNN]-[req-name]/generated/start-and-resume.md`:

```markdown
# Start and Resume Guide — [NNN]-[req-name]

## Quick Start
1. Read `.dev/blueprint.md` — project-wide status across all requirements
2. Read `init.md` — this requirement's scope
3. Read `plan.md` — technical approach
4. Read `tasks.md` — find the next `not-started` task
5. Review the standards sections below before writing any code

## Resuming After Interruption
1. Read `00-agent-execution.md § Handling Project Overview Queries` — use this format to present the full project status when asked
2. Read `.dev/blueprint.md` — see all requirements, their phases, and overall project status
3. Open `tasks.md` for the active requirement and find the first task not in `done`
4. If a task is `in-progress`, read its Notes for context before continuing
5. If a task is `blocked`, read the Notes and address the blocker first
6. Review the standards sections below before continuing

## Session Bootstrap (new agent session on existing project)

When entering an existing project that already has `.dev/` content, bootstrap the session before any work:

1. **Check `.dev/blueprint.md`** — if missing, scan `.dev/` directories to reconstruct it from existing `init.md` files, then inform the user

2. **Read** the blueprint and for each ▶ active or ⏸ blocked requirement, read `tasks.md` for current task detail

3. **Present** the full status to the user proactively (don't wait to be asked):

   ```
   ## Project Status
   
   Blueprint: 3 requirements | ✅ Done: 1 | ▶ Active: 1 | ⏳ Idle: 1
   Backlog:   .dev/TODO.md has 2 unresolved items
   
   | Req | Phase | Status | Priority | Next Action |
   |-----|-------|--------|----------|-------------|
   | 001 user-auth | 07 Execution | ▶ in-progress | P0 | T-003: implement login |
   | 002 task-crud | 04 Plan | ⏳ pending | P1 | wait for 001 |
   | 003 collab    | 07 Done     | ✅ done    | P2 | shipped |
   
   **Recommended**: continue with 001-user-auth (T-003 is in-progress)
   **Active requirement**: 001-user-auth
   **Next task**: T-003 — implement login
   ```

   Edge cases:
   - **All done**: "🎉 All requirements complete! Check `.dev/TODO.md` for backlog items."
   - **All blocked**: "⚠ All active requirements are blocked. See blocker notes below."
   - **Blueprint missing**: "No blueprint found. Scanned `.dev/` and found N requirements. Reconstructing blueprint..."

4. **Ask** the user which requirement to focus on, or proceed with the active one
5. **Open** `tasks.md` for the chosen requirement and find the next unstarted task

> This applies whenever `.dev/blueprint.md` exists, regardless of whether the user explicitly asked for an overview. Do not skip to task execution without first showing the blueprint.

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

**Step 1 — Check available execution skills from session context:**
- Read the list of available skills provided by the system at session start (visible in the session context / system-reminder)
- Find all execution-class skills — look for skills whose `description` mentions: autonomous execution, parallel agents, task completion, workflow automation, or persistent loops
- For each candidate skill, invoke it via the Skill tool to read its `Use_When` / `Do_Not_Use_When` descriptions
- If no execution-class skills are found, proceed directly to the Execution Loop below

**Step 2 — Analyze `tasks.md`:**
- Total task count, type distribution (feature / test / UI / architecture / etc.)
- Dependency structure (sequential chain vs. parallelizable)

**Step 3 — Match against `Use_When` conditions** and recommend the best-fit execution mode to the user.

**Step 4 — If a multi-agent mode is recommended (e.g. `/team`):**

a. Read the list of available agent types from the session context (system-reminder)

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

d. After user confirmation, **agent directly invokes** the chosen execution mode using the Skill tool (e.g. `Skill(skill='oh-my-claudecode:team', args='3:executor "<task description>"')`), and instructs each worker to read `start-and-resume.md` at startup for Constitution and coding standards.

> Rules: Agent role types are read from OMC in real time — never hardcoded. When OMC adds new execution modes, this step picks them up automatically on next run.

## After Completing Phase 06

`start-and-resume.md` now exists and the execution mode is set. Before entering the execution loop, update `.dev/blueprint.md`: advance this requirement's Phase to `06 Start-and-resume` and Status to `▶ in-progress` (execution is about to begin).

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

1. **Update `.dev/blueprint.md`** — set this requirement's Phase to `07 Done` and Status to `✅ done`. Read [09-blueprint-management.md](09-blueprint-management.md) for the full blueprint update rules.
2. Notify the user:
   > ✅ All tasks complete for requirement [NNN]-[req-name].
   > Summary: X tasks completed, Y deviations from plan recorded in tasks.md.
3. Check `.dev/TODO.md` — if any items have `Source` matching this requirement, update their status to `pending` (confirm they are still relevant)
4. Create a pull request or local merge using the format in [§ Pull Request / Local Merge](#pull-request--local-merge). For PRs: `gh pr create --title "<type>(<scope>): <summary>" --body "$(cat <<'EOF'\n...\nEOF\n)"`. For local merge: `git checkout main && git merge --no-ff <type>/[NNN]-[req-name]` with the same message format.

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

See [10-git-workflow.md](10-git-workflow.md) — all git rules live there (branch naming, commit format, pre-commit checks, PR/local merge, release tagging). This section contains only the execution-loop-specific reminders.

### In the Execution Loop

Step 11 of the execution loop commits following the format defined in [10-git-workflow.md § Commit Messages](10-git-workflow.md#commit-messages):
```
git commit -m "[NNN] T-XXX <type>: <imperative summary ≤ 72 chars>"
```

### At Requirement Complete

Step 4 of [§ Requirement Complete](#requirement-complete) creates a PR or local merge using the format in [10-git-workflow.md § Pull Request / Local Merge](10-git-workflow.md#pull-request--local-merge).

---

## File Reading and Writing Discipline

- Read files in sections if large — never load an entire large file at once
- Write one file at a time; do not exceed ~200 lines in a single write
- Do not rewrite an entire file when only a small change is needed — use targeted edits
