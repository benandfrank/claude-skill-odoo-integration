# Reviewing an Odoo Integration

Use this guide when reviewing an integration generated or proposed with this skill.

---

## Review workflow

### Step 1 — Review the contract
Confirm that the integration makes these explicit:
- source model
- sync mode
- checkpoint field and ordering
- idempotency key
- sink choice

### Step 2 — Check common failure modes
Read the [anti-patterns section](#) in [`anti-patterns.md`](anti-patterns.md).

Look especially for:
- sink-as-checkpoint behavior
- overlapping runs
- debug-scripts-as-tests thinking
- ambiguous guarantees

### Step 3 — Check guarantees vs assumptions
Confirm that the proposal does not overclaim:
- field compatibility
- workflow compatibility
- sink guarantees
- exactly-once behavior

### Step 4 — Inspect operability
Review [`operability.md`](operability.md).

Confirm that:
- runtime observability exists
- checkpoint behavior is safe
- sink choice matches the use case

### Step 5 — Review tests
Confirm that:
- mapping and checkpoint behavior are tested
- boundary behavior is tested appropriately
- diagnostic scripts are not the only confidence mechanism

### Rule of thumb
If the integration is not explicit, observable, and reviewable, it is not ready.

---

## Review checklist

### Problem and contract
- [ ] The business intent is explicit
- [ ] The source model is explicit
- [ ] The sync mode is explicit
- [ ] The idempotency key is explicit
- [ ] The checkpoint field and ordering are explicit

### Architecture and boundaries
- [ ] Odoo access is encapsulated behind a gateway/adapter
- [ ] Domain logic is separate from sink logic
- [ ] Scheduling is separate from domain behavior
- [ ] Checkpoint storage is separate from the sink

### Reliability
- [ ] Overlap prevention exists
- [ ] Checkpoint advances only after durable sink success
- [ ] Retries are limited to safe transient failures
- [ ] Replay behavior is understood or documented

### Observability
- [ ] Structured logs are present
- [ ] Counts and run outcomes are emitted
- [ ] Last-success or stale-sync signals can be observed
- [ ] Failure paths are visible, not silent

### Testing
- [ ] Mapping logic has unit tests
- [ ] Boundary behavior has integration or contract tests
- [ ] Failure scenarios are covered at an appropriate level
- [ ] Debug scripts are not being treated as tests

### Odoo-specific verification
- [ ] Fields are verified in the target instance
- [ ] State values and meanings are verified
- [ ] Context-sensitive fields are used with care
- [ ] Custom modules or workflow differences are considered
