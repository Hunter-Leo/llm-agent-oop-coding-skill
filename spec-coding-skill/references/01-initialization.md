# Phase 01 — Initialization

Create the requirement definition document before any planning or coding begins.

## Pre-flight Checks

Before creating `init.md`, complete these two steps (each asked only once per requirement):

1. **Language** — detect the user's communication language and ask if needed (see [00-agent-execution.md](00-agent-execution.md) § Language)
2. **Interactive mode** — ask the user whether to proceed interactively or automatically (see [00-agent-execution.md](00-agent-execution.md) § Interactive Mode)

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
