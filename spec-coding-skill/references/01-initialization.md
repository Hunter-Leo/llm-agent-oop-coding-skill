# Phase 01 — Initialization

Create the requirement definition document before any planning or coding begins.

## Step 0 — Requirement Clarity Check

Before starting Pre-flight Checks, assess whether the requirement is clear enough to write `init.md` directly.

**Step 0a — Scan available clarification skills in real time:**
- Scan `~/.claude/skills/` and plugin cache directories (e.g. `~/.claude/plugins/cache/`)
- Identify skills whose `description` mentions: brainstorming, requirements gathering, interview, spec, or design exploration
- Build a candidate list with their names and one-line descriptions

**Step 0b — Assess requirement clarity:**

Clear signals (any one is sufficient — proceed directly to Pre-flight Checks):
- User provided specific functional description, file paths, or technical approach
- User already completed a clarification step and provided a spec/output doc path

Vague signals (recommend a clarification skill):
- Requirement is a single sentence with no technical detail
- User explicitly says "I haven't figured it out yet" or "I have a vague idea"

**Step 0c — If vague**, match the best-fit skill from the scanned list and recommend it:

| Situation | Typical best-fit skill |
|---|---|
| Has direction, needs design exploration | `superpowers:brainstorming` (or equivalent found in scan) |
| Very vague, many hidden assumptions, risk of misalignment | `oh-my-claudecode:deep-interview` (or equivalent found in scan) |

Inform the user:
> "The requirement is still vague. Based on available skills, I recommend `<skill-name>` because `<one-line reason>`.
> After completing it, share the output doc path and I'll reference it in `init.md`'s `# Spec` section.
> Or reply **skip** to proceed directly."

Wait for user confirmation before continuing.

**Step 0d — If clear, or user chooses to skip:** proceed directly to Pre-flight Checks below.

> Note: This is a suggestion, not a hard gate. The output doc is referenced (path recorded in `init.md`), not merged.

---

## Pre-flight Checks

Before creating `init.md`, complete these two steps (each asked only once per requirement):

1. **Language** — detect the user's communication language and ask if needed (see [00-agent-execution.md](00-agent-execution.md) § Language)
2. **Interactive mode** — ask the user whether to proceed interactively or automatically (see [00-agent-execution.md](00-agent-execution.md) § Interactive Mode)

## Step 2 — Skill & Agent Discovery

Before writing the `# Action Items` section of `init.md`, scan for available tools that could help complete this requirement:

1. **Scan available skills in real time:**
   - Scan `~/.claude/skills/` and plugin cache directories
   - Read each skill's `description` field
   - Match against the current requirement type (UI, data analysis, security, testing, etc.)

2. **Scan available OMC agent types in real time:**
   - Read the OMC agents directory (e.g. `~/.claude/plugins/cache/omc/.../agents/`)
   - Identify agents relevant to the requirement (e.g. `designer` for UI work, `scientist` for data analysis, `security-reviewer` for auth/security)

3. **Add matched skills and agents as optional Prerequisite entries in `# Action Items`:**
   ```
   **Optional tools discovered** (use if relevant):
   - [ ] Use `<skill-name>` to produce `<specific output>` — <one-line reason>
   - [ ] Delegate to `<omc-agent-type>` agent for `<specific subtask>` — <one-line reason>
   ```

4. **Inform the user** which skills/agents were found and briefly explain why they may help.

> Rules: Scan in real time — never hardcode skill or agent names. Only surface tools with clear relevance to the current requirement. All entries are optional.

---

## Output

`.dev/[NNN]-[req-name]/init.md`

Determine `NNN` by listing `.dev/` and using the next available three-digit number (001, 002, …).

## Requirement Name Convention

- Lowercase letters and hyphens only
- Concise and descriptive: `api-analysis`, `code-refactor`, `user-auth`

## init.md Structure

### # Project Stage

Declare the current stage of the project. This affects whether breaking changes are permitted during execution.

```
project_stage: pre-launch   # or: live
```

- `pre-launch`: MVP has not yet shipped. Breaking changes to existing structure are permitted when adding new modules, in order to keep the overall architecture clean and unambiguous.
- `live`: Product is in production. All changes must be backward-compatible; breaking changes require explicit user approval.

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

> Complete all checked prerequisite documents before proceeding to the required documents below.

**Required documents** (always, in order):
- [ ] `generated/plan.md`              — Phase 04
- [ ] `generated/tasks.md`             — Phase 05
- [ ] `generated/start-and-resume.md`  — Phase 06 (must exist before any task execution)
```

All generated documents go under `.dev/[NNN]-[req-name]/generated/`.

### # Constitution

Coding standards for this specific requirement. First read the following documents, then copy and trim the relevant sections:

- [07-oop-principles.md](07-oop-principles.md) — OOP & SOLID principles
- [08-coding-standards.md](08-coding-standards.md) — language-specific standards

Keep only the languages and rules that apply to this requirement.

## After Creating init.md

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

> ✅ `init.md` created at `.dev/[NNN]-[req-name]/init.md`
>
> Review the document and either edit it directly or tell me what to change.
> Reply **yes** or **continue** to begin executing the Action Items.
