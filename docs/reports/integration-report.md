# Reporte de integración

Este documento registra las validaciones E2E realizadas por el Integrador para cada Historia de Usuario.

**Actualización:** Incremental (NO sobrescribir reportes anteriores)

---

## Plantilla para reporte de integración

```markdown
## HU-XXX — [Título]
**Fecha validación:** YYYY-MM-DD  
**Integrador:** [Nombre del validador o "AI Agent"]

### Contrato de integración
**Endpoints involucrados:**
- [GET/POST/PUT/DELETE /api/resource]

**Estado compartido:**
- [Lista de estado relevante en frontend]

**Dependencias externas:**
- [APIs, servicios, etc.]

### Flujo End-to-End

1. **Inicio:** [Punto de entrada usuario]
2. **Interacción frontend:** [Acciones UI]
3. **Request backend:** [Detalles de la llamada]
4. **Procesamiento backend:** [Lógica ejecutada]
5. **Response:** [Formato y contenido]
6. **Actualización UI:** [Cambios en frontend]

### Validación funcional

| Criterio de aceptación | Estado | Evidencia |
|---|---|---|
| [Criterio 1] | ✓ PASS / ✗ FAIL | [Test/captura] |
| [Criterio 2] | ✓ PASS / ✗ FAIL | [Test/captura] |

### Validación de caminos de error

**Escenarios probados:**
- [ ] Error de validación (400)
- [ ] No autorizado (401/403)
- [ ] Recurso no encontrado (404)
- [ ] Error de servidor (500)
- [ ] Timeout / red caída

**Manejo en frontend:**
- [Descripción de feedback al usuario para cada escenario]

### Validación de seguridad

- [ ] **AuthN:** Endpoints protegidos requieren autenticación
- [ ] **AuthZ:** Solo usuarios autorizados acceden
- [ ] **Validación de entrada:** Backend rechaza datos inválidos
- [ ] **Output encoding:** No se expone información sensible
- [ ] **HTTPS:** Tráfico encriptado (producción)

### Validación UX

- [ ] **Estados claros:** Loading, success, error, empty
- [ ] **Feedback inmediato:** Usuario sabe qué está pasando
- [ ] **Errores específicos:** Mensajes claros por campo/operación
- [ ] **Navegación coherente:** Flujo lógico
- [ ] **Responsive:** Funciona en mobile/tablet/desktop

### Validación de accesibilidad (WCAG 2.1 AA)

- [ ] **Keyboard navigation:** Todo operable con teclado
- [ ] **Screen reader:** Etiquetas y roles ARIA
- [ ] **Contraste:** Ratio 4.5:1 mínimo para texto
- [ ] **Foco visible:** Indica dónde está el usuario
- [ ] **Formularios:** Labels asociados a inputs

### Validación SEO (si aplica)

- [ ] **Meta tags:** title, description apropiados
- [ ] **Semantic HTML:** Uso correcto de h1-h6, nav, main, etc.
- [ ] **URLs limpias:** Rutas descriptivas
- [ ] **Performance:** Lighthouse score > 90

### Validación de performance

**Métricas medidas:**
- **Backend response time:** [X ms]
- **Frontend render time:** [Y ms]
- **Lighthouse Performance:** [Z/100]

**Optimizaciones aplicadas:**
- [Lista o "Ninguna necesaria"]

### Consistencia de base de datos

- [ ] **Transacciones:** Operaciones atómicas cuando aplica
- [ ] **Constraints:** FK, unique, not null respetados
- [ ] **Rollback:** Errores no dejan estado inconsistente
- [ ] **Migrations:** Aplicadas correctamente

### Tests ejecutados

**Unitarios:**
- Backend: X/X passing
- Frontend: Y/Y passing

**Integración:**
- API: Z/Z passing

**E2E:**
- Flujo completo: W/W passing

### Acceptance Score

**Fórmula:** (Criterios cumplidos / Criterios totales) × 100

**Resultado:** [X]%

**Umbral mínimo:** 90%

**Estado:** ✓ APROBADO / ✗ REQUIERE CORRECCIÓN

### Problemas detectados

| Problema | Severidad | Estado | Responsable |
|---|---|---|---|
| [Descripción] | CRÍTICO/ALTO/MEDIO/BAJO | ABIERTO/RESUELTO | Backend/Frontend |

### Trazabilidad

**HU original:** `docs/user-stories.md#HU-XXX`  
**Código backend:** [Commits/PRs]  
**Código frontend:** [Commits/PRs]  
**Reporte implementación:** `docs/implementation-report.md#HU-XXX`

---
```

---

_Los reportes de integración se agregarán conforme se validen las HUs._
