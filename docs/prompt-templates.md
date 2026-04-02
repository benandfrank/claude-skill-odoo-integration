# Prompt Templates

Use these templates when asking Claude for help with Odoo integrations.

## 1. Lightweight reporting sync

```text
Build a lightweight Odoo reporting sync for [MODEL] into [SINK].
Use [CHECKPOINT_FIELD] as checkpoint, avoid duplicate rows,
keep the sink separate from the checkpoint store, and include minimal overlap protection.
Also include unit tests for mapping and checkpoint progression.
```

## 2. Production-grade operational sync

```text
Design a production-grade Odoo sync for [MODEL] into [SINK].
Use explicit sync mode, write_date checkpointing with deterministic ordering,
overlap prevention, structured logs, runtime metrics, and tests for mapping,
checkpoint progression, and boundary behavior.
```

## 3. Existing script hardening

```text
Refactor my existing Odoo XML-RPC sync.
Preserve the business outcome, but move checkpointing outside the sink,
prevent overlapping runs, add structured logs, classify retry behavior,
and add tests for idempotency and resume-after-failure behavior.
```

## 4. Review request

```text
Review this Odoo integration for architecture, CUPID qualities,
checkpointing, idempotency, observability, resiliency, and testing.
Identify anti-patterns and suggest the smallest safe improvements.
```

## Reviewer prompt examples

```text
Review this generated Odoo sync using the repo's review checklist, anti-patterns,
and guarantees-vs-assumptions guidance. Tell me what is explicit, what is risky,
and what still needs target-instance verification.
```

```text
Act as a reviewer for this Odoo integration. Focus on checkpoint safety,
overlap prevention, sink choice, observability, and whether the tests match the risk.
```

## 5. Write-back / command integration

```text
Design an Odoo write-back integration for [MODEL/ACTION].
Clarify idempotency expectations, failure handling, retry safety,
and what must be observed at runtime to verify correctness.
```

## Bad prompt → better prompt examples

### Too vague
```text
Make a quick Odoo script to sync orders to Sheets.
```

### Better
```text
Build a lightweight Odoo reporting sync for sale.order into Google Sheets.
Use write_date as checkpoint, keep checkpoint storage outside the sheet,
avoid duplicate rows, add minimal overlap protection, and include unit tests for mapping.
```

### Too implementation-led
```text
Use cron and XML-RPC to dump stock data somewhere.
```

### Better
```text
Design an Odoo sync for stock.picking records awaiting availability.
Recommend the right sink, make sync mode explicit, use deterministic checkpointing,
prevent overlapping runs, and include observability and test guidance.
```

## Prompt hygiene tips

Prefer prompts that make these explicit:
- model
- sink or downstream target
- sync mode
- checkpoint expectation
- reliability expectations
- testing expectations

Avoid prompts that ask only for:
- “a quick script”
- “just sync it somewhere”
- “use Sheets” without duplicate-handling or checkpoint rules
