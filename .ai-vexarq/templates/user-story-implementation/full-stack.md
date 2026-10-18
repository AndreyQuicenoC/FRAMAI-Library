@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implementa la siguiente Historia de Usuario usando el workflow multiagente de user-story-implementation.

## Instrucciones de ejecución
1. **Orquestador** analiza y clasifica la HU, genera contrato compartido y execution_plan
2. Según el plan, ejecuta SOLO los agentes necesarios:
   - Backend: @.ai-vexarq/workflows/user-story-implementation/backend.xml
   - Frontend: @.ai-vexarq/workflows/user-story-implementation/frontend.xml
   - Integrador: @.ai-vexarq/workflows/user-story-implementation/integrator.xml
3. Cada agente actualiza docs/ (user-stories.md, code-structure.md, implementation-report.md, integration-report.md)
4. Implementa código real y ejecuta tests
5. No te detengas solo en análisis; produce cambios verificables

---

## Historia de Usuario

**ID:** HU-XXX

**Como** [actor]  
**Quiero** [acción]  
**Para** [beneficio]

### Criterios de aceptación
1. [Criterio verificable]
2. [Criterio verificable]
3. ...

---

## Contexto del proyecto

**Ruta:** .

**Restricciones adicionales:** Ninguna

**Alcance solicitado:** auto (dejar que Orquestador clasifique)

---

## Output esperado

- Documentación actualizada en `docs/user-stories.md`, `docs/code-structure.md`, `docs/implementation-report.md`, `docs/integration-report.md`
- Código implementado y funcionando
- Tests ejecutados y pasando
- Trazabilidad HU → código → tests → evidencia
