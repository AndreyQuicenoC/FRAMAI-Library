# Input: Message sent to Cursor

```
@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implement the following user story using the user-story-implementation multi-agent workflow.

## Execution instructions
1. The **Orchestrator** analyzes and classifies the user story, and generates the shared contract and execution_plan
2. According to the plan, run ONLY the necessary agents:
   - Backend: @.ai-vexarq/workflows/user-story-implementation/backend.xml
   - Frontend: @.ai-vexarq/workflows/user-story-implementation/frontend.xml
   - Integrator: @.ai-vexarq/workflows/user-story-implementation/integrator.xml
3. Each agent updates `docs/dev-history.md` with its section
4. Implement real code and run tests
5. Do not stop at analysis alone; produce verifiable changes

---

## User Story

**ID:** HU-023

**As a** cashier  
**I want** to register a sale with products and quantities  
**So that** I can generate a ticket and update inventory

### Acceptance criteria
1. Form with a product selector, a quantity field, and a unit price
2. Total calculated automatically when products are added/modified
3. POST /api/sales persists to the database and returns 201 with the sale ID
4. Success message on screen and redirect to /sales
5. Validation errors visible inline in the form
6. Only users with the "cashier" role can register sales

---

## Project context

**Path:** .

**Stack:**
- Backend: Node.js + Express + PostgreSQL + Prisma
- Frontend: React + Vite + React Router + Axios
- Auth: JWT with roles (existing middleware)

**Additional constraints:** None

**Requested scope:** auto (let the Orchestrator classify)

---

## Expected output

- Entry added to `docs/dev-history.md` with analysis, implementation, and validation
- Code implemented and working
- Tests executed and passing
- Traceability: user story → code → tests → evidence
```

---

**Date:** 2026-09-14  
**Cursor version:** 0.42.x  
**Model:** Claude Sonnet 4.5 (Agent mode)
