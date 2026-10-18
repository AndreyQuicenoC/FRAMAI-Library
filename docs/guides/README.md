# Reference guides

This directory contains reference documentation for the prompt ecosystem.

---

## Available documents

The VEXARQ cycle usage guide is in `.ai-vexarq/guides/usar-vexarq.md`.

## Documents in this directory

### 1. **`architecture.md`**
**Purpose:** Architecture of the software system (real project).

**Contents:**
- Technology stack (backend, frontend, DB, infrastructure)
- Folder structure
- Design patterns applied
- Project conventions
- Protected critical structures

**Update:** Manual when the architecture or technology changes.

---

### 2. **`quality-standards.md`**
**Purpose:** Quality standards applied to the project.

**Contents:**
- Reference to ISO/IEC 25010, WCAG 2.1, OWASP Top 10
- Concrete application in the project
- Target metrics
- Validation processes
- Evolution of standards

**Update:** Manual when new standards are adopted or metrics change.

---

### 3. **`complexity-optimization-analysis.md`** ✨ v2.1.0
**Purpose:** Analysis of the quality/consumption balance in the adaptive strategy.

**Contents:**
- Motivation for the adaptive strategy
- Comparison of v2.0.0 vs v2.1.0
- Value analysis (is the sophistication worth it?)
- Typical distribution of user stories in a real project
- Answers to key questions
- Usage recommendations

**Update:** Manual when metrics or the strategy change.

---

## Relationship with other documents

| Document | Relationship with `guides/` |
|---|---|
| `docs/user-stories.md` | Consumes the architecture from `architecture.md` |
| `docs/code-structure.md` | Documents structures mentioned in `architecture.md` |
| `docs/implementation-report.md` | Applies the standards from `quality-standards.md` |
| `docs/integration-report.md` | Validates according to `quality-standards.md` |
| `.ai-vexarq/core/adaptive-policies.xml` | Justification in `complexity-optimization-analysis.md` |

---

## When to update

### `architecture.md`
- ✅ Major framework/language/technology change
- ✅ Architectural pattern change
- ✅ Identification of new critical structures
- ✅ Deviation detected between the doc and reality

### `quality-standards.md`
- ✅ Adoption of new standards (e.g. ASVS, GDPR)
- ✅ Change of target metrics
- ✅ New CI/CD validations
- ✅ Definition of team conventions

### `complexity-optimization-analysis.md`
- ✅ Change in the adaptive strategy
- ✅ New metrics measured
- ✅ Adjustment of complexity thresholds
- ✅ Results of academic experiments

---

**Version:** 2.1.0  
**Last updated:** 2026-09-14
