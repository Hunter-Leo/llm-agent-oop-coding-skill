# Agent Execution Requirements

These rules govern how the agent must behave throughout **all phases** of this skill. They take precedence over any phase-specific instructions.

---

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
