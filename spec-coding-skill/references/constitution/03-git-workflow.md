# Git Workflow

All Git messages must be in **English** and follow the [Google style guide](https://google.github.io/styleguide/) for commit messages.

---

## Pre-Commit Checks

Before every commit, ensure:

- [ ] All existing tests pass (no regressions)
- [ ] New unit tests pass
- [ ] No linting errors introduced (run linter if available)
- [ ] No hardcoded secrets or environment-specific values
- [ ] All docstrings and type annotations are complete
- [ ] Unnecessary files (`.pyc`, `.DS_Store`, logs) are not staged

---

## Branch

Create a new branch for each requirement:

- **M+ tasks:** `<type>/[NNN]-[req-name]` (e.g. `feat/001-user-auth`)
- **XS/S tasks:** `<type>/<short-description>` (e.g. `fix/email-retry-param`)

Always create the branch **before** starting any implementation — never commit to `main` or another requirement's branch. Never delete a branch unless the user explicitly asks.

```bash
git checkout -b <type>/[NNN]-[req-name]
# e.g. git checkout -b feat/001-user-auth
#      git checkout -b fix/002-payment-timeout
#      git checkout -b refactor/003-extract-base-processor
```

**Type values:**

| Type | When to Use |
|------|-------------|
| `feat` | New feature, module, or capability |
| `fix` | Bug fix or regression repair |
| `refactor` | Code restructuring without behavior change |
| `test` | Adding or updating tests |
| `docs` | Documentation, comments, or docstrings |
| `style` | Code formatting, whitespace, semicolons — no logic change |
| `perf` | Performance optimization |
| `ci` | CI/CD pipeline or config changes |
| `build` | Build system, dependencies, or tooling |
| `chore` | Maintenance, cleanup, or non-functional housekeeping |

---

## Commit Messages

Format (Google style):

```
[NNN] T-XXX <type>: <capitalized imperative summary, ≤ 72 chars>

<blank line, then body: what and why, not how. Wrap at 72 chars.>
```

- Summary: capitalized, imperative mood, no trailing period
- Body: blank line after summary, explain *why* (motivation) and *what* (context), not *how*
- Body lines wrapped at 72 characters

**Examples:**

```
[001] T-001 feat: add UserRepository with CRUD operations

The login flow requires persistent user storage. This commit introduces
a UserRepository class that wraps SQLite access, supporting create, read,
update, and delete operations with parameterized queries to prevent SQL
injection.

[001] T-002 test: add unit tests for UserRepository

Covers normal CRUD paths, empty result sets, and duplicate key errors.
```

**Rules:**
- Commit after each task reaches `done`
- Summary line ≤ 72 characters
- Use imperative mood: "add", "fix", "extract" — not "added", "fixing"
- One logical change per commit — do not batch multiple tasks into one commit
- **XS/S tasks:** omit `[NNN]` and `T-XXX`. Use `<type>: <summary>` instead.

---

## Pull Request / Local Merge

When all tasks are done, merge via PR (remote) or local merge. **Both use the same format.**

**Merge message format:**

```
<type>(<scope>): <imperative summary, ≤ 72 chars>

## Summary

<1-3 bullet points describing what changed and why>

## Changes

| Commit | Summary |
|--------|---------|
| abc1234 | T-001 feat: add UserRepository |
| def5678 | T-002 test: add unit tests |

## Breaking Changes

None. (or list any)

## Test Plan

- [ ] All existing tests pass
- [ ] No regressions introduced
```

**Local merge command:**
```bash
git checkout main && git merge --no-ff <type>/[NNN]-[req-name] -m "$(cat <<'EOF'
<type>(<scope>): <imperative summary>

## Summary

<1-3 bullet points>

## Changes

| Commit | Summary |
|--------|---------|
| abc1234 | T-001 feat: add UserRepository |
| def5678 | T-002 test: add unit tests |

## Breaking Changes

None. (or list any)

## Test Plan

- [ ] All existing tests pass
- [ ] No regressions introduced
EOF
)"
```

**Pre-merge checklist:**
- [ ] Branch is up to date with target (rebase if needed: `git rebase main`)
- [ ] All tests pass
- [ ] No merge conflicts
- [ ] TODO.md items for this requirement are updated
- [ ] Blueprint updated to `07 Done`

After merge, ask the user: "Branch `<type>/[NNN]-[req-name]` can be deleted. Do you want me to clean it up?" Only delete on explicit confirmation.

---

## Tagging a Release

When the user requests a release, or enough changes have accumulated:

```bash
git tag -a v<major>.<minor>.<patch> --cleanup=verbatim -m "$(cat)"
```

**Tag message format — full changelog since previous tag:**

```
vX.Y.Z

## New Features
- feat: summary of each feature commit

## Bug Fixes
- fix: summary of each bugfix commit

## Refactoring
- refactor: summary of each refactor commit

## Documentation
- docs: summary of each docs commit
```

Generate the changelog by grouping commits since the last tag:

```bash
git log --oneline --no-merges <previous-tag>..HEAD
```

**Version conventions:**
- **Patch** (1.0.0→1.0.1): bug fixes only
- **Minor** (1.0.0→1.1.0): new features, backward compatible
- **Major** (1.0.0→2.0.0): breaking changes
