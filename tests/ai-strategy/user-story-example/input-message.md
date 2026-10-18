# Input: Mensaje enviado a Cursor

```
@.ai-vexarq/core/policies.xml
@.ai-vexarq/workflows/user-story-implementation/orchestrator.xml

Implementa la siguiente Historia de Usuario usando el workflow multiagente de user-story-implementation.

## Instrucciones de ejecución
1. **Orquestador** analiza y clasifica la HU, genera contrato compartido y execution_plan
2. Según el plan, ejecuta SOLO los agentes necesarios:
   - Backend: @.ai-vexarq/workflows/user-story-implementation/backend.xml
   - Frontend: @.ai-vexarq/workflows/user-story-implementation/frontend.xml
   - Integrador: @.ai-vexarq/workflows/user-story-implementation/integrator.xml
3. Cada agente actualiza `docs/dev-history.md` con su sección
4. Implementa código real y ejecuta tests
5. No te detengas solo en análisis; produce cambios verificables

---

## Historia de Usuario

**ID:** HU-023

**Como** cajero  
**Quiero** registrar una venta con productos y cantidades  
**Para** generar ticket y actualizar inventario

### Criterios de aceptación
1. Formulario con selector de productos, campo cantidad y precio unitario
2. Total calculado automáticamente al agregar/modificar productos
3. POST /api/sales persiste en base de datos y retorna 201 con ID de venta
4. Mensaje de éxito en pantalla y redirección a /sales
5. Errores de validación visibles inline en el formulario
6. Solo usuarios con rol "cashier" pueden registrar ventas

---

## Contexto del proyecto

**Ruta:** .

**Stack:**
- Backend: Node.js + Express + PostgreSQL + Prisma
- Frontend: React + Vite + React Router + Axios
- Auth: JWT con roles (middleware existente)

**Restricciones adicionales:** Ninguna

**Alcance solicitado:** auto (dejar que Orquestador clasifique)

---

## Output esperado

- Entrada agregada en `docs/dev-history.md` con análisis, implementación y validación
- Código implementado y funcionando
- Tests ejecutados y pasando
- Trazabilidad HU → código → tests → evidencia
```

---

**Fecha:** 2026-09-14  
**Cursor version:** 0.42.x  
**Model:** Claude Sonnet 4.5 (Agent mode)
