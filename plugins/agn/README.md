# agn — Agentic SDLC

Agentic software-delivery loop for Claude Code. Claude facilitates writing specs and designing architecture, then autonomously builds implementation plans, implements, and tests — all through structured `/agn:*` skills with built-in review gates and document consistency.

## Install

From the `agenture` marketplace:

```
/plugin marketplace add AgentureHQ/agenture-loop
/plugin install agn@agenture
```

## Work-unit hierarchy

The plugin supports a four-tier hierarchy. Each tier is optional one level up — a task may have no feature, and a feature may have no epic.

```
product → epic → feature → task
```

| Tier | Purpose | File location |
|------|---------|---------------|
| **Product** | Vision, specification, requirements | `docs/vision.md`, `docs/spec.md`, `docs/requirements.md` |
| **Epic** | Functional block larger than a feature; decomposes into features | `tasks/epics/YYYYMMDD_<slug>.md` |
| **Feature** | Named product initiative; produces a plan and child tasks | `tasks/features/YYYYMMDD_<slug>.md` |
| **Task** | Unit of implementation work | `tasks/<state>/YYYYMMDD_<slug>.md` (where state is `backlog`, `active`, or `done`) |

Bugs are tasks with `kind: bug`. Same lifecycle. Can be attached to a feature or ad-hoc.

See `rules/task-management.md` for the full model.

## Skills

The plugin ships 14 skills, namespaced as `/agn:<scope>-<action>`. Skills group by scope under tab completion.

### SDLC workflow (run in order for new products)

| Scope | Skill | What it does |
|-------|-------|--------------|
| Product | `/agn:product-define` | Vision, spec, requirements in `docs/` |
| Product | `/agn:product-design` | `docs/architecture.md` |
| Epic | `/agn:epic-create` | Epic file + linked feature files |
| Epic | `/agn:epic-implement` | Execute every linked feature of an epic in order |
| Feature | `/agn:feature-create` | Feature file + linked task files |
| Feature | `/agn:feature-implement` | Execute every open task of a feature in order |
| Task | `/agn:task-create` | Task or bug ticket in `tasks/backlog/` |
| Task | `/agn:task-implement` | Execute a single task: detailed design → code → tests |
| QA | `/agn:qa-integration` | Integration test for a feature, epic boundary, or ad-hoc work |
| QA | `/agn:qa-system` | Full product system test before release |

### Code and maintenance

| Skill | What it does |
|-------|--------------|
| `/agn:code-review` | Read-only codebase audit; produces backlog tasks |
| `/agn:code-comment` | Add explanatory comments to source code |
| `/agn:code-commit` | Stage files and write a well-formed git commit message |

## Workflows supported

- **New product** — `/agn:product-define` → `/agn:product-design` → `/agn:epic-create` (or `/agn:feature-create` if no epic tier needed) → `/agn:epic-implement` / `/agn:feature-implement` → `/agn:qa-system`
- **Bug fix** — `/agn:task-create bug` → `/agn:task-implement` → `/agn:qa-integration` → `/agn:qa-system`
- **Ad-hoc maintenance** — `/agn:task-create` → `/agn:task-implement` → `/agn:qa-integration`
- **Incremental feature** — `/agn:feature-create` (against existing product docs) → `/agn:feature-implement` → `/agn:qa-integration`
- **Codebase optimization** — `/agn:code-review` → `/agn:feature-create` (or `/agn:epic-create` for larger scope) → `/agn:feature-implement` → `/agn:qa-system`

## taskman.sh — task and feature CLI

All create / move / close / list operations on epics, features, and tasks go through `scripts/taskman.sh`. Skills compose content in dialog with the user, then hand off to taskman as the save step. Do not write task files directly.

```bash
./scripts/taskman.sh help                                # full surface
./scripts/taskman.sh new epic    --slug <s> --title T < body
./scripts/taskman.sh new feature --slug <s> --title T [--epic <s>] < body
./scripts/taskman.sh new task    --title T [--feature <s>] [--kind task|bug] < body
./scripts/taskman.sh finalize <path>                     # clear draft: true
./scripts/taskman.sh discard  <path>                     # delete a draft
./scripts/taskman.sh move <task-path> <backlog|active|done>
./scripts/taskman.sh list epics    [--status backlog|active|done]
./scripts/taskman.sh list features [--epic <s>] [--status backlog|active|done]
./scripts/taskman.sh list tasks    [--feature <s>] [--status backlog|active|done] [--kind task|bug]
./scripts/taskman.sh epic show    <slug>
./scripts/taskman.sh epic close   <slug>                 # fails unless all member features in done
./scripts/taskman.sh feature show  <slug>
./scripts/taskman.sh feature close <slug>                # fails unless all member tasks in done
./scripts/taskman.sh validate
```

`epic close` and `feature close` enforce the lifecycle rule: an epic can only close when every feature with matching `epic: <slug>` is in `done`, and a feature can only close when every task with matching `feature: <slug>` is in `done`.

## Project layout created by workflows

```
docs/vision.md
docs/spec.md
docs/requirements.md
docs/architecture.md
docs/<area>/.../-spec.md         # feature-scoped specs
tasks/
  epics/                          # YYYYMMDD_<slug>.md
  features/                       # YYYYMMDD_<slug>.md
  backlog/                        # YYYYMMDD[_NN]_<slug>.md
  active/
  done/
```

Created incrementally as you run each stage — no upfront scaffolding.

## Loading rules into your project

Plugins do not auto-load `rules/`. To activate the behavioral rules in your project, append this block to your project's `CLAUDE.md`:

```
@~/.claude/plugins/cache/agenture/agn/0.1.0/rules/first-principles.md
@~/.claude/plugins/cache/agenture/agn/0.1.0/rules/task-management.md
@~/.claude/plugins/cache/agenture/agn/0.1.0/rules/writing-guideline.md
```

Update the version (`0.1.0`) when you upgrade the plugin.

The three files:

- `first-principles.md` — design discipline (YAGNI, KISS, DRY), surgical changes, goal-driven execution.
- `task-management.md` — epic/feature/task hierarchy, lifecycle, frontmatter shapes.
- `writing-guideline.md` — crisp, no-fluff prose style.

Interim mechanism. A planned change will load each rule only when a relevant skill runs (tracked in backlog: `split_task_management_rules`).

## Plugin layout

```
plugins/agn/
├── .claude-plugin/plugin.json    # plugin manifest
├── skills/                       # 14 /agn:* skills
├── rules/                        # first-principles, task-management, writing-guideline
├── scripts/taskman.sh            # task lifecycle CLI
└── README.md
```
