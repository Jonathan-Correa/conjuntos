# Docker — ConjunApp

## Objetivo

Un desarrollador debe poder clonar el monorepo y ejecutar:

```bash
cp .env.example .env
docker compose up --build
```

## Servicios

| Servicio | Imagen / build | Puerto host | Descripción |
|----------|----------------|-------------|-------------|
| `db` | `postgres:16-alpine` | 5432 | PostgreSQL |
| `back` | `./conjunapp-back` | 8000 | FastAPI |
| `admin` | `./conjunapp-admin` | 5173 (dev) / 8080 (prod) | Panel React |
| `resident` | `./conjunapp-app` (`Dockerfile.web`) | 5174 | App residentes (Flutter Web + nginx) |

## Archivos

| Archivo | Uso |
|---------|-----|
| `docker-compose.yml` | Base (db + back + admin + resident) |
| `docker-compose.dev.yml` | Overrides de desarrollo (hot reload admin/back) |
| `docker-compose.prod.yml` | Overrides de producción |
| `*/Dockerfile` / `Dockerfile.web` | Build de cada servicio |
| `*/.dockerignore` | Contexto mínimo |
| `.env.example` | Variables del compose |

## Redes y volúmenes

- Red bridge: `conjunapp-net`
- Volumen: `pgdata` para datos de PostgreSQL

## Healthchecks

- `db`: `pg_isready`
- `back`: HTTP `GET /api/v1/health`
- `admin` (prod) / `resident`: `wget` a nginx `/` o `/health`

`back` espera a `db` healthy; `admin` y `resident` esperan a `back` healthy.

## App residentes en el navegador

Tras `docker compose up --build`:

- URL: http://localhost:5174
- Login: `ana@example.com` / `residente123`

La primera compilación Flutter puede tardar varios minutos. `RESIDENT_API_BASE_URL` se embebe en build-time y debe ser alcanzable **desde el navegador** (`http://localhost:8000/api/v1`).

## Migraciones

Hoy no hay Alembic. El schema se crea en startup (`create_all`) y el seed corre si la DB está vacía.

## Comandos útiles

```bash
docker compose up --build
docker compose up --build resident   # solo reconstruir app residentes
docker compose logs -f resident
docker compose down -v
```

## Flutter nativo (opcional)

```bash
cd conjunapp-app
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/api/v1
```
