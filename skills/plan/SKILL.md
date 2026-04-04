---
name: plan
description: Planning stage — produce implementation plan and task files in tasks/backlog/. Invoke with /stc:plan after architecture exists.
---

# Plan (`/stc:plan`)

## Preconditions
**Required before starting:**
- `docs/vision.md`, `docs/spec.md`, `docs/requirements.md`
- `docs/architecture.md`

If architecture is missing, stop: *Cannot run `/stc:plan` — `docs/architecture.md` not found. Run `/stc:design` first.*

## Outputs
| Artifact | Path |
|----------|------|
| Implementation plan (DAG, phases) | `docs/implementation-plan.md` |
| Task files | `tasks/backlog/*.md` per task in the plan |

Each task file MUST follow the task management standard (YAML `status`, problem, scope, acceptance criteria, quality gates).

## Usage
```
/stc:plan
```

## Workflow

1. Read all definition + architecture docs.

2. Draft `docs/implementation-plan.md`: phases, tasks, dependencies (DAG), ordering notes, and what each task delivers.

3. For each task in the plan, create `tasks/backlog/YYYYMMDD[_NN]_<description>.md` with `status: backlog` and full structure per the task management standard.

4. **Validation** — Report: each task is unambiguous and executable without guesswork; coverage cross-check against spec/requirements.

5. **Consistency** — If planning reveals scope changes, update `docs/spec.md` / `docs/requirements.md` / `docs/architecture.md` as needed and document why.

6. **Gate** — User approves the plan before implementation skills run.

## Discipline
- Task naming: `YYYYMMDD[_NN]_<description>.md` (lowercase, underscores).
- Do not mark tasks `active` or `done` here; that happens under `/stc:implement-task`.
