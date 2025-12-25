---
description: 'Documentation and content creation standards for markdown files'
applyTo: '**/*.md'
lastUpdated: 2025-12-24
---
<!-- Owner: [YOUR TEAM] | Last updated: 2025-12-24 -->
# Markdown Documentation Standards

Standards for all markdown documentation in this repository.

## Document Types

| Type | Location | Purpose |
|------|----------|---------|
| **README.md** | Root, subfolders | Quick reference, architecture overview |
| **Specification** | `docs/specs/*.md` | Full API reference, formal requirements |
| **Reference Guide** | `docs/references/*.md` | Tutorials, background concepts |
| **Changelog** | `CHANGELOG.md` or README section | Version history |

---

## README.md Conventions

### Required Section Order

1. **Overview** -- One-paragraph description, key features
2. **Quick Start** -- Minimal steps to run the tool
3. **Architecture** -- Package structure, data flow diagram
4. **Key Files** -- Table of important files with descriptions
5. **User Extension Points** -- Where users add custom scripts
6. **Testing** -- How to run tests, test coverage
7. **Changelog** -- Version history (or link to CHANGELOG.md)
8. **Author** -- Owner/maintainer info

### README Constraints

- **Keep under 1000 lines** -- Link to `docs/specs/specification.md` for details
- **Use tables for file lists** -- Not prose paragraphs
- **One H1 only** -- The tool name
- **Relative links** -- Use `./docs/specs/` not absolute paths

### Good vs Bad Examples

[X] **Bad: Verbose prose for file list**
```markdown
The package contains several important files. The App.py file is the main 
application entry point that handles the GUI. The Config.py file stores
configuration parameters. The Utils.py file contains helper functions...
```

[x] **Good: Table format**
```markdown
| File | Purpose |
|------|---------|
| `App.py` | Main application entry point, GUI handler |
| `Config.py` | Configuration parameters storage |
| `Utils.py` | Helper functions |
```

---

## Specification Document Conventions

### YAML Frontmatter (Required)

```yaml
---
title: [Concise specification title]
version: 1.0
date_created: YYYY-MM-DD
last_updated: YYYY-MM-DD
owner: [Team name]
tags: [tool-name, category]
---
```

### Required Sections

| Section | Purpose |
|---------|---------|
| **Purpose & Scope** | What the spec covers, intended audience |
| **Definitions** | Acronyms, domain terms |
| **Requirements** | REQ-001, CON-001, GUD-001 format |
| **Interfaces & Data Contracts** | APIs, data structures |
| **Acceptance Criteria** | AC-001 with Given-When-Then format |
| **Examples & Edge Cases** | Code snippets, boundary conditions |

### Requirement Prefixes

| Prefix | Meaning |
|--------|---------|
| `REQ-` | Functional requirement |
| `CON-` | Constraint (must not violate) |
| `GUD-` | Guideline (recommended) |
| `PAT-` | Pattern to follow |

---

## General Markdown Style

### Headings

- **H1 (`#`)**: Document title only, one per file
- **H2 (`##`)**: Major sections
- **H3 (`###`)**: Subsections
- **Avoid H4+**: Restructure content if needed

### Code Blocks

Always specify language for syntax highlighting:

~~~markdown
```python
# Python code
result = process_data(config)
```
~~~

### Links

- **Internal files**: `[Config](src/Config.py)`
- **Relative paths**: `./docs/specs/specification.md`
- **Section anchors**: `[Testing](#testing)`

### Tables Over Lists

Use tables when data has multiple attributes:

[X] **Avoid**:
```markdown
- `run()` - Executes the analysis, takes data struct, returns results
- `plot()` - Generates visualization, takes figure handle
```

[x] **Prefer**:
```markdown
| Method | Input | Output | Description |
|--------|-------|--------|-------------|
| `run()` | data struct | results | Executes analysis |
| `plot()` | figure handle | -- | Generates visualization |
```

---

## Changelog Format

Use semantic versioning with date:

```markdown
## Changelog

### v1.2.0 (2025-12-15)
- Added batch processing support
- Fixed edge case in empty input handling
- Updated documentation for new API

### v1.1.0 (2025-11-01)
- Initial public release
```

---

## Validation Checklist

Before committing markdown changes:

- [ ] No broken internal links
- [ ] Code blocks have language specifiers
- [ ] Tables are properly formatted
- [ ] README under 1000 lines
- [ ] Frontmatter present (for specs)
- [ ] Changelog updated if API changed
