# Anti-patterns and Why They Fail

This page explains the most important anti-patterns the skill tries to prevent.

## 1. Sink as checkpoint store

Why it fails:
- manual edits can corrupt progress state
- presentation sinks rarely provide reliable concurrency semantics
- replay and resume behavior become fragile

Better:
- use a dedicated checkpoint store

## 2. Business date as default technical watermark

Why it fails:
- business dates do not necessarily reflect mutation order
- updates to old records can be missed
- replay logic becomes harder to reason about

Better:
- prefer `write_date` with deterministic ordering

## 3. Overlapping runs with no protection

Why it fails:
- duplicate writes
- race conditions
- checkpoint corruption

Better:
- use a run lock, singleton worker, or equivalent protection

## 4. Debug scripts treated as tests

Why it fails:
- they are not repeatable confidence mechanisms
- they rarely cover failure scenarios or invariants
- they encourage manual verification drift

Better:
- use unit, integration, and contract tests where appropriate
- keep debug scripts for diagnostics

## 5. Parsing display fields as durable relationships

Why it fails:
- display fields are often human-oriented and unstable
- custom modules may alter format and meaning
- relations become brittle and hard to verify

Better:
- prefer relational fields or explicit identifiers

## 6. Ambiguous guarantees

Why it fails:
- teams assume exactly-once behavior that does not exist
- operators cannot tell whether retries are safe
- failures are harder to triage

Better:
- state the actual guarantee clearly, usually at-least-once with idempotent sink behavior
