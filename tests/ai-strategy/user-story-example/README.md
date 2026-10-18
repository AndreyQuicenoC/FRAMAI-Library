# Example: Implementation of HU-023 — Register sale

Complete example of running the user-story implementation workflow with a multi-agent architecture.

## Original user story

```markdown
## HU-023: Register sale

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
```

---

## Project context (example)

**Stack:**
- Backend: Node.js + Express + PostgreSQL + Prisma
- Frontend: React + Vite + React Router + Axios
- Auth: JWT with roles
- Tests: Jest (backend) + Vitest (frontend)

**Structure:**
```
project/
├── backend/
│   ├── src/
│   │   ├── api/sales/
│   │   ├── models/
│   │   ├── services/
│   │   └── middleware/auth.js
│   └── tests/
└── frontend/
    ├── src/
    │   ├── pages/Sales/
    │   ├── components/
    │   ├── hooks/
    │   └── services/api.js
    └── tests/
```

---

## Message sent to Cursor

See [input-message.md](input-message.md)

---

## Orchestrator output

See [orchestrator-output.md](orchestrator-output.md)

**Highlights:**
- **Classification:** `FULL_STACK`
- **Agents:** orchestrator → backend → frontend → integrator
- **Shared contract:** Fully defined (API, data, states, errors)
- **Critical structures:** auth middleware, existing Sale model
- **Ambiguities:** None (clear user story)

---

## Backend output

See [backend-output.md](backend-output.md)

**Files modified:**
- `backend/src/api/sales/routes.js` — POST /api/sales endpoint
- `backend/src/services/SaleService.js` — Business logic
- `backend/src/models/Sale.js` — Prisma model (already existed, no changes)
- `backend/tests/sales.test.js` — Unit tests

**Implementation:**
```javascript
// backend/src/api/sales/routes.js
router.post('/sales', authenticate, authorize(['cashier']), async (req, res) => {
  try {
    const sale = await SaleService.create(req.user.id, req.body);
    res.status(201).json({ id: sale.id, message: 'Sale created', sale });
  } catch (error) {
    if (error.name === 'ValidationError') {
      return res.status(400).json({ error: error.message, fields: error.fields });
    }
    res.status(500).json({ error: 'Internal server error' });
  }
});
```

**Tests executed:**
```
✓ POST /api/sales creates sale with valid data (201)
✓ POST /api/sales rejects invalid total (400)
✓ POST /api/sales rejects unauthorized user (401)
✓ POST /api/sales rejects non-cashier (403)
✓ SaleService validates items not empty
✓ SaleService calculates total correctly
```

**V1 validation:** 3/3 backend criteria (API, validation, authZ)  
**V2 validation:** ✓ 6 tests passing

---

## Frontend output

See [frontend-output.md](frontend-output.md)

**Files modified:**
- `frontend/src/pages/Sales/NewSale.jsx` — Registration page
- `frontend/src/components/SaleForm.jsx` — Form
- `frontend/src/hooks/useSales.js` — API hook
- `frontend/tests/SaleForm.test.jsx` — Tests

**Implementation:**
```jsx
// frontend/src/components/SaleForm.jsx
export function SaleForm() {
  const [items, setItems] = useState([]);
  const [total, setTotal] = useState(0);
  const { createSale, loading, error } = useSales();

  useEffect(() => {
    const newTotal = items.reduce((sum, item) => sum + (item.quantity * item.price), 0);
    setTotal(newTotal);
  }, [items]);

  const handleSubmit = async (e) => {
    e.preventDefault();
    const result = await createSale({ items, total });
    if (result.success) {
      toast.success('Sale created successfully');
      navigate('/sales');
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <ProductSelector items={items} onChange={setItems} />
      <div>Total: ${total.toFixed(2)}</div>
      {error && <ErrorMessage error={error} />}
      <button type="submit" disabled={loading}>
        {loading ? 'Creating...' : 'Create Sale'}
      </button>
    </form>
  );
}
```

**Tests executed:**
```
✓ renders form with product selector
✓ calculates total automatically when items change
✓ shows loading state during submission
✓ shows success message and redirects on success
✓ displays validation errors inline
```

**V1 validation:** 5/5 frontend criteria  
**V2 validation:** ✓ 5 tests passing + UX verified

---

## Integrator output

See [integrator-output.md](integrator-output.md)

**E2E flow validated:**

1. ✓ An authenticated user with the cashier role loads `/sales/new`
2. ✓ Selects products and quantities
3. ✓ The total is calculated automatically in real time
4. ✓ Click "Create Sale" → loading spinner
5. ✓ The backend receives the POST, validates, and persists → 201
6. ✓ The frontend receives the response, shows a toast, and redirects to `/sales`
7. ✓ The sale appears in the list

**Contract validation:**

| Field | Backend | Frontend | Status |
|---|---|---|---|
| Endpoint | POST /api/sales | POST /api/sales | ✓ |
| Auth | Bearer + role cashier | Bearer token | ✓ |
| Request body | { items, total } | { items, total } | ✓ |
| Response 201 | { id, message, sale } | expected | ✓ |
| Error 400 | { error, fields } | shown inline | ✓ |
| Error 401 | { error } | redirect login | ✓ |

**Acceptance criteria:**

1. ✓ **Form with selector** → FULFILLED (ProductSelector component)
2. ✓ **Automatic total** → FULFILLED (useEffect recalculates)
3. ✓ **POST persists and 201** → FULFILLED (integration test)
4. ✓ **Message and redirect** → FULFILLED (toast + navigate)
5. ✓ **Inline errors** → FULFILLED (ErrorMessage component)
6. ✓ **Cashiers only** → FULFILLED (authZ middleware)

**Acceptance Score:** 6/6 (100%)

**Tests executed:**
- Unit: 11 passing (6 backend + 5 frontend)
- Integration: 1 passing (full E2E flow)

**Evidence:**
- Code: implemented and reviewed
- Tests: all green
- Manual: flow verified in development

**Final status:** `COMPLETED`

---

## Result in docs/

See [expected-output-example.md](expected-output-example.md) for the 4 generated documents.

**Summary:**
- User story added exactly in `docs/user-stories.md`
- Structures documented in `docs/code-structure.md`
- Implementation recorded in `docs/implementation-report.md`
- Complete E2E validation in `docs/integration-report.md`
- Complete traceability: user story → code → tests → evidence
- Documentation persisted in 4 incremental files

---

## Lessons

### What worked well
✓ Automatic classification (FULL_STACK correct)  
✓ The shared contract avoided misalignment  
✓ V1/V2 detected problems early  
✓ Tests executed (not only documented)  
✓ Minimal change (no general refactor)

### Improvements identified
- Add an automated E2E test with Playwright/Cypress
- Consider performance if items > 100
- Document edge cases (products out of stock)

---

## Example files

| File | Description |
|---|---|
| `input-message.md` | Complete message sent to Cursor |
| `orchestrator-output.md` | Orchestrator analysis and contract |
| `backend-output.md` | Backend implementation and validation |
| `frontend-output.md` | Frontend implementation and validation |
| `integrator-output.md` | E2E validation and closure |
| `expected-output-example.md` | Final output in docs/ (4 documents) |
| `code-samples/` | Generated code (extracts) |

---

This example demonstrates the full flow of the user-story implementation workflow with dynamic routing, a shared contract, two-round validation, and verifiable traceability.
