# Maintainers Guide

## Alignment rule

Keep these files aligned:
- [`README.md`](README.md): onboarding, positioning, quick-start guidance
- [`SKILL.md`](SKILL.md): canonical skill behavior and engineering defaults
- [`EXAMPLES.md`](EXAMPLES.md): compact usage scenarios
- [`CHANGELOG.md`](CHANGELOG.md): release-level summary of behavior changes
- [`.env.example`](.env.example): example configuration contract
- [`docs/stance.md`](docs/stance.md): compact statement of the repo's engineering stance
- [`docs/release-checklist.md`](docs/release-checklist.md): pre-release quality gate for maintainers

## When to update what

### Update `SKILL.md` when
- the skill behavior changes
- defaults change
- new required engineering practices are added
- architecture, testing, observability, or reliability guidance changes

### Update `README.md` when
- the skill positioning changes
- onboarding expectations change
- the recommended architecture or decision guide changes materially

### Update `EXAMPLES.md` when
- a new common scenario appears
- the recommended way to distinguish lightweight vs production integrations changes

### Update `.env.example` when
- the configuration contract changes
- new required runtime variables are introduced
- supported sinks or backends change materially

### Update `CHANGELOG.md` when
- a release changes the skill’s generated guidance or default engineering stance
- a new support document materially changes how users should navigate or apply the repo

### Update `docs/stance.md` when
- the repo’s engineering stance changes materially
- core expectations around checkpointing, observability, testing, or reliability change

### Update `docs/release-checklist.md` when
- release quality gates change
- new core docs or required support docs are introduced

## Reading order for maintainers

1. [`MAINTAINERS.md`](MAINTAINERS.md)
2. [`docs/stance.md`](docs/stance.md)
3. [`docs/release-checklist.md`](docs/release-checklist.md)
4. [`CHANGELOG.md`](CHANGELOG.md)
5. [`SKILL.md`](SKILL.md)
6. [`TESTING.md`](TESTING.md)
7. [`tests/manual-test-cases.md`](tests/manual-test-cases.md)

## Maintainer release workflow

1. Review [`docs/stance.md`](docs/stance.md) to confirm the intended stance still holds
2. Review [`docs/release-checklist.md`](docs/release-checklist.md)
3. Confirm `README.md`, `SKILL.md`, and `EXAMPLES.md` remain aligned
4. Update `CHANGELOG.md` for any user-visible guidance change
5. Run or review the golden prompt suite in [`tests/manual-test-cases.md`](tests/manual-test-cases.md)
6. Verify new support docs are linked from the right places

## Review checklist for maintainers

Before publishing a change, verify:
- README and SKILL do not contradict each other
- examples reflect current preferred architecture
- config examples match the documented contract
- new guidance still works for both small and production-grade integrations
- observability, checkpointing, and testing guidance remain explicit
- [`docs/stance.md`](docs/stance.md) still matches the repo’s current stance
- [`docs/release-checklist.md`](docs/release-checklist.md) has been reviewed for the release

## Editing philosophy

Prefer:
- small, reviewable updates
- explicit invariants over clever prose
- practical examples over abstract claims
- alignment with current Odoo integration realities

Avoid:
- reintroducing sink-as-checkpoint patterns
- presenting debug scripts as substitutes for tests
- implying exactly-once guarantees unless truly implemented
- narrowing the skill back to XML-RPC script snippets only
