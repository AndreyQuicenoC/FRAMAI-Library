@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implement the following User Story using the user-story-implementation multi-agent workflow.

## Execution instructions
1. **Orchestrator** analyzes and classifies the user story, generates the shared contract and execution_plan
2. According to the plan, run ONLY the necessary agents:
   - Backend: @.ai-vexarq/workflows/user-story-implementation/backend.xml
   - Frontend: @.ai-vexarq/workflows/user-story-implementation/frontend.xml
   - Integrator: @.ai-vexarq/workflows/user-story-implementation/integrator.xml
3. Each agent updates docs/ (user-stories.md, code-structure.md, implementation-report.md, integration-report.md)
4. Implement real code and run tests
5. Do not stop at analysis alone; produce verifiable changes

---

## User Story

**ID:** HU-XXX

**As** [actor]  
**I want** [action]  
**So that** [benefit]

### Acceptance criteria
1. [Verifiable criterion]
2. [Verifiable criterion]
3. ...

---

## Project context

**Path:** .

**Additional constraints:** None

**Requested scope:** auto (let the Orchestrator classify)

---

## Expected output

- Documentation updated in `docs/user-stories.md`, `docs/code-structure.md`, `docs/implementation-report.md`, `docs/integration-report.md`
- Code implemented and working
- Tests executed and passing
- Traceability user story → code → tests → evidence
