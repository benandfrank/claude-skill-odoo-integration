# Skill Test Result — Case 2: Production-grade operational sync

## Test run metadata
- Date: 2026-04-02
- Reviewer: maintainer
- Skill version / commit: v2.0.0 (6b2ad1a)
- Environment / client used: Claude Code with skill installed at ~/.claude/skills/odoo-integration

---

## Prompt case
- Case name: Case 2 — Production-grade operational sync
- Prompt: Design a production-grade Odoo sync for stock.picking records awaiting availability into Postgres. Use explicit sync mode, deterministic checkpointing, overlap prevention, structured logs, metrics, and tests.
- Did the skill activate? Yes

## Scores (0 to 2)
- Activation correctness: 2
- Odoo domain correctness: 2
- Sync contract clarity: 2
- Reliability / resiliency guidance: 2
- Observability guidance: 2
- Testing guidance: 2
- Anti-pattern avoidance: 2
- Practical usefulness: 2

### Total score: 16

## Expected traits observed
- explicit sync mode (upsert)
- write_date checkpoint with deterministic ordering
- run lock / overlap prevention
- structured logs and metrics
- unit + integration + contract test guidance

## Forbidden traits observed
- none

## Notes
- Response correctly distinguished between Odoo model states and sink upsert semantics
- Recommended verifying field availability in the target instance
- Did not overclaim exactly-once behavior

## Decision
- Pass
