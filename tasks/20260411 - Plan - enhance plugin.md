# Implementation Plan: Design Review Remediation

**Source:** `docs/design-review-report.md` (2026-04-04)

## Problem Statement

The plugin relies exclusively on skills (probabilistic agent compliance) for all guarantees including structural validation, quality gates, and behavioral consistency. Skills themselves lack operational depth — thin procedural outlines with no templates, no embedded rubrics, and no enforcement. Output quality depends on model training rather than plugin domain knowledge.

## Objective

Restructure the plugin around a three-tier enforcement model (hooks, agent, skills) and deepen all SDLC skills to carry domain expertise that produces consistent, high-quality artifacts.

## Phases and Tasks

### Phase 1: Foundation

Prerequisites for all subsequent work. Establishes the enforcement architecture and extended data model.

| Task | Description | Depends On |
|------|-------------|------------|
| 01 | Consolidate agent: single source of truth for rules + expanded behavioral context | — |
| 02 | Extend task YAML schema (phase, depends_on fields) | — |
| 03 | Add structural validation hooks | 02 |

Phase 1 tasks 01 and 02 are independent. Task 03 depends on 02 (hooks validate the schema defined in 02).

### Phase 2: Core Skill Deepening

Highest-value content work. Each task deepens one skill (or skill pair) with templates, validation rubrics, and operational guidance.

| Task | Description | Depends On |
|------|-------------|------------|
| 04 | Deepen `define-product` skill | 01 |
| 05 | Deepen `design` skill | 01 |
| 06 | Deepen `plan` skill | 01, 02 |
| 07 | Strengthen QA skills (`qa-integration-test`, `qa-system-test`) | 01 |

All Phase 2 tasks depend on 01 (agent must carry expanded context before skills are rewritten). Task 06 additionally depends on 02 (plan skill must generate extended YAML). Tasks 04, 05, 07 are independent of each other.

### Phase 3: Cross-Cutting

Improvements that build on the foundation and deepened skills.

| Task | Description | Depends On |
|------|-------------|------------|
| 08 | Add `/agn:status` skill | 02 |
| 09 | Strengthen implementation skill (`implement` — unified task/phase/all) | 01 |
| 10 | Add SDLC document review to `codebase-review` | — |
| 11 | Resolve `tasks/intake/` directory | — |

Tasks 10 and 11 have no dependencies and can execute at any time.

## Dependency Graph

```
01 (agent consolidation)  ──┬──→ 04 (define-product)
                            ├──→ 05 (design)
                            ├──→ 06 (plan) ←── 02 (task YAML schema)
                            ├──→ 07 (QA skills)
                            └──→ 09 (implementation skills)

02 (task YAML schema) ──┬──→ 03 (hooks)
                        ├──→ 06 (plan)
                        └──→ 08 (status skill)

10 (codebase-review) ── no dependencies
11 (intake cleanup)  ── no dependencies
```

## Acceptance Criteria

- AC1: All rules content exists in exactly one canonical location (no duplication between agent and rule files)
- AC2: Agent system prompt contains decision heuristics, context management policy, and error recovery guidance beyond current rules
- AC3: Task YAML schema supports `phase` and `depends_on` fields; task management standard documents them
- AC4: At least one hook enforces structural validation on commit (YAML validity, directory-status consistency)
- AC5: `define-product`, `design`, `plan` skills each contain: artifact template with required sections, enumerated validation checklist, minimum information thresholds
- AC6: QA skills contain: test discovery step, structured report template, explicit "no tests" handling
- AC7: `/agn:status` skill exists and reports SDLC progress from artifact/task state
- AC8: Implementation skills explicitly restate per-task contract (no shortcuts in batch mode) and require architecture compliance check
- AC9: `codebase-review` includes architecture and spec compliance categories
- AC10: `tasks/intake/` is either documented as a lifecycle state or removed
- AC11: `docs/spec-to-code-specification.md` and `README.md` are updated to reflect all changes

## Risks

| Risk | Mitigation |
|------|-----------|
| Deepened skills become too long for context window | Keep skills under 200 lines. Move artifact templates to referenced files if needed. |
| Hooks reject valid work due to false positives | Start with minimal hook scope (YAML structure only). Expand after validation. |
| Agent prompt expansion competes with skill content for context | Agent carries behavioral policy only (heuristics, not templates). Templates stay in skills. |
| Specification drift during implementation | Each task that changes behavior updates `docs/spec-to-code-specification.md` as part of its quality gate. |
