# ⚖️ CopilotTribunal

> **The ultimate orchestration framework for GitHub Copilot AI agents in VS Code.**

CopilotTribunal provides a structured, spec-driven approach to AI-assisted development. Like a well-run tribunal, it ensures order, accountability, and quality through:

- **🏛️ 3-Rule Agent Architecture** — Clear hierarchy with Main Agent orchestrating specialized subagents
- **📋 Spec-Driven Development (SDD)** — Create specifications before implementation for complex work
- **📜 Scribe Agent** — Automatic documentation and progress tracking
- **⚖️ Quality Control** — Validation, review, and approval workflows

### Prerequisites

| Requirement | Recommended |
|-------------|-------------|
| **VS Code** | Latest version |
| **GitHub Copilot** | Copilot Pro or Business subscription |
| **Copilot Mode** | Agent mode (not Ask or Edit) |
| **Model** | Claude Opus 4.5 (Preview) -- highly recommended |

**To configure:** Open Copilot Chat -> Click mode dropdown -> Select "Agent" -> Click model dropdown -> Select "Claude Opus 4.5 (Preview)"

---

## 🆕 Spec-Driven Development (SDD)

CopilotTribunal includes a **Spec-Driven Development** workflow for complex projects. Instead of "vibe coding", you create structured specifications first:

```
DISCOVERY → REQUIREMENTS → ARCHITECTURE → TASKS → BUILD
```

**When to use SDD:**
- New projects from scratch
- Complex features with 3+ components
- Major refactoring efforts
- When requirements are unclear

**How to start:**
- Say "Let's use SDD for this project"
- Or use `#sdd-kickoff` prompt
- Specs are created in `docs/specs/`

**Benefits:**
- Clear expectations upfront
- No ambiguity for AI or humans
- Living documentation
- Easier onboarding

> See [processes/spec-driven-development.md](.github/processes/spec-driven-development.md) for full workflow.

---

## 📜 The Scribe

The **Scribe** agent automatically maintains project documentation:

| Tracked | Location |
|---------|----------|
| Roadmap & milestones | `docs/dev/ROADMAP.md` |
| Version changelog | `docs/dev/CHANGELOG.md` |
| Known issues & bugs | `docs/dev/KNOWN-ISSUES.md` |
| Feature status | `docs/dev/FEATURES/` |

The Scribe is invoked after features complete, bugs are found/fixed, or milestones are reached.

> See [agents/scribe.agent.md](.github/agents/scribe.agent.md) for details.

---

## AI Setup Instructions

> **For Copilot:** When a user asks for help setting up their workspace (e.g., "help me set up", "configure copilot", "get started"), follow this orchestration workflow.

> ### CRITICAL: `ask_user` IS MANDATORY
>
> You (Copilot) MUST call `ask_user` at the end of **EVERY** response during this setup process. This applies to all interactions: questions, confirmations, status updates--everything.
>
> **Red flags:** Saying "Let me know if you need anything else" instead of calling `ask_user`.
>
> When calling `ask_user`, use `"agentName": "Main Orchestrator"`.

### Step 1: Check Top-Level Router

The workspace root (the folder opened in VS Code) needs a `.github/copilot-instructions.md` that routes to each repository's instructions.

**Check:** Does `<workspace-root>/.github/copilot-instructions.md` exist?

- The workspace root is typically one level up from individual repos (e.g., if this starter kit is at `~/Github/copilot-starter-kit/`, the workspace root is `~/Github/`)
- **If missing:** Create it using the router template below
- **If exists but missing routing table or `ask_user` rule:** Add those sections (preserve existing content)
- **If seems conflicting or unusual:** Ask user before modifying

**Router Template** (create at `<workspace-root>/.github/copilot-instructions.md`):

```markdown
# GitHub Copilot Instructions -- Multi-Repository Workspace

This workspace contains multiple repositories. Each has its own instruction set.

---

## CRITICAL: `ask_user` IS MANDATORY

The main agent MUST call `ask_user` at the end of **EVERY** response. This applies to all task types: questions, code changes, clarifications, errors, status updates--everything.

**Red flags:** Saying "Let me know if you need anything else" instead of calling `ask_user`.

**Enforcement:** If you are about to finish a response without calling `ask_user`, STOP and add the tool call.

When calling `ask_user`, provide your agent name:
- `"agentName": "Main Orchestrator"` for the main agent
- `"agentName": "Generic Sub-Agent"` for unnamed subagents
- `"agentName": "<your-name>"` if you have an `.agent.md` file

---

## FIRST ACTION -- Repository Detection

Before responding, identify which repository the user is working in:

| Repository | Detection | Instructions Location |
|------------|-----------|----------------------|
| **[repo-name]** | File path contains `[repo-name]/` | `[repo-name]/.github/copilot-instructions.md` |

**Action:** Read and follow the instructions from the detected repository.

---

## CRITICAL: Agent Priority Order

When invoking agents (Scribe, Planner, Worker, etc.):

1. **FIRST:** Check `<project>/.github/agents/` for project-specific agent
2. **FALLBACK:** Only if missing or vague, use `CopilotTribunal/.github/agents/`

**Example for USD project:**
- Scribe → Use `USD/.github/agents/scribe.agent.md` (has project-specific domain)
- NOT → `CopilotTribunal/.github/agents/scribe.agent.md`

**CopilotTribunal is a TEMPLATE repository** — its agents are reference implementations.
Each project's agents may have customizations which need to be prioritized.

---

## When Context Is Ambiguous

If you cannot determine which repository the user is working in:
1. Ask the user which repository they're working with
2. Read the appropriate instruction file before proceeding

---

## Cross-Repository Work

When working across multiple repositories:
1. Read instructions from both repositories
2. Follow the more restrictive rule when there's a conflict
3. Clearly indicate which repository each action applies to

---

## CRITICAL: No Auto-Commit

**NEVER automatically make a git commit.** Always ask the user first:

1. Complete your work (code, docs, etc.)
2. Call `ask_user` to ask: "Ready to commit? I can commit with message 'X' or provide the message for you to commit manually."
3. Only commit if user explicitly approves

This prevents unwanted commits when the user has additional input or wants to review changes first.

---

## Updating Tribunal in Projects

When CopilotTribunal is updated and the user asks to propagate changes:

1. **Read** the Update Workflow section in CopilotTribunal/README.md
2. **Compare** changed files in CopilotTribunal with project versions
3. **Merge** new content while preserving `<!-- PROJECT-SPECIFIC -->` sections
4. **Present** changes via `plan_review` before applying

**Key rule:** Never blindly overwrite — always merge with customizations preserved.
```

### Step 2: Discover Repositories

Scan the workspace root for all repositories (folders containing `.git/` or `README.md`).

**Classify each repository:**

| Classification | Criteria | Action |
|----------------|----------|--------|
| **STARTER-KIT** | This folder (`copilot-starter-kit/`) | Skip -- explain it's a template, not a project |
| **FULLY-CONFIGURED** | Has `.github/copilot-instructions.md` with NO `<!-- TODO:` markers | Skip -- report as "ready, no changes needed" |
| **NEEDS-SETUP** | Missing `.github/` folder OR has `.github/copilot-instructions.md` with `<!-- TODO:` markers | Guide through setup |
| **NEW-PROJECT** | User only has the starter kit | Offer to create a new repository |

**Present a status table to the user:**

```markdown
## Workspace Status

| Repository | Status | Action Needed |
|------------|--------|---------------|
| copilot-starter-kit | Template | None (this is your source of templates) |
| my-project | [!] Needs Setup | Missing .github/ folder |
| my-other-project | [x] Configured | None |
| another-project | [!] Incomplete | Has TODO markers to fill in |
```

### Step 3: Guide Per-Repository Setup

For each repository that **NEEDS-SETUP**, ask the user if they want to configure it, then:

1. **Copy the `.github/` folder** from this starter kit to the target repository
2. **Run the MANDATORY configuration questionnaire** (see below)
3. **Update `copilot-instructions.md`** -- Fill in all `<!-- TODO: ... -->` sections
4. **Create language-specific instruction files** for detected languages
5. **Offer agent/prompt discovery** from Awesome Copilot
6. **Update the top-level router** -- Add this repository to the routing table

#### MANDATORY: Configuration Questionnaire

**You MUST run through this questionnaire for EVERY repository, even if you can infer answers.**

Present inferred data as a confirmation table:

```markdown
## Project Analysis - Please Confirm

| Field | Inferred Value | Correct? |
|-------|----------------|----------|
| **Project Name** | [detected] | ✓ / ✗ |
| **Owner/Team** | [detected] | ✓ / ✗ |
| **Languages** | [detected] | ✓ / ✗ |
| **Frameworks** | [detected] | ✓ / ✗ |

Please confirm or correct before I proceed.
```

Then ask additional questions:
- Coding conventions and patterns?
- Folders AI should NOT modify?
- How do you run tests?

**After configuration, offer discovery:**
- "Would you like me to search Awesome Copilot for language-specific instructions?"
- "Would you like me to search for specialized agents or prompts?"

### Step 3b: VS Code Configuration (Ask User)

After repository setup, offer to configure VS Code settings:

**Extensions:**
- Check if recommended extensions from `.vscode/extensions.json` are installed
- If missing, ask: "Would you like to install the recommended extensions? (Copilot, Python, MATLAB, Jupyter, etc.)"
- If approved, install missing extensions

**Terminal Auto-Approve Rules:**
- Explain: "The starter kit includes terminal auto-approve rules that let Copilot run common commands (ls, mkdir, git status, python, npm, etc.) without asking for approval each time."
- Ask: "Would you like to add these rules to your VS Code settings? (Recommended for smoother workflow)"
- If approved:
  - For per-repo: Copy `.vscode/settings.json` to the target repository
  - For workspace-wide: Offer to merge into User Settings

**Show what will be auto-approved:**
```
File operations: ls, cat, head, tail, mkdir, cp, mv
Python: python, pip, pytest, poetry, ruff
Node.js: npm, node, npx, yarn, pnpm
Git: git status, git diff, git log, git branch
Build tools: make, cargo, go build, dotnet
```

### Step 4: Handle New Project Requests

If the user only has the starter kit and wants to create a new project:

1. Ask for the project name
2. Create the folder at `<workspace-root>/<project-name>/`
3. Copy `.github/` from this starter kit
4. Create a basic `README.md`
5. Initialize git
6. Run the configuration workflow (Step 3)
7. Update the top-level router

### Step 5: Completion

After all repositories are configured:

1. **Summarize** what was created/updated
2. **Explain** how to use the system going forward
3. **Recommend** periodic review of instructions (every few months)
4. **Call `ask_user`** to confirm everything looks good

### Special Cases

#### User Is Not in Agent Mode

If user asks for setup but Copilot cannot access tools (file creation, terminal, etc.):
1. Explain: "To set up your workspace, I need to be in Agent mode. Please:"
2. Instructions: 
   - Click the chat mode dropdown (next to the prompt field)
   - Select "Agent" mode
   - Try your request again
3. If Agent mode is unavailable, point to the [Manual Setup](#manual-setup-alternative) section

#### User Has Not Configured the Recommended Model

For best results, Copilot should use **Claude Opus 4.5 (Preview)** or a similar high-capability model.

If you detect the user might be using a less capable model (e.g., responses seem limited, tool calls fail unexpectedly, or user mentions model issues):
1. Explain: "This starter kit works best with Claude Opus 4.5 (Preview). To change your model:"
2. Instructions:
   - Click the model dropdown in the Copilot chat panel (usually shows "GPT-4o" or similar)
   - Select "Claude Opus 4.5 (Preview)" or "Claude Sonnet 4" 
   - If not available, check that you have GitHub Copilot Pro or Business subscription
3. Note: Model availability depends on your Copilot subscription tier

#### User Asks to "Update" or "Refresh" Existing Setup

If user says "update my instructions", "refresh copilot setup", or similar:
1. This is NOT initial setup -- skip Steps 1-4
2. Point to the [Personalizing Instructions](#personalizing-instructions) section
3. Offer to run the Awesome Copilot discovery prompts
4. Help review and update specific instruction files

#### User Hasn't Cloned the Starter Kit Yet

If user asks for setup help but the starter kit isn't in the workspace:
1. Explain they need to clone it first
2. Provide: `git clone https://github.com/camival1999/copilot-starter-kit.git`
3. Ask them to try again after cloning

#### User Has Repos in Scattered Locations

If repositories are in different folders (not under one workspace root):

> **Recommended:** Move all repositories under a single workspace folder (e.g., `~/Github/` or `~/Projects/`). This enables:
> - Single top-level router managing all repos
> - Consistent `ask_user` enforcement
> - Easier workspace management
>
> **To transition:** Create a workspace folder, move or re-clone repos there, then run setup again.

If user prefers scattered repos, explain they'll need to:
- Manage separate VS Code workspaces
- Have a router in each workspace root
- This is not recommended but supported

---

## Quick Start (For Humans)

**Just ask Copilot: "Help me set up"** -- The AI will analyze your workspace and guide you through configuration.

Or if you prefer manual setup, see the [Manual Setup](#manual-setup-alternative) section below.

---

## Recommended Workspace Structure

For the best experience, organize all repositories under a single workspace folder:

```
~/Github/                              <- Workspace root (open this in VS Code)
+-- .github/
|   +-- copilot-instructions.md        <- Top-level router (auto-created)
+-- CopilotTribunal/                   <- This template (keep for reference)
+-- my-project/                        <- Your project
|   +-- .github/                       <- Per-repo instructions
|   +-- docs/
|   |   +-- specs/                     <- SDD specifications
|   |   +-- dev/                       <- Scribe-maintained tracking
|   +-- src/                           <- Source code
+-- another-project/                   <- Another project
|   +-- .github/                       <- Per-repo instructions
+-- ...
```

**Benefits:**
- One router manages all repositories
- Consistent `ask_user` enforcement across all repos
- SDD specs and Scribe tracking per project
- Easy to add new repositories
- Copilot can cross-reference between projects when needed

> See [standard-repo-structure.md](.github/processes/standard-repo-structure.md) for the full recommended project structure.

---

## Personalizing Instructions

The initial setup creates a working configuration, but **personalization is encouraged**:

### Per-Repository Customization

Each repository's `.github/copilot-instructions.md` should reflect that specific project:
- Coding conventions unique to that project
- Architecture decisions and patterns
- Testing strategies
- Team-specific workflows

### Ongoing Maintenance

Revisit your instructions periodically (every few months or after major changes):
- Are the conventions still accurate?
- Has the folder structure changed?
- Are there new patterns to document?
- Have team practices evolved?

### Discovering Better Instructions

Use the **Awesome Copilot** prompts to find community-tested instructions:

| Prompt | Purpose |
|--------|---------|
| `#suggest-awesome-github-copilot-instructions` | Find language-specific instruction templates |
| `#suggest-awesome-github-copilot-agents` | Discover specialized agents for your workflow |
| `#suggest-awesome-github-copilot-prompts` | Find useful prompt templates |

These prompts search [github.com/github/awesome-copilot](https://github.com/github/awesome-copilot) and suggest relevant content for your tech stack.

---

## 🔄 Updating Existing Projects

When CopilotTribunal is updated with new features, directives, or fixes, you need to propagate those changes to your existing projects **without overwriting customizations**.

> **For Copilot:** When a user asks to "update tribunal", "sync .github", "propagate changes", or similar, follow this workflow.

### Update Workflow

1. **Identify what changed** in CopilotTribunal:
   - Check `CopilotTribunal/docs/dev/CHANGELOG.md` for recent changes
   - Or ask the user what specific updates they want to propagate

2. **For workspace router** (`<workspace-root>/.github/copilot-instructions.md`):
   - Compare with CopilotTribunal's router template in README.md
   - **ADD** new sections (e.g., new mandatory rules)
   - **PRESERVE** user's routing table and custom rules
   - **MERGE** conflicting sections by combining both (user customizations take priority)

3. **For project `.github/` folders** (e.g., `USD/.github/`, `JARVIS/.github/`):
   - Compare each file with CopilotTribunal's template version
   - Use this merge strategy:

| File Type | Merge Strategy |
|-----------|----------------|
| `copilot-instructions.md` | Add new rules, preserve project-specific sections (marked with `<!-- PROJECT-SPECIFIC -->`) |
| `agents/*.agent.md` | Add new agents, update existing only if project hasn't customized them |
| `processes/*.md` | Replace with latest (these are reference docs) |
| `prompts/*.prompt.md` | Add new prompts, preserve custom prompts |
| `instructions/*.instructions.md` | Preserve all (these are project-specific by design) |

4. **Present changes before applying** using `plan_review`:
   - Show what will be added/modified/preserved
   - Wait for user approval

### Marking Project-Specific Sections

To protect customizations during future updates, wrap them with markers:

```markdown
<!-- PROJECT-SPECIFIC: START -->
## My Custom Section
This section contains project-specific rules that should not be overwritten.
<!-- PROJECT-SPECIFIC: END -->
```

### Quick Update Commands

| User Says | Action |
|-----------|--------|
| "Update tribunal" | Full sync of all projects |
| "Update agents" | Sync only `.github/agents/` |
| "Update processes" | Replace `.github/processes/` with latest |
| "Add new rule X" | Add specific rule to all projects |

---

## What's Included

| Component | Purpose |
|-----------|---------|
| **3-Rule Architecture** | Simple, enforceable agent coordination rules |
| **Spec-Driven Development** | Create specs before code for complex projects |
| **Scribe Agent** | Automatic documentation and progress tracking |
| **Curated Agent Profiles** | Planner, Worker, Validator, Spec Writer, Scribe, Generic |
| **Tiered Instructions** | Always-on core rules + auto-scoped file-specific guidance |
| **Prompt Files** | SDD kickoff + setup wizard + Awesome Copilot discovery |
| **Standard Repo Structure** | Consistent folder layout for new projects |
| **Quality Processes** | Agent patterns and lifecycle documentation |
| **VS Code Settings** | Terminal auto-approve rules and Copilot configuration |

---

## Manual Setup (Alternative)

<details>
<summary>Click to expand manual setup steps</summary>

### 1. Copy the `.github/` and `docs/` folders to your repository

```bash
cp -r CopilotTribunal/.github /path/to/your-repo/
cp -r CopilotTribunal/docs /path/to/your-repo/
```

### 2. (Optional) Copy VS Code Settings

```bash
cp -r CopilotTribunal/.vscode /path/to/your-repo/
```

### 3. Update `copilot-instructions.md`

Open `.github/copilot-instructions.md` and update all sections marked with `<!-- TODO: ... -->`:

1. **Repository Description** -- Replace placeholder with your project description
2. **Repository Architecture** -- Add your folder structure and tool mapping
3. **Instruction Routing** -- Map file patterns to instruction files
4. **Critical Conventions** -- Define your project-specific patterns
5. **Code Quality Triggers** -- Customize triggers for your tech stack

### 4. Create Language-Specific Instructions

Create instruction files for your tech stack in `.github/instructions/`:

| Language/Framework | Suggested Filename | `applyTo` Pattern |
|--------------------|-------------------|-------------------|
| Python | `python.instructions.md` | `**/*.py` |
| TypeScript/React | `typescript.instructions.md` | `**/*.ts,**/*.tsx` |
| Go | `go.instructions.md` | `**/*.go` |
| MATLAB | `matlab.instructions.md` | `**/*.m` |
| C#/.NET | `dotnet.instructions.md` | `**/*.cs` |

Each file should have frontmatter with `applyTo` patterns:

```yaml
---
description: 'Python development standards for this project'
applyTo: '**/*.py'
---
```

### 5. (Multi-Repo Only) Create Top-Level Router

If your workspace has multiple repositories, create `<workspace-root>/.github/copilot-instructions.md` using the router template from the [AI Setup Instructions](#-ai-setup-instructions) section above.

### 6. Test the Setup

Start a conversation with Copilot and verify:

- [ ] Agent calls `ask_user` at the end of responses
- [ ] Instructions are loading for your file types

</details>

---

## Core Workflows

### Pre-Response Check

Every response follows the 3-rule architecture from `.github/copilot-instructions.md`:

```
User prompt -> Main agent processes -> Subagent work (if needed) -> ask_user
```

**Key rules:**
1. **`ask_user` is mandatory** — Main agent must call at end of every response
2. **3-element subagent header** — Declaration, date, profile reference
3. **Subagents never call `ask_user`** — They return results to main agent

### 3-Rule Architecture

The **main agent** is the orchestrator and user-facing agent. It:
- Receives user prompts and determines approach
- Spawns subagents for complex or parallelizable tasks
- Triggers SDD workflow for new projects or complex features
- Calls `ask_user` at end of every response

**Subagents** are spawned by the main agent as needed:
- **Planner** — Breaks down complex tasks into steps
- **Worker** — Implements code, documentation, analysis
- **Validator** — Reviews outputs for quality and correctness
- **Spec Writer** — Creates SDD specifications
- **Scribe** — Maintains documentation and tracks progress
- **Generic Subagent** — Flexible helper for ad-hoc tasks

Subagents **never** call `ask_user` — they return results to main agent (max 250 lines).

### Tiered Instruction System

| Tier | Location | Loading |
|------|----------|---------|
| **Tier 1** | `copilot-instructions.md` | Always-on |
| **Tier 2** | `instructions/*.instructions.md` | Auto-scoped by file pattern |
| **Tier 3** | Tool READMEs, `processes/` | On-demand |

---

## File Structure

```
CopilotTribunal/
+-- README.md                      # This file
+-- .github/
|   +-- copilot-instructions.md    # Core agent rules (CUSTOMIZE THIS)
|   +-- AGENTS.md                  # Pointer for non-Copilot AI tools
|   +-- agents/
|   |   +-- README.md                  # Agent directory overview
|   |   +-- planner.agent.md           # Task decomposition subagent
|   |   +-- worker.agent.md            # Implementation subagent
|   |   +-- qa-validator.agent.md      # Quality review subagent
|   |   +-- spec-writer.agent.md       # SDD specification writer
|   |   +-- scribe.agent.md            # Documentation & progress tracker
|   |   +-- generic-subagent.agent.md  # Flexible ad-hoc subagent
|   +-- instructions/
|   |   +-- markdown.instructions.md   # Base Markdown standards
|   +-- processes/
|   |   +-- agent-patterns.md              # Workflow patterns and lifecycle
|   |   +-- spec-driven-development.md     # SDD workflow reference
|   |   +-- standard-repo-structure.md     # Recommended folder structure
|   +-- prompts/
|       +-- sdd-kickoff.prompt.md                           # SDD initiation wizard
|       +-- setup-wizard.prompt.md                          # Interactive setup
|       +-- suggest-awesome-github-copilot-*.prompt.md      # Discovery prompts
+-- docs/
    +-- README.md                  # Docs index
    +-- specs/                     # SDD specification templates
    |   +-- DISCOVERY.md
    |   +-- REQUIREMENTS.md
    |   +-- ARCHITECTURE.md
    |   +-- TASKS.md
    +-- dev/                       # Scribe-maintained tracking
        +-- ROADMAP.md
        +-- CHANGELOG.md
        +-- KNOWN-ISSUES.md
        +-- FEATURES/
```

---

## Customization Guide

### Discovering Agents, Instructions & Prompts

This starter kit is intentionally minimal. Use the **Awesome Copilot** discovery prompts to find and install only what you need:

| Prompt | Purpose |
|--------|----------|
| `#suggest-awesome-copilot-agents` | Discover specialized agents for your workflow |
| `#suggest-awesome-copilot-instructions` | Find language-specific instruction templates |
| `#suggest-awesome-copilot-prompts` | Find useful prompt templates |

These prompts search [github.com/github/awesome-copilot](https://github.com/github/awesome-copilot) and suggest relevant content for your tech stack.

### Adding Custom Prompts

1. Create `.github/prompts/your-prompt.prompt.md`
2. Add frontmatter with `description`, `agent`, `tools`
3. Define the prompt's workflow and output format

---

## Prompts Overview

| Prompt | Purpose |
|--------|---------|
| **sdd-kickoff** | 🆕 Start Spec-Driven Development workflow for new projects |
| **setup-wizard** | Interactive wizard to customize the starter kit for your repo |
| **suggest-awesome-copilot-agents** | Discover pre-built agents from the community |
| **suggest-awesome-copilot-instructions** | Discover language-specific instruction templates |
| **suggest-awesome-copilot-prompts** | Discover useful prompt templates |

> **Need more?** Use the discovery prompts to find community-tested agents, instructions, and prompts from [Awesome Copilot](https://github.com/github/awesome-copilot).

---

## Troubleshooting

### Agent doesn't call `ask_user`

- This is enforced in the 3-rule section of `copilot-instructions.md`
- Check that the critical rules section is not modified

### Instructions not auto-loading

- Verify `applyTo` frontmatter patterns match your files
- Use glob patterns like `**/*.py` not regex
- Bracket characters need escaping: `[[]Folder[]]/**/*.py`

### Subagent output too verbose

- Check `CRITICAL: Subagent Rules` section in `copilot-instructions.md`
- Subagents should return max 250 lines

---

## Resources

- [VS Code Copilot Customization Docs](https://code.visualstudio.com/docs/copilot/customization)
- [Awesome Copilot Repository](https://github.com/github/awesome-copilot)
- [Custom Instructions Documentation](https://code.visualstudio.com/docs/copilot/customization/custom-instructions)
- [Prompt Files Documentation](https://code.visualstudio.com/docs/copilot/customization/prompt-files)
- [AGENTS.md Specification](https://agents.md/)

---

## License

CopilotTribunal is adapted for personal usage by Camilo Valencia, based on Github's Awesome Copilot Project.

---

## Changelog

### v5.0.1 (2025-01-18)
- **NEW: Update Workflow** -- Instructions for propagating Tribunal changes to existing projects without overwriting customizations
- **NEW: Code Quality Directive** -- QA Validator now enforces zero Pylance/linter warnings (with MCU exceptions)
- **NEW: No Auto-Commit Rule** -- Main agent must ask before making commits
- Updated router template with Update Workflow section and new Quick Links

### v5.0.0 (2025-12-24)
- **🎉 Renamed to CopilotTribunal** -- New identity reflecting the structured, tribunal-like workflow
- **NEW: Spec-Driven Development (SDD)** -- Create specs before code for complex projects
  - Added `spec-writer.agent.md` for specification generation
  - Added `sdd-kickoff.prompt.md` for interactive SDD workflow
  - Added `spec-driven-development.md` process documentation
  - Added `docs/specs/` folder with DISCOVERY, REQUIREMENTS, ARCHITECTURE, TASKS templates
- **NEW: Scribe Agent** -- Automatic documentation and progress tracking
  - Added `scribe.agent.md` for documentation maintenance
  - Added `docs/dev/` folder with ROADMAP, CHANGELOG, KNOWN-ISSUES, FEATURES templates
- **NEW: Standard Repo Structure** -- Consistent folder layout for new projects
  - Added `standard-repo-structure.md` with language-specific adaptations
- Updated `copilot-instructions.md` with SDD detection and Scribe triggers
- Updated all agent profiles to v5.0

### v4.0.0 (2025-12-23)
- **Simplified to 3-rule architecture** -- Replaced CEA/COA with simple main agent/subagent model
- Removed CEA-checklist.md, coa-protocol.md, architecture-overview.md, audit-procedures.md
- Removed coa.agent.md (replaced by main agent being the orchestrator)
- Renamed validator.agent.md to qa-validator.agent.md for clarity
- Updated all agent profiles with 3-element header (declaration, date, profile)
- Streamlined processes/ to just agent-patterns.md

### v1.3.0 (2025-05-17)
- **Replaced Manager/Subagent with CEA/COA architecture** -- Chief Executive and Operational agents
- Added curated agent profiles: `coa.agent.md`, `planner.agent.md`, `worker.agent.md`, `validator.agent.md`, `generic-subagent.agent.md`
- Replaced Director Checklist with CEA Structured Audit Table (`CEA-checklist.md`)
- Added `coa-protocol.md` for COA evaluation and routing
- Updated `architecture-overview.md` with CEA/COA reference
- ASCII-safe encoding throughout all files

### v1.2.0 (2025-12-15)
- **Slimmed down to 10 essential files** -- agents/prompts/instructions fetched on-demand
- Removed 13 agent files, 10 prompt files, 3 instruction files, 2 process files
- Added Awesome Copilot discovery workflow to setup wizard
- Users now discover and install only what they need

### v1.1.0 (2025-12-15)
- **NEW: Intelligent AI Setup** -- Just ask "help me set up" for guided workspace configuration
- Automatic workspace detection and repository classification
- Auto-creation of top-level router for multi-repo workspaces
- Smart detection of fully-configured vs needs-setup repositories
- Simplified onboarding -- no manual steps required

### v1.0.0 (2025-12-15)
- Initial release with full agent/prompt/instruction kit
- Templated `copilot-instructions.md` with TODO markers
- Setup wizard prompt for easy customization (`#setup-wizard`)
- 13 agents, 14 prompts, 4 instruction files, 4 process files
