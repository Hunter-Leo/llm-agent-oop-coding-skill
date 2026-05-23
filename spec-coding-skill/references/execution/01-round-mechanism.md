---
name: round-mechanism
description: "Round-based execution model with dual-layer state machine (requirement lifecycle + round execution), agent decision matrix, Phase 01* re-entry flow, issues.md format, and illegal state rules. Reference for the iterative round-driven development cycle. Use when entering Phase 07, resuming after a round completes, or when handling execution deviations."
---

# Round Mechanism — Round-Based Execution Model

This file defines the round-based execution model for spec-driven development. It transforms the one-shot Phase 01–07 flow into an iterative cycle where execution findings feed back into planning.

Read this when entering Phase 07 or when resuming after a Round completes.

> **Authority boundary**: This file defines the **state machine** (states, transitions, decision matrix). The **Deviation Protocol** (how to handle deviations during execution) is defined in `00-start-and-resume.md § Deviation Protocol` — that file is the authority for execution-time decisions.

## 1. Why Rounds

Without rounds, the flow is linear:
```
Plan -> Execute -> Done
               ^
               | (no feedback path — deviations handled ad-hoc)
```

With rounds, execution becomes cyclic:
```
Round N: Plan -> Execute -> Review
                              |
                              v (issues found during execution)
Round N+1: Re-Plan (based on issues) -> Execute -> Review
                                                      |
                                                      v (more issues or done)
```

Each round is a full pass through the spec-driven workflow. The output of Round N (in the form of `issues.md`) becomes the input for Round N+1's planning.

## 2. Directory Layout

```
.dev/
├── blueprint.md                           # Project-level view (includes Round column)
├── TODO.md                                # Cross-requirement backlog
│
└── 001-user-auth/
    ├── init.md                            # Requirement definition (stable; scope changes only)
    ├── issues.md                          # Round-to-round issue log (the bridge)
    └── generated/
        ├── start-and-resume.md            # ★ Shared across rounds: execution rules + Round History
        └── rounds/
            ├── round-001/
            │   ├── plan.md                # Round 1 plan
            │   └── tasks.md               # Round 1 tasks
            └── round-002/                 # Current round
                ├── plan.md
                ├── tasks.md
                └── issues.md              # Issues discovered in this round
```

## 3. State Machine Architecture

Two-layer state machine:

```
+--------------------------------------------------------------+
|  Layer 1: Requirement Lifecycle State Machine                  |
|  (Cross-round decision: "should there be another round?")     |
|                                                               |
|  INIT -> PLANNING -> EXECUTING -> REVIEW -> DONE/KILLED       |
+--------------------------------------------------------------+
                          | delegates state
                          v
+--------------------------------------------------------------+
|  Layer 2: Round Execution State Machine                        |
|  (Within-round decision: "which Phase am I in?")              |
|                                                               |
|  S01 <-> S02 <-> S03 -> S04A -> S03                          |
|                  |                                             |
|                  +-> S05 -> (back to Layer 1)                  |
+--------------------------------------------------------------+
```

---

## 4. Layer 1: Requirement Lifecycle

This layer tracks the overall lifecycle of one requirement across multiple rounds.

```
                    +----------+
                    |  INIT    |  <- init.md created
                    +----+-----+
                         |
                    +----v-----+
              +-----|PLANNING  |  <- start-and-resume.md exists
              |     +----+-----+
              |          |
              |     +----v-----+     +----------+
              |     |EXECUTING |<--->| BLOCKED  |  <- all tasks blocked
              |     +----+-----+     +----------+
              |          |
              |     +----v-----+
              |     | REVIEW   |  <- Round Summary written
              |     +----+-----+
              |          |
              |    +-----+------+
              |    |            |
              |   no          yes
              | open         open
              | issue        issue
              |    |            |
              |    v            v
              | +------+  +----------+
              | | DONE |  | PLANNING |  <- back to PLANNING, Round += 1
              | +------+  +----------+
              |
              +--- user can force re-plan at any time
```

### Layer 1 State Definitions

| State | Meaning | Entry Condition | Exit Condition |
|-------|---------|-----------------|----------------|
| INIT | Requirement defined | init.md created | plan.md + tasks.md + start-and-resume.md done |
| PLANNING | Planning/documentation phase | start-and-resume.md created (Round N) | Round execution begins |
| EXECUTING | Execution in progress | Phase 07 starts | All tasks done or all blocked |
| BLOCKED | Execution halted | All runnable tasks are blocked | At least one blocker resolved |
| REVIEW | Round complete, evaluating | Round Summary written | User decision made |
| DONE | Development complete | REVIEW with no open issues | (terminal) |
| KILLED | Cancelled | User/system abort | (terminal) |

### Layer 1 Transition Matrix

R = Row state, C = Column state. `✓` = valid transition.

| | INIT | PLANNING | EXECUTING | BLOCKED | REVIEW | DONE | KILLED |
|---|---|---|---|---|---|---|---|
| INIT | - | ✓ | | | | | ✓ |
| PLANNING | | - | ✓ | | ✓ | | ✓ |
| EXECUTING | | | - | ✓ | ✓ | | ✓ |
| BLOCKED | | | ✓ | - | ✓ | ✓ | ✓ |
| REVIEW | | ✓ | | | - | ✓ | ✓ |
| DONE | | | | | | - | |
| KILLED | | | | | | | - |

---

## 5. Layer 2: Round Execution

This layer tracks the detailed state within a single round.

```
                          +-------------------------------+
                          |  Blueprint + TODO.md          |
                          +-------------------------------+
                                        |
            +---------------------------+----------------------------+
            |  S01: INIT_ROUND                                       |
            |  Behavior: Phase 01-05                                 |
            |  Round 1: create from scratch                          |
            |  Round N+: incremental update from issues.md           |
            +---------------------------+----------------------------+
                                        | tasks.md planned
                                        v
            +---------------------------+----------------------------+
            |  S02: PREPARE_EXECUTION                                |
            |  Behavior: Phase 06                                    |
            |  Create/update start-and-resume.md                     |
            |  Set Round number                                      |
            +---------------------------+----------------------------+
                                        | start-and-resume.md ready
                                        v
            +---------------------------+----------------------------+
   +--------|  S03: EXECUTING                                       |
   |        |  Behavior: Phase 07 Execution Loop                    |
   |        |  Task by task: re-read -> implement -> test -> commit |
   |        +---------------------------+----------------------------+
   |                                    |
   |      +-----------------------------+----------+
   |      |                                        |
   |      v                                        v
   |  High severity                             Med severity
   |  deviation                                 deviation
   |      |                                        |
   |      |                                   +---+------+
   |  +---+------------+                      |Log issue |
   |  | S04A: STOPPED   |                     |Continue  |
   |  | Deviation HIGH  |                     +----------+
   |  | STOP + user     |
   |  | decision needed |    Low severity deviation -> adjust directly
   |  +---+-----+------+
   |      |     |
   |      |User |
   |  +---+--+--+-------+
   |  |      |          |
   |  Fix   Defer     Rollback
   |  now   to R+1    approach
   |  |      |          |
   |  +------+----------+
   |         |
   |    +----+----+
   |    |         |
   |  back      back
   |  S03       S01 (next round)
   +----+
        |
        | all tasks done or all blocked
        v
+---------------------------+----------------------------+
|  S05: ROUND_COMPLETE                                   |
|  Behavior: Write Round Summary                         |
|  Update blueprint + start-and-resume.md Round History |
+---------------------------+----------------------------+
                            |
              +-------------+------------+
              |                          |
         has open issue              no open issue
              |                          |
              v                          v
       Layer1 PLANNING              Layer1 DONE
       (Round +1)                   (terminal)
```

### Layer 2 Agent Decision Matrix

At every step, consult this table:

| Current State | Trigger | Decision | Next State |
|---------------|---------|----------|------------|
| First Phase 01 | New requirement | Create init.md, register in blueprint | S01 |
| Phase N document done | Document ready | Proceed to next phase | S01 (within Phase) |
| S02 complete | start-and-resume.md ready | Enter execution | S03 |
| S03 task done | Commit succeeded | Pick next task | S03 (loop) |
| S03 low severity deviation | Minor impl adjustment | Adjust, note in commit | S03 |
| S03 medium severity deviation | New bug found | Log to issues.md, continue | S03 |
| S03 high severity deviation | Plan proven infeasible | STOP, follow Deviation Protocol | S04A |
| S04A user: "fix now" | Decision received | Fix within current round | back to S03 |
| S04A user: "next round" | Decision received | Mark issue open, continue other tasks | back to S03 |
| S04A user: "rollback" | Decision received | Adjust direction | back to S03 |
| S03 all tasks done | No pending tasks | Write Round Summary | S05 |
| S05 has open issues | User confirms next round | Round += 1, start Phase 01* | Layer1 PLANNING |
| S05 no open issues | User confirms done | Mark DONE | Layer1 DONE |
| Any time | User: "skip round" | Exit immediately | KILLED / DONE |
| Any time | User: "re-plan" | Go back to Phase 01 | S01 |

---

## 6. Phase 01* — Round N+1 Re-entry Flow

When Round N completes and open issues remain, the requirement re-enters Phase 01 (called Phase 01*). Each round gets its own `plan.md` and `tasks.md` under a round-specific subdirectory.

```
Phase 01* entry
     |
     1. Read issues.md -> list all open issues
     2. Read start-and-resume.md -> confirm current round number
     3. Create generated/rounds/round-(N+1)/
     4. Read previous round's plan.md and tasks.md for context
     5. For each open issue:
        - plan-deviation: update plan in new round's plan.md
        - discovered-bug: add task to new round's tasks.md
        - scope-creep: update init.md + new round's plan.md + tasks.md
     6. Update blueprint: Round += 1
     7. Update start-and-resume.md Round History:
        - Mark Round N as complete
        - Add Round N+1 entry with "Current Round: N+1"
     8. Proceed through Phase 02-05 (incremental only)
```

**Key rules:**
- Phase 01* does NOT rewrite documents from scratch. It creates new round-specific files and only modifies what the open issues require.
- Previous round's plan.md and tasks.md are never modified — they stay as historical records under `rounds/round-N/`.
- If no open issue touches `init.md`'s scope, `init.md` is not modified.

### Round Numbering

- Round 1: first pass through Phase 01-07. plan.md and tasks.md go in `generated/rounds/round-001/`.
- Round N+1: enters Phase 01*. Creates `generated/rounds/round-(N+1)/` with new plan.md and tasks.md.
- Round counter stored in `blueprint.md` Round column and `start-and-resume.md` Round History.

---

## 7. issues.md — Dual-Layer Issue Tracking

Issues have two layers:

| Layer | File | Role | Content |
|-------|------|------|---------|
| Active | `.dev/[NNN]-[req-name]/issues.md` | Cross-round issue tracker | All **open** issues from any round |
| Snapshot | `.dev/[NNN]-[req-name]/generated/rounds/round-NNN/issues.md` | Per-round log | Issues discovered **in this round** (including resolved) |

### Writing to issues (during execution)

When a deviation is detected, write to **both** layers:

```
1. Append to .dev/[NNN]-[req-name]/issues.md (status: open)
2. Append to generated/rounds/round-NNN/issues.md (status: open)
```

### At Round Complete

Mark all entries in `rounds/round-NNN/issues.md` with their final status. Entries in `issues.md` that are still open persist to the next round.

### Active Template (`issues.md`)

```markdown
# Issues — [NNN]-[req-name]

Cross-round issue tracker. Open issues drive the next round's planning.

| ID | Round | Type | Severity | Summary | Status |
|-----|-------|------|----------|---------|--------|
| ISS-001 | 1 | plan-deviation | high | ... | open |
| ISS-002 | 2 | discovered-bug | medium | ... | resolved |

### ISS-001 — [short title]

- **Round:** NNN
- **Type:** [plan-deviation / discovered-bug / scope-creep / technical-contradiction / performance-issue]
- **Severity:** [high / medium / low]
- **Found in:** T-XXX
- **Description:** What was discovered and why it's a problem
- **Evidence:** Facts supporting the finding (test output, code analysis, etc.)
- **Suggested fix:** What should change to resolve this
- **Affected files:** [file paths if known]
- **Status:** [open / in-progress / resolved / wontfix]
```

### Round Snapshot Template (`rounds/round-NNN/issues.md`)

```markdown
# Issues — Round NNN

Issues discovered during Round NNN execution.

| ID | Type | Severity | Summary | Status |
|-----|------|----------|---------|--------|
| ISS-001 | plan-deviation | high | ... | resolved |

### ISS-001 — [short title]
```

The round snapshot is simpler (no cross-round fields). It records what happened in this specific round.

### Issue Types

| Type | Meaning |
|------|---------|
| plan-deviation | plan.md's design doesn't match reality during implementation |
| discovered-bug | Bug found in existing code (outside current task scope) |
| scope-creep | Work discovered that should be in scope but wasn't planned |
| technical-contradiction | Plan assumed something the codebase doesn't support |
| performance-issue | Implementation doesn't meet performance requirements |

### Status Values

| Status | Meaning |
|--------|---------|
| open | Not yet addressed |
| in-progress | Being addressed in current round |
| resolved | Addressed in a completed round |
| wontfix | Accepted, will not be fixed |

### Severity Guide

| Severity | When to Use | Impact on Current Round |
|----------|-------------|------------------------|
| low | Implementation detail differs from plan (variable naming, helper extraction, minor refactor) | Directly adjust, no round impact |
| medium | Bug in unrelated code, need small new class/tool not in plan, optional improvement discovered | Log issue, continue current task |
| high | plan.md module design infeasible, interface contract must change, core dependency incompatible | STOP task, trigger Deviation Protocol |

### Issue Conflict Resolution

When two issues suggest contradictory fixes (e.g. ISS-001 says "merge modules A and B", ISS-002 says "split A further"), resolve by these rules:

**Rule 1 — Severity overrides.** Higher severity issue takes priority. High > medium > low.

**Rule 2 — Blocker first.** If one issue's resolution blocks another (ISS-001 must be resolved before ISS-002 can proceed), the blocker resolves first regardless of severity.

**Rule 3 — User decides contradictions.** If Rules 1-2 don't resolve the conflict (same severity, no dependency), present both issues with their contradictions to the user:

```
⚠ Conflicting issues in issues.md

ISS-001 (high): Merge modules A and B
ISS-002 (high): Split module A into sub-modules
Conflict: Both affect module A's structure differently.

Recommendation: [your recommendation]
Please decide:
1. Follow ISS-001 (merge)
2. Follow ISS-002 (split)
3. Different approach
```

**Rule 4 — Resolved issues are closed.** Once an issue's fix is committed, set its status to `resolved` and note the resolving round. If a resolution reopens an existing resolved issue, the new entry supersedes the old one.

---

## 8. Round Summary Format

After Round N's tasks are all done (or all blocked), write a Round Summary in `start-and-resume.md § Round History`:

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

Always include a `**Current Round:**` line at the top of Round History. When a round completes and a new round starts, update this line. Previous rounds stay below as history.

---

## 9. Illegal State Rules

| Forbidden Action | Reason | Correct Approach |
|---|---|---|
| Modify plan.md mid-task during execution | Plan changes should be gated by Deviation Protocol | Log issue, defer to next round |
| Add tasks directly in tasks.md during execution | Task changes belong in Phase 05 | Log issue, update in Phase 01* next round |
| Skip S05 and jump S03->S01 directly | Round must complete before re-planning | Write Round Summary first |
| Agent decides Deviation Protocol outcome without showing user | Core discipline of the protocol | Always present options to user |
| Execute work not in tasks.md during S03 | Scope creep | Record in issues.md, follow protocol |

---

## 10. Round Complete Checklist

Before declaring Round N complete:

- [ ] All tasks in `generated/rounds/round-NNN/tasks.md` are `done` or `blocked`
- [ ] `issues.md` updated with any new issues found (even low/medium that were handled inline)
- [ ] Round Summary written in `start-and-resume.md § Round History` with `**Current Round:**` line
- [ ] `blueprint.md` updated (Round column, status, notes on open issues)
- [ ] All `[DEBUG-*]` instrumentation removed (if any)
- [ ] User informed: "Round N complete. X/Y tasks done. Z open issues."
- [ ] User asked: "Start Round N+1?" or "Done for now?"

> **Method reference:** If the Method Selection step at Round Complete (see `00-start-and-resume.md § Requirement Complete — Round End`) triggered Dual-Axis Review, run it before this checklist. The review outputs feed directly into the checklist items.
