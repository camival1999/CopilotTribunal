# Requirements: [Project Name]

> **Status:** Template | **Created:** YYYY-MM-DD | **Updated:** YYYY-MM-DD

---

## Functional Requirements

<!-- What the system must DO. Each requirement should be testable. -->

| ID | Requirement | Priority | Status |
|----|-------------|----------|--------|
| FR-001 | [System shall do X when Y happens] | Must | Draft |
| FR-002 | [System shall provide capability Z] | Must | Draft |
| FR-003 | [System shall allow user to A] | Should | Draft |
| FR-004 | [System shall support B under conditions C] | Should | Draft |
| FR-005 | [System shall optionally enable D] | Could | Draft |

### Priority Key (MoSCoW)

| Priority | Meaning |
|----------|---------|
| **Must** | Critical, non-negotiable — project fails without it |
| **Should** | Important, but workarounds exist if needed |
| **Could** | Nice to have, if time and resources permit |
| **Won't** | Explicitly out of scope for this version |

### Status Key

| Status | Meaning |
|--------|---------|
| Draft | Not yet validated |
| Approved | Validated and committed |
| Implemented | Code complete |
| Tested | Verified working |

---

## Non-Functional Requirements

<!-- How the system must PERFORM. Include measurable targets. -->

| ID | Category | Requirement | Target Metric |
|----|----------|-------------|---------------|
| NFR-001 | Performance | [Response time for operation X] | < 100ms |
| NFR-002 | Reliability | [System uptime requirement] | 99.9% |
| NFR-003 | Scalability | [Concurrent users/operations] | 100 simultaneous |
| NFR-004 | Usability | [Learning curve for new users] | < 30 min to basic competency |
| NFR-005 | Security | [Data protection requirement] | Encrypted at rest |
| NFR-006 | Maintainability | [Code quality standard] | >80% test coverage |

---

## User Stories

<!-- From the user's perspective: As a [role], I want [goal] so that [benefit] -->

### [User Role 1: e.g., End User]

- As a **[role]**, I want to **[action/goal]** so that **[benefit/reason]**
- As a **[role]**, I want to **[action/goal]** so that **[benefit/reason]**
- As a **[role]**, I want to **[action/goal]** so that **[benefit/reason]**

### [User Role 2: e.g., Administrator]

- As a **[role]**, I want to **[action/goal]** so that **[benefit/reason]**
- As a **[role]**, I want to **[action/goal]** so that **[benefit/reason]**

### [User Role 3: e.g., Developer]

- As a **[role]**, I want to **[action/goal]** so that **[benefit/reason]**

---

## Out of Scope

<!-- What this project explicitly does NOT include. Be specific to prevent scope creep. -->

- [Feature or capability explicitly excluded from this version]
- [Integration that will not be built]
- [Use case that is not supported]
- [Platform or environment not targeted]

---

## Dependencies

<!-- External systems, libraries, hardware, or conditions required -->

| Dependency | Type | Required For | Version/Notes |
|------------|------|--------------|---------------|
| [Library/Framework] | Library | [Which features need it] | v1.2.3+ |
| [External Service] | Service | [Which features need it] | API v2 |
| [Hardware Component] | Hardware | [Which features need it] | Specific model |
| [Other Project] | Internal | [Which features need it] | Must complete first |

---

## Assumptions

<!-- Things we assume to be true. If wrong, may require revisiting requirements. -->

1. [Assumption about user environment — e.g., "Users have stable internet connection"]
2. [Assumption about technical environment — e.g., "Python 3.10+ is available"]
3. [Assumption about business context — e.g., "Existing API will not change during development"]
4. [Assumption about data — e.g., "Input data is well-formed JSON"]

---

## Traceability

<!-- Map requirements back to goals from DISCOVERY.md -->

| Requirement | Supports Goal |
|-------------|---------------|
| FR-001 | Goal 1 |
| FR-002, FR-003 | Goal 2 |
| NFR-001 | Goal 3 |

---

## Related Documents

- [DISCOVERY.md](DISCOVERY.md) — Previous phase (vision, goals)
- [ARCHITECTURE.md](ARCHITECTURE.md) — Next phase (system design)
- [SDD Process](../../.github/processes/spec-driven-development.md) — Workflow reference
