# Reportes

Este directorio contiene reportes, análisis y documentos adicionales organizados por número y categoría.

## Estructura

```
reports/
├── 001-analisis-inicial/
│   └── informe.md
├── 002-metricas-sprint-1/
│   └── metricas.md
└── ...
```

## Convenciones

### Numeración
- Usar prefijo de 3 dígitos: `001-`, `002-`, `003-`, etc.
- Numeración secuencial por orden cronológico

### Nombres
- Formato: `NNN-nombre-descriptivo/`
- Usar kebab-case (minúsculas con guiones)
- Nombres claros y concisos

### Contenido
Cada carpeta numerada puede contener:
- `README.md` o `informe.md` — Documento principal
- Archivos adicionales (imágenes, datos, etc.)
- Subcarpetas si es necesario

## Ejemplos

### Análisis
- `001-analisis-arquitectura/`
- `002-estudio-rendimiento/`

### Métricas
- `003-metricas-sprint-1/`
- `007-cobertura-tests/`

### Incidentes/Postmortems
- `004-postmortem-fallo-db/`
- `008-analisis-bug-critico/`

### Decisiones técnicas
- `005-decision-migracion-db/`
- `009-seleccion-framework/`

---

**Regla:** NO usar nombres genéricos. Cada reporte debe tener número único y nombre descriptivo.
