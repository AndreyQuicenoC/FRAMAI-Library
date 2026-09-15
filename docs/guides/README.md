# Guías de referencia

Este directorio contiene documentación de referencia para el ecosistema de prompts.

---

## Documentos disponibles

### 1. **`architecture.md`**
**Propósito:** Arquitectura del sistema de software (proyecto real).

**Contenido:**
- Stack tecnológico (backend, frontend, DB, infraestructura)
- Estructura de carpetas
- Patrones de diseño aplicados
- Convenciones del proyecto
- Estructuras críticas protegidas

**Actualización:** Manual cuando cambie arquitectura o tecnología.

---

### 2. **`quality-standards.md`**
**Propósito:** Estándares de calidad aplicados al proyecto.

**Contenido:**
- Referencia a ISO/IEC 25010, WCAG 2.1, OWASP Top 10
- Aplicación concreta en el proyecto
- Métricas objetivo
- Procesos de validación
- Evolución de estándares

**Actualización:** Manual cuando se adopten nuevos estándares o cambien métricas.

---

### 3. **`complexity-optimization-analysis.md`** ✨ v2.1.0
**Propósito:** Análisis del balance calidad/consumo en estrategia adaptativa.

**Contenido:**
- Motivación para estrategia adaptativa
- Comparativa v2.0.0 vs v2.1.0
- Análisis de valor (¿vale la pena la sofisticación?)
- Distribución típica de HUs en proyecto real
- Respuestas a preguntas clave
- Recomendaciones de uso

**Actualización:** Manual cuando cambien métricas o estrategia.

---

## Relación con otros documentos

| Documento | Relación con `guides/` |
|---|---|
| `docs/user-stories.md` | Consume arquitectura de `architecture.md` |
| `docs/code-structure.md` | Documenta estructuras mencionadas en `architecture.md` |
| `docs/implementation-report.md` | Aplica estándares de `quality-standards.md` |
| `docs/integration-report.md` | Valida según `quality-standards.md` |
| `.ai/core/adaptive-policies.md` | Justificación en `complexity-optimization-analysis.md` |

---

## Cuándo actualizar

### `architecture.md`
- ✅ Cambio de framework/lenguaje/tecnología mayor
- ✅ Cambio de patrón arquitectónico
- ✅ Identificación de nuevas estructuras críticas
- ✅ Desviación detectada entre doc y realidad

### `quality-standards.md`
- ✅ Adopción de nuevos estándares (ej. ASVS, GDPR)
- ✅ Cambio de métricas objetivo
- ✅ Nuevas validaciones CI/CD
- ✅ Definición de convenciones por equipo

### `complexity-optimization-analysis.md`
- ✅ Cambio en estrategia adaptativa
- ✅ Nuevas métricas medidas
- ✅ Ajuste de umbrales de complejidad
- ✅ Resultados de experimentos académicos

---

**Versión:** 2.1.0  
**Última actualización:** 2026-09-14
