---
name: feature-implement
description: Execute every open task of a feature in order, stop-per-task for review. Invoke with /agn:feature-implement <slug>.
argument-hint: <feature-slug>
---

# Feature Implement (`/agn:feature-implement`)

## Arguments

| Position | Variable | Value |
|----------|----------|-------|
| `$0` | slug | Feature slug |

## Usage

```
/agn:feature-implement worktree_isolated_dev_stacks
```

## Argument validation

If `$0` is missing, **stop and ask** the user for the feature slug. Do not guess.

## Preconditions
- A feature file exists for `$0`. Confirm: `./scripts/taskman.sh feature show $0`.
- If not, stop: *Cannot run — feature not found. Create it first with `/agn:feature-create`.*

## Workflow

1. **Read the feature file** at `tasks/features/YYYYMMDD_<slug>.md`. Understand problem / objective / acceptance criteria.

2. **Enumerate member tasks**
   ```bash
   ./scripts/taskman.sh feature show $0
   ./scripts/taskman.sh list tasks --feature $0 --status backlog
   ./scripts/taskman.sh list tasks --feature $0 --status active
   ```
   Plan execution in DAG order. If the feature body numbers requirements (R1, R2, …) or lists tasks in order, honor that ordering.

3. **For each open task, in order**, run the full per-task contract by invoking the `task-implement` workflow on that task:
   - Activate (`move … active`).
   - Required reading (task file + feature file + epic file if any + architecture).
   - Detailed design → architecture compliance checkpoint → implement with tests.
   - User confirmation.
   - Append `## Summary` to the task file.
   - Complete (`move … done`).

   Run tasks **sequentially** (v1 — no parallel agent execution).

4. If a task is blocked, stop and report; do not skip silently.

5. **When every member task is in `done/`**, run **`/agn:qa-integration`** scoped to the feature, then:
   - Append a `## Summary` section to the feature file.
   - Close the feature:
     ```bash
     ./scripts/taskman.sh feature close $0
     ```
   This verifies the precondition (all members done) and sets the feature's status to `done`.

6. **Interrupts** — The user may stop at any task boundary; report what's complete and what remains.

## Discipline
- Respect scope in the feature body; flag conflicts with the user.
- Do not close the feature until the user has approved the final task and integration results.
- Bugs that surface during implementation: create them via `/agn:task-create` with `--kind bug --feature $0` so they remain traceable.
- Use `taskman.sh` for every state transition. Never `mv` or hand-edit YAML status.
