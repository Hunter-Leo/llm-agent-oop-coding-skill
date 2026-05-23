---
name: dual-axis-review
description: "Parallel code review by two independent axes — one checking against coding standards and OOP/SOLID principles, the other against the original spec and requirements. Use at Round Complete (Phase 07) or before creating a pull request. Do NOT use for XS/S tasks — the built-in self-check suffices."
tags: [review, quality-gate, standards, spec, phase-07, round-complete]
---

# Dual-Axis Review

Two independent review axes run as sub-agents in parallel. Adapted from the review methodology.

## When to Use

- **Round Complete** — before declaring done, after all tasks in tasks.md reach done
- **Pre-PR** — before creating a pull request or local merge
- **Critical path items** — during execution when a task introduces significant new code

Skip for XS/S tasks — the built-in self-check in 00-agent-execution.md § Self-Check covers these adequately.

## Process

### 1. Pin the fixed point

Determine what to compare against: a branch diff, the previous state before the round started, or the last commit.

### 2. Spawn both axes in parallel

Use two sub-agents (e.g. Agent tool with general-purpose type) in a single message:

**Axis 1 — Standards Review:**
- Read coding standards (constitution/02-coding-standards.md)
- Read OOP/SOLID principles (constitution/01-oop-principles.md)
- Read the diff
- Report per-file violations of: naming conventions, type annotations, docstrings, error handling, testing coverage, no hardcoded secrets, no lint errors
- Distinguish hard violations from judgment calls

**Axis 2 — Spec Review:**
- Read init.md (spec, requirements, acceptance criteria)
- Read plan.md (what was planned vs what was implemented)
- Read the diff
- Report: (a) requirements asked for that are missing or partial; (b) behavior in the diff that wasn't asked for (scope creep); (c) requirements that look implemented but incorrectly

### 3. Aggregate

Present reports under `## Standards` and `## Spec` headings. Do NOT merge or rerank — the two axes are deliberately independent.

End with: total findings per axis, and the worst single issue flagged.

## Why Two Axes

A change can pass one axis and fail the other:

- Code that follows every standard but implements the wrong thing → **Standards pass, Spec fail**
- Code that does exactly what the spec asked but breaks conventions → **Spec pass, Standards fail**

Reporting separately stops one axis from masking the other.

## Reconciliation

| Standards | Spec | Action |
|-----------|------|--------|
| ✅ Pass | ✅ Pass | Declare round complete |
| ❌ Fail | ✅ Pass | Fix standards violations, re-review |
| ✅ Pass | ❌ Fail | Fix spec gaps, re-review |
| ❌ Fail | ❌ Fail | Both need fixing — fix spec first (it changes what code is needed) |

If axes contradict (standards says refactor, spec says ship), present the trade-off to the user.
