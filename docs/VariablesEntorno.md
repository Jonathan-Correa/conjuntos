# Variables de entorno — ConjunApp

Toda variable usada por el sistema está documentada aquí. Plantillas: `.env.example` en la raíz y en cada repositorio.

## Raíz / Docker Compose

| Variable | Default (example) | Descripción |
|----------|-------------------|-------------|
| `POSTGRES_USER` | `postgres` | Usuario DB |
| `POSTGRES_PASSWORD` | `root` | Password DB (**cambiar en prod**) |
| `POSTGRES_DB` | `conjunapp` | Nombre de la base |
| `DATABASE_URL` | `postgresql+psycopg://postgres:root@db:5432/conjunapp` | URL SQLAlchemy (host `db` en red Docker) |
| `ENVIRONMENT` | `development` | `development` \| `production` |
| `JWT_SECRET_KEY` | `change-this-secret-in-production` | Secreto JWT |
| `JWT_ALGORITHM` | `HS256` | Algoritmo JWT |
| `JWT_ACCESS_TOKEN_EXPIRE_MINUTES` | `720` | Expiración en minutos |
| `CORS_ORIGINS` | `http://localhost:5173,http://localhost:8080` | Orígenes permitidos (CSV) |
| `APP_NAME` | `ConjunApp API` | Título OpenAPI |
| `ALLOW_ADMIN_REGISTER` | `true` (dev) / `false` (prod compose) | Habilita `POST /auth/admin/register` |
| `SEED_ON_STARTUP` | `true` (dev) / `false` (prod compose) | Ejecuta seed demo al arrancar |
| `VITE_API_BASE_URL` | `http://localhost:8000/api/v1` | Base API para el admin (build/runtime Vite) |

## conjunapp-back

Mapeo `Settings` (`app/core/config.py`):

| Env | Atributo Python | Default |
|-----|-----------------|---------|
| `APP_NAME` | `app_name` | `ConjunApp API` |
| `ENVIRONMENT` | `environment` | `development` |
| `DATABASE_URL` | `database_url` | `postgresql+psycopg://postgres:root@localhost:5432/conjunapp` |
| `CORS_ORIGINS` | `cors_origins` | `http://localhost:5173` |
| `JWT_SECRET_KEY` | `jwt_secret_key` | `change-this-secret-in-production` |
| `JWT_ALGORITHM` | `jwt_algorithm` | `HS256` |
| `JWT_ACCESS_TOKEN_EXPIRE_MINUTES` | `jwt_access_token_expire_minutes` | `720` |
| `ALLOW_ADMIN_REGISTER` | `allow_admin_register` | `true` |
| `SEED_ON_STARTUP` | `seed_on_startup` | `true` |

Con `ENVIRONMENT=production` el arranque **falla** si el JWT es el default o la `DATABASE_URL` usa passwords débiles (`root`, `password`, etc.).

Archivo local: `conjunapp-back/.env` (no versionado).

## conjunapp-admin

| Env | Default | Descripción |
|-----|---------|-------------|
| `VITE_API_BASE_URL` | `http://localhost:8000/api/v1` | Prefijo de todas las llamadas REST |

Solo variables con prefijo `VITE_` son visibles en el browser bundle.

## conjunapp-app

No usa `.env` de Flutter. Configuración vía `--dart-define`:

| Define | Default | Descripción |
|--------|---------|-------------|
| `API_BASE_URL` | `http://localhost:8000/api/v1` | Prefijo API |

Ejemplo:

```bash
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/api/v1
```

## Secretos y buenas prácticas

- Nunca commitear `.env` reales.
- En producción: `ENVIRONMENT=production`, password DB fuerte, `JWT_SECRET_KEY` aleatorio largo, `ALLOW_ADMIN_REGISTER=false`, `SEED_ON_STARTUP=false`, HTTPS, CORS restrictivo.
