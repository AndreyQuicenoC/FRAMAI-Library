# Reports

This directory contains reports, analyses, and additional documents organized by number and category.

## Structure

```
reports/
├── 001-analisis-inicial/
│   └── informe.md
├── 002-metricas-sprint-1/
│   └── metricas.md
└── ...
```

## Conventions

### Numbering
- Use a 3-digit prefix: `001-`, `002-`, `003-`, and so on.
- Sequential numbering in chronological order

### Names
- Format: `NNN-nombre-descriptivo/`
- Use kebab-case (lowercase with hyphens)
- Clear and concise names

### Contents
Each numbered folder may contain:
- `README.md` or `informe.md` — Main document
- Additional files (images, data, and so on)
- Subfolders if necessary

## Examples

### Analysis
- `001-analisis-arquitectura/`
- `002-estudio-rendimiento/`

### Metrics
- `003-metricas-sprint-1/`
- `007-cobertura-tests/`

### Incidents/Postmortems
- `004-postmortem-fallo-db/`
- `008-analisis-bug-critico/`

### Technical decisions
- `005-decision-migracion-db/`
- `009-seleccion-framework/`

---

**Rule:** DO NOT use generic names. Each report must have a unique number and a descriptive name.
