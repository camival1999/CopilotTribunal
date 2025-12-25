# Spec-Driven Development (SDD) Process

> **Owner:** CopilotTribunal | **Version:** 1.0 | **Last updated:** 2025-12-24

> **Tier 3 On-Demand Reference:** This document describes the Spec-Driven Development workflow for new projects and complex features. Load on-demand when initiating SDD.

---

## What is Spec-Driven Development?

Spec-Driven Development (SDD) is a structured approach where **detailed specifications are created before implementation**, making specs the "source of truth" that guides both AI agents and human developers.

### Why SDD?

| Problem | SDD Solution |
|---------|--------------|
| "Vibe coding" leads to inconsistent results | Specs define clear expectations upfront |
| AI misunderstands complex requirements | Structured specs leave no ambiguity |
| Scope creep and feature drift | Requirements documented and prioritized |
| Difficult onboarding for new developers | Specs serve as comprehensive documentation |
| Hard to validate if work is "done" | Success criteria defined in advance |

---

## When to Use SDD

### Trigger SDD When:

- 🆕 Starting a **new project** from scratch
- 🏗️ Building a feature with **3+ components** or complex interactions
- 🔄 **Major refactoring** affecting multiple systems
- 🤖 The user explicitly asks for **specs, architecture, or design**
- ❓ Requirements are **unclear or ambiguous**
- 👥 **Multiple stakeholders** need alignment

### Skip SDD When:

- 🐛 Simple **bug fixes** with clear cause
- 📝 Minor **documentation updates**
- 🔧 Single-file changes with obvious intent
- ⚡ User explicitly says **"quick"** or **"just code it"**
- ✅ Modifying **well-documented** existing code

---

## SDD Phases

```
┌─────────────────────────────────────────────────────────────────────┐
│                                                                     │
│   DISCOVERY  →  REQUIREMENTS  →  ARCHITECTURE  →  TASKS  →  BUILD  │
│       │              │                │             │         │     │
│       v              v                v             v         v     │
│   DISCOVERY.md  REQUIREMENTS.md  ARCHITECTURE.md  TASKS.md  Code    │
│                                                                     │
│   ←─────────────── Specs (Pre-Implementation) ──────────────→ │     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

| Phase | Purpose | Artifact | Key Questions |
|-------|---------|----------|---------------|
| **Discovery** | Understand the "why" | `DISCOVERY.md` | What problem? Who benefits? What constraints? |
| **Requirements** | Define the "what" | `REQUIREMENTS.md` | What must it do? How well? What's out of scope? |
| **Architecture** | Design the "how" | `ARCHITECTURE.md` | What components? How do they connect? What tech? |
| **Tasks** | Plan the "when" | `TASKS.md` | What order? What depends on what? What's risky? |
| **Build** | Execute the plan | Code + Tests | Does it match specs? Does it pass tests? |

---

## Phase Details

### Phase 1: Discovery

**Goal:** Establish shared understanding of project vision and constraints.

**Agent:** `spec-writer.agent.md`

**Outputs:**
- Clear vision statement (1 sentence)
- 3-5 high-level goals
- Known constraints (technical, time, resource)
- Stakeholder identification
- Success criteria (measurable)
- Open questions requiring clarification

**Template:** See `docs/specs/DISCOVERY.md`

---

### Phase 2: Requirements

**Goal:** Define exactly what the system must do and how well.

**Agent:** `spec-writer.agent.md`

**Outputs:**
- Functional requirements (FR-XXX) with priorities
- Non-functional requirements (NFR-XXX) with metrics
- User stories by role
- Explicit "out of scope" list
- Dependencies and assumptions

**Template:** See `docs/specs/REQUIREMENTS.md`

**Prioritization (MoSCoW):**
- **Must** — Non-negotiable, project fails without
- **Should** — Important, but workarounds exist
- **Could** — Nice to have, if time/resources permit
- **Won't** — Explicitly excluded from this version

---

### Phase 3: Architecture

**Goal:** Design the system structure and key technical decisions.

**Agent:** `spec-writer.agent.md`

**Outputs:**
- System overview (diagram + description)
- Component breakdown with responsibilities
- Data flow description
- Technology stack with rationale
- Folder structure
- Interface definitions
- Security and scalability considerations

**Template:** See `docs/specs/ARCHITECTURE.md`

---

### Phase 4: Tasks

**Goal:** Break down implementation into actionable, ordered tasks.

**Agent:** `spec-writer.agent.md`

**Outputs:**
- Phased task breakdown
- Effort estimates
- Task dependencies (what blocks what)
- Risk areas with mitigations
- Definition of done

**Template:** See `docs/specs/TASKS.md`

---

### Phase 5: Build

**Goal:** Implement according to specs, validate against requirements.

**Agents:** `worker.agent.md`, `qa-validator.agent.md`, `scribe.agent.md`

**Process:**
1. Worker implements tasks in order from TASKS.md
2. Validator checks work against REQUIREMENTS.md
3. Scribe updates CHANGELOG.md and feature tracking
4. Loop until all tasks complete

---

## SDD Workflow Integration

### Main Agent Behavior

When SDD is triggered, the main agent:

1. **Invoke Spec Writer** with phase = `discovery`
2. **Present specs** to user for review/approval
3. **Iterate** on feedback until specs are approved
4. **Proceed** through remaining phases
5. **Only after TASKS.md is approved**, begin implementation

### Spec Review Cycle

```
Spec Writer creates spec
        │
        v
Main Agent presents to user (plan_review)
        │
        ├──[Approved]──> Next phase
        │
        └──[Changes requested]──> Spec Writer revises
                                        │
                                        └──> Loop
```

### Handoff to Implementation

When all specs are approved:

1. Main Agent invokes **Planner** with TASKS.md as input
2. Planner routes to appropriate **Worker** for each phase
3. **Validator** checks against REQUIREMENTS.md
4. **Scribe** logs progress in `docs/dev/`

---

## Spec Maintenance

Specs are **living documents** that evolve with the project.

### When to Update Specs

| Event | Action |
|-------|--------|
| New requirement discovered | Add to REQUIREMENTS.md, re-prioritize |
| Architecture change needed | Update ARCHITECTURE.md, notify affected tasks |
| Task blocked or scope change | Update TASKS.md, add to risk areas |
| Requirement completed | Update status in REQUIREMENTS.md |
| Major pivot | Re-run Discovery phase |

### Scribe Integration

The Scribe agent automatically updates:
- Feature completion status in `docs/dev/FEATURES/`
- CHANGELOG.md when milestones are reached
- Cross-references between specs and implementation

---

## Quick Reference: SDD Prompts

| Need | Say This |
|------|----------|
| Start new project with SDD | "Let's use SDD for this new project" |
| Generate specs for feature | "Create specs for [feature]" |
| Review existing specs | "Review the specs in docs/specs/" |
| Update a spec | "Update REQUIREMENTS.md with [change]" |
| Skip SDD (simple task) | "Just code it" or "Quick fix for..." |

---

## Related Files

- [Spec Writer Agent](../agents/spec-writer.agent.md)
- [Scribe Agent](../agents/scribe.agent.md)
- [Agent Patterns](agent-patterns.md)
- [Core Agent Rules](../copilot-instructions.md)

---

## Spec Templates Location

All spec templates are stored in `docs/specs/`:

```
docs/specs/
├── DISCOVERY.md
├── REQUIREMENTS.md
├── ARCHITECTURE.md
└── TASKS.md
```

When starting a new project, copy these templates and fill in project-specific content.
