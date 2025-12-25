````markdown
# AI Agent Instructions

<!-- Owner: CopilotTribunal | Version: 5.0 | Last updated: 2025-12-24 -->

This repository uses a **3-rule agent architecture** with Spec-Driven Development and progressive disclosure.

## Tiered Instruction System

| Tier | Location | Description |
|------|----------|-------------|
| **Tier 1** | [copilot-instructions.md](copilot-instructions.md) | Always-on core agent rules (~630 tokens) |
| **Tier 2** | [instructions/](instructions/) | Auto-scoped tool/language instructions |
| **Tier 3** | [processes/](processes/), [agents/](agents/), Component READMEs | On-demand deep context |

## Tier 1: Core Rules (Always-On)

**Primary instruction file:** [copilot-instructions.md](copilot-instructions.md)

## Tier 2: Auto-Scoped Instructions

<!-- TODO: Update this list with your actual instruction files -->

**Language-specific instructions** (auto-applied by path):
- [instructions/markdown.instructions.md](instructions/markdown.instructions.md) — All `.md` files

## Tier 3: On-Demand Reference

- [processes/spec-driven-development.md](processes/spec-driven-development.md) — SDD workflow
- [processes/standard-repo-structure.md](processes/standard-repo-structure.md) — Recommended folder structure
- [processes/agent-patterns.md](processes/agent-patterns.md) — Lifecycle, escalation patterns
- [agents/README.md](agents/README.md) — Available agent profiles
- Component READMEs — Architecture and API documentation

## Quick Rules

1. **Main agent** must call `ask_user` at the end of every response
2. **Main agent** triggers SDD for new projects or complex features
3. **Main agent** invokes Scribe after significant events
4. **Main agent** delegates complex work to subagents with 3-element header
5. **Subagents** never call `ask_user` — return results to main agent only
6. **Documentation-first**: Read README files before making changes
7. **Git operations** require explicit user approval

For complete rules, see [copilot-instructions.md](copilot-instructions.md).
````
