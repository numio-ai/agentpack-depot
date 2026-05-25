---
name: epic-create
description: Create a new epic (functional-block-sized plan + linked features). Invoke with /agn:epic-create. Produces one epic file under tasks/epics/ and zero or more feature files under tasks/features/, all linked via the epic slug.
argument-hint: [epic title]
---

# Epic Create (`/agn:epic-create`)

## Objective

Collaborate with the user to **define an epic** — a named functional block larger than a feature — and the set of features needed to deliver it. Produce:

1. One **epic file** under `tasks/epics/YYYYMMDD_<slug>.md` holding the functional-block-level plan.
2. Zero or more **feature files** under `tasks/features/`, each stamped with `epic: <slug>`.

This skill is **requirements + decomposition only**. Do not implement. Do not propose code-level designs. Capture WHAT and WHY at functional-block level; leave HOW to `/agn:feature-create` and `/agn:feature-implement` for each feature, and `/agn:task-implement` for each task within. See `rules/first-principles.md` (Planning Guidelines).

## Preconditions

None strictly required. If the epic is architecture-sensitive, consider running `/agn:product-design` first to produce `docs/architecture.md`. If the user is starting from vision/spec docs under `docs/`, read them before the dialog.

## When to create an epic vs a feature

Create an **epic** when the work spans multiple features that share a coherent objective at the functional-block level. Examples: "AWS onboarding", "Authentication & authorization", "Cost ingestion pipeline".

Create a **feature** directly via `/agn:feature-create` when the work fits inside a single coherent slice that produces tasks at one level of depth. Most product work is a feature, not an epic. Epics are for genuinely larger blocks where decomposition by feature adds clarity.

If unclear, ask the user.

## Workflow

### 1. Interview — problem and objective

- **Current situation** — what is true today at the functional-block level?
- **Problem statement** — what pain, risk, or gap does the epic address?
- **Objective** — the end state at the functional-block level. One or two sentences.
- **Who benefits and how** — users, operators, internal teams.
- Apply first-principles checks: is the problem real? what happens if we do nothing? is there a simpler shape (one feature instead of an epic)?

### 2. Interview — scope and success

- **In scope / out of scope** — call out adjacent functional blocks that will NOT be included.
- **Acceptance criteria** — observable conditions at functional-block level that prove the epic is complete. These are coarser than feature-level acceptance criteria.

Skip **Risks and mitigations** unless each risk has a named owner and a mitigation plan the team will track.

### 3. Propose the slug and title

Slug format: `[a-z][a-z0-9_]*` — lowercase, underscores, stable over the epic's life. Reuse in branch names by convention.

Example: title *"AWS onboarding"* → slug `aws_onboarding`.

Confirm both with the user.

### 4. Break the epic into features

List the features in order, each with:

- A short title (will become the feature slug)
- A one-line summary of what it delivers
- Whether you will create the feature file now (`--epic <slug>` flag) or defer until later

Keep features small enough that each is itself a coherent slice with its own tasks. Resist the temptation to design the implementation here.

### 5. Compose the epic body

Required sections:

- `## Problem statement`
- `## Objective`
- `## Scope`
- `## Acceptance criteria`
- `## Linked features` — ordered list of feature titles or slugs that compose this epic

Do not add `## Requirements` to the epic file — those live in feature specs or `docs/<area>/.../-spec.md`. Do not add `## Risks and mitigations` unless each risk has a named owner and active mitigation. Do not add `## Success metrics` — fold testable measures into `## Acceptance criteria`.

### 6. Write drafts and preview

Write the epic first, then any feature files the user wants created now. `taskman.sh new` stamps each file with `draft: true` so they are visibly mid-creation until the user approves. Feature creation will fail if the epic does not exist yet.

```bash
cat <<'EOF' | ./scripts/taskman.sh new epic --slug <slug> --title "<title>"
# epic body here
EOF
```

```bash
cat <<'EOF' | ./scripts/taskman.sh new feature --slug <feature-slug> --title "<feature title>" --epic <epic-slug>
# feature body here
EOF
```

Collect the printed paths. Re-read each file from disk and present them to the user. Apply edits in place if requested.

### 7. Finalize or discard

On approval, finalize every draft (epic first, then features):

```bash
./scripts/taskman.sh finalize <epic-path>
./scripts/taskman.sh finalize <feature-path>
```

On rejection of any individual file, discard it:

```bash
./scripts/taskman.sh discard <path>
```

If the user rejects the epic outright, discard the epic **and** every feature created against it.

### 8. Report and stop

Report:

- The created epic file path
- The created feature file paths (if any)
- A one-sentence next step (typically: *"Ready for `/agn:epic-implement <epic-slug>`"* — or *"Run `/agn:feature-create --epic <epic-slug>`"* if features are still to be defined)

Do not transition any feature out of `backlog`. That belongs to `/agn:feature-implement` (or `/agn:epic-implement`, which iterates features).

## Discipline

- Use `taskman.sh`. Never write epic or feature files directly.
- Always end the dialog with `taskman.sh finalize` (per file) or `taskman.sh discard` (per file). Do not leave files with `draft: true` behind.
- One epic per invocation. If the user describes two epics, suggest splitting.
- If the user asks "how should we build it?", capture as a follow-up — implementation design is not this skill's job.
- If the user describes work that fits in a single feature, suggest `/agn:feature-create` instead.
