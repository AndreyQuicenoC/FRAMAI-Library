# Extended backlog

## File

`backlog-extendido.csv`, UTF-8, comma-separated, [RFC 4180](https://www.rfc-editor.org/rfc/rfc4180).

## Columns

`Type`, `Story ID`, `Task ID`, `Epic`, `MVP`, `Sprint`, `Role`, `Feature`, `Reason`, `Acceptance Criteria`, `Validation Status`, `Observation`, `Task Type`, `Definition of Ready`, `Definition of Done`, `Estimate (Fibonacci SP)`, `Estimate Justification`, `MoSCoW Priority`, `Priority Justification`, `Dependencies`, `Risk`, `Analyst Notes`.

## Rows

`Story` repeats the story and leaves the task fields empty. Each `Task` repeats the story's identity and fills only its own task. `Feature` carries the original feature on the story row and the task title on the task row.

## Text

`Acceptance Criteria`, `Definition of Ready`, and `Definition of Done` use real line breaks inside double quotes. Definitions of ready and done use the bullet `•`.

Validation status has three values: `Ready`, `With remarks`, `Pending clarification`.

## What does not change

The identifier, epic, MVP, sprint, role, feature, reason, criteria, status, and observation are copied from the current backlog. This format does not authorize paraphrasing them.

## Report

`validacion.md` accompanies the CSV: stories read, stories written, integrity, categories covered, alerts, and confirmation that no other project's example was imported.
