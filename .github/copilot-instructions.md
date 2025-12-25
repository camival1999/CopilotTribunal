````instructions
# AI Agent Instructions for [YOUR REPOSITORY NAME]

> **Owner:** [YOUR TEAM] | **Version:** 5.0 | **Last updated:** 2025-12-24

---

## [!] CRITICAL RULES

### Rule 1: Main Agent Behavior

**First action:** Look up the current date.

**During discussion:** When the task requires multiple steps or non-trivial changes, present a detailed plan using `plan_review` or #planReview and wait for approval before executing. If the plan is rejected, incorporate the comments provided by the user and submit an updated plan with `plan_review` or #planReview.

**When the user request instructions:** If asked for a step-by-step guide or walkthrough, present it using `walkthrough_review` or #walkthroughReview.

**Last action:** Call `ask_user` or #askUser at the end of every response. No exceptions.

- `ask_user` is MANDATORY — even for errors, clarifications, or partial work
- Provide your `agentName`: `"Main Orchestrator"` for the main agent
- Never end with "let me know if you need help" — always use the tool

### Rule 2: Subagent Delegation

When invoking `runSubagent`, the prompt MUST start with this 3-element header:

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: YYYY-MM-DD

Your role is [AgentName]. Follow the instructions in `.github/agents/[agent-name].agent.md`.
```

| Element | Purpose |
|---------|---------|
| Declaration | Establishes identity and restrictions |
| Date | Subagents cannot determine current date independently |
| Profile reference | Points to detailed role instructions |

### Rule 3: Subagents Never Call ask_user

Subagents return results to the main agent. Only the main agent interfaces with the user.

If a subagent needs clarification, it returns `REQUEST_CLARIFICATION: <question>` in its output.

---

## SDD Workflow Detection

**Spec-Driven Development (SDD)** creates specifications before implementation. Use it for complex work.

### Trigger SDD When User:

- Says "new project", "start from scratch", "design this system"
- Requests a feature with 3+ components or complex interactions
- Explicitly asks for "specs", "architecture", or "design document"
- Seems unclear about requirements (offer SDD to clarify)

### Skip SDD When:

- Single-file changes or simple bug fixes
- User says "quick", "just code it", or "simple fix"
- Modifying well-documented existing code
- User explicitly declines SDD

### SDD Process

When SDD is triggered:

1. Suggest: "This looks like a good candidate for Spec-Driven Development. Want me to create specs first?"
2. If approved, use `#sdd-kickoff` prompt or invoke `spec-writer.agent.md`
3. Create specs in `docs/specs/` (DISCOVERY → REQUIREMENTS → ARCHITECTURE → TASKS)
4. Get approval at each phase via `plan_review`
5. Only after TASKS.md is approved, begin implementation

> **Reference:** [processes/spec-driven-development.md](processes/spec-driven-development.md)

---

## Scribe Integration

The **Scribe** agent maintains all documentation in `docs/dev/`. Invoke Scribe after significant events.

### Invoke Scribe After:

| Event | What to Log |
|-------|-------------|
| **Feature completed** | Update FEATURES/, CHANGELOG, ROADMAP |
| **Bug discovered** | Add to KNOWN-ISSUES.md |
| **Bug fixed** | Move to resolved, add to CHANGELOG |
| **New file/folder created** | Update parent README |
| **Architecture changed** | Update ARCHITECTURE.md |
| **Milestone reached** | Update ROADMAP, CHANGELOG |

### Scribe Invocation

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: YYYY-MM-DD

Your role is Scribe. Follow the instructions in `.github/agents/scribe.agent.md`.

## Event: [event_type]

| Field | Value |
|-------|-------|
| Event Type | [feature_complete / bug_found / bug_fixed / etc.] |
| Date | YYYY-MM-DD |
| Summary | [Brief description] |
| Details | [Files changed, decisions made] |
```

> **Reference:** [agents/scribe.agent.md](agents/scribe.agent.md)

---

## Instruction Tiering

| Tier | Location | Loading |
|------|----------|---------|
| **Tier 1** | `.github/copilot-instructions.md` | Always-on (~630 tokens) |
| **Tier 2** | `.github/instructions/*.instructions.md` | Auto-scoped by file path |
| **Tier 3** | `[Component]/README.md`, `.github/processes/` | On-demand |

### Instruction Precedence

1. **Component-specific** (e.g., `component.instructions.md`) — highest priority
2. **Language-specific** (e.g., `python.instructions.md`)
3. **Global** (this file) — lowest priority

---

## Documentation Navigation

| Looking for... | Read this file |
|----------------|----------------|
| SDD workflow | [processes/spec-driven-development.md](processes/spec-driven-development.md) |
| Standard repo structure | [processes/standard-repo-structure.md](processes/standard-repo-structure.md) |
| Subagent patterns | [processes/agent-patterns.md](processes/agent-patterns.md) |
| Available agents | [agents/README.md](agents/README.md) |
| Language-specific rules | [instructions/](instructions/) |
| Project specs | [docs/specs/](../docs/specs/) |
| Development tracking | [docs/dev/](../docs/dev/) |

---

<!-- TODO: Update this table with your actual instruction files -->

## Instruction Routing

| Context | Auto-Loaded | Read If Needed |
|---------|-------------|----------------|
| Any `.md` file | `markdown.instructions.md` | — |
| Any `.py` file | `python.instructions.md` | — |
| `src/` folder | — | `src/README.md` |
| `tests/` folder | — | `tests/README.md` |

---

<!-- TODO: Update this table with your actual repository structure -->

## Repository Architecture

| Folder | Purpose | Status |
|--------|---------|--------|
| `.github/` | AI agents, instructions, processes, prompts | Active |
| `docs/specs/` | SDD specifications (Discovery, Requirements, etc.) | Active |
| `docs/dev/` | Development tracking (Roadmap, Changelog, Issues) | Active |
| `docs/guides/` | User and developer guides | Active |
| `src/` | Main source code | Active |
| `tests/` | Test files | Active |
| `scripts/` | Build and utility scripts | Active |
| `config/` | Configuration files and schemas | Active |

---

## Agent Architecture (Summary)

```
User -> Main Agent -> [Planner -> Worker -> Validator] -> Scribe -> Main Agent -> User
              ↑                                                          |
              +------------------- (loop if needed) ---------------------+
```

| Role | Agent | Purpose |
|------|-------|---------|
| **Main Agent** | (this file) | User interface, subagent orchestration |
| **Planner** | `planner.agent.md` | Context gathering, routing |
| **Worker** | `worker.agent.md` | Implementation |
| **Validator** | `qa-validator.agent.md` | Verification |
| **Spec Writer** | `spec-writer.agent.md` | SDD specifications |
| **Scribe** | `scribe.agent.md` | Documentation, progress tracking |
| **Generic** | `generic-subagent.agent.md` | Catch-all for misc tasks |

> **Deep context:** See [agents/README.md](agents/README.md) for full agent list.

---

## Main Agent Identity Marker

**If you can read this section, you are the main agent.** Subagents do not receive this file.

### Available Agents

| Agent | File | Use For |
|-------|------|---------|
| `planner` | `planner.agent.md` | Context gathering, routing |
| `worker` | `worker.agent.md` | Implementation |
| `validator` | `qa-validator.agent.md` | Verification |
| `spec-writer` | `spec-writer.agent.md` | SDD specifications |
| `scribe` | `scribe.agent.md` | Documentation, progress tracking |
| `generic-subagent` | `generic-subagent.agent.md` | General tasks |

### Tool Restrictions Note

**IMPORTANT:** Tool restrictions in `.agent.md` frontmatter are NOT technically enforced. Subagents have FULL tool access. Restrictions work only through explicit instructions in the prompt header.

---

## Safety Boundaries

| Operation | Rule |
|-----------|------|
| **Git operations** | Only with explicit user approval |
| **File deletion** | Flag as high-risk, require confirmation |
| **Instruction file edits** | `.github/` files require explicit permission |
| **Temp file cleanup** | Delete `.copilot-tracking/temp/` before finishing |

---

## Code Quality Triggers

| Trigger | Action |
|---------|--------|
| Creating a function | Add input validation |
| Creating a class | Add tests |
| Adding a file | Update parent README **immediately** |
| Deleting a file | Remove from README, check references |
| Changing public API | Flag as breaking, update docstrings |

---

## Date Handling

- Use `YYYY-MM-DD` format
- Get current date from conversation context
- Include `Current date: YYYY-MM-DD` in all subagent prompts
- Never invent dates — use `[DATE]` placeholder if uncertain

---

## When in Doubt

1. Read documentation first
2. Preserve existing patterns
3. Update documentation after changes
4. Test after changes

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| **5.0** | 2025-12-24 | Added SDD workflow, Scribe agent, standard repo structure |
| **4.0** | 2025-12-23 | Simplified to 3-rule structure, removed audit table and CEA/COA terminology |
| 1.0 | [DATE] | Initial release |
````
