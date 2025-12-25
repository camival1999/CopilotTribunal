# Architecture: [Project Name]

> **Status:** Template | **Created:** YYYY-MM-DD | **Updated:** YYYY-MM-DD

---

## System Overview

<!-- High-level description of the system architecture -->

[Describe the overall system: what it is, how it's structured at a high level, and how the major pieces fit together.]

### System Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   [Component A] ──────> [Component B] ──────> [Output]      │
│        │                     │                              │
│        v                     v                              │
│   [Component C] <────── [Component D]                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Components

<!-- Major system components and their responsibilities -->

| Component | Responsibility | Key Interfaces |
|-----------|----------------|----------------|
| [Component A] | [What it does, single responsibility] | [What it connects to, APIs it exposes] |
| [Component B] | [What it does, single responsibility] | [What it connects to, APIs it exposes] |
| [Component C] | [What it does, single responsibility] | [What it connects to, APIs it exposes] |
| [Component D] | [What it does, single responsibility] | [What it connects to, APIs it exposes] |

### Component Details

#### [Component A]

- **Purpose:** [Detailed description]
- **Inputs:** [What data/signals it receives]
- **Outputs:** [What data/signals it produces]
- **Dependencies:** [Other components it relies on]

#### [Component B]

- **Purpose:** [Detailed description]
- **Inputs:** [What data/signals it receives]
- **Outputs:** [What data/signals it produces]
- **Dependencies:** [Other components it relies on]

---

## Data Flow

<!-- How data moves through the system -->

```
[Input Source]
      │
      v
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  Ingestion  │ ──> │  Processing │ ──> │   Output    │
└─────────────┘     └─────────────┘     └─────────────┘
      │                   │                   │
      v                   v                   v
  [Log/Store]        [Transform]          [Display]
```

### Key Data Structures

| Structure | Purpose | Location | Format |
|-----------|---------|----------|--------|
| [Data Structure 1] | [What it represents] | [File/module where defined] | [JSON/Class/etc.] |
| [Data Structure 2] | [What it represents] | [File/module where defined] | [JSON/Class/etc.] |

---

## Technology Stack

| Layer | Technology | Rationale |
|-------|------------|-----------|
| Language | [e.g., Python 3.11] | [Why this choice — performance, ecosystem, team expertise] |
| Framework | [e.g., FastAPI] | [Why this choice] |
| Database | [e.g., SQLite] | [Why this choice] |
| UI | [e.g., PyQt6] | [Why this choice] |
| Hardware | [e.g., ESP32] | [Why this choice] |
| Build Tool | [e.g., PlatformIO] | [Why this choice] |

---

## Folder Structure

```
[ProjectName]/
├── README.md
├── .github/                    # AI & GitHub configuration
│   ├── copilot-instructions.md
│   ├── agents/
│   ├── instructions/
│   └── processes/
├── docs/                       # Documentation
│   ├── specs/                  # These specification files
│   ├── dev/                    # Development tracking
│   └── guides/                 # User guides
├── src/                        # Source code
│   ├── core/                   # [Purpose of core/]
│   │   ├── [module].py         # [What this module does]
│   │   └── ...
│   ├── [component]/            # [Purpose of component/]
│   └── utils/                  # Shared utilities
├── tests/                      # Test suite
│   ├── unit/
│   └── integration/
├── config/                     # Configuration files
│   └── schemas/                # Data schemas
└── scripts/                    # Build/utility scripts
```

---

## Interfaces

<!-- APIs, protocols, or integration points -->

### [Interface Name 1: e.g., REST API]

| Endpoint | Method | Input | Output | Description |
|----------|--------|-------|--------|-------------|
| `/api/resource` | GET | Query params | JSON list | Retrieve resources |
| `/api/resource` | POST | JSON body | JSON object | Create resource |
| `/api/resource/{id}` | PUT | JSON body | JSON object | Update resource |

### [Interface Name 2: e.g., Hardware Protocol]

| Command | Format | Response | Description |
|---------|--------|----------|-------------|
| `CMD_START` | `0x01 [params]` | `ACK/NAK` | Start operation |
| `CMD_STOP` | `0x02` | `ACK/NAK` | Stop operation |

---

## Error Handling Strategy

| Error Type | Handling Approach | Recovery |
|------------|-------------------|----------|
| Network failure | Retry with exponential backoff | Cache last known state |
| Invalid input | Validate at boundary, reject early | Return descriptive error |
| Hardware fault | Monitor watchdog, trigger safety | Alert user, safe shutdown |

---

## Security Considerations

<!-- Security-relevant architecture decisions -->

- **Authentication:** [How users/systems are authenticated]
- **Authorization:** [How permissions are managed]
- **Data Protection:** [Encryption, sanitization, etc.]
- **Attack Surface:** [Known risks and mitigations]

---

## Scalability Considerations

<!-- How the system handles growth -->

- **Horizontal Scaling:** [Can components be replicated? How?]
- **Bottlenecks:** [Known performance limits and thresholds]
- **Caching:** [What is cached and where]
- **Async Processing:** [Background jobs, queues, etc.]

---

## Design Decisions

<!-- Key architectural decisions and their rationale -->

| Decision | Options Considered | Choice | Rationale |
|----------|-------------------|--------|-----------|
| [Decision 1] | [Option A, Option B] | [Chosen] | [Why] |
| [Decision 2] | [Option A, Option B] | [Chosen] | [Why] |

---

## Related Documents

- [DISCOVERY.md](DISCOVERY.md) — Vision and goals
- [REQUIREMENTS.md](REQUIREMENTS.md) — What the system must do
- [TASKS.md](TASKS.md) — Next phase (implementation plan)
- [SDD Process](../../.github/processes/spec-driven-development.md) — Workflow reference
