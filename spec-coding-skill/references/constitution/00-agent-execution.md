# Agent Execution Requirements

> ## STOP. Read this file before doing anything else.
>
> These rules override all phase-specific instructions. **If you skip this file, you will violate mandatory process rules.** The most commonly violated rules are summarized below — read each section in full before acting.

## Quick-Reference: Rules You Must Not Break

| # | Rule | Section |
|---|------|---------|
| 1 | Run Task Sizing before any work — announce size to user | [§ Task Sizing](#task-sizing) |
| 2 | Re-read the phase doc before entering ANY phase | [§ Phase Transition Re-read Discipline](#phase-transition-re-read-discipline) |
| 3 | Run Method Selection at every phase entry — justify skips with specific facts | [§ Phase Transition Re-read Discipline](#phase-transition-re-read-discipline) |
| 4 | Self-Check after each task: tests pass, no regressions, no secrets | [§ Self-Check After Each Task](#self-check-after-each-task) |
| 5 | When plan != reality: follow Deviation Protocol, do NOT silently fix | [§ Handling Uncertainty & Blockers](#handling-uncertainty--blockers) |
| 6 | Update tasks.md status immediately on start and completion | [§ Document Update Discipline](#document-update-discipline) |
| 7 | Out-of-scope discovery → log to TODO.md, do NOT act on it | [§ Out-of-Scope Discovery](#out-of-scope-discovery--todomd) |

---

These rules govern how the agent must behave throughout **all phases** of this skill. They take precedence over any phase-specific instructions.

---

## Task Sizing

At the start of every new requirement, first check whether the requirement is clear enough to size at all. Then assess size.

### Clarity Gate (applies to all task sizes)

Before sizing, check:

**Clear signals** (proceed to sizing):
- User provided specific functional description, file paths, or technical approach
- Requirement is concrete enough to estimate files affected

**Vague signals** (pause and ask instead of proceeding):
- Requirement is a single sentence with no technical detail: *"I want something like Twitter but better"*
- User says "I haven't figured it out yet" or has a "vague idea"
- You cannot estimate how many files the change requires

If vague, tell the user:

> "This requirement sounds vague. I'd recommend clarifying it before I start working. Could you share more detail about what you're looking for? Or if you'd like, I can suggest a structured exploration approach."

Then wait for a clearer description before proceeding to sizing.

### Sizing

Once the requirement is clear enough, ask:

> "How many files will this change? Is the requirement well-defined or exploratory?"

| Size | Criteria | Workflow |
|------|----------|----------|
| **XS** | 1 file, trivial change (typo, rename, default param) | Brief `init.md` (inline plan only), user review, implement, test. |
| **S** | 1–3 files, well-defined, no design ambiguity | Lightweight `init.md` (Spec + Requirements + Plan), user review, implement, test. |
| **M** | Multiple modules, moderate complexity | Full Phase 01–07 flow as documented. |
| **L / XL** | Cross-requirement, multiple dependencies, high uncertainty | Full flow + Blueprint layer. Requires Phase 02 (research/inspect) before planning. |

**Bug fix sizing:** Non-trivial bugs (intermittent, root cause unknown, cross-module) default to **M** — root cause ambiguity satisfies the "moderate complexity" criterion and ensures Structured Debugging is reachable via Phase 02. Trivial one-line fixes with immediately visible root causes may still size as XS/S.

**Every size must produce a reviewable plan before implementation starts.** The plan lives in `init.md` — its depth scales with task size, but the principle is the same: the person who raised the requirement gets to review and approve the approach before any code is written.

| Size | Plan location | Plan content | Review trigger |
|------|-------------|-------------|----------------|
| XS | Inline in `init.md` | 1-3 bullet points: what changes, which files, any risks | "Here's the plan — sound good?" |
| S | Inline in `init.md` | §Plan section: files to add/modify, approach, test strategy | "init.md ready for review — OK to proceed?" |
| M | Separate `plan.md` | Full plan: project structure, tech decisions, implementation path | Phase 04 Design Compliance Review |
| L/XL | Separate `plan.md` + prereq docs | Full plan + research/inspect findings | Phase 02 + Phase 04 review |

**Example — XS init.md inline plan:**
```markdown
# Plan — send_email retry

## Changes
- Add `retry_count: int = 0` param to `send_email()` in `utils/email.py`
- Wrap SMTP call in retry loop, retry only on SMTPException
- Add unit tests for retry success, exhaustion, and no-retry paths
```

**XS and S tasks skip the standard phase flow** (no plan.md, tasks.md, start-and-resume.md), but they still follow the [Git Workflow](03-git-workflow.md). The full rules are in `03-git-workflow.md` — the essentials for XS/S:

- Create a branch first (never commit to main): `<type>/<short-description>` (e.g. `fix/email-retry-param`)
- Commit format (Google style): `<type>: <capitalized imperative summary, ≤ 72 chars>`
- Body explains why and what, not how. Wrap at 72 chars. In English.
- Pre-commit: tests pass, no lint errors, no secrets, docstrings complete
- After merge: ask user before deleting the branch

For XS: read the file, make the change, verify, commit.
For S: create a lightweight `init.md`, implement, test, commit.

**For both XS and S: ensure the feature branch is created before writing any code:**
```bash
git checkout -b <type>/<short-description>
```
Never commit to main.

**However, XS and S code must still comply with all standards.** Task size only affects document ceremony — code quality is not negotiable:

- **OOP & SOLID principles** apply to every line of code, regardless of task size ([01-oop-principles.md](01-oop-principles.md))
- **Coding standards** (type annotations, docstrings, naming, error handling) apply to all changes ([02-coding-standards.md](02-coding-standards.md))
- **Self-check checklist** from [00-agent-execution.md](00-agent-execution.md#self-check-after-each-task) applies: tests, no regressions, no hardcoded secrets, no lint errors
- **Comments and docstrings** are required for new public functions, regardless of task size

> An XS task still needs a docstring on the modified function and tests for the new behavior. Sizing is about how many documents you write before coding, not about how thoroughly you code.

Record the task in `.dev/TODO.md` if out of scope.

This sizing replaces the one-size-fits-all approach. When in doubt between sizes, default to the **lighter** workflow — it is easier to add ceremony than to undo over-engineering.

### Communicating the Sizing Decision

After sizing, **tell the user** your size decision and what to expect:

- **XS**: `"This is XS — I'll draft a quick plan in init.md for your review, then implement."`
- **S**: `"This is S — I'll put together init.md with spec and plan for you to review, then implement."`
- **M**: `"This involves multiple modules (M). I'll proceed with the full Phase 01–07 flow and you'll review at each phase."`
- **L/XL**: `"This has cross-cutting concerns (L). I'll start with research and share findings before planning."`

After announcing the size, ask briefly: `"Sound good?"` and proceed unless the user objects. This gives the user a chance to correct the sizing without requiring a formal confirmation step.

## User Interaction

### Language

At the start of Phase 01, detect the language the user is communicating in:

- If the user is writing in **English**: use English for all `.dev` documents (default).
- If the user is writing in a **non-English language**: ask once:

  > The `.dev` documents will be written in English by default. Would you like me to use [detected language] instead?

  Use whichever language the user confirms. Apply it consistently to all generated documents for this requirement.

### Interactive Mode

At the start of Phase 01, ask the user once:

> Would you like to proceed interactively? In interactive mode, I'll pause after each phase and after each task during execution for your review.
> Reply **yes** for interactive mode, or **no** to run all phases automatically.

**Interactive mode behavior:**
- Phases 01–06: pause after completing each phase or document, show a summary and ask:
  > ✅ [Phase/document name] complete. Review the output and reply **continue** to proceed, or tell me what to adjust.
- Phase 07 (Execution loop): pause after each task reaches `done`, show a summary and ask:
  > ✅ T-XXX complete. Summary: <what was done>. Continue to the next task? (reply 'continue' or tell me what to adjust)

**Automatic mode behavior:** proceed through all phases without pausing, unless a blocker or ambiguity requires user input.

### Progress Display

At the start of execution and before each phase, display the current progress:

```
[NNN] req-name — Progress
──────────────────────────────
✅ Phase 01 — Initialization
✅ Phase 02 — Prerequisite Tasks
▶  Phase 04 — Implementation Plan   ← now
   Phase 05 — Task Planning
   Phase 06 — Create start-and-resume.md
   Phase 07 — Execution
──────────────────────────────
```

- `✅` = completed
- `▶` = in progress
- (blank) = not started
- Omit optional phases that were skipped

---

## File Reading Discipline

- Read existing files before modifying them — never edit blind
- Read no more than **3–5 files per action** — if more context is needed, read in batches across multiple steps
- For large files, read in sections (e.g. lines 1–100, then 101–200) — never load an entire large file at once to avoid context overflow errors
- When analyzing existing code, read the most relevant file first, then expand outward as needed
- Do not load the entire codebase upfront — load files on demand as each task requires them

---

## File Writing Discipline

- Write **one file at a time**
- For large files, write in logical sections across multiple operations — do not create a file exceeding **~200 lines in a single write**
- Do not rewrite an entire file when only a small change is needed — use targeted edits
- After writing, verify the output is consistent with the plan and constitution before moving on

---

## Design Principle Compliance

All design and code produced across every phase must follow:

- **OOP principles** — encapsulation, abstraction, inheritance, polymorphism (see [01-oop-principles.md](01-oop-principles.md))
- **SOLID principles** — SRP, OCP, LSP, ISP, DIP (see [01-oop-principles.md](01-oop-principles.md))

These apply from Phase 01 onward. The Constitution section in `init.md` (Phase 01) is the first enforcement point — do not defer OOP/SOLID thinking to Phase 04.

---

## Self-Check After Each Task

Before marking a task `done`, verify:

- [ ] Reviewed `plan.md` and relevant docs before writing any code
- [ ] Code follows the Constitution defined in `init.md`
- [ ] All new public functions, classes, and files have complete documentation comments
- [ ] Existing tests still pass (no regressions)
- [ ] New unit tests written and cover: normal cases, edge cases, error/exception cases
- [ ] All tests pass — do not proceed to the next task if any test fails
- [ ] No hardcoded secrets or environment-specific values
- [ ] No new linting errors introduced

---

## Handling Uncertainty & Blockers

| Situation | Action |
|---|---|
| Requirement is ambiguous | Ask the user before writing any code |
| Tech choice has multiple valid options | Present options with trade-offs, ask the user to decide |
| A task cannot proceed due to a dependency or error | Mark as `blocked`, record reason in `tasks.md` Notes, ask the user |
| Existing code contradicts the plan | Follow § Deviation Protocol in 00-start-and-resume.md — assess severity, log to issues.md, present options to user |
| Discovery of unplanned work during execution | Assess severity: low → adjust silently; medium → log to issues.md, continue; high → trigger Deviation Protocol, present options |

---

## Phase Transition Re-read Discipline

Before entering any phase, re-read that phase's reference file. After re-reading, run the **Method Selection** step documented in that phase's reference to evaluate which extended methods apply.

| Phase | Reference to Re-read |
|-------|---------------------|
| Phase 01 (first round) | `00-initialization.md` |
| Phase 01* (round N+1 re-entry) | `00-initialization.md` + `01-round-mechanism.md` § Phase 01* |
| Phase 02 | `01-prerequisite-tasks.md` |
| Phase 03 | `02-algorithm-design.md` |
| Phase 04 | `03-implementation-plan.md` |
| Phase 05 | `04-task-planning.md` |
| Phase 06 | `00-start-and-resume.md` § Step 0 |
| Phase 07 | **Must** re-read `00-start-and-resume.md` + `01-round-mechanism.md` |
| Round Complete | `01-round-mechanism.md` § 10 Round Complete Checklist |

This ensures the agent always has the current execution rules in context before acting.

---

## Document Update Discipline

- Update `tasks.md` status **immediately** when a task starts (`in-progress`) and when it completes (`done`)
- Do not defer status updates to the end of a session
- Record any notable decisions or issues in the task's Notes field while context is fresh

---

## Out-of-Scope Discovery — TODO.md

During execution, you will sometimes notice bugs or potential improvements that are **outside the current requirement's scope**. When this happens:

1. **Do not act on it** — stay focused on the current task
2. **Immediately append** an entry to `.dev/TODO.md`
3. **Continue** with the current task

### TODO.md entry format

**Backlog table row:**

```
| TODO-NNN | <type> | <priority> | <one-line summary> | [NNN] T-XXX | pending |
```

**Detail block:**

```markdown
### TODO-NNN — <one-line summary>

**Type:** bug / feature / improvement / tech-debt
**Priority:** high / medium / low
**Source:** [NNN] T-XXX
**Status:** pending

**Description:**
What was observed and why it matters.

**Location:**
File or module where the issue was found.

**Notes:**
Any additional context or suggested approach.
```

**Type values:** `bug` · `feature` · `improvement` · `tech-debt`
**Priority values:** `high` · `medium` · `low`
**Status values:** `pending` · `planned` (a new `init.md` has been created) · `ignored`

---

## Handling Project Overview Queries

When the user asks for a project-level overview — progress summary, status check, bird's-eye view, "where are we", or "what's next":

1. **Read `.dev/blueprint.md`** — this is the single source of truth for project-wide status
2. **For each requirement in `▶ in-progress` status**, also read its `tasks.md` for task-level detail
3. **For each requirement in `⏸ blocked`**, read the `tasks.md` Notes and `init.md` to explain the blocker
4. **Check `.dev/TODO.md`** for unresolved backlog items that may impact progress
5. **Present** a structured summary:

   ```
   ## Project Status
   
   Blueprint: N requirements | ✅ Done: X | ▶ Active: Y | ⏳ Idle: Z
   Backlog:   TODO.md has W unresolved items
   
   | Req | Phase | Status | Priority | Dependencies | Next Action |
   |-----|-------|--------|----------|--------------|-------------|
   | 001 user-auth | 07 Execution | ▶ in-progress | P0 | - | T-003: implement login |
   | 002 task-crud | 04 Plan | ⏳ pending | P1 | 001 | ready to start Phase 05 |
   
   **Recommended focus**: 001-user-auth (highest priority, actively in progress)
   ```

6. If the user then asks about a specific requirement, follow the normal per-requirement flow from the relevant phase document.

Do not answer a project overview query by describing only one requirement — always read the blueprint first. If the blueprint is missing, scan `.dev/` directories for `init.md` files and reconstruct it.

---

## Round-based Execution Model

The skill uses a round-based execution model. A round is one full pass through the spec-driven flow:

```
Round 1: Phase 01-05 -> Phase 06 -> Phase 07 -> Review -> (done or next round)
Round N+1: Phase 01* (incremental) -> Phase 02-05 -> Phase 06 -> Phase 07 -> Review -> ... -> DONE
```

### When Multiple Rounds Apply

- Round 1: full create from scratch
- Round N+1: triggered when `issues.md` has open issues after a round completes

### Round Trigger Summary

| Situation | Action |
|-----------|--------|
| First time working on this requirement | Enter Phase 01 as Round 1 |
| Phase 07 tasks all done, issues.md has open issues | Offer user: "Start Round N+1?" |
| User confirms Round N+1 | Enter Phase 01* — read issues.md, incrementally update plan/tasks |
| Phase 07 tasks all done, no open issues | Declare requirement complete |
| All tasks blocked, blocker depends on another requirement | Mark round blocked; re-enter when blocker resolved |

See [01-round-mechanism.md](../execution/01-round-mechanism.md) for the full state machine, decision matrix, and issues.md format.
