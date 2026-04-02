# Engineering Stance

This page summarizes the repo's engineering principles and clarifies what the skill guarantees versus what must be verified in the target environment.

---

## Core principles

- Prefer explicit sync contracts over implicit behavior
- Prefer checkpointing outside the sink
- Prefer deterministic ordering over best-effort scans
- Prefer at-least-once processing with idempotent sinks over ambiguous guarantees
- Prefer observability in runtime over assumptions in design
- Prefer small, testable boundaries over script entanglement
- Prefer domain language over stringly-typed incidental code
- Prefer practical reliability over transport-specific cleverness

## What these principles mean in practice

- define sync mode before coding
- define checkpoint field and ordering before coding
- prevent overlapping runs
- classify failures before retrying
- treat debug scripts as diagnostics, not tests
- verify target-instance fields and workflows when correctness matters

## Why this matters

Odoo integrations often begin as lightweight scripts and gradually become business-critical. These principles help keep that evolution safe, observable, and maintainable.

---

## Guarantees vs assumptions

### Guidance you can rely on from this repo

The skill strongly guides toward:
- explicit sync contracts
- external checkpointing over sink-as-control-plane patterns
- deterministic ordering for incremental syncs
- overlap prevention for recurring jobs
- structured observability and test strategy
- practical Odoo-aware reliability patterns

### Assumptions you must still verify

You must still verify in the target environment:
- field existence
- model relations
- state meanings
- custom module behavior
- company, warehouse, and context-sensitive semantics
- downstream sink guarantees and replay behavior

### Rule of thumb

Treat this repo as a strong engineering guide, not as a universal compatibility guarantee.
