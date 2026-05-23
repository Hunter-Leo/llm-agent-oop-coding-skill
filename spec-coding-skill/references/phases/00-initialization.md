# Phase 01 — Initialization

Create the requirement definition document before any planning or coding begins.

**Entry:** A need or requirement has been identified, sized M or larger via [Task Sizing](../constitution/00-agent-execution.md#task-sizing).
**Exit:** `init.md` created at `.dev/[NNN]-[req-name]/init.md`, `.dev/blueprint.md` updated with the new requirement.

**Round check:** Before starting, check if `.dev/[NNN]-[req-name]/issues.md` exists. If **yes** (open issues): this is Round N+1 re-entry — read [01-round-mechanism.md](../execution/01-round-mechanism.md) § Phase 01* first, then update init.md based on open issues.

**XS/S sizing:** complete Pre-flight Checks below, then produce lightweight init.md with inline plan (no separate plan.md/tasks.md). See [Task Sizing](../constitution/00-agent-execution.md#task-sizing) for the direct execution flow.

## Step 0 — Requirement Clarity Check

Before Pre-flight Checks, assess requirement clarity:

| Signal | Condition | Action |
|--------|-----------|--------|
| Clear | Specific description, file paths, or spec doc provided | Proceed to Pre-flight Checks |
| Vague | Single sentence, no detail, "vague idea" | Recommend clarification skill. Wait for user response. |

**If vague:** scan available skills for brainstorming/requirements/interview. Recommend best match (see table below). Present user with: "The requirement is still vague. Based on available skills, I recommend `<skill>` because `<reason>`. After completing it, share the output doc path and I'll reference it in `init.md`. Or reply **skip** to proceed directly."

| Situation | Typical best-fit skill |
|-----------|----------------------|
| Has direction, needs design exploration | `superpowers:brainstorming` (or equivalent found in scan) |
| Very vague, many hidden assumptions | `oh-my-claudecode:deep-interview` (or equivalent found in scan) |

Fallback to table above if no matching skills found in scan. Always prefer discovered skills over defaults.

Wait for user confirmation before continuing. If user provides a spec doc path, record it in `init.md` § Spec.

**If clear, or user skips:** proceed to Pre-flight Checks below.

**Note:** This is a suggestion, not a hard gate. The output doc is referenced (path recorded in `init.md`), not merged.

---

## Pre-flight Checks

Before creating `init.md`, complete these two steps (each asked only once per requirement):

1. **Language** — detect the user's communication language and ask if needed (see [00-agent-execution.md](../constitution/00-agent-execution.md) § Language)
2. **Interactive mode** — ask the user whether to proceed interactively or automatically (see [00-agent-execution.md](../constitution/00-agent-execution.md) § Interactive Mode)

---

## Method Selection

Before writing `init.md`, evaluate which methods apply to this phase.

**Methods tagged for Phase 01:**
- [Term Grilling + ADR](../methods/00-term-grilling-and-adr.md) — Term alignment and ADR creation

**Evaluate:**

1. Read the method's `## When to Use` / `## Do Not Use` sections
2. Check for trigger signals in the current spec:
   - Fuzzy/ambiguous terminology (signal words: "robust", "scalable", "efficient", undefined concepts)
   - Hard-to-reverse technical decisions (tech stack, data model, interface contracts)
3. Populate the table:

```
| Method | Phase | Triggers Present? | Apply? | Skip Justification |
|--------|-------|-------------------|--------|--------------------|
| Term Grilling + ADR | 01 | Yes/No | Yes/No | (if No: specific reason) |
```

4. **If Yes**: execute the method after drafting `init.md` (see Term Grilling + ADR doc). Create `.dev/CONTEXT.md` glossary and ADRs as needed.
5. **If No**: record a specific factual justification. Generic "not needed" is invalid.
   Valid examples: "All terminology is concrete and well-defined", "No hard-to-reverse decisions in this requirement"

After Method Selection, proceed to the step below.

---

## Step 1 — Skill & Agent Discovery

Before writing the `# Action Items` section of `init.md`, check for available tools that could help complete this requirement:

1. **Check available skills from session context:**
   - Read the list of available skills provided by the system at session start (visible in the session context / system-reminder)
   - Read each skill's `description` field
   - Match against the current requirement type (UI, data analysis, security, testing, etc.)

2. **Check available OMC agent types from session context:**
   - Read the list of available agent types provided by the system at session start (visible in the session context / system-reminder)
   - Identify agents relevant to the requirement (e.g. `designer` for UI work, `scientist` for data analysis, `security-reviewer` for auth/security)

3. **Add matched skills and agents as optional Prerequisite entries in `# Action Items`:**
   ```
   **Optional tools discovered** (use if relevant):
   - [ ] Use `<skill-name>` to produce `<specific output>` — <one-line reason>
   - [ ] Delegate to `<omc-agent-type>` agent for `<specific subtask>` — <one-line reason>
   ```

4. **Inform the user** which skills/agents were found and briefly explain why they may help.

**Rules:** Read from session context — never hardcode skill or agent names. Only surface tools with clear relevance to the current requirement. All entries are optional.

---

## Output

`.dev/[NNN]-[req-name]/init.md`

Determine `NNN` by listing `.dev/` and using the next available three-digit number (001, 002, …).

## Requirement Name Convention

- Lowercase letters and hyphens only
- Concise and descriptive: `api-analysis`, `code-refactor`, `user-auth`

## init.md Structure

### # Project Stage

Declare the current stage of the project.

```
project_stage: pre-launch   # or: live
```

See `../execution/00-start-and-resume.md § Breaking Changes Policy` for how this value affects execution.

### # Spec

High-level description of the requirement:

- Background and motivation
- Core problem being solved
- Overall goal and vision
- Business value
- Usage scenarios

### # Requirements

Detailed specification:

- Core objectives
- Functional requirements
- Technical requirements
- Interface or module descriptions
- Expected outputs

### # Action Items

Ordered list of documents to produce. Use this template:

```
**Prerequisite documents** (if needed — see Phase 02 & 03):
- [ ] `generated/inspect.md` — existing code analysis
- [ ] `generated/research.md` — technology research
- [ ] `generated/profiling.md` — performance analysis
- [ ] `generated/diagnosis.md` — bug diagnosis
- [ ] `generated/algorithm-design.md` — algorithm design

Complete all checked prerequisite documents before proceeding to the required documents below.

**Round artifacts** (maintained across rounds):
- [ ] `issues.md`                     — Round-to-round issue log (created in Phase 07, read on re-entry)

**Required documents** (always, in order):
- [ ] `generated/rounds/round-001/plan.md`     — Phase 04 (Round 1). Subsequent rounds: `round-NNN/plan.md`
- [ ] `generated/rounds/round-001/tasks.md`    — Phase 05. Each round gets its own tasks.md
- [ ] `generated/start-and-resume.md`          — Phase 06 (must exist before any task execution)
```

All generated documents go under `.dev/[NNN]-[req-name]/generated/`. Requirement-level files (`init.md`, `issues.md`) live under `.dev/[NNN]-[req-name]/`. Round-specific docs (plan, tasks, issues) live under `generated/rounds/round-NNN/`.

### # Constitution

Mandatory coding standards for this requirement. Read the following documents and include applicable rules in this section of `init.md`:

- [../constitution/01-oop-principles.md](../constitution/01-oop-principles.md) — OOP & SOLID principles (**all code must comply; this section must include the relevant rules**)
- [../constitution/02-coding-standards.md](../constitution/02-coding-standards.md) — language-specific standards

Keep only the languages and rules that apply to this requirement. Document any project-specific extensions (e.g., team conventions not covered by the reference documents).

## Step 3 — Update Blueprint

After creating `init.md` and its Action Items, register this requirement in the project blueprint.

1. **Read** `.dev/blueprint.md` if it exists
2. **Create** `.dev/blueprint.md` if this is the first requirement — use the template from [05-blueprint-management.md](05-blueprint-management.md)
3. **Add a row** for this new requirement with:
   - Phase: `01 Init`
   - Status: `⏳ pending`
   - Priority: inferred from the requirement urgency (P0/P1/P2)
   - Dependencies: inferred from the requirement context
4. **Update** the Progress Summary section to reflect the new total

If `.dev/TODO.md` does not exist, create it now with the following skeleton:

```markdown
# Project TODO

Tracks out-of-scope bugs and features discovered during task execution.
Do not act on these during an active task — log and continue.

## Backlog

| ID | Type | Priority | Summary | Source | Status |
|---|---|---|---|---|---|

## Details
```

Then inform the user:

✅ `init.md` created at `.dev/[NNN]-[req-name]/init.md`

✅ Blueprint updated at `.dev/blueprint.md`

Review the documents and either edit them directly or tell me what to change.
Reply **yes** or **continue** to begin executing the Action Items.
