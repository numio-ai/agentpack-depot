---
name: design
description: Architecture stage — produce high-level architecture document. Invoke with /stc:design after definition docs exist.
---

# Design (`/stc:design`)

## Preconditions
**Required before starting:**
- `docs/vision.md`
- `docs/spec.md`
- `docs/requirements.md`

If any are missing, stop and tell the user: *Cannot run `/stc:design` — definition documents not found. Run `/stc:define-product` first.*

## Output
| Artifact | Path |
|----------|------|
| High-level architecture | `docs/architecture.md` |

**In scope:** technology choices, system architecture, domain dictionary (key terms), workflows and **key** APIs, security mechanisms.

**Out of scope:** detailed API signatures, database schemas, function-level design — those belong in task-level detailed design during implementation.

## Usage
```
/stc:design
```

## Workflow

1. Draft `docs/architecture.md` from the definition documents.

2. **Review cycle** with the user: incorporate feedback.

3. **Self-validation** — produce a report covering: completeness, consistency with specs, over/under-engineering, design-principle issues.

4. **Document consistency** — If architectural decisions change scope (e.g. defer a feature), update `docs/spec.md` and/or `docs/requirements.md` and state why. Chain: vision → spec → requirements → architecture.

5. **Gate** — User explicitly approves before `/stc:plan`.

## Discipline
- Do not invent requirements; align with definition docs or flag conflicts to the user.
- Architecture impact that contradicts prior commitments requires explicit user agreement.
