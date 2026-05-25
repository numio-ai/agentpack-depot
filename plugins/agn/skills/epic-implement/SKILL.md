---
name: epic-implement
description: Execute every linked feature of an epic in order. Iterates feature-implement for each, with integration tests at feature boundaries. Invoke with /agn:epic-implement <slug>.
argument-hint: <epic-slug>
---

# Epic Implement (`/agn:epic-implement`)

## Arguments

| Position | Variable | Value |
|----------|----------|-------|
| `$0` | slug | Epic slug |

## Usage

```
/agn:epic-implement aws_onboarding
```

## Argument validation

If `$0` is missing, **stop and ask** the user for the epic slug. Do not guess.

## Preconditions
- An epic file exists for `$0` at `tasks/epics/YYYYMMDD_<slug>.md`. Confirm: `./scripts/taskman.sh list epics`.
- If not, stop: *Cannot run — epic not found. Create it first with `/agn:epic-create`.*
- The epic body's `## Linked features` lists the features that compose the epic, in execution order.

## Workflow

1. **Read the epic file** at `tasks/epics/YYYYMMDD_<slug>.md`. Understand problem / objective / scope / acceptance criteria / linked features.

2. **Enumerate member features**
   ```bash
   ./scripts/taskman.sh list features --epic $0 --status backlog
   ./scripts/taskman.sh list features --epic $0 --status active
   ```
   Cross-check the enumerated features against the epic body's `## Linked features` ordering. If the body has a different order than the slug list, honor the body ordering.

3. **For each open feature, in order**, run the full per-feature contract by invoking the `feature-implement` workflow on that feature:
   - Read the feature file and its linked tasks.
   - Iterate through each task using the `task-implement` contract.
   - User confirmation per task.
   - Append `## Summary` to each task file.
   - When every task of the feature is `done`, run **`/agn:qa-integration`** scoped to that feature.
   - Append `## Summary` to the feature file.
   - Close the feature: `./scripts/taskman.sh feature close <feature-slug>`.

   Run features **sequentially**. Within a feature, tasks also run sequentially (v1 — no parallel agent execution).

4. If a feature is blocked, stop and report; do not skip silently.

5. **Feature boundary gate** — After each feature is closed and its integration test passes, the user reviews the results before the next feature starts. Do not auto-advance.

6. **When every linked feature is in `done`**, run **`/agn:qa-integration`** scoped to the whole epic (cross-feature interactions), then:
   - Append a `## Summary` section to the epic file.
   - Close the epic:
     ```bash
     ./scripts/taskman.sh epic close $0
     ```
   This verifies the precondition (all linked features done) and sets the epic's status to `done`.

7. **Interrupts** — The user may stop at any feature boundary; report what's complete and what remains.

8. After the epic closes, point the user to **`/agn:qa-system`** for full product verification if the epic represents a release-ready slice.

## Discipline
- Respect scope in the epic body; flag conflicts with the user.
- Do not start the next feature without user approval.
- Do not expand scope beyond the approved epic without user instruction.
- Same architecture-impact rules as `task-implement` and `feature-implement` — any architectural change requires a stop-and-discuss before coding.
- Use `taskman.sh` for every state transition. Never `mv` or hand-edit YAML status.
