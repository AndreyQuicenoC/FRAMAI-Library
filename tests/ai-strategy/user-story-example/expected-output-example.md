# Expected output of the multi-agent system

This example shows how the 4 incremental documents are updated for **HU-023: Register sale**.

---

## 1. `docs/user-stories.md` (updated by the Orchestrator and the Integrator)

```markdown
# User Stories

## HU-023 — Register sale
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

**Received date:** 2026-09-14  
**Status:** COMPLETED

---
```

---

## 2. `docs/code-structure.md` (updated by the Orchestrator, Backend, and Frontend)

```markdown
# Code structure

## Backend

### API Endpoints
- POST /api/sales (HU-023) — Create a sale with items and a total
- GET /api/products (HU-023) — List available products

### Services
- SaleService (HU-023) — Create a sale, validate stock, update inventory

### Models
- Sale (HU-023) — { id, userId, items: [{ productId, quantity, price }], total, createdAt }
- Product (existing, used in HU-023)

### Middleware
- auth.js — AuthN/AuthZ (verify the cashier role in HU-023)
- validate.js — Request validation (sale schema in HU-023)

## Frontend

### Pages
- /sales/new (HU-023) — Page for registering a new sale

### Components
- SaleForm (HU-023) — Main form with ProductSelector + TotalCalculator
- ProductSelector (HU-023) — Dropdown with products + quantity
- TotalCalculator (HU-023) — Shows the total in real time

### Hooks
- useSales (HU-023) — Hook for POST /api/sales
- useProducts (HU-023) — Hook for GET /api/products

### Services
- apiClient (existing, used in HU-023)

## Critical structures
- backend/src/middleware/auth.js — DO NOT modify AuthN logic
- frontend/src/services/apiClient.js — Configured HTTP client

---
```

---

## 3. `docs/implementation-report.md` (updated by all agents)

```markdown
# Implementation report

## HU-023 — Register sale
**Status:** COMPLETED  
**Date:** 2026-09-14

### Analysis (Orchestrator)
**Classification:** FULL_STACK

**Backend scope:**
- POST /api/sales endpoint
- Validation: items not empty, quantities > 0, total >= 0
- AuthZ: cashier role
- Persistence: Sale model (already exists)

**Frontend scope:**
- Page /sales/new
- Form with ProductSelector
- Automatic total calculation
- Inline error handling
- Redirect after success

**Shared contract:**
```json
// POST /api/sales
Request: { items: [{ productId: string, quantity: number, price: number }], total: number }
Response: 201 { id: string, message: string }
Errors: 400 (validation), 401 (unauthorized), 403 (forbidden)
```

**Critical structures:**
- auth.js (middleware) — DO NOT modify AuthN
- apiClient (frontend) — DO NOT change the base configuration

---

### Backend changes
**Files modified:**
- src/api/sales/routes.js (POST /api/sales endpoint)
- src/services/SaleService.js (business logic: create sale + validate)
- src/models/Sale.js (model already existed, no changes)
- tests/sales.test.js (unit + integration tests)

**API implemented:**
- POST /api/sales
- Request: `{ items: [{ productId, quantity, price }], total }`
- Response: `201 { id, message: "Sale created" }`
- Errors: 400 (validation), 401 (unauthenticated), 403 (incorrect role)

**Business rules:**
- Items cannot be empty
- Quantities must be > 0
- Total must be >= 0
- The user must have the "cashier" role

**Tests:**
- ✓ Create a valid sale (happy path)
- ✓ Reject empty items (400)
- ✓ Reject a negative quantity (400)
- ✓ Reject a user without the cashier role (403)
- ✓ Reject an unauthenticated user (401)
- Coverage: 95%

**V1 validation:** 3/3 backend criteria (endpoint, validation, authZ)  
**V2 validation:** ✓ Tests passing (5/5)

---

### Frontend changes
**Files modified:**
- src/pages/Sales/NewSale.jsx (page with SaleForm)
- src/components/SaleForm.jsx (main form)
- src/components/ProductSelector.jsx (product selector)
- src/components/TotalCalculator.jsx (automatic total)
- src/hooks/useSales.js (API hook POST /api/sales)
- src/hooks/useProducts.js (API hook GET /api/products)
- tests/SaleForm.test.jsx (component tests)
- tests/NewSale.test.jsx (integration tests)

**Components:**
- SaleForm: form with inline validation
- ProductSelector: product dropdown + quantity input
- TotalCalculator: shows the calculated total in real time

**Routes:**
- /sales/new → NewSale (page)

**States handled:**
- loading: spinner during POST /api/sales
- success: toast "Sale created" + redirect /sales
- error: inline messages per field (validation) or toast (server error)
- validation: real-time errors (on blur)

**UX:**
- Automatic total calculation when products are added/changed
- On-blur validation on quantity and products
- Submit button disabled during loading
- Immediate feedback with a toast

**Tests:**
- ✓ Renders the form correctly
- ✓ Calculates the total in real time
- ✓ Shows validation errors
- ✓ Redirects on success
- ✓ Shows an API error (500)
- Coverage: 92%

**V1 validation:** 3/3 frontend criteria (form, automatic total, inline errors)  
**V2 validation:** ✓ Tests passing (5/5)

---

### Final validation (Integrator)
**Status:** COMPLETED  
**Closing date:** 2026-09-14

**E2E flow verified:**
1. ✓ The user opens /sales/new
2. ✓ Selects products and quantities
3. ✓ The total is calculated automatically
4. ✓ Submit → POST /api/sales → 201
5. ✓ Toast "Sale created successfully"
6. ✓ Redirect /sales
7. ✓ Sale visible in the list

**Acceptance criteria:**
1. ✓ Form with products → FULFILLED (E2E test)
2. ✓ Automatic total → FULFILLED (unit test)
3. ✓ POST persists → FULFILLED (integration test)
4. ✓ Success message + redirect → FULFILLED (E2E test)
5. ✓ Inline errors → FULFILLED (component test)
6. ✓ Cashier AuthZ → FULFILLED (integration test)

**Acceptance Score:** 6/6 (100%)

**Contract:**
- ✓ Backend implements according to the contract
- ✓ Frontend consumes according to the contract
- ✓ Types aligned
- ✓ Errors handled correctly

**Security:**
- ✓ Server validation (does not trust the client)
- ✓ AuthZ verified (cashier only)
- ✓ Does not expose sensitive data

**Tests executed:**
- Unit: 8 passing (backend) + 5 passing (frontend)
- Integration: 4 passing (API)
- E2E: 1 passing (full flow)

**Risks/pending items:** None

---
```

---

## 4. `docs/integration-report.md` (updated by the Integrator)

```markdown
# Integration report

## HU-023 — Register sale
**Validation date:** 2026-09-14

### Integration contract
**Endpoints involved:**
- POST /api/sales

**Shared state:**
- Sales list cache (invalidate when a new sale is created)

**External dependencies:**
- None

### End-to-end flow
1. **Start:** An authenticated user with the "cashier" role opens /sales/new
2. **Interaction:** Selects products from the dropdown and enters quantities
3. **Calculation:** The total updates automatically in real time
4. **Request:** POST /api/sales { items: [...], total: 150 }
5. **Processing:** The backend validates the data, verifies AuthZ, and persists to the DB
6. **Response:** 201 { id: "abc123", message: "Sale created successfully" }
7. **UI update:** Success toast + redirect /sales + sale visible in the list

### Functional validation
| Criterion | Status | Evidence |
|---|---|---|
| Form with products | ✓ PASS | E2E test (screenshot: form-loaded.png) |
| Automatic total | ✓ PASS | Unit test (TotalCalculator.test.jsx) |
| POST persists | ✓ PASS | Integration test (sales.test.js) |
| Success message + redirect | ✓ PASS | E2E test (screenshot: success-flow.png) |
| Inline errors | ✓ PASS | Component test (SaleForm.test.jsx) |
| Cashier AuthZ | ✓ PASS | Integration test (403 for an incorrect role) |

### Error-path validation
**Scenarios tested:**
- ✓ Validation error (400) → Inline errors in the form ("Quantity must be > 0")
- ✓ Unauthorized (401) → Redirect /login
- ✓ Forbidden (403) → Toast "You do not have permission to register sales"
- ✓ Resource not found (404) → Does not apply to this endpoint
- ✓ Server error (500) → Toast "Unexpected error. Try again later."

**Handling in the frontend:**
- 400: Mapping of `error.fields` to inline errors per field
- 401: Clears the token + redirect /login
- 403: Specific error toast
- 500: Generic error toast (does not expose the stack trace)

### Security validation
- ✓ **AuthN:** The POST /api/sales endpoint requires a valid token
- ✓ **AuthZ:** Only users with the "cashier" role can create sales (403 for other roles)
- ✓ **Input validation:** The backend rejects empty items, quantities <= 0, and a negative total
- ✓ **Output encoding:** Does not expose sensitive data (stack traces, internal IDs)
- ✓ **HTTPS:** Encrypted traffic in production

### UX validation
- ✓ **Clear states:** Loading spinner on the submit button, success/error toast
- ✓ **Immediate feedback:** On-blur validation, real-time total calculation
- ✓ **Specific errors:** "Quantity must be > 0", not "Generic error"
- ✓ **Coherent navigation:** Redirect /sales after success
- ✓ **Responsive:** Works on mobile, tablet, and desktop (tested in DevTools)

### Accessibility validation (WCAG 2.1 AA)
- ✓ **Keyboard navigation:** Tab between fields, Enter to submit, Escape closes modals
- ✓ **Screen reader:** Associated labels (`<label htmlFor="...">`), ARIA roles on the toast
- ✓ **Contrast:** 4.5:1 on text (verified with WebAIM)
- ✓ **Visible focus:** Outline on active fields (not `outline: none`)
- ✓ **Forms:** Every input has a visible label

### SEO validation (if it applies)
- N/A (private page behind authentication)

### Performance validation
**Metrics measured:**
- Backend response time: 95ms (acceptable, < 200ms)
- Frontend render time: 38ms (acceptable, < 100ms)
- Lighthouse Performance: 94/100

**Optimizations applied:**
- None needed (performance within thresholds)

### Database consistency
- ✓ **Transactions:** Sale + items inserted atomically
- ✓ **Constraints:** FK to products, total >= 0, quantities > 0
- ✓ **Rollback:** A validation error rolls back the entire operation
- ✓ **Migrations:** Applied correctly (no new ones were required)

### Tests executed
**Unit:**
- Backend: 8/8 passing (SaleService, validations)
- Frontend: 5/5 passing (TotalCalculator, ProductSelector)

**Integration:**
- API: 4/4 passing (POST /api/sales with different cases)

**E2E:**
- Full flow: 1/1 passing (Cypress: create-sale.spec.js)

### Acceptance Score
**Formula:** (6 criteria met / 6 total) × 100
**Result:** 100%
**Minimum threshold:** 90%
**Status:** ✓ APPROVED

### Issues found
None

### Traceability
**Original user story:** `docs/user-stories.md#HU-023`  
**Backend code:** Commits 7a3f8b2, 9c1d4e6  
**Frontend code:** Commits 2b5e8a1, 4f7c9d3  
**Implementation report:** `docs/implementation-report.md#HU-023`

---
```

---

## Summary

The multi-agent system generated updates in **4 incremental documents**:

1. ✅ **`docs/user-stories.md`**: HU-023 added exactly, status COMPLETED
2. ✅ **`docs/code-structure.md`**: Endpoints, services, models, pages, and components documented
3. ✅ **`docs/implementation-report.md`**: Analysis + backend/frontend changes + final validation
4. ✅ **`docs/integration-report.md`**: Exhaustive E2E validation with complete evidence

**Global Acceptance Score:** 100% (6/6 criteria)  
**Tests:** 18/18 passing (8 unit backend + 5 unit frontend + 4 integration + 1 E2E)  
**Final status:** COMPLETED
