# Glossary

This glossary defines the key engineering terms used throughout the skill.

## Checkpoint

A durable marker of sync progress.

Typical example:
- `write_date` plus `id` as deterministic ordering

Why it matters:
- avoids rescanning blindly
- supports resume after interruption
- enables controlled replay behavior

## Idempotency

A property where repeating the same effective operation does not create incorrect duplicate side effects.

Typical examples:
- upserting by source record ID
- using a durable idempotency key in the sink

Why it matters:
- protects against retries and replays causing duplicate writes

## Replay

Reprocessing records or rerunning a slice of synchronization after a failure, bug fix, or operational correction.

Why it matters:
- supports recovery from partial failures
- helps fix data issues without ad hoc manual edits

## Overlap prevention

A mechanism that prevents multiple sync runs from executing the same job concurrently.

Typical examples:
- run lock
- singleton worker
- external scheduler with singleton guarantees

Why it matters:
- avoids duplicate writes
- avoids checkpoint corruption
- reduces race conditions

## Sync mode

The intended write behavior of the integration.

Common modes:
- `append`
- `upsert`
- `snapshot`
- `reconcile`

Why it matters:
- determines deduplication behavior
- determines replay behavior
- influences sink choice and test strategy
