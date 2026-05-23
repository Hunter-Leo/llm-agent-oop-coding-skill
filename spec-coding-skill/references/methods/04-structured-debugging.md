---
name: structured-debugging
description: "Six-phase structured debugging process for non-trivial bug fixes. Use during Phase 02 Prerequisite Tasks when the requirement is a bug fix with unclear root cause. Do NOT use for trivial bugs where the root cause is immediately visible from the symptom."
tags: [debugging, diagnosis, bug-fix, hypothesis, regression, phase-02]
---

# Structured Debugging

Six-phase process for hard bugs. Never skip phases — treat each one as mandatory unless you can explicitly justify why it doesn't apply. Adapted from the diagnose methodology.

## Phase 1 — Build a Feedback Loop

**This is the most important phase.** If you have a fast, deterministic, agent-runnable pass/fail signal for the bug, you will find the cause. If you don't, no amount of staring at code will save you.

Spend disproportionate effort here. **Be aggressive. Be creative. Refuse to give up.**

Ways to construct one (try in this order):

1. **Failing test** at whatever seam reaches the bug
2. **HTTP script** (curl / httpie) against a running dev server
3. **CLI invocation** with a fixture input, diffing stdout against known-good snapshot
4. **Playwright / browser script** — drives UI, asserts on DOM/console/network
5. **Replay a captured trace** — save real request to disk, replay through code path
6. **Throwaway harness** — minimal subset of the system exercising the bug path
7. **Bisection** — if bug appeared between two known states, automate "boot at state X, check"

Iterate on the loop itself:
- Can I make it faster? (Cache setup, skip unrelated init)
- Can I make the signal sharper? (Assert on specific symptom, not "didn't crash")
- Can I make it more deterministic? (Pin time, seed RNG, freeze network)

**If you genuinely cannot build a loop:** Stop and say so explicitly. List what you tried. Ask the user for access to the reproduction environment, a captured artifact, or permission to add temporary production instrumentation.

Do NOT proceed to Phase 2 without a loop you believe in.

## Phase 2 — Reproduce

Run the loop. Confirm:
- [ ] The loop reproduces the bug the **user** described — not a different nearby failure
- [ ] The failure is reproducible across multiple runs
- [ ] You have captured the exact symptom (error message, wrong output, timing)

## Phase 3 — Hypothesise

Generate **3-5 ranked hypotheses** before testing any. Single-hypothesis generation anchors on the first plausible idea.

Each hypothesis must be **falsifiable** — state the prediction it makes:

> "If <X> is the cause, then <changing Y> will make the bug disappear."

If you cannot state the prediction, the hypothesis is a guess — discard or sharpen it.

Rank hypotheses by: explanatory power (does it explain all symptoms?), testability (can you prove/disprove with one experiment?), likelihood (is this the most probable cause?).

**Show the ranked list to the user before testing.** They often have domain knowledge that re-ranks instantly.

## Phase 4 — Instrument

Each probe must map to one specific hypothesis. **Change one variable at a time.**

Tool preference:
1. Debugger / REPL inspection — one breakpoint beats ten logs
2. Targeted logs at the boundaries that distinguish hypotheses
3. Never "log everything and grep"

**Tag every debug log** with a unique prefix (e.g. `[DEBUG-a4f2]`) — cleanup becomes a single grep.

## Phase 5 — Fix + Regression Test

Write the regression test **before the fix** — but only at a correct seam:

1. Turn the repro into a failing test at the right seam
2. Watch it fail
3. Apply the minimal fix
4. Watch it pass
5. Re-run the original (un-minimised) feedback loop

**If no correct seam exists**, that itself is the finding — the architecture prevents the bug from being locked down. Note this and flag it.

## Phase 6 — Cleanup + Post-mortem

- [ ] Original repro no longer reproduces (re-run Phase 1 loop)
- [ ] Regression test passes (or absence of seam documented)
- [ ] All `[DEBUG-*]` instrumentation removed
- [ ] Diagnosis documented in `diagnosis.md` (root cause, fix approach, regression details)
- [ ] Root cause stated in commit message — so the next debugger learns

Then ask: **what would have prevented this bug?** If the answer involves architectural change (no test seam, tangled callers, hidden coupling), log to `.dev/TODO.md`.
