# Salida esperada del sistema multiagente

Este ejemplo muestra cómo se actualizan los 4 documentos incrementales para **HU-023: Registrar venta**.

---

## 1. `docs/user-stories.md` (actualizada por Orquestador e Integrador)

```markdown
# Historias de Usuario

## HU-023 — Registrar venta
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

**Fecha recepción:** 2026-09-14  
**Estado:** COMPLETED

---
```

---

## 2. `docs/code-structure.md` (actualizada por Orquestador, Backend, Frontend)

```markdown
# Estructura del código

## Backend

### API Endpoints
- POST /api/sales (HU-023) — Crear venta con items y total
- GET /api/products (HU-023) — Listar productos disponibles

### Servicios
- SaleService (HU-023) — Crear venta, validar stock, actualizar inventario

### Modelos
- Sale (HU-023) — { id, userId, items: [{ productId, quantity, price }], total, createdAt }
- Product (existente, usado en HU-023)

### Middleware
- auth.js — AuthN/AuthZ (verificar rol cashier en HU-023)
- validate.js — Validación requests (esquema venta en HU-023)

## Frontend

### Páginas
- /sales/new (HU-023) — Página para registrar nueva venta

### Componentes
- SaleForm (HU-023) — Formulario principal con ProductSelector + TotalCalculator
- ProductSelector (HU-023) — Dropdown con productos + cantidad
- TotalCalculator (HU-023) — Muestra total en tiempo real

### Hooks
- useSales (HU-023) — Hook para POST /api/sales
- useProducts (HU-023) — Hook para GET /api/products

### Servicios
- apiClient (existente, usado en HU-023)

## Estructuras críticas
- backend/src/middleware/auth.js — NO modificar lógica AuthN
- frontend/src/services/apiClient.js — Cliente HTTP configurado

---
```

---

## 3. `docs/implementation-report.md` (actualizada por todos los agentes)

```markdown
# Reporte de implementación

## HU-023 — Registrar venta
**Estado:** COMPLETED  
**Fecha:** 2026-09-14

### Análisis (Orquestador)
**Clasificación:** FULL_STACK

**Alcance Backend:**
- Endpoint POST /api/sales
- Validación: items no vacío, cantidades > 0, total >= 0
- AuthZ: rol cashier
- Persistencia: Sale model (ya existe)

**Alcance Frontend:**
- Página /sales/new
- Formulario con ProductSelector
- Cálculo total automático
- Manejo errores inline
- Redirect después de éxito

**Contrato compartido:**
```json
// POST /api/sales
Request: { items: [{ productId: string, quantity: number, price: number }], total: number }
Response: 201 { id: string, message: string }
Errors: 400 (validation), 401 (unauthorized), 403 (forbidden)
```

**Estructuras críticas:**
- auth.js (middleware) — NO modificar AuthN
- apiClient (frontend) — NO cambiar configuración base

---

### Cambios Backend
**Archivos modificados:**
- src/api/sales/routes.js (endpoint POST /api/sales)
- src/services/SaleService.js (lógica negocio: crear venta + validar)
- src/models/Sale.js (modelo ya existente, sin cambios)
- tests/sales.test.js (tests unitarios + integración)

**API implementada:**
- POST /api/sales
- Request: `{ items: [{ productId, quantity, price }], total }`
- Response: `201 { id, message: "Venta creada" }`
- Errores: 400 (validación), 401 (no autenticado), 403 (rol incorrecto)

**Reglas de negocio:**
- Items no puede estar vacío
- Cantidades deben ser > 0
- Total debe ser >= 0
- Usuario debe tener rol "cashier"

**Tests:**
- ✓ Crear venta válida (happy path)
- ✓ Rechazar items vacío (400)
- ✓ Rechazar cantidad negativa (400)
- ✓ Rechazar usuario sin rol cashier (403)
- ✓ Rechazar usuario no autenticado (401)
- Coverage: 95%

**Validación V1:** 3/3 criterios backend (endpoint, validación, authZ)  
**Validación V2:** ✓ Tests passing (5/5)

---

### Cambios Frontend
**Archivos modificados:**
- src/pages/Sales/NewSale.jsx (página con SaleForm)
- src/components/SaleForm.jsx (formulario principal)
- src/components/ProductSelector.jsx (selector productos)
- src/components/TotalCalculator.jsx (total automático)
- src/hooks/useSales.js (hook API POST /api/sales)
- src/hooks/useProducts.js (hook API GET /api/products)
- tests/SaleForm.test.jsx (tests componente)
- tests/NewSale.test.jsx (tests integración)

**Componentes:**
- SaleForm: formulario con validación inline
- ProductSelector: dropdown productos + input cantidad
- TotalCalculator: muestra total calculado en tiempo real

**Rutas:**
- /sales/new → NewSale (página)

**Estados manejados:**
- loading: spinner durante POST /api/sales
- success: toast "Venta creada" + redirect /sales
- error: mensajes inline por campo (validación) o toast (error servidor)
- validation: errores en tiempo real (on blur)

**UX:**
- Cálculo total automático al agregar/cambiar productos
- Validación on blur en cantidad y productos
- Botón submit disabled durante loading
- Feedback inmediato con toast

**Tests:**
- ✓ Renderiza formulario correctamente
- ✓ Calcula total en tiempo real
- ✓ Muestra errores de validación
- ✓ Redirect al éxito
- ✓ Muestra error API (500)
- Coverage: 92%

**Validación V1:** 3/3 criterios frontend (formulario, total auto, errores inline)  
**Validación V2:** ✓ Tests passing (5/5)

---

### Validación final (Integrador)
**Estado:** COMPLETED  
**Fecha cierre:** 2026-09-14

**Flujo E2E verificado:**
1. ✓ Usuario accede /sales/new
2. ✓ Selecciona productos y cantidades
3. ✓ Total se calcula automáticamente
4. ✓ Submit → POST /api/sales → 201
5. ✓ Toast "Venta creada exitosamente"
6. ✓ Redirect /sales
7. ✓ Venta visible en listado

**Criterios de aceptación:**
1. ✓ Formulario con productos → FULFILLED (test E2E)
2. ✓ Total automático → FULFILLED (test unit)
3. ✓ POST persiste → FULFILLED (test integration)
4. ✓ Mensaje éxito + redirect → FULFILLED (test E2E)
5. ✓ Errores inline → FULFILLED (test component)
6. ✓ AuthZ cashier → FULFILLED (test integration)

**Acceptance Score:** 6/6 (100%)

**Contrato:**
- ✓ Backend implementa según contrato
- ✓ Frontend consume según contrato
- ✓ Tipos alineados
- ✓ Errores manejados correctamente

**Seguridad:**
- ✓ Validación servidor (no confía en cliente)
- ✓ AuthZ verificada (solo cashier)
- ✓ No expone datos sensibles

**Tests ejecutados:**
- Unit: 8 passing (backend) + 5 passing (frontend)
- Integration: 4 passing (API)
- E2E: 1 passing (flujo completo)

**Riesgos/pendientes:** Ninguno

---
```

---

## 4. `docs/integration-report.md` (actualizada por Integrador)

```markdown
# Reporte de integración

## HU-023 — Registrar venta
**Fecha validación:** 2026-09-14

### Contrato de integración
**Endpoints involucrados:**
- POST /api/sales

**Estado compartido:**
- Sales list cache (invalidar al crear nueva venta)

**Dependencias externas:**
- Ninguna

### Flujo End-to-End
1. **Inicio:** Usuario autenticado con rol "cashier" accede /sales/new
2. **Interacción:** Selecciona productos del dropdown, ingresa cantidades
3. **Cálculo:** Total se actualiza automáticamente en tiempo real
4. **Request:** POST /api/sales { items: [...], total: 150 }
5. **Procesamiento:** Backend valida datos, verifica AuthZ, persiste en DB
6. **Response:** 201 { id: "abc123", message: "Venta creada exitosamente" }
7. **Actualización UI:** Toast éxito + redirect /sales + venta visible en listado

### Validación funcional
| Criterio | Estado | Evidencia |
|---|---|---|
| Formulario con productos | ✓ PASS | Test E2E (captura: form-loaded.png) |
| Total automático | ✓ PASS | Test unit (TotalCalculator.test.jsx) |
| POST persiste | ✓ PASS | Test integration (sales.test.js) |
| Mensaje éxito + redirect | ✓ PASS | Test E2E (captura: success-flow.png) |
| Errores inline | ✓ PASS | Test component (SaleForm.test.jsx) |
| AuthZ cashier | ✓ PASS | Test integration (403 para rol incorrecto) |

### Validación de caminos de error
**Escenarios probados:**
- ✓ Error validación (400) → Errores inline en formulario ("Cantidad debe ser > 0")
- ✓ No autorizado (401) → Redirect /login
- ✓ Forbidden (403) → Toast "No tienes permisos para registrar ventas"
- ✓ Recurso no encontrado (404) → No aplica a este endpoint
- ✓ Error servidor (500) → Toast "Error inesperado. Intente más tarde."

**Manejo en frontend:**
- 400: Mapeo de `error.fields` a errores inline por campo
- 401: Limpia token + redirect /login
- 403: Toast error específico
- 500: Toast error genérico (no expone stack trace)

### Validación de seguridad
- ✓ **AuthN:** Endpoint POST /api/sales requiere token válido
- ✓ **AuthZ:** Solo usuarios con rol "cashier" pueden crear ventas (403 para otros roles)
- ✓ **Validación entrada:** Backend rechaza items vacío, cantidades <= 0, total negativo
- ✓ **Output encoding:** No expone datos sensibles (stack traces, IDs internos)
- ✓ **HTTPS:** Tráfico encriptado en producción

### Validación UX
- ✓ **Estados claros:** Loading spinner en botón submit, toast success/error
- ✓ **Feedback inmediato:** Validación on blur, cálculo total en tiempo real
- ✓ **Errores específicos:** "Cantidad debe ser > 0", no "Error genérico"
- ✓ **Navegación coherente:** Redirect /sales después de éxito
- ✓ **Responsive:** Funciona en mobile, tablet, desktop (testado en DevTools)

### Validación de accesibilidad (WCAG 2.1 AA)
- ✓ **Keyboard navigation:** Tab entre campos, Enter submit, Escape cierra modals
- ✓ **Screen reader:** Labels asociados (`<label htmlFor="...">`), ARIA roles en toast
- ✓ **Contraste:** 4.5:1 en textos (verificado con WebAIM)
- ✓ **Foco visible:** Outline en campos activos (no `outline: none`)
- ✓ **Formularios:** Todos los inputs tienen label visible

### Validación SEO (si aplica)
- N/A (página privada detrás de autenticación)

### Validación de performance
**Métricas medidas:**
- Backend response time: 95ms (aceptable, < 200ms)
- Frontend render time: 38ms (aceptable, < 100ms)
- Lighthouse Performance: 94/100

**Optimizaciones aplicadas:**
- Ninguna necesaria (performance dentro de umbrales)

### Consistencia de base de datos
- ✓ **Transacciones:** Venta + items insertados atómicamente
- ✓ **Constraints:** FK a products, total >= 0, cantidades > 0
- ✓ **Rollback:** Error en validación revierte toda la operación
- ✓ **Migrations:** Aplicadas correctamente (no se requirieron nuevas)

### Tests ejecutados
**Unitarios:**
- Backend: 8/8 passing (SaleService, validaciones)
- Frontend: 5/5 passing (TotalCalculator, ProductSelector)

**Integración:**
- API: 4/4 passing (POST /api/sales con diferentes casos)

**E2E:**
- Flujo completo: 1/1 passing (Cypress: create-sale.spec.js)

### Acceptance Score
**Fórmula:** (6 criterios cumplidos / 6 totales) × 100
**Resultado:** 100%
**Umbral mínimo:** 90%
**Estado:** ✓ APROBADO

### Problemas detectados
Ninguno

### Trazabilidad
**HU original:** `docs/user-stories.md#HU-023`  
**Código backend:** Commits 7a3f8b2, 9c1d4e6  
**Código frontend:** Commits 2b5e8a1, 4f7c9d3  
**Reporte implementación:** `docs/implementation-report.md#HU-023`

---
```

---

## Resumen

El sistema multiagente generó actualizaciones en **4 documentos incrementales**:

1. ✅ **`docs/user-stories.md`**: HU-023 agregada exacta, estado COMPLETED
2. ✅ **`docs/code-structure.md`**: Endpoints, servicios, modelos, páginas, componentes documentados
3. ✅ **`docs/implementation-report.md`**: Análisis + cambios backend/frontend + validación final
4. ✅ **`docs/integration-report.md`**: Validación E2E exhaustiva con evidencia completa

**Acceptance Score global:** 100% (6/6 criterios)  
**Tests:** 18/18 passing (8 unit backend + 5 unit frontend + 4 integration + 1 E2E)  
**Estado final:** COMPLETED
