---
status: active
kind: task
title: Extend AI SDLC adoption guideline: Sections 2 and 5
---

# Extend AI SDLC adoption guideline: Sections 2 and 5

## Problem statement

Reader feedback on `docs/ai-coding--developers-adoption-guideline-1.md` flags two gaps:

1. **Section 2** has no treatment of developer skills, mindset, or what shifts during AI adoption. The section covers practices and tools, not the human change.
2. **Section 5** defers autonomous-agent practices to "a subsequent paper" (line 60) that does not exist. Readers seeking destination-state guidance — multi-agent architecture, human-machine boundaries, end-to-end workflow changes for bugs, features, and incident response — hit a dead end.

A separate workshop document already covers stepping-stone training material; that need is not addressed here.

## Scope

In scope:

- Extend `docs/ai-coding--developers-adoption-guideline-1.md` only. No new files.
- **Section 2** addition: a block on developer skills to develop, mindset shift, and what changes in day-to-day work during adoption.
- **Section 5** expansion: replace the "subsequent paper" sentence (line 60 of current file) with the autonomous-agent practices content itself — multi-agent architecture (named roles, collaboration, human oversight), what crosses each human-machine boundary, and end-to-end workflows for feature delivery, bug fix, and incident response.
- Use `docs/refs/AI Maturity Model for Engineering Teams.pdf` and `docs/refs/CapitalG - Agentic SDLC Thesis (Q1'2026).pdf` as primary source material. Cite specific findings.
- Comply with `rules/writing-guideline.md`.

Out of scope:

- New documents. All content lives in the existing guideline.
- Workshop content or training material (covered by a separate existing document).
- Implementation of any agent, tool, or system described.
- Edits outside Sections 2 and 5 and the Sources block.
- Changes to `/scripts`, `/rules`, or task-management artifacts.

## Acceptance criteria

Section 2 addition:

1. New block added alongside existing Section 2 bullets covering, at minimum:
   - **Skills to develop** — named adoption-time skills (spec writing, prompt design, AI output review, and any others surfaced by the source PDFs), each with a one-line definition.
   - **Mindset shift** — treat AI output as a first draft, verify before commit, retain ownership over judgments AI is not trusted to make.
   - **Day-to-day shift** — what changes in the developer's work during adoption.
2. New block matches existing Section 2 in style, voice, and depth. No section header changes.

Section 5 expansion:

3. The sentence "A subsequent paper covers autonomous-agent practices in depth." is removed and replaced with substantive content.
4. New content frames the autonomous-agent state as the **next sequential phase** following Sections 1–4, consistent with the existing Section 5 opener ("Once Sections 1–4 are in place..."). No timelines, year horizons, or calendar dates. The technology is developing too fast for time-bounded predictions to age well.
5. Describes the multi-agent SDLC state: named agent roles (at minimum developer-assist, QA, PR review, architecture review, cost/governance), how agents collaborate, where human oversight sits, what crosses each human-machine boundary.
6. Describes end-to-end workflow changes for at minimum: feature delivery, bug fix, incident response. Each workflow names the agent vs. human steps.
7. Preserves the existing caution that field-wide validation tooling has not caught up with operational risk at production scale.

Sources and structure:

8. Sources block updated if new citations are added.
9. No content changes outside Sections 2, 5, and the Sources block.
10. Existing five-section structure preserved. Edits are additive within sections.

Compliance:

11. Edited file passes `rules/writing-guideline.md`: every sentence under 30 words, active voice, no weasel/peacock words, all acronyms defined on first use, all dates absolute.
12. Every substantive new claim cites a specific source: `docs/refs/AI Maturity Model for Engineering Teams.pdf`, `docs/refs/CapitalG - Agentic SDLC Thesis (Q1'2026).pdf`, or a publicly verifiable external reference.

## Quality gates

- Section 5 expansion outline reviewed and approved by user before prose drafting. This is the highest-drift section — outline gate catches scope drift early.
- Final draft reviewed and approved by user before close.
- Manual writing-guideline pass: grep for weasel terms (might, could, often, significant, recently, soon, very, essentially) and relative time references; resolve every hit.
- Citations resolve to a specific page, section, or finding in the named source. Pages may be approximate where PDF text extraction is unavailable to the implementer.
- File length stays proportionate. Section 5 should not exceed the combined length of Sections 1–4.

## Constraints and assumptions

- Constraint: the five-section sequential structure remains intact. New content is additive within Sections 2 and 5; existing bullets are not rewritten.
- Constraint: no timelines, year horizons, or calendar dates anywhere in the new content. Section 5 framing continues the existing "sequential phases" model — autonomous-agent practices are the phase after Sections 1–4, not a 2-year vision.
- Constraint: vision content within Section 5 stays grounded — every substantive claim cites a source.
- Assumption: source PDFs in `docs/refs/` are authoritative for cited claims and remain available to the implementer.
- Assumption: the separate workshop document referenced by the user exists; this task does not depend on or modify it.

## Risks and rollback

- Risk: Section 5 grows too large and unbalances the doc. Mitigation: length-cap gate.
- Risk: vision drifts into speculation. Mitigation: outline reviewed before prose; every substantive claim cites a source.
- Risk: Section 2 addition duplicates or contradicts existing bullets. Mitigation: implementer reads existing Section 2 before drafting.
- Rollback: all edits revert via `git checkout` of the original file.
