# Contributing

Related docs:
- [`README.md`](README.md)
- [`SKILL.md`](SKILL.md)
- [`MAINTAINERS.md`](MAINTAINERS.md)
- [`docs/README.md`](docs/README.md)
- [`docs/stance.md`](docs/stance.md)
- [`docs/release-checklist.md`](docs/release-checklist.md)

Thanks for contributing to `claude-skill-odoo-integration`.

This repo is a skill product, not just a markdown file. Changes should improve:
- correctness
- clarity
- operability
- maintainability

## Contribution expectations

When proposing a change:
- keep changes small and reviewable
- explain the problem being solved
- prefer explicit invariants over vague advice
- preserve alignment between documentation files
- avoid reintroducing fragile defaults

## Files that must stay aligned

- [`README.md`](README.md)
- [`SKILL.md`](SKILL.md)
- [`EXAMPLES.md`](EXAMPLES.md)
- [`CHANGELOG.md`](CHANGELOG.md)
- [`.env.example`](.env.example)
- [`MAINTAINERS.md`](MAINTAINERS.md)
- [`docs/stance.md`](docs/stance.md)
- [`docs/release-checklist.md`](docs/release-checklist.md)

## Good contribution examples

- improve checkpointing guidance
- refine observability recommendations
- add a realistic Odoo example scenario
- clarify compatibility caveats for versions or custom modules
- improve testing guidance without adding unnecessary complexity

## Changes that need extra care

Please be especially careful with changes that:
- weaken idempotency guidance
- move checkpointing back into the sink
- present debug scripts as a replacement for tests
- imply exactly-once guarantees without implementation detail
- narrow the skill back to XML-RPC snippets only

## Review checklist

Before opening or merging a change, verify:
- the README still matches the skill positioning
- SKILL.md remains the canonical source of behavior guidance
- examples still reflect preferred patterns
- config examples still match documented expectations
- new guidance works for both lightweight and production-grade integrations
- observability, checkpointing, and testing remain explicit
- [`docs/stance.md`](docs/stance.md) still reflects the intended engineering stance
- [`docs/release-checklist.md`](docs/release-checklist.md) still matches release expectations

## Pull request guidance

A good pull request should include:
- what changed
- why it changed
- which files were intentionally updated
- any follow-up slices or open questions

## Scope boundaries

In scope:
- improving Odoo integration guidance
- improving examples, testing guidance, observability guidance, and reliability guidance
- clarifying compatibility or operational caveats
- expanding docs in ways that preserve the current skill philosophy

Out of scope unless clearly justified:
- turning the repo into a full framework implementation
- adding unrelated ERP guidance
- narrowing the skill to a single sink or deployment platform
- adding runtime-specific recommendations that conflict with the canonical skill defaults without explanation

## Documentation style

Prefer:
- concrete examples
- short sections
- domain-specific wording
- actionable checklists

Avoid:
- generic advice without Odoo context
- over-abstract architecture language
- long prose that hides the operational rule
