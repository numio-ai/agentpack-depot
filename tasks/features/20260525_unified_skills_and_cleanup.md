---
status: backlog
slug: unified_skills_and_cleanup
epic: agentic_sdlc_rework
title: Unify skill naming under verb-noun pattern; retire obsolete artifacts
draft: true
---

# Unify skill naming under verb-noun pattern; retire obsolete artifacts


## Problem statement

Existing skill names mix patterns (`epic-create`, `task-implement`, `qa-system`) and hide the recursive SDLC structure. The `session-load` skill and `stc` agent references in docs are leftovers from an abandoned design and confuse users about how rules load.

## Objective

Single verb-noun naming surface (`/agn:<verb> <level>`) for lifecycle skills. Obsolete artifacts deleted.

## Acceptance criteria

- Lifecycle skills migrated to verbs: `define`, `design`, `plan`, `implement`, `validate`. Each takes the level (`product|epic|feature|task` as applicable) as the skill argument.
- Tool skills unchanged: `/agn:code-review`, `/agn:code-comment`, `/agn:code-commit`.
- `plugins/agn/skills/session-load/` directory deleted.
- `settings.json.disabled` deleted.
- README, CLAUDE.md, and `docs/agenture-loop--specification.md` updated; no remaining references to old skill names or the `stc` agent.
- Workflow logic preserved within each renamed skill — only naming and argument structure change in this feature.

## Scope

In scope: file renames; content migration into argument-driven skills; deletion of obsolete artifacts; documentation updates.

Out of scope: behavioral changes (Planner sub-agent, QA sub-agent, escalation, hook — covered by sibling features).

## Tasks

To be decomposed during `/agn:implement feature` of this feature.
