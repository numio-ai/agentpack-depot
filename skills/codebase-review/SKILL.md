---
name: codebase-review
description: Review the codebase for quality, consistency, and maintainability. Run periodically after completing a batch of tasks to catch technical debt, inconsistencies, and quality regressions introduced by narrow task-focused work. Produces backlog tasks for each finding — does not modify code.
---

# Codebase Review

Read-only review of the codebase. Produces actionable backlog tasks for findings. Does **not** modify code.

## When to Use

Run periodically — after completing a batch of tasks, before a release, or when the codebase has accumulated drift from multiple contributors or agents working on isolated tasks.

## Workflow

1. **Determine scope** — ask the user what to review: the entire codebase, a specific module, or recent changes (e.g., `git diff main..HEAD` or a date range).
2. **Run the checklist** — evaluate each category below. For each finding, note the file, line, and a concrete recommendation.
3. **Present summary to user** — show findings grouped by category and severity. Discuss with user which findings warrant action.
4. **Create backlog tasks** — for each approved finding (or group of related findings), create a task file in `tasks/backlog/` following the task management standard. These tasks are then executed later via `/stc:implement-task`.

## Review Checklist

### 1. Dead Code & Unused Artifacts
- Unused imports, variables, functions, classes, and modules
- Commented-out code blocks (belongs in version control, not in source)
- Unused dependencies in `requirements.txt`, `package.json`, etc.
- Orphaned configuration files or scripts that no longer serve a purpose

### 2. Type Safety
- **Python**: all function signatures should have type hints for parameters and return values.
- **TypeScript**: no `any` types without justification. Prefer interfaces over inline types for reused shapes.
- Data transfer boundaries (API endpoints, serialization) should have explicit type definitions.

### 3. Function & Module Design
- Functions should be small and do one thing. If a function exceeds ~30 lines, evaluate whether it can be decomposed.
- Modules should have clear single responsibility.
- Nesting depth: flag functions with more than 3 levels of nesting.

### 4. DRY Violations
- Duplicated logic across files or modules.
- Similar but slightly different implementations of the same concept.
- Opportunities to extract shared utilities, base classes, or mixins.

### 5. Naming & Readability
- Consistent naming conventions within and across modules.
- Variable and function names should reveal intent.
- Boolean variables/functions should read as questions (`is_active`, `has_permission`, `canEdit`).

### 6. Error Handling
- No bare `except:` or `catch {}` that silently swallow errors.
- Error messages should be actionable.
- Consistent error handling patterns across the codebase.

### 7. API & Interface Consistency
- Consistent patterns for endpoint structure, request/response shapes, and status codes.
- Internal module interfaces should follow the same conventions as the rest of the codebase.

### 8. Configuration & Secrets
- No hardcoded values that should be configurable.
- No secrets, credentials, or tokens in source code.

### 9. Test Coverage Gaps
- New functionality added without corresponding tests.
- Critical paths (authentication, payment, data mutations) must have test coverage.

### 10. Dependency Health
- Outdated dependencies with known vulnerabilities.
- Unused dependencies still listed in manifests.

## Summary Format

```
## Codebase Review Summary

### Critical (must fix)
- [category] file:line — description and recommendation

### Improvement (should fix)
- [category] file:line — description and recommendation

### Minor (nice to have)
- [category] file:line — description and recommendation

### Positive observations
- Things that are well-done and should be preserved as patterns
```

## Task Creation

After the user reviews the summary and approves which findings to act on:

- Create one task file per finding (or group of closely related findings) in `tasks/backlog/`.
- Follow the naming convention: `YYYYMMDD[_NN]_<description>.md`
- Each task file must include the standard YAML header (`status: backlog`) and the required sections.
- Reference the review finding in the task's problem statement so the executor has full context.

## Discipline
- This is a **read-only** skill — do NOT modify source code, configuration, or infrastructure.
- The only files this skill creates are task definitions in `tasks/backlog/`.
- Group related findings into a single task when they share the same root cause.
