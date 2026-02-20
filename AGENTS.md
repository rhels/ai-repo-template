# AGENTS.md — AI-First Repository Guide

> **What is this?** A README for AI agents. Read this before doing anything in this repo.
> Compatible with: [AGENTS.md standard](https://agents.md), GitHub Copilot, Claude Code, Gemini CLI, Codex CLI, Cursor, Aider.

---

## On Every Session

Before making any changes:

1. **Read this file** (`AGENTS.md`) — understand the rules and conventions.
2. **Read `MEMORY.md`** — curated lessons, decisions, and constraints.
3. **Read `memory/changelog.md`** — recent changes and their rationale.
4. **Read the relevant subdirectory README** for component-specific context.

**Do not skip these steps.** They exist because agents before you lost time rediscovering things that were already known.

---

## Project Overview

<!-- TEMPLATE: Replace this section with your project description -->

**Project:** [Your project name]
**Purpose:** [What this repo does]
**Stack:** [Languages, frameworks, platforms]
**Deployment:** [How and where this deploys]

---

## Build & Test Commands

<!-- TEMPLATE: Replace with your actual commands -->

| Command | Purpose |
|---------|---------|
| `make build` | Build the project |
| `make test` | Run all tests |
| `make lint` | Run linters |
| `make validate` | Pre-commit validation |

**Always run tests before committing.** If you add a feature, add tests for it.

---

## Code Style & Conventions

<!-- TEMPLATE: Add your project's conventions -->

- Follow existing patterns in the codebase
- Keep PRs focused and under 300 lines where possible
- Commit message format: `<TICKET>: <imperative summary>`
- Branch naming: `<type>/<ticket>-<short-desc>` (types: docs, feature, fix, infra, security)

---

## Memory System

This repo uses structured, in-repo memory to solve the context loss problem across agent sessions.

### `MEMORY.md` (root)
**Curated, high-signal knowledge.** Not a log — a living document.

Sections:
- **Architecture Decisions** — why things are the way they are
- **Constraints & Gotchas** — things that will bite you if you don't know them
- **Patterns That Work** — proven approaches in this repo
- **Patterns That Failed** — what was tried and why it didn't work
- **Environment Context** — cluster details, tool versions, access patterns

**Rules:**
- Keep under 500 lines. Prune ruthlessly.
- Every entry must answer: "Would a future agent need this to avoid a mistake or make a better decision?"
- Date every entry.

### `memory/changelog.md`
**Append-only log of significant changes.**

Format:
```markdown
## YYYY-MM-DD — <short summary>
**Agent:** <name>
**Branch:** <branch>
**What changed:** <description>
**Why:** <rationale>
**What I learned:** <discoveries, gotchas, surprises>
**What failed first:** <if applicable>
```

### `memory/decisions/`
**Architecture Decision Records (ADRs)** for non-obvious choices.

Use when: you chose approach A over B and the reasoning matters.
Format: `memory/decisions/NNNN-<title>.md`

### `memory/failures/`
**Dedicated failure records.** The highest-value memory you can leave.

Format: `memory/failures/YYYY-MM-DD-<short-title>.md`
```markdown
## What was attempted
## What happened (exact error)
## Why it failed
## What worked instead
```

---

## Before You Change Anything

1. **Check memory first.** Someone may have already tried what you're about to do.
2. **Check `memory/failures/`** for related past attempts.
3. **Understand the deployment model** before modifying infrastructure.
4. **Understand policy constraints** before adding new dependencies or images.

---

## After You Change Something

### Always
- [ ] Update `memory/changelog.md` with what you did and why
- [ ] If you discovered something non-obvious, add it to `MEMORY.md`
- [ ] If you made an architecture decision, create an ADR in `memory/decisions/`

### If something failed
- [ ] Record the failure in `memory/failures/`
- [ ] Include: what you tried, the exact error, why it failed, what worked instead

### If you modified code
- [ ] Run `make test` (or equivalent) and ensure all tests pass
- [ ] Run `make lint` (or equivalent) and fix any issues
- [ ] Add or update tests for changed behavior

---

## For Sub-Agents

If you are a sub-agent spawned for a specific task:

1. **Read `AGENTS.md` and `MEMORY.md` before starting.**
2. **Scope your changes.** Don't refactor unrelated code.
3. **Write your changelog entry before finishing.** Your parent agent may not have your context.
4. **If you're blocked**, record what blocked you and why — that's valuable memory even if you didn't finish.

---

## For Human Engineers

This repo is AI-first but not AI-only. The memory system benefits everyone — it's structured context that outlives any individual session, human or AI.

If you make changes manually, please follow the same memory discipline: update `MEMORY.md` and `memory/changelog.md`.

---

## Testing Philosophy

Tests are **executable documentation**. They define:
- What the system does (behavior)
- What constraints exist (edge cases)
- What was already tried (regression prevention)

By reading and running tests, agents can infer existing behavior and align new features accordingly.

---

## Security

- Never commit secrets, tokens, passwords, or private keys
- Use environment variables or secret managers for sensitive values
- Rotate immediately if a secret is accidentally committed
- Document security decisions explicitly in `memory/decisions/`

---

## Principles

1. **Memory is a first-class artifact.** Treat it with the same care as code.
2. **Record failures, not just successes.** Failed approaches are the highest-value memory.
3. **Prune aggressively.** Stale memory is worse than no memory — it misleads.
4. **One source of truth.** Don't duplicate context across files. Link instead.
5. **Future agents are your users.** Write for someone who knows nothing about what you just did.
6. **Tests are documentation.** If behavior isn't tested, it's not guaranteed.
