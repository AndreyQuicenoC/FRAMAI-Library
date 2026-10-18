\# PROMPT — Conversión del Product Backlog del MVP 1 (DashNode) a tabla de trabajo con tareas, DoR, DoD, estimación y priorización

> Cómo usarlo: pega este prompt completo en un chat nuevo de Claude y, a continuación, pega el contenido de la pestaña **"Refinado"** correspondiente al **MVP 1** (Sprints 1, 2 y 3). No pegues la pestaña "Generado".

---

## **1\. ROL**

Actúas como **Analista Ágil sénior y Scrum Master** con experiencia en refinamiento de Product Backlogs, descomposición de historias de usuario en tareas técnicas, estimación con Planning Poker (Fibonacci) y priorización MoSCoW. Trabajas en español, con precisión y sin inventar información.

## **2\. CONTEXTO**

* **Proyecto:** DashNode (sigla **DN**): plataforma de gestión de proyectos, backlog, sprints, tareas, reuniones, documentación e issues con asistencia de IA.  
* **Alcance de esta ejecución:** únicamente el **MVP 1 — Fundamentos y Colaboración Base**:  
  * Sprint 1 — Autenticación, Seguridad y Gestión de Usuarios (AUT, USU)  
  * Sprint 2 — Gestión de Proyectos, Espacio de Trabajo y Disponibilidad (PRO, DIS)  
  * Sprint 3 — Agendamiento y Reuniones con Google Calendar (AGE)  
* **Fuente única de verdad:** el backlog de la pestaña **"Refinado"** que pego debajo de este prompt. Ya está validado (todas las HU en ✅ Lista) y sus decisiones (P-01 a P-33) están cerradas. **No uses ninguna otra versión del backlog.**  
* **Plan total del proyecto:** 3 MVP y 9 sprints. En esta ejecución solo se procesa el MVP 1; los MVP 2 y 3 se procesarán después con este mismo prompt.  
* **Roles del sistema:** Visitante, Administrador, Gerente, Desarrollador.  
* **Cantidad esperada de HU en el MVP 1:** según el encabezado del backlog son 20 (Sprint 1\) \+ 13 (Sprint 2\) \+ 11 (Sprint 3\) \= **44 HU**. Los IDs `HU_DN_01_15` y `HU_DN_03_43` NO existen porque fueron absorbidos por otras HU (`01_14` y `03_42`); no los generes ni los menciones como faltantes. Cuenta las HU realmente recibidas y repórtalo (ver sección 8).

## **3\. OBJETIVO**

Transformar cada HU del MVP 1 en un conjunto de filas listo para pegar en **Google Sheets** (y exportable luego a CSV para importar a Jira), **conservando íntegro todo el contenido original de la HU** y **agregando** los elementos que faltan: **tareas, DoR, DoD, estimación (Fibonacci) y prioridad (MoSCoW)**.

## **4\. REGLA DE ORO: NO PERDER NINGÚN ELEMENTO ORIGINAL**

Cada HU original tiene estos elementos, y **todos deben aparecer en la salida, sin resumir, sin parafrasear y sin omitir nada**:

1. Id de la Historia  
2. Épica  
3. MVP  
4. Sprint  
5. Rol  
6. Característica / Funcionalidad  
7. Razón / Resultado  
8. Criterio de Aceptación completo, con sus 6 categorías (Dónde, Campos con sub-ítems, Validación, Restricción, Éxito, Fallo / Fallo técnico), incluyendo **cada mensaje literal entre comillas, cada límite numérico, cada regla de rol y cada referencia a otra HU, RF, RN o RNF**  
9. Estado de validación (solo el texto, sin emoji)  
10. Observación (incluye trazabilidad: RF, RN, RNF, prioridad del SRS, notas de dependencia con otros sprints)

Copia estos textos **literalmente** desde el backlog. No corrijas redacción, no cambies mensajes, no unifiques HU, no cambies IDs. **Únicas adaptaciones permitidas (ver sección 6):** (a) las comillas de los mensajes literales pasan a comillas simples (' ') dentro de las celdas encerradas en comillas dobles; el texto del mensaje no cambia; (b) el campo Estado de validación se escribe sin emoji (por ejemplo, '✅ Lista' pasa a `Lista`).

## **5\. ELEMENTOS NUEVOS QUE DEBES GENERAR POR CADA HU**

### **5.1 Tareas**

* Divide cada HU en **tareas técnicas atómicas y verificables**, derivadas **directamente de los criterios de aceptación**.  
* **Cada categoría del criterio de aceptación debe quedar cubierta por al menos una tarea** (Dónde → interfaz/navegación; Campos → formulario y validaciones de cliente; Validación → reglas de servidor; Restricción → seguridad/permisos/reglas de negocio; Éxito y Fallo → mensajes y estados; Fallo técnico → manejo de errores).  
* Incluye siempre, cuando aplique: tarea de **base de datos/modelo**, **backend/API**, **frontend**, **pruebas** (unitarias y de aceptación) y **documentación**. No inventes tareas de capas que la HU no toca (por ejemplo, una HU de solo lectura no lleva tarea de formulario).  
* Cada tarea lleva: **ID de tarea** con el formato `TK_DN_<sprint>_<consecutivo HU>_<n>` (ejemplo: `TK_DN_01_02_1`), **título** que inicie con verbo en infinitivo, **tipo** (Frontend, Backend, BD, QA, Seguridad, Documentación, DevOps) y **criterio(s) de aceptación que cubre** (por número, ejemplo: "2.3, 4").  
* Una HU debe tener entre 3 y 10 tareas. Si necesitas más de 10, señala en el campo de justificación que la HU podría ser demasiado grande.  
* No agregues funcionalidades que no estén en la HU. Si detectas una ambigüedad real, no la resuelvas por tu cuenta: regístrala en "Notas del analista" (sección 6).

### **5.2 DoR — Definition of Ready (por HU)**

Lista de condiciones que deben cumplirse **antes** de que el equipo tome la HU en un sprint. Debe incluir:

* Las **condiciones generales** (válidas para toda HU): HU con rol, funcionalidad y razón definidos; criterios de aceptación completos y sin pendientes ❓; mensajes de éxito y fallo definidos; estimada por el equipo; priorizada; sin dependencias bloqueantes sin resolver; mockup o diseño de Figma disponible cuando la HU tiene interfaz.  
* Las **condiciones específicas** de la HU, derivadas de sus dependencias reales (por ejemplo, "HU\_DN\_01\_03 terminada" si la HU depende del inicio de sesión; "esquema de BD de usuarios definido" si aplica; "servicio de correo transaccional configurado" si aplica; "disponibilidad registrada por los usuarios" para el cálculo de horarios).

### **5.3 DoD — Definition of Done (por HU)**

Lista de condiciones que deben cumplirse para considerar la HU terminada. Debe incluir:

* Las **condiciones generales**: código en la rama de desarrollo con revisión por pares; todos los criterios de aceptación verificados; pruebas unitarias y de aceptación pasando; validaciones repetidas en el servidor; mensajes literales exactos a los del backlog; accesibilidad básica (no depender solo del color, operable con teclado); sin errores de lint; documentación actualizada; demo aprobada por el Product Owner.  
* Las **condiciones específicas** de la HU: seguridad que aplique (hash de contraseñas, cookie HttpOnly, control de acceso por rol en el servidor, registro de auditoría), reglas de negocio críticas (por ejemplo, "una solicitud directa con rol Administrador es rechazada") y requisitos no funcionales citados en la HU (por ejemplo, RNF-REN-01: respuesta ≤ 3 s en percentil 95).

### **5.4 Estimación — escala Fibonacci**

* Usa **únicamente** los valores **1, 2, 3, 5, 8, 13, 21** (puntos de historia; es la misma secuencia que ya usa el SRS).  
* La estimación es **por HU** (no por tarea) y considera: complejidad técnica, cantidad de campos y reglas de validación, número de roles y alcances de datos, seguridad, integraciones externas (Google Calendar, correo), dependencias y riesgo.  
* Referencia de calibración: **1** \= cambio mínimo y sin lógica (una acción simple, un listado sencillo); **3** \= formulario o consulta con validaciones moderadas; **5** \= formulario con varias reglas de rol y mensajes, o lógica no trivial; **8** \= lógica compleja o varias piezas coordinadas (por ejemplo, cálculo de horarios); **13** \= HU grande con riesgo alto. **21** \= señal de que la HU debería dividirse.  
* Si asignas **13 o 21**, indícalo en "Notas del analista" y sugiere cómo dividirla (sin dividirla tú: la HU se conserva tal cual).  
* Cada estimación lleva una **justificación de una frase**.  
* Aclara que es una **estimación preliminar del analista** sujeta a ajuste en el Planning Poker del equipo.

### **5.5 Prioridad — MoSCoW**

* Usa únicamente: **Must have, Should have, Could have, Won't have (this MVP)**.  
* Criterios de decisión, en este orden:  
  1. La prioridad que el SRS asignó a la HU y que aparece en **Observación** ("Prioridad SRS: Crítica / Alta / Media / Baja"). Referencia: Crítica → Must; Alta → Must o Should; Media → Should o Could; Baja → Could.  
  2. **Dependencias:** una HU de la que dependen otras (por ejemplo, inicio de sesión, creación de proyectos, disponibilidad) sube a **Must** aunque su prioridad SRS sea menor.  
  3. Valor para que el MVP 1 sea utilizable de extremo a extremo (autenticación → proyecto → disponibilidad → reunión).  
  4. Regla de negocio explícita del cliente (registro solo de Gerente, no crear Administradores, solo el creador cancela una reunión).  
* **Won't** solo si la HU claramente no es necesaria para el MVP 1; justifícalo.  
* Cada prioridad lleva **justificación de una frase** que cite el criterio usado.  
* Como control de calidad: en un MVP, **Must no debería superar aproximadamente el 60 % de las HU**. Si lo superas, revisa y reporta.

## **6\. FORMATO DE SALIDA**

Entrega **una tabla en formato TSV (valores separados por tabulaciones)**, dentro de **un único bloque de código**, para pegarla directamente en la hoja **"MVP 1"** de Google Sheets. Primera fila \= encabezados. **Sin celdas combinadas, sin viñetas ni listas de Markdown (las únicas viñetas permitidas son • dentro de DoR y DoD), sin negritas, sin emojis en ninguna columna.** En particular, el campo Estado de validación va **sin emoji** (ver regla siguiente).

**Regla obligatoria del campo Estado de validación:** elimina cualquier emoji y escribe únicamente el texto. Por ejemplo, si el original dice '✅ Lista', escribe únicamente `Lista`. Los valores posibles son `Lista`, `Con observaciones` y `Pendiente de aclaración`. Esto es obligatorio para que el texto coincida exactamente con las opciones del menú desplegable (drop-down) configurado en Google Sheets. No agregues espacios al inicio ni al final, ni comillas, ni punto final.

**Estructura: una HU \= una fila de tipo "HU" seguida de sus filas de tipo "Tarea"** (esto permite filtrar en Sheets y luego importar a Jira como historia \+ subtareas).

**Columnas, en este orden exacto:**

| \# | Columna | Fila HU | Fila Tarea |
| ----- | ----- | ----- | ----- |
| 1 | Tipo | `HU` | `Tarea` |
| 2 | Id de la Historia | ID original | ID de la HU padre (repetido) |
| 3 | Id de Tarea | vacío | `TK_DN_xx_xx_n` |
| 4 | Épica | original | vacío |
| 5 | MVP | original | vacío |
| 6 | Sprint | original | vacío |
| 7 | Rol | original | vacío |
| 8 | Característica / Funcionalidad | original, literal (en la fila Tarea va el título de la tarea) | título de la tarea |
| 9 | Razón / Resultado | original, literal | vacío |
| 10 | Criterio de Aceptación | original, completo y literal, entre comillas dobles, un ítem numerado por línea | criterio(s) que cubre la tarea (ej. 2.3, 4\) |
| 11 | Estado de validación | original, sin emoji (solo el texto, ej. `Lista`) | vacío |
| 12 | Observación | original, literal | vacío |
| 13 | Tipo de tarea | vacío | Frontend / Backend / BD / QA / Seguridad / Documentación / DevOps |
| 14 | DoR | celda entre comillas dobles, una condición por línea con viñeta • | vacío |
| 15 | DoD | celda entre comillas dobles, una condición por línea con viñeta • | vacío |
| 16 | Estimación (SP Fibonacci) | número | vacío |
| 17 | Justificación de estimación | una frase | vacío |
| 18 | Prioridad MoSCoW | valor | vacío |
| 19 | Justificación de prioridad | una frase | vacío |
| 20 | Notas del analista | ambigüedades, dependencias entre sprints, sugerencia de división si SP ≥ 13; vacío si no hay | vacío |

Reglas de formato de celdas (obligatorias para que Google Sheets muestre listas estructuradas dentro de una misma casilla):

* **Criterio de Aceptación, DoR y DoD:** encierra **todo el contenido de la celda entre comillas dobles ("")**.  
* **Dentro de esas comillas dobles, usa saltos de línea reales (Enter)** para separar cada punto. **No uses diagonales (/) ni barras verticales (|) como separador.**  
* En el **Criterio de Aceptación** conserva la numeración original al inicio de cada línea (1., 2., 2.1, 2.2, …), un ítem por línea.  
* En **DoR** y **DoD** inicia cada línea con una viñeta estándar (•), una condición por línea.  
* **Comillas de los mensajes literales:** se conservan los mensajes con su texto exacto, pero dentro de una celda que ya está encerrada en comillas dobles debes escribirlos con **comillas simples (' ')** en lugar de comillas dobles o tipográficas (“ ”), para no romper el formato TSV. Ejemplo: `Éxito: 'Sesión cerrada correctamente.'`  
* Cualquier otra celda que contenga texto con saltos de línea o con comillas también se encierra entre comillas dobles y sigue las mismas reglas; las celdas de una sola línea y sin comillas no necesitan encerrarse.  
* Si una celda debe quedar vacía, déjala vacía (dos tabulaciones seguidas); no escribas "N/A".  
* Las tabulaciones solo se usan para separar columnas; nunca dentro del contenido de una celda.

## **7\. FORMA DE TRABAJO (para que no se trunque ni se pierda contenido)**

La salida es larga. Procesa en **3 entregas, una por sprint**, y **espera mi confirmación** ("continúa") entre cada una:

* **Entrega 1:** Sprint 1 (esperadas 20 HU).  
* **Entrega 2:** Sprint 2 (esperadas 13 HU). **No repitas encabezados** si te lo pido; por defecto sí repítelos para poder pegar cada entrega por separado.  
* **Entrega 3:** Sprint 3 (esperadas 11 HU) \+ el resumen final de la sección 8\.

Si una entrega no cabe en la respuesta, **córtala en una HU completa** (nunca a mitad de una HU), indícalo y continúa cuando yo lo pida.

## **8\. VALIDACIÓN FINAL OBLIGATORIA (antes de cerrar cada entrega)**

Al final de **cada entrega**, fuera del bloque de código, incluye un chequeo breve:

1. **Conteo:** HU recibidas en el sprint vs. HU entregadas. Deben coincidir. Lista los IDs entregados.  
2. **Integridad:** confirma que los elementos 1 a 10 de la sección 4 están completos y literales en cada HU. Si detectas que omitiste algo, corrígelo antes de entregar.  
3. **Cobertura de tareas:** confirma que cada categoría de criterio (Dónde, Campos, Validación, Restricción, Éxito, Fallo) de cada HU tiene al menos una tarea asociada.  
4. **Escala:** confirma que todas las estimaciones pertenecen a {1, 2, 3, 5, 8, 13, 21} y que todas las prioridades pertenecen a {Must, Should, Could, Won't}.  
5. **Alertas:** lista las HU con SP ≥ 13, las HU con dependencias hacia otros sprints (por ejemplo, las notificaciones que dependen del Sprint 9\) y cualquier ambigüedad detectada.

En la **Entrega 3**, agrega además un **resumen del MVP 1**: total de HU, total de tareas, suma de puntos por sprint y total, distribución MoSCoW (cantidad y porcentaje de Must / Should / Could / Won't) y el conteo final de HU contra las 44 esperadas.

## **9\. RESTRICCIONES**

* **No inventes** reglas, mensajes, campos, valores ni requisitos que no estén en el backlog.  
* **No modifiques** el contenido original de las HU ni sus IDs.  
* **No uses** la pestaña "Generado" ni conocimiento externo para cambiar el alcance de una HU.  
* **No dividas ni unas** HU. Si crees que una HU debería dividirse, solo sugiérelo en "Notas del analista".  
* **No asignes** responsables ni fechas (eso lo define el equipo).  
* Si algo es ambiguo, **pregunta o regístralo en "Notas del analista"** en lugar de suponer.

## **10\. EJEMPLO DE REFERENCIA (solo para ilustrar el formato; no lo incluyas en la salida)**

Para una HU simple como `HU_DN_01_04 — Cerrar mi sesión`:

* Fila HU: todos los campos originales copiados literalmente; DoR (condiciones generales \+ "HU\_DN\_01\_03 terminada (existe sesión que cerrar)"); DoD (generales \+ "tras cerrar sesión, una solicitud con la sesión previa es rechazada"); Estimación **2**; Prioridad **Must have** (Prioridad SRS Crítica; sin cierre de sesión el acceso no es seguro).  
* Filas Tarea (ejemplo): `TK_DN_01_04_1` Agregar la opción 'Cerrar sesión' al menú de usuario (Frontend, criterio 1\) · `TK_DN_01_04_2` Implementar el endpoint que invalida la sesión en el servidor (Backend, criterios 3, 4\) · `TK_DN_01_04_3` Limpiar la sesión del cliente y redirigir al inicio de sesión mostrando 'Sesión cerrada correctamente.' (Frontend, criterio 5\) · `TK_DN_01_04_4` Mostrar 'No fue posible completar la operación. Intente nuevamente.' ante error no anticipado (Frontend, criterio 6\) · `TK_DN_01_04_5` Escribir pruebas que verifiquen el rechazo de solicitudes con la sesión previa (QA, criterio 4).  
* Ejemplo de cómo debe verse la celda de Criterio de Aceptación (el contenido real va completo; aquí se abrevia):

```
"1. Dónde: opción 'Cerrar sesión' del menú de usuario, visible en todas las vistas autenticadas.
2. Campos: no aplica captura de datos; la acción no requiere formulario.
3. Validación: al activar la opción se invalida la sesión en el servidor y se limpia la sesión del cliente.
..."
```

* Ejemplo de la celda DoD:

```
"• Código revisado por pares y fusionado en la rama de desarrollo.
• Todos los criterios de aceptación verificados.
• Tras cerrar sesión, una solicitud con la sesión previa es rechazada.
..."
```

---

## **11\. INSTRUCCIÓN FINAL**

Confirma en una línea que entendiste las reglas y que vas a empezar. Luego cuenta cuántas HU del MVP 1 recibiste (debería ser 44), reporta el conteo por sprint y entrega la **Entrega 1 (Sprint 1\)**. Espera mi "continúa" antes de pasar a la siguiente.

**A continuación pego el backlog del MVP 1 (pestaña "Refinado"):**

\[PEGAR AQUÍ SOLO LOS SPRINTS 1, 2 Y 3 DE LA PESTAÑA "REFINADO"\]

