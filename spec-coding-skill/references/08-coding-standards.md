# Coding Standards

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

---

## Python

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

Use `uv` for all dependency operations:

```bash
uv add <package>       # add dependency
uv run <script>        # run script
uv sync                # sync dependencies
```

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

Use `npm`, `yarn`, or `pnpm` (be consistent within the project).

---

## Java

### Type System

- Use Bean Validation annotations (`@NotNull`, `@Size`, etc.) on all input models
- Define clear interfaces and abstract classes as extension points
- Use enums instead of string or int constants
- Follow SOLID principles (see [07-oop-principles.md](07-oop-principles.md))

### Documentation

- JavaDoc on all public classes and methods
- Document all parameters (`@param`), return values (`@return`), and exceptions (`@throws`)

### Dependency Management

Use Maven or Gradle (be consistent within the project).
