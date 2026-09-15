# Ejemplo: Implementación de HU-023 — Registrar venta

Ejemplo completo de ejecución del workflow de implementación de HU con arquitectura multiagente.

## HU Original

```markdown
## HU-023: Registrar venta

**Como** cajero  
**Quiero** registrar una venta con productos y cantidades  
**Para** generar ticket y actualizar inventario

### Criterios de aceptación
1. Formulario con selector de productos, campo cantidad y precio unitario
2. Total calculado automáticamente al agregar/modificar productos
3. POST /api/sales persiste en base de datos y retorna 201 con ID de venta
4. Mensaje de éxito en pantalla y redirección a /sales
5. Errores de validación visibles inline en el formulario
6. Solo usuarios con rol "cashier" pueden registrar ventas
```

---

## Contexto del proyecto (ejemplo)

**Stack:**
- Backend: Node.js + Express + PostgreSQL + Prisma
- Frontend: React + Vite + React Router + Axios
- Auth: JWT con roles
- Tests: Jest (backend) + Vitest (frontend)

**Estructura:**
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

## Mensaje enviado a Cursor

Ver [input-message.md](input-message.md)

---

## Salida del Orquestador

Ver [orchestrator-output.md](orchestrator-output.md)

**Highlights:**
- **Clasificación:** `FULL_STACK`
- **Agentes:** orchestrator → backend → frontend → integrator
- **Contrato compartido:** Definido completamente (API, datos, estados, errores)
- **Estructuras críticas:** auth middleware, Sale model existente
- **Ambigüedades:** Ninguna (HU clara)

---

## Salida de Backend

Ver [backend-output.md](backend-output.md)

**Archivos modificados:**
- `backend/src/api/sales/routes.js` — POST /api/sales endpoint
- `backend/src/services/SaleService.js` — Lógica de negocio
- `backend/src/models/Sale.js` — Modelo Prisma (ya existía, sin cambios)
- `backend/tests/sales.test.js` — Tests unitarios

**Implementación:**
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

**Tests ejecutados:**
```
✓ POST /api/sales creates sale with valid data (201)
✓ POST /api/sales rejects invalid total (400)
✓ POST /api/sales rejects unauthorized user (401)
✓ POST /api/sales rejects non-cashier (403)
✓ SaleService validates items not empty
✓ SaleService calculates total correctly
```

**Validación V1:** 3/3 criterios backend (API, validación, authZ)  
**Validación V2:** ✓ 6 tests passing

---

## Salida de Frontend

Ver [frontend-output.md](frontend-output.md)

**Archivos modificados:**
- `frontend/src/pages/Sales/NewSale.jsx` — Página de registro
- `frontend/src/components/SaleForm.jsx` — Formulario
- `frontend/src/hooks/useSales.js` — Hook API
- `frontend/tests/SaleForm.test.jsx` — Tests

**Implementación:**
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

**Tests ejecutados:**
```
✓ renders form with product selector
✓ calculates total automatically when items change
✓ shows loading state during submission
✓ shows success message and redirects on success
✓ displays validation errors inline
```

**Validación V1:** 5/5 criterios frontend  
**Validación V2:** ✓ 5 tests passing + UX verificada

---

## Salida del Integrador

Ver [integrator-output.md](integrator-output.md)

**Flujo E2E validado:**

1. ✓ Usuario autenticado con rol cashier carga `/sales/new`
2. ✓ Selecciona productos y cantidades
3. ✓ Total se calcula automáticamente en tiempo real
4. ✓ Click "Create Sale" → loading spinner
5. ✓ Backend recibe POST, valida, persiste → 201
6. ✓ Frontend recibe respuesta, muestra toast, redirige a `/sales`
7. ✓ Venta aparece en listado

**Validación de contrato:**

| Campo | Backend | Frontend | Estado |
|---|---|---|---|
| Endpoint | POST /api/sales | POST /api/sales | ✓ |
| Auth | Bearer + role cashier | Bearer token | ✓ |
| Request body | { items, total } | { items, total } | ✓ |
| Response 201 | { id, message, sale } | esperado | ✓ |
| Error 400 | { error, fields } | mostrado inline | ✓ |
| Error 401 | { error } | redirect login | ✓ |

**Criterios de aceptación:**

1. ✓ **Formulario con selector** → FULFILLED (componente ProductSelector)
2. ✓ **Total automático** → FULFILLED (useEffect recalcula)
3. ✓ **POST persiste y 201** → FULFILLED (test integración)
4. ✓ **Mensaje y redirect** → FULFILLED (toast + navigate)
5. ✓ **Errores inline** → FULFILLED (ErrorMessage component)
6. ✓ **Solo cashiers** → FULFILLED (middleware authZ)

**Acceptance Score:** 6/6 (100%)

**Tests ejecutados:**
- Unit: 11 passing (6 backend + 5 frontend)
- Integration: 1 passing (E2E flujo completo)

**Evidencia:**
- Código: implementado y revisado
- Tests: all green
- Manual: flujo verificado en desarrollo

**Estado final:** `COMPLETED`

---

## Resultado en docs/

Ver [expected-output-example.md](expected-output-example.md) para los 4 documentos generados.

**Resumen:**
- HU agregada exacta en `docs/user-stories.md`
- Estructuras documentadas en `docs/code-structure.md`
- Implementación registrada en `docs/implementation-report.md`
- Validación E2E completa en `docs/integration-report.md`
- Trazabilidad completa: HU → código → tests → evidencia
- Documentación persistida en 4 archivos incrementales

---

## Lecciones

### Lo que funcionó bien
✓ Clasificación automática (FULL_STACK correcto)  
✓ Contrato compartido evitó desalineación  
✓ V1/V2 detectó problemas tempranos  
✓ Tests ejecutados (no solo documentados)  
✓ Cambio mínimo (no refactor general)

### Mejoras identificadas
- Agregar test E2E automatizado con Playwright/Cypress
- Considerar performance si items > 100
- Documentar casos edge (productos sin stock)

---

## Archivos de ejemplo

| Archivo | Descripción |
|---|---|
| `input-message.md` | Mensaje completo enviado a Cursor |
| `orchestrator-output.md` | Análisis y contrato del Orquestador |
| `backend-output.md` | Implementación y validación Backend |
| `frontend-output.md` | Implementación y validación Frontend |
| `integrator-output.md` | Validación E2E y cierre |
| `expected-output-example.md` | Salida final en docs/ (4 documentos) |
| `code-samples/` | Código generado (extractos) |

---

Este ejemplo demuestra el flujo completo del workflow de implementación de HU con routing dinámico, contrato compartido, validación en dos rondas y trazabilidad verificable.
