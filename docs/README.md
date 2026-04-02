# Documentation Index

This folder contains focused support documentation for the Odoo integration skill.

## Contents

- [`operability.md`](operability.md)
  - checkpointing patterns
  - sink selection guidance
  - observability checklist
- [`glossary.md`](glossary.md)
  - key concepts such as checkpoint, idempotency, replay, and sync mode
- [`review-checklist.md`](review-checklist.md)
  - compact review checklist for generated integrations
- [`decision-tree.md`](decision-tree.md)
  - choose sync mode and sink based on the use case
- [`FAQ.md`](FAQ.md)
  - recurring questions about checkpoints, XML-RPC, Sheets, and runtime expectations
- [`principles.md`](principles.md)
  - summary of the repo's engineering stance
- [`release-checklist.md`](release-checklist.md)
  - maintainer checklist before publishing changes
- [`prompt-templates.md`](prompt-templates.md)
  - safer prompt shapes for common Odoo integration tasks
- [`anti-patterns.md`](anti-patterns.md)
  - common failure modes and why they fail
- [`guarantees-vs-assumptions.md`](guarantees-vs-assumptions.md)
  - what the skill strongly guides vs what must still be verified
- [`review-workflow.md`](review-workflow.md)
  - practical review flow combining checklist, anti-patterns, and guarantees

## Related top-level docs

- [`../README.md`](../README.md)
  - onboarding and quick start
- [`../SKILL.md`](../SKILL.md)
  - canonical skill behavior and engineering defaults
- [`../EXAMPLES.md`](../EXAMPLES.md)
  - compact example scenarios
- [`../MIGRATION-v1-to-v2.md`](../MIGRATION-v1-to-v2.md)
  - migration guidance from the old skill posture
- [`../MAINTAINERS.md`](../MAINTAINERS.md)
  - maintainer alignment rules
- [`../CONTRIBUTING.md`](../CONTRIBUTING.md)
  - contributor expectations and review checklist

## How to use this docs folder

Use these docs when you want a more focused explanation than the README provides, without reading the entire skill definition.

Suggested reading paths:
- New user: [`../README.md`](../README.md) → [`glossary.md`](glossary.md) → [`decision-tree.md`](decision-tree.md) → [`operability.md`](operability.md)
- Reviewer: [`review-workflow.md`](review-workflow.md) → [`review-checklist.md`](review-checklist.md) → [`guarantees-vs-assumptions.md`](guarantees-vs-assumptions.md) → [`anti-patterns.md`](anti-patterns.md)
- Maintainer: [`principles.md`](principles.md) → [`release-checklist.md`](release-checklist.md) → [`../MAINTAINERS.md`](../MAINTAINERS.md)
- Migrating user: [`../MIGRATION-v1-to-v2.md`](../MIGRATION-v1-to-v2.md) → [`operability.md`](operability.md)
