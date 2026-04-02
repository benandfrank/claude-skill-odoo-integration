# Migration from v1 to v2

Related docs:
- [`README.md`](README.md)
- [`SKILL.md`](SKILL.md)
- [`EXAMPLES.md`](EXAMPLES.md)
- [`docs/operability.md`](docs/operability.md)

This guide helps users understand the shift from the earlier skill guidance to v2.

## What changed conceptually

### v1 mindset
- XML-RPC script first
- in-process cron as the default shape
- Google Sheets commonly treated as both sink and control plane
- debug scripts emphasized more than test strategy

### v2 mindset
- operable integration first
- explicit sync contract
- external checkpointing preferred
- overlap prevention and runtime observability expected
- tests are first-class
- Google Sheets is a reporting sink, not the default source of truth

## If you used the old patterns

### Old pattern: checkpoint in the sheet
Move to:
- dedicated checkpoint store
- deterministic ordering
- explicit replay behavior

### Old pattern: `date` as the default watermark
Move to:
- `write_date` as the default technical checkpoint
- `id` as deterministic tie-breaker

### Old pattern: cron callback owns everything
Move to:
- orchestration separated from domain logic and sink logic
- explicit lock/overlap prevention

### Old pattern: debug scripts instead of tests
Move to:
- unit tests for mapping and checkpoint progression
- integration/contract tests for boundaries
- keep debug scripts for diagnostics only

## What you do not need to change

You do **not** need to abandon:
- XML-RPC
- Node.js
- lightweight integrations
- Google Sheets for reporting

The v2 shift is about safer defaults and stronger operational guidance, not forcing every use case into a heavyweight platform.

## Practical migration checklist

- [ ] Make sync mode explicit
- [ ] Move checkpoint storage outside the sink
- [ ] Use deterministic checkpoint ordering
- [ ] Add overlap prevention
- [ ] Add structured logs
- [ ] Add a minimum test set
- [ ] Document retry and replay behavior
