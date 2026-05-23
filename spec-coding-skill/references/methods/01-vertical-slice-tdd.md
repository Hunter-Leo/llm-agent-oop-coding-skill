---
name: vertical-slice-tdd
description: "Test-Driven Development with vertical slicing — each test cuts through all application layers end-to-end through the public interface. Use during Phase 07 Execution Loop when implementing tasks with clear, stable public interfaces. Do NOT use for exploratory work, data-science scripts, or when the public interface is undefined."
tags: [tdd, testing, vertical-slice, red-green-refactor, phase-07]
---

# Vertical Slice TDD

RED-GREEN-REFACTOR micro-cycles, each verifying behavior through the public interface. Adapted from the tdd methodology.

## Core Principle

**One test at a time, minimal code to pass, then refactor.** Each test is a thin vertical slice — it exercises the complete path from public interface through all layers to verify one behavior.

```
RED:   Write one failing test through the public interface
GREEN: Implement minimal code to pass this test and only this test
REFACTOR: Improve code quality without changing behavior
          ↓
     (repeat for next behavior)
```

## Vertical vs Horizontal Slicing

**Horizontal (wrong):**
```
RED:   test1, test2, test3, test4, test5     ← tests imagined behavior
GREEN: impl1, impl2, impl3, impl4, impl5     ← may pass wrong tests
```
Writing all tests first commits to test structure before understanding the implementation. Tests test "shape" rather than behavior.

**Vertical (correct):**
```
RED→GREEN: test1 → impl1      ← proves one behavior end-to-end
RED→GREEN: test2 → impl2      ← builds on what was learned
RED→GREEN: test3 → impl3      ← each cycle tightens feedback
```

Each cycle responds to what the previous one revealed. Because you just wrote the code, you know exactly what behavior matters and how to verify it.

## Cycle Rules

- One test per behavior, through the public interface only
- Minimal code to pass — do not anticipate future tests
- Never refactor while RED — get to GREEN first
- Tests describe WHAT, not HOW — they survive internal refactors

```python
# GOOD — tests behavior through public interface
def test_user_can_retrieve_created_order():
    user = create_user("alice@example.com")
    order = place_order(user.id, [Product("book", 10)])
    retrieved = get_order(order.id)
    assert retrieved.status == "confirmed"
    assert retrieved.total == 10

# BAD — tests implementation, coupled to internals
def test_order_service_creates_order_in_db():
    mock_repo = Mock(OrderRepository)
    service = OrderService(mock_repo)
    service.create(...)
    assert mock_repo.save.called  # tests call sequence, not behavior
```

## Integration with Execution Loop

When using this method, the Phase 07 execution loop (steps 5-10) becomes a micro-cycle per behavior within each task:

```
For each behavior in the task:
  a. RED: Write one failing test → confirm failure
  b. GREEN: Implement minimal code → pass
  c. REFACTOR: Clean up, run all tests → pass
  d. Commit: one commit per behavior
```

The "write code → write tests" two-step is replaced by the RED-GREEN-REFACTOR cycle. The verification checklist still applies at the task level.

## When NOT to Use

- **Performance benchmarks** — need targeted measurement, not behavior tests
- **Integration smoke tests** — verify wiring, not individual behavior
- **Migration tasks** — moving code without changing behavior
- **Exploratory / data-science** — interface is undefined or unstable
- **Prototypes** — throwaway code, TDD overhead wasted

In these cases, fall back to the standard implement-then-test sequence.
