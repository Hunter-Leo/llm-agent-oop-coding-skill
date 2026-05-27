# Coding Standards

> ## Read Before Writing Any Code
>
> These standards apply to **every line of code** in every phase. The execution loop requires you to re-read this file before implementation (step 4). **Do not skip — violations will be caught at pre-commit check and require rework.**

## Universal Standards (All Languages)

### Language & Naming

- All identifiers (variables, functions, classes, constants) must be in English
- All comments and documentation must be in English
- Follow the naming convention of the project language (snake_case for Python, camelCase for JS/TS, PascalCase for classes everywhere)

### Type Annotations

- Weakly-typed languages (Python, JavaScript) **must** use type annotations on all function parameters and return values — no untyped signatures
- Strongly-typed languages must define explicit types for all public interfaces

### Comments

- Follow Google Style Guide for all comments and docstrings
- Every file, public class, and public function must have a documentation comment
- For modules that implement abstract algorithms or rely on domain-specific background knowledge, the comment **must** include an ASCII diagram or concrete example that makes the logic self-explanatory without external references
- Comments explain *why*, not *what* — the code already shows what

### Code Quality

- DRY: do not duplicate logic — extract shared code into a function or class
- Single responsibility: each function does one thing
- Avoid deeply nested conditionals — use early returns or guard clauses
- Keep functions short; if a function needs a comment to explain each section, split it

### Project Structure

- **No circular dependencies between modules.** If module A imports from B and B imports from A (directly or transitively), the project structure has a problem.
- **Fix circular dependencies by extracting shared code**, not by adding workarounds. The shared type, utility, or interface that causes the cycle belongs in its own file or module. If a language provides escape hatches for circular imports (Python `TYPE_CHECKING`, TypeScript `import type`), treat those as a last resort after restructuring has been considered — see Python § Import Practices for the decision flow.
- A flat module with clean dependency direction is better than a deeply nested one with cycles.

### Error Handling

- Handle errors explicitly — never silently swallow exceptions
- Error messages must be meaningful and actionable
- Distinguish between recoverable errors (return/raise domain error) and programming errors (let them propagate)

### Security

- Never hardcode secrets, API keys, or passwords — use environment variables or config files
- Validate and sanitize all external inputs
- Avoid SQL injection, XSS, and CSRF vulnerabilities

### Performance

- Avoid N+1 query patterns
- Use pagination or streaming for large data sets
- Avoid unnecessary nested loops and redundant computation
- Use caching where appropriate, but document cache invalidation strategy

### Testing

- Write tests for each module before moving to the next
- Every test file must cover: normal cases, edge cases, error/exception cases
- Minimum coverage for core logic: 80%

| Language | Test file naming |
|---|---|
| Python | `test_*.py` or `*_test.py` |
| TypeScript/JavaScript | `*.test.ts` or `*.spec.ts` |
| Java | `*Test.java` |
| Go | `*_test.go` |
| Rust | `#[cfg(test)] mod tests` in same file |

### Testing Philosophy

Tests should verify behavior through **public interfaces**, not implementation details. A good test survives internal refactors — if you rename a private function and tests break, those tests were testing implementation, not behavior.

**Principle — "The interface is the test surface":**

Prefer testing through the same seam callers use. This produces tests that:
- Describe WHAT the system does, not HOW
- Remain stable across refactors
- Give confidence the feature actually works end-to-end

```python
# GOOD — verifies behavior through public interface
def test_checkout_confirms_with_valid_cart():
    cart = create_cart([product])
    result = checkout(cart, valid_payment)
    assert result.status == "confirmed"

# BAD — verifies implementation details
def test_checkout_calls_payment_service():
    mock_payment = Mock(PaymentService)
    checkout(cart, mock_payment)
    assert mock_payment.process.called  # tests HOW not WHAT
```

**This is a default, not a dogma.** Deviate from "interface-first" testing when:

| Scenario | Why to deviate | Alternative |
|----------|---------------|-------------|
| Data pipeline / ETL | Full end-to-end too slow | Test each transform in isolation |
| ML / data science | Interface (predict()) too thin | Test internal pre-processing + model logic separately |
| Complex algorithm | Top-level test makes debugging hard | Layer tests: unit test internals + integration test interface |
| Performance-sensitive | Correctness != performance | Separate benchmark harness for perf, standard tests for correctness |
| Glue code / script | Interface barely exists | Test individual functions |
| IaC / K8s operator | Interface is declarative config, real execution deploys resources | Module-level unit tests + dry-run integration |
| Third-party integration | External API not available in test env | Test your adapter logic with a fake, not the real endpoint |

**Mock discipline:**

Only mock at **system boundaries** — external APIs, databases (prefer test DB), time/randomness, file system. Never mock your own classes, modules, or internal collaborators.

```python
# OK — mock at system boundary
def test_send_email_retries_on_timeout():
    smtp_client = Mock(SMTPClient)
    smtp_client.send.side_effect = [TimeoutError, ResponseOk()]
    result = send_notification(smtp_client, "test@example.com", "Hello")
    assert result.delivered

# BAD — mock internal collaborator
def test_order_service_creates_order():
    repo = Mock(OrderRepository)     # your own module, don't mock
    service = OrderService(repo)
    order = service.create(...)
    assert repo.save.called          # tests call sequence, not behavior
```

**Test anti-patterns to avoid:**

| Pattern | Why it's wrong | Fix |
|---------|---------------|-----|
| Test private methods | Tests snap to internal structure, break on refactor | Test through public interface |
| Assert on call count/order | Knows HOW, not WHAT | Assert on result value or state |
| Bypass interface to verify (e.g., direct DB query) | Tests the test's understanding of internals | Verify through the public return value |
| One test asserts many things | When first assertion fails, rest are hidden | One logical assertion per test |
| Test name describes implementation ("calls X method") | Names become wrong after refactor | Name describes behavior ("returns confirmed status") |

### Type System

- Use Pydantic v2 for all meaningful data structures
- No bare `str`, `int`, `float` as domain types — wrap them in Pydantic models or use constrained types
- Prefer `StrEnum` or `Literal` over plain `str` for values with a fixed set of options
- Use constrained types for bounded values: `conint(ge=0)`, `confloat(gt=0.0)`
- Field definition format: `Annotated[Type, Field(description=..., example=...)] = default`

**Prohibited patterns:**

| Pattern | Why | Alternative |
|---|---|---|
| `Dict[str, T]` with variable keys | Keys carry implicit meaning, no schema | `list[SomeModel]` with a named id field |
| `Tuple[str, str, int]` | Positional, no field names | Named Pydantic model |
| Nested plain collections (`List[Dict[str, Any]]`) | No type safety, no validation | Nested Pydantic models |

**Allowed:** `Dict[Literal["x", "y"], float]` — fixed, known keys are fine.

```python
# Bad
scores: Dict[str, int]           # variable keys
point: Tuple[float, float]       # positional, no names
tags: List[Dict[str, str]]       # nested plain collection

# Good
class UserScore(BaseModel):
    user_id: UserId
    score: conint(ge=0)

class Point(BaseModel):
    x: float
    y: float

class Tag(BaseModel):
    key: TagKey        # StrEnum
    value: TagValue    # StrEnum
```

### Documentation

- Every file must have a module-level docstring
- Every class must have a class-level docstring
- Every function must have a full docstring with `Args`, `Returns`, and `Raises`

### Dependency Management

**uv is the preferred toolchain** for all Python dependency and script operations. It replaces pip, venv, pip-tools, and poetry in a single binary.

If `uv` is not installed, recommend global installation:
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Project mode** (requirements have a `pyproject.toml`):
```bash
uv add <package>       # add dependency
uv sync                # sync all dependencies
uv run <script>        # run script in project venv
uv lock                # update lockfile
```

**Script mode** (one-shot with temporary dependencies — no project needed):
```bash
uv run --with pandas,numpy,requests script.py
```
This downloads dependencies into a temporary cache, runs the script, and discards nothing. Useful for ad-hoc data analysis, debugging scripts, or quick prototypes.

If a project already has a `requirements.txt` / `Pipfile` / `poetry.lock`, `uv` can import from them (`uv add -r requirements.txt`). Migrate fully to `uv` when practical.

### Import Practices

**Avoid unnecessary lazy imports.** Import at the top of the file unless there is a concrete circular-import or startup-performance reason to defer:

```python
# Bad: lazy import hides the dependency
def send_email() -> None:
    from smtplib import SMTP  # no good reason for lazy import

# Good: top-level import
from smtplib import SMTP

def send_email() -> None:
    ...
```

**Use `if TYPE_CHECKING` for circular imports, not lazy imports.** If two modules depend on each other at type-check time only, guard the import:

```python
from __future__ import annotations  # all annotations are strings
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from models.user import User  # only needed for type hints
```

**If `if TYPE_CHECKING` is needed, consider restructuring.** A need for `if TYPE_CHECKING` is often a smell that the project has a circular dependency at the module or package level. Before adding the guard, ask:

- Can the shared type be extracted to a dedicated `types.py` or `protocols.py`?
- Can the dependency direction be inverted (which module depends on which)?
- Does a new file need to be extracted?

If the answer to all three is "no", then `if TYPE_CHECKING` is the right tool. If any is "yes", refactor first — it will produce cleaner code and fewer surprises.

---

## TypeScript / JavaScript

### Type System

- Enable TypeScript strict mode (`"strict": true`)
- No `any` — use `unknown` and narrow it, or define a proper type
- Use interfaces or type aliases for all data shapes
- Use enums or union types instead of string constants
- Use Zod (or equivalent) for runtime validation of external data

### Documentation

- JSDoc comments on all exported functions and classes
- Document all parameters and return types

### Dependency Management

**fnm + pnpm** is the recommended modern toolchain.

**Node version management** — use `fnm` (Fast Node Manager, Rust-based, faster than nvm):

```bash
eval "$(fnm env)"         # activate (add to .zshrc / .bashrc)
fnm install                # install from .node-version / .nvmrc
fnm use                    # switch to project's version
```

Projects should pin their Node version with a `.node-version` file:
```
20.12.0
```

`fnm` auto-reads `.node-version` and `.nvmrc` when `fnm use` is run.

**Package management** — prefer `pnpm` over npm/yarn:

```bash
pnpm add <package>         # add dependency
pnpm add -D <package>      # add dev dependency
pnpm dev / pnpm build      # run scripts (reads pnpm build)
```

Reasons for pnpm preference:
- **Disk efficient** — hard links a single copy per machine, not per project
- **Strict dependency resolution** — prevents phantom dependencies (importing packages not in `package.json`)
- **Faster** — parallel installation, efficient caching

If a project already has a `package-lock.json` (npm) or `yarn.lock`, keep using the existing tool for consistency — switching mid-project creates unnecessary churn. For new projects, use `pnpm init`.

---

## Java

### Type System

- Use Bean Validation annotations (`@NotNull`, `@Size`, etc.) on all input models
- Define clear interfaces and abstract classes as extension points
- Use enums instead of string or int constants
- Follow SOLID principles (see [01-oop-principles.md](01-oop-principles.md))

### Documentation

- JavaDoc on all public classes and methods
- Document all parameters (`@param`), return values (`@return`), and exceptions (`@throws`)

### Dependency Management

Use Maven or Gradle (be consistent within the project).
