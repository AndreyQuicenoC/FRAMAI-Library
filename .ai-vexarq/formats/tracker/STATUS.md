# Tablero de estado

El archivo que ve el usuario es `docs/vexarq/status.html`, copiado desde `.ai-vexarq/tracker/project-status.template.html` la primera vez.

## Qué se actualiza

Solo elementos con `data-vexarq`. No se añaden secciones.

| data-vexarq | Contenido |
|---|---|
| project-name | Nombre del proyecto |
| business-model | Modelo identificado, o "Sin identificar" |
| execution-mode | validar o continuo |
| profile | ligero, estandar, avanzado o critico |
| current-phase | Nombre de la fase en curso |
| next-action | Una frase: qué falta para avanzar |
| updated-at | Fecha de la solicitud |
| request-id | Id del reporte |

Cada fase tiene `data-phase` y un hijo `[data-vexarq="state"]` con uno de: pendiente, en curso, preliminar, vigente, bloqueado, no aplica.

El enlace `[data-vexarq="doc"]` apunta al vigente. Si solo hay preliminar, el texto del enlace es Preliminar y el href apunta a ese archivo. Si no hay documento, el enlace se omite.

## Qué no muestra

Prompts, diffs, secretos, historial de chat ni listas largas de tareas.
