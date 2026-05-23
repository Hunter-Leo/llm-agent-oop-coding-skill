---
name: term-grilling-and-adr
description: "Rigorous terminology alignment and Architecture Decision Record creation for spec-driven development. Use during Phase 01 (after drafting init.md) or Phase 04 (after drafting plan.md) when the spec or plan contains ambiguous terminology, undefined concepts, or hard-to-reverse technical decisions that need formal documentation."
tags: [terminology, adr, alignment, phase-01, phase-02, phase-04]
---

# Term Grilling + ADR

Method for sharpening terminology and documenting decisions. Adapted from the grill-with-docs methodology.

## When to Use

**Signal words** in the spec/plan that suggest fuzzy terminology needs sharpening:
- "robust", "scalable", "efficient", "modern" — these describe feelings, not requirements
- Terms used in multiple places with potentially different meanings
- New concepts introduced without definition

**Hard-to-reverse decisions** that deserve an ADR:
- Tech stack choices (database, message bus, auth provider, deployment target)
- Interface contracts between modules
- Data model / schema decisions
- Architectural shape (monorepo vs multi-repo, sync vs event-driven)
- Deliberate deviations from the obvious path

Do **not** use this method when: the terminology is already well-defined (project has an existing glossary), decisions are trivially reversible, or the task is XS/S.

## Glossary (CONTEXT.md)

Read or create `.dev/CONTEXT.md` as a project-level glossary:

```markdown
# Project Glossary

## Language

**Order:**
A request for goods or services placed by a customer.
_Avoid:_ Purchase, transaction

**Customer:**
A person or organization that places orders.
_Avoid:_ Client, user (when referring to the buying entity)

## Relationships

- A **Customer** places many **Orders**
- An **Order** contains one or more **Line Items**

## Flagged Ambiguities

- "Account" was used to mean both login credentials and billing customer — resolved: login is **User**, billing is **Customer**
```

**Rules:**
- Be opinionated — when multiple words exist for the same concept, pick the best one and list others as aliases to avoid
- Keep definitions to 1-2 sentences. Define what it IS, not what it does
- Only include project-specific domain terms — general programming concepts (timeout, error type, utility) do not belong
- Write an example dialogue showing how terms interact naturally
- Create the file lazily — only when the first term is resolved

## Architecture Decision Records (ADR)

ADRs live at `.dev/adr/NNNN-slug.md` (sequential numbering). Create the directory lazily when the first ADR is needed.

**Simple template** — an ADR can be a single paragraph:

```markdown
# Use Postgres for Write Model

Event-sourced write model needs a battle-tested ACID store. Postgres is the team's existing expertise, supports JSONB for event storage, and has mature change-data-capture tooling. DynamoDB would also work but adds a new operational surface.
```

**Only offer an ADR when ALL three are true:**
1. **Hard to reverse** — changing your mind later costs meaningful effort
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **Result of a real trade-off** — genuine alternatives existed with specific reasons for the choice

**Optional sections** (only when they add value):
- `**Status:** proposed | accepted | deprecated | superseded by ADR-NNNN`
- `**Considered Options:**` — when rejected alternatives are worth remembering
- `**Consequences:**` — when non-obvious downstream effects need to be called out

## Plan Stress Test

After plan.md is drafted, walk through each implementation step with concrete scenarios:

- "This step expects input X — what happens with input Y?"
- "If module A fails, does module B have a fallback?"
- "Does the plan's terminology survive this scenario?"
- "Is every term used consistently across all steps?"

For each uncovered issue, either update the plan or create an ADR.

## Outputs

- Updated terminology in the relevant document (init.md or plan.md)
- `.dev/CONTEXT.md` — project glossary (created or updated)
- `.dev/adr/NNNN-*.md` — ADR files
