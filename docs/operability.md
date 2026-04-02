# Operability Guide

This page summarizes the core operational guidance behind the skill.

---

## 1. Checkpointing patterns

A checkpoint is the integration's notion of progress.

### Preferred default
Use:
- checkpoint field: `write_date`
- deterministic order: `write_date asc, id asc`

Why:
- `write_date` is usually a better technical watermark than business date fields
- `id` provides a deterministic tie-breaker when multiple rows share the same timestamp

### Good checkpoint rules
- store checkpoints outside the output sink
- commit the checkpoint only after successful durable sink write
- make checkpoint progression testable
- document replay behavior

### Avoid
- using Google Sheets as the authoritative checkpoint store
- relying on business dates by default
- advancing checkpoints before sink success
- assuming exactly-once semantics

---

## 2. Sink selection guide

Choose the sink based on the outcome and blast radius.

### Google Sheets
Best for:
- lightweight reporting
- human-readable exports
- small operational dashboards

Not ideal for:
- high-volume syncs
- checkpoint persistence
- strong concurrency guarantees
- system-of-record workflows

### CSV
Best for:
- simple exports
- batch handoffs
- local workflows or analyst consumption

### Postgres or similar database
Best for:
- operational syncs
- upsert behavior
- replayability
- downstream system dependencies
- audit-friendly workflows

### Rule of thumb
If correctness, freshness, and replay matter, prefer a durable data store over a presentation sink.

---

## 3. Observability checklist

An integration is not complete if runtime success is invisible.

### Structured logs
Include at least:
- run ID
- sync policy name
- source model
- checkpoint before and after
- processed, written, skipped, failed counts
- duration
- result status

### Metrics
Track at least:
- sync duration
- source fetch latency
- sink write latency
- retries
- failure counts
- last successful run timestamp
- stale sync age
- overlap-prevention count

### Alerts
Strong candidates for alerting:
- stale sync
- repeated failures
- checkpoint not advancing
- zero records when not expected
- abnormal mapping failures

### Support tooling
Helpful scripts and modes:
- dry-run mode
- field inspection
- state inspection
- failed-record replay
- checkpoint inspection
