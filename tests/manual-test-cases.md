# Manual Test Cases

Use these prompts as a golden regression suite for the skill.

For each case, record:
- Did the skill activate?
- What guidance did it provide?
- Did it include the expected traits?
- Did it avoid the forbidden traits?

---

## Case 1 — Lightweight reporting sync

### Prompt
```text
Export confirmed sale.order records from Odoo v16 to CSV once per hour.
Keep it simple, but make the checkpoint explicit and avoid duplicate rows.
```

### Expected traits
- explicit lightweight posture
- explicit checkpoint field
- duplicate prevention guidance
- minimal overlap protection
- at least unit tests for mapping/checkpoint behavior

### Forbidden traits
- sink used as checkpoint store
- production-only complexity without reason
- no mention of checkpointing

---

## Case 2 — Production-grade operational sync

### Prompt
```text
Design a production-grade Odoo sync for stock.picking records awaiting availability into Postgres.
Use explicit sync mode, deterministic checkpointing, overlap prevention, structured logs,
metrics, and tests.
```

### Expected traits
- explicit sync mode
- `write_date` or clear technical checkpoint guidance
- deterministic ordering
- run lock / overlap prevention
- observability guidance
- unit + integration/contract testing guidance

### Forbidden traits
- business date used casually as watermark
- console logs only
- sink-as-checkpoint pattern
- exactly-once implied without support

---

## Case 3 — Existing script hardening

### Prompt
```text
Refactor my existing Odoo XML-RPC sync to Google Sheets.
Keep the business outcome, but move checkpointing outside the sheet,
prevent overlapping runs, and add tests.
```

### Expected traits
- preserves business outcome
- external checkpoint store
- overlap prevention
- tests beyond debug scripts
- Sheets treated as reporting sink

### Forbidden traits
- checkpoint kept in Sheets
- no testing guidance
- no operational guidance

---

## Case 4 — Reviewer mode

### Prompt
```text
Review this Odoo integration for architecture, CUPID qualities, checkpointing,
idempotency, observability, resiliency, and testing.
Identify anti-patterns and the smallest safe improvements.
```

### Expected traits
- review posture instead of implementation posture
- anti-pattern detection
- explicit smallest-safe-improvement framing
- references to uncertainty and environment verification where needed

### Forbidden traits
- rewriting everything without need
- overclaiming certainty
- ignoring runtime observability

---

## Case 5 — Write-back integration

### Prompt
```text
Design an Odoo write-back integration for purchase.order updates.
Clarify idempotency expectations, retry safety, and what must be observed at runtime.
```

### Expected traits
- write safety concerns called out
- retry guidance classified by safety
- runtime verification signals
- explicit note that write retries differ from read retries

### Forbidden traits
- blind write retries
- vague failure handling
- no runtime validation guidance

---

## Case 6 — Migration from v1 patterns

### Prompt
```text
Upgrade my old Odoo-to-Sheets skill usage to the v2 approach.
I currently use date as watermark and keep state in the sheet.
```

### Expected traits
- migration framing
- recommends `write_date` where appropriate
- recommends external checkpoint store
- explains why old pattern is fragile

### Forbidden traits
- accepts old pattern without warning
- no migration guidance

---

## Case 7 — Non-trigger control

### Prompt
```text
Build a generic REST API polling service with retries and CSV export.
```

### Expected traits
- skill should not strongly activate on Odoo-specific guidance

### Forbidden traits
- Odoo-specific model guidance
- XML-RPC-specific assumptions
- stock.picking / sale.order examples

---

## Case 8 — Reviewer prompt on weak design

### Prompt
```text
Review this Odoo sync design: it stores checkpoints in Google Sheets, runs every 5 minutes with cron,
and retries writes automatically. Identify the main risks and the smallest safe improvements.
```

### Expected traits
- catches sink-as-checkpoint anti-pattern
- flags retry safety concerns for writes
- flags overlap risk if schedule/runtime can collide
- recommends small, safe improvements instead of full rewrite

### Forbidden traits
- accepting the design as production-safe
- ignoring write-side retry risk
- no mention of checkpoint safety

---

## Case 9 — Compatibility caution

### Prompt
```text
Build an Odoo sync that depends on custom stock fields in our instance.
```

### Expected traits
- warns that target-instance verification is required
- distinguishes guidance from guaranteed field availability
- suggests field verification or introspection

### Forbidden traits
- assumes field existence universally
- overconfident cross-version claim
