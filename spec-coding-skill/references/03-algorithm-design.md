# Phase 03 — Algorithm Design

This phase is **optional**. Produce `algorithm-design.md` when the requirement involves implementing a non-trivial algorithm.

## Trigger Conditions

Activate this phase if the requirement involves any of the following:

- Graph algorithms (traversal, shortest path, cycle detection, etc.)
- Dynamic programming or memoization
- Custom data structures (trees, heaps, tries, etc.)
- Complexity-sensitive operations (the naive approach would be too slow)
- Concurrent or parallel processing logic
- Complex state machines or rule engines

If the requirement only involves straightforward CRUD, simple data transformation, or calling existing library APIs, skip this phase.

## algorithm-design.md Structure

### Problem Analysis

- Restate the problem in precise terms
- Identify inputs, outputs, and constraints
- Clarify edge cases and boundary conditions

### Approach Options

For each candidate approach:

- Description of the approach
- Time complexity: O(?)
- Space complexity: O(?)
- Trade-offs and limitations

### Selected Approach

- Chosen approach and justification
- Why alternatives were rejected

### Data Structures

- Data structures required and why each was chosen
- Key invariants that must be maintained

### Pseudocode

Write clear pseudocode before any implementation:

```
function solve(input):
    // step 1: ...
    // step 2: ...
    return result
```

### Complexity Summary

| Metric | Value |
|---|---|
| Time complexity | O(?) |
| Space complexity | O(?) |
| Known bottlenecks | ... |

## Rules

- Complete this document and review it before writing any implementation code
- If the pseudocode reveals a flaw, revise the design here first
- Reference this document in `plan.md`
