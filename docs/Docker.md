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

La app Flutter (`conjunapp-app`) se desarrolla en host; opcionalmente build web con `Dockerfile.web`.

## Archivos

| Archivo | Uso |
|---------|-----|
| `docker-compose.yml` | Base (db + back + admin) |
| `docker-compose.dev.yml` | Overrides de desarrollo (hot reload, volúmenes) |
| `docker-compose.prod.yml` | Overrides de producción (admin nginx, sin reload) |
| `*/Dockerfile` | Build de cada servicio |
| `*/.dockerignore` | Contexto mínimo |
| `.env.example` | Variables del compose |

## Redes y volúmenes

- Red bridge: `conjunapp-net`
- Volumen: `pgdata` para datos de PostgreSQL

## Healthchecks

- `db`: `pg_isready`
- `back`: HTTP `GET /api/v1/health`
- `admin` (prod): `wget` a nginx

`back` espera a `db` healthy antes de arrancar.

## Migraciones

Hoy no hay Alembic. El schema se crea en startup (`create_all`) y el seed corre si la DB está vacía. Cuando se añada Alembic, el entrypoint de `back` deberá ejecutar `alembic upgrade head` antes de Uvicorn.

## Comandos útiles

```bash
# Desarrollo por defecto
docker compose up --build

# Dev explícito
docker compose -f docker-compose.yml -f docker-compose.dev.yml up --build

# Producción local
docker compose -f docker-compose.yml -f docker-compose.prod.yml up --build -d

# Solo infraestructura
docker compose up db

# Logs
docker compose logs -f back

# Rebuild limpio
docker compose down -v
docker compose up --build
```

## Flutter

```bash
cd conjunapp-app
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/api/v1
```

Con back en Docker, desde emulador Android el host es `10.0.2.2`.

## Notas de optimización

- Backend: imagen slim Python 3.12, usuario no-root, healthcheck.
- Admin prod: multi-stage Node build → nginx alpine.
- Admin dev: `npm run dev -- --host 0.0.0.0` con bind mounts.
