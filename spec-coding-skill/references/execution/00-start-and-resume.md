# Phase 06 — Create start-and-resume.md / Phase 07 — Execution

> ## STOP — Are You Resuming an Interrupted Session?
>
> If you are returning to work after any interruption (new conversation, session restart, context switch), you MUST run the [Session Bootstrap](#session-bootstrap-new-agent-session-on-existing-project) before doing anything else. **Do not skip straight to a task — you will miss critical context.**
>
> Quick self-check:
> - [ ] Read `.dev/blueprint.md` — what's the overall project status?
> - [ ] Read `issues.md` — any open issues from previous rounds?
> - [ ] Read `tasks.md` — which task is next? Any `in-progress` or `blocked` tasks?
> - [ ] Read `init.md § Constitution` — what design rules apply?

**Entry (Phase 06):** `tasks.md` exists at `.dev/[NNN]-[req-name]/generated/tasks.md`.
**Exit (Phase 06):** `start-and-resume.md` created, execution mode confirmed.
**Entry (Phase 07):** `start-and-resume.md` exists (mandatory gate).
**Exit (Phase 07):** All tasks in `tasks.md` reach `done`, `.dev/blueprint.md` updated.

## Step 0 — Phase 06: Create start-and-resume.md (mandatory gate)
**Do not begin the execution loop until this file exists.**

Create `.dev/[NNN]-[req-name]/generated/start-and-resume.md`:

```markdown

# Start and Resume Guide — [NNN]-[req-name]
**Current Round:** 1

## Quick Start
1. Read `.dev/blueprint.md` — project-wide status across all requirements
2. Read `init.md` — this requirement's scope
3. Read `rounds/round-[NNN]/plan.md` — technical approach (NNN = current round from above)
4. Read `rounds/round-[NNN]/tasks.md` — find the next `not-started` task
5. Review the standards sections below before writing any code

## Resuming After Interruption
1. Read `00-agent-execution.md § Handling Project Overview Queries` — use this format to present the full project status when asked
2. Read `.dev/blueprint.md` — see all requirements, their phases, and overall project status
3. Check `issues.md` for open issues from previous rounds
4. Open `rounds/round-[NNN]/tasks.md` for the active requirement and find the first task not in `done`
5. If a task is `in-progress`, read its Notes for context before continuing
6. If a task is `blocked`, read the Notes and address the blocker first
7. Review the standards sections below before continuing

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
5. **Open** `issues.md` for open issues (if round > 1)
6. **Open** `generated/rounds/round-[NNN]/tasks.md` for the chosen requirement and find the next unstarted task

> This applies whenever `.dev/blueprint.md` exists, regardless of whether the user explicitly asked for an overview. Do not skip to task execution without first showing the blueprint.

## Key Documents
- Requirement: `.dev/[NNN]-[req-name]/init.md`
- Issues (cross-round): `.dev/[NNN]-[req-name]/issues.md`
- Current plan: `.dev/[NNN]-[req-name]/generated/rounds/round-[NNN]/plan.md`
- Current tasks: `.dev/[NNN]-[req-name]/generated/rounds/round-[NNN]/tasks.md`

---

## Round History
Tracked in this document's own `## Round History` section (see template below). Each round adds an entry here when it completes. See [01-round-mechanism.md](01-round-mechanism.md) for the full round-based execution model, state machine definitions, and agent decision matrix.

```markdown

## Round History
**Current Round:** [N]

### Round [N] (complete)
- **Status:** [completed / blocked]
- **Location:** `generated/rounds/round-NNN/`
- **Tasks:** X planned, Y completed, Z deferred
- **New issues:** ISS-NNN, ISS-MMM
- **Open issues:** W remaining
- **Summary:** One or two sentences about what this round achieved.
```

---

## Constitution
See `init.md § Constitution` for this requirement's design rules, and [../constitution/01-oop-principles.md](../constitution/01-oop-principles.md) / [../constitution/02-coding-standards.md](../constitution/02-coding-standards.md) for the full standards.

Key principles that apply to every task:
- **OOP & SOLID** — encapsulation, SRP, OCP (no if/elif chains), DIP (depend on abstractions)
- **Type annotations** — all function/method parameters and return types
- **Docstrings** — every public class, function, and file
- **Testing** — normal cases + edge cases + error cases
- **No hardcoded secrets** — use environment variables
- **Import rules** — no lazy imports; restructure to avoid circular deps before using `TYPE_CHECKING`

---

## Git Workflow
See [../constitution/03-git-workflow.md](../constitution/03-git-workflow.md) for the full rules — this section only summarizes what you need during execution.

Branch: `<type>/[NNN]-[req-name]`

Commit format:
```
[NNN] T-XXX <type>: <imperative summary ≤ 72 chars>
```
Types: `feat` · `fix` · `refactor` · `test` · `docs` · `style` · `perf` · `ci` · `build` · `chore`
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

> ## STOP — Before You Write Any Code
>
> For **every task**, you MUST complete these checks before writing a single line:
>
> | # | Check | Skip Consequence |
> |---|-------|------------------|
> | 1 | Read `plan.md` for this task's approach | Implement the wrong design |
> | 2 | Read `init.md § Constitution` for design rules | Violate OOP/SOLID requirements |
> | 3 | Read affected source files (in sections if large) | Reimplement existing code or break callers |
> | 4 | Evaluate [Vertical Slice TDD](../methods/01-vertical-slice-tdd.md) trigger for this task | Miss TDD when it applies |
> | 5 | Confirm you are on the correct feature branch (`git branch --show-current`) | Commit to wrong branch |
>
> **After implementing**, you MUST complete the [Self-Check](../constitution/00-agent-execution.md#self-check-after-each-task) before marking the task `done`.

Repeat for each task in the current round's `tasks.md` (at `generated/rounds/round-[NNN]/tasks.md`) — **never skip a step, never batch tasks**:

**Pre-loop:** Determine the current round number from `§ Round History` above, then:
- Read [01-round-mechanism.md](01-round-mechanism.md) for the state machine, decision matrix, and issues.md format
- Read `generated/rounds/round-[NNN]/tasks.md` for the current round's task list
- Read `issues.md` for any open issues from previous rounds
- When resuming a round, re-read this document's § Deviation Protocol and § Round History.

```
0.  Ensure the correct feature branch exists and is checked out:
      git branch --show-current
      If not on the feature branch:
      git checkout -b <type>/[NNN]-[req-name]

1.  Mark task as `in-progress` in `generated/rounds/round-[NNN]/tasks.md`

2.  Read the current round's `plan.md` and relevant generated docs to confirm the approach for this task

3.  Read all affected existing source files (in sections if large)

4.  Read standards before writing any code:
      a. init.md § Constitution — design rules for this requirement
      b. § OOP & SOLID Principles above — key OOP/SOLID rules
      c. § Coding Standards above — key coding rules
      d. ../constitution/03-git-workflow.md § Branch — confirm branch naming and type

4a. **Method Selection — Vertical Slice TDD:** Evaluate whether [Vertical Slice TDD](../methods/01-vertical-slice-tdd.md) applies to this task:

    **Triggers Present?** Does this task have a clear, stable public interface?
    - **Yes**: Apply TDD. Replace steps 5-10 below with RED → GREEN → REFACTOR micro-cycles per behavior. See the method doc for integration details.
    - **No**: Record justification (e.g. "Required interface is undefined", "Migration task with no new behavior") and proceed with the standard sequence.

    This evaluation repeats for **each task** — some tasks in the same requirement may use TDD while others do not.

5.  Implement the minimal change needed

    ---

    ## ⚠ DEVIATION CHECK (run after EVERY implementation step)

    > **Before you move on to verification, ask yourself:** did anything in this implementation differ from what `plan.md` or `tasks.md` described?

    | You just... | Severity | Action |
    |-------------|----------|--------|
    | Renamed a variable, extracted a helper, adjusted an internal detail | **Low** | Continue. Note in commit message. |
    | Found a bug in unrelated code, needed a small new class/tool not in plan | **Medium** | **Log to `issues.md` NOW** (status: `open`). Then continue. |
    | plan.md design won't work, interface contract must change, dependency incompatible | **High** | **STOP immediately.** Do not write another line. Log to `issues.md`. Set task to `blocked`. Present options to user (see [§ High Severity Protocol](#high-severity-protocol-complete)). |

    **If you're unsure about severity, treat it as HIGH.** Presenting options to the user is always safer than silently changing the plan.

    **Forbidden at all severity levels:** silently modifying `plan.md`, adding files not in `plan.md`, or changing public interfaces without logging the deviation.

    ---

6.  Verify code against standards:
      [ ] Design follows SOLID principles defined in Constitution
      [ ] All function/method parameters and return types annotated
      [ ] Every new file has a module-level docstring
      [ ] Every new class has a class-level docstring
      [ ] Every new public function has a full docstring (Args / Returns / Raises)
      [ ] No hardcoded secrets or environment-specific values
      [ ] No linting errors introduced
      [ ] Import rules followed — no lazy imports, no circular deps

7.  Run existing tests — must pass (no regressions)

8.  Read ../constitution/02-coding-standards.md § Testing to confirm test file naming and coverage
    requirements, then write unit tests:

    > If using [Vertical Slice TDD](../methods/01-vertical-slice-tdd.md), the test-writing step is folded into the RED phase of each RED-GREEN-REFACTOR cycle. Verify that the micro-cycle has produced sufficient coverage instead.
      [ ] Normal cases covered
      [ ] Edge cases covered
      [ ] Error / exception cases covered

9. Run new tests — all must pass before continuing

10. Update tasks.md:
      a. Set status to `done`
      b. Write implementation summary in Notes (include any deviation from plan.md)

11. Pre-commit check:
      [ ] All tests pass (existing + new)
      [ ] No linting errors introduced
      [ ] No hardcoded secrets or environment-specific values
      [ ] Docstrings complete on all new public items
      [ ] Unnecessary files (.pyc, .DS_Store, logs) are not staged
      [ ] On the correct feature branch (not main)
      [ ] Commit message follows Google style: ≤72 chars, imperative, English

12. Commit following the [Git Workflow](../constitution/03-git-workflow.md):
      git add <specific files>
      git commit -m "[NNN] T-XXX <type>: <imperative summary ≤ 72 chars>"

13. If interactive mode: show a task summary and ask before proceeding:
      "✅ T-XXX complete. Summary: <what was done>. Continue to the next task?
      (reply 'continue' or tell me what to adjust)"
```

**Mandatory flow: check branch → review docs → implement (with deviation check) → verify → test → update tasks.md → pre-commit check → commit → (interactive pause)**

> After task 12 (commit), the loop repeats from step 0 for the next task.
> If the branch was already created in a previous task's step 0, step 0 will confirm it exists and do nothing.

---

## STOP — Deviation Protocol

> **The most common process violation is silently fixing problems instead of logging them. Read this section before you start coding — when a deviation happens mid-implementation, you won't have time to read it then.**

**This section is the authority for handling execution deviations.** The state machine and decision matrix in [01-round-mechanism.md](01-round-mechanism.md) tell you *which* state to transition to; this section tells you *how* to handle the deviation itself.

**Rule: never silently modify plan.md, add files not in plan.md, or change interfaces without going through this protocol.**

When a deviation is discovered during execution (plan doesn't match reality, new bug found, scope creep), follow the protocol below instead of silently modifying course. Deviations are logged to `issues.md` and resolved either within the current round or deferred to Round N+1.

### Severity Levels
| Severity | When | Action |
|----------|------|--------|
| **Low** | Implementation detail differs (variable naming, helper extraction, minor refactor) | Adjust directly. Note deviation in commit message. No round impact. |
| **Medium** | Bug in unrelated code, need small new tool/class not in plan | Log to `issues.md` (open). Continue current task. |
| **High** | `plan.md` module design infeasible, interface contract must change, core dependency incompatible | **STOP** current task at clean checkpoint. Log to `issues.md` (open). Set task to `blocked` if it cannot continue. Present options to user. |

### High Severity Protocol (complete)
1. **STOP** — do not continue writing code until decision is made
2. **Record** — write to both `issues.md` and `generated/rounds/round-[NNN]/issues.md` with type, severity, description, evidence, suggested fix
3. **Generate options** — use the template below for the deviation's type. Always include your recommendation.

   **Template: plan-deviation** (plan.md design infeasible):
   ```
   Option A: Update plan.md with revised design, fix within current round
       → Use when: deviation scope is contained, fix doesn't ripple across modules
       → Cost: restart current task, update plan.md, adjust tasks.md
   Option B: Log issue, defer to Round N+1 (recommended)
       → Use when: deviation requires significant design or cross-module changes
       → Cost: current task stays blocked; rest of round continues
   Option C: Rollback to last checkpoint, restart task with different approach
       → Use when: current approach is fundamentally wrong and no clear path forward
       → Cost: lost work on current task, but clean slate for replanning
   ```

   **Template: discovered-bug** (bug in unrelated code):
   ```
   Option A: Log to TODO.md, continue current task (recommended)
       → Use when: bug doesn't affect current task or requirement scope
       → Cost: none to current round; bug tracked cross-requirement
   Option B: Fix in current round — add fix task to tasks.md
       → Use when: bug blocks current task or is trivially small
       → Cost: extends current round by 1-2 tasks
   ```

   **Template: scope-creep** (work discovered that wasn't planned):
   ```
   Option A: Add to current round's tasks.md (recommended)
       → Use when: work is small (< 1 task) and user has confirmed priority
       → Cost: extends current round slightly
   Option B: Log to issues.md, plan for Round N+1
       → Use when: work is significant or user hasn't confirmed priority
       → Cost: deferred, no impact on current round
   ```

4. **Present to user** — show the finding, options, and your recommendation. Format:
   ```
   ⚠ [deviation-type] — [one-line summary]

   **Found in:** T-XXX — [task name]
   **Description:** [what was discovered]

   **Options:**
   1. Option A: [title] (recommended) — [one-line rationale]
   2. Option B: [title] — [one-line rationale]
   3. Option C: [title] — [one-line rationale]

   Please choose, or suggest another approach.
   ```
5. **Wait for decision** — user picks one; if no response after 5 min, execute recommended option as default
6. **Execute** — implement the decision, update `tasks.md` / `plan.md` if needed

### Mid-Round New Tasks
If a small new task is needed within the current round (user confirms, or low severity):

1. **Stop** current task at a clean checkpoint
2. **Add the new task** to `tasks.md` (status: `not-started`, with Notes explaining why)
3. **Resume** the current task or switch to the new based on dependency order

Do not execute an unplanned task without first recording it in `tasks.md`.

### Issues vs TODO.md
- `issues.md` — execution problems within the current requirement that affect plan/task fidelity
- `.dev/TODO.md` — cross-requirement out-of-scope discoveries (bugs, features, improvements)

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

## Requirement Complete — Round End
When all tasks in `tasks.md` reach `done` (or all runnable tasks are `blocked`):

1. **Finalize round issues** — update `generated/rounds/round-NNN/issues.md`: set status of all entries to their final state (resolved / open / wontfix). Open entries remain in `issues.md` for cross-round tracking.

2. **Check `issues.md`** — read all open issues and summarize for the user

3. **Write Round Summary** — update `start-and-resume.md § Round History`:
   ```
   ### Round [N] (complete)
   - **Status:** ✅ done (or ⏸ blocked)
   - **Tasks:** X planned, Y completed, Z deferred
   - **New issues:** ISS-NNN, ISS-MMM
   - **Summary:** What this round achieved.
   ```

4. **Update `.dev/blueprint.md`** — set Round, Phase, Status. Read [../phases/05-blueprint-management.md](../phases/05-blueprint-management.md).

5. **Method Selection — Dual-Axis Review:** Before declaring Round Complete, evaluate whether this method applies.

    **Triggers Present?** Is this a significant round with substantial new code (not XS/S)?
    - **Yes**: Apply [Dual-Axis Review](../methods/02-dual-axis-review.md). Spawn two parallel reviews (standards axis + spec axis). If both pass, proceed. If either flags issues, follow the reconciliation process in the method doc.
    - **No**: Record justification. The standard self-check checklist (step 12) suffices for trivial rounds.

6. **If no open issues remain:** declare requirement complete, create PR/merge (step 8), and notify:
   > ✅ All tasks complete for requirement [NNN]-[req-name].
   > Summary: X tasks completed, Y deviations from plan recorded in tasks.md.
   > No open issues. Development complete.

7. **If open issues remain,** notify the user with:
   > ✅ Round N complete for requirement [NNN]-[req-name].
   > X/Y tasks done. Open issues: ISS-NNN, ISS-MMM.
   >
   > **Start Round N+1?** I'll re-read issues.md, update plan.md and tasks.md incrementally,
   > and resolve the open issues in a new execution round.
   > Or reply **no** to finish for now.

   If user confirms Round N+1, proceed to Phase 01* (see [01-round-mechanism.md](01-round-mechanism.md) § Phase 01* — Round N+1 Re-entry Flow).

8. **Create a pull request or local merge** using the format in [§ Pull Request / Local Merge](#pull-request--local-merge). For PRs: `gh pr create --title "<type>(<scope>): <summary>" --body "$(cat <<'EOF'\n...\nEOF\n)"`. For local merge: `git checkout main && git merge --no-ff <type>/[NNN]-[req-name] -m "$(cat <<'EOF'\n<type>(<scope>): <imperative summary>\n\n## Summary\n...\nEOF\n)"`

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
See [../constitution/03-git-workflow.md](../constitution/03-git-workflow.md) — all git rules live there (branch naming, commit format, pre-commit checks, PR/local merge, release tagging). This section contains only the execution-loop-specific reminders.

### In the Execution Loop
Step 11 of the execution loop commits following the format defined in [../constitution/03-git-workflow.md § Commit Messages](../constitution/03-git-workflow.md#commit-messages):
```
git commit -m "[NNN] T-XXX <type>: <imperative summary ≤ 72 chars>"
```

### At Requirement Complete
Step 4 of [§ Requirement Complete](#requirement-complete) creates a PR or local merge using the format in [../constitution/03-git-workflow.md § Pull Request / Local Merge](../constitution/03-git-workflow.md#pull-request--local-merge).

---

## File Reading and Writing Discipline
- Read files in sections if large — never load an entire large file at once
- Write one file at a time; do not exceed ~200 lines in a single write
- Do not rewrite an entire file when only a small change is needed — use targeted edits
