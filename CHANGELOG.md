# Changelog

Related docs:
- [`README.md`](README.md)
- [`SKILL.md`](SKILL.md)
- [`MIGRATION-v1-to-v2.md`](MIGRATION-v1-to-v2.md)
- [`docs/README.md`](docs/README.md)

## Changelog conventions

For future releases:
- add the newest version at the top
- summarize behavior changes, not just file edits
- list new support docs when they affect how the skill should be used
- update related links if the docs structure changes

## v2.0.0

### Changed (documentation consolidation)
- Consolidated `review-checklist.md` + `review-workflow.md` into `reviewing.md`
- Consolidated `glossary.md` + `FAQ.md` into `concepts.md`
- Consolidated `principles.md` + `guarantees-vs-assumptions.md` into `stance.md`
- Reduced `docs/` from 12 files to 9 without losing content
- Added table of contents to `SKILL.md`
- Added `.gitignore`
- Added skill testing strategy, golden prompt suite, results template, and example scored output

### Changed
- Repositioned the skill from XML-RPC script guidance to operable Odoo integration guidance
- Added explicit sync contract guidance
- Added architecture guidance with clearer separation of concerns
- Added testing strategy as a first-class section
- Added observability, resiliency, and reliability guidance
- Added stronger guidance around checkpointing, idempotency, and overlap prevention
- Reframed Google Sheets as a reporting sink rather than the default control plane

### Added
- `README.md` updates for v2 positioning
- compact reference architecture in the README
- test matrix and minimum scenarios in the README
- small-vs-production integration decision guide in the README
- `.env.example`
- `EXAMPLES.md`
- maintainer alignment guidance
- support docs for operability, glossary, decision-making, FAQ, prompt templates, anti-patterns, guarantees vs assumptions, review workflow, principles, and release checks

### Notes
- The skill remains useful for XML-RPC-heavy Odoo environments
- The default posture is now safer for production-oriented integrations
