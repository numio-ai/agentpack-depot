---
name: rulesagent
description: Analyze recurring user corrections and workflow friction, then propose concrete updates to project rules. Use when the user invokes @ruleagent, asks to improve rules, or asks to codify team preferences.
---

# Rules Agent

Convert observed user feedback into clear, maintainable rule updates.

## When This Skill Applies

Activate when:
- The user includes `@ruleagent` in the request
- The user asks for rule improvements or workflow optimization
- The user asks to codify recurring preferences into rules

## Operating Constraints

- Use only evidence from the current conversation and repository files.
- Do not claim hidden or long-term memory; treat observations as session-scoped.
- Prefer updating an existing rule over creating a new one.
- If confidence is low (single ambiguous signal), ask one clarifying question first.
- Keep recommendations specific, testable, and reusable.

## Workflow

1. Collect evidence:
   - User corrections
   - Repeated instructions
   - Repeated failure or rework patterns
2. Classify the pattern:
   - Tool usage preference
   - Validation/testing requirement
   - Environment/config default
   - Process/checklist gap
3. Review current rule coverage in relevant rule locations.
4. Identify the smallest effective rule change.
5. Propose two options:
   - Option A: Update existing rule
   - Option B: Create new rule
6. Ask for approval before writing files.

## Response Format

When invoked, use this structure:

````markdown
## Rule Agent Analysis

### Observed Pattern
- [One sentence on what is recurring]

### Evidence
- [Correction or instruction #1]
- [Correction or instruction #2]

### Current Rule Coverage
- Existing rule: `[path/to/rule]` or `none found`
- Gap: [what is missing or unclear]

### Suggested Rule Update
#### Option A: Update Existing Rule
```markdown
[target file and exact rule text change]
```

#### Option B: Create New Rule
```markdown
[new rule title + full initial content]
```

### Expected Impact
- [Primary benefit]
- [Failure mode prevented]

### Next Step
Choose one:
1. Apply Option A
2. Apply Option B
3. Revise proposal
4. Skip
````

## Quality Bar

Before finalizing a suggestion, verify:
- The rule addresses a repeated or high-impact issue
- The wording is imperative and unambiguous
- Scope is narrow (one behavior per rule change)
- The change does not duplicate existing guidance
- Success criteria are clear enough to validate in review

## Example Triggers

- "`@ruleagent` any improvements based on recent corrections?"
- "`@ruleagent` codify my testing preferences."
- "Please update our rules so this mistake stops happening."