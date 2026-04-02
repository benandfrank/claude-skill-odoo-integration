# Skill Test Result — Case 8: Reviewer prompt on weak design

## Test run metadata
- Date: 2026-04-02
- Reviewer: maintainer (skill content review)
- Skill version / commit: v2.0.0
- Environment / client used: direct SKILL.md content evaluation

---

## Prompt case
- Case name: Case 8 — Reviewer prompt on weak design
- Prompt: Review this Odoo sync design: it stores checkpoints in Google Sheets, runs every 5 minutes with cron, and retries writes automatically. Identify the main risks and the smallest safe improvements.
- Did the skill activate? Yes — matches Odoo, sync, review triggers

## Scores (0 to 2)
- Activation correctness: 2
- Odoo domain correctness: 2 — Odoo-specific operational caveats are documented
- Sync contract clarity: 2 — the skill explicitly flags sink-as-checkpoint and overlap issues
- Reliability / resiliency guidance: 2 — retry classification and write-side safety are explicit
- Observability guidance: 1 — observability is required but the review posture could be slightly more reviewer-directed
- Testing guidance: 1 — the skill pushes testing but the reviewer framing is less explicit than the builder framing
- Anti-pattern avoidance: 2 — all three anti-patterns in the prompt (sink checkpoint, overlap, blind retries) are documented
- Practical usefulness: 2 — the anti-patterns doc and reviewing workflow directly support this use case

### Total score: 14

## Expected traits observed
- catches sink-as-checkpoint anti-pattern
- flags retry safety concerns for writes
- flags overlap risk
- recommends small safe improvements

## Forbidden traits observed
- none

## Notes
- The skill's review posture is strong but slightly more implementation-weighted than reviewer-weighted
- Prompt templates now include reviewer-specific examples which help
- Score of 14 is acceptable; the two 1s reflect that the skill was designed primarily as a builder guide

## Decision
- Pass (acceptable, minor improvement opportunity in reviewer-directed guidance)
