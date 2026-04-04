# spec-to-code

Claude Code plugin for AI-assisted SDLC: definition, architecture, planning, implementation, QA.


## Install


## Getting started

1. Install the plugin. 

```bash
/plugin install spec-to-code@<marketplace-name>
```

or install from local directory:
```bash
claude --plugin-dir /path/to/spec-to-code
```

2. The `stc` agent activates automatically and loads behavioral rules. If your setup cannot use the `stc` agent, run `/stc:init` at session start instead.

3. Use plugin skills to run the workflows. Run `/stc:define-product` to start a new product, or `/stc:create-task` for a one-off task or bug ticket. See "Skills" section below for more details.

## Skills

### SDLC workflow (run in order for new products)

| Step | Command | What it does |
|------|---------|--------------|
| 0. Init (if no agent) | `/stc:init` | Load rules into session context |
| 1. Definition | `/stc:define-product` | Vision, spec, requirements in `docs/` |
| 2. Architecture | `/stc:design` | `docs/architecture.md` |
| 3. Planning | `/stc:plan` | `docs/implementation-plan.md` + task files in `tasks/backlog/` |
| 4. Implementation | `/stc:implement-task <task>` | One task: detailed design, code, tests |
| | `/stc:implement-phase <N>` | All tasks in phase N (sequential) |
| | `/stc:implement-plan` | Full plan, phase by phase |
| 5. Integration test | `/stc:qa-integration-test` | Verify phase or ad-hoc work + regression |
| 6. System test | `/stc:qa-system-test` | Full product test, release gate |

### Task and maintenance

| Command | What it does |
|---------|--------------|
| `/stc:create-task` | New task or bug ticket in `tasks/backlog/` (dev tasks, bugs, refactors, chores) |
| `/stc:codebase-review` | Read-only audit, produces backlog tasks |

### Utilities

| Command | What it does |
|---------|--------------|
| `/stc:commit-code` | Staging and git commit guidance |
| `/stc:comment-code` | Add explanatory comments to code |

## Workflows supported

- **New product** — `/stc:define-product` → `/stc:design` → `/stc:plan` → `/stc:implement-*` → `/stc:qa-system-test`
- **Bug fix** — `/stc:create-task bug` → `/stc:implement-task` → `/stc:qa-integration-test` → `/stc:qa-system-test`
- **Maintenance** — `/stc:create-task task` → `/stc:implement-task` → `/stc:qa-integration-test`
- **Incremental feature** — same as new product, but `/stc:define-product` amends existing docs
- **Codebase optimization** — `/stc:codebase-review` → `/stc:plan` → `/stc:implement-phase` → `/stc:qa-system-test`

## Project layout created by workflows

```
docs/vision.md
docs/spec.md
docs/requirements.md
docs/architecture.md
docs/implementation-plan.md
tasks/backlog/ | active/ | done/
```

Created incrementally as you run each stage — no upfront scaffolding.

## How rules are loaded

| Mechanism | How | Survives compaction | When to use |
|-----------|-----|---------------------|-------------|
| `stc` agent (default) | Automatic via `settings.json` | Yes | Recommended for most users |
| `/stc:init` | Manual, run once per session | No | When agent conflicts with another plugin |

Rules live in `rules/first-principles.md` and `rules/task-management.md`. The agent inlines them in its system prompt; `/stc:init` injects them via `!cat`.

## Development and testing

```bash
# Load the plugin locally without installing:
claude --plugin-dir ./

# After making changes, reload inside Claude Code:
/reload-plugins
```

See [`docs/spec-to-code-specification.md`](docs/spec-to-code-specification.md) for the full product vision and functional specification.
