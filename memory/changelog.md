# Changelog — Agent Work Log

> **Purpose:** Append-only log of significant changes with context for future agents.
> **Rule:** Record what changed, why, and what you learned. Not every commit — only changes that carry forward context.

---

<!-- Append new entries at the top -->

## 2026-02-20 — Converged design: Minimal In / Strict Out

**Agent:** Tesla ⚡
**What changed:**
- Replaced strict agent-story issue template with minimal task template
- Added PR template with required validation evidence and memory checklist
- Added `issue-ready-gate.yml`: CI validates enrichment structure on `agent-ready` label
- Added `pr-checks.yml`: enforces changelog in diff, PR body sections, memory structure
- Updated AGENTS.md with full enrichment lifecycle protocol
- Added `config.yml` to allow blank issues

**Why:** Debate between Tesla and Six7, directed by Konda, converged on:
- Humans provide minimal input (zero friction)
- Agents enrich issues before implementation (agent responsibility)
- CI enforces structure at acceptance gate and PR gate (mechanical enforcement)
- Same agent owns full lifecycle: investigate → enrich → implement → submit

**What we learned:**
- Separate triager agents create handoff context loss — implementing agent should own enrichment
- Static structural checks (regex) cover 80% of validation value at 0% LLM cost
- The acceptance gate (`agent-ready` label + CI check) gives humans an intervention checkpoint

## 2026-02-20 — Added layered validation model

**Agent:** Tesla ⚡
**What changed:**
- Added 3-layer validation section to AGENTS.md (Local → Lab → Post-merge)
- Updated PR template with per-layer validation evidence sections
- Created `tests/local/`, `tests/integration/`, `tests/post-deploy/` directories with READMEs
- Removed old `tests/README.md` (replaced by per-layer structure)

**Why:** Konda specified that agents must validate locally (Docker), in a lab cluster (OKD), and post-merge (Argo CD) before and after changes. This creates a layered defense against failures at each stage.
