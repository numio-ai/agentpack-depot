---
name: qa-system-test
description: Full system test — codebase review against specs, full test suite, e2e flows, severity report. Invoke with /stc:qa-system-test before release.
---

# QA System Test (`/stc:qa-system-test`)

## Preconditions
- `docs/spec.md` and `docs/requirements.md` (or equivalent product docs) exist.
- Implementation is intended to be **feature-complete** for the release under test.

If docs are missing, say so and ask whether to proceed with a reduced checklist.

## Purpose
- Verify the **whole product** against documented behavior.
- Run the **full automated test suite**; add or extend tests where critical paths lack coverage.
- Exercise **end-to-end user flows** from the spec.
- Produce a **system test report** with **Critical / Major / Minor** findings.

## Output
System test report including:
- Coverage summary (what was exercised)
- Issues with severity
- What was fixed vs escalated
- Sign-off recommendation (ready / not ready)

## Usage
```
/stc:qa-system-test
```

## Workflow

1. Re-read specs and requirements; build a checklist of flows and non-functional expectations.

2. Run full test suite; inspect failures; add tests for critical gaps.

3. Walk end-to-end scenarios in order of risk.

4. Categorize issues; fix autonomously where clear; escalate decisions.

5. Re-test after fixes until **Critical** and **Major** are cleared (per product policy).

6. **Gate** — User approves release readiness.
