# Quality standards applied to the project

This document defines the quality standards that the team and the AI workflows must follow.

## Main reference

See the full specification: **`.ai-vexarq/standards/quality-attributes.xml`** and **`.ai-vexarq/core/engineering-standards.xml`**

The prompt ecosystem uses the following as reference:
- **ISO/IEC 25010:2011** — Software product quality model
- **WCAG 2.1** — Web accessibility (target level AA)
- **OWASP Top 10** — Security
- **Clean Code / SOLID** — Design principles
- **Conventional Commits** — Traceability

## Application in this project

### Functional Suitability

✓ **Explicit acceptance criteria** in each user story  
✓ V1/V2 validation against the criteria  
✓ Verifiable evidence (tests)

### Security

✓ Input validation in the backend  
✓ AuthN/AuthZ according to roles  
✓ Parameterized queries (ORM)  
✓ Do not expose secrets or stack traces  
✓ HTTPS in production

### Usability

✓ Clear states: loading, success, error, empty  
✓ Immediate feedback  
✓ Specific errors per field  
✓ WCAG 2.1 AA accessibility when it applies  
✓ Responsive design

### Performance Efficiency

✓ Optimized queries (indexes when necessary)  
✓ Lazy loading in the frontend  
✓ Reasonable bundle size  
✓ Optimize only if the user story requires it or there is degradation

### Maintainability

✓ Minimal change per user story  
✓ Reuse of components/services  
✓ No unnecessary general refactoring  
✓ Unit tests + integration tests  
✓ Documentation in `docs/dev-history.md`

### Reliability

✓ Robust error handling  
✓ Transactions when they apply  
✓ E2E tests of the full flow  
✓ Validation of error cases

### Compatibility

✓ Do not break existing functionality  
✓ Contracts respected (Backend ↔ Frontend)  
✓ Backward compatibility in public APIs  
✓ Protection of critical structures

## Target metrics

| Metric | Target | Measurement |
|---|---|---|
| Acceptance Score | ≥ 90% | Criteria met / verifiable |
| Test Coverage | ≥ 80% | Jest/Vitest |
| Defect Rate | < 10% | Errors found after implementation / user stories |
| Security Issues | 0 critical | Manual analysis + workflows |
| Accessibility (WCAG AA) | 100% on public functionality | axe-core / manual |

## Validation processes

### User story implementation

1. **V1 (specialists):** Validation against acceptance criteria
2. **V2 (specialists):** Tests executed + correction of V1 failures
3. **Integrator:** E2E validation with evidence

### Code review (manual)

- Security
- Architecture
- Clean Code
- Sufficient tests

### CI/CD

- Linter
- Automated tests
- Successful build
- Security scan (optional)

## Evolution of standards

This document must be updated when:
1. New standards are adopted (e.g. adding OWASP ASVS)
2. Metric targets change
3. CI/CD validations are added
4. The team defines new conventions

---

**Last review:** 2026-09-14  
**Responsible:** [Define according to the project]  
**Complete technical reference:** `.ai-vexarq/standards/quality-attributes.xml` and `.ai-vexarq/core/engineering-standards.xml`
