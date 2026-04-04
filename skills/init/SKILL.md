---
name: init
description: Load spec-to-code rules into the current session. Run this once at session start if you are not using the stc agent. Rules persist until context compaction.
disable-model-invocation: true
---

# spec-to-code Rules

The following rules govern all spec-to-code SDLC skills. They are loaded into your session context now.

---

## Core Principles
!`cat ${CLAUDE_SKILL_DIR}/../../rules/first-principles.md`

---

## Task Management Standard
!`cat ${CLAUDE_SKILL_DIR}/../../rules/task-management.md`

---

Rules loaded. You can now use any `/stc:*` skill with these rules active. If the session compacts, run `/stc:init` again.
