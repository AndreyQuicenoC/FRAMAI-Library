# Ecosistema de Prompts para Desarrollo con IA

> **Context engineering sobre prompt engineering**
> Proveer el contexto preciso, en el momento adecuado, al agente correcto.

Plataforma profesional y modular de prompts, workflows y guías para desarrollo de software asistido por IA.

## Filosofía

- **Separación de responsabilidades** mediante workflows especializados
- **Políticas centralizadas** que todos los workflows heredan
- **Carga dinámica** según la tarea (no enviar todo siempre)
- **Trazabilidad verificable** del requisito al código
- **Calidad ejecutable**: quality gates, pruebas por capa, integración con la pila real
- **Seguridad por defecto**: sin secretos en el repositorio, sesiones seguras, publicación solo con todo en verde
- **Extensibilidad** para agregar nuevos workflows sin afectar existentes

## Inicio rápido (5 minutos)

1. **Copia una plantilla:** `.ai/templates/user-story-implementation/full-stack.md` (una HU) o `epic-full-stack.md` (lote de HU / épica)
2. **Completa tus HU** (ID + descripción + criterios)
3. **Pega en Cursor Agent** (Cmd/Ctrl+L)
4. **Deja trabajar:** Orquestador, Backend, Frontend, Integrador
5. **Revisa resultado:** 4 documentos actualizados:
   - `docs/user-stories.md`
   - `docs/code-structure.md`
   - `docs/implementation-report.md`
   - `docs/integration-report.md`

Ver guía detallada: [guides/user-story-implementation.xml](guides/user-story-implementation.xml)

## Estructura

```
.ai/
├── README.md                     Estás aquí
├── core/                         Políticas globales (heredadas por todos)
│   ├── policies.xml              Reglas siempre cargadas
│   ├── adaptive-policies.xml     Complejidad, factores de elevación, modo lote
│   ├── engineering-standards.xml Seguridad, quality gates, pruebas, entorno, contenedores
│   ├── git-workflow.xml          Ramas, commits por HU, puertas previas a push
│   └── project-modes.xml         Extensión, arquetipo, productización de mockup
├── workflows/                    Flujos especializados por tipo de tarea
│   └── user-story-implementation/
│       ├── README.md
│       ├── orchestrator.xml
│       ├── backend.xml
│       ├── frontend.xml
│       └── integrator.xml
├── templates/                    Plantillas de mensajes reutilizables
│   └── user-story-implementation/
│       ├── full-stack.md
│       ├── epic-full-stack.md
│       ├── backend-only.md
│       ├── frontend-only.md
│       └── integration-fix.md
├── guides/                       Guías de uso paso a paso
│   └── user-story-implementation.xml
└── standards/                    Estándares de calidad y referencias
    └── quality-attributes.xml
```

## Workflows disponibles

| Workflow | Descripción | Cuándo usar |
|---|---|---|
| **[user-story-implementation](workflows/user-story-implementation/)** | Implementación de Historias de Usuario (HU) o épicas con arquitectura multiagente | Desarrollar nueva funcionalidad a partir de requisitos |

## Agregar nuevo workflow

1. Crear carpeta en `workflows/[nombre-workflow]/`
2. Incluir `README.md` con descripción y uso
3. Definir agentes/etapas necesarios
4. Cada agente hereda `core/policies.xml`
5. Crear templates en `templates/[nombre-workflow]/`
6. Documentar en `guides/[nombre-workflow].xml`

Ejemplos de workflows futuros:
- `code-review/` — Revisión de PR con checklist de calidad
- `refactoring/` — Refactorización segura con tests
- `bug-fix/` — Diagnóstico y corrección de bugs
- `documentation/` — Generación/actualización de docs
- `testing/` — Generación de tests unitarios/integración
- `performance-optimization/` — Análisis y mejora de rendimiento

## Uso rápido

### Para implementar una HU:

```bash
# Opción 1: Usar plantilla
Copiar contenido de: .ai/templates/user-story-implementation/full-stack.md
Completar HU y pegar en Cursor Agent

# Opción 2: Comando directo
@.ai/core/policies.xml @.ai/workflows/user-story-implementation/orchestrator.xml

Implementa HU-023: [pegar HU completa]
```

### Para implementar una épica (varias HU, varios repositorios):

Usar `.ai/templates/user-story-implementation/epic-full-stack.md`. El Orquestador ordena las HU por dependencias, genera un contrato de épica y un plan de commits; Backend y Frontend cierran cada HU con tests y commit; el Integrador valida con la pila real antes de cualquier push.

Ver guía completa: [guides/user-story-implementation.xml](guides/user-story-implementation.xml)

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

Ver [standards/quality-attributes.xml](standards/quality-attributes.xml) y [core/engineering-standards.xml](core/engineering-standards.xml)

## Contribuir

Al agregar prompts, workflows o guías:

1. Mantener herencia de `core/policies.xml`
2. Documentar en README del workflow
3. Proveer templates de uso
4. Agregar ejemplo en `tests/ai-strategy/`
5. Actualizar este README con el nuevo workflow

---

**Versión:** 2.2.0
**Última actualización:** 2026-10-11
**Licencia:** MIT
