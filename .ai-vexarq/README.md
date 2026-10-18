# .ai-vexarq

Carpeta portable de VEXARG. Cópiala a la raíz de un proyecto y pega una plantilla de `templates/lifecycle/`.

Guía: [guides/usar-vexarq.md](guides/usar-vexarq.md).

## Entradas

| Archivo | Uso |
|---|---|
| [workflows/lifecycle/orchestrator.xml](workflows/lifecycle/orchestrator.xml) | Detecta la fase o recorre el ciclo |
| [templates/lifecycle/ciclo.md](templates/lifecycle/ciclo.md) | Texto para pegar, modo validar |
| [templates/lifecycle/fase.md](templates/lifecycle/fase.md) | Una sola fase |
| [templates/lifecycle/continuo.md](templates/lifecycle/continuo.md) | De inicio a fin, con supuestos visibles |
| [workflows/user-story-implementation/orchestrator.xml](workflows/user-story-implementation/orchestrator.xml) | Implementar una HU cuando ya hay base |

## Regla de linaje

`docs/vexarq/preliminary/` es la salida de la IA. `docs/vexarq/current/` es lo que la siguiente fase puede leer. El tablero es `docs/vexarq/status.html`, creado desde [tracker/project-status.template.html](tracker/project-status.template.html).

## Canon

No editar [sources/canon/](sources/canon/). El ejemplo de otro proyecto está en [sources/examples/](sources/examples/) y no se aplica fuera de su producto.

## Implementación de historias

El motor adaptativo (orquestador, backend, frontend, integrador, perfiles de complejidad y estándares) se conserva. Su guía operativa sigue en [guides/user-story-implementation.xml](guides/user-story-implementation.xml).

Versión 3.0.0.
