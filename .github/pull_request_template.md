## Summary

<!-- What does this PR do and why? Link the issue. -->

Closes #

## Changes

<!-- Bullet list of what changed. -->

-

## Validation Evidence

<!-- Machine-verifiable proof that this works. Required. Fill in layers that apply. -->

### Layer 1: Local
- **Build:** <!-- docker build output, make build, or N/A -->
- **Tests:** <!-- test output or link -->
- **Lint:** <!-- clean or N/A -->

### Layer 2: Lab Cluster
- **Deployment:** <!-- pod status, rollout output, or N/A for non-deployable changes -->
- **Health check:** <!-- route/endpoint HTTP status, or N/A -->
- **Integration tests:** <!-- output or link, or N/A -->

### Layer 3: Post-Merge (filled after merge)
- **Sync status:** <!-- Argo CD status, or N/A -->
- **Post-deploy check:** <!-- verification output, or N/A -->

## Memory Updates

<!-- Required. CI will fail if memory/changelog.md is not in the diff. -->

- [ ] `memory/changelog.md` updated with what changed, why, and what was learned
- [ ] `MEMORY.md` updated (if new knowledge discovered)
- [ ] `memory/decisions/` ADR added (if architecture decision made)
- [ ] `memory/failures/` record added (if something failed during this work)

## Checklist

- [ ] All existing tests pass
- [ ] New/modified behavior has test coverage
- [ ] No secrets or credentials in the diff
