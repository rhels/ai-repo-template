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

## Issue Lifecycle: Enrichment → Acceptance → Implementation

When you pick up an issue labeled `triage`:

### Step 1: Investigate
- Read `MEMORY.md` and `memory/failures/` for related context
- Inspect the codebase: grep for relevant code, check recent commits
- Check tests, logs, CI history for signals
- Understand the problem before proposing a solution

### Step 2: Enrich the issue (Multi-Persona Review)
Edit the issue body to add these sections. You must think from **three perspectives** before writing code:

```markdown
---
## Agent Enrichment (added by <your-name>)

### Problem Definition
<what you found after inspecting code/logs/tests>

### Scope & Constraints
<what's in scope, what's out, what constraints exist>

### Acceptance Criteria
- [ ] <verifiable criterion 1>
- [ ] <verifiable criterion 2>

### 🎩 QA Review (Testability & TDD)
- **Failing tests to write first:** <what tests prove this works>
- **Edge cases:** <what could break>
- **Regression risk:** <what existing behavior could be affected>

### 🎩 SRE Review (Observability & Operations)
- **SLO impact:** <does this affect availability, latency, error rate?>
- **Runbook updates:** <any new failure modes to document?>
- **Monitoring:** <new alerts, dashboards, or log patterns needed?>
- *(Skip if change is docs-only or has no operational impact)*

### Validation Plan
<how you will prove the fix works — reference the 3 validation layers>
```

**Why multi-persona?** Thinking like QA catches bugs before they exist. Thinking like SRE prevents 2AM incidents. The implementing agent owns all three hats (V1). Future V2: dedicated QA/SRE agents auto-comment on issues.

### Step 3: Comment your analysis
Add a comment: "I've analyzed this issue. Here's my understanding: [summary]. Proceeding with implementation unless corrected."

This gives humans a checkpoint to intervene if the analysis is wrong.

### Step 4: Apply `agent-ready` label
CI will validate the enrichment structure. If it passes, proceed to implementation.

### Step 5: Implement
You already have full context from the enrichment phase. Build, test, submit PR.

---

## Validation Layers

Every change must pass through layered validation before submission. The depth depends on the change type.

### Layer 1: Local (Agent VM)
Run before committing. Fast feedback.

```bash
# TEMPLATE: Replace with your project's local checks
docker build -t <image> .                    # Build succeeds
docker run --rm <image> <smoke-test-cmd>     # Basic runtime check
make test                                    # Unit tests pass
make lint                                    # Linting clean
```

Place local test scripts in `tests/local/`.

### Layer 2: Lab Cluster (Real Environment)
Deploy to a lab/staging environment and verify. Run before opening a PR.

```bash
# TEMPLATE: Replace with your project's lab deployment
oc apply -k <path> -n <test-namespace>       # Deploy to lab
oc rollout status deploy/<name> --timeout=300s
curl -k https://<route>                      # Health check
./tests/integration/verify.sh                # Integration tests
```

Place integration test scripts in `tests/integration/`.

**Validation evidence from this layer is required in the PR.** Include pod status, route health checks, and test output.

### Layer 3: Post-Merge (Production/GitOps)
After PR merge, verify the change deployed correctly to the target environment.

```bash
# TEMPLATE: Replace with your project's post-merge checks
oc get application <app> -o wide             # Argo CD sync status
./tests/post-deploy/verify.sh                # Post-deploy smoke tests
```

Place post-deploy scripts in `tests/post-deploy/`.

### When to use each layer

| Change type | Layer 1 (Local) | Layer 2 (Lab) | Layer 3 (Post-merge) |
|-------------|:-:|:-:|:-:|
| Code / logic | ✅ | ✅ | ✅ |
| K8s manifests | ✅ dry-run | ✅ | ✅ |
| Documentation | ✅ lint | — | — |
| Memory only | — | — | — |

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
