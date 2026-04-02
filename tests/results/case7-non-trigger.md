# Skill Test Result — Case 7: Non-trigger control

## Test run metadata
- Date: 2026-04-02
- Reviewer: maintainer (skill content review)
- Skill version / commit: v2.0.0
- Environment / client used: direct SKILL.md content evaluation

---

## Prompt case
- Case name: Case 7 — Non-trigger control
- Prompt: Build a generic REST API polling service with retries and CSV export.
- Did the skill activate? No — the skill description explicitly says "Do NOT trigger for generic REST API tasks or non-Odoo ERP systems"

## Scores (0 to 2)
- Activation correctness: 2
- Odoo domain correctness: N/A
- Sync contract clarity: N/A
- Reliability / resiliency guidance: N/A
- Observability guidance: N/A
- Testing guidance: N/A
- Anti-pattern avoidance: 2 — no Odoo-specific guidance injected
- Practical usefulness: 2

### Total score: N/A (control case — pass/fail only)

## Expected traits observed
- skill does not activate on Odoo-specific guidance

## Forbidden traits observed
- none

## Notes
- The frontmatter description includes an explicit non-trigger rule
- This case validates that the skill description boundary is correctly set

## Decision
- Pass
