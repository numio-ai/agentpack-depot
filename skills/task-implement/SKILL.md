---
name: task-implement
description: Execute implementation work for a single task. Invoke with /agn:task-implement <path>. Runs the full per-task contract — activate, read, detailed design, architecture compliance, execute, complete.
argument-hint: <task-path>
---

# Task Implement (`/agn:task-implement`)

## Arguments

| Position | Variable | Value |
|----------|----------|-------|
| `$0` | target | Task file path under `tasks/backlog/` or `tasks/active/` |

## Usage

```
# Implement a single task (ad-hoc or a specific task within a feature/epic)
/agn:task-implement @tasks/backlog/20260419_wire_up_widget.md
```

## Argument validation

If `$0` is missing, **stop and ask** the user for the task file path. Do not guess.

## Lifecycle actions — always via taskman.sh

All file moves and status transitions go through [`scripts/taskman.sh`](../../scripts/taskman.sh). **Do not** `mv` task files or edit YAML `status` by hand.

```bash
./scripts/taskman.sh move <task-path> active        # backlog → active
./scripts/taskman.sh move <task-path> done          # active  → done
./scripts/taskman.sh feature close <slug>           # close feature when all tasks done
./scripts/taskman.sh epic close <slug>              # close epic when all features done
```

---

## Preconditions
- A valid task file under `tasks/backlog/` or `tasks/active/`.
- If the task carries `feature: <slug>`, also read the matching feature file under `tasks/features/`.
- If the parent feature carries `epic: <slug>`, also read the matching epic file under `tasks/epics/`.
- For work that **changes high-level architecture**, stop and discuss with the user before coding (**architecture impact gate**).

## Required reading (re-read at task start, do not rely on prior context)
- The task file itself (`$0`).
- The parent feature file (if the task has `feature:`).
- The parent epic file (if the feature has `epic:`).
- `docs/architecture.md`.
- Relevant sections of `docs/spec.md` and `docs/requirements.md` referenced by the task.

## Execution steps

1. **Activate the task**
   - `./scripts/taskman.sh move <path> active` (does the folder move and YAML update atomically).

2. **Read and understand**
   - Read everything in the required reading list.
   - **Detailed design first** — API shapes, data touched, interfaces, key logic — before any code.

3. **Architecture compliance checkpoint**
   - Verify the detailed design against `docs/architecture.md` constraints.
   - If it violates any constraint, flag to the user before coding — do not proceed until resolved.

4. **Execute**
   - Ask for clarification if the task is ambiguous.
   - For multi-step or risky work, get user approval before executing.
   - Implement, then write unit tests as appropriate.
   - Stay within task scope; flag scope creep.

5. **Complete (after user confirms)**
   - Append a `## Summary` section to the task file describing steps completed, changes made, notable decisions, links.
   - `./scripts/taskman.sh move <active-path> done`.
   - If the task had `feature: <slug>` and this was the last open task, offer to run `./scripts/taskman.sh feature close <slug>`.

6. **Documentation** — If behavior or intent in docs is wrong or incomplete, update `docs/*` (dependency order: vision → spec → requirements → architecture → tasks).

## After single-task flows (bugfix / ad-hoc)
When the user expects integration coverage, run **`/agn:qa-integration`** for the affected scope before hand-off. Full regression uses **`/agn:qa-system`**.

## Discipline
- Use `taskman.sh` for every state transition. Never `mv` or hand-edit YAML status.
- Never expand scope beyond the task body without user instruction.
- Append the `## Summary` section before moving to `done` — required by the task-management standard.
