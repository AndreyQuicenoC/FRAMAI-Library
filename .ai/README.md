# Ecosistema de Prompts para Desarrollo con IA

> **Context engineering sobre prompt engineering**  
> Proveer el contexto preciso, en el momento adecuado, al agente correcto.

Plataforma profesional y modular de prompts, workflows y guías para desarrollo de software asistido por IA.

## Filosofía

- **Separación de responsabilidades** mediante workflows especializados
- **Políticas centralizadas** que todos los workflows heredan
- **Carga dinámica** según la tarea (no enviar todo siempre)
- **Trazabilidad verificable** del requisito al código
- **Extensibilidad** para agregar nuevos workflows sin afectar existentes

## 🚀 Inicio rápido (5 minutos)

1. **Copia template:** `.ai/templates/user-story-implementation/full-stack.md`
2. **Completa tu HU** (ID + descripción + criterios)
3. **Pega en Cursor Agent** (Cmd/Ctrl+L)
4. **Deja trabajar:** Orquestador → Backend → Frontend → Integrador
5. **Revisa resultado:** 4 documentos actualizados:
   - `docs/user-stories.md`
   - `docs/code-structure.md`
   - `docs/implementation-report.md`
   - `docs/integration-report.md`

Ver guía detallada: [guides/user-story-implementation.md](guides/user-story-implementation.md)

## Estructura

```
.ai/
├── README.md                    ← Estás aquí
├── core/                        ← Políticas globales (heredadas por todos)
│   └── policies.md
├── workflows/                   ← Flujos especializados por tipo de tarea
│   └── user-story-implementation/
│       ├── README.md
│       ├── orchestrator.md
│       ├── backend.md
│       ├── frontend.md
│       └── integrator.md
├── templates/                   ← Plantillas de mensajes reutilizables
│   └── user-story-implementation/
│       ├── full-stack.md
│       ├── backend-only.md
│       ├── frontend-only.md
│       └── integration-fix.md
├── guides/                      ← Guías de uso paso a paso
│   └── user-story-implementation.md
└── standards/                   ← Estándares de calidad y referencias
    └── quality-attributes.md
```

## Workflows disponibles

| Workflow | Descripción | Cuándo usar |
|---|---|---|
| **[user-story-implementation](workflows/user-story-implementation/)** | Implementación de Historias de Usuario (HU) con arquitectura multiagente | Desarrollar nueva funcionalidad a partir de requisitos |

## Agregar nuevo workflow

1. Crear carpeta en `workflows/[nombre-workflow]/`
2. Incluir `README.md` con descripción y uso
3. Definir agentes/etapas necesarios
4. Cada agente hereda `core/policies.md`
5. Crear templates en `templates/[nombre-workflow]/`
6. Documentar en `guides/[nombre-workflow].md`

Ejemplos de workflows futuros:
- `code-review/` → Revisión de PR con checklist de calidad
- `refactoring/` → Refactorización segura con tests
- `bug-fix/` → Diagnóstico y corrección de bugs
- `documentation/` → Generación/actualización de docs
- `testing/` → Generación de tests unitarios/integración
- `performance-optimization/` → Análisis y mejora de rendimiento

## Uso rápido

### Para implementar una HU:

```bash
# Opción 1: Usar template
Copiar contenido de: .ai/templates/user-story-implementation/full-stack.md
Completar HU y pegar en Cursor Agent

# Opción 2: Comando directo
@.ai/core/policies.md @.ai/workflows/user-story-implementation/orchestrator.md

Implementa HU-023: [pegar HU completa]
```

Ver guía completa: [guides/user-story-implementation.md](guides/user-story-implementation.md)

## Documentación del proyecto

| Archivo | Propósito |
|---|---|
| `docs/user-stories.md` | HUs exactas recibidas |
| `docs/code-structure.md` | Estructuras de código analizadas/modificadas |
| `docs/implementation-report.md` | Resultados de implementación por HU |
| `docs/integration-report.md` | Validación E2E completa por HU |
| `docs/guides/architecture.md` | Arquitectura del sistema |
| `docs/guides/quality-standards.md` | Estándares de calidad aplicados |

## Tests y ejemplos

Ver `tests/ai-strategy/` para ejemplos completos de ejecución de cada workflow.

## Estándares de calidad

El ecosistema integra:
- **ISO/IEC 25010** (calidad del producto software) como referencia
- **WCAG 2.1** (accesibilidad) cuando aplica
- **OWASP Top 10** (seguridad) en validaciones
- **Clean Code / SOLID** en generación de código
- **Conventional Commits** en trazabilidad

Ver [standards/quality-attributes.md](standards/quality-attributes.md)

## Para la tesis

- **Prompts originales (110 pág):** Eliminados (cumplieron propósito académico; `.ai/` es la referencia operacional)
- **Versión operativa:** Esta estructura modular (versión 2.0)
- **CHANGELOG:** Ver `CHANGELOG.md` en raíz para historial de versiones

## Contribuir

Al agregar prompts, workflows o guías:

1. Mantener herencia de `core/policies.md`
2. Documentar en README del workflow
3. Proveer templates de uso
4. Agregar ejemplo en `tests/ai-strategy/`
5. Actualizar este README con el nuevo workflow

---

**Versión:** 2.1.0  
**Última actualización:** 2026-09-14  
**Licencia:** [Definir según proyecto]
