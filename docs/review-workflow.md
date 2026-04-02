# Review Workflow

Use this workflow when reviewing an Odoo integration generated or proposed with this skill.

## Step 1 — Review the contract
Start with [`review-checklist.md`](review-checklist.md).

Confirm that the integration makes these explicit:
- source model
- sync mode
- checkpoint field and ordering
- idempotency key
- sink choice

## Step 2 — Check common failure modes
Read [`anti-patterns.md`](anti-patterns.md).

Look especially for:
- sink-as-checkpoint behavior
- overlapping runs
- debug-scripts-as-tests thinking
- ambiguous guarantees

## Step 3 — Check guarantees vs assumptions
Read [`guarantees-vs-assumptions.md`](guarantees-vs-assumptions.md).

Confirm that the proposal does not overclaim:
- field compatibility
- workflow compatibility
- sink guarantees
- exactly-once behavior

## Step 4 — Inspect operability
Review [`operability.md`](operability.md).

Confirm that:
- runtime observability exists
- checkpoint behavior is safe
- sink choice matches the use case

## Step 5 — Review tests
Confirm that:
- mapping and checkpoint behavior are tested
- boundary behavior is tested appropriately
- diagnostic scripts are not the only confidence mechanism

## Rule of thumb
If the integration is not explicit, observable, and reviewable, it is not ready.
