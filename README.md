# Agenture — Claude Code Plugin Marketplace

A marketplace of Claude Code plugins for agentic software development workflows.

## Install

Add the marketplace to Claude Code once, then install any plugin from it:

```
/plugin marketplace add AgentureHQ/agenture-loop
/plugin install <plugin-name>@agenture
```

## Available plugins

| Plugin | What it does | Docs |
|--------|--------------|------|
| `agn` | Agentic SDLC loop — spec, design, plan, implement, and test through structured `/agn:*` skills with built-in review gates | [plugins/agn/README.md](plugins/agn/README.md) |

Install the SDLC plugin:

```
/plugin install agn@agenture
```

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
