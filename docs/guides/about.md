# User story engine

This document describes the implementation engine that VEXARQ keeps from FRAMAI. The full product cycle is in `.ai-vexarq/guides/usar-vexarq.md`.

# Adaptive Multi-Agent Framework for Implementing User Stories with AI

## 🎯 Description

**FRAMAI** (Adaptive Multi-Agent Framework for AI) is a system of specialized prompts that optimizes the balance between **quality** and **token consumption** to implement user stories in real software projects.

**Problem it solves:**  
User stories vary dramatically in complexity (from changing a CSS color to implementing a complete payment system). Applying the same exhaustive machinery to all of them consumes tokens unnecessarily, while simplifying too much sacrifices quality in complex cases.

**Solution:**  
Automatic complexity classification (TRIVIAL→SIMPLE→MODERATE→COMPLEX) that dynamically adjusts which agents to run, which policies to load, and how much validation to apply.

---

## 🏗️ Architecture

```
HU + Existing project
       ↓
  ORCHESTRATOR (always)
   ├─ Classifies complexity (4 levels)
   ├─ Technical routing (FULL_STACK|BACKEND_ONLY|FRONTEND_ONLY...)
   ├─ Decides which policies to load
   └─ Generates contracts + handoffs
       ↓
  SPECIALISTS (according to routing and complexity)
   ├─ BACKEND (API, logic, DB, security, transactions)
   ├─ FRONTEND (UI, UX, a11y, responsive, SEO)
   └─ INTEGRATOR (E2E validation, contract, full flow)
       ↓
  INCREMENTAL DOCUMENTATION
   ├─ docs/user-stories.md
   ├─ docs/code-structure.md
   ├─ docs/implementation-report.md
   └─ docs/integration-report.md
```

**Agents:**
- **Orchestrator:** Analyzes the user story, classifies complexity, detects technical scope, generates the shared contract, and decides what to run
- **Backend:** Implements the API, business logic, DB, validations, transactions, and security
- **Frontend:** Implements UI/UX, states, forms, API integration, a11y, responsive design, and SEO
- **Integrator:** Validates the E2E flow, Backend↔Frontend compatibility, acceptance criteria, and evidence

---

## 🔄 Flows

### Typical flow (FULL_STACK | MODERATE)
1. The user provides a user story with acceptance criteria
2. Orchestrator → classifies MODERATE → FULL_STACK routing → detailed contract
3. Backend → implements API + logic + tests → V1 + V2
4. Frontend → implements UI + integration → V1 + V2
5. Integrator → validates E2E + contract + criteria → documents evidence
6. Result: Code + tests + docs updated incrementally

### Simplified flow (TRIVIAL)
1. User story → Orchestrator → TRIVIAL
2. A single specialist (e.g. Frontend to change button text)
3. No contract, no Integrator, no exhaustive validation
4. Minimal documentation

### Complex flow (COMPLEX)
1. User story → Orchestrator → COMPLEX → exhaustive contract + complete policies
2. Backend → Frontend → Integrator
3. Complete V1/V2 validations, unit + integration + E2E tests
4. Exhaustive documentation in 4 documents

---

## 🎚️ Complexity levels

| Level | Files | Logic | Validation | Agents | Estimated tokens |
|---|---|---|---|---|---|
| **TRIVIAL** | 1 | Cosmetic | Simple review | 1 specialist | 5-10K |
| **SIMPLE** | 2-4 | Simple | Light V1 + V2 | 1-2 specialists | 15-30K |
| **MODERATE** | 5-10 | Business + transactions | Complete V1/V2 | 3 (+ Integrator) | 40-70K |
| **COMPLEX** | 10+ | Multiple dependencies | Exhaustive + E2E | 3 + Integrator | 80-150K |

**Examples:**
- **TRIVIAL:** "Change the button color to blue" → 1 CSS file → Frontend only
- **SIMPLE:** "Add a 'phone' field to the registration form" → Backend already exists, 2-3 files → Backend + light Frontend
- **MODERATE:** "Implement a contact form with email sending" → Form + validation + API + email integration → Full pipeline
- **COMPLEX:** "Payment system with Stripe (capture, webhook, confirmation, rollback)" → 10+ files, transactions, external integrations, critical security → Exhaustive pipeline

---

## 🚀 How to use it

### Installation
1. Copy the `.ai-vexarq/` folder into the root of your project (or inside a specific microservice)
2. Make sure there is identifiable base code (backend/ or frontend/ or both)
3. Open Cursor in Agent mode

### Basic use
1. Prepare a user story with clear and verifiable acceptance criteria
2. In Cursor, open the template according to scope:
   - `.ai-vexarq/templates/user-story-implementation/full-stack.md` (recommended by default)
   - `.ai-vexarq/templates/user-story-implementation/backend-only.md`
   - `.ai-vexarq/templates/user-story-implementation/frontend-only.md`
3. Fill in the template with your user story
4. Run it; let the Orchestrator classify and route automatically
5. Review the generated code + tests + docs

### Projects with multiple microservices
- Copy `.ai-vexarq/` to each microservice OR use it once with `project_path` pointing to the specific service
- Use additional constraints if necessary: "Only modify auth-service"

---

## 📊 Consumption estimates

| Scenario | Average tokens | Approx. cost (GPT-4) |
|---|---|---|
| Weekly trivial user story (x5) | 50K | ~$2-3/week |
| Simple user story (x10/month) | 250K | ~$10-15/month |
| Moderate user story (x5/month) | 300K | ~$15-20/month |
| Complex user story (x2/month) | 250K | ~$12-18/month |
| **Complete project (20 user stories/month)** | **~850K** | **~$40-60/month** |

**Comparison versus an approach without optimization:**  
A system without classification would apply ~120K tokens/user story on average → ~2.4M tokens/month → $110-180/month  
**Estimated savings: 60-70%** in real mixed projects

---

## ⚙️ Tuning the system

### To increase consumption (more exhaustive)
1. **Force a higher complexity:** Edit `complexity_score` manually if the Orchestrator underestimates
2. **Load more policies:** In handoffs, explicitly add policies (e.g. "Include performance + SEO even if it is SIMPLE")
3. **More rigorous validation:** Request E2E tests even for SIMPLE
4. **More detailed documentation:** Modify templates to require more context

### To reduce consumption (more agile)
1. **Use specific templates:** `backend-only` or `frontend-only` instead of `full-stack` when they apply
2. **Minimum criteria:** A user story with 2-3 clear criteria vs. 10 ambiguous ones
3. **Omit the Integrator:** For SIMPLE if there is no critical E2E flow (modify execution_plan)
4. **Small projects:** Skip the Orchestrator if the scope is obvious (use the specialists directly)

---

## ✅ Advantages

1. **Automatic quality-consumption balance:** You do not pay for unnecessary exhaustiveness
2. **Adaptability:** Works in a single project or in microservices; full-stack, backend-only, frontend-only
3. **Complete traceability:** User story → code → tests → evidence; 4 incremental documents
4. **Consistent quality:** Acceptance Score ≥90% at every level
5. **Specialization:** Each agent is an expert in its domain (backend, frontend, integration)
6. **Reuse:** Modular prompts; policies loaded only when they apply
7. **Non-inventive:** Respects the existing architecture, stack, and code; minimal change
8. **Portable:** Copy `.ai-vexarq/` and use it; it does not require complex configuration

---

## ⚠️ Disadvantages

1. **Learning curve:** Requires understanding complexity classification and routing
2. **Initial overhead:** The Orchestrator always runs (even for TRIVIAL)
3. **AI dependency:** Classification can be wrong (mitigated by manual review)
4. **Documentation overhead:** 4 generated documents can be excessive for ultra-small projects
5. **Limited to well-written user stories:** Ambiguous criteria → incorrect classification or a block
6. **Does not replace a human architect:** Critical decisions require human validation

---

## 🎓 Applicability

**Ideal for:**
- Academic projects with traceability requirements (e.g. Integrador Project)
- Small teams (1-3 devs) that want speed without sacrificing quality
- Projects with mixed user stories (trivial + complex)
- Learning good practices (security, a11y, testing, clean code)

**Not ideal for:**
- Projects without a code base (building from scratch; the system requires an existing project)
- Ultra-fast startups that prioritize speed over traceability
- Teams with very established QA/testing processes (it can be redundant)

---

## 📦 Repository structure

```
.ai-vexarq/
├── core/
│   ├── policies.xml              # Global policies (inherited by everyone)
│   └── adaptive-policies.xml     # Classification strategy and selective loading
├── workflows/
│   └── user-story-implementation/
│       ├── orchestrator.xml      # Analysis, classification, contract, routing
│       ├── backend.xml           # Server implementation
│       ├── frontend.xml          # UI/UX implementation
│       └── integrator.xml        # E2E validation
├── templates/
│   └── user-story-implementation/
│       ├── full-stack.md         # Complete template (recommended)
│       ├── backend-only.md
│       ├── frontend-only.md
│       └── integration-fix.md
├── guides/
│   └── user-story-implementation.xml  # Usage guide
├── standards/
│   └── quality-attributes.xml    # ISO 25010, WCAG, OWASP
└── README.md                     # Framework description
```

**To use:** Copy the complete `.ai-vexarq/` folder into your project. It does not require `docs/` or `tests/` (they are created automatically).

---

## 🔗 References

- **Multi-agent architecture:** Prompt chaining with shared context
- **Adaptive strategy:** Context engineering according to complexity
- **Standards:** ISO/IEC 25010:2011, WCAG 2.1 AA, OWASP Top 10
- **Inspiration:** Clean Code (Martin), SOLID, DDD (Evans), Testing best practices

---

## 📝 Version

**v2.1.1** — Framework with an optimized adaptive strategy and structured XML format  
**Last updated:** 2026-09-14
