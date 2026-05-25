---
name: task-create
description: Create a new task file in tasks/backlog/ — either a development task or a bug ticket, optionally attached to a feature. Invoke with /agn:task-create. Pass type (task | bug) as an argument.
argument-hint: task <title> | bug <title>
---

# Task Create (`/agn:task-create`)

## Usage

```
/agn:task-create                         # Claude will ask for the task type
/agn:task-create task                    # Development task
/agn:task-create bug                     # Bug ticket
/agn:task-create task <short title>
/agn:task-create bug  <short title>
```

If the task belongs to an existing feature, mention the feature slug during the dialog — it becomes `feature: <slug>` in the YAML header. If you don't know the slug, ask: *"Does this task belong to a feature? If yes, which slug under `tasks/features/`?"* A task without a feature is **ad-hoc** — perfectly valid.

## Objective

Collaborate with the user to **define a task** with clear, non-ambiguous requirements that can be executed later — product-manager style, not implementation design.

**This skill is requirements definition only:**
- Do not implement the task.
- Do not make code/config changes beyond writing the task file (via `taskman.sh`).
- Do not propose or evaluate solutions. If the user asks "how should we build it?", capture that as a follow-up for `/agn:task-implement`.

## Task Type

If the user did not provide a type argument, ask:
> *"What type — development task or bug ticket?"*

Accept `task` / `feature` / `dev` as task; `bug` / `defect` / `fix` as bug.

---

## Development Task — Collaboration Process

### 1. Establish the problem (and whether it's real)

- Current situation — what is happening today?
- Problem statement — the concrete pain, failure, or risk.
- Evidence — errors, logs, screenshots, links.
- Impact — who is affected and how often.
- Who is the user/system experiencing the problem? What decision or workflow is blocked?

Apply first-principles checks:
- Do we understand what problem we solve?
- Is it really a problem (not just preference)?
- What happens if we don't do it?

### 2. Feature attachment

- Ask whether the task belongs to an existing feature. If yes, confirm the slug by listing candidates: `./scripts/taskman.sh list features`.
- If no feature, the task is ad-hoc.

### 3. Specify scope precisely

- In-scope — what must change, where, and what must be true afterward.
- Out-of-scope / non-goals — adjacent work that will NOT be included.
- Dependencies — environments, hosts, repos, external services.
- Constraints — security, availability, performance, deadlines.
- Assumptions — what we assume true, and how we will validate.

Iterate until the task is executable without guesswork.

### 4. Define success

- Acceptance criteria — specific, observable outcomes.
- Success metrics (optional) — what metric moves, in what direction.

Do not choose implementation approach here.

### 5. Define validation and rollback

- Quality gates — the exact commands and/or manual steps that must pass.
- Rollback plan — how to revert safely (when relevant).

### 6. Compose the body

Required sections (validated by `taskman.sh`):
- `## Problem statement`
- `## Scope`
- `## Acceptance criteria`
- `## Quality gates`

Recommended: `## Constraints and assumptions`, `## Risks and rollback`.

### 7. Write the draft and preview

Write the task file directly. `taskman.sh new` stamps the YAML with
`draft: true` so the file is clearly mid-creation until the user approves.

```bash
cat <<'EOF' | ./scripts/taskman.sh new task --title "<title>" [--feature <slug>]
# body with the required sections
EOF
```

Capture the printed path. Re-read the file from disk, show it to the user,
confirm title / feature attachment / body. Offer edits — apply them by editing
the file in place.

### 8. Finalize or discard

On approval, clear the draft marker:

```bash
./scripts/taskman.sh finalize <path>
```

On rejection, delete the draft:

```bash
./scripts/taskman.sh discard <path>
```

Report the final path to the user. Stop. User will run `/agn:task-implement <path>` when ready.

---

## Bug Ticket — Collaboration Process

### 1. Observed problem

What is broken, where, and what is the user/system impact?

### 2. Expected vs actual

Clear contrast between correct behaviour and current.

### 3. Reproduction steps

Minimal, numbered, reliable.

### 4. Context

Environment, version, logs, screenshots, links.

### 5. Feature attachment

Bugs can be attached to a feature (post-merge follow-up) or left ad-hoc. Ask.

### 6. Compose the body

Same required sections as a task — `## Problem statement`, `## Scope`, `## Acceptance criteria`, `## Quality gates`. Fill each with bug-appropriate content:
- Problem statement: observed vs expected.
- Scope: what is and is not included in the fix.
- Acceptance criteria: the reproduction no longer reproduces; regression test added.
- Quality gates: the exact validation commands.

### 7. Write the draft, preview, finalize

```bash
cat <<'EOF' | ./scripts/taskman.sh new task --kind bug --title "<title>" [--feature <slug>]
# body
EOF
```

Re-read the file from disk and show the user. On approval run
`./scripts/taskman.sh finalize <path>`; on rejection run
`./scripts/taskman.sh discard <path>`. Report the final path, stop.

---

## Discipline

- Use `taskman.sh`. Never write task files directly.
- Always end the dialog with either `taskman.sh finalize` or `taskman.sh discard`. Do not leave files with `draft: true` behind.
- Never choose implementation solutions — capture requirements and validation only.
- For maintenance types (`refactor`, `upgrade`, `chore`), use the task flow; scope tightly.
- Stop as soon as the file is written and the path is reported. No execution follows from this skill.
- If the task implements something defined in a spec under `docs/<area>/.../-spec.md`, reference the spec rather than restating its requirements. Spec sections drift if duplicated.
