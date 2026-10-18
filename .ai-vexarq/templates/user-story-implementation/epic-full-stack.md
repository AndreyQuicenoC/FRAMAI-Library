@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implement the following User Stories (batch / epic) using the user-story-implementation multi-agent workflow.

## Execution instructions
1. **Orchestrator** analyzes the project, determines `project_mode` per repository, orders the user stories by dependencies, generates ONE epic contract, `execution_plan`, `epic_plan`, and `commit_plan`
2. According to the plan, run ONLY the necessary agents:
   - Backend: @.ai-vexarq/workflows/user-story-implementation/backend.xml
   - Frontend: @.ai-vexarq/workflows/user-story-implementation/frontend.xml
   - Integrator: @.ai-vexarq/workflows/user-story-implementation/integrator.xml
3. Standards and flow (load the sections the Orchestrator indicates):
   - @.ai-vexarq/core/engineering-standards.xml
   - @.ai-vexarq/core/git-workflow.xml
   - @.ai-vexarq/core/project-modes.xml
4. Each agent updates docs/ (user-stories.md, code-structure.md, implementation-report.md, integration-report.md)
5. Implement real code and run ALL the quality gates; do not declare COMPLETED without evidence
6. Do not stop at analysis alone; produce verifiable changes

---

## User Stories

[Paste each user story with its ID, role, functionality, acceptance criteria, DoR/DoD, and technical tasks, without summarizing]

---

## Project context

**Paths:** [backend: path] [frontend: path]

**Untouchable directories:** [e.g. demo/ — do not modify]

**Additional constraints:** None

**Requested scope:** auto (let the Orchestrator classify)

---

## Git flow (see git-workflow.xml)

- Verify or create `develop` from `main` in each repository; create the epic branch `feature/ep-XX-<name>` from `develop`
- Work only on the epic branch; do NOT merge or modify `main` or `develop` after preparing them
- One commit per successfully completed user story (plus a foundation commit and cross-cutting commits separately)
- Push of the epic branch only when everything works and the integration is validated

## Environment and operation

- Local database: [engine and mode, e.g. PostgreSQL in a container]
- Email / password recovery: [leave variables prepared with known links and examples of the rest]
- Local `.env` files ready and `.env.example` with the same structure in each repository
- Containerization of each service and a functional compose
- Test and integration scripts in `scripts/` of each repository

## Security (mandatory)

- No secret, credential, or sensitive data in code, documentation, logs, or commits
- Secret scanning before each commit; real `.env` out of version control

## Expected output

- Documentation updated in `docs/user-stories.md`, `docs/code-structure.md`, `docs/implementation-report.md`, `docs/integration-report.md`
- Code implemented and working in backend and frontend
- Tests executed and passing; real integration validated
- Traceability user story → code → tests → evidence
