---
name: implement-plan
description: Execute every remaining task from the implementation plan across all phases in order. Invoke with /stc:implement-plan. Runs stc:qa-integration-test at each phase boundary.
---

# Implement All (`/stc:implement-plan`)

## Preconditions
- `docs/implementation-plan.md` exists.
- Task files exist per the plan.

If missing: *Cannot run `/stc:implement-plan` — complete `/stc:plan` first.*

## Usage
```
/stc:implement-plan
```

## Workflow

1. Read `docs/implementation-plan.md` and enumerate phases in order.

2. For **each phase**, execute the same logic as **`/stc:implement-phase`**: all tasks in dependency order using **`/stc:implement-task`**, then **`/stc:qa-integration-test`** at the phase boundary.

3. **Interrupts** — The user may stop after any phase; report what is complete and what remains.

4. **v1:** tasks run **sequentially** within a phase (no parallel execution).

5. When all planned tasks are `done`, point the user to **`/stc:qa-system-test`** for full product verification.

## Discipline
- Same scope and architecture-impact rules as `/stc:implement-task`.
- Do not expand scope beyond the approved plan without user instruction.
