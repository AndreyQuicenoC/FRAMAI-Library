# Template — Targeted correction (front / back / full-stack)

## Metadata

| Field | Value |
|-------|--------|
| **Type** | `targeted-fix` |
| **Scope** | `frontend-only` \| `backend-only` \| `full-stack` |
| **Complexity** | `auto` (inferred) or `TRIVIAL` … `COMPLEX` |

## Expected input

- List of UX corrections, bugs, or improvements (not necessarily a user story).
- Affected repositories and base branch (`feature/ep-XX-…` or `develop`).
- Optional visual reference (e.g. demo in `DashNode-Demo/`).
- Constraints: do not break CI, safe migrations, literal domain messages.

## Recommended flow

1. **Orchestrator** with `requested_scope` and batch mode if there are >10 items.
2. Classify complexity (`auto`: count files, auth, migrations, API contracts).
3. **Backend** first if there are new contracts or security rules.
4. **Frontend** aligned to the demo (Lucide icons, no emojis in production UI).
5. **Integrator**: `make check` / `npm run check`, migration `alembic upgrade head`.
6. Separate commits per layer or per theme (`fix(ui): …`, `feat(auth): …`).

## Output

- Summary of closed items vs pending.
- Test evidence and push to the agreed branch.
