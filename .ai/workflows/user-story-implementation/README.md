# Workflow: User Story Implementation

Implementación de Historias de Usuario mediante arquitectura multiagente con routing dinámico.

## Descripción

Pipeline especializada que analiza una HU, clasifica su alcance (frontend, backend, full-stack) y ejecuta **solo** los agentes necesarios. Prioriza cambio mínimo, trazabilidad verificable y validación E2E.

## Arquitectura

```
CORE (políticas globales)
    ↓
ORQUESTADOR (análisis + routing + contrato)
    ↓
    ├── BACKEND (opcional según clasificación)
    ├── FRONTEND (opcional según clasificación)
    └── INTEGRADOR (opcional según clasificación)
```

## Agentes

| Agente | Archivo | Responsabilidad | Ejecuta siempre |
|---|---|---|---|
| **Orquestador** | `orchestrator.md` | Análisis, clasificación, contrato compartido | ✓ |
| **Backend** | `backend.md` | Implementación servidor (API, DB, negocio) | Según plan |
| **Frontend** | `frontend.md` | Implementación UI/UX | Según plan |
| **Integrador** | `integrator.md` | Validación E2E y corrección de integración | Según plan |

## Clasificaciones

| Tipo HU | Pipeline | Ejemplo |
|---|---|---|
| `FRONTEND_ONLY` | O → F | Cambio de estilos, componente UI puro |
| `BACKEND_ONLY` | O → B | Endpoint, exportación, cron job |
| `FULL_STACK` | O → B → F → I | Formulario + API + persistencia |
| `INTEGRATION_FIX` | I (+ B/F si necesario) | Contrato roto entre capas |
| `SPECIALIST_FIX` | O → (B o F) | Bug acotado en una capa |

## Uso

### Mensaje básico

```
@.ai/core/policies.md
@.ai/workflows/user-story-implementation/orchestrator.md

Implementa la siguiente Historia de Usuario según el workflow de implementación.

## HU-023: Registrar venta
Como cajero quiero registrar una venta con productos para generar ticket.

### Criterios de aceptación
1. Formulario con productos, cantidades, precios
2. Cálculo automático de total
3. POST /api/sales persiste y retorna 201
4. Mensaje éxito y redirección
5. Errores visibles en formulario

## Proyecto
.

## Alcance
auto
```

### Plantillas disponibles

- **Full-stack:** `../../templates/user-story-implementation/full-stack.md`
- **Backend-only:** `../../templates/user-story-implementation/backend-only.md`
- **Frontend-only:** `../../templates/user-story-implementation/frontend-only.md`
- **Fix integración:** `../../templates/user-story-implementation/integration-fix.md`

## Salida

Cada agente persiste en **`docs/`** (4 archivos incrementales):

```markdown
# Historia de desarrollo

## HU-023 — Registrar venta
**Estado:** COMPLETED  
**Clasificación:** FULL_STACK  
**Fecha:** 2026-09-14

### Análisis (Orquestador)
[...]

### Backend
[Archivos modificados, API, tests]

### Frontend
[Componentes, rutas, estados]

### Validación (Integrador)
- Criterios cumplidos: 5/5
- Tests E2E: ✓
- Evidencia: [...]
```

## Documentos generados/actualizados

- `docs/user-stories.md` — HUs exactas recibidas
- `docs/code-structure.md` — Estructuras de código
- `docs/implementation-report.md` — Resultados por HU
- `docs/integration-report.md` — Validación E2E por HU
- `docs/architecture.md` — Arquitectura actualizada si cambió
- `docs/quality-standards.md` — Validaciones aplicadas

## Flujo detallado

1. **Orquestador** analiza HU y proyecto → emite `<execution_plan>`
2. **Backend** (si aplica) → implementa API, lógica, persistencia
3. **Frontend** (si aplica) → implementa UI, consumo API, UX
4. **Integrador** (si aplica) → valida flujo E2E, contrato, evidencia

Cada etapa ejecuta **validación en 2 rondas** (V1: requisitos, V2: tests).

## Métricas generadas

- Acceptance Score (% criterios cumplidos)
- Coverage (tests ejecutados vs requeridos)
- Trazabilidad (HU → código → test → evidencia)
- Defect Rate (errores detectados por etapa)

## Ejemplo completo

Ver `tests/ai-strategy/user-story-example/` para ejecución real documentada.

## Guía detallada

Ver `../../guides/user-story-implementation.md`
