# Documentation Index

This folder contains focused support documentation for the Odoo integration skill.

## Contents

- [`operability.md`](operability.md) — checkpointing patterns, sink selection, observability checklist
- [`concepts.md`](concepts.md) — glossary of key terms and frequently asked questions
- [`reviewing.md`](reviewing.md) — review workflow and checklist for generated integrations
- [`decision-tree.md`](decision-tree.md) — choose sync mode and sink based on the use case
- [`stance.md`](stance.md) — engineering principles, guarantees vs assumptions
- [`prompt-templates.md`](prompt-templates.md) — safer prompt shapes for common Odoo tasks
- [`anti-patterns.md`](anti-patterns.md) — common failure modes and why they fail
- [`release-checklist.md`](release-checklist.md) — maintainer checklist before publishing changes

## Testing

- [`../TESTING.md`](../TESTING.md) — how to test the skill using golden prompts and a review rubric
- [`../tests/manual-test-cases.md`](../tests/manual-test-cases.md) — manual golden prompt regression suite
- [`../tests/results-template.md`](../tests/results-template.md) — template for recording test results
- [`../tests/results/`](../tests/results/) — example scored outputs

## Related top-level docs

- [`../README.md`](../README.md) — onboarding and quick start
- [`../SKILL.md`](../SKILL.md) — canonical skill behavior and engineering defaults
- [`../EXAMPLES.md`](../EXAMPLES.md) — compact example scenarios
- [`../MIGRATION-v1-to-v2.md`](../MIGRATION-v1-to-v2.md) — migration guidance from the old skill posture
- [`../MAINTAINERS.md`](../MAINTAINERS.md) — maintainer alignment rules
- [`../CONTRIBUTING.md`](../CONTRIBUTING.md) — contributor expectations and review checklist

## Reading paths

- New user: [`../README.md`](../README.md) → [`concepts.md`](concepts.md) → [`decision-tree.md`](decision-tree.md) → [`operability.md`](operability.md)
- Reviewer: [`reviewing.md`](reviewing.md) → [`stance.md`](stance.md) → [`anti-patterns.md`](anti-patterns.md)
- Maintainer: [`stance.md`](stance.md) → [`release-checklist.md`](release-checklist.md) → [`../MAINTAINERS.md`](../MAINTAINERS.md)
- Migrating user: [`../MIGRATION-v1-to-v2.md`](../MIGRATION-v1-to-v2.md) → [`operability.md`](operability.md)
