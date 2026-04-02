# Release Checklist

Use this checklist before publishing a meaningful change to the skill.

## Documentation alignment
- [ ] `README.md` matches the current skill positioning
- [ ] `SKILL.md` remains the canonical behavior definition
- [ ] `EXAMPLES.md` still reflects preferred patterns
- [ ] `.env.example` still matches the documented configuration contract
- [ ] `CHANGELOG.md` includes the relevant release summary

## Guidance quality
- [ ] checkpointing guidance is still explicit
- [ ] idempotency guidance is still explicit
- [ ] overlap prevention guidance is still explicit
- [ ] observability guidance is still explicit
- [ ] testing guidance is still explicit

## Safety checks
- [ ] no sink-as-checkpoint regressions were introduced
- [ ] no exactly-once claims were introduced without support
- [ ] no guidance regressed into script-only thinking for production cases
- [ ] compatibility caveats remain visible

## Navigation
- [ ] new supporting docs are linked where appropriate
- [ ] outdated links were updated or removed
