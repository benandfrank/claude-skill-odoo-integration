# Testing the Skill

This repo is a skill product, so the most effective testing approach is a **golden prompt regression suite** plus a **review rubric**.

Use three layers of testing.

---

## 1. Activation tests

Goal:
- verify the skill activates when it should
- verify it does not trigger for unrelated prompts

### Odoo prompts that should trigger
- "Sync stock.picking to Postgres every 15 minutes"
- "Review my Odoo XML-RPC integration"
- "Export confirmed sale.order records to Google Sheets"
- "Refactor my Odoo sync to use checkpointing and overlap prevention"

### Prompts that should not trigger
- "Build a generic REST polling service"
- "Integrate SAP"
- "Create a CRM dashboard"
- "Build a generic webhook consumer"

### Pass criteria
- Odoo integration prompts activate the skill
- unrelated prompts do not pick up the Odoo skill unnecessarily

---

## 2. Behavior tests

Goal:
- verify the skill produces the intended engineering guidance

For each generated answer, check whether it makes these explicit when appropriate:
- sync mode
- checkpoint field and ordering
- idempotency key or equivalent idempotency strategy
- overlap prevention
- observability signals
- test strategy
- sink choice rationale
- environment-specific verification caveats

### Common failure signals
- uses the sink as checkpoint store
- defaults to `date` instead of `write_date` without justification
- relies only on debug scripts instead of tests
- recommends overlapping cron without protection
- implies exactly-once semantics without support
- gives overconfident compatibility claims for all Odoo instances

---

## 3. Outcome tests

Goal:
- verify the skill is useful on realistic prompts

Take a small set of representative prompts and evaluate whether the answer is:
- correct enough for Odoo
- practical
- observable
- testable
- resilient
- aligned with the repo's principles

This layer matters most because it validates the skill as a usable product, not just a document.

---

## Recommended test workflow

1. Run prompts from [`tests/manual-test-cases.md`](tests/manual-test-cases.md)
2. Evaluate answers using:
   - [`docs/reviewing.md`](docs/reviewing.md)
   - [`docs/anti-patterns.md`](docs/anti-patterns.md)
   - [`docs/stance.md`](docs/stance.md)
3. Record whether the skill:
   - triggered correctly
   - produced the expected guidance
   - avoided forbidden guidance
4. Compare results over time to detect regressions
5. Record results using [`tests/results-template.md`](tests/results-template.md)

---

## Suggested scoring rubric

Score each prompt from 0 to 2 in each category:
- Activation correctness
- Odoo domain correctness
- Sync contract clarity
- Reliability / resiliency guidance
- Observability guidance
- Testing guidance
- Anti-pattern avoidance
- Practical usefulness

### Interpretation
- 14–16: excellent
- 10–13: acceptable but review needed
- below 10: regression or weak skill response

---

## How to interpret regressions

Treat these as likely regressions that need review before release:
- activation becomes too broad or too narrow
- responses stop making checkpointing explicit
- `date` replaces `write_date` without clear justification
- Google Sheets is treated as the checkpoint store again
- debug scripts start replacing tests in recommendations
- observability becomes optional or disappears
- compatibility claims become overconfident

A lower score is not the only signal. A single severe anti-pattern can matter more than a small score delta.

---

## When to run the suite

- Before any release that changes `SKILL.md` behavior or defaults
- After significant changes to `EXAMPLES.md`, `docs/operability.md`, or `docs/anti-patterns.md`
- Periodically (e.g. quarterly) to check for drift even without changes
- After upgrading the underlying model or client environment

Full suite runs are most valuable before releases. Spot checks on 2–3 prompts are useful as a quick sanity check during development.

---

## Top 5 regressions to watch

1. **Checkpoint in the sink** — the skill recommends storing checkpoints in Google Sheets or the output target
2. **No overlap prevention** — the response uses cron without any lock or singleton protection
3. **Debug scripts as tests** — the response treats exploratory scripts as sufficient testing
4. **Overconfident compatibility** — the response assumes fields or states exist in all Odoo instances without caveats
5. **Missing observability** — the response does not mention structured logs, metrics, or runtime signals

A single occurrence of any of these is worth investigating before release.

---

## Activation / non-trigger quick checklist

Use this for fast activation-only testing without full scoring:

- [ ] "Sync stock.picking to Postgres" → skill activates
- [ ] "Export sale.order to CSV" → skill activates
- [ ] "Review my Odoo XML-RPC integration" → skill activates
- [ ] "Build a generic REST polling service" → skill does NOT activate on Odoo guidance
- [ ] "Integrate SAP" → skill does NOT activate on Odoo guidance
- [ ] "Build a webhook consumer" → skill does NOT activate on Odoo guidance

---

## Minimum release gate

Before publishing significant skill changes:
- run the golden prompts in [`tests/manual-test-cases.md`](tests/manual-test-cases.md)
- review outputs using [`docs/reviewing.md`](docs/reviewing.md)
- confirm no regression in checkpointing, idempotency, observability, or testing guidance
- update [`CHANGELOG.md`](CHANGELOG.md) if skill behavior changed materially

## Example results

See [`tests/results/`](tests/results/) for example scored outputs.
