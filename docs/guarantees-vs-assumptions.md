# Guarantees vs Assumptions

This page clarifies what the skill strongly guides versus what must still be verified in the target environment.

## Guidance you can rely on from this repo

The skill strongly guides toward:
- explicit sync contracts
- external checkpointing over sink-as-control-plane patterns
- deterministic ordering for incremental syncs
- overlap prevention for recurring jobs
- structured observability and test strategy
- practical Odoo-aware reliability patterns

## Assumptions you must still verify

You must still verify in the target environment:
- field existence
- model relations
- state meanings
- custom module behavior
- company, warehouse, and context-sensitive semantics
- downstream sink guarantees and replay behavior

## Why this distinction matters

A good skill can improve defaults and reduce mistakes, but it cannot replace validation in the real Odoo instance and business workflow.

## Rule of thumb

Treat this repo as a strong engineering guide, not as a universal compatibility guarantee.
