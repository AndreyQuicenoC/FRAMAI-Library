# Workflow: User Story Implementation

Implementación de Historias de Usuario (una o varias) mediante arquitectura multiagente con routing dinámico.

## Descripción

Pipeline especializada que analiza una HU o una épica, determina el modo de proyecto, clasifica su alcance (frontend, backend, full-stack) y ejecuta **solo** los agentes necesarios. Prioriza cambio mínimo, trazabilidad verificable, validación con la pila real y publicación segura.

## Arquitectura

```
CORE (políticas globales + estándares + flujo Git + modos de proyecto)
    ↓
ORQUESTADOR (análisis + modo + routing + contrato de épica + plan de commits)
    ↓
    ├── BACKEND (opcional según clasificación)
    ├── FRONTEND (opcional según clasificación)
    └── INTEGRADOR (opcional según clasificación; obligatorio en full-stack)
```

## Agentes

| Agente | Archivo | Responsabilidad | Ejecuta siempre |
|---|---|---|---|
| **Orquestador** | `orchestrator.xml` | Análisis, modo de proyecto, clasificación, contrato compartido, plan de lote | Sí |
| **Backend** | `backend.xml` | Servidor: API, DB, negocio, seguridad, migraciones, entorno, contenedor | Según plan |
| **Frontend** | `frontend.xml` | UI/UX, rutas, cliente HTTP, productización de mockup, contenedor | Según plan |
| **Integrador** | `integrator.xml` | Pila real, E2E, contrato, correcciones cruzadas, puerta de publicación | Según plan |

## Módulos core que acompañan al workflow

| Archivo | Cuándo se carga |
|---|---|
| `../../core/policies.xml` | Siempre |
| `../../core/adaptive-policies.xml` | Clasificación de complejidad, factores de elevación, modo lote |
| `../../core/engineering-standards.xml` | Secciones según `policies_to_load` (seguridad, calidad, pruebas, entorno) |
| `../../core/git-workflow.xml` | Si hay ramas, commits o push |
| `../../core/project-modes.xml` | Gate inicial (EXTEND_EXISTING, ARCHETYPE_EXTENSION, MOCKUP_PRODUCTIONIZATION) |

## Clasificaciones

| Tipo HU | Pipeline | Ejemplo |
|---|---|---|
| `FRONTEND_ONLY` | O, F | Cambio de estilos, componente UI puro |
| `BACKEND_ONLY` | O, B | Endpoint, exportación, cron job |
| `FULL_STACK` | O, B, F, I | Formulario + API + persistencia |
| `INTEGRATION_FIX` | I (+ B/F si necesario) | Contrato roto entre capas |
| `SPECIALIST_FIX` | O, (B o F) | Bug acotado en una capa |

## Uso

### Mensaje básico (una HU)

```
@.ai/core/policies.xml
@.ai/workflows/user-story-implementation/orchestrator.xml

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

### Épica (varias HU, varios repositorios)

Usar `../../templates/user-story-implementation/epic-full-stack.md`. Añade rutas por capa, directorios intocables, flujo Git, entorno y seguridad.

### Plantillas disponibles

- **Full-stack:** `../../templates/user-story-implementation/full-stack.md`
- **Épica full-stack:** `../../templates/user-story-implementation/epic-full-stack.md`
- **Backend-only:** `../../templates/user-story-implementation/backend-only.md`
- **Frontend-only:** `../../templates/user-story-implementation/frontend-only.md`
- **Fix integración:** `../../templates/user-story-implementation/integration-fix.md`

## Salida

Cada agente persiste en **`docs/`** (4 archivos incrementales). En proyectos multi-repositorio, cada repositorio mantiene los suyos:

```markdown
# Historia de desarrollo

## HU-023 - Registrar venta
**Estado:** COMPLETED
**Clasificación:** FULL_STACK
**Fecha:** 2026-09-14

### Análisis (Orquestador)
[...]

### Backend
[Archivos modificados, API, tests, decisiones, matriz de trazabilidad, comandos]

### Frontend
[Componentes, rutas, estados, tests]

### Validación (Integrador)
- Criterios cumplidos: 5/5
- Tests E2E: aprobados
- Evidencia: [...]
```

## Documentos generados/actualizados

- `docs/user-stories.md` - HUs exactas recibidas
- `docs/code-structure.md` - Estructuras de código
- `docs/implementation-report.md` - Resultados por HU
- `docs/integration-report.md` - Validación E2E por HU
- `docs/architecture.md` - Arquitectura actualizada si cambió
- `docs/quality-standards.md` - Validaciones aplicadas

## Flujo detallado

1. **Orquestador** analiza HU y proyecto, determina `project_mode`, ordena el lote y emite `<execution_plan>`, contrato de épica y `commit_plan`
2. **Backend** (si aplica) implementa API, lógica, persistencia, seguridad, migraciones; ejecuta todos los gates; commit por HU
3. **Frontend** (si aplica) productiza el mockup si corresponde, implementa UI, consumo API, UX; ejecuta todos los gates; commit por HU
4. **Integrador** (si aplica) levanta la pila real, valida E2E, corrige en conjunto y autoriza o deniega la publicación

Cada etapa ejecuta **validación en 2 rondas** (V1: requisitos, V2: gates y tests).

## Métricas generadas

- Acceptance Score (% criterios cumplidos)
- Coverage (tests ejecutados vs requeridos)
- Trazabilidad (HU, código, test, evidencia)
- Quality gates (comando y resultado por puerta)
- Defect Rate (errores detectados por etapa)

## Ejemplo completo

Ver `tests/ai-strategy/user-story-example/` para ejecución real documentada.

## Guía detallada

Ver `../../guides/user-story-implementation.xml`
