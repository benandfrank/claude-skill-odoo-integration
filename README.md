# odoo-integration

![odoo-integration banner](banner.png)

A [Claude Code](https://claude.ai/code) skill for designing **safe, testable, observable, and resilient Odoo integrations**.

It helps Claude generate better solutions for:
- Odoo XML-RPC and version-aware integrations
- incremental sync jobs with explicit checkpointing
- exports to Google Sheets, databases, CSVs, and downstream systems
- worker/cron-based Odoo automations
- production-safe integration patterns with testing, o11y, and failure handling

Works with **Odoo v8 through v17+**.

---

## What changed in v2

This skill is no longer centered on “just make an XML-RPC script.”

For a compact migration summary, see [MIGRATION-v1-to-v2.md](MIGRATION-v1-to-v2.md).

It now teaches Claude to prefer:
- explicit sync contracts
- external checkpointing
- deterministic ordering
- overlap prevention
- structured logging and metrics
- testable architecture with clear boundaries
- safer defaults for Google Sheets and other sinks

In short: it moves from a script-first mindset to an **operable integration** mindset.

---

## When this skill is useful

Use it when you need Claude to help with:

- reading or writing Odoo models such as `stock.picking`, `sale.order`, `purchase.order`, `product.product`, or `res.partner`
- building incremental syncs from Odoo to another system
- handling Odoo field/version differences safely
- designing cron or worker-based integrations
- making an existing Odoo sync more reliable, observable, or testable
- reasoning about checkpointing, idempotency, retries, or replay

## Common prompts

```text
"Sync stock.picking records awaiting availability into Postgres every 15 minutes"
"Export confirmed sale orders from Odoo v16 to Google Sheets with idempotent upserts"
"Refactor my Odoo XML-RPC script to use checkpointing and overlap protection"
"Debug why my sync stopped advancing its checkpoint"
"Review my Odoo integration for reliability, observability, and anti-patterns"
"Turn my reporting sync into a production-grade worker with external checkpointing"
```

---

## What the skill teaches Claude

The skill teaches Claude to define and preserve these invariants:

- What problem the integration solves
- Which sync mode it uses: `append`, `upsert`, `snapshot`, or `reconcile`
- Which field is the checkpoint and why
- What the idempotency key is
- How overlap is prevented
- How runtime success is observed
- How failures are classified, retried, and replayed
- What tests prove the intended behavior

---

## Compact reference architecture

The skill now prefers this compact project structure:

```text
odoo-sync/
├── src/
│   ├── app/
│   │   └── run-sync.js
│   ├── domain/
│   │   ├── sync-policy.js
│   │   ├── mapping.js
│   │   └── queries/
│   ├── ports/
│   │   ├── odoo-gateway.js
│   │   ├── checkpoint-store.js
│   │   ├── output-sink.js
│   │   └── run-lock.js
│   ├── adapters/
│   │   ├── odoo/xmlrpc-client.js
│   │   ├── checkpoint/sqlite.js
│   │   ├── sink/google-sheets.js
│   │   └── observability/logger.js
│   └── config/
│       └── env.js
├── tests/
├── scripts/
├── .env.example
└── package.json
```

### Why this structure

It separates:
- domain rules
- Odoo transport
- checkpoint persistence
- sink behavior
- runtime orchestration
- observability

That makes the integration more:
- composable
- predictable
- testable
- maintainable

---

## Test matrix

The skill encourages the lowest-level test that gives confidence.

| Test level | Purpose | Typical Odoo integration examples |
|---|---|---|
| Unit | Validate logic and invariants | domain builders, mapping, checkpoint progression, idempotency rules |
| Integration | Validate collaboration at boundaries | XML-RPC adapter behavior, sink writes, checkpoint store, retry handling |
| Contract | Validate payload assumptions | recorded Odoo responses, version-specific fields, many2one shapes |
| End-to-end | Validate critical runtime path | one essential sync flow, deployment smoke path |

### Minimum scenarios

- happy path
- duplicate records
- out-of-order updates
- missing optional fields
- transient source failure
- transient sink failure
- partial mapping failure
- overlap prevention
- resume after interruption

---

## Decision guide: small integration vs production integration

Use this guide to choose the right level of engineering.

### Small integration

Appropriate when:
- low volume
- internal visibility/reporting use case
- limited blast radius
- manual recovery is acceptable
- one operator or a small team owns it

Reasonable choices:
- simple worker or cron
- lightweight checkpoint store
- Google Sheets as a reporting sink
- basic structured logs
- unit + a few integration tests

Still required:
- explicit sync mode
- explicit checkpoint field
- no secrets in git
- no sink-as-checkpoint anti-pattern
- at least minimal overlap protection

### Production integration

Appropriate when:
- higher volume or business-critical workflow
- downstream systems depend on freshness/correctness
- failures cause customer, operational, or financial impact
- replayability and auditability matter
- multiple environments or teams are involved

Recommended choices:
- external scheduler or singleton worker
- durable checkpoint store
- explicit run lock
- structured logs + metrics + alerts
- integration and contract tests
- replay support for failed records
- documented retry policy and failure taxonomy

### Escalation signals

Treat a “small” integration as needing production hardening when you see:
- overlapping runs
- manual checkpoint edits
- repeated duplicate writes
- hidden failures discovered by users
- long-running syncs
- business reliance increasing over time

---

## Odoo-specific guidance included in the skill

The skill includes guidance for:
- `stock.picking`
- `stock.move`
- `sale.order`
- `purchase.order`
- `product.product` / `product.template`
- `res.partner`

It also teaches important caveats such as:
- many2one fields returning `[id, display_name]`
- version differences like `move_lines` vs `move_ids`
- `write_date` usually being a better technical checkpoint than business date fields
- stock quantities being context-sensitive
- avoiding fragile parsing of display fields like `origin`

---

## Installation

```bash
git clone git@github.com:benandfrank/claude-skill-odoo-integration.git \
  ~/.claude/skills/odoo-integration
```

Claude Code discovers skills automatically from `~/.claude/skills/`.

---

## Quick start

1. Install the skill
2. Open a project where you want help with an Odoo integration
3. Ask Claude for the outcome you want, not just the transport details
4. Let the skill guide the design toward explicit checkpointing, idempotency, testing, and observability

Example:

```text
Build a worker-based Odoo sync from stock.picking to Postgres.
Use write_date as the checkpoint, prevent overlapping runs,
add structured logs and metrics, and include unit + integration tests.
```

If your use case is smaller, you can also ask for a lightweight reporting sync:

```text
Export confirmed sale.order records from Odoo v16 to CSV once per hour.
Keep it simple, but make the checkpoint explicit and avoid duplicate rows.
```

For more detailed patterns, see:
- [`SKILL.md`](SKILL.md)
- [`EXAMPLES.md`](EXAMPLES.md)
- [`.env.example`](.env.example)
- [`docs/README.md`](docs/README.md)
- [`docs/operability.md`](docs/operability.md)
- [`docs/glossary.md`](docs/glossary.md)
- [`docs/review-checklist.md`](docs/review-checklist.md)
- [`docs/review-workflow.md`](docs/review-workflow.md)
- [`docs/decision-tree.md`](docs/decision-tree.md)
- [`docs/FAQ.md`](docs/FAQ.md)
- [`docs/prompt-templates.md`](docs/prompt-templates.md)
- [`docs/anti-patterns.md`](docs/anti-patterns.md)
- [`docs/principles.md`](docs/principles.md)
- [`docs/release-checklist.md`](docs/release-checklist.md)
- [`docs/guarantees-vs-assumptions.md`](docs/guarantees-vs-assumptions.md)
- [`TESTING.md`](TESTING.md)
- [`tests/manual-test-cases.md`](tests/manual-test-cases.md)
- [`tests/results-template.md`](tests/results-template.md)
- [`MIGRATION-v1-to-v2.md`](MIGRATION-v1-to-v2.md)

---

## Requirements

- [Claude Code](https://claude.ai/code)
- An Odoo instance with API access
- Node.js 18+ in your project if you want Claude to generate Node-based implementation scaffolding directly
- A sink such as Google Sheets, Postgres, CSV, or another downstream system

Note: the skill can still advise on architecture, checkpoints, testing, and Odoo semantics even if your final target implementation uses a different runtime. Node.js 18+ is the preferred default for the concrete scaffolding taught by this repo.

---

## Recommended defaults taught by the skill

If the user does not specify otherwise, the skill tends to prefer:
- a dedicated Odoo gateway
- `write_date` as checkpoint field
- deterministic ordering with `id` tie-breaker
- checkpoint storage outside the sink
- structured logs
- basic runtime metrics
- overlap prevention
- tests around mapping, checkpointing, and boundaries
- Google Sheets as a reporting sink, not the control plane
- worker or scheduler designs that can evolve beyond Railway-only deployments

---

## Reading order by persona

### New user
1. [`README.md`](README.md)
2. [`docs/glossary.md`](docs/glossary.md)
3. [`docs/decision-tree.md`](docs/decision-tree.md)
4. [`EXAMPLES.md`](EXAMPLES.md)

### Reviewer
1. [`docs/review-workflow.md`](docs/review-workflow.md)
2. [`docs/review-checklist.md`](docs/review-checklist.md)
3. [`docs/guarantees-vs-assumptions.md`](docs/guarantees-vs-assumptions.md)
4. [`SKILL.md`](SKILL.md)

### Maintainer
1. [`MAINTAINERS.md`](MAINTAINERS.md)
2. [`CONTRIBUTING.md`](CONTRIBUTING.md)
3. [`CHANGELOG.md`](CHANGELOG.md)
4. [`docs/release-checklist.md`](docs/release-checklist.md)

---

## How to review a generated integration

Use [`docs/review-workflow.md`](docs/review-workflow.md) as the fast path, and [`docs/review-checklist.md`](docs/review-checklist.md) as the compact checklist.

At minimum, check that the generated integration makes these explicit:
- sync mode
- checkpoint field and ordering
- idempotency key
- overlap prevention
- observable runtime signals
- test coverage at the right level

---

## Prompt anti-patterns

Avoid prompts like:
- "Make a quick Odoo script to dump data somewhere"
- "Sync Odoo to Sheets" without stating checkpointing or duplicate-handling expectations
- "Use cron and XML-RPC" when the real need is a reliable operational integration

Prefer prompts like:
- "Build an idempotent Odoo sync with explicit checkpointing and overlap prevention"
- "Design a reporting sync and keep the sink separate from the checkpoint store"
- "Review this Odoo integration for reliability, observability, and anti-patterns"

---

## Release notes

See [CHANGELOG.md](CHANGELOG.md) for the v2 summary.

---

## Maintainers

If you maintain this skill, keep [`README.md`](README.md), [`SKILL.md`](SKILL.md), and [`EXAMPLES.md`](EXAMPLES.md) aligned. See [MAINTAINERS.md](MAINTAINERS.md).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution expectations and review checks.

---

## Credits

Created by [@Txampa](https://github.com/txampa) - JeanOnCode.

Originally inspired by a real production integration on Odoo v8. Expanded and modernized to support safer patterns across Odoo v8–v17+.

---

## Compatibility note

This skill is designed to help with **Odoo v8 through v17+** patterns, but real-world compatibility still depends on:
- installed Odoo modules
- custom fields and custom workflows
- API exposure and access rights
- company, warehouse, and context-specific behavior

Treat version guidance in this repo as a strong starting point, not a guarantee that every field or workflow exists unchanged in every instance. When correctness depends on a field, model, or relation, verify it in the target Odoo instance.

---

## License

MIT — free to use, modify, and distribute. See [LICENSE.txt](LICENSE.txt).
