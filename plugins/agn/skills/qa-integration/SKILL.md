---
name: qa-integration
description: Integration testing — verify new work works with the running app and prior phases/regressions. Invoke after a feature, epic phase, or after ad-hoc /agn:task-implement when integration coverage is needed.
---

# QA Integration (`/agn:qa-integration`)

## Purpose
Validate that:
1. **New functionality** delivered in the current scope behaves end-to-end.
2. **Existing functionality** (prior features or related areas) still works.

## Preconditions
- A runnable app or test environment the project uses (tests, dev server, etc.).
- Enough implementation exists to exercise the scope (otherwise say what is missing).

## Output
Write an **integration test report** (markdown — `docs/` or user-chosen path), including:
- Scope (epic slug, feature slug, task IDs, or "ad-hoc: ...")
- What was executed (commands, URLs, scenarios)
- Pass/fail per scenario
- Regressions found and severity
- Open issues

## Usage
```
/agn:qa-integration
```
Optionally scope in the user message: *"feature worktree_isolated_dev_stacks only"*, *"after defect TASK-xxx"*, etc.

## Workflow

1. Infer scope from context (recent feature, epic, user instruction, or task just completed).

2. Run the project's integration/e2e tests if present; supplement with **manual checks** where needed.

3. Focus on **interfaces between components** and **realistic user paths** for this slice of work.

4. Do not claim release readiness — that is **`/agn:qa-system`**.

## Discipline
- Fix failures you can within scope; escalate product decisions.
- Keep the report factual and actionable.
