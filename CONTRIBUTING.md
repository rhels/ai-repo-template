# Contributing

This is an **AI-first repository** — primarily maintained by AI agents with human oversight.

## For AI Agents

Follow the protocol in `AGENTS.md`. Key points:

1. **Read before writing**: `AGENTS.md` → `MEMORY.md` → `memory/changelog.md`
2. **Test everything**: Run tests before and after changes
3. **Update memory**: Changelog entry is mandatory. MEMORY.md and failure records as needed.

## For Humans

The same memory protocol applies to you. Update `MEMORY.md` and `memory/changelog.md` so agents picking up after you have full context.

## Workflow

### 1) Create a branch
Branch naming: `<type>/<ticket>-<short-desc>`
Types: docs, feature, fix, infra, security

### 2) Commit message format
```
<TICKET>: <imperative summary>

- Why the change is needed
- How to test
```

### 3) Open a Pull Request
- Target: `main`
- Keep PRs under 300 lines where possible
- Include: what changed, how to validate

### 4) Review & Merge
- All CI checks must pass
- Squash merge for small changes

## Memory Protocol (Required)

After completing any work:

1. **Update `memory/changelog.md`** — what you changed, why, what you learned
2. **Update `MEMORY.md`** — if you discovered something non-obvious
3. **Create an ADR** in `memory/decisions/` — if you made an architecture decision
4. **Create a failure record** in `memory/failures/` — if something failed during your work

## Security

- Never commit secrets, tokens, passwords, or private keys
- Use environment variables or secret managers for sensitive values
- Rotate immediately if a secret is accidentally committed
