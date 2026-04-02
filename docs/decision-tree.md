# Decision Tree

Use this guide to choose an appropriate sync mode and sink.

## 1. What is the integration trying to do?

### A. Produce a human-readable report
Prefer:
- sync mode: `append` or simple `snapshot`
- sink: CSV or Google Sheets
- checkpoint: explicit, outside the sink if the job is recurring

### B. Keep another system aligned with Odoo records
Prefer:
- sync mode: `upsert`
- sink: Postgres or another durable database
- checkpoint: `write_date` with deterministic ordering

### C. Rebuild a full current-state dataset regularly
Prefer:
- sync mode: `snapshot`
- sink: database, files, or warehouse depending on scale
- note: make replacement semantics explicit

### D. Detect and correct drift between systems
Prefer:
- sync mode: `reconcile`
- sink: durable store or downstream system with comparison support
- note: replay and auditability matter more here

---

## 2. How important is freshness and correctness?

### Low to moderate
- reporting sink may be enough
- basic overlap prevention may be enough
- structured logs are still required

### High
- durable sink preferred
- explicit run lock required
- metrics and alerting required
- replay behavior should be defined

---

## 3. Which sink should you choose?

### Google Sheets
Choose when:
- humans consume the output
- volume is low
- operational risk is low

Avoid when:
- checkpointing must be durable
- concurrency is likely
- downstream systems rely on the data as a source of truth

### CSV
Choose when:
- output is periodic and simple
- downstream ingestion is manual or batch-based

### Postgres or similar
Choose when:
- you need upserts
- you need replayability
- you need auditability
- other systems depend on the data

---

## 4. Default decision rule

If unsure:
- use `write_date` as checkpoint field
- use deterministic ordering with `id` tie-breaker
- keep checkpoint storage outside the sink
- choose `upsert` + durable database for operational integrations
- choose `append` + reporting sink for lightweight reporting integrations
