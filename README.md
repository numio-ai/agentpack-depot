# Agenture — Claude Code Plugin Marketplace

A marketplace of Claude Code plugins for AI-assisted software development.

Agenture moves teams from ad-hoc AI use to a repeatable, review-gated lifecycle: **you write the specs and approve each gate, AI drafts the design, code, and tests from those specs.** The `agn` plugin implements that loop inside Claude Code.

## Install

Add the marketplace to Claude Code once, then install any plugin from it:

```
/plugin marketplace add AgentureHQ/agenture-loop
/plugin install agn@agenture
```

Restart Claude Code (or run `/reload-plugins`) and the `/agn:*` skills become available.

## Available plugins

| Plugin | What it does | Docs |
|--------|--------------|------|
| `agn` | Agentic SDLC loop — spec, design, plan, implement, and test through structured `/agn:*` skills with built-in review gates | [plugins/agn/README.md](plugins/agn/README.md) |

## How it works

The `agn` plugin structures work as a four-tier hierarchy. Each tier is optional one level up — a task can stand alone, a feature can exist without an epic.

```
product → epic → feature → task
```

Every stage follows the same contract: **you define WHAT and WHY, the agent derives HOW, and a review gate stands between stages.** Specs and plans hold requirements and acceptance criteria — never implementation code. The agent generates implementation from the spec, not from a ticket title or chat history.

## Using the skills

Skills are namespaced `/agn:<scope>-<action>`. Run them in sequence for the workflow you need:

**New product**
```
/agn:product-define     # vision, spec, requirements → docs/
/agn:product-design     # architecture → docs/architecture.md
/agn:epic-create        # epic + linked feature files   (or /agn:feature-create directly)
/agn:feature-implement  # execute each task, stop per task for your review
/agn:qa-system          # full system test before release
```

**Single feature against existing docs**
```
/agn:feature-create     # feature plan + child tasks in tasks/backlog/
/agn:feature-implement  # implement each task in order
/agn:qa-integration     # verify it works with the running app and prior work
```

**One task or bug fix**
```
/agn:task-create        # define a task or bug — requirements only, no design
/agn:task-implement     # detailed design → code → tests
/agn:qa-integration
```

**Maintenance**
```
/agn:code-review        # read-only audit; files findings as backlog tasks
/agn:code-comment       # add explanatory comments
/agn:code-commit        # stage and write a well-formed commit message
```

You approve every backlog → active → done transition. The agent stops at each gate for review rather than running the whole pipeline unattended. See [plugins/agn/README.md](plugins/agn/README.md) for the full skill reference and the `taskman.sh` lifecycle CLI.

## Repository layout

```
agenture-loop/
├── .claude-plugin/
│   └── marketplace.json          # marketplace manifest
├── plugins/
│   └── agn/                      # the agentic-SDLC plugin
│       ├── .claude-plugin/plugin.json
│       ├── skills/
│       ├── rules/
│       ├── scripts/
│       └── README.md
├── docs/                         # product docs for the marketplace
├── tasks/                        # this repo's own SDLC tracking (dogfoods agn)
├── LICENSE
├── PRIVACY.md
└── README.md
```

## Adding a new plugin to the marketplace

1. Create `plugins/<your-plugin>/` with its own `.claude-plugin/plugin.json` and any of `skills/`, `agents/`, `commands/`, `hooks/`, `.mcp.json`. All paths must be self-contained inside the plugin directory.
2. Add a new entry to the `plugins` array in `.claude-plugin/marketplace.json`.
3. Update the **Available plugins** table above.

See [Claude Code's plugin marketplace docs](https://code.claude.com/docs/en/plugin-marketplaces.md) for the full schema.

## Local development

The repo dogfoods its own `agn` plugin via symlinks in `.claude/`:

```
.claude/skills -> plugins/agn/skills
.claude/rules  -> plugins/agn/rules
```

When developing inside this repo with Claude Code, the `agn` skills load automatically without going through `/plugin install`.

To test the marketplace before publishing:

```
/plugin marketplace add ./
/plugin install agn@agenture
```

## License

[Apache 2.0](LICENSE). See [PRIVACY.md](PRIVACY.md) for the privacy policy.
