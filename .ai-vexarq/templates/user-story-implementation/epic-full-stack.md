@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implementa las siguientes Historias de Usuario (lote / épica) usando el workflow multiagente de user-story-implementation.

## Instrucciones de ejecución
1. **Orquestador** analiza el proyecto, determina `project_mode` por repositorio, ordena las HU por dependencias, genera UN contrato de épica, `execution_plan`, `epic_plan` y `commit_plan`
2. Según el plan, ejecuta SOLO los agentes necesarios:
   - Backend: @.ai-vexarq/workflows/user-story-implementation/backend.xml
   - Frontend: @.ai-vexarq/workflows/user-story-implementation/frontend.xml
   - Integrador: @.ai-vexarq/workflows/user-story-implementation/integrator.xml
3. Estándares y flujo (cargar las secciones que el Orquestador indique):
   - @.ai-vexarq/core/engineering-standards.xml
   - @.ai-vexarq/core/git-workflow.xml
   - @.ai-vexarq/core/project-modes.xml
4. Cada agente actualiza docs/ (user-stories.md, code-structure.md, implementation-report.md, integration-report.md)
5. Implementa código real y ejecuta TODOS los quality gates; no declares COMPLETED sin evidencia
6. No te detengas solo en análisis; produce cambios verificables

---

## Historias de Usuario

[Pegar cada HU con su ID, rol, funcionalidad, criterios de aceptación, DoR/DoD y tareas técnicas, sin resumir]

---

## Contexto del proyecto

**Rutas:** [backend: ruta] [frontend: ruta]

**Directorios intocables:** [ej. demo/ — no modificar]

**Restricciones adicionales:** Ninguna

**Alcance solicitado:** auto (dejar que Orquestador clasifique)

---

## Flujo Git (ver git-workflow.xml)

- Verificar o crear `develop` desde `main` en cada repositorio; crear la rama de épica `feature/ep-XX-<nombre>` desde `develop`
- Trabajar solo en la rama de la épica; NO hacer merge ni modificar `main` ni `develop` después de prepararlas
- Un commit por HU completada con éxito (más commit de fundación y commits transversales aparte)
- Push de la rama de la épica solo cuando todo funcione y la integración esté validada

## Entorno y operación

- Base de datos local: [motor y modo, ej. PostgreSQL en contenedor]
- Correo / recuperación de contraseña: [dejar variables preparadas con enlaces conocidos y ejemplos del resto]
- Archivos `.env` locales listos y `.env.example` con la misma estructura en cada repositorio
- Contenerización de cada servicio y compose funcional
- Scripts de pruebas e integración en `scripts/` de cada repositorio

## Seguridad (obligatorio)

- Ningún secreto, credencial o dato sensible en código, documentación, logs ni commits
- Escaneo de secretos antes de cada commit; `.env` real fuera del control de versiones

## Output esperado

- Documentación actualizada en `docs/user-stories.md`, `docs/code-structure.md`, `docs/implementation-report.md`, `docs/integration-report.md`
- Código implementado y funcionando en backend y frontend
- Tests ejecutados y pasando; integración real validada
- Trazabilidad HU → código → tests → evidencia
