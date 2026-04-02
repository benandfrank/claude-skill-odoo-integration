# Principles

This page summarizes the engineering stance behind the skill.

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
