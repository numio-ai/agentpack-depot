---
name: create-task
description: Create a new task file in tasks/backlog/ — either a development task or a bug ticket. Invoke with /stc:create-task. Optionally pass type as an argument (task | bug).
---

# Create Task (`/stc:create-task`)

## Usage
```
/stc:create-task             # Claude will ask for the task type
/stc:create-task task        # Create a development task
/stc:create-task bug         # Create a bug ticket
/stc:create-task task <short title or topic>
/stc:create-task bug <short title or topic>
```

You may also target a path explicitly: `/stc:create-task @tasks/backlog/YYYYMMDD_description.md`

## Objective
Collaborate with the user to **define a task** with **clear, non-ambiguous requirements** that can be executed later — **product-manager style**, not implementation design.

**This skill is requirements definition only:**
- Do **not** implement the task.
- Do **not** make code/config changes beyond writing the task file.
- Do **not** propose or evaluate solutions/approaches. If the user asks "how should we build it?", capture that as a follow-up to be handled later under execution/planning.

## Task Type

If the user does not provide a type argument, ask:
> "What type of task would you like to create — **development task** or **bug ticket**?"

Accept `task` / `feature` / `dev` as development task; accept `bug` / `defect` / `fix` as bug ticket.

---

## Development Task — Collaboration Process

### 1. Establish the problem (and whether it's real)
Ask for:
- **Current situation** — what is happening today?
- **Problem statement** — what is the concrete pain / failure / risk?
- **Evidence** — errors, log snippets, screenshots, links to docs, etc.
- **Impact** — who is affected and how often?
- **Who is the user or system** experiencing the problem? What decision or workflow is blocked/degraded?

Apply these checks (ask explicitly if unclear):
- **Do we understand what problem we solve?**
- **Is it really a problem?** (or just a preference / "nice to have")
- **What happens if we don't do it?** (cost of inaction, risks, opportunity cost)

### 2. Specify scope precisely
Convert goals into precise statements:
- **In-scope** — what must change (and where), what must be true afterward
- **Out-of-scope / Non-goals** — what we explicitly will not do; call out tempting adjacent work
- **Dependencies** — environments, hosts, repos/submodules, external services
- **Constraints** — security, availability, performance, backwards compatibility, deadlines
- **Assumptions** — what we assume is true and how we will validate those assumptions

Identify missing details and ask targeted clarification questions until the task is **executable without guesswork**.

### 3. Define success
- **Success metrics** (optional) — what metric should move, in what direction? (e.g., reduce failures, shorten time-to-deploy, reduce manual steps)
- **Acceptance criteria** — specific, observable outcomes that must be true when done
- **Non-goals** — explicitly list adjacent work that will not be included

Do not choose an implementation approach here.

### 4. Define validation and rollback
- **Validation steps** — exact commands and/or manual steps to confirm success
- **Rollback plan** — how to revert safely (when relevant)

### 5. Write the task file
Create under `tasks/backlog/YYYYMMDD[_NN]_<description>.md`:

```yaml
---
status: backlog
type: task
---
```

Sections:
- Title
- Date
- Context
- Problem
- Goals
- Non-goals
- Constraints & assumptions
- Success metrics *(optional)*
- Acceptance criteria
- Validation steps
- Risks & rollback

### 6. Confirm
Stop after the file is written. User confirms the task is ready for **`/stc:implement-task`** (or `/stc:plan` if part of a larger plan). Do not proceed to implementation.



---

## Bug Ticket — Collaboration Process

### 1. Observed problem
What is broken, where, and what is the user/system impact?

### 2. Expected vs actual
Clear contrast between correct behaviour and current behaviour.

### 3. Reproduction steps
Minimal, numbered steps to reproduce reliably.

### 4. Context
Environment, version, logs, screenshots, links as available.

### 5. Write the task file
Create under `tasks/backlog/YYYYMMDD[_NN]_<description>.md`:

```yaml
---
status: backlog
type: bug
---
```

Sections:
- Title
- Date
- Observed problem
- Expected vs actual
- Reproduction steps
- Environment & context
- Risks & impact

### 6. Confirm
Stop after the file is written. User confirms the ticket is complete and actionable before moving to **`/stc:implement-task`**.

---

## Discipline
- Never choose implementation solutions; capture requirements and validation only.
- For maintenance types (`refactor`, `upgrade`, `chore`), use the development task flow; scope tightly.
- Stop as soon as the definition is written and confirmed. No execution follows from this skill.
