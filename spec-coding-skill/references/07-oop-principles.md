# OOP & SOLID Principles

All code produced under this skill must follow object-oriented design and the SOLID principles.

---

## Object-Oriented Design

### Encapsulation

Keep internal state private. Expose only what callers need.

```python
# Bad
class Order:
    total = 0

# Good
class Order:
    def __init__(self) -> None:
        self._total: float = 0.0

    def get_total(self) -> float:
        return self._total
```

### Abstraction

Hide implementation details behind interfaces or abstract classes. Callers depend on contracts, not internals.

### Inheritance

Use inheritance to model genuine "is-a" relationships. Prefer composition over inheritance when in doubt.

### Polymorphism

Design so that different implementations can be substituted without changing the calling code.

---

## SOLID Principles

### O — Open/Closed Principle ⭐ Most Important

Open for extension, closed for modification. Add new behavior by adding new code, not by editing existing code.

This is the most impactful SOLID principle. A codebase that consistently applies OCP can absorb new requirements without touching existing, tested logic.

- Use abstract base classes or interfaces as extension points
- Avoid long `if/elif` chains that must be edited for every new case — replace with polymorphism

```python
# Bad: must edit this function for every new payment type
def process_payment(type: str, amount: float) -> None:
    if type == "card": ...
    elif type == "paypal": ...

# Good: extend by adding a new class, existing code untouched
class PaymentProcessor(ABC):
    @abstractmethod
    def process(self, amount: float) -> None: ...

class CardProcessor(PaymentProcessor): ...
class PaypalProcessor(PaymentProcessor): ...
```

### S — Single Responsibility Principle

A class or function should have one reason to change.

- One class = one responsibility
- If you need "and" to describe what a class does, split it

```python
# Bad: handles both parsing and persistence
class UserManager:
    def parse_csv(self, data: str) -> list[User]: ...
    def save_to_db(self, user: User) -> None: ...

# Good: separate responsibilities
class UserParser:
    def parse_csv(self, data: str) -> list[User]: ...

class UserRepository:
    def save(self, user: User) -> None: ...
```

### L — Liskov Substitution Principle

A subclass must be usable wherever its parent is expected, without breaking correctness.

- Subclasses must not weaken preconditions or strengthen postconditions
- If a subclass needs to override a method in a way that changes its contract, reconsider the hierarchy

### I — Interface Segregation Principle

Clients should not depend on methods they do not use. Prefer small, focused interfaces over large general ones.

```python
# Bad: implementors must provide methods they don't need
class Worker(ABC):
    def work(self) -> None: ...
    def eat(self) -> None: ...   # robots don't eat

# Good: separate interfaces
class Workable(ABC):
    def work(self) -> None: ...

class Eatable(ABC):
    def eat(self) -> None: ...
```

### D — Dependency Inversion Principle

High-level modules should not depend on low-level modules. Both should depend on abstractions.

- Inject dependencies rather than instantiating them inside a class
- Depend on interfaces, not concrete implementations

```python
# Bad: high-level class creates its own dependency
class OrderService:
    def __init__(self) -> None:
        self._repo = MySQLOrderRepository()  # hard dependency

# Good: dependency injected from outside
class OrderService:
    def __init__(self, repo: OrderRepository) -> None:
        self._repo = repo
```

---

## Design Patterns — When to Use

| Pattern | Use when |
|---|---|
| Factory / Factory Method | Object creation logic is complex or varies by type |
| Strategy | You need to swap algorithms or behaviors at runtime |
| Observer | One change should notify multiple dependents |
| Repository | You want to decouple data access from business logic |
| Decorator | You need to add behavior to objects without subclassing |
| Singleton | Exactly one instance is required (use sparingly) |
