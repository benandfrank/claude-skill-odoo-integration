# FAQ

## Why use `write_date` instead of `date` as the default checkpoint?

Because `write_date` is usually a better technical watermark for incremental syncs. Business date fields often describe business events, not mutation order.

## Why should checkpoint storage stay outside Google Sheets?

Because Sheets is better as a reporting or presentation sink than as a control plane. Manual edits, weak concurrency guarantees, and append-oriented workflows make it a fragile checkpoint store.

## When is XML-RPC still acceptable?

Often. Many Odoo environments still rely on XML-RPC successfully. The skill does not reject XML-RPC; it rejects fragile integration design around it.

## Does this skill guarantee compatibility with every Odoo instance?

No. Custom modules, access rights, field availability, and workflow semantics vary. Always verify target-instance behavior when correctness matters.

## Does this skill require Node.js?

Node.js 18+ is the preferred default for the implementation scaffolding taught here, but the architectural guidance, checkpointing rules, and Odoo integration reasoning can still apply to other runtimes.
