---
name: feature-create
description: Create a new feature (plan file + child task files). Invoke with /agn:feature-create. Produces one feature file under tasks/features/ and any number of tasks under tasks/backlog/, all linked via the feature slug.
argument-hint: [feature title]
---

# Feature Create (`/agn:feature-create`)

## Objective

Collaborate with the user to **define a feature** — a named product initiative — and the set of tasks needed to deliver it. Produce:

1. One **feature file** under `tasks/features/YYYYMMDD_<slug>.md` holding the plan.
2. Zero or more **task files** under `tasks/backlog/`, each stamped with `feature: <slug>`.

This skill is **requirements + planning only**. Do not implement. Do not propose code-level designs. Capture WHAT and WHY; leave HOW to `/agn:feature-implement` / `/agn:task-implement`. See `rules/first-principles.md` (Planning Guidelines).

## Preconditions

None strictly required. If the feature is large and architecture-sensitive, consider running `/agn:product-design` first to produce `docs/architecture.md`. If the user is starting from vision/spec docs under `docs/`, read them before the dialog. If the feature belongs to an existing epic, confirm the epic slug during the dialog and pass `--epic <slug>` when creating the feature.

## Workflow

### 1. Interview — problem and objective

- **Current situation** — what is true today?
- **Problem statement** — what pain, risk, or gap does the feature address?
- **Objective** — the end state. One or two sentences.
- **Who benefits and how** — users, operators, internal teams.
- Apply first-principles checks: is the problem real? what happens if we do nothing? is there a simpler shape?

### 2. Interview — scope and success

- **In scope / out of scope** — call out tempting adjacent work that will NOT be included.
- **Acceptance criteria** — testable conditions that prove the feature is complete. Include post-launch success measures here as observable conditions, not in a separate section.
- **Tasks** — ordered list of work items that deliver the feature.

Detailed requirements (R1, R2, …) belong in the **spec**, not the feature file. If the feature has no spec yet, agree with the user where it will live (`docs/<area>/.../-spec.md`) and note it in the feature body under a `## Linked spec` heading.

Skip **Risks and mitigations** unless each risk has a named owner and a mitigation plan the team will track. Otherwise the section turns into noise.

### 3. Epic attachment

- Ask whether the feature belongs to an existing epic. If yes, confirm the slug by listing candidates: `./scripts/taskman.sh list epics`.
- If no epic, the feature is stand-alone.

### 4. Propose the slug and title

Slug format: `[a-z][a-z0-9_]*` — lowercase, underscores, stable over the feature's life. Reuse in branch names and worktree names by convention.

Example: title *"Worktree-isolated dev stacks"* → slug `worktree_isolated_dev_stacks`.

Confirm both with the user.

### 5. Break the feature into tasks

List the tasks in order, each with:

- A short title
- Kind (`task` or, rarely, `bug` for a known defect rolled into the feature)
- A one-line summary of what it delivers

Keep tasks small enough that each is executable in a single focused session. Resist the temptation to design the implementation here — the task file states WHAT, not HOW.

### 6. Compose bodies

For the feature file, compose:

- `## Problem statement`
- `## Objective`
- `## Acceptance criteria`
- Recommended: `## Scope`, `## Tasks` (ordered), `## Linked spec`

Do not add `## Requirements` to the feature file — those live in the linked spec. Do not add `## Risks and mitigations` unless each risk has a named owner and active mitigation. Do not add `## Success metrics` — fold testable measures into `## Acceptance criteria`.

For each task, compose:

- `## Problem statement`
- `## Scope`
- `## Acceptance criteria`
- `## Quality gates`

These section names are required — `taskman.sh` validates them on creation.

### 7. Write drafts and preview

Write the feature first, then the tasks. `taskman.sh new` stamps each file
with `draft: true` so they are visibly mid-creation until the user approves.
Task creation will fail if the feature does not exist yet.

```bash
cat <<'EOF' | ./scripts/taskman.sh new feature --slug <slug> --title "<title>" [--epic <epic-slug>]
# feature body here
EOF
```

```bash
cat <<'EOF' | ./scripts/taskman.sh new task --title "<task title>" --feature <slug>
# task body here
EOF
```

Collect the printed paths. Re-read each file from disk and present them to
the user. Apply edits in place if requested.

### 8. Finalize or discard

On approval, finalize every draft (feature first, then tasks):

```bash
./scripts/taskman.sh finalize <feature-path>
./scripts/taskman.sh finalize <task-path>
```

On rejection of any individual file, discard it:

```bash
./scripts/taskman.sh discard <path>
```

If the user rejects the feature outright, discard the feature **and** every
task created against it — orphaned tasks pointing at a missing feature should
not be left behind.

### 9. Report and stop

Report:

- The created feature file path
- The created task file paths
- A one-sentence next step (typically: *"Ready for `/agn:feature-implement <feature-slug>`"* or per-task implementation)

Do not transition any task out of `backlog/`. That belongs to `/agn:task-implement` or `/agn:feature-implement`.

## Discipline

- Use `taskman.sh`. Never write task files directly.
- Always end the dialog with `taskman.sh finalize` (per file) or `taskman.sh discard` (per file). Do not leave files with `draft: true` behind.
- One feature per invocation. If the user describes two features, suggest splitting.
- If the user asks "how should we build it?", capture as a follow-up — implementation design is not this skill's job.

## Where the feature spec lives

If the feature needs a spec document (UI layout, field-by-field contract, API payload, etc.), place it under `docs/<area>/[<subarea>/]<name>-spec.md`. Group by product area, not by feature lifecycle. Example: `docs/settings/integration/aws-onboarding-spec.md`.

Feature files under `tasks/features/` hold the plan (problem, objective, requirements). Feature specs under `docs/<area>/...` hold the implementation contract. Link one to the other.
