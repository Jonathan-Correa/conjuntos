# Flujo de autenticación — ConjunApp

## Modelo

Dos audiencias JWT independientes:

| Audiencia (`aud`) | Tabla | Cliente |
|-------------------|-------|---------|
| `admin` | `admin_users` | conjunapp-admin |
| `resident` | `resident_users` | conjunapp-app |

Algoritmo: HS256. Header: `Authorization: Bearer <token>`.

## Login admin

```mermaid
sequenceDiagram
  participant U as Browser Admin
  participant A as conjunapp-admin
  participant B as Backend
  participant D as PostgreSQL

  U->>A: email + password
  A->>B: POST /api/v1/auth/admin/login
  B->>D: buscar AdminUser
  B->>B: verify bcrypt + crear JWT aud=admin
  B-->>A: access_token + user
  A->>A: Zustand persist localStorage
  A->>B: requests con Bearer
```

## Login residente

```mermaid
sequenceDiagram
  participant U as App Flutter
  participant B as Backend
  participant D as PostgreSQL

  U->>B: POST /api/v1/auth/resident/login
  B->>D: ResidentUser
  B-->>U: access_token + user
  U->>U: FlutterSecureStorage
  U->>B: GET /auth/resident/me (al iniciar)
```

## Guards

- `get_current_admin`: valida JWT `aud=admin` y usuario activo.
- `get_current_resident`: valida JWT `aud=resident` y usuario activo.
- Flag `is_super_admin` existe en modelo pero **aún no se usa** en endpoints.

## Registro

| Endpoint | Estado | Control |
|----------|--------|---------|
| `POST /auth/admin/register` | Condicional | Requiere `ALLOW_ADMIN_REGISTER=true` |
| `POST /auth/resident/register` | Público | Requiere torre/unidad existentes |
| `POST /admin/residents` | Admin | Password vía `initial_password` (mín. 8) |

En `docker-compose.prod.yml`, `ALLOW_ADMIN_REGISTER` y `SEED_ON_STARTUP` default a `false`. Con `ENVIRONMENT=production` el API no arranca con JWT/DB inseguros.

## Almacenamiento cliente

| Cliente | Storage | Clave |
|---------|---------|-------|
| Admin | localStorage (Zustand persist) | `auth-store` |
| App | flutter_secure_storage | token + user JSON |

## Gaps conocidos

- Sin refresh tokens (la app borra `refresh_token` nunca guardado).
- Admin no revalida `/auth/admin/me` al cargar; solo mira flag local.
- Secret por defecto inseguro si no se configura `.env`.
- Expiración larga (12 h) por defecto.
