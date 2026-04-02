# Generated Integration Review Checklist

Use this checklist when reviewing an integration proposed or generated with this skill.

## Problem and contract
- [ ] The business intent is explicit
- [ ] The source model is explicit
- [ ] The sync mode is explicit
- [ ] The idempotency key is explicit
- [ ] The checkpoint field and ordering are explicit

## Architecture and boundaries
- [ ] Odoo access is encapsulated behind a gateway/adapter
- [ ] Domain logic is separate from sink logic
- [ ] Scheduling is separate from domain behavior
- [ ] Checkpoint storage is separate from the sink

## Reliability
- [ ] Overlap prevention exists
- [ ] Checkpoint advances only after durable sink success
- [ ] Retries are limited to safe transient failures
- [ ] Replay behavior is understood or documented

## Observability
- [ ] Structured logs are present
- [ ] Counts and run outcomes are emitted
- [ ] Last-success or stale-sync signals can be observed
- [ ] Failure paths are visible, not silent

## Testing
- [ ] Mapping logic has unit tests
- [ ] Boundary behavior has integration or contract tests
- [ ] Failure scenarios are covered at an appropriate level
- [ ] Debug scripts are not being treated as tests

## Odoo-specific verification
- [ ] Fields are verified in the target instance
- [ ] State values and meanings are verified
- [ ] Context-sensitive fields are used with care
- [ ] Custom modules or workflow differences are considered
