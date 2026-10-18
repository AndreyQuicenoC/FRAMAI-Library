# Plantilla — Corrección puntual (front / back / full-stack)

## Metadatos

| Campo | Valor |
|-------|--------|
| **Tipo** | `targeted-fix` |
| **Alcance** | `frontend-only` \| `backend-only` \| `full-stack` |
| **Complejidad** | `auto` (inferida) o `TRIVIAL` … `COMPLEX` |

## Entrada esperada

- Lista de correcciones UX, bugs o mejoras (no necesariamente HU).
- Repositorios afectados y rama base (`feature/ep-XX-…` o `develop`).
- Referencia visual opcional (p. ej. demo en `DashNode-Demo/`).
- Restricciones: no romper CI, migraciones seguras, mensajes literales de dominio.

## Flujo recomendado

1. **Orquestador** con `requested_scope` y modo lote si hay >10 ítems.
2. Clasificar complejidad (`auto`: contar archivos, auth, migraciones, contratos API).
3. **Backend** primero si hay contratos nuevos o reglas de seguridad.
4. **Frontend** alineado a demo (iconos Lucide, sin emojis en UI productiva).
5. **Integrador**: `make check` / `npm run check`, migración `alembic upgrade head`.
6. Commits separados por capa o por tema (`fix(ui): …`, `feat(auth): …`).

## Salida

- Resumen de ítems cerrados vs pendientes.
- Evidencia de pruebas y push a la rama acordada.
