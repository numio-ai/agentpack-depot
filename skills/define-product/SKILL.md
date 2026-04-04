---
name: define-product
description: Definition stage — produce vision, product specification, and requirements in docs/. Invoke with /stc:define-product when starting discovery for a new or incremental product.
---

# Definition (`/stc:define-product`)

## Preconditions
- **None.** This skill may create `docs/` and initial documents if they do not exist.
- For **incremental features**, existing `docs/vision.md`, `docs/spec.md`, and `docs/requirements.md` are the baseline; produce **additions/amendments** so the product (including the new feature) is fully described — no separate "feature-only" docs.

## Outputs
| Artifact | Path |
|----------|------|
| Vision | `docs/vision.md` |
| Specification | `docs/spec.md` |
| Requirements | `docs/requirements.md` |

## Usage
```
/stc:define-product
```
Collaborate with the user through drafts and feedback until they approve proceeding.

## Workflow

1. **Vision** — Interview the user (problem, users, capabilities). Draft a one-page `docs/vision.md`. Iterate until the user is satisfied with the direction.

2. **Specification** — Using the approved vision plus any references (products, docs, domain notes), draft `docs/spec.md`: functionality, user flows, interactions. Iterate.

3. **Requirements** — Draft `docs/requirements.md` to formalize and disambiguate the spec. Iterate.

4. **Validation** — Run two rubrics and produce a short **validation report**:
   - **Business case:** problem understanding, whether the spec solves it, alternatives, monetization, risks.
   - **Functional completeness:** consistency, gaps, ambiguities, missing detail for design/implementation.

5. **Consistency** — If any document changes, check downstream docs in the chain: vision → spec → requirements. State explicitly what you updated and why.

6. **Gate** — Do not treat the stage as complete until the **user** explicitly approves moving to `/stc:design`.

**Discipline:** Do not silently skip validation. Do not proceed to architecture without user approval to leave this stage.

