# Phase board

## File

`docs/vexarq/status.html`, created by copying `.ai-vexarq/tracker/project-status.template.html`.

## Fields

Only `data-vexarq` attributes change:

- `data-project`: the project name.
- `data-updated`: the date of the request.
- `data-phase`: the id of the current phase.
- `data-state`: `pending`, `in progress`, `preliminary`, `current`, `blocked`, or `not applicable`.
- `data-href`: a relative link to the document, or empty.
- `data-note`: one short sentence.

Visible text follows the language selected for the project. The state tokens above stay in English because the template's selectors depend on them.

## Rows

Discovery, SRS, database, backlog, extended backlog, SAD, archetype, implementation, delivery, and recommendations.

When the SAD defines a structure, the board may show frontend, backend, data, tests, and documentation as rows of that structure. They are not extra lifecycle phases.

## Content

No prompt text, no history, and no metrics. A link points at the current document. If only a preliminary exists, the link text is `Preliminary`.
