---
name: odoo-integration
version: "2.0.0"
description: "Use this skill when the user needs to integrate with Odoo safely and observably: reading or writing Odoo models, synchronizing Odoo data to external systems, designing cron or worker-based sync jobs, handling Odoo XML-RPC or version-aware API access, or deploying reliable Odoo integrations. Also use when the user mentions stock.picking, sale.order, purchase.order, albaranes, pedidos, ERP sync, Odoo automation, reconciliation, checkpointing, or idempotent exports. Do NOT trigger for generic REST API tasks or non-Odoo ERP systems."
license: MIT. See LICENSE.txt
---

# Odoo Integration Skill

You are an expert in Odoo integrations and operationally safe sync design.

You know how to integrate with Odoo using XML-RPC and version-aware access patterns across Odoo v8 through v17+. You design integrations that are:

- small and testable
- observable in runtime
- idempotent by design
- resilient to transient failures
- explicit about checkpointing and replay
- aligned with domain language, not only transport details

You do not default to fragile script patterns when the use case implies production reliability, auditability, or scale.

For supporting material and compact usage examples, also see:
- [`README.md`](README.md)
- [`EXAMPLES.md`](EXAMPLES.md)
- [`docs/operability.md`](docs/operability.md)
- [`docs/decision-tree.md`](docs/decision-tree.md)
- [`docs/FAQ.md`](docs/FAQ.md)
- [`docs/prompt-templates.md`](docs/prompt-templates.md)
- [`docs/anti-patterns.md`](docs/anti-patterns.md)
- [`docs/guarantees-vs-assumptions.md`](docs/guarantees-vs-assumptions.md)
- [`MIGRATION-v1-to-v2.md`](MIGRATION-v1-to-v2.md)

## Docs map

Use the repo docs like this:
- start with [`README.md`](README.md) for positioning, quick start, and reading order
- use [`EXAMPLES.md`](EXAMPLES.md) for concrete scenarios
- use [`docs/operability.md`](docs/operability.md) for runtime guidance
- use [`docs/decision-tree.md`](docs/decision-tree.md) for sink and sync-mode selection
- use [`docs/FAQ.md`](docs/FAQ.md) for recurring questions
- use [`docs/prompt-templates.md`](docs/prompt-templates.md) for safer prompt shapes
- use [`docs/anti-patterns.md`](docs/anti-patterns.md) for common failure modes
- use [`docs/guarantees-vs-assumptions.md`](docs/guarantees-vs-assumptions.md) to understand guidance vs environment-specific verification
- use [`MIGRATION-v1-to-v2.md`](MIGRATION-v1-to-v2.md) when adapting older skill usage

---

## Operating Principles

For every Odoo integration:

1. Define the business intent first
   - What job is the integration doing?
   - Is it append-only reporting, upsert sync, reconciliation, or command/write-back?

2. Make the sync contract explicit
   - source model
   - selection criteria
   - sync mode
   - checkpoint field
   - idempotency key
   - replay behavior
   - failure behavior

3. Separate concerns
   - transport/access to Odoo
   - domain mapping and rules
   - checkpoint persistence
   - output sink
   - scheduling/orchestration
   - observability

4. If it is not observable in runtime, it is not complete

5. Prefer at-least-once processing with an idempotent sink unless stronger guarantees are truly implemented

---

## Recommended Architecture

Do not put business logic directly inside transport clients or cron callbacks.

Prefer this structure:

```text
odoo-sync/
├── src/
│   ├── app/
│   │   └── run-sync.js
│   ├── domain/
│   │   ├── sync-policy.js
│   │   ├── mapping.js
│   │   ├── invariants.js
│   │   └── queries/
│   ├── ports/
│   │   ├── odoo-gateway.js
│   │   ├── checkpoint-store.js
│   │   ├── output-sink.js
│   │   └── run-lock.js
│   ├── adapters/
│   │   ├── odoo/xmlrpc-client.js
│   │   ├── checkpoint/sqlite.js
│   │   ├── checkpoint/postgres.js
│   │   ├── sink/google-sheets.js
│   │   └── observability/logger.js
│   ├── config/
│   │   └── env.js
│   └── main.js
├── tests/
├── scripts/
├── .env.example
├── Procfile
└── package.json
```

### Responsibility boundaries

- `app/run-sync.js`
  - orchestration only
- `domain/*`
  - business terms, mappings, query builders, invariants
- `ports/*`
  - capability contracts
- `adapters/*`
  - concrete implementations
- `config/env.js`
  - config parsing and validation
- `scripts/*`
  - diagnostics, exploration, replay, support

This structure is preferred because it is:

- **Composable**
- **Predictable**
- **Testable**
- **Operationally clearer**

---

## Sync Contract — define this before coding

Every integration should define a sync contract like this:

```js
const syncPolicy = {
  name: 'stock-picking-awaiting-availability',
  sourceModel: 'stock.picking',
  mode: 'append', // append | upsert | snapshot | reconcile
  checkpoint: {
    field: 'write_date',
    order: ['write_date asc', 'id asc'],
  },
  identity: {
    sourceIdField: 'id',
    idempotencyKey: 'id',
  },
  selection: {
    states: ['waiting', 'confirmed'],
  },
  sink: {
    type: 'google-sheets',
  },
};
```

### Preferred defaults

- checkpoint field: `write_date`
- deterministic ordering: `write_date asc, id asc`
- sink writes: idempotent if possible
- checkpoint commit: only after successful durable sink write

### Avoid

- using `date` as a checkpoint unless the business case truly requires it
- using the sink as the authoritative checkpoint store
- parsing human display fields such as `origin` as primary relational identifiers

---

## Odoo Access Layer

Always encapsulate Odoo access behind a gateway. Do not scatter raw XML-RPC calls through business logic.

### Required gateway capabilities

- authenticate
- search
- read
- search_read
- create
- write
- paginated fetch
- field existence checks
- optional context support
- timeout and retry policy for safe transient failures

### Example shape

```javascript
// ports/odoo-gateway.js
class OdooGateway {
  async authenticate() {}
  async search(model, domain, options = {}) {}
  async read(model, ids, fields = [], options = {}) {}
  async searchRead(model, domain, fields = [], options = {}) {}
  async create(model, values, options = {}) {}
  async write(model, ids, values, options = {}) {}
  async fieldsGet(model, attributes = []) {}
}
```

### XML-RPC adapter guidelines

- Support both HTTP and HTTPS by config
- Validate environment variables at startup
- Add request timeout
- Retry only transient transport errors
- Never blindly retry writes unless the operation is proven idempotent
- Support page iteration for large result sets

### Configuration notes

Odoo instances vary by:
- version
- custom modules
- enabled models/fields
- company and warehouse context
- user access rights

Always verify fields in the target instance when correctness depends on them.

---

## Odoo Domain Knowledge

Use domain language, not only raw strings.

### `stock.picking`
Business meaning:
- delivery order / transfer / warehouse operation

Common states:
- `draft`
- `waiting`
- `confirmed`
- `assigned`
- `done`
- `cancel`

Typical reporting use case:
- pending outbound operations waiting for availability

Caveats:
- fields vary by version and customization
- `move_lines` vs `move_ids` depends on version/use case
- `origin` is a display/business field, not a stable relationship contract
- prefer relational fields if available

### `stock.move`
Business meaning:
- inventory movement line

Caveats:
- state meaning is operational and can vary with workflows
- quantities may reflect reservation or execution state depending on field used

### `sale.order`
Business meaning:
- sales order lifecycle

Caveats:
- state alone may not express fulfillment/invoicing status
- linkages to pickings/invoices should be validated in the target instance

### `purchase.order`
Business meaning:
- purchasing lifecycle and inbound dependencies

### `product.product` and `product.template`
Business meaning:
- variant vs template

Important warning:
- stock fields such as `qty_available` are context-sensitive
- company, location, warehouse, and reservation context matter
- do not assume `qty_available` is the business answer without clarifying context

### `res.partner`
Business meaning:
- customer/vendor/contact entity

Important warning:
- data quality often varies significantly in production instances
- missing emails, duplicate names, and multi-address patterns are common

---

## Version Compatibility Rules

The XML-RPC protocol is broadly stable across Odoo versions, but models and fields differ.

Known examples:

| Concept | Older versions | Newer versions | Safe approach |
|--------|----------------|----------------|---------------|
| picking move fields | `move_lines` | `move_ids` | detect and fallback |
| scheduled date | `min_date` | `scheduled_date` | detect and fallback |
| invoice model | `account.invoice` | `account.move` | ask version or introspect |
| detailed ops | varies | `move_line_ids` | verify before use |

### Safe compatibility pattern

```javascript
const moveIds = picking.move_ids || picking.move_lines || [];
const scheduledDate = picking.scheduled_date || picking.min_date || null;
```

When the version is unknown:
- use safe fallbacks
- add comments
- verify fields exist before depending on them

---

## Query and Mapping Guidance

Avoid stringly-typed logic scattered across the codebase.

Prefer:
- query builders
- field constants
- state constants
- mapping helpers

Example:

```javascript
const PickingStates = {
  WAITING: 'waiting',
  CONFIRMED: 'confirmed',
};

function buildPendingPickingDomain(checkpoint) {
  const domain = [['state', 'in', [PickingStates.WAITING, PickingStates.CONFIRMED]]];
  if (checkpoint?.writeDate) {
    domain.push(['write_date', '>=', checkpoint.writeDate]);
  }
  return domain;
}
```

This improves:
- readability
- testability
- refactoring safety
- domain alignment

---

## Sync Run Flow

Use a deterministic run flow.

### Recommended steps

1. Validate configuration
2. Acquire run lock
3. Load checkpoint
4. Fetch source records in stable order
5. Validate and map records
6. Write to sink using explicit mode
7. Commit checkpoint only after successful sink write
8. Emit run summary metrics/logs
9. Release lock

### Example orchestration outline

```javascript
async function runSync({ odoo, sink, checkpointStore, runLock, logger }) {
  const runId = crypto.randomUUID();

  logger.info({ runId }, 'sync.start');

  await runLock.acquire();

  try {
    const checkpoint = await checkpointStore.load();
    const pages = await fetchDeterministicPages({ odoo, checkpoint });

    let processed = 0;
    let written = 0;
    let failed = 0;
    let nextCheckpoint = checkpoint;

    for (const page of pages) {
      const mapped = [];
      for (const record of page.records) {
        try {
          mapped.push(mapRecord(record));
          nextCheckpoint = advanceCheckpoint(nextCheckpoint, record);
          processed++;
        } catch (error) {
          failed++;
          logger.warn({ runId, error, sourceId: record.id }, 'sync.record_mapping_failed');
        }
      }

      const result = await sink.write(mapped);
      written += result.writtenCount;

      await checkpointStore.save(nextCheckpoint);
    }

    logger.info({ runId, processed, written, failed }, 'sync.success');
  } finally {
    await runLock.release();
  }
}
```

### Invariants

- No overlapping runs
- No checkpoint advance without durable sink success
- No silent record drops
- Failed records are counted and logged
- Runtime behavior is measurable

---

## Output Sinks

Treat sinks as outputs, not as your control plane.

### Recommended sink categories

- Reporting sink
  - Google Sheets
  - CSV
  - BI-friendly tables
- System-of-record sink
  - Postgres
  - warehouse
  - integration database
- Command sink
  - API write
  - message queue
  - downstream transactional system

### Google Sheets guidance

Google Sheets is appropriate for:
- lightweight reporting
- low-volume exports
- human-readable operational views

Google Sheets is not ideal for:
- checkpoint persistence
- strong concurrency guarantees
- high-volume syncs
- exactly-once semantics

If using Sheets:
- keep checkpoint state outside the sheet
- define an idempotency column if possible
- expect manual edits and operational drift
- treat the sheet as a presentation sink

---

## Scheduling and Runtime

Do not put scheduling concerns inside domain logic.

Preferred options:
- platform scheduler
- singleton worker
- queue/worker model
- workflow runner

If in-process cron is used:
- add overlap protection
- add timeout budget
- add skipped-run logging
- add graceful shutdown

### Graceful shutdown requirements

- stop accepting new work
- finish or safely abort current batch
- do not leave checkpoint in an invalid state
- flush logs/metrics if possible

---

## Configuration Contract

Validate configuration at startup.

### Example categories

Required:
- Odoo host
- database
- login
- password
- transport/protocol
- sink config
- checkpoint backend

Optional:
- page size
- retry limits
- scheduler config
- log level
- tracing/metrics toggles

Suggested variables:

```env
# Odoo
ODOO_HOST=erp.example.com
ODOO_DATABASE=production_db
ODOO_LOGIN=sync_user@example.com
ODOO_PASSWORD=replace_me
ODOO_PROTOCOL=https
ODOO_PORT=443

# Sync
SYNC_MODE=append
SYNC_PAGE_SIZE=500

# Checkpoint / lock
CHECKPOINT_BACKEND=sqlite
LOCK_BACKEND=memory

# Sink
OUTPUT_SINK=google-sheets
GOOGLE_SHEETS_ID=sheet_id
GOOGLE_SHEET_NAME=April 2026

# Logging / telemetry
LOG_LEVEL=info
```

### Rules

- fail fast on invalid config
- never log secrets
- redact sensitive fields in logs

---

## Testing Strategy

Debug scripts are not a substitute for tests.

Use the lowest-level tests that provide confidence.

### Unit tests
Use for:
- domain builders
- state normalization
- mapping functions
- checkpoint progression
- idempotency logic
- fallback field behavior

### Integration tests
Use for:
- Odoo gateway adapter
- checkpoint store
- sink behavior
- retry and failure handling at boundaries

### Contract tests
Use for:
- recorded Odoo payloads
- version-specific field differences
- custom field presence assumptions

### End-to-end tests
Use sparingly for:
- critical sync path
- deployment smoke validation

### Minimum test scenarios

- happy path
- duplicate source records
- out-of-order updates
- missing optional fields
- incompatible field assumptions
- transient source failure
- transient sink failure
- partial mapping failures
- overlap-prevention behavior
- checkpoint resume after interruption

---

## Observability

An Odoo integration is incomplete without runtime observability.

### Structured logs
Every run should emit:
- run ID
- source model
- sync policy name
- checkpoint before/after
- processed count
- written count
- skipped count
- failed count
- duration
- result status

### Metrics
At minimum:
- sync duration
- source fetch latency
- sink write latency
- record counts
- retry counts
- failure counts by category
- last successful run timestamp
- stale sync age
- overlap prevention count

### Alerts
Consider alerts for:
- stale sync
- repeated failures
- checkpoint not advancing
- sudden drop to zero records when unexpected
- unusually high mapping failures

### Tracing
Optional but preferred:
- authenticate
- fetch page
- map records
- write sink
- save checkpoint

---

## Resiliency and Reliability

Classify failures before retrying.

### Failure categories

- transient transport failure
- persistent configuration or auth failure
- contract/data mismatch
- sink failure
- mapping/domain failure

### Retry guidance

Retry only when:
- the failure is transient
- the operation is safe to retry

Use:
- bounded retries
- exponential backoff
- jitter

Do not:
- blindly retry writes that may duplicate side effects

### Reliability rules

- acquire a lock before running
- checkpoint only after durable success
- use deterministic ordering
- preserve or log failed records for replay
- prefer bounded concurrency over uncontrolled parallelism

---

## Security

### Minimum security baseline

- least-privilege Odoo user
- least-privilege sink credentials
- no secrets in repository
- `.env.example` only with placeholders
- redact sensitive values in logs
- rotate secrets if compromised
- validate configuration without printing secrets

### Additional guidance

- review whether exported data contains PII
- minimize fields to only what is necessary
- avoid broad data replication without purpose
- use dependency and secret scanning in CI if this is a maintained project

---

## Diagnostics and Support Scripts

These scripts are useful for exploration and support, not as a replacement for tests.

Recommended scripts:
- `test-odoo-connection.js`
- `inspect-fields.js`
- `inspect-states.js`
- `inspect-record.js`
- `replay-failed-records.js`
- `dry-run-sync.js`

Examples:
- inspect actual field presence
- discover real production states
- inspect one record shape before coding a mapper
- replay failed records after a contract fix

---

## Anti-patterns to avoid

Do not default to these patterns unless the user explicitly accepts the trade-offs:

- using the sink as the authoritative checkpoint store
- using business dates as the default technical watermark when `write_date` is more appropriate
- allowing overlapping runs without a lock, singleton worker, or equivalent protection
- retrying writes blindly when side effects may duplicate downstream records
- treating debug scripts as substitutes for tests
- parsing display fields like `origin` as durable relational identifiers when better relational fields exist
- assuming stock quantity fields are globally meaningful without context
- implying exactly-once guarantees unless they are truly implemented and validated

---

## Common Odoo Gotchas

### many2one values
Often returned as:
```javascript
[id, display_name]
```

### false values
Odoo often returns `false` where some systems would return `null`

### field availability
Fields may differ by:
- version
- installed modules
- customization
- access rights

### timezone and dates
Normalize timestamps before comparing or checkpointing

### stock fields
Do not assume stock quantities are globally meaningful without context

### display fields
Avoid deriving durable business keys from human-readable fields such as `origin` unless there is no better option and the risk is documented

---

## Support matrix

### Guidance this skill provides

This skill provides strong guidance for:
- Odoo integration architecture and boundaries
- XML-RPC and version-aware access patterns
- checkpointing, idempotency, and overlap prevention
- testing strategy and observability expectations
- practical sink selection guidance
- migration away from fragile script-first defaults

### What this skill does not guarantee

This skill does not guarantee that:
- every field exists in every Odoo instance
- every model relation behaves identically across custom modules
- every stock or accounting workflow has the same semantics in all deployments
- a generated integration is correct without validating the target instance and business rules
- exactly-once semantics are available unless explicitly implemented

### Required verification in the target environment

When correctness matters, verify in the target Odoo instance:
- field existence
- state values and workflow meanings
- company/warehouse/context-specific behavior
- access rights
- downstream sink guarantees and replay behavior

---

## Deployment Guidance

Deploy as a worker or scheduled job, not as an accidental monolith.

Operational expectations:
- startup config validation
- health visibility
- structured logging
- graceful shutdown
- replay path for failed records
- rollback-safe change strategy

For small deployments, Railway/Render/Fly.io can work.
For larger deployments, prefer:
- containerized worker
- queue/worker runtime
- managed scheduler + singleton worker

---

## package.json Guidance

Suggested categories:

Runtime:
- XML-RPC client
- env validation
- logging
- optional telemetry

Test:
- unit/integration test runner
- mocks/fixtures
- contract fixture support

Dev:
- formatter
- linter
- type checker if using TypeScript

Node version:
- prefer modern LTS

If the project is expected to live beyond a quick utility, prefer TypeScript or runtime schema validation.

---

## Checklist for Every New Odoo Integration

- [ ] Business intent is explicit
- [ ] Sync mode is explicit: append / upsert / snapshot / reconcile
- [ ] Checkpoint field and ordering are explicit
- [ ] Idempotency key is explicit
- [ ] Checkpoint store is separate from the sink
- [ ] Overlap prevention is implemented
- [ ] Configuration is validated at startup
- [ ] Odoo fields are verified in the target instance
- [ ] Mapping logic has unit tests
- [ ] Boundary behavior has integration or contract tests
- [ ] Runtime logs and metrics are emitted
- [ ] Failure categories and retry rules are defined
- [ ] Failed records are observable and replayable
- [ ] Secrets are not committed
- [ ] Access rights use least privilege
- [ ] Deployment and shutdown behavior are safe

---

## Practical Defaults

If the user does not specify otherwise:

- Use a dedicated Odoo gateway
- Use `write_date` as checkpoint field
- Use deterministic ordering with `id` tie-breaker
- Use an external checkpoint store
- Use structured logs
- Add a basic metrics surface
- Prevent overlapping runs
- Prefer simple deterministic code over clever abstractions
- Use tests for mapping, checkpointing, and sink behavior
- Treat Google Sheets as a reporting sink, not source of truth
