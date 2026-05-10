# Agent Execution Requirements

These rules govern how the agent must behave throughout **all phases** of this skill. They take precedence over any phase-specific instructions.

---

## Task Sizing

At the start of every new requirement, assess its size before entering the phase flow. Ask:

> "How many files will this change? Is the requirement well-defined or exploratory?"

| Size | Criteria | Workflow |
|------|----------|----------|
| **XS** | 1 file, trivial change (typo, rename, default param) | Implement + test directly. No documents. |
| **S** | 1–3 files, well-defined, no design ambiguity | Create a lightweight 1-page `init.md` (only Spec + Requirements), implement, test. Skip `plan.md`, `tasks.md`, `start-and-resume.md`. |
| **M** | Multiple modules, moderate complexity | Full Phase 01–07 flow as documented. |
| **L / XL** | Cross-requirement, multiple dependencies, high uncertainty | Full flow + Blueprint layer. Requires Phase 02 (research/inspect) before planning. |

**XS and S tasks skip the standard phase flow.** For XS: read the file, make the change, verify, commit. For S: create a minimal `init.md`, implement, test, commit. Record the task in `.dev/TODO.md` if out of scope.

This sizing replaces the one-size-fits-all approach. When in doubt between sizes, default to the **lighter** workflow — it is easier to add ceremony than to undo over-engineering.

### Communicating the Sizing Decision

After sizing, **tell the user** what you decided and why:

- **XS**: `"This looks like a small change (XS). I'll go ahead and implement it directly."`
- **S**: `"This is well-defined and affects few files (S). I'll create a quick init.md and implement."`
- **M**: `"This involves multiple modules (M). I'll proceed with the full Phase 01–07 flow."`
- **L/XL**: `"This has cross-cutting concerns (L). I'll start with research before planning."`

After announcing the size, ask briefly: `"Sound good?"` and proceed unless the user objects. This gives the user a chance to correct the sizing without requiring a formal confirmation step.

> For XS and S tasks, do not ask "which language" or "interactive mode" — those questions only apply to the full phase flow. Just size, announce, and proceed.

## User Interaction

### Language

At the start of Phase 01, detect the language the user is communicating in:

- If the user is writing in **English**: use English for all `.dev` documents (default).
- If the user is writing in a **non-English language**: ask once:

  > The `.dev` documents will be written in English by default. Would you like me to use [detected language] instead?

  Use whichever language the user confirms. Apply it consistently to all generated documents for this requirement.

### Interactive Mode

At the start of Phase 01, ask the user once:

> Would you like to proceed interactively? In interactive mode, I'll pause after each phase and ask for your review before continuing.
> Reply **yes** for interactive mode, or **no** to run all phases automatically.

**Interactive mode behavior:** after completing each phase or document, show a summary and ask:

> ✅ [Phase/document name] complete. Review the output and reply **continue** to proceed, or tell me what to adjust.

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

## Thinking & Reasoning

- Think step by step before acting — do not jump to implementation without completing the required planning phases
- Process one task at a time; never batch multiple tasks in a single action
- When a requirement is ambiguous or has multiple valid interpretations, **stop and ask the user** before proceeding — do not assume
- When a technology choice is unclear, **stop and ask the user** — do not pick arbitrarily
- If a decision was made in a previous phase (e.g., in `plan.md`), follow it; do not re-decide silently

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

- **OOP principles** — encapsulation, abstraction, inheritance, polymorphism (see [07-oop-principles.md](07-oop-principles.md))
- **SOLID principles** — SRP, OCP, LSP, ISP, DIP (see [07-oop-principles.md](07-oop-principles.md))

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
| Existing code contradicts the plan | Do not silently deviate — surface the conflict and ask |

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

5. If the user then asks about a specific requirement, follow the normal per-requirement flow from the relevant phase document.

Do not answer a project overview query by describing only one requirement — always read the blueprint first. If the blueprint is missing, scan `.dev/` directories for `init.md` files and reconstruct it.
