# References

## Constitution — Must-read before execution

Always read before writing any code. Located in `constitution/`:

- [00-agent-execution.md](constitution/00-agent-execution.md) — Global agent behavior rules (all phases)
- [01-oop-principles.md](constitution/01-oop-principles.md) — OOP & SOLID principles
- [02-coding-standards.md](constitution/02-coding-standards.md) — Coding standards (universal + language-specific)
- [03-git-workflow.md](constitution/03-git-workflow.md) — Git workflow

## Round Execution — Enter the execution loop

Read when entering Phase 07 or resuming after a round completes. Located in `execution/`:

- [00-start-and-resume.md](execution/00-start-and-resume.md) — Execution loop + Deviation Protocol + Round History
- [01-round-mechanism.md](execution/01-round-mechanism.md) — Dual-layer state machine + Agent decision matrix + issues.md format

## Phase Workflow — Read on demand

Read when entering the corresponding phase. Located in `phases/`:

- [00-initialization.md](phases/00-initialization.md) — Phase 01 initialization
- [01-prerequisite-tasks.md](phases/01-prerequisite-tasks.md) — Phase 02 prerequisite tasks
- [02-algorithm-design.md](phases/02-algorithm-design.md) — Phase 03 algorithm design
- [03-implementation-plan.md](phases/03-implementation-plan.md) — Phase 04 implementation plan
- [04-task-planning.md](phases/04-task-planning.md) — Phase 05 task planning
- [05-blueprint-management.md](phases/05-blueprint-management.md) — Blueprint management

## Methods — Trigger-Driven Selection

Evaluated at phase entry, not advisory. Each method's trigger conditions determine whether it applies. Located in `methods/`:

- [00-term-grilling-and-adr.md](methods/00-term-grilling-and-adr.md) — Terminology alignment + ADR (Phase 01/02/04)
- [01-vertical-slice-tdd.md](methods/01-vertical-slice-tdd.md) — RED-GREEN-REFACTOR per behavior (Phase 07)
- [02-dual-axis-review.md](methods/02-dual-axis-review.md) — Standards + Spec parallel review (Round Complete)
- [03-architecture-deepening.md](methods/03-architecture-deepening.md) — Shallow module detection (Phase 02)
- [04-structured-debugging.md](methods/04-structured-debugging.md) — Six-phase bug diagnosis (Phase 02)

These methods are **trigger-driven**: at the entry of each applicable phase, the agent must evaluate trigger conditions and apply the method if they match, or justify skipping if they do not. See [SKILL.md § Method Selection](../SKILL.md#method-selection--trigger-driven) for the full rules.

## Naming Convention

All prefixes are unique within their directory:
- **00-09** = constitution (`constitution/`)
- **00-09** = extended methods (`methods/`) — same range, separate directory, no conflict
- **00-01** = round execution (`execution/`) — numbered for reading order
- **00-05** = phase workflow (`phases/`) — numbered for reading order
