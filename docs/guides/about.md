# Motor de historias de usuario

Este documento describe el motor de implementación que VEXARG conserva de FRAMAI. El ciclo de producto completo está en `.ai-vexarq/guides/usar-vexarq.md`.

# Framework Multiagente Adaptativo para Implementación de HUs con IA

## 🎯 Descripción

**FRAMAI** (Framework Multiagente Adaptativo para IA) es un sistema de prompts especializados que optimiza el balance entre **calidad** y **consumo de tokens** para implementar Historias de Usuario (HU) en proyectos de software reales.

**Problema que resuelve:**  
Las HUs varían dramáticamente en complejidad (desde cambiar un color CSS hasta implementar un sistema de pagos completo). Aplicar la misma maquinaria exhaustiva a todas consume tokens innecesariamente, mientras que simplificar demasiado sacrifica calidad en casos complejos.

**Solución:**  
Clasificación automática de complejidad (TRIVIAL→SIMPLE→MODERATE→COMPLEX) que ajusta dinámicamente qué agentes ejecutar, qué políticas cargar y cuánta validación aplicar.

---

## 🏗️ Arquitectura

```
HU + Proyecto existente
       ↓
  ORQUESTADOR (siempre)
   ├─ Clasifica complejidad (4 niveles)
   ├─ Routing técnico (FULL_STACK|BACKEND_ONLY|FRONTEND_ONLY...)
   ├─ Decide políticas a cargar
   └─ Genera contratos + handoffs
       ↓
  ESPECIALISTAS (según routing y complejidad)
   ├─ BACKEND (API, lógica, DB, seguridad, transacciones)
   ├─ FRONTEND (UI, UX, a11y, responsive, SEO)
   └─ INTEGRADOR (validación E2E, contrato, flujo completo)
       ↓
  DOCUMENTACIÓN INCREMENTAL
   ├─ docs/user-stories.md
   ├─ docs/code-structure.md
   ├─ docs/implementation-report.md
   └─ docs/integration-report.md
```

**Agentes:**
- **Orquestador:** Analiza HU, clasifica complejidad, detecta alcance técnico, genera contrato compartido, decide qué ejecutar
- **Backend:** Implementa API, lógica de negocio, DB, validaciones, transacciones, seguridad
- **Frontend:** Implementa UI/UX, estados, formularios, integración API, a11y, responsive, SEO
- **Integrador:** Valida flujo E2E, compatibilidad Backend↔Frontend, criterios de aceptación, evidencia

---

## 🔄 Flujos

### Flujo Típico (FULL_STACK | MODERATE)
1. Usuario provee HU con criterios de aceptación
2. Orquestador → clasifica MODERATE → routing FULL_STACK → contrato detallado
3. Backend → implementa API + lógica + tests → V1 + V2
4. Frontend → implementa UI + integración → V1 + V2
5. Integrador → valida E2E + contrato + criterios → documenta evidencia
6. Resultado: Código + tests + docs actualizadas incrementalmente

### Flujo Simplificado (TRIVIAL)
1. HU → Orquestador → TRIVIAL
2. Un solo especialista (ej. Frontend para cambiar texto de botón)
3. Sin contrato, sin Integrador, sin validación exhaustiva
4. Documentación mínima

### Flujo Complejo (COMPLEX)
1. HU → Orquestador → COMPLEX → contrato exhaustivo + políticas completas
2. Backend → Frontend → Integrador
3. Validaciones V1/V2 completas, tests unit + integration + E2E
4. Documentación exhaustiva en 4 documentos

---

## 🎚️ Niveles de Complejidad

| Nivel | Archivos | Lógica | Validación | Agentes | Tokens estimados |
|---|---|---|---|---|---|
| **TRIVIAL** | 1 | Cosmético | Revisión simple | 1 especialista | 5-10K |
| **SIMPLE** | 2-4 | Simple | V1 ligera + V2 | 1-2 especialistas | 15-30K |
| **MODERATE** | 5-10 | Negocio + transacciones | V1/V2 completa | 3 (+ Integrador) | 40-70K |
| **COMPLEX** | 10+ | Múltiples dependencias | Exhaustiva + E2E | 3 + Integrador | 80-150K |

**Ejemplos:**
- **TRIVIAL:** "Cambiar color de botón a azul" → 1 archivo CSS → Frontend solo
- **SIMPLE:** "Agregar campo 'teléfono' a formulario de registro" → Backend ya existe, 2-3 archivos → Backend + Frontend ligero
- **MODERATE:** "Implementar formulario de contacto con envío de email" → Formulario + validación + API + integración email → Full pipeline
- **COMPLEX:** "Sistema de pagos con Stripe (captura, webhook, confirmación, rollback)" → 10+ archivos, transacciones, integraciones externas, seguridad crítica → Pipeline exhaustiva

---

## 🚀 Cómo Usarlo

### Instalación
1. Copiar carpeta `.ai-vexarq/` en la raíz de tu proyecto (o dentro de un microservicio específico)
2. Asegurar que existe código base identificable (backend/ o frontend/ o ambos)
3. Abrir Cursor en Agent mode

### Uso Básico
1. Preparar HU con criterios de aceptación claros y verificables
2. En Cursor, abrir template según alcance:
   - `.ai-vexarq/templates/user-story-implementation/full-stack.md` (recomendado por defecto)
   - `.ai-vexarq/templates/user-story-implementation/backend-only.md`
   - `.ai-vexarq/templates/user-story-implementation/frontend-only.md`
3. Completar plantilla con tu HU
4. Ejecutar; dejar que Orquestador clasifique y rute automáticamente
5. Revisar código + tests + docs generadas

### Proyectos con Múltiples Microservicios
- Copiar `.ai-vexarq/` a cada microservicio O usar una vez con `project_path` apuntando al servicio específico
- Usar restricciones adicionales si es necesario: "Solo modificar auth-service"

---

## 📊 Estimaciones de Consumo

| Escenario | Tokens Promedio | Costo aprox. (GPT-4) |
|---|---|---|
| HU trivial semanal (x5) | 50K | ~$2-3/semana |
| HU simple (x10/mes) | 250K | ~$10-15/mes |
| HU moderate (x5/mes) | 300K | ~$15-20/mes |
| HU complex (x2/mes) | 250K | ~$12-18/mes |
| **Proyecto completo (20 HUs/mes)** | **~850K** | **~$40-60/mes** |

**Comparación vs. enfoque sin optimización:**  
Sistema sin clasificación aplicaría ~120K tokens/HU promedio → ~2.4M tokens/mes → $110-180/mes  
**Ahorro estimado: 60-70%** en proyectos reales mixtos

---

## ⚙️ Afinar el Sistema

### Para Aumentar Consumo (Más Exhaustivo)
1. **Forzar complejidad más alta:** Editar `complexity_score` manualmente si Orquestador subestima
2. **Cargar más políticas:** En handoffs, agregar explícitamente políticas (ej. "Incluir performance + SEO aunque sea SIMPLE")
3. **Validación más rigurosa:** Solicitar tests E2E incluso para SIMPLE
4. **Documentación más detallada:** Modificar templates para requerir más contexto

### Para Reducir Consumo (Más Ágil)
1. **Usar templates específicos:** `backend-only` o `frontend-only` en lugar de `full-stack` cuando aplique
2. **Criterios mínimos:** HU con 2-3 criterios claros vs. 10 ambiguos
3. **Omitir Integrador:** Para SIMPLE si no hay flujo E2E crítico (modificar execution_plan)
4. **Proyectos pequeños:** Skippear Orquestador si el alcance es obvio (usar directamente especialistas)

---

## ✅ Ventajas

1. **Balance automático calidad-consumo:** No paga por exhaustividad innecesaria
2. **Adaptabilidad:** Funciona en proyecto único o microservicios; full-stack, backend-only, frontend-only
3. **Trazabilidad completa:** HU → código → tests → evidencia; 4 documentos incrementales
4. **Calidad consistente:** Acceptance Score ≥90% en todos los niveles
5. **Especialización:** Cada agente experto en su dominio (backend, frontend, integración)
6. **Reutilización:** Prompts modulares; políticas cargadas solo cuando aplican
7. **No inventivo:** Respeta arquitectura, stack y código existente; cambio mínimo
8. **Portable:** Copiar `.ai-vexarq/` y usar; no requiere configuración compleja

---

## ⚠️ Desventajas

1. **Curva de aprendizaje:** Requiere entender clasificación de complejidad y routing
2. **Overhead inicial:** Orquestador siempre se ejecuta (aunque sea TRIVIAL)
3. **Dependencia de IA:** Clasificación puede equivocarse (mitigable con revisión manual)
4. **Sobrecarga documental:** 4 documentos generados pueden ser excesivos para proyectos ultra-pequeños
5. **Limitado a HUs bien escritas:** Criterios ambiguos → clasificación incorrecta o bloqueo
6. **No reemplaza arquitecto humano:** Decisiones críticas requieren validación humana

---

## 🎓 Aplicabilidad

**Ideal para:**
- Proyectos académicos con requisitos de trazabilidad (ej. Proyecto Integrador)
- Equipos pequeños (1-3 devs) que buscan velocidad sin sacrificar calidad
- Proyectos con HUs mixtas (triviales + complejas)
- Aprendizaje de buenas prácticas (seguridad, a11y, testing, clean code)

**No ideal para:**
- Proyectos sin código base (construir desde cero; el sistema requiere proyecto existente)
- Startups ultra-rápidas que priorizan velocidad sobre trazabilidad
- Equipos con procesos QA/testing muy consolidados (puede ser redundante)

---

## 📦 Estructura del Repositorio

```
.ai-vexarq/
├── core/
│   ├── policies.xml              # Políticas globales (heredadas por todos)
│   └── adaptive-policies.xml     # Estrategia de clasificación y carga selectiva
├── workflows/
│   └── user-story-implementation/
│       ├── orchestrator.xml      # Análisis, clasificación, contrato, routing
│       ├── backend.xml           # Implementación servidor
│       ├── frontend.xml          # Implementación UI/UX
│       └── integrator.xml        # Validación E2E
├── templates/
│   └── user-story-implementation/
│       ├── full-stack.md         # Template completo (recomendado)
│       ├── backend-only.md
│       ├── frontend-only.md
│       └── integration-fix.md
├── guides/
│   └── user-story-implementation.xml  # Guía de uso
├── standards/
│   └── quality-attributes.xml    # ISO 25010, WCAG, OWASP
└── README.md                     # Descripción del framework
```

**Para usar:** Copiar `.ai-vexarq/` completo a tu proyecto. No requiere `docs/` ni `tests/` (se crean automáticamente).

---

## 🔗 Referencias

- **Arquitectura multiagente:** Prompt chaining con contexto compartido
- **Estrategia adaptativa:** Context engineering según complejidad
- **Estándares:** ISO/IEC 25010:2011, WCAG 2.1 AA, OWASP Top 10
- **Inspiración:** Clean Code (Martin), SOLID, DDD (Evans), Testing best practices

---

## 📝 Versión

**v2.1.1** — Framework con estrategia adaptativa optimizada y formato XML estructurado  
**Última actualización:** 2026-09-14
