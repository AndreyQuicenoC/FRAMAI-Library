# Changelog

Todas las modificaciones notables a este ecosistema de prompts multiagente serán documentadas en este archivo.

El formato está basado en [Keep a Changelog](https://keepachangelog.com/es-ES/1.0.0/),
y este proyecto adhiere a [Versionado Semántico](https://semver.org/lang/es/).

---

## [2.1.1] - 2026-09-14

### 🏗️ Restructured (Reestructurado)
- **Conversión a XML estructurado:** Todos los prompts principales (`.ai/core/`, `.ai/workflows/`, `.ai/guides/`, `.ai/standards/`) convertidos de `.md` a `.xml` con formato jerárquico usando etiquetas `<prompt>`, mejorando parseo y claridad estructural.
  - `policies.md` → `policies.xml`
  - `adaptive-policies.md` → `adaptive-policies.xml`
  - `orchestrator.md` → `orchestrator.xml`
  - `backend.md` → `backend.xml`
  - `frontend.md` → `frontend.xml`
  - `integrator.md` → `integrator.xml`
  - `quality-attributes.md` → `quality-attributes.xml`
  - `user-story-implementation.md` → `user-story-implementation.xml`
- **Templates actualizados:** Referencias en templates (`full-stack.md`, `backend-only.md`, `frontend-only.md`, `integration-fix.md`) actualizadas para referenciar archivos `.xml`
- **Backups automáticos:** Archivos `.md.bak` creados durante conversión como respaldo

### 📦 Added (Agregado)
- **`EXPOSICION.md`:** Documento conciso (no extenso) para exposiciones académicas con:
  - Arquitectura del sistema (diagrama textual)
  - Flujos típicos por complejidad (TRIVIAL→SIMPLE→MODERATE→COMPLEX)
  - Niveles de complejidad con ejemplos concretos
  - Guía de uso (instalación, uso básico, proyectos con microservicios)
  - Estimaciones de consumo por escenario
  - Instrucciones para afinar el sistema (aumentar/reducir consumo)
  - Ventajas y desventajas objetivas
  - Aplicabilidad (ideal para / no ideal para)
  - Estructura del repositorio
- **Sección de portabilidad en README:** Nueva subsección "🚢 Portabilidad: Uso en Diferentes Estructuras de Proyecto" con:
  - Escenario 1: Proyecto con directorio único
  - Escenario 2: Monorepo con múltiples microservicios
  - Escenario 3: Microservicios independientes
  - Uso de `project_path` y `additional_constraints`
  - Claridad sobre no requerir configuración adicional
- **`docs/updates/`:** Nuevo subdirectorio para documentos de actualización
- **`docs/updates/v2.1.0-adaptive-strategy.md`:** `IMPLEMENTATION-SUMMARY.md` renombrado y movido desde raíz

### 🔄 Changed (Cambiado)
- **README.md:** 
  - Versión actualizada a `2.1.1`
  - Cabecera con nota de portabilidad destacada
  - Estructura de archivos actualizada con extensiones `.xml`
  - Referencias de `.md` a `.xml` en todas las secciones
- **Raíz limpia:** Solo `README.md`, `CHANGELOG.md` y `EXPOSICION.md` en raíz (documentos operativos eliminados)

### 🗑️ Removed (Eliminado)
- **Archivos `.md` originales:** Eliminados tras conversión a `.xml` (respaldados como `.md.bak`)
- **`IMPLEMENTATION-SUMMARY.md` de raíz:** Movido a `docs/updates/v2.1.0-adaptive-strategy.md`

### 📚 Documentation (Documentación)
- **Claridad de formato XML:** Mejor organización jerárquica con etiquetas XML (`<metadata>`, `<flujo>`, `<output>`, etc.)
- **Portabilidad documentada:** Instrucciones claras para usar `.ai/` en cualquier estructura de proyecto
- **Exposición académica:** Documento completo pero conciso (`EXPOSICION.md`) con toda la información relevante para presentaciones

### 🎯 Portability (Portabilidad)
- **`.ai/` completamente autónomo:** Confirmado y documentado que solo copiando `.ai/` el ecosistema funciona
- **Sin dependencias externas:** No requiere `docs/`, `tests/` ni configuración adicional
- **Flexible:** Funciona en proyecto único, monorepo o microservicios independientes

### ⚡ Performance (Rendimiento)
- **Sin cambios en consumo:** Las optimizaciones de v2.1.0 se mantienen; la conversión a XML es puramente estructural

---

## [2.1.0] - 2026-09-14

### 🚀 Added (Agregado)
- **Estrategia adaptativa:** Sistema de ejecución proporcional a complejidad de HU
  - Clasificación de complejidad: TRIVIAL, SIMPLE, MODERATE, COMPLEX
  - Carga selectiva de políticas según necesidad
  - Routing condicional de agentes
  - Optimización de contexto (no enviar todo a todos)
- **`.ai/core/adaptive-policies.md`:** Documento completo de estrategia adaptativa con:
  - Criterios de clasificación de complejidad
  - Matriz de decisión por nivel
  - Carga selectiva de políticas
  - Optimización de contexto
  - Métricas de éxito
  - Balance calidad/consumo

### 🔄 Changed (Cambiado)
- **Orchestrator (`orchestrator.md`):**
  - Nuevo paso 5: Clasificación de complejidad (antes de routing)
  - Output adaptativo según complejidad
  - Handoffs optimizados (no repetir contexto)
  - Contrato compartido condicional (según complejidad + routing)
  - Matriz de responsabilidades solo si MODERATE/COMPLEX
- **`.cursor/rules/hu-framework.mdc`:** Actualizado para reflejar estrategia adaptativa y 4 documentos

### ⚡ Performance (Rendimiento)
- **Optimización de tokens:**
  - TRIVIAL: ~5-10K tokens (vs ~40K en v2.0.0)
  - SIMPLE: ~15-30K tokens (vs ~50K en v2.0.0)
  - MODERATE: ~40-70K tokens (igual, justificado)
  - COMPLEX: ~80-150K tokens (igual, necesario)
- **Reducción de latencia:** Menos agentes ejecutados para HUs simples
- **Carga selectiva:** Solo políticas aplicables a cada HU

### 📚 Documentation (Documentación)
- **Balance calidad/consumo:** Análisis detallado en `adaptive-policies.md`
- **Fórmula de decisión:** Cálculo de complejidad basado en factores cuantificables

---

## [2.0.0] - 2026-09-14

### 🚀 Added (Agregado)
- **Documentación incremental de 4 archivos:** Sistema robusto de documentación con `user-stories.md`, `code-structure.md`, `implementation-report.md`, e `integration-report.md`.
- **Prompt Backend ampliado:** Secciones detalladas sobre:
  - Transacciones y concurrencia
  - Error handling exhaustivo con códigos HTTP específicos
  - Logging estructurado y observabilidad
  - Performance (índices, paginación, caché, N+1 queries)
  - External integrations (timeout, retry, circuit breaker)
  - Files and uploads (validación, storage, seguridad)
  - Dependency policy
  - Code quality (Clean Code, SOLID)
  - Backward compatibility
  - Test quality (cobertura 80%+, tipos de tests)
- **Prompt Frontend ampliado:** Secciones detalladas sobre:
  - Component architecture (composición, naming, jerarquía)
  - Modern frontend practices (hooks, TypeScript, linting)
  - UX/Usability (5 estados obligatorios, feedback inmediato)
  - Notifications (tipos y uso apropiado)
  - Forms (validación cliente, errores específicos)
  - State management (local, context, global)
  - Routing (rutas claras, protected routes, 404)
  - API error handling (mapeo HTTP codes a UI)
  - Accessibility detallada (teclado, screen readers, contraste, WCAG 2.1 AA)
  - Responsive design (breakpoints, mobile-first, touch targets)
  - SEO (meta tags, semantic HTML, Core Web Vitals)
  - Performance (lazy loading, memoización, virtualización, bundle size)
  - Security (no secretos, sanitización, CSP)
- **Prompt Integrator ampliado:** Persistencia de `integration-report.md` con validación exhaustiva E2E.
- **`docs/guides/` directory:** Subdirectorio para documentación de referencia.
- **`docs/integration-report.md`:** Cuarto documento obligatorio para validación E2E completa.
- **CHANGELOG.md:** Control de versiones del ecosistema.

### 🔄 Changed (Cambiado)
- **`core/policies.md`:** Actualizado para reflejar 4 documentos incrementales en lugar de 3.
- **Estructura de `docs/`:** 
  - `quality-standards.md` → `docs/guides/quality-standards.md`
  - `architecture.md` → `docs/guides/architecture.md`
  - Mantiene en raíz solo los 4 documentos incrementales principales.
- **Backend persistence:** Actualizado para documentar en `code-structure.md` e `implementation-report.md`.
- **Frontend persistence:** Actualizado para documentar en `code-structure.md` e `implementation-report.md`.
- **Integrator persistence:** Actualizado para documentar en `user-stories.md`, `implementation-report.md` e `integration-report.md`.

### 🗑️ Removed (Eliminado)
- **`docs/ai-strategy/`:** Directorio completo eliminado (incluye `/archive/` con prompts originales verbose).
  - **Razón:** Los prompts originales cumplieron su propósito académico; el sistema actual (`.ai/`) es la referencia operacional.
  - **Impacto:** Reduce confusión entre sistema viejo y nuevo; favorece adopción del framework modular.

### 📚 Documentation (Documentación)
- **README.md (raíz):** Único archivo de documentación en raíz; completamente actualizado para reflejar el sistema de 4 documentos incrementales.
- **`docs/reports/README.md`:** Convenciones para reportes numerados futuros.

### 🔐 Security (Seguridad)
- Estándares de seguridad (OWASP Top 10) integrados explícitamente en prompts Backend, Frontend e Integrator.
- Validación de entrada exhaustiva, manejo de secretos, y AuthN/AuthZ documentados en detalle.

### ♿ Accessibility (Accesibilidad)
- Guías WCAG 2.1 AA integradas en Frontend con checklists detallados (keyboard navigation, screen readers, contraste, focus visible).

### ⚡ Performance (Rendimiento)
- Optimización de prompts para reducir tokens (compactación vs prompts originales de 110 páginas).
- Guías de performance para Backend (queries, índices, paginación) y Frontend (lazy loading, memoización, bundle size).

---

## [1.0.0] - 2026-09-13

### 🎉 Initial Release
- **Framework multiagente modular:** Orquestador → Backend → Frontend → Integrador.
- **Context engineering:** Carga dinámica de políticas, estándares y contratos según etapa.
- **Prompt chaining:** Ejecución secuencial de especialistas con handoffs explícitos.
- **Dynamic routing:** Clasificación de HUs en `FRONTEND_ONLY`, `BACKEND_ONLY`, `FULL_STACK`, `INTEGRATION_FIX`, `SPECIALIST_FIX`.
- **Shared contract:** Contrato centralizado de API, datos, estado, errores y seguridad.
- **Two-round validation (V1/V2):** Validación iterativa por especialistas.
- **Quality attributes:** Estándares ISO/IEC 25010, WCAG 2.1, OWASP Top 10 integrados.
- **Conventional Commits:** Trazabilidad de cambios.
- **Directorio `.ai/`:** 
  - `core/policies.md` (políticas globales)
  - `workflows/user-story-implementation/` (orchestrator, backend, frontend, integrator)
  - `standards/quality-attributes.md` (ISO 25010, WCAG, OWASP)
  - `contracts/shared-contract-template.md`
  - `templates/` (plantillas full-stack, frontend-only, backend-only, integration-fix, specialist-fix)
  - `guides/user-story-implementation.md`
- **Directorio `docs/`:** 
  - `dev-history.md` (concepto inicial de documentación incremental, reemplazado en 2.0.0)
  - `quality-standards.md`
  - `architecture.md`
- **Directorio `tests/ai-strategy/`:** Ejemplos de entrada para la IA.

---

## Guía de versionado

### Major (X.0.0)
- Cambios incompatibles en estructura de prompts o flujo de agentes.
- Eliminación de documentos/directorios críticos.
- Cambios en formato de entrada/salida de agentes.

### Minor (x.Y.0)
- Nuevas funcionalidades o secciones en prompts.
- Nuevos agentes o workflows.
- Mejoras significativas en profundidad técnica.
- Nuevos estándares integrados.

### Patch (x.y.Z)
- Correcciones de errores.
- Mejoras de claridad en prompts existentes.
- Actualizaciones de documentación sin cambios funcionales.
- Typos y formato.

---

## Notas de migración

### De 1.0.0 a 2.0.0

**Acción requerida:**
1. **Documentación incremental:**
   - Si usabas `docs/dev-history.md`, migra contenido a:
     - `docs/user-stories.md` (HUs exactas)
     - `docs/code-structure.md` (estructuras código)
     - `docs/implementation-report.md` (resultados implementación)
     - `docs/integration-report.md` (validación E2E)
   
2. **Prompts originales:**
   - Si referenciabas `docs/ai-strategy/archive/`, actualiza referencias o mantén copia local si necesario.
   - Sistema actual (`.ai/`) es la referencia operacional.

3. **Guías y plantillas:**
   - Revisa `.ai/guides/user-story-implementation.md` para flujo actualizado.
   - Usa plantillas en `.ai/templates/` con 4 documentos incrementales.

**Beneficios:**
- ✅ Prompts 5-10x más detallados que versión 1.0.0
- ✅ Validación E2E exhaustiva con `integration-report.md`
- ✅ Estándares integrados directamente en instrucciones de agentes
- ✅ Documentación estructurada en 4 archivos incrementales (vs 1 archivo `dev-history.md`)
- ✅ Trazabilidad completa: HU → implementación → integración

---

**Responsable:** Sistema de IA multiagente  
**Última actualización:** 2026-09-14
