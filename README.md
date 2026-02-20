# AI-First Repository Template

A reusable GitHub template for repositories that are primarily maintained by AI agents.

## What This Solves

AI agents lose context between sessions. Sub-agents lose it even faster. Without shared memory, every interaction starts from zero — rediscovering constraints, repeating failed approaches, and losing hard-won knowledge.

This template provides:
- **Structured memory** that persists across agent sessions
- **Standard entry points** compatible with all major AI coding tools
- **Enforcement** via CI pipelines and issue templates
- **Failure tracking** as first-class knowledge

## How to Use This Template

1. Click **"Use this template"** on GitHub to create a new repo
2. Replace the `<!-- TEMPLATE: ... -->` sections in `AGENTS.md` with your project details
3. Customize `.github/workflows/ci.yml` with your build/test commands
4. Start working — agents will read `AGENTS.md` and `MEMORY.md` automatically

## File Structure

```
├── AGENTS.md                         # Agent entry point (universal standard)
├── CLAUDE.md                         # → symlink to AGENTS.md (Claude Code)
├── GEMINI.md                         # → symlink to AGENTS.md (Gemini CLI)
├── .github/
│   ├── copilot-instructions.md       # GitHub Copilot compatibility
│   ├── ISSUE_TEMPLATE/
│   │   └── agent-story.md            # Standardized task format for agents
│   └── workflows/
│       └── ci.yml                    # CI with memory structure validation
├── MEMORY.md                         # Shared agent knowledge base
├── memory/
│   ├── changelog.md                  # Append-only work log
│   ├── decisions/                    # Architecture Decision Records
│   │   └── 0000-template.md          # ADR template
│   └── failures/                     # Failure records (highest-value memory)
│       └── 0000-template.md          # Failure record template
├── CONTRIBUTING.md                   # Contribution guide (includes memory protocol)
├── tests/                            # Executable documentation
└── README.md                         # This file
```

## The Three Layers

### Layer 1: Instructions (static)
`AGENTS.md` — how to build, test, lint, and contribute. Compatible with the [AGENTS.md standard](https://agents.md) used by 60k+ open-source projects.

### Layer 2: Memory (dynamic)
`MEMORY.md` + `memory/` — what the team has learned. Curated knowledge, change logs, architecture decisions, and failure records.

### Layer 3: Enforcement (automated)
CI pipelines validate memory structure. Issue templates enforce structured input/output. Tests serve as executable documentation.

## Tool Compatibility

| Tool | Entry Point | Support |
|------|-------------|---------|
| GitHub Copilot | `.github/copilot-instructions.md` + `AGENTS.md` | ✅ Native |
| Claude Code | `CLAUDE.md` → `AGENTS.md` | ✅ Native |
| Gemini CLI | `GEMINI.md` → `AGENTS.md` | ✅ Native |
| OpenAI Codex | `AGENTS.md` | ✅ Native |
| Cursor | `AGENTS.md` | ✅ Native |
| Aider | `AGENTS.md` (via config) | ✅ Configurable |
| OpenClaw | `AGENTS.md` (workspace) | ✅ Native |

## Memory Principles

1. **Memory is a first-class artifact** — treat it with the same care as code
2. **Record failures, not just successes** — failed approaches are the highest-value memory
3. **Prune aggressively** — stale memory misleads; MEMORY.md capped at 500 lines
4. **One source of truth** — don't duplicate context; link instead
5. **Future agents are your users** — write for someone who knows nothing about what you just did

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).
