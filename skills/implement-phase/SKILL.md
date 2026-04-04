---
name: implement-phase
description: Run all tasks in a numbered phase from the implementation plan, sequentially. Invoke with /stc:implement-phase N. Integration test at phase end via stc:qa-integration-test.
---

# Implement Phase (`/stc:implement-phase`)

## Preconditions
- `docs/implementation-plan.md` exists and lists phases and tasks.
- Task files exist under `tasks/backlog/` (or paths referenced by the plan).

If no plan: *Cannot run `/stc:implement-phase` — complete `/stc:plan` first.*

## Usage
```
/stc:implement-phase 1
```

## Workflow

1. Read `docs/implementation-plan.md` and identify **phase `<N>`** and its tasks in DAG order (respect dependencies).

2. For **each task** in the phase, execute the **`/stc:implement-task`** workflow (activate → detailed design → implement → tests → move to `done` when user confirms). **v1:** run tasks **sequentially** (no parallel agent execution).

3. If a task is blocked, stop and report; do not skip silently.

4. **Phase boundary** — After all tasks in the phase are `done`, run **`/stc:qa-integration-test`** with scope = this phase + regression against prior phases.

5. **Gate** — User reviews integration results before starting the next phase.

## Discipline
- Respect scope in the plan; flag conflicts with the user.
- Do not start the next phase without user approval.
