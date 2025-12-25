# Standard Repository Structure

> **Owner:** CopilotTribunal | **Version:** 1.0 | **Last updated:** 2025-12-24

> **Tier 3 On-Demand Reference:** This document defines the standard folder structure for new projects. Load on-demand when setting up new repositories.

---

## Overview

This structure is designed to:
- **Support SDD workflow** with dedicated `docs/specs/` folder
- **Enable Scribe tracking** with `docs/dev/` folder
- **Scale from simple to complex** projects
- **Work across languages** with adaptable patterns

---

## Standard Structure

```
<ProjectName>/
├── README.md                           # Project overview, quick start
├── LICENSE                             # License file (MIT, Apache, etc.)
├── .gitignore                          # Language-specific ignores
│
├── .github/                            # AI & GitHub configuration
│   ├── copilot-instructions.md         # Tier 1: Core agent rules
│   ├── AGENTS.md                       # Pointer for non-Copilot AI tools
│   ├── agents/                         # Subagent profiles
│   │   ├── README.md                   # Agent directory
│   │   ├── planner.agent.md            # Context gathering, routing
│   │   ├── worker.agent.md             # Implementation
│   │   ├── qa-validator.agent.md       # Quality verification
│   │   ├── spec-writer.agent.md        # SDD specifications
│   │   ├── scribe.agent.md             # Documentation keeper
│   │   └── generic-subagent.agent.md   # Flexible fallback
│   ├── instructions/                   # Tier 2: Auto-scoped by file type
│   │   ├── README.md
│   │   └── [language].instructions.md
│   ├── processes/                      # Tier 3: On-demand workflows
│   │   ├── agent-patterns.md
│   │   ├── spec-driven-development.md
│   │   └── standard-repo-structure.md
│   └── prompts/                        # Reusable prompt templates
│       └── sdd-kickoff.prompt.md
│
├── docs/                               # All documentation
│   ├── README.md                       # Docs index
│   ├── specs/                          # SDD specifications
│   │   ├── DISCOVERY.md                # Vision, goals, constraints
│   │   ├── REQUIREMENTS.md             # Functional & non-functional
│   │   ├── ARCHITECTURE.md             # System design, components
│   │   └── TASKS.md                    # Implementation breakdown
│   ├── dev/                            # Ongoing development tracking
│   │   ├── ROADMAP.md                  # Future plans, milestones
│   │   ├── CHANGELOG.md                # Version history
│   │   ├── KNOWN-ISSUES.md             # Active bugs, workarounds
│   │   └── FEATURES/                   # Per-feature status
│   │       └── [feature-name].md
│   └── guides/                         # User & developer guides
│       ├── getting-started.md
│       └── contributing.md
│
├── src/                                # Main source code
│   ├── README.md                       # Source overview
│   ├── core/                           # Core logic, orchestration
│   ├── devices/                        # Hardware/device abstractions
│   ├── ui/                             # User interface components
│   └── utils/                          # Cross-cutting utilities
│
├── tests/                              # Test suite
│   ├── README.md                       # Testing guide
│   ├── unit/                           # Unit tests
│   ├── integration/                    # Integration tests
│   └── fixtures/                       # Test data & mocks
│
├── scripts/                            # Build, deploy, utility scripts
│   └── README.md
│
├── config/                             # Configuration files
│   ├── README.md
│   └── schemas/                        # Data schemas (JSON, YAML)
│
└── [data/, logs/, build/, dist/]       # Output folders (gitignored)
```

---

## Folder Purposes

### `.github/` — AI & GitHub Configuration

| Folder | Purpose |
|--------|---------|
| `agents/` | Subagent profiles for specialized tasks |
| `instructions/` | File-type-specific rules (auto-loaded) |
| `processes/` | Workflow documentation (loaded on-demand) |
| `prompts/` | Reusable prompt templates |

### `docs/` — Documentation

| Folder | Purpose |
|--------|---------|
| `specs/` | SDD artifacts (Discovery, Requirements, Architecture, Tasks) |
| `dev/` | Scribe-maintained development tracking |
| `guides/` | User and developer documentation |

### `src/` — Source Code

| Folder | Purpose |
|--------|---------|
| `core/` | Central logic, orchestration, main algorithms |
| `devices/` | Hardware interfaces, drivers, abstractions |
| `ui/` | User interface (GUI, CLI, web) |
| `utils/` | Shared utilities, helpers, common functions |

### `tests/` — Testing

| Folder | Purpose |
|--------|---------|
| `unit/` | Isolated component tests |
| `integration/` | Cross-component tests |
| `fixtures/` | Test data, mocks, stubs |

---

## Language-Specific Adaptations

### Python Project

```
<ProjectName>/
├── pyproject.toml                      # Modern Python config
├── requirements.txt                    # Dependencies (or use pyproject.toml)
├── src/
│   ├── __init__.py
│   ├── core/
│   │   └── __init__.py
│   └── ...
├── tests/
│   ├── conftest.py                     # pytest fixtures
│   └── ...
```

### C++/Embedded Project

```
<ProjectName>/
├── CMakeLists.txt                      # CMake config
├── platformio.ini                      # PlatformIO config (if applicable)
├── include/                            # Public headers
│   └── *.h
├── src/                                # Implementation
│   └── *.cpp
├── lib/                                # Private libraries
├── firmware/                           # Embedded-specific (replaces src/ sometimes)
```

### Node.js/TypeScript Project

```
<ProjectName>/
├── package.json
├── tsconfig.json                       # TypeScript config
├── src/
│   ├── index.ts
│   └── ...
├── dist/                               # Compiled output (gitignored)
├── node_modules/                       # Dependencies (gitignored)
```

### Mixed Language Project (like USD)

Use the standard structure, add language-specific config files at root:

```
<ProjectName>/
├── pyproject.toml                      # Python
├── CMakeLists.txt                      # C++
├── package.json                        # Node.js (for tooling)
├── src/
│   ├── python/                         # Python sources
│   ├── cpp/                            # C++ sources
│   └── ...
```

---

## docs/dev/ Templates

### ROADMAP.md

```markdown
# Roadmap

## Current Version: v0.0.0

## Upcoming Milestones

| Milestone | Target | Status | Key Features |
|-----------|--------|--------|--------------|
| v0.1.0 | YYYY-MM | 📋 Planned | [Features] |
| v0.2.0 | YYYY-MM | 📋 Planned | [Features] |

## Completed Milestones

| Version | Released | Highlights |
|---------|----------|------------|
```

### CHANGELOG.md

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/).

## [Unreleased]

### Added
- 

### Changed
- 

### Fixed
- 

## [v0.0.0] - YYYY-MM-DD

### Added
- Initial project structure
```

### KNOWN-ISSUES.md

```markdown
# Known Issues

## Active Issues

| ID | Severity | Summary | Found | Status | Workaround |
|----|----------|---------|-------|--------|------------|

## Recently Resolved

| ID | Summary | Resolved | Version |
|----|---------|----------|---------|
```

### FEATURES/[feature-name].md

```markdown
# Feature: [Feature Name]

| Field | Value |
|-------|-------|
| **Status** | 📋 Planned / 🟡 In Progress / ✅ Complete |
| **Priority** | High / Medium / Low |
| **Started** | YYYY-MM-DD or — |
| **Completed** | YYYY-MM-DD or — |
| **Related Issues** | BUG-XXX, BUG-XXX |

## Description

<!-- What this feature does -->

## Implementation Notes

<!-- Key decisions, gotchas, dependencies -->

## Files Changed

<!-- List of files created/modified for this feature -->
```

---

## Minimal Structure (Small Projects)

For simple projects, start minimal and expand as needed:

```
<ProjectName>/
├── README.md
├── .github/
│   └── copilot-instructions.md
├── src/
│   └── main.[ext]
└── tests/
```

Add folders as the project grows.

---

## .gitignore Essentials

```gitignore
# Build outputs
build/
dist/
*.exe
*.dll
*.so
*.dylib

# Dependencies
node_modules/
.venv/
venv/
__pycache__/
*.pyc

# IDE
.idea/
*.swp
*.swo

# Logs and data
logs/
data/
*.log

# OS files
.DS_Store
Thumbs.db

# Environment
.env
.env.local
```

---

## Related Files

- [SDD Process](spec-driven-development.md)
- [Agent Patterns](agent-patterns.md)
- [Core Agent Rules](../copilot-instructions.md)
