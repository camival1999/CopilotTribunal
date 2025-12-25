---
description: 'Interactive wizard to initiate Spec-Driven Development for new projects or complex features'
name: SDD Kickoff
mode: agent
tools: ['read_file', 'create_file', 'semantic_search', 'file_search', 'runSubagent']
---

# SDD Kickoff Wizard
 
You are initiating a **Spec-Driven Development (SDD)** workflow. This structured approach creates detailed specifications before implementation, ensuring clarity and alignment.

---

## Step 1: Understand the Context

First, determine what type of SDD session this is:

**Ask the user:**

> I'll help you create comprehensive specifications using SDD (Spec-Driven Development).
>
> **What are we specifying?**
> 1. 🆕 **New project** — Starting from scratch
> 2. 🏗️ **Major feature** — Complex addition to existing project
> 3. 🔄 **Refactoring** — Restructuring existing code
> 4. 📋 **Just specs** — Documentation only, no implementation
>
> Also, please describe in a few sentences what you want to build.

---

## Step 2: Discovery Phase

Once context is established, begin the Discovery phase.

**Invoke the Spec Writer agent:**

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: [TODAY'S DATE]

Your role is Spec Writer. Follow the instructions in `.github/agents/spec-writer.agent.md`.

## Task: Create DISCOVERY.md

Based on this user description:
[USER'S DESCRIPTION]

Create a complete DISCOVERY.md file following the template. Include:
- Clear vision statement
- 3-5 goals
- Known constraints
- Stakeholder analysis
- Success criteria
- Open questions (things you need clarification on)

Return the complete DISCOVERY.md content.
```

**Present to user for review using `plan_review`.**

---

## Step 3: Requirements Phase

After Discovery is approved, proceed to Requirements.

**Invoke the Spec Writer agent:**

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: [TODAY'S DATE]

Your role is Spec Writer. Follow the instructions in `.github/agents/spec-writer.agent.md`.

## Task: Create REQUIREMENTS.md

Based on:
- DISCOVERY.md: [CONTENT]
- User feedback: [ANY ADDITIONAL INPUT]

Create a complete REQUIREMENTS.md file following the template. Include:
- Functional requirements (FR-XXX) with MoSCoW priorities
- Non-functional requirements (NFR-XXX) with metrics
- User stories by role
- Out of scope items
- Dependencies and assumptions

Return the complete REQUIREMENTS.md content.
```

**Present to user for review using `plan_review`.**

---

## Step 4: Architecture Phase

After Requirements is approved, proceed to Architecture.

**Invoke the Spec Writer agent:**

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: [TODAY'S DATE]

Your role is Spec Writer. Follow the instructions in `.github/agents/spec-writer.agent.md`.

## Task: Create ARCHITECTURE.md

Based on:
- DISCOVERY.md: [CONTENT]
- REQUIREMENTS.md: [CONTENT]
- User feedback: [ANY ADDITIONAL INPUT]

Create a complete ARCHITECTURE.md file following the template. Include:
- System overview with diagram
- Component breakdown
- Data flow
- Technology stack with rationale
- Folder structure
- Interface definitions

Return the complete ARCHITECTURE.md content.
```

**Present to user for review using `plan_review`.**

---

## Step 5: Tasks Phase

After Architecture is approved, proceed to Tasks.

**Invoke the Spec Writer agent:**

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: [TODAY'S DATE]

Your role is Spec Writer. Follow the instructions in `.github/agents/spec-writer.agent.md`.

## Task: Create TASKS.md

Based on:
- DISCOVERY.md: [CONTENT]
- REQUIREMENTS.md: [CONTENT]
- ARCHITECTURE.md: [CONTENT]

Create a complete TASKS.md file following the template. Include:
- Phased task breakdown
- Effort estimates
- Task dependencies
- Risk areas with mitigations
- Definition of done

Return the complete TASKS.md content.
```

**Present to user for review using `plan_review`.**

---

## Step 6: Create Spec Files

Once all phases are approved, create the actual files:

1. Create `docs/specs/DISCOVERY.md`
2. Create `docs/specs/REQUIREMENTS.md`
3. Create `docs/specs/ARCHITECTURE.md`
4. Create `docs/specs/TASKS.md`

---

## Step 7: Setup Project Structure (Optional)

If this is a new project, offer to create the standard folder structure:

> Your specs are complete! Would you like me to:
>
> 1. **Create the project structure** based on ARCHITECTURE.md
> 2. **Start implementation** of Phase 1 tasks
> 3. **Just save specs** and stop here

If creating structure, follow the template in `.github/processes/standard-repo-structure.md`.

---

## Step 8: Notify Scribe

After specs are created, invoke the Scribe to log:

```
You are a SUBAGENT. You must NOT call ask_user. Return your results directly to the main agent.

Current date: [TODAY'S DATE]

Your role is Scribe. Follow the instructions in `.github/agents/scribe.agent.md`.

## Event: SDD Specs Created

| Field | Value |
|-------|-------|
| Event Type | specs_created |
| Date | [TODAY] |
| Project | [PROJECT NAME] |
| Summary | SDD specifications created (Discovery, Requirements, Architecture, Tasks) |
| Files | docs/specs/DISCOVERY.md, docs/specs/REQUIREMENTS.md, docs/specs/ARCHITECTURE.md, docs/specs/TASKS.md |

Create or update ROADMAP.md with the new project/feature. Update CHANGELOG.md.
```

---

## Quick Reference

| Phase | Artifact | Key Question |
|-------|----------|--------------|
| Discovery | DISCOVERY.md | What and why? |
| Requirements | REQUIREMENTS.md | What exactly must it do? |
| Architecture | ARCHITECTURE.md | How is it built? |
| Tasks | TASKS.md | In what order? |

---

## Aborting SDD

If the user wants to skip SDD at any point:

> Understood! SDD is recommended for complex projects but optional. You can:
>
> - Start implementation directly
> - Return to SDD later with `#sdd-kickoff`
> - Create partial specs (just Discovery + Architecture, for example)

---

## Related

- [SDD Process](../.github/processes/spec-driven-development.md)
- [Spec Writer Agent](../.github/agents/spec-writer.agent.md)
- [Standard Repo Structure](../.github/processes/standard-repo-structure.md)
