# Skill Test Result — Case 1: Lightweight reporting sync

## Test run metadata
- Date: 2026-04-02
- Reviewer: maintainer (skill content review)
- Skill version / commit: v2.0.0
- Environment / client used: direct SKILL.md content evaluation

---

## Prompt case
- Case name: Case 1 — Lightweight reporting sync
- Prompt: Export confirmed sale.order records from Odoo v16 to CSV once per hour. Keep it simple, but make the checkpoint explicit and avoid duplicate rows.
- Did the skill activate? Yes — matches sale.order, Odoo, sync, and export triggers

## Scores (0 to 2)
- Activation correctness: 2
- Odoo domain correctness: 2 — sale.order states and fields are documented
- Sync contract clarity: 2 — skill requires explicit sync mode even for lightweight cases
- Reliability / resiliency guidance: 1 — lightweight framing is present but retry/failure guidance is production-weighted
- Observability guidance: 2 — structured logs required even for lightweight syncs
- Testing guidance: 2 — unit tests for mapping and checkpoint progression explicitly required
- Anti-pattern avoidance: 2 — sink-as-checkpoint and debug-as-test anti-patterns are explicit
- Practical usefulness: 2 — decision guide and examples cover this exact case

### Total score: 15

## Expected traits observed
- explicit lightweight posture via decision guide and examples
- explicit checkpoint field guidance (write_date)
- duplicate prevention via idempotency key
- minimal overlap protection via run lock guidance
- unit tests for mapping and checkpoint progression

## Forbidden traits observed
- none

## Notes
- The skill does not push unnecessary production complexity for lightweight cases
- The decision guide clearly separates lightweight from production-grade expectations

## Decision
- Pass
