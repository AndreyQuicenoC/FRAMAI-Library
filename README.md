# VEXARG

Sistema modular de ingeniería de software asistida por IA. Se copia la carpeta `.ai-vexarq` a un proyecto y el agente ya tiene cómo interpretar una solicitud, qué artefacto producir, qué debe validar una persona y qué evidencia cierra el trabajo.

VEXARG continúa FRAMAI. El motor multiagente de historias de usuario, los estándares de ingeniería y el flujo Git de la versión 2.2.0 siguen en la biblioteca. El cambio de la 3.0.0 es el ciclo de vida alrededor de ese motor.

## Qué problema resuelve

Un paquete de prompts sueltos genera documentos que no se hablan entre sí. VEXARG separa el motor, las plantillas, los contratos de salida, el contexto vigente y la comprobación. La fase siguiente no puede tomar un borrador de la IA si todavía no es el documento vigente.

## En qué se diferencia

| | Spec Kit | BMAD | VEXARG |
|---|---|---|---|
| Unidad de trabajo | Especificación de una funcionalidad, con comandos por fase | Equipo de agentes y entregas entre archivos | Ciclo de producto y, dentro de él, el motor de historias ya probado en FRAMAI |
| Autoridad del requisito | Constitución y especificación del kit | Handoff del agente anterior | Documento vigente editado por la persona. El preliminar de la IA no alimenta la fase siguiente |
| Backlog e arquetipo | Tareas derivadas de la spec | Planificación por roles | Generadores canónicos propios, ejecutados sin reescribirlos, más un backlog extendido CSV agnóstico del proyecto |
| Cierre | Convergencia de la funcionalidad | Revisión del agente de calidad | Tablero HTML de una página, reporte de solicitud y recomendaciones de producto, técnica, documentación, negocio y trabajo futuro |
| Publicación | Depende del agente anfitrión | Depende del agente anfitrión | Push prohibido salvo remoto, rama y autorización explícitos, con checks en verde |

Spec Kit y BMAD son referencias de desarrollo dirigido por especificación. VEXARG no los envuelve: fija preliminares y vigentes, conserva los generadores que ya funcionaban y exige evidencia antes de dar una fase por cerrada.

## Cómo se usa

1. Copia `.ai-vexarq/` a la raíz del proyecto.
2. Abre `.ai-vexarq/templates/lifecycle/ciclo.md`, `fase.md` o `continuo.md`.
3. Pega la plantilla en el agente y escribe el pedido.

Guía completa: [`.ai-vexarq/guides/usar-vexarq.md`](.ai-vexarq/guides/usar-vexarq.md).

`validar` es el modo por defecto: la IA deja el resultado en `docs/vexarq/preliminary/` y espera. `continuo` rellena huecos con supuestos visibles y puede recorrer documentación, frontend, backend y pruebas. Ninguno de los dos hace push por su cuenta.

## Fases

Acercamiento, SRS, base de datos, backlog, backlog extendido, SAD, arquetipo, implementación, entrega y recomendaciones. El tablero `docs/vexarq/status.html` muestra en qué fase está el proyecto, qué falta y el enlace al documento. La plantilla vacía está en `.ai-vexarq/tracker/project-status.template.html`.

El acercamiento identifica el modelo de negocio (SaaS, marketplace, comercio, herramienta interna, API, contenido, on-premise, institucional u otro). No asume SaaS.

## Qué vive en cada formato

- XML: prompts, políticas y registros.
- Markdown: contratos de documentos.
- CSV: backlog extendido.
- HTML estático: tablero de fase, reescrito en cada solicitud sin convertirla en un informe.

Los generadores de backlog y de arquetipo están en `.ai-vexarq/sources/canon/` y se ejecutan completos. El prompt de backlog extendido de un proyecto externo queda en `sources/examples/` como calibración; la generación universal es `workflows/lifecycle/backlog-extended.xml`.

## Seguridad de trabajo

- No editar archivos ajenos al alcance.
- No cambiar lógica funcional que el pedido no pidió cambiar.
- No commitear secretos.
- No hacer push sin autorización explícita del destino.
- No declarar una prueba como pasada si no se ejecutó.

Detalle: [SECURITY.md](SECURITY.md) y `.ai-vexarq/core/git-workflow.xml`.

## Mapa de la carpeta

```text
.ai-vexarq/
  workflows/lifecycle/          una plantilla de fase y el orquestador general
  workflows/user-story-implementation/   motor FRAMAI de historias
  sources/canon/                backlog y arquetipo, sin modificar
  sources/examples/             ejemplo externo de backlog extendido
  formats/                      contratos Markdown
  tracker/                      HTML base del estado
  core/                         políticas, modos, linaje, git, diseño
  guides/usar-vexarq.md         guía de uso
  templates/lifecycle/          textos listos para pegar
```

## Licencia

MIT. Ver [LICENSE](LICENSE).
