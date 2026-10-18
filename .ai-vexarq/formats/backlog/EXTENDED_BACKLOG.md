# Contrato del backlog extendido

Versión de columnas: 1.0.0

El archivo de trabajo es CSV UTF-8, RFC 4180, separado por comas. El Markdown explica el contrato y no sustituye al CSV.

## Cabecera obligatoria

```csv
Tipo,Id de la Historia,Id de Tarea,Epica,MVP,Sprint,Rol,Caracteristica / Funcionalidad,Razon / Resultado,Criterio de Aceptacion,Estado de validacion,Observacion,Tipo de tarea,DoR,DoD,Estimacion (SP Fibonacci),Justificacion de estimacion,Prioridad MoSCoW,Justificacion de prioridad,Dependencias,Riesgo,Notas del analista
```

## Filas

- `HU`: conserva el texto original del backlog vigente. Id de Tarea, Tipo de tarea y los campos de tarea van vacíos. DoR, DoD, estimación, prioridad, dependencias, riesgo y notas viven en esta fila.
- `Tarea`: repite el id de la HU padre, trae su id de tarea y su título en Caracteristica / Funcionalidad, el tipo de tarea y los números de criterio que cubre.

## Campos nuevos respecto del ejemplo externo

- Dependencias: identificadores, separados por espacio.
- Riesgo: `bajo`, `medio` o `alto`, y una frase en la misma celda.

El ejemplo de otro proyecto usaba TSV para una hoja de cálculo y fijaba sprints, siglas y conteos de ese producto. Esos datos no forman parte de este contrato.

## Codificación

- Comillas dobles alrededor de cualquier campo con coma, comilla o salto de línea.
- Una comilla doble interna se escapa duplicándola, salvo la excepción de los mensajes literales ya convertidos a comillas simples por el prompt de fase.
- Sin BOM obligatorio. Sin filas de título antes de la cabecera.

## Informe que acompaña al CSV

`validacion.md` junto al CSV: conteos, integridad, cobertura de categorías, escala, alertas y confirmación de que no se importó el alcance del ejemplo externo.
