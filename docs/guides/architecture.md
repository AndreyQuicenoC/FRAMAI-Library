# Arquitectura del sistema

> Este archivo debe completarse según la arquitectura real del proyecto.  
> Plantilla de referencia para ser actualizada por el Orquestador durante análisis de HUs.

## Stack tecnológico

### Backend
- **Lenguaje:** [Node.js / Python / Java / .NET / etc.]
- **Framework:** [Express / FastAPI / Spring Boot / etc.]
- **Base de datos:** [PostgreSQL / MySQL / MongoDB / etc.]
- **ORM:** [Prisma / SQLAlchemy / Hibernate / etc.]
- **Auth:** [JWT / OAuth / Passport / etc.]

### Frontend
- **Lenguaje:** [JavaScript / TypeScript]
- **Framework:** [React / Vue / Angular / Next.js / etc.]
- **Build tool:** [Vite / Webpack / etc.]
- **State management:** [Redux / Zustand / Context / etc.]
- **HTTP client:** [Axios / Fetch / etc.]

### Infraestructura
- **Hosting:** [AWS / Azure / Vercel / etc.]
- **CI/CD:** [GitHub Actions / GitLab CI / etc.]
- **Contenedores:** [Docker / Kubernetes]
- **Monitoreo:** [Sentry / Datadog / etc.]

## Arquitectura general

```
┌─────────────┐
│   Cliente   │
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

## Estructura de carpetas

```
project/
├── backend/
│   ├── src/
│   │   ├── api/          # Endpoints (routes + controllers)
│   │   ├── services/     # Lógica de negocio
│   │   ├── models/       # Modelos de datos
│   │   ├── middleware/   # Auth, validation, error handling
│   │   ├── config/       # Configuración
│   │   └── utils/        # Utilidades
│   ├── tests/
│   └── package.json
│
├── frontend/
│   ├── src/
│   │   ├── pages/        # Páginas/rutas
│   │   ├── components/   # Componentes reutilizables
│   │   ├── hooks/        # Custom hooks
│   │   ├── services/     # API clients
│   │   ├── store/        # State management
│   │   └── utils/
│   ├── tests/
│   └── package.json
│
└── docs/
```

## Patrones de diseño

### Backend
- **Arquitectura:** [Layered / Clean / Hexagonal]
- **Endpoints:** REST / GraphQL
- **Validación:** [Joi / Zod / class-validator]
- **Error handling:** Centralizado con middleware

### Frontend
- **Arquitectura:** Component-based
- **Routing:** [React Router / Vue Router]
- **Forms:** [React Hook Form / Formik]
- **Styling:** [CSS Modules / Tailwind / styled-components]

## Flujo de autenticación

1. Usuario envía credenciales → POST /auth/login
2. Backend valida → genera JWT
3. Frontend almacena token (httpOnly cookie o memory)
4. Requests subsecuentes incluyen token en header Authorization
5. Middleware verifica token en cada request protegido

## Flujo típico de una HU

```
1. Frontend: usuario interactúa con UI
2. Frontend: validación cliente (UX)
3. Frontend: POST/GET/PUT/DELETE /api/resource
4. Backend: middleware auth/authz
5. Backend: validación de entrada
6. Backend: lógica de negocio (service)
7. Backend: persistencia (model/repository)
8. Backend: response (200/201/400/401/500)
9. Frontend: manejo de respuesta (success/error)
10. Frontend: actualización UI (toast, redirect, etc.)
```

## Convenciones

### Naming
- Variables: camelCase
- Archivos: kebab-case
- Componentes: PascalCase
- Constantes: UPPER_SNAKE_CASE

### Commits
- Formato: `type(scope): description` (Conventional Commits)
- Types: feat, fix, docs, style, refactor, test, chore

### Tests
- Unitarios: junto a código fuente
- Integración: carpeta tests/
- E2E: carpeta e2e/

## Dependencias críticas

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

## Estructuras críticas protegidas

| Ruta | Tipo | Razón protección |
|---|---|---|
| `backend/src/middleware/auth.js` | Middleware | AuthN/AuthZ central |
| `backend/src/config/database.js` | Config | Conexión DB |
| `frontend/src/services/api.js` | Client | HTTP client configurado |
| `.env` / `.env.example` | Config | Variables de entorno |

---

**Nota:** Este archivo debe actualizarse cuando:
1. Se agregue/cambie tecnología mayor
2. Cambie patrón arquitectónico
3. Se identifiquen nuevas estructuras críticas
4. El Orquestador detecte desviación entre doc y realidad

Ver `docs/implementation-report.md` para historia de cambios arquitectónicos por HU.
