---
name: implement-task
description: Execute a single task — detailed design, implementation, tests. Invoke with /stc:implement-task and a task file path. For planned work, defects, and ad-hoc tasks.
---

# Implement Task (`/stc:implement-task`)

## Usage
```
/stc:implement-task @tasks/backlog/20260210_add_feature.md
/stc:implement-task $ARGUMENTS[0]
```

## Preconditions
- A valid task file under `tasks/backlog/` or `tasks/active/` (or path given by user).
- For work that **changes high-level architecture**, stop and discuss with the user before coding (**architecture impact gate**).

## Execution Steps

1. **Activate the task**
   - Move the task file to `tasks/active/` if it is in `tasks/backlog/`
   - Set `status: active` in the task file YAML header

2. **Read and understand the task**
   - Read the task file
   - **Detailed design first** — API shapes, data touched, interfaces, key logic — before writing code (informed by `docs/architecture.md` and the task text)

3. **Execute the task**
   - Ask for clarification if the task is ambiguous
   - For multi-step or risky work, get user approval before executing
   - Implement, then unit tests as appropriate
   - Stay within task scope; flag scope creep to the user

4. **Complete the task (after user confirms)**
   - Set `status: done` in the task file YAML header
   - Move the task file to `tasks/done/`

5. **Documentation** — If behavior or intent in docs is wrong or incomplete, update the relevant `docs/*` files and say what changed (dependency order: vision → spec → requirements → architecture → implementation plan → tasks).

## After single-task flows (bugfix / ad-hoc)
When the user expects integration coverage, run **`/stc:qa-integration-test`** for the affected scope before hand-off. Full regression uses **`/stc:qa-system-test`**.
