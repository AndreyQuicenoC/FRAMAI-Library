# FRAMAI-Library

[![Version](https://img.shields.io/badge/version-2.2.0-blue.svg)](CHANGELOG.md)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-production-brightgreen.svg)](CHANGELOG.md)
[![Format](https://img.shields.io/badge/prompts-XML-purple.svg)](.ai/)
[![Cursor](https://img.shields.io/badge/IDE-Cursor-000000.svg)](https://cursor.com)

**FRAMAI** (Framework Multiagente Adaptativo para IA) — Ecosistema portable de prompts para implementar Historias de Usuario con calidad verificable y consumo adaptativo de tokens.

---

## ¿Qué es este ecosistema?

Sistema profesional y modular de **prompts, workflows y guías** para desarrollo de software asistido por Inteligencia Artificial.

### Objetivo principal

Convertir **Historias de Usuario (HU)** en:
- Código funcional implementado
- Tests automatizados ejecutados
- Documentación completa generada
- Trazabilidad verificable (HU → código → tests → evidencia)

### Tecnología base

- **Modelo:** Claude Sonnet (o compatible)
- **IDE:** Cursor con Agent mode
- **Metodología:** Context engineering + Prompt chaining
- **Arquitectura:** Multiagente con routing dinámico

---

## Filosofía y principios

### Context engineering sobre prompt engineering

> **"Proveer el contexto preciso, en el momento adecuado, al agente correcto"**

No se trata de enviar todo siempre, sino de **cargar dinámicamente** lo necesario según la tarea.

### Principios fundamentales

1. **Separación de responsabilidades** — Cada agente tiene un rol específico
2. **Políticas centralizadas** — Las reglas globales se heredan, no se repiten
3. **Ejecución adaptativa** — Sofisticación proporcional a la complejidad de la HU
4. **Carga dinámica y selectiva** — Solo se carga el contexto y políticas necesarias
5. **Trazabilidad verificable** — Del requisito al código con evidencia
6. **Extensibilidad** — Fácil agregar nuevos workflows sin afectar existentes
7. **Cambio mínimo** — Modificar solo lo necesario para la HU
8. **Validación proporcional** — Desde ligera (TRIVIAL) hasta exhaustiva (COMPLEX)

### Estrategia adaptativa (v2.1.0)

**Balance calidad/consumo optimizado:**

| Complejidad | Tokens | Agentes | Validación | Uso |
|---|---|---|---|---|
| **TRIVIAL** | 5-10K | 1 | Ligera | Cambios cosméticos |
| **SIMPLE** | 15-30K | 1-2 | V1+V2 | Lógica simple, pocos archivos |
| **MODERATE** | 40-70K | 3 | V1+V2+E2E | Lógica negocio, transacciones |
| **COMPLEX** | 80-150K | 4 | Exhaustiva | Múltiples deps, crítico |

**Ahorro estimado:** 36% en tokens globalmente, manteniendo o mejorando calidad.

Ver análisis completo: `docs/guides/complexity-optimization-analysis.md`

### Capacidad full-stack de producción (v2.2.0)

Para que el resultado sea un sistema operable y no solo código, el framework añade módulos core que los agentes cargan de forma condicional:

| Módulo | Aporte |
|---|---|
| `engineering-standards.xml` | Secretos y `.env`, seguridad de autenticación y sesión, contrato de error, quality gates por capa, matriz de pruebas, base de datos, contenedores, scripts, accesibilidad |
| `git-workflow.xml` | `develop`, rama de épica, un commit por HU, escaneo de secretos, push solo con integración verde, ramas protegidas |
| `project-modes.xml` | Extensión, arquetipo y productización de mockup (inventario, limpieza de datos sintéticos, estructura profesional, política de funcionalidades fuera de alcance) |
| `adaptive-policies.xml` | Factores de elevación (auth, datos personales, mockup) y modo lote para épicas |
| Orquestador | Modo lote: orden por dependencias, contrato de épica, `commit_plan`, `epic_plan` |
| Integrador | Pila real en ejecución, E2E, scripts en `scripts/`, bucle de corrección, puerta de publicación |

Plantilla para épicas: `.ai/templates/user-story-implementation/epic-full-stack.md`.

---

## Estructura completa

```
proyecto/
|
+-- README.md
+-- CHANGELOG.md
+-- LICENSE
|
+-- .ai/                              # Ecosistema portable
|   +-- README.md
|   +-- core/
|   |   +-- policies.xml
|   |   +-- adaptive-policies.xml
|   |   +-- engineering-standards.xml
|   |   +-- git-workflow.xml
|   |   +-- project-modes.xml
|   +-- workflows/user-story-implementation/
|   |   +-- orchestrator.xml
|   |   +-- backend.xml
|   |   +-- frontend.xml
|   |   +-- integrator.xml
|   +-- templates/user-story-implementation/
|   +-- guides/user-story-implementation.xml
|   +-- standards/quality-attributes.xml
|
+-- docs/
|   +-- guides/about.md
+-- tests/
+-- .cursor/rules/hu-framework.mdc
```

---

## Cómo funciona el sistema

### Flujo general

```
1. Usuario escribe HU con criterios de aceptación
2. Copia template según alcance (full-stack, backend-only, etc.)
3. Completa HU en template
4. Pega en Cursor Agent (Cmd/Ctrl+L)
5. Sistema ejecuta workflow automático:

 ORQUESTADOR
 ↓ analiza proyecto + HU
 ↓ clasifica alcance
 ↓ genera contrato compartido
 ↓ decide qué agentes ejecutar
 ↓
 [BACKEND] (si aplica)
 ↓ implementa API, lógica, persistencia
 ↓ tests unitarios + integración
 ↓ validación V1 + V2
 ↓
 [FRONTEND] (si aplica)
 ↓ implementa UI, componentes, consumo API
 ↓ tests componente
 ↓ validación V1 + V2
 ↓
 [INTEGRADOR] (si aplica)
 ↓ valida flujo E2E completo
 ↓ verifica contrato BE ↔ FE
 ↓ tests integración/E2E
 ↓ cierra HU con evidencia
 ↓
 RESULTADO:
 - Código implementado
 - Tests pasando
 - 4 documentos actualizados
 - Trazabilidad completa
```

### Decisión de routing (crítico)

El **Orquestador** clasifica cada HU y decide qué agentes ejecutar:

| Clasificación | Pipeline | Cuándo | Ejemplo |
|---|---|---|---|
| **FRONTEND_ONLY** | O → F | Solo cambios UI/estilos | Cambiar color botón |
| **BACKEND_ONLY** | O → B | Solo servidor/API | Exportar a CSV |
| **FULL_STACK** | O → B → F → I | Requiere ambos lados | Formulario + API + DB |
| **INTEGRATION_FIX** | I (+ B/F) | Contrato roto | Error en integración BE↔FE |
| **SPECIALIST_FIX** | O → (B o F) | Bug acotado en una capa | Corrección específica |

**Beneficio:** No ejecutar agentes innecesarios = menor coste + mayor velocidad

---

## Arquitectura multiagente

### 1. ORQUESTADOR (obligatorio, siempre ejecuta)

**Archivo:** `.ai/workflows/user-story-implementation/orchestrator.xml`

**Responsabilidad:**
- Analizar proyecto real (stack, arquitectura, convenciones)
- Comprender HU y extraer requisitos
- Detectar ambigüedades
- **Clasificar complejidad** (TRIVIAL → COMPLEX) — **v2.1.0**
- **Decidir políticas a cargar** según complejidad — **v2.1.0**
- Clasificar alcance técnico (frontend, backend, full-stack)
- Generar **contrato compartido** (nivel de detalle adaptativo)
- Identificar estructuras críticas a proteger
- Decidir qué agentes ejecutar (execution_plan)
- Producir handoffs optimizados para Backend/Frontend

**NO implementa código**, solo analiza y coordina.

**Output:**
- **Clasificación de complejidad** + score + rationale — **v2.1.0**
- **Políticas a cargar** por cada agente — **v2.1.0**
- **Tokens estimados** para la ejecución — **v2.1.0**
- Análisis técnico completo
- Contrato compartido adaptativo (detalle según complejidad)
- execution_plan con routing + estrategia adaptativa
- backend_handoff optimizado (solo contexto necesario)
- frontend_handoff optimizado (solo contexto necesario)

**Persistencia:**
- Agrega HU en `docs/user-stories.md`
- Actualiza `docs/code-structure.md` con estructuras identificadas
- Inicia reporte en `docs/implementation-report.md`

---

### 2. BACKEND (opcional según clasificación)

**Archivo:** `.ai/workflows/user-story-implementation/backend.xml`

**Responsabilidad:**
- Implementar API, lógica de negocio, persistencia
- Validación de entrada (tipos, rangos, reglas de negocio)
- AuthN/AuthZ según proyecto existente
- Manejo de errores estructurado
- Tests unitarios + integración
- Validación V1 (requisitos vs HU)
- Corrección de incumplimientos
- Validación V2 (tests + V1 corregido)

**Consume:** backend_handoff del Orquestador + contrato compartido

**Produce:**
- Código backend implementado
- Tests ejecutados
- Resultado validación V1/V2
- Acceptance score backend (% criterios backend cumplidos)

**Persistencia:**
- Actualiza `docs/code-structure.md` (endpoints, servicios, modelos)
- Agrega sección Backend en `docs/implementation-report.md`

---

### 3. FRONTEND (opcional según clasificación)

**Archivo:** `.ai/workflows/user-story-implementation/frontend.xml`

**Responsabilidad:**
- Implementar UI, componentes, formularios
- Consumo de API según contrato compartido
- Estados (loading, success, error, empty)
- UX + feedback inmediato
- Accesibilidad (WCAG 2.1 AA cuando aplica)
- SEO (cuando aplica)
- Tests componente
- Validación V1 (requisitos vs HU)
- Corrección de incumplimientos
- Validación V2 (tests + V1 corregido)

**Consume:** frontend_handoff del Orquestador + contrato compartido

**Produce:**
- Código frontend implementado
- Tests ejecutados
- Resultado validación V1/V2
- Acceptance score frontend (% criterios frontend cumplidos)

**Persistencia:**
- Actualiza `docs/code-structure.md` (páginas, componentes, hooks)
- Agrega sección Frontend en `docs/implementation-report.md`

---

### 4. INTEGRADOR (opcional según clasificación)

**Archivo:** `.ai/workflows/user-story-implementation/integrator.xml`

**Responsabilidad:**
- Validar flujo E2E completo (del usuario real)
- Verificar contrato BE ↔ FE alineado
- Validar criterios de aceptación con evidencia
- Tests de integración/E2E
- Corregir problemas de integración (mínimamente)
- Cerrar HU con estado final

**NO repite** validaciones V1/V2 de Backend/Frontend. Se enfoca en **integración**.

**Consume:**
- orchestrator_analysis
- backend_result
- frontend_result
- shared_contract

**Produce:**
- Validación E2E documentada
- Acceptance score global (% HU completa)
- Estado final: COMPLETED | COMPLETED_WITH_PENDING | BLOCKED
- Evidencia completa (tests, screenshots, etc.)

**Persistencia:**
- Actualiza estado en `docs/user-stories.md`
- Agrega sección Validación en `docs/implementation-report.md`
- Agrega reporte E2E completo en `docs/integration-report.md`

---

## Documentación generada

El ecosistema mantiene **4 documentos principales** que se actualizan incrementalmente (NO se sobrescriben):

### 1. `docs/user-stories.md`

**Propósito:** Registro de todas las HUs **exactamente** como fueron recibidas.

**Reglas:**
- NO modificar, resumir ni reinterpretar
- Agregar nuevas al final (orden cronológico)
- Mantener HUs anteriores
- Actualizar estado cuando corresponda

**Formato:**
```markdown
## HU-001 — Registrar venta
**Como** cajero
**Quiero** registrar una venta
**Para** generar ticket

### Criterios de aceptación
1. Formulario con productos
2. Total calculado automáticamente
3. POST /api/sales persiste

**Fecha recepción:** 2026-09-14
**Estado:** COMPLETED
```

**Actualiza:** Orquestador (agrega), Integrador (cambia estado)

---

### 2. `docs/code-structure.md`

**Propósito:** Estructuras de código analizadas/modificadas.

**Reglas:**
- Actualizar incrementalmente (append)
- Preservar registros anteriores
- NO sobrescribir

**Formato:**
```markdown
## Backend

### API Endpoints
- POST /api/sales (HU-001) — Crear venta
- GET /api/sales/:id (HU-003) — Obtener venta

### Servicios
- SaleService (HU-001, HU-005)
- ProductService (HU-002)

## Frontend

### Páginas
- /sales/new (HU-001) — Registro de ventas

### Componentes
- SaleForm (HU-001)
```

**Actualiza:** Orquestador, Backend, Frontend

---

### 3. `docs/implementation-report.md`

**Propósito:** Resultados de implementación por HU.

**Reglas:**
- Un reporte por HU
- Actualizar incrementalmente
- Preservar reportes anteriores

**Formato:**
```markdown
## HU-001 — Registrar venta
**Estado:** COMPLETED
**Fecha:** 2026-09-14

### Análisis (Orquestador)
- Clasificación: FULL_STACK
- Alcance Backend: POST /api/sales
- Alcance Frontend: formulario

### Cambios Backend
**Archivos:** routes/sales.js, services/SaleService.js
**Tests:** 7 passing
**Validación V2:**

### Cambios Frontend
**Archivos:** pages/NewSale.jsx, components/SaleForm.jsx
**Tests:** 6 passing
**Validación V2:**

### Validación final (Integrador)
**Acceptance Score:** 100% (6/6)
**Tests E2E:** 1 passing
**Estado:** COMPLETED
```

**Actualiza:** Orquestador, Backend, Frontend, Integrador

---

### 4. `docs/integration-report.md`

**Propósito:** Validación E2E exhaustiva del flujo completo por el Integrador.

**Reglas:**
- Un reporte por HU
- Actualizar incrementalmente
- Enfoque en integración, no repetir V1/V2 de especialistas

**Formato:**
```markdown
## HU-001 — Registrar venta
**Fecha validación:** 2026-09-14

### Contrato de integración
**Endpoints:** POST /api/sales
**Estado compartido:** Sales list cache

### Flujo E2E
1. Usuario accede /sales/new
2. Completa formulario
3. POST /api/sales → 201
4. Redirect /sales + toast éxito

### Validación funcional
| Criterio | Estado | Evidencia |
|---|---|---|
| Formulario | PASS | Test E2E |
| Total auto | PASS | Test unit |

### Validación seguridad
- AuthN/AuthZ, validación entrada, no expone secretos

### Validación UX
- Loading/success/error states, errores específicos

### Validación accesibilidad
- Keyboard navigation, labels, contraste

### Acceptance Score
**Resultado:** 100% (6/6)
**Estado:** APROBADO
```

**Actualiza:** Integrador únicamente

---

### Documentos adicionales

| Archivo | Propósito | Actualización |
|---|---|---|
| `docs/guides/architecture.md` | Arquitectura del sistema | Manual cuando cambia |
| `docs/guides/quality-standards.md` | Estándares aplicados | Manual según equipo |
| `docs/reports/` | Reportes numerados adicionales | Por necesidad |

---

## Uso paso a paso

### Preparación (una sola vez)

1. Proyecto con código base (frontend y/o backend)
2. Cursor IDE instalado
3. Familiarizarse con `.ai/README.md`

### Portabilidad: Uso en Diferentes Estructuras de Proyecto

**El ecosistema `.ai/` es completamente autónomo y portable.** Solo copia la carpeta `.ai/` en tu proyecto y úsalo inmediatamente.

#### Escenario 1: Proyecto con directorio único
```
mi-app/
├── .ai/ ← Copiar aquí
├── src/
├── package.json
└── ...
```
**Uso:** En templates, usar `project_path: .` (por defecto)

#### Escenario 2: Monorepo con múltiples microservicios
```
monorepo/
├── .ai/ ← Copiar en raíz O en cada servicio
├── backend/
│ ├── auth-service/
│ ├── payment-service/
│ └── ...
├── frontend/
└── ...
```
**Uso:**
- **Opción A (global):** `.ai/` en raíz + usar `project_path: backend/auth-service` en template
- **Opción B (por servicio):** Copiar `.ai/` dentro de cada servicio independiente

#### Escenario 3: Microservicios independientes
```
proyectos/
├── auth-service/
│ ├── .ai/ ← Copia independiente
│ └── src/
├── payment-service/
│ ├── .ai/ ← Copia independiente
│ └── src/
└── ...
```
**Uso:** Cada servicio tiene su propia instancia del ecosistema

**Restricciones adicionales:**
En HUs que afecten múltiples servicios, usar `additional_constraints` en template:
```markdown
**Restricciones adicionales:** Solo modificar auth-service, no tocar payment-service
```

**No requiere:**
- Configuración adicional
- Dependencias externas
- Archivos fuera de `.ai/`
- Cambios en tu proyecto existente

### Por cada HU

#### Paso 1: Preparar la HU (2 min)

Escribir HU con criterios de aceptación **claros y verificables**:

```markdown
## HU-023: Registrar venta

**Como** cajero
**Quiero** registrar una venta con productos
**Para** generar ticket

### Criterios de aceptación
1. Formulario con selector de productos, cantidades y precios
2. Total calculado automáticamente en tiempo real
3. POST /api/sales persiste en DB y retorna 201 con ID
4. Mensaje de éxito y redirección a /sales
5. Errores de validación visibles inline
6. Solo cajeros autenticados pueden registrar ventas
```

**Características de buenos criterios:**
- Verificables (puedes escribir un test)
- Específicos (no ambiguos)
- Independientes entre sí
- Cubren happy path + casos de error

#### Paso 2: Elegir template (30 seg)

| Escenario | Template |
|---|---|
| HU requiere backend + frontend | `.ai/templates/user-story-implementation/full-stack.md` |
| Solo servidor (API, job, export) | `.ai/templates/user-story-implementation/backend-only.md` |
| Solo UI (estilos, componente visual) | `.ai/templates/user-story-implementation/frontend-only.md` |
| Corregir integración BE↔FE | `.ai/templates/user-story-implementation/integration-fix.md` |

#### Paso 3: Copiar y completar template (1 min)

Abrir template elegido, copiar todo, completar:
- ID de HU
- Descripción de HU
- Criterios de aceptación

#### Paso 4: Ejecutar en Cursor (30 seg)

1. Abrir proyecto en Cursor
2. `Cmd/Ctrl+L` → Agent mode
3. Pegar mensaje completo
4. Enter

#### Paso 5: Dejar trabajar al sistema (2-10 min)

El sistema ejecutará automáticamente la pipeline según clasificación.

**Monitoreo:**
- Ver output en Cursor chat
- El agente irá reportando progreso por etapa

#### Paso 6: Verificar resultado (2 min)

**Revisar documentos generados:**

1. `docs/user-stories.md` → HU agregada
2. `docs/code-structure.md` → Estructuras actualizadas
3. `docs/implementation-report.md` → Reporte completo con:
 - Acceptance score
 - Tests ejecutados
 - Estado final

**Revisar código:**
- Archivos modificados/creados
- Tests ejecutados y pasando

**Validar manualmente (crítico):**
- Ejecutar flujo completo como usuario
- Verificar criterios de aceptación
- Revisar casos de error

---

## Templates disponibles

### 1. Full-stack (HU completa)

**Archivo:** `.ai/templates/user-story-implementation/full-stack.md`

**Cuándo usar:** HU requiere cambios en backend + frontend + integración

**Ejemplo:** Formulario de registro que guarda en DB

**Pipeline esperada:** O → B → F → I

---

### 2. Backend-only

**Archivo:** `.ai/templates/user-story-implementation/backend-only.md`

**Cuándo usar:** HU solo afecta servidor (API, lógica, DB)

**Ejemplo:** Exportar ventas a CSV, crear endpoint

**Pipeline esperada:** O → B

---

### 3. Frontend-only

**Archivo:** `.ai/templates/user-story-implementation/frontend-only.md`

**Cuándo usar:** HU solo afecta UI (estilos, componentes visuales)

**Ejemplo:** Cambiar color de botón, agregar tooltip

**Pipeline esperada:** O → F (+ I opcional si consume API existente)

---

### 4. Integration-fix

**Archivo:** `.ai/templates/user-story-implementation/integration-fix.md`

**Cuándo usar:** Contrato roto entre backend y frontend, error de integración

**Ejemplo:** Frontend envía `quantity` pero backend espera `qty`

**Pipeline esperada:** I (+ B/F si necesario)

---

## Ejemplos

### Ejemplo completo: HU-023 Registrar venta

Ver documentación completa en: `tests/ai-strategy/user-story-example/`

**Entrada:**
```
HU-023: Como cajero quiero registrar una venta...
Stack: Node + Express + React + Postgres
```

**Resultado:**
- POST /api/sales implementado (backend)
- SaleForm component implementado (frontend)
- 13 tests passing (7 BE + 6 FE)
- 1 test E2E passing
- Acceptance score: 100% (6/6 criterios)
- 4 documentos actualizados
- Trazabilidad: HU → código → tests → evidencia

---

## Extensión del ecosistema

### Agregar nuevo workflow

1. Crear `.ai/workflows/[nombre-workflow]/`
2. Definir agentes (archivos .md)
3. Cada agente debe heredar `.ai/core/policies.xml`
4. Crear templates en `.ai/templates/[nombre-workflow]/`
5. Documentar en `.ai/guides/[nombre-workflow].md`
6. Agregar referencia en `.ai/README.md`

### Ejemplos de workflows futuros

- **code-review/** → Revisión de PR con checklist
- **refactoring/** → Refactorización segura con tests
- **bug-fix/** → Diagnóstico y corrección de bugs
- **testing/** → Generación de tests unitarios/integración
- **performance/** → Análisis y mejora de rendimiento
- **documentation/** → Generación/actualización de docs

---

## Troubleshooting

| Problema | Causa | Solución |
|---|---|---|
| `BLOCKED` — no hay proyecto | No existe código base | Verificar proyecto existe; crear si necesario |
| No clasifica bien | HU ambigua | Aclarar alcance en HU o forzar con template específico |
| Backend/Frontend desalineados | Contrato no respetado | Ejecutar Integrador; revisar contrato en implementation-report |
| No ejecuta tests | Test runner no configurado | Configurar Jest/Vitest/etc. en proyecto |
| Contexto muy largo | Chat saturado | Iniciar nuevo chat; referenciar docs/ |
| Agrega código no pedido | No respeta cambio mínimo | Recordar policies en siguiente mensaje |
| Documentos no actualizados | Agente no siguió persistencia | Revisar que agente lea docs antes de escribir |

---

## Para la tesis

### Documentos académicos

| Archivo | Contenido |
|---|---|
| `docs/ai-strategy/archive/` | Prompts originales completos (110 pág) |
| Este README | Especificación operativa completa |
| `.ai/` estructura | Diseño modular v2 |

### Presentar como

> **"Framework de orquestación multiagente con cuatro roles especializados, carga dinámica de contexto, routing adaptativo y validación en dos rondas para implementación automatizada de Historias de Usuario"**

### Métricas evaluables

- Acceptance Score (% criterios cumplidos)
- Token consumption (input/output por HU)
- Cost/Successful HU (coste por HU completada)
- Defect Rate (errores por etapa: BE, FE, integración)
- Test Coverage (tests ejecutados vs requeridos)
- Human Intervention (correcciones manuales necesarias)
- Time to Implementation (tiempo por HU)

### Comparación vs versión monolítica

| Métrica | v1 (monolítica) | v2 (modular) | Mejora |
|---|---:|---:|---:|
| Prompts operativos | 3.269 líneas | 434 líneas | **86.7%** |
| Políticas | ~1.000 (×4 repetidas) | 105 líneas | **89.5%** |
| Redundancia | Alta | Mínima | ~73% |
| Extensibilidad | Difícil | Fácil | — |
| Routing | Fijo (4 etapas) | Dinámico | — |

---

## Documentación adicional

| Documento | Dónde | Qué contiene |
|---|---|---|
| **Índice ecosistema** | `.ai/README.md` | Quick start, workflows disponibles |
| **Guía completa HU** | `.ai/guides/user-story-implementation.xml` | Paso a paso detallado |
| **Estándares** | `.ai/standards/quality-attributes.xml` | ISO 25010, WCAG, OWASP |
| **Ejemplo completo** | `tests/ai-strategy/user-story-example/` | Caso real documentado |
| **Reportes** | `docs/reports/README.md` | Convenciones para reportes |

---

## Checklist para empezar

- [ ] Leer este README completo
- [ ] Explorar `.ai/README.md`
- [ ] Revisar ejemplo en `tests/ai-strategy/user-story-example/`
- [ ] Preparar primera HU con criterios claros
- [ ] Copiar template `.ai/templates/.../full-stack.md`
- [ ] Completar y ejecutar en Cursor Agent
- [ ] Verificar 4 documentos generados
- [ ] Validar código y tests
- [ ] Revisar flujo E2E manualmente

---

**Versión:** 2.2.0 (modular, extensible, adaptativa, full-stack de producción)
**Tecnologías:** Context engineering, Prompt chaining, ISO 25010, WCAG 2.1, OWASP Top 10
**Año:** 2026

---

*Para soporte o dudas, consultar `.ai/guides/user-story-implementation.xml` o ejemplo completo en `tests/ai-strategy/user-story-example/`*
