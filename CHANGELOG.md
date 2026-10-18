# Changelog

All notable changes to this multi-agent prompt ecosystem will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [3.0.0] - 2026-10-18

VEXARQ replaces the name FRAMAI, and the portable folder changes from `.ai/` to `.ai-vexarq/`. The user-story engine from 2.2.0 is kept.

### Added

- Lifecycle with a general orchestrator and one template per phase: approach, SRS, data, backlog, extended backlog, SAD, archetype, implementation, delivery, and recommendations.
- Modes `validar` (default) and `continuo`, with preliminary lineage versus current lineage.
- Static HTML board and update contract.
- Business-model identification and a closing recommendations document.
- Canonical backlog and archetype generators incorporated without modification. The extended backlog is generalized from an external project's example.
- Usage guides and templates ready to paste into the agent.
- Standards catalog with level obligatoria, condicional, or orientativa.
- Output-language rule: generated documents, interface copy, and comments follow the language of the user's request, or the language the user names.

### Changed

- Push requires explicit authorization of the remote and the branch, in addition to green checks.
- Generation of a new archetype is reserved for its phase. The user-story workflow no longer starts it on its own.

## [2.2.0] - 2026-10-11

Full-stack capability audit. Gaps found in 2.1.1: only one user story per run, no authentication/session security criteria or secrets policy, quality gates undefined, validators and tests oriented to JavaScript, no protocol to turn a mockup or archetype into an operable system, no Git flow or secure publishing, integration without requiring the real stack, and obsolete references to `.md` and `.md.bak` files.

### Added
- `.ai/core/engineering-standards.xml`: secrets and configuration, literal messages, authentication and session security (salted hash, HttpOnly cookie, attempt limit, anti-enumeration, CSRF/CORS), error contract, quality gates, test strategy by layer, database, containers, scripts, accessibility, and mapping by stack.
- `.ai/core/git-workflow.xml`: preparation of `develop` and the epic branch, one commit per user story, pre-commit and pre-push gates, protected branches, secret scanning.
- `.ai/core/project-modes.xml`: modes `EXTEND_EXISTING`, `ARCHETYPE_EXTENSION`, `MOCKUP_PRODUCTIONIZATION`, productionization protocol, and policy for out-of-scope features.
- `.ai/templates/user-story-implementation/epic-full-stack.md`: template for user-story batches with Git, environment, and security.
- Orchestrator: batch mode (order by dependencies, epic contract, `commit_plan`, `epic_plan`), new entries (`git_instructions`, `untouchable_paths`, `environment_instructions`), `project_mode`, and `integrator_handoff`.
- Adaptive: elevation factors (authentication, personal data, new contract, migration, mockup, operable delivery) and `modo_lote`.

### Changed
- Backend, Frontend, and Integrator rewritten with a prior baseline, all gates in V2, operational deliverables (`.env.example`, container, `scripts/`), real execution protocol, and publication gate.
- `policies.xml`: new prohibitions (secrets, protected branches, untouchable paths), multi-repository documentation rules, and clarification that a missing tool is not a blocker.
- `quality-attributes.xml`: metrics for secrets, quality gates, and real integration; coverage independent of JavaScript.

### Fixed
- `orchestrator.xml`: malformed closing tag (`</descrip>`) that invalidated the XML.
- Obsolete references to `.md` and `.md.bak` in `.ai/README.md`, the workflow README, the Cursor rule, the guides, the test example, and the main README.

---

## [2.1.1] - 2026-09-14

### 🏗️ Restructured
- **Conversion to structured XML:** All main prompts (`.ai/core/`, `.ai/workflows/`, `.ai/guides/`, `.ai/standards/`) converted from `.md` to `.xml` with a hierarchical format using `<prompt>` tags, improving parsing and structural clarity.
  - `policies.md` → `policies.xml`
  - `adaptive-policies.md` → `adaptive-policies.xml`
  - `orchestrator.md` → `orchestrator.xml`
  - `backend.md` → `backend.xml`
  - `frontend.md` → `frontend.xml`
  - `integrator.md` → `integrator.xml`
  - `quality-attributes.md` → `quality-attributes.xml`
  - `user-story-implementation.md` → `user-story-implementation.xml`
- **Templates updated:** References in templates (`full-stack.md`, `backend-only.md`, `frontend-only.md`, `integration-fix.md`) updated to reference `.xml` files
- **Automatic backups:** `.md.bak` files created during conversion as a backup

### 📦 Added
- **`EXPOSICION.md`:** Concise document (not lengthy) for academic presentations with:
  - System architecture (textual diagram)
  - Typical flows by complexity (TRIVIAL→SIMPLE→MODERATE→COMPLEX)
  - Complexity levels with concrete examples
  - Usage guide (installation, basic use, projects with microservices)
  - Consumption estimates by scenario
  - Instructions for tuning the system (increase/reduce consumption)
  - Objective advantages and disadvantages
  - Applicability (ideal for / not ideal for)
  - Repository structure
- **Portability section in the README:** New subsection "🚢 Portability: Use in Different Project Structures" with:
  - Scenario 1: Project with a single directory
  - Scenario 2: Monorepo with multiple microservices
  - Scenario 3: Independent microservices
  - Use of `project_path` and `additional_constraints`
  - Clarity that no additional configuration is required
- **`docs/updates/`:** New subdirectory for update documents
- **`docs/updates/v2.1.0-adaptive-strategy.md`:** `IMPLEMENTATION-SUMMARY.md` renamed and moved from the root

### 🔄 Changed
- **README.md:** 
  - Version updated to `2.1.1`
  - Header with a highlighted portability note
  - File structure updated with `.xml` extensions
  - References from `.md` to `.xml` in every section
- **Clean root:** Only `README.md`, `CHANGELOG.md`, and `EXPOSICION.md` at the root (operational documents removed)

### 🗑️ Removed
- **Original `.md` files:** Removed after conversion to `.xml` (backed up as `.md.bak`)
- **`IMPLEMENTATION-SUMMARY.md` from the root:** Moved to `docs/updates/v2.1.0-adaptive-strategy.md`

### 📚 Documentation
- **XML format clarity:** Better hierarchical organization with XML tags (`<metadata>`, `<flujo>`, `<output>`, etc.)
- **Portability documented:** Clear instructions for using `.ai/` in any project structure
- **Academic presentation:** Complete but concise document (`EXPOSICION.md`) with all information relevant to presentations

### 🎯 Portability
- **`.ai/` fully autonomous:** Confirmed and documented that the ecosystem works by copying only `.ai/`
- **No external dependencies:** Does not require `docs/`, `tests/`, or additional configuration
- **Flexible:** Works in a single project, a monorepo, or independent microservices

### ⚡ Performance
- **No change in consumption:** The v2.1.0 optimizations are kept; the conversion to XML is purely structural

---

## [2.1.0] - 2026-09-14

### 🚀 Added
- **Adaptive strategy:** Execution system proportional to user-story complexity
  - Complexity classification: TRIVIAL, SIMPLE, MODERATE, COMPLEX
  - Selective loading of policies according to need
  - Conditional agent routing
  - Context optimization (do not send everything to everyone)
- **`.ai/core/adaptive-policies.md`:** Complete adaptive-strategy document with:
  - Complexity classification criteria
  - Decision matrix by level
  - Selective policy loading
  - Context optimization
  - Success metrics
  - Quality/consumption balance

### 🔄 Changed
- **Orchestrator (`orchestrator.md`):**
  - New step 5: Complexity classification (before routing)
  - Adaptive output according to complexity
  - Optimized handoffs (do not repeat context)
  - Conditional shared contract (according to complexity + routing)
  - Responsibility matrix only if MODERATE/COMPLEX
- **`.cursor/rules/hu-framework.mdc`:** Updated to reflect the adaptive strategy and 4 documents

### ⚡ Performance
- **Token optimization:**
  - TRIVIAL: ~5-10K tokens (vs ~40K in v2.0.0)
  - SIMPLE: ~15-30K tokens (vs ~50K in v2.0.0)
  - MODERATE: ~40-70K tokens (same, justified)
  - COMPLEX: ~80-150K tokens (same, necessary)
- **Latency reduction:** Fewer agents executed for simple user stories
- **Selective loading:** Only policies applicable to each user story

### 📚 Documentation
- **Quality/consumption balance:** Detailed analysis in `adaptive-policies.md`
- **Decision formula:** Complexity calculation based on quantifiable factors

---

## [2.0.0] - 2026-09-14

### 🚀 Added
- **Incremental documentation of 4 files:** Robust documentation system with `user-stories.md`, `code-structure.md`, `implementation-report.md`, and `integration-report.md`.
- **Expanded Backend prompt:** Detailed sections on:
  - Transactions and concurrency
  - Exhaustive error handling with specific HTTP codes
  - Structured logging and observability
  - Performance (indexes, pagination, cache, N+1 queries)
  - External integrations (timeout, retry, circuit breaker)
  - Files and uploads (validation, storage, security)
  - Dependency policy
  - Code quality (Clean Code, SOLID)
  - Backward compatibility
  - Test quality (80%+ coverage, test types)
- **Expanded Frontend prompt:** Detailed sections on:
  - Component architecture (composition, naming, hierarchy)
  - Modern frontend practices (hooks, TypeScript, linting)
  - UX/Usability (5 mandatory states, immediate feedback)
  - Notifications (types and appropriate use)
  - Forms (client validation, specific errors)
  - State management (local, context, global)
  - Routing (clear routes, protected routes, 404)
  - API error handling (mapping HTTP codes to the UI)
  - Detailed accessibility (keyboard, screen readers, contrast, WCAG 2.1 AA)
  - Responsive design (breakpoints, mobile-first, touch targets)
  - SEO (meta tags, semantic HTML, Core Web Vitals)
  - Performance (lazy loading, memoization, virtualization, bundle size)
  - Security (no secrets, sanitization, CSP)
- **Expanded Integrator prompt:** Persistence of `integration-report.md` with exhaustive E2E validation.
- **`docs/guides/` directory:** Subdirectory for reference documentation.
- **`docs/integration-report.md`:** Fourth mandatory document for complete E2E validation.
- **CHANGELOG.md:** Version control for the ecosystem.

### 🔄 Changed
- **`core/policies.md`:** Updated to reflect 4 incremental documents instead of 3.
- **`docs/` structure:** 
  - `quality-standards.md` → `docs/guides/quality-standards.md`
  - `architecture.md` → `docs/guides/architecture.md`
  - Keeps only the 4 main incremental documents at the root.
- **Backend persistence:** Updated to document in `code-structure.md` and `implementation-report.md`.
- **Frontend persistence:** Updated to document in `code-structure.md` and `implementation-report.md`.
- **Integrator persistence:** Updated to document in `user-stories.md`, `implementation-report.md`, and `integration-report.md`.

### 🗑️ Removed
- **`docs/ai-strategy/`:** Entire directory removed (includes `/archive/` with the original verbose prompts).
  - **Reason:** The original prompts served their academic purpose; the current system (`.ai/`) is the operational reference.
  - **Impact:** Reduces confusion between the old system and the new one; favors adoption of the modular framework.

### 📚 Documentation
- **README.md (root):** The only documentation file at the root; fully updated to reflect the system of 4 incremental documents.
- **`docs/reports/README.md`:** Conventions for future numbered reports.

### 🔐 Security
- Security standards (OWASP Top 10) explicitly integrated into the Backend, Frontend, and Integrator prompts.
- Exhaustive input validation, secrets handling, and AuthN/AuthZ documented in detail.

### ♿ Accessibility
- WCAG 2.1 AA guides integrated into Frontend with detailed checklists (keyboard navigation, screen readers, contrast, visible focus).

### ⚡ Performance
- Prompt optimization to reduce tokens (compaction versus the original 110-page prompts).
- Performance guides for Backend (queries, indexes, pagination) and Frontend (lazy loading, memoization, bundle size).

---

## [1.0.0] - 2026-09-13

### 🎉 Initial Release
- **Modular multi-agent framework:** Orchestrator → Backend → Frontend → Integrator.
- **Context engineering:** Dynamic loading of policies, standards, and contracts according to the stage.
- **Prompt chaining:** Sequential execution of specialists with explicit handoffs.
- **Dynamic routing:** Classification of user stories into `FRONTEND_ONLY`, `BACKEND_ONLY`, `FULL_STACK`, `INTEGRATION_FIX`, `SPECIALIST_FIX`.
- **Shared contract:** Centralized contract for API, data, state, errors, and security.
- **Two-round validation (V1/V2):** Iterative validation by specialists.
- **Quality attributes:** ISO/IEC 25010, WCAG 2.1, and OWASP Top 10 standards integrated.
- **Conventional Commits:** Change traceability.
- **`.ai/` directory:** 
  - `core/policies.md` (global policies)
  - `workflows/user-story-implementation/` (orchestrator, backend, frontend, integrator)
  - `standards/quality-attributes.md` (ISO 25010, WCAG, OWASP)
  - `contracts/shared-contract-template.md`
  - `templates/` (full-stack, frontend-only, backend-only, integration-fix, and specialist-fix templates)
  - `guides/user-story-implementation.md`
- **`docs/` directory:** 
  - `dev-history.md` (initial concept of incremental documentation, replaced in 2.0.0)
  - `quality-standards.md`
  - `architecture.md`
- **`tests/ai-strategy/` directory:** Input examples for the AI.

---

## Versioning guide

### Major (X.0.0)
- Incompatible changes in prompt structure or agent flow.
- Removal of critical documents/directories.
- Changes in agent input/output format.

### Minor (x.Y.0)
- New features or sections in prompts.
- New agents or workflows.
- Significant improvements in technical depth.
- New integrated standards.

### Patch (x.y.Z)
- Bug fixes.
- Clarity improvements in existing prompts.
- Documentation updates without functional changes.
- Typos and formatting.

---

## Migration notes

### From 1.0.0 to 2.0.0

**Required action:**
1. **Incremental documentation:**
   - If you used `docs/dev-history.md`, migrate the content to:
     - `docs/user-stories.md` (exact user stories)
     - `docs/code-structure.md` (code structures)
     - `docs/implementation-report.md` (implementation results)
     - `docs/integration-report.md` (E2E validation)
   
2. **Original prompts:**
   - If you referenced `docs/ai-strategy/archive/`, update the references or keep a local copy if needed.
   - The current system (`.ai/`) is the operational reference.

3. **Guides and templates:**
   - Review `.ai/guides/user-story-implementation.md` for the updated flow.
   - Use the templates in `.ai/templates/` with the 4 incremental documents.

**Benefits:**
- ✅ Prompts 5-10x more detailed than version 1.0.0
- ✅ Exhaustive E2E validation with `integration-report.md`
- ✅ Standards integrated directly into agent instructions
- ✅ Structured documentation in 4 incremental files (vs 1 `dev-history.md` file)
- ✅ Complete traceability: user story → implementation → integration

---

**Responsible:** Multi-agent AI system  
**Last updated:** 2026-09-14
