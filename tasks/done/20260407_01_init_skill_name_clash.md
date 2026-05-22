---
status: done
type: bug
---

# `/init` skill name clashes with Claude Code's built-in `/init`

**Date:** 2026-04-07

## Observed problem

The SpecToCode plugin exposes an `init` skill at `skills/init/SKILL.md`, intended to load the spec-to-code rule files into the current session. Its slash-command form collides with Claude Code's built-in `/init` command (which generates/updates `CLAUDE.md`).

When users type `/init` in a project where the SpecToCode plugin is installed, they hit Claude Code's built-in, not the plugin skill. The plugin skill is effectively unreachable under the natural name, and the name itself misleads users about what the command will do.

## Expected vs actual

**Expected:** The SpecToCode rule-loading skill has a distinctive name that does not collide with any Claude Code built-in command, so users can invoke it unambiguously and the plugin does not shadow (or appear to shadow) a core Claude Code feature.

**Actual:** The skill is named `init`. `/init` invokes Claude Code's built-in command, not the plugin. If the user discovers and invokes the namespaced form (`/agn:init`), they then hit a separate sandbox-permission failure (tracked in `tasks/backlog/20260404_12_init_skill_blocked_by_sandbox.md`). In practice there is no working path for a user to load the rules via a slash command from an external project.

## Reproduction steps

1. Install SpecToCode as a Claude Code plugin in any project (inside or outside the SpecToCode repo).
2. Open a Claude Code session in that project.
3. Type `/init` at the prompt.
4. Observe that Claude Code's built-in `/init` runs (generating/updating `CLAUDE.md`), not the SpecToCode rule loader.
5. Try `/agn:init` and observe either (a) the sandbox-permission error from ticket 12 when invoked from an external project, or (b) correct behaviour only when invoked from inside the SpecToCode repo.

## Environment & context

- **Affected file:** `skills/init/SKILL.md` — skill name `init` declared in frontmatter (line 2).
- **Related files:**
  - `rules/first-principles.md` and `rules/task-management.md` — content the skill is supposed to load.
  - `README.md` and `docs/spec-to-code-specification.md` — any user-facing references to `/init` or `/agn:init` that will need updating once the skill is renamed.
- **Clash target:** Claude Code's built-in `/init` command, which creates/updates `CLAUDE.md` and is a core, documented feature of Claude Code.
- **Scope:** No other SpecToCode skill name collides with a Claude Code built-in based on the current `skills/` directory (`codebase-review`, `commit-code`, `create-plan`, `create-task`, `define-product`, `design`, `implement`, `qa-integration-test`, `qa-system-test`, `comment-code` are all distinct).
- **Related ticket:** `tasks/backlog/20260404_12_init_skill_blocked_by_sandbox.md` — separate but adjacent bug. That ticket covers the sandbox permission failure when the skill *is* reached via `/agn:init`. This ticket covers the naming collision itself. Both should likely be resolved together when the skill is redesigned, but they are independently valid defects.

## Risks & impact

- **Severity:** High — combined with ticket 12, the rule-loading entry point is effectively broken for the plugin's primary use case (installation into external projects).
- **User confusion:** Users reading the README and typing `/init` silently get a different, unrelated command. They may overwrite or mutate their `CLAUDE.md` thinking they are loading spec-to-code rules.
- **Discoverability:** Because `disable-model-invocation: true` is set on the skill, the model will not auto-invoke it, so users are the only path — and the user-facing path is broken.
- **Blast radius:** All users of the plugin who follow the documented onboarding flow.
- **Mitigation scope (not a solution proposal, for context only):** The fix will involve renaming the skill, updating the frontmatter, renaming its directory, and updating every user-facing reference in docs and README. The rename must also be coordinated with the sandbox-permission fix in ticket 12 so the renamed command actually works end-to-end.
