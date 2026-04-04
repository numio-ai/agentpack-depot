# spec-to-code: Vision, Specification and Requirements

## Vision


### spec-to-code goals:
- Provide a framework and mechanisms  for accelerated software development using AI coding tools (Claude Code)
- Establish AI-assisted SDLC, where repeatable steps in lifecycle are automated or assisted using AI agents. Therefore overall speed of delivery is accelerated. 
- Stretch goal - establishe feedback loop for incremental improvements of spec-to-code artifacts (skills, rules, agents, hooks, tools/MCPs) - the more users use it, the better it serves their projects

### SDLC to implement in spec-to-code

#### New product  development
    
- **Definition**
    - **Objective**: establish vision document, product specification, and requirements documents that will be used in subsequent stages of SDLC
    - **Inputs**:
        - Product owner's vision and requirements
        - Any other available sources of information (e.g. if there is a similar product available online, or online documentation). 
    - **Outputs**:
        - Vision document (vision) - 1 pager document that captures problem this product solves, key functional capabilities, explain how it solves user problem, and answers to question "So what?"
        - Product specification (spec)- detailed document that describes product functionality, user flows, and user interactions. Spec describes how system behaves after we build it. It focuses on overall UX, functionality and flows. 
        - Requirements (reqs) - addendum to spec. It goes in depth for specific areas, which may not fully detaileded in spec. It is more detailed and formal description of product functionality and it complements spec. 
    - **How it is done**:
        1. Compose initial draft of vision document (1 pager). This is collaborative process between product owner and agent. Agent will use product owner's vision and requirements to compose initial draft of vision document.
        2. Compose initial draft of product specification. Agent takes initial step and drafts specification using as input a vision document and any other available sources of information (e.g. addional inputs could be more detailed description of user problem, or online documentation). Once draft is generated product owner reviews it and provides feedback. Agent updates specification based on feedback.
        3. Compose requirements document. Agent takes initial step and drafts requirements using a vision and a spec as input. The role of requirements is to disambiguate spec, and make sure that together spec and requirements provide full details required for design and implementation of the product. Once draft is generated product owner reviews it and provides feedback. Agent updates requirements and possibly vision and spec based on feedback.
        4. Output validation: Agent reviews outputs and provides feedback to product owner in the form of report. Product owner reviews feedback and provides feedback to agent. Process repeats until all critical issues are addressed, or user instructs agent that he is ready to move to next step. Rubricks we use to validate outputs are:
            - Business case validation: using critical and analytics skills we check:
                - do we demonstrate understanding of business and user problem? 
                - is our spec really solves business problem? is there a better way to solve business problem?  
                - do we have a sound business case for monetization?
                - what are possible risks, and did we address them? 
            - Functional validation: Are produced documents define-product functionality consistently, and do they provide full details required for design and implementation of the product?
                - Is outputted functionality in a way that is easy to understand and follow?
                - Are there any gaps in functionality?
                - Are there any ambiguities in functionality?
                - Are there any inconsistencies in functionality?
                - Are there any missing details in functionality?

- **High-level design (Architecture)**
    - **Objective**: establish high-level architecture design that serves as input for (1) implementation plan at stage 'planning implementation' and (2) tasks detailed design at stage 'implementation'. This document sets high-level technical direction, but keep it high level. Focus on choice of technologies, software architecture and system design, defines domain dictionary (limited to key elements, no detailed schemas), workflows and key APIs, security mechanisms. We don't do detailed design at this stage. I.e. detailed APIs signatures, tables schema designs, function names etc are out of scope. We go in depth to detailed design at components level during implementation phase when work on specific task or phase.
    - **Inputs**:
        - Vision, specification and requirements from Definition stage
    - **Outputs**:
        - High-level architecture design document. 
    - **How it is done**:
        - Agent takes initial step and drafts high-level architecture design document using as input a vision, specification and requirements from Definition stage. Once draft is generated product owner reviews it and provides feedback. Agent updates architecture design document based on feedback.
        - QA: The next step is to validate the architecture design document. The agent reviews design documents and looks for any issues that could cause problems at the planning or implementation stage — such as incomplete or inconsistent architecture, over-engineering, under-engineering, violations of core software design principles, ambiguities, missing details, or unaddressed requirements. As an additional cross-check, the agent verifies that the architecture addresses all functionality outlined in the vision and specs. The agent writes all findings into a report document, and the product owner provides answers and feeds them back to the agent. With the product owner's input, the agent updates the architecture design document and repeats the validation step. This process continues until all critical issues are addressed, or the user instructs the agent to move on to the next step.
        - As the last step in this process, and before moving to planning implementation step, agent decides if it need to update specs or requirements. There are situation when it is needed to update specs or requirements. E.g. some functionality is too expensive to deliver and we decide to push to to future release, or we decide to achieve some requirement differently than originally planned. In such cases agent will update specs and requirements along with archicture design to ensure that all documets are consistent with each other. 
        - Final user review: User reviews produced and updated documents, may ask for additoinal changes, and instructs agent to move to next step when all action items are cleared.

- **Planning implementation**

    - **Objective**: Establish an implementation plan that serves as the blueprint for the implementation stage. The plan is structured as a DAG (directed acyclic graph) of tasks grouped into phases, giving implementation agents clear visibility into sequencing and parallelization opportunities. It consists of two artifacts: (1) an implementation plan document covering overall scope, phases, tasks, and dependencies; and (2) individual task documents. See the task management standard for details on how tasks are defined and managed. The implementation plan is used as input for the Implementation stage.
    - **Inputs**: Specs, requirements, and high-level architecture design document from prior stages.
    - **Outputs**: Implementation plan consisting of tasks grouped into phases and organized as a DAG.
    - **How it is done**:
        - The agent drafts the implementation plan using the high-level architecture design document, specs, and requirements as inputs. Once the draft is generated, the product owner reviews it and provides feedback. The agent updates the plan based on that feedback.
        - QA validation: The agent reviews the implementation plan to find any issues that could cause problems during implementation — such as incomplete or ambiguous task definitions, over-engineering, inconsistencies, or missing coverage of functionality outlined in the vision and specs. A key cross-check at this stage is that each task provides sufficient non-ambiguous detail for an implementation agent to proceed with detailed design and coding, as this is the last gate before handing the plan to automated agents. The agent writes all findings into a report document, and the product owner provides answers and feeds them back to the agent. The agent then updates the implementation plan and repeats the validation step. This process continues until all critical issues are addressed, or the user instructs the agent to move on.
        - Feedback to prior stages: If during planning it becomes clear that specs or requirements need to change — for example, because some functionality is too costly and will be pushed to a future release, or because a requirement can be achieved more optimally — the agent updates the relevant definition documents alongside the implementation plan to ensure all artifacts remain consistent.
        - The user reviews the final implementation plan, requests any additional changes if needed, or approves it to proceed to the implementation stage.

- **Implementation**
    - **Objective**: Execute the implementation plan by completing tasks in the order defined by the DAG, producing working, tested, and documented code. The user controls execution granularity — they can instruct the agent to execute a single task, an entire phase, or the full DAG in one run. Each task goes through detailed design before coding begins, ensuring the implementation is grounded in the architecture and specs rather than improvised. The stage is largely autonomous — agents work through tasks independently, escalating to the product owner only when blockers arise or decisions fall outside the scope of existing artifacts.
    - **Inputs**: Implementation plan (phases, tasks, DAG), high-level architecture design document, specs, and requirements.
    - **Outputs**: Working codebase, per-task detailed design notes, per-phase integration test results, and updated artifacts (specs, architecture, task docs) reflecting any decisions made during implementation.
    - **How it is done**:
        - The agent processes tasks in the order defined by the implementation plan, respecting phase boundaries and DAG dependencies. Where the DAG allows it, tasks within a phase can be executed in parallel.
        - For each task, the agent first produces a detailed design — including API signatures, data schemas, component interfaces, and key logic — before writing any code. This keeps implementation decisions explicit and reviewable. The detailed design is informed by the high-level architecture document and the task definition.
        - The agent implements the task, writes unit tests, and verifies that the implementation passes all relevant tests and does not break existing functionality.
        - If during implementation the agent encounters ambiguities, gaps, or decisions that require product owner input — such as a requirement conflict, an architectural tradeoff, or a scope question — it surfaces these as blockers and waits for resolution before continuing.
        - Feedback to prior stages: If implementation reveals that specs, requirements, or the architecture need to be updated — for example, because a design decision turns out to be impractical, or a scope adjustment is needed — the agent updates the relevant documents to keep all artifacts consistent.
        - **Integration test (per phase)**: Once all tasks in a phase are complete, the agent runs an integration test against the live application on the development machine or test environment. The integration test validates: (1) all functional requirements delivered by this phase work correctly end-to-end, and (2) functionality delivered by previous phases continues to work — confirming that new tasks have not introduced regressions. Issues found are fixed before proceeding. The product owner reviews the phase output and integration test results, and approves progression to the next phase or requests corrections.
        - The stage concludes when all phases are complete, all integration tests pass, and the product owner approves the implementation as ready for system testing.

- **QA (System Test)**
    - **Objective**: Perform a full system test once the entire implementation scope is complete. While integration tests during the Implementation stage validate each phase in isolation, this stage tests the product as a whole — verifying that all features work together correctly, all functional and non-functional requirements from the Definition stage are met, and the product is ready for release.
    - **Inputs**: Fully implemented codebase, specs, requirements, and integration test results from the Implementation stage.
    - **Outputs**: System test report documenting findings; a verified, release-ready codebase with all critical issues resolved.
    - **How it is done**:
        - The agent reviews the full codebase against the specs and requirements, checking that all specified functionality is present, behaves correctly end-to-end, and handles edge cases appropriately.
        - The agent runs the full test suite and checks for test coverage gaps across the entire product. Where coverage is insufficient for critical paths, it adds missing tests.
        - The agent validates all end-to-end user flows as specified, exercising the system as a whole rather than individual components.
        - Any issues found are written into a system test report, categorized by severity (critical, major, minor). Critical and major issues must be resolved before the product can be released. The agent fixes issues it can resolve autonomously; issues requiring product owner decisions are escalated.
        - After fixes are applied, affected areas are re-tested to confirm resolution. The test cycle repeats until all critical and major issues are cleared.
        - The product owner reviews the final system test report and the verified codebase, and approves the product as ready for release or requests further action.



#### Bug fixes

Bug fixes apply to an existing product where all definition documents (vision, specs, requirements) and architecture documents are already present in the `docs/` folder. The workflow is streamlined compared to new product development — Definition and Architecture stages are skipped, and the process begins directly from a defect task.

- **Defect task (input)**
    - **Objective**: Capture a well-defined, actionable description of the bug so the agent has everything it needs to investigate and fix it without ambiguity.
    - **How it is done**: The user creates a task of type `defect` in `tasks/backlog/` containing: a description of the observed problem, the expected result, the actual result, and any reproduction steps or context (environment, logs, screenshots). The task serves as the primary input for all subsequent stages.

- **Planning implementation**
    - **Objective**: Understand the root cause of the defect and produce a targeted fix plan, without touching architecture unless absolutely necessary.
    - **Inputs**: Defect task, existing docs (vision, specs, requirements, architecture design), codebase.
    - **Outputs**: Root cause analysis note and a fix plan — a concise set of tasks describing exactly what needs to change in the codebase to resolve the defect.
    - **How it is done**:
        - The agent investigates the codebase to identify the root cause of the defect as described in the task. It traces the problem through relevant components, data flows, and logic.
        - The agent produces a root cause analysis note and a fix plan outlining the specific changes required. The fix plan is added to the defect task or as a linked task set in `tasks/`.
        - **Architecture impact check**: If the root cause reveals that fixing the defect requires an architectural change — such as modifying a core system boundary, data model, or cross-cutting mechanism — the agent must stop, document the architectural impact clearly, and enter an interactive discussion with the user before proceeding. No fix work begins until the user explicitly approves the approach.
        - For straightforward bugs with no architectural impact, the agent proceeds to implementation after the fix plan is drafted.

- **Implementation**
    - **Objective**: Apply the fix as defined in the fix plan, with the defect task as the primary guide alongside the existing architecture and specs.
    - **Inputs**: Defect task (with root cause analysis and fix plan), architecture design document, specs, and requirements.
    - **Outputs**: Fixed codebase, updated task reflecting the resolution, and updated docs if the fix required any spec or architecture clarifications.
    - **How it is done**:
        - The agent implements the fix following the same task-level process as in new product development: detailed design first, then code, then unit tests.
        - If the fix reveals that specs, requirements, or architecture docs need minor updates to reflect the correct intended behavior, the agent updates those documents to keep all artifacts consistent.
        - If during implementation the agent encounters scope creep — changes beyond what is needed to fix the specific defect — it flags this and does not proceed without user instruction.

- **Integration test**
    - **Objective**: Verify the fix works correctly in the live application and has not introduced regressions in related functionality.
    - **How it is done**: The agent runs an integration test against the live application on the development machine or test environment. The test validates: (1) the defect is resolved and the expected behavior is now observed, and (2) functionality related to or touched by the fix continues to work correctly. Issues found are resolved before proceeding.

- **System test (regression)**
    - **Objective**: Confirm that the fix has not broken any other part of the product by running a full system test.
    - **How it is done**: The agent runs the full test suite and validates key end-to-end user flows across the entire product. Any regressions found are fixed, and affected areas are re-tested. The agent produces a brief system test report. The product owner reviews the report and approves the fix as ready for release, or requests further action.

#### Maintenance

Maintenance covers ongoing work on an existing product that does not introduce new user-facing features. Two scenarios are supported:

---

**Scenario 1: Ad-hoc maintenance tasks**

The user identifies a specific maintenance need — such as refactoring a module, migrating a database, upgrading a dependency, or improving test coverage — and initiates it through the standard task workflow.

- **How it is done**:
    - The user creates a task in `tasks/backlog/` using the `create-task` skill, with a clear problem statement, scope, and acceptance criteria. The task type should reflect the nature of the work (e.g. `task`, `bug`, `refactor`, `upgrade`, `chore`).
    - The agent picks up the task using the `implement-task` skill and executes it following the same Implementation process as in new product development: detailed design first, then code, then unit tests.
    - Architecture impact check: if the task requires architectural changes, the agent must flag this and enter an interactive discussion with the user before proceeding.
    - Integration and regression tests are run after the task is complete to confirm nothing is broken. See the Integration test and System test steps in the Bug fixes workflow for reference — the same process applies here.
    - The user approves the task as done once all quality gates pass.

---

**Scenario 2: Codebase optimization**

Over time, as agents implement individual tasks with a narrow focus, the codebase can become fragmented — locally optimized per task but globally degraded in quality, consistency, and structure. Codebase optimization is a periodic process where the agent audits the full codebase and proposes a holistic improvement plan.

- **How it is done**:
    - **Audit (agent-driven)**: The agent reviews the full codebase and produces a review report identifying areas of technical debt, fragmentation, inconsistency, duplication, over-complexity, performance issues, or deviations from the architecture. This replaces the product-owner-driven Definition stage — the agent is the one surfacing the problems. The agent presents findings to the product owner for review and prioritization.
    - **High-level design (Architecture)**: If the optimization plan requires structural or architectural changes, the agent produces or updates the architecture design document to reflect the target state. The product owner reviews and approves before planning begins.
    - **Planning implementation**: The agent translates the approved optimization plan into a set of tasks, grouped into phases and organized as a DAG, following the same process as in new product development. The product owner reviews and approves the plan.
    - **Implementation, Integration test, QA (System Test)**: Executed using the same process defined in the New product development workflow. The scope is the approved optimization plan rather than new features, but the mechanics are identical.



#### Incremental features to existing product

Adding incremental features to an existing product follows the same overall workflow as new product development, with one key difference: the Definition, Architecture, and Planning stages operate in the context of existing artifacts rather than starting from scratch. All existing docs (vision, specs, requirements, architecture) are already present in `docs/` and serve as the baseline.

- **Definition**
    - **Objective**: Define the new feature clearly — what problem it solves, how it fits into the existing product, and what it requires — and update the existing definition documents to incorporate it.
    - **How it is done**: The process mirrors Definition in new product development, but the agent works from the existing vision, specs, and requirements as a starting point. The agent drafts additions and amendments to those documents to cover the new feature. The product owner reviews and iterates until the feature is fully and consistently specified. Output validation follows the same rubric (business case and functional validation). The goal is that after this stage, the existing docs fully describe the product including the new feature — there are no separate "feature docs".

- **High-level design (Architecture)**
    - **Objective**: Assess whether the new feature requires architectural changes and, if so, extend or update the architecture design document accordingly.
    - **How it is done**: The agent reviews the existing architecture design document and determines whether the new feature can be implemented within the current architecture, or whether changes are needed. If changes are needed, the agent proposes them and the product owner reviews and approves before planning begins. If no architectural changes are required, this stage is lightweight — the agent documents that assessment and proceeds. The architecture document is updated to reflect the final state. QA validation and feedback to prior stages follow the same process as in new product development.

- **Planning implementation**
    - **Objective**: Produce an implementation plan scoped to the new feature, taking care to account for dependencies on and impacts to existing functionality.
    - **How it is done**: Same process as in new product development. The agent drafts the plan using the updated specs, requirements, and architecture as inputs. A key consideration at this stage is identifying which existing components are touched by the new feature, so those areas are explicitly covered in the plan and in testing scope. QA validation and product owner approval follow the same process.

- **Implementation**
    - Same process as in new product development. The defect task equivalent here is the feature scope defined in the implementation plan. The scope creep principle applies — the agent does not modify functionality outside the approved feature scope without explicit user instruction.

- **Integration test**
    - Same process as in new product development. The integration test validates: (1) the new feature works correctly end-to-end, and (2) existing functionality has not been broken by the changes.

- **QA (System Test)**
    - Same process as in new product development. The full product is tested as a whole, covering both the new feature and the existing functionality, before the release is approved.


## Current State

spec-to-code is packaged as a **Claude Code plugin** (name: `stc`). Users install it from GitHub and invoke skills via `/stc:<skill>`.

The plugin ships:
- **`stc` agent** — default agent whose system prompt contains the behavioral rules (first-principles, task-management standard). Activated via `settings.json`. Rules load once at session start and survive compaction.
- **13 skills** covering the full SDLC: `/stc:init` (manual rule loader), `/stc:define-product`, `/stc:design`, `/stc:plan`, `/stc:implement-task`, `/stc:implement-phase`, `/stc:implement-plan`, `/stc:qa-integration-test`, `/stc:qa-system-test`, `/stc:create-task`, `/stc:codebase-review`, `/stc:comment-code`, `/stc:commit-code`.
- **Canonical rules** in `rules/first-principles.md` and `rules/task-management.md` — single source of truth used by both the agent and `/stc:init`.
- **Artifacts:** `tasks/` directory (backlog / active / done), `docs/*` specs, designs, architecture, plans.

What works: a developer runs `/stc:implement-task` on a well-defined task file and the agent implements and unit-tests it with minimal intervention. When tasks are unambiguous, agent output quality is high.


## What is needed next

Validate plugin installation and skill invocation end-to-end.
Publish to a Claude Code marketplace or official registry for wider distribution.
Stretch: parallel task execution via agents, feedback loop for skill improvement.

# Specification and Requirements

## Workflows: User Experience

spec-to-code supports four workflows. Each is a sequence of stages driven by skill invocations. At every stage the user invokes a skill, the agent executes it collaboratively, and the user approves the output before moving forward.

### New Product Development

Starting point: the user has an idea for a new product and no existing documents.

**Definition** — `/stc:define-product`

The agent guides the user through producing three documents:

1. *Vision* (`docs/vision.md`): The agent interviews the user about the problem, target users, and key capabilities. It drafts a one-page vision. The user reviews, gives feedback, iterates until satisfied.

2. *Specification* (`docs/spec.md`): Using the approved vision plus any additional inputs the user provides (reference products, online docs, domain knowledge), the agent drafts a product specification — functionality, user flows, interactions. The user reviews and iterates.

3. *Requirements* (`docs/requirements.md`): The agent drafts requirements that disambiguate and complement the spec with formal detail. The user reviews and iterates.

After all three documents are drafted, the agent validates them against two rubrics:
- *Business case*: Does the spec solve the stated problem? Is there a better approach? Is the monetization case sound? Are risks addressed?
- *Functional completeness*: Is the functionality consistent, unambiguous, complete, and detailed enough for design and implementation?

The agent produces a validation report. The user addresses findings. The cycle repeats until all critical issues are resolved or the user approves moving forward.

**Architecture** — `/stc:design`

The agent drafts a high-level architecture document (`docs/architecture.md`) covering: technology choices, system architecture, domain dictionary (key terms only), workflows and key APIs, security mechanisms. Detailed design (API signatures, schemas) is explicitly out of scope — that happens during implementation.

Review cycle:
1. User reviews and gives feedback. Agent updates.
2. Agent validates: completeness, consistency with specs, over/under-engineering, design principle violations. Produces a report. User addresses findings.
3. Agent checks whether specs or requirements need updates based on architectural decisions (e.g., a feature deferred due to cost). Updates all documents to stay consistent.
4. User gives final approval.

**Planning** — `/stc:plan`

The agent produces an implementation plan (`docs/implementation-plan.md`): a DAG of tasks grouped into phases. Each task becomes a file in `tasks/backlog/` following the task management standard.

Review cycle:
1. User reviews plan and task definitions.
2. Agent validates: each task is unambiguous and self-contained enough for an implementation agent. Cross-checks coverage against specs. Produces report.
3. Specs/requirements updated if needed.
4. User approves.

**Implementation** — one of:
- `/stc:implement-task <task>` — single task
- `/stc:implement-phase <N>` — all tasks in a phase
- `/stc:implement-plan` — full plan

Per task, the agent: (1) produces detailed design, (2) implements, (3) writes and runs tests. Blockers and ambiguities are surfaced to the user.

At the end of each phase, the agent runs integration tests (`/stc:qa-integration-test`): new functionality works and prior phases are not broken. User reviews before the next phase begins. Documents are updated if implementation reveals necessary changes.

**QA** — `/stc:qa-system-test`

The agent reviews the full codebase against specs, runs the test suite, adds missing tests for critical paths, validates end-to-end flows. Produces a system test report (critical/major/minor). Fixes what it can; escalates the rest. Re-tests until all critical and major issues are cleared. User approves.

---

### Bug Fix

Precondition: existing product with documents in `docs/`.

1. User creates a bug ticket: `/stc:create-task bug` — provides observed problem, expected result, actual result, reproduction steps.
2. User runs `/stc:implement-task <defect-task>`.
3. Agent investigates root cause. Produces root cause analysis and fix plan.
4. **Architecture gate**: if the fix requires architectural changes, the agent stops and discusses with the user before proceeding.
5. Agent implements: detailed design → code → tests.
6. Agent runs integration test (`/stc:qa-integration-test`) — defect resolved + related functionality intact.
7. Agent runs system test (`/stc:qa-system-test`) — full regression. Produces report.
8. User approves.

---

### Maintenance

Precondition: existing product with documents in `docs/`.

**Ad-hoc task:**

1. User creates a task: `/stc:create-task task` (type: `refactor`, `upgrade`, `chore`, etc.).
2. User runs `/stc:implement-task <task>`.
3. Same execution flow as a single implementation task. Architecture gate applies.
4. Integration test (`/stc:qa-integration-test`) + regression tests. User approves.

**Codebase optimization:**

1. User invokes `/stc:codebase-review`.
2. Agent audits the full codebase. Produces a report: technical debt, fragmentation, inconsistency, duplication, performance issues.
3. User reviews and prioritizes findings.
4. If structural changes are needed: agent updates architecture doc, user approves.
5. Agent produces implementation plan (phases, tasks, DAG), user approves.
6. Execution follows the standard Implementation → `/stc:qa-integration-test` → `/stc:qa-system-test` flow.

---

### Incremental Feature

Precondition: existing product with documents in `docs/`.

Same stages as new product development, but the agent operates on existing documents:

1. `/stc:define-product` — Agent drafts additions/amendments to existing vision, spec, requirements. After this stage, the existing docs describe the product including the new feature — no separate "feature docs."
2. `/stc:design` — Agent assesses whether architecture changes are needed. If none needed, this is lightweight — agent documents the assessment and moves on.
3. `/stc:plan` — Scoped to the new feature but accounts for dependencies on existing code.
4. Implementation, QA — same as new product development.

---

## Plugin Specification

Derived from the workflows above. Everything below exists to support the user experience described in the previous section.

### Skills

spec-to-code is a Claude Code plugin named `stc`. All skills are namespaced as `/stc:<skill>`.

| Skill | Invocation | Drives | Input | Output |
|-------|-----------|--------|-------|--------|
| `init` | `/stc:init` | Rule loading (manual fallback) | None | Rules injected into session context |
| `define-product` | `/stc:define-product` | Definition stage | User input, reference materials | `docs/vision.md`, `docs/spec.md`, `docs/requirements.md` |
| `design` | `/stc:design` | Architecture stage | Docs from Definition | `docs/architecture.md` |
| `plan` | `/stc:plan` | Planning stage | Docs from Definition + Architecture | `docs/implementation-plan.md`, task files in `tasks/backlog/` |
| `implement-task` | `/stc:implement-task <task>` | Single task execution | Task file | Code changes, test results |
| `implement-phase` | `/stc:implement-phase <N>` | Phase execution | Implementation plan, task files | Working code, test results |
| `implement-plan` | `/stc:implement-plan` | Full plan execution | Implementation plan, task files | Working code, test results |
| `qa-integration-test` | `/stc:qa-integration-test` | Integration testing | Codebase, specs, phase scope | Integration test report |
| `qa-system-test` | `/stc:qa-system-test` | System testing | Full codebase, all docs | System test report |
| `create-task` | `/stc:create-task` | Task / bug ticket creation | User input (optional: `task` or `bug`) | Task file in `tasks/backlog/` |
| `codebase-review` | `/stc:codebase-review` | Codebase audit | Full codebase | Audit report |
| `commit-code` | `/stc:commit-code` | Version control | Staged changes | Git commit |
| `comment-code` | `/stc:comment-code` | Code commenting | Source files | Commented source files |

#### Skill behavior contract

Every skill that drives a workflow stage must:
1. **Validate preconditions** before starting. `/stc:design` requires definition docs to exist. `/stc:plan` requires architecture doc. `/stc:implement-phase` and `/stc:implement-plan` require an approved plan. `/stc:qa-system-test` requires a completed implementation. If preconditions are not met, the skill tells the user what is missing.
2. **Produce artifacts** as specified in the output column.
3. **Run a validation cycle** after producing artifacts: self-review against rubrics, produce a report, address findings with user input.
4. **Maintain document consistency**: if one document changes, dependent documents are reviewed and updated. The dependency chain is: vision → spec → requirements → architecture → implementation plan → tasks. Changes flow forward; upstream documents are updated only when downstream work reveals the need.
5. **Get user approval** before the stage is considered complete.

### Behavioral Guardrails

Plugins don't support auto-loaded `.claude/rules/`. spec-to-code uses two mechanisms to load rules into the session:

1. **`stc` agent (recommended)** — The plugin ships a default agent (`agents/stc.md`) activated via `settings.json`. Its system prompt contains both rule sets. Rules load once at session start and survive context compaction.
2. **`/stc:init` skill (fallback)** — Manually injects rules via `!cat ${CLAUDE_SKILL_DIR}/../../rules/...`. For users who can't use the agent (e.g., conflict with another plugin). Does not survive compaction.

| Guardrail | Content | Source |
|-----------|---------|--------|
| Core Principles | YAGNI, KISS, DRY, single-task focus, clarify ambiguity, step-by-step approval, root-cause troubleshooting | `rules/first-principles.md` |
| Task Management Standard | Task lifecycle (backlog → active → done), file structure, naming, approval model | `rules/task-management.md` |

Skills themselves contain only workflow instructions — no inlined rules. They rely on the rules being present in the session via the agent or `/stc:init`.

### User's Project Structure

spec-to-code skills create the following layout in the user's project as they progress through SDLC stages:

```
user-project/
├── docs/
│   ├── vision.md
│   ├── spec.md
│   ├── requirements.md
│   ├── architecture.md
│   └── implementation-plan.md
├── tasks/
│   ├── backlog/
│   ├── active/
│   └── done/
```

The `/stc:define-product` skill creates the `docs/` directory and initial documents if they don't exist. The `/stc:plan` skill creates task files in `tasks/backlog/`. The project structure is created incrementally as the user progresses through stages — no upfront scaffolding required.

### Workflow State

Each skill checks its own preconditions by examining which artifacts exist and whether they contain the expected content. There is no separate workflow state file. The documents ARE the state:

- Definition complete → `vision.md`, `spec.md`, `requirements.md` exist with validated content.
- Architecture complete → `architecture.md` exists and is consistent with definition docs.
- Planning complete → `implementation-plan.md` exists and task files are generated.
- Implementation complete → all tasks in `tasks/done/`.
- QA complete → system test report shows all critical/major issues resolved.

The user can always look at `docs/` and `tasks/` to understand where they are.

---

## Requirements

Cross-cutting concerns that apply across all workflows.

### Document Consistency

When any document changes, the skill that made the change must check dependent documents for inconsistencies and update them. The dependency order is: vision → spec → requirements → architecture → implementation plan → tasks. The agent must explicitly state which documents it is updating and why.

### Precondition Enforcement

Every skill validates its preconditions before starting work. Missing preconditions produce a clear message ("Cannot run `/stc:design` — definition documents not found. Run `/stc:define-product` first."). No skill silently proceeds with incomplete inputs.

### User Approval Gates

Stage transitions require explicit user approval. The agent must not proceed to the next stage until the user says to. Within a stage the agent can iterate autonomously (draft → validate → update), but completing a stage and moving forward is always a user decision.

### Architecture Impact Gate

Any change that touches the high-level architecture — whether discovered during bug fixing, maintenance, or implementation — triggers a mandatory discussion with the user before proceeding. The agent presents the architectural impact, proposes options, and waits for explicit approval.

### Task DAG Execution

When executing multiple tasks (`/stc:implement-phase` or `/stc:implement-plan`), the agent respects phase boundaries and DAG dependencies. In v1, tasks run sequentially within a phase; parallel execution is deferred to a future version. Integration tests (`/stc:qa-integration-test`) run at phase boundaries. The user can interrupt execution at any phase boundary.

### Validation Rubrics

Two rubrics applied during Definition and reused as quality gates throughout:

**Business case:**
- Do we demonstrate understanding of the business and user problem?
- Does the spec actually solve the business problem? Is there a better way?
- Is the monetization case sound?
- Are risks identified and addressed?

**Functional:**
- Is functionality defined consistently across all documents?
- Are there gaps, ambiguities, inconsistencies, or missing details?
- Is the level of detail sufficient for the next stage to proceed without guesswork?

### Scope Discipline

During implementation, the agent does not modify functionality outside the scope of the current task, phase, or approved feature. Any scope creep is flagged to the user. This applies to all workflows — the agent works within the boundaries defined by the approved plan.

---

## Traceability: Workflows → Plugin Components

Cross-reference confirming that every workflow step maps to a plugin component.

| Workflow Step | Skill (`/stc:*`) | Artifacts Produced | Guardrails (via agent) |
|--------------|----------------|--------------------|-----------------------|
| New product → Definition | `define-product` | vision, spec, requirements | core principles |
| New product → Architecture | `design` | architecture | core principles |
| New product → Planning | `plan` | implementation-plan, task files | core principles, task-management |
| New product → Implementation | `implement-task`, `implement-phase`, `implement-plan` | code, tests | core principles, task-management |
| New product → Integration test | `qa-integration-test` | integration test report | core principles |
| New product → QA | `qa-system-test` | system test report | core principles |
| Bug fix → Task creation | `create-task` | bug ticket file | task-management |
| Bug fix → Fix | `implement-task` | code, tests, root cause analysis | core principles, task-management |
| Bug fix → Integration test | `qa-integration-test` | integration test report | core principles |
| Bug fix → Regression | `qa-system-test` | system test report | core principles |
| Maintenance → Ad-hoc | `create-task`, `implement-task` | task file, code, tests | core principles, task-management |
| Maintenance → Ad-hoc → Test | `qa-integration-test` | integration test report | core principles |
| Maintenance → Optimization | `codebase-review`, `plan`, `implement-phase` | audit report, plan, code | core principles, task-management |
| Incremental feature | same as new product | same as new product (amended) | core principles, task-management |

All four workflows are fully covered by 13 skills, 1 agent, and 2 rule files. Behavioral guardrails are loaded into the session via the `stc` agent (automatic) or `/stc:init` (manual fallback). No workflow step requires a component not listed in the plugin specification.
