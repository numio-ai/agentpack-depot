## What is it?

agenture-loop is a Claude Code plugin that incorporates software development practices of spec driven development. It provides tools, rules and skills to make SDLC effieicient when using with Claude Code.

Current code of 'agenture-loop' is irrelevant. It contains old code, outdated now. During our work on Numio we improved Cluade Code artifacts a lot, and we want to ereuse them to create new version of agenture-loop (former spec-to-code or stc). 

Below document is our gap analysis of additions we want to make on top of skills implemented in Numio (/Users/vladimirkroz/repos/numio-root/numio-app/.claude)


## Current state



12 skills, no consistent naming, three different patterns:

| Pattern | Skills |
|---|---|
| verb-noun | `define-product`, `create-feature`, `create-task`, `commit-code`, `comment-code` |
| noun-noun / scoped | `qa-integration-test`, `qa-system-test`, `codebase-review`, `start-stop-app`, `load-rules` |
| bare verb | `design`, `implement` |

Hierarchy of work units already covered: **product → feature → task**. Missing: **epic**.

`implement` is a single skill with three modes (`task`, `feature`, `plan`). The `plan` mode is marked "legacy" in the source. `docs/implementation-plan.md` and its "phases" are doing scope-decomposition work that an epic should own.

## Three problems worth fixing together

1. **Inconsistent skill names** — tab completion is unpredictable. `/agn:c<TAB>` matches `commit-code`, `comment-code`, `create-feature`, `create-task`, `codebase-review`. Five unrelated things.
2. **No epic tier** — gap between product and feature.
3. **`implement` overloads three contracts** into one skill. The `plan` mode is already deprecated and overlaps with what epic should do.

## Proposal: `<scope>-<action>`

Scope-first puts hierarchy in the name and groups skills under tab completion: `/agn:epic-<TAB>` → every epic action.

### Rename + add map

| Current | Proposed | Change |
|---|---|---|
| `define-product` | `product-define` | rename |
| `design` | `product-design` | rename — "design" alone is ambiguous |
| — | `epic-create` | **new** |
| — | `epic-implement` | **new** — absorbs `implement plan` |
| `create-feature` | `feature-create` | rename |
| `implement feature <slug>` | `feature-implement` | extract from `implement` |
| `create-task` | `task-create` | rename |
| `implement task <path>` | `task-implement` | extract from `implement` |
| `implement plan <path>` | (retired) | replaced by `epic-implement` |
| `codebase-review` | `code-review` | rename |
| `comment-code` | `code-comment` | rename |
| `commit-code` | `code-commit` | rename |
| `qa-integration-test` | `qa-integration` | drop redundant `test` |
| `qa-system-test` | `qa-system` | drop redundant `test` |
| `start-stop-app` | `app-run` | rename |
| `load-rules` | `session-load` | rename |

Skill count: 12 → 14 (split `implement` into three, add `epic-create`, retire `implement plan`).

## Epic data model

Mirrors feature, one tier up.

**File:** `tasks/epics/YYYYMMDD_<slug>.md`

**YAML:**
```yaml
---
status: backlog | active | done
slug: <epic-slug>
title: <human-readable title>
---
```

**Required body sections:**
- Problem statement
- Objective
- Scope (in / out)
- Acceptance criteria — observable conditions at functional-block level
- Linked features — ordered list of feature slugs that compose this epic

**Feature YAML gets an optional `epic: <slug>` field**, analogous to how task gets `feature: <slug>`.

**Lifecycle:** epic → `done` requires every linked feature to be `done`. Same pattern as feature → done requiring its tasks.

## Rules changes

`task-management.md` updates:
- Add Epic row to Concepts table
- Add `tasks/epics/` to Storage
- Add epic YAML schema
- Add `epic: <slug>` to feature schema
- Add `taskman.sh new epic`, `taskman.sh epic close <slug>`, `taskman.sh list epics`

`first-principles.md` — no change.

## Trade-offs to flag

1. **Renaming is a breaking change for muscle memory.** Every `/agn:create-task` you type today becomes `/agn:task-create`. Cost is real and one-time. No partial migration — pick a cutover date.

2. **Epic without a concrete first instance is speculative.** I'd recommend writing `epic-create` and `epic-implement` *against a real epic* on Numio rather than spec-first. Pick one functional block that's clearly bigger than a feature (Numio: auth? billing? data ingestion?) and use it to drive the skill design.

3. **`implementation-plan.md` is now orphaned.** If you adopt this, decide its fate: (a) retire it entirely, (b) demote to a thin roadmap listing epics, or (c) keep it as an execution-sequencing artifact orthogonal to epics. My recommendation: (a). The user's earlier YAGNI principle applies — one decomposition axis, not two.

4. **`app-run` collapses start and stop into one skill.** Alternative: keep them split as `app-start` and `app-stop`. Either works; one skill with a verb argument is more typical for ops actions.

## Open questions before I edit any file

1. Confirm scope-first naming (`<scope>-<action>`) is the direction. If you prefer action-first, the table flips.
2. Confirm splitting `implement` into three skills versus keeping one skill with three modes.
3. Confirm retiring `implementation-plan.md` / `plan` mode.
4. Do you want me to draft the new/renamed SKILL.md files and the updated `task-management.md`, or stay in proposal mode for another round?