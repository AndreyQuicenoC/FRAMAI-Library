# Cómo usar VEXARG

VEXARG es la carpeta `.ai-vexarq`. Se copia entera a la raíz del proyecto y se usa desde el chat del agente. No hace falta instalar nada.

## 1. Copiar

Copia `.ai-vexarq/` a la raíz del repositorio donde vas a trabajar. Opcional: copia `.ai-vexarq/cursor/vexarq.mdc` a `.cursor/rules/vexarq.mdc` para que el editor recuerde las reglas.

## 2. Elegir cómo empezar

| Intención | Qué pegar |
|---|---|
| Recorrer el ciclo y que el sistema detecte la fase | `templates/lifecycle/ciclo.md` |
| Trabajar una sola fase | `templates/lifecycle/fase.md` |
| Construir de inicio a fin sin parar en cada revisión | `templates/lifecycle/continuo.md` |
| Implementar una historia cuando ya hay código o arquetipo | `templates/user-story-implementation/` |

El archivo que el agente debe obedecer es `workflows/lifecycle/orchestrator.xml`. Las plantillas solo lo invocan.

## 3. Modos

`validar` es el modo por defecto. La IA escribe en `docs/vexarq/preliminary/` y se detiene. Tú editas y dejas la versión buena en `docs/vexarq/current/`. La fase siguiente solo lee `current/`.

`continuo` rellena los huecos con los formatos y ejemplos de la biblioteca, marca cada hueco como supuesto y sigue hasta el alcance pedido: documentación, frontend, backend y pruebas, según el producto. No autoriza un push.

## 4. Orden de las fases

1. Acercamiento, con el modelo de negocio.
2. SRS.
3. Base de datos.
4. Backlog, con el generador canónico intacto.
5. Backlog extendido en CSV.
6. SAD.
7. Arquetipo, con el generador canónico intacto.
8. Implementación, según la estructura real: frontend, backend, datos, pruebas y documentación.
9. Entrega.
10. Recomendaciones de software, técnica, documentación, negocio y funcionalidades futuras.

Una corrección pequeña no recorre esa lista.

## 5. Tablero

`docs/vexarq/status.html` se crea desde `tracker/project-status.template.html` y se actualiza al cerrar cada solicitud. Muestra fase, estado y enlace. El contrato está en `formats/tracker/STATUS.md`.

## 6. Qué no tocar

- `sources/canon/`: se ejecutan tal cual.
- Un vigente ya editado por ti: la IA no lo pisa.
- Archivos fuera del alcance, lógica que no pediste cambiar, secretos y pushes no autorizados.

## 7. Dónde leer después

- Fases y puertas: `core/approval-gates.xml`
- Preliminares y vigentes: `core/artifact-lineage.xml`
- Backlog extendido: `workflows/lifecycle/backlog-extended.xml` y `formats/backlog/EXTENDED_BACKLOG.md`
- Historias de usuario sobre código existente: `guides/user-story-implementation.xml`
- Git: `core/git-workflow.xml` y `formats/delivery/COMMIT_CONVENTIONS.md`
