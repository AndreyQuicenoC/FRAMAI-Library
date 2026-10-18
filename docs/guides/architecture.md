# System architecture

> This file must be completed according to the real architecture of the project.  
> Reference template to be updated by the Orchestrator during user-story analysis.

## Technology stack

### Backend
- **Language:** [Node.js / Python / Java / .NET / etc.]
- **Framework:** [Express / FastAPI / Spring Boot / etc.]
- **Database:** [PostgreSQL / MySQL / MongoDB / etc.]
- **ORM:** [Prisma / SQLAlchemy / Hibernate / etc.]
- **Auth:** [JWT / OAuth / Passport / etc.]

### Frontend
- **Language:** [JavaScript / TypeScript]
- **Framework:** [React / Vue / Angular / Next.js / etc.]
- **Build tool:** [Vite / Webpack / etc.]
- **State management:** [Redux / Zustand / Context / etc.]
- **HTTP client:** [Axios / Fetch / etc.]

### Infrastructure
- **Hosting:** [AWS / Azure / Vercel / etc.]
- **CI/CD:** [GitHub Actions / GitLab CI / etc.]
- **Containers:** [Docker / Kubernetes]
- **Monitoring:** [Sentry / Datadog / etc.]

## General architecture

```
┌─────────────┐
│   Client    │
│  (Browser)  │
└──────┬──────┘
       │ HTTP/HTTPS
       ▼
┌─────────────┐
│  Frontend   │
│   (React)   │
└──────┬──────┘
       │ REST API
       ▼
┌─────────────┐
│   Backend   │
│  (Express)  │
└──────┬──────┘
       │ SQL
       ▼
┌─────────────┐
│  Database   │
│ (Postgres)  │
└─────────────┘
```

## Folder structure

```
project/
├── backend/
│   ├── src/
│   │   ├── api/          # Endpoints (routes + controllers)
│   │   ├── services/     # Business logic
│   │   ├── models/       # Data models
│   │   ├── middleware/   # Auth, validation, error handling
│   │   ├── config/       # Configuration
│   │   └── utils/        # Utilities
│   ├── tests/
│   └── package.json
│
├── frontend/
│   ├── src/
│   │   ├── pages/        # Pages/routes
│   │   ├── components/   # Reusable components
│   │   ├── hooks/        # Custom hooks
│   │   ├── services/     # API clients
│   │   ├── store/        # State management
│   │   └── utils/
│   ├── tests/
│   └── package.json
│
└── docs/
```

## Design patterns

### Backend
- **Architecture:** [Layered / Clean / Hexagonal]
- **Endpoints:** REST / GraphQL
- **Validation:** [Joi / Zod / class-validator]
- **Error handling:** Centralized with middleware

### Frontend
- **Architecture:** Component-based
- **Routing:** [React Router / Vue Router]
- **Forms:** [React Hook Form / Formik]
- **Styling:** [CSS Modules / Tailwind / styled-components]

## Authentication flow

1. The user sends credentials → POST /auth/login
2. The backend validates → generates a JWT
3. The frontend stores the token (httpOnly cookie or memory)
4. Subsequent requests include the token in the Authorization header
5. Middleware verifies the token on every protected request

## Typical flow of a user story

```
1. Frontend: the user interacts with the UI
2. Frontend: client validation (UX)
3. Frontend: POST/GET/PUT/DELETE /api/resource
4. Backend: auth/authz middleware
5. Backend: input validation
6. Backend: business logic (service)
7. Backend: persistence (model/repository)
8. Backend: response (200/201/400/401/500)
9. Frontend: response handling (success/error)
10. Frontend: UI update (toast, redirect, etc.)
```

## Conventions

### Naming
- Variables: camelCase
- Files: kebab-case
- Components: PascalCase
- Constants: UPPER_SNAKE_CASE

### Commits
- Format: `type(scope): description` (Conventional Commits)
- Types: feat, fix, docs, style, refactor, test, chore

### Tests
- Unit: next to the source code
- Integration: tests/ folder
- E2E: e2e/ folder

## Critical dependencies

### Backend
- express
- prisma / sequelize / typeorm
- jsonwebtoken
- bcrypt
- joi / zod

### Frontend
- react / vue / angular
- react-router-dom
- axios
- react-hook-form

## Protected critical structures

| Path | Type | Protection reason |
|---|---|---|
| `backend/src/middleware/auth.js` | Middleware | Central AuthN/AuthZ |
| `backend/src/config/database.js` | Config | DB connection |
| `frontend/src/services/api.js` | Client | Configured HTTP client |
| `.env` / `.env.example` | Config | Environment variables |

---

**Note:** This file must be updated when:
1. A major technology is added/changed
2. The architectural pattern changes
3. New critical structures are identified
4. The Orchestrator detects a deviation between the doc and reality

See `docs/implementation-report.md` for the history of architectural changes per user story.
