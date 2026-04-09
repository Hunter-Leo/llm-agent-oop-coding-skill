# spec-coding-skill Integration Design

**Date:** 2026-04-09
**Branch:** feat/integrate-brainstorming-omc

## Goal

Integrate `superpowers:brainstorming` and `oh-my-claudecode` (OMC) execution modes into `spec-coding-skill` via two thin routing nodes, without changing the core document format or any of the 6 existing reference files.

## Background

Current workflow:

```
superpowers:brainstorming  →  (manual sync)  →  spec-coding-skill  →  (manual handoff)  →  /team
```

Three friction points:
1. brainstorming spec doc and init.md are two separate documents requiring manual sync
2. tasks.md is designed for single-agent sequential execution; /team ignores it and re-decomposes tasks
3. start-and-resume.md (Constitution + coding standards) is not passed to team workers

## Design Principles

- **Thin routing nodes** — routing logic is 10–15 lines of instructions, not embedded business logic
- **External tool independence** — spec-coding-skill never embeds the internal logic of brainstorming or OMC; it only calls them at boundaries
- **Core documents unchanged** — init.md, tasks.md, start-and-resume.md formats are fully preserved
- **Upgrade resilience** — when brainstorming or OMC upgrades, only the corresponding routing node needs updating (one line at most)

## Architecture

```
[superpowers:brainstorming]
         ↓
  Phase 01 — Routing Node (new, ~10 lines in 01-initialization.md)
         ↓
    init.md  (format unchanged)
         ↓
  Phase 02–06  (all reference files unchanged)
         ↓
    tasks.md  (format unchanged)
         ↓
  Phase 07 — Routing Node (new, ~15 lines in 06-start-and-resume.md)
         ↓
[OMC execution mode: /team / /ralph / /ultrawork / etc.]
```

**Files changed:** `01-initialization.md`, `06-start-and-resume.md`
**Files unchanged:** `00-agent-execution.md`, `02-prerequisite-tasks.md`, `03-algorithm-design.md`, `04-implementation-plan.md`, `05-task-planning.md`, `07-oop-principles.md`, `08-coding-standards.md`

---

## Phase 01 — Routing Node

**Location:** `spec-coding-skill/references/01-initialization.md`, before the existing Pre-flight Checks section.

**Behavior:**

Add a `## Step 0 — Requirement Clarity Check` section:

1. Assess whether the requirement is clear enough to write init.md directly.

   Clear signals (any one is sufficient — proceed to Pre-flight Checks):
   - User provided specific functional description, file paths, or technical approach
   - User already completed brainstorming and provided a spec doc path

   Vague signals (suggest brainstorming first):
   - Requirement is a single sentence with no technical detail
   - User explicitly says "I haven't figured it out yet"

2. If vague: inform the user —
   > "The requirement is still vague. I recommend clarifying it first with `superpowers:brainstorming`.
   > After brainstorming, share the spec doc path and I'll reference it in init.md's `# Spec` section."
   > Wait for user confirmation before continuing.

3. If clear, or user chooses to skip: proceed directly to Pre-flight Checks (existing flow unchanged).

**Key decisions:**
- This is a **suggestion**, not a hard gate — user can skip
- The spec doc is **referenced** (path recorded in init.md), not merged — two documents remain independent
- When brainstorming upgrades, only the skill name in this step needs updating

---

## Phase 07 — Routing Node

**Location:** `spec-coding-skill/references/06-start-and-resume.md`, before the existing Execution Loop section.

**Behavior:**

Add a `## Step 0.5 — Execution Mode Recommendation` section:

1. **Read OMC execution skills in real time**
   - Scan `~/.claude/skills/` and OMC plugin cache directory
   - Find all Tier 4 execution-class skills
   - Read each skill's `Use_When` / `Do_Not_Use_When` descriptions

2. **Analyze tasks.md**
   - Total task count, type distribution (feature / test / UI / architecture / etc.)
   - Dependency structure (sequential chain vs. parallelizable)

3. **Match against Use_When conditions** and recommend the best-fit execution mode to the user

4. **If a multi-agent mode is recommended (e.g., /team):**

   a. Read OMC agents directory in real time to get available role types

   b. Map tasks.md task types to roles; naming convention:
      ```
      <omc-role-type>-<index>
      ```
      Examples: `executor-1`, `executor-2`, `designer-1`, `test-engineer-1`
      Forbidden: `worker-1`, `agent-2`, or any non-semantic names

   c. Present the team configuration to the user:
      > Based on tasks.md analysis (N tasks: X feature + Y test + Z UI), I recommend /team:
      > - executor-1, executor-2 — T-001 ~ T-004
      > - test-engineer-1 — T-005 ~ T-006
      > - designer-1 — T-007
      >
      > Each worker will read `start-and-resume.md` on startup for Constitution and coding standards.
      >
      > Confirm this configuration, or tell me what to adjust?

   d. After user confirmation, agent directly invokes the chosen execution mode,
      passing the team configuration and `start-and-resume.md` path as worker context.

**Key decisions:**
- Agent role types are read from OMC in real time — never hardcoded in spec-coding-skill
- spec-coding-skill does not directly call /team; it invokes the execution mode after user confirmation
- `start-and-resume.md` path is always included so Constitution and coding standards are passed to workers
- When OMC adds new execution modes, this step automatically picks them up on next run

---

## Out of Scope

- Changing init.md, tasks.md, or start-and-resume.md document formats
- Changing Phase 02–06 reference files
- Embedding OMC or brainstorming internal logic into spec-coding-skill
- Supporting Codex / Gemini CLI workers (OMC handles this internally)
