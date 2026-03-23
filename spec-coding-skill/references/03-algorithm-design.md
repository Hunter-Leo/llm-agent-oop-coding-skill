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

### Diagrams

For every non-trivial algorithm or data structure, include at least one ASCII diagram illustrating the structure or flow:

```
Example — linked list traversal:

head → [A] → [B] → [C] → null
        ↑
     current
```

For state machines or multi-step flows, include a state/flow diagram:

```
[idle] --start--> [processing] --done--> [complete]
                       |
                    --error--> [failed]
```

### Use Cases & Examples

Provide at least two concrete examples that walk through the algorithm with real input/output values:

```
Example 1 — normal case:
  Input:  [3, 1, 4, 1, 5]
  Steps:  ...
  Output: [1, 1, 3, 4, 5]

Example 2 — edge case (empty input):
  Input:  []
  Output: []
```

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

- Read `init.md § Constitution` to understand the coding constraints for this requirement before designing
- Read [07-oop-principles.md](07-oop-principles.md) before designing — ensure the algorithm and data structures are compatible with OOP & SOLID principles
- Complete this document and review it before writing any implementation code
- If the pseudocode reveals a flaw, revise the design here first
- Reference this document in `plan.md`
