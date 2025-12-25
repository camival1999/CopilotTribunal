# Custom Agents

<!-- Owner: CopilotTribunal | Version: 5.0 | Last updated: 2025-12-24 -->

Registry of AI agents available for specialized tasks in this repository.

## Agent Architecture

The main agent invokes these subagents for specialized work:

| Role | Agent | Purpose |
|------|-------|--------|
| **Planner** | `planner.agent.md` | Analyzes requests, gathers context |
| **Worker** | `worker.agent.md` | Executes implementation tasks |
| **Validator** | `qa-validator.agent.md` | Verifies work answers the request |
| **Spec Writer** | `spec-writer.agent.md` | Creates SDD specifications |
| **Scribe** | `scribe.agent.md` | Maintains documentation, tracks progress |
| **Generic** | `generic-subagent.agent.md` | Catch-all for misc tasks |

## Workflow

```
User -> Main Agent -> [Planner -> Worker -> Validator] -> Scribe -> Main Agent -> User
              ↑                                                          |
              +------------------- (loop if needed) ---------------------+
```

For new projects or complex features, the SDD workflow is recommended:

```
User -> Main Agent -> Spec Writer -> [DISCOVERY -> REQUIREMENTS -> ARCHITECTURE -> TASKS]
                                               ↓
                          Worker -> Validator -> Scribe -> Main Agent -> User
```

## Usage

Invoke agents in VS Code Copilot Chat via `@agent <agent-name>` or by selecting them from the agent picker.

## Available Agents

| Agent | Role | Description |
|-------|------|-------------|
| `planner.agent.md` | Planner | Analyzes requests, gathers context |
| `worker.agent.md` | Worker | Executes implementation tasks with engineering excellence |
| `qa-validator.agent.md` | Validator | Validates work answers the user request |
| `spec-writer.agent.md` | Spec Writer | Creates SDD specifications (Discovery, Requirements, Architecture, Tasks) |
| `scribe.agent.md` | Scribe | Maintains docs/dev/ folder, tracks features/bugs/progress |
| `generic-subagent.agent.md` | Fallback | For tasks that don't match specialized profiles |

## Additional Agents

Additional specialized agents can be fetched from the **awesome-copilot MCP server**:

```
@mcp_awesome-copilot search_instructions "agent-name"
```

Examples available:
- `adr-generator` — Architecture Decision Records
- `blueprint-mode` — System architecture design
- `critical-thinking` — Critical analysis and review
- `implementation-plan` — Detailed implementation planning
- `mentor` — Code mentoring and guidance
- `specification` — Formal specification generation
- `tech-debt-remediation` — Technical debt analysis

## Subagent Constraints

All subagents in this workspace follow these rules:

1. **NEVER call `ask_user`** — only the main agent interfaces with the user
2. **NEVER spawn subagents** — only the main agent can invoke runSubagent
3. **Maximum 250 lines output** — use temp files for overflow
4. **End with "Returning to main agent."**

## Related

- [Agent Patterns](../processes/agent-patterns.md)
- [SDD Process](../processes/spec-driven-development.md)
- [Standard Repo Structure](../processes/standard-repo-structure.md)
- [Core Agent Rules](../copilot-instructions.md)
