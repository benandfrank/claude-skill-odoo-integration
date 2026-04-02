# Odoo Integration Examples

This file complements [`SKILL.md`](SKILL.md) with compact example scenarios.

---

## Example 1 — Lightweight reporting sync

### Use case
Export `sale.order` records from Odoo to CSV every hour for internal reporting.

### Recommended shape
- sync mode: `append`
- checkpoint field: `write_date`
- sink: CSV or Google Sheets
- lock: minimal overlap protection
- tests: unit tests for mapping + one integration test for source fetch
- observability: structured logs + last successful run timestamp

### Prompt example

```text
Create a lightweight Odoo sync that exports confirmed sale.order records to CSV every hour.
Use write_date as checkpoint, prevent duplicate rows, keep overlap protection simple,
and include unit tests for mapping and checkpoint progression.
```

### Why this is lightweight
- low blast radius
- human-consumed output
- manual recovery may be acceptable

Still required:
- explicit checkpoint
- no sink-as-checkpoint anti-pattern
- no secrets in git

---

## Example 2 — Production-grade operational sync

### Use case
Sync `stock.picking` records awaiting availability from Odoo into Postgres for downstream operational workflows.

### Recommended shape
- sync mode: `upsert`
- checkpoint field: `write_date`
- deterministic ordering: `write_date asc, id asc`
- sink: Postgres
- lock: explicit run lock
- retry policy: transient source/sink failures only
- observability: structured logs, metrics, stale-sync alert
- tests: unit + integration + contract tests
- replay support: failed record capture or rerun strategy

### Prompt example

```text
Design a production-grade Odoo sync for stock.picking records in waiting or confirmed state.
Sync into Postgres using upsert semantics, write_date checkpointing, overlap prevention,
structured logs, metrics, and tests for mapping, checkpoint progression, and Odoo payload contracts.
```

### Why this is production-grade
- downstream operational dependency
- freshness matters
- duplicate or missed records create business impact
- replayability and visibility are required

---

## Example 3 — Existing script hardening

### Use case
You already have an XML-RPC script that writes to Google Sheets, but it occasionally duplicates rows and silently fails.

### Hardening goals
- move checkpointing out of the sheet
- add overlap prevention
- add structured logs
- make duplicate prevention explicit
- add tests around mapping and checkpoint advancement

### Prompt example

```text
Refactor my existing Odoo XML-RPC to Google Sheets sync.
Keep the output behavior, but move checkpointing outside the sheet, prevent overlapping runs,
add structured logs, and add tests for idempotency and resume-after-failure behavior.
```

---

## Anti-patterns to avoid

- using the sink as the checkpoint store
- using `date` as the default technical watermark without a clear reason
- allowing overlapping runs with no lock or singleton protection
- relying on debug scripts instead of tests
- parsing display fields like `origin` as durable relational identifiers when better fields exist
- assuming stock quantity fields are globally meaningful without context
- implying exactly-once guarantees without implementation support

---

## Example decision rule

Use a lightweight pattern when:
- output is mainly for humans
- blast radius is small
- manual recovery is acceptable

Use a production-grade pattern when:
- other systems depend on the data
- correctness/freshness matter operationally
- duplicates or omissions are costly
- replay and observability are necessary
