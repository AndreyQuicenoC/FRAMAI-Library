# Workflow: User Story Implementation

Implementation of User Stories (one or several) through a multi-agent architecture with dynamic routing.

## Description

Specialized pipeline that analyzes a user story or an epic, determines the project mode, classifies its scope (frontend, backend, full-stack), and runs **only** the necessary agents. It prioritizes minimal change, verifiable traceability, validation with the real stack, and safe publication.

## Architecture

```
CORE (global policies + standards + Git flow + project modes)
    ↓
ORCHESTRATOR (analysis + mode + routing + epic contract + commit plan)
    ↓
    ├── BACKEND (optional according to classification)
    ├── FRONTEND (optional according to classification)
    └── INTEGRATOR (optional according to classification; mandatory in full-stack)
```

## Agents

| Agent | File | Responsibility | Always runs |
|---|---|---|---|
| **Orchestrator** | `orchestrator.xml` | Analysis, project mode, classification, shared contract, batch plan | Yes |
| **Backend** | `backend.xml` | Server: API, DB, business logic, security, migrations, environment, container | Per plan |
| **Frontend** | `frontend.xml` | UI/UX, routes, HTTP client, mockup productionization, container | Per plan |
| **Integrator** | `integrator.xml` | Real stack, E2E, contract, cross-layer corrections, publication gate | Per plan |

## Core modules that accompany the workflow

| File | When it is loaded |
|---|---|
| `../../core/policies.xml` | Always |
| `../../core/adaptive-policies.xml` | Complexity classification, elevation factors, batch mode |
| `../../core/engineering-standards.xml` | Sections according to `policies_to_load` (security, quality, testing, environment) |
| `../../core/git-workflow.xml` | If there are branches, commits, or push |
| `../../core/project-modes.xml` | Initial gate (EXTEND_EXISTING, ARCHETYPE_EXTENSION, MOCKUP_PRODUCTIONIZATION) |

## Classifications

| User-story type | Pipeline | Example |
|---|---|---|
| `FRONTEND_ONLY` | O, F | Style change, pure UI component |
| `BACKEND_ONLY` | O, B | Endpoint, export, cron job |
| `FULL_STACK` | O, B, F, I | Form + API + persistence |
| `INTEGRATION_FIX` | I (+ B/F if necessary) | Broken contract between layers |
| `SPECIALIST_FIX` | O, (B or F) | Bounded bug in one layer |

## Usage

### Basic message (one user story)

```
@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implement the following User Story according to the implementation workflow.

## HU-023: Register sale
As a cashier I want to register a sale with products in order to generate a ticket.

### Acceptance criteria
1. Form with products, quantities, prices
2. Automatic total calculation
3. POST /api/sales persists and returns 201
4. Success message and redirection
5. Errors visible on the form

## Project
.

## Scope
auto
```

### Epic (several user stories, several repositories)

Use `../../templates/user-story-implementation/epic-full-stack.md`. It adds per-layer paths, untouchable directories, Git flow, environment, and security.

### Available templates

- **Full-stack:** `../../templates/user-story-implementation/full-stack.md`
- **Full-stack epic:** `../../templates/user-story-implementation/epic-full-stack.md`
- **Backend-only:** `../../templates/user-story-implementation/backend-only.md`
- **Frontend-only:** `../../templates/user-story-implementation/frontend-only.md`
- **Integration fix:** `../../templates/user-story-implementation/integration-fix.md`

## Output

Each agent persists to **`docs/`** (4 incremental files). In multi-repository projects, each repository keeps its own:

```markdown
# Development story

## HU-023 - Register sale
**Status:** COMPLETED
**Classification:** FULL_STACK
**Date:** 2026-09-14

### Analysis (Orchestrator)
[...]

### Backend
[Files modified, API, tests, decisions, traceability matrix, commands]

### Frontend
[Components, routes, states, tests]

### Validation (Integrator)
- Criteria met: 5/5
- E2E tests: passed
- Evidence: [...]
```

## Documents generated/updated

- `docs/user-stories.md` - Exact user stories received
- `docs/code-structure.md` - Code structures
- `docs/implementation-report.md` - Results per user story
- `docs/integration-report.md` - E2E validation per user story
- `docs/architecture.md` - Architecture updated if it changed
- `docs/quality-standards.md` - Validations applied

## Detailed flow

1. **Orchestrator** analyzes the user story and the project, determines `project_mode`, orders the batch, and emits `<execution_plan>`, the epic contract, and `commit_plan`
2. **Backend** (if applicable) implements API, logic, persistence, security, migrations; runs all the gates; commit per user story
3. **Frontend** (if applicable) productionizes the mockup if that applies, implements UI, API consumption, UX; runs all the gates; commit per user story
4. **Integrator** (if applicable) brings up the real stack, validates E2E, corrects together, and authorizes or denies publication

Each stage runs **validation in 2 rounds** (V1: requirements, V2: gates and tests).

## Metrics generated

- Acceptance Score (% criteria met)
- Coverage (tests executed vs required)
- Traceability (user story, code, test, evidence)
- Quality gates (command and result per gate)
- Defect Rate (errors detected per stage)

## Complete example

See `tests/ai-strategy/user-story-example/` for a documented real execution.

## Detailed guide

See `../../guides/user-story-implementation.xml`
