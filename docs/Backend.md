# Backend — conjunapp-back

## Descripción

API REST de ConjunApp. Expone autenticación dual (admin / residente), dashboard, unidades, residentes, facturación, reservas, visitantes, anuncios, paz y salvo y reporte contable básico.

## Tecnologías

| Componente | Versión |
|------------|---------|
| Python | 3.12 |
| FastAPI | 0.115.6 |
| Uvicorn | 0.34.0 |
| SQLAlchemy | 2.x |
| Alembic | migraciones |
| PostgreSQL | 16 (Compose) |
| pydantic-settings | 2.7.1 |
| passlib + bcrypt | passwords |
| python-jose | JWT HS256 |
| pytest | tests |

## Estructura

```
conjunapp-back/
├── Dockerfile
├── requirements.txt
├── alembic.ini
├── alembic/versions/
├── tests/
├── .env.example
└── app/
    ├── main.py
    ├── core/config.py, security.py
    ├── db/session.py, migrate.py
    ├── models/domain.py
    ├── schemas/domain.py
    ├── api/auth.py, routes.py
    └── services/seed.py, reservations.py
```

## Instalación local

```bash
cd conjunapp-back
python -m venv .venv
# Windows
.venv\Scripts\activate
pip install -r requirements.txt
cp .env.example .env
# Ajustar DATABASE_URL si hace falta
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

## Variables de entorno

Ver [VariablesEntorno.md](./VariablesEntorno.md) y `.env.example`.

## Endpoints principales

Prefijo: `/api/v1`. Documentación interactiva: `/docs`.

| Área | Ejemplos |
|------|----------|
| Health | `GET /health` |
| Auth admin | `POST /auth/admin/login`, `/register`, `GET /auth/admin/me` |
| Auth residente | `POST /auth/resident/login`, `/register`, `GET /auth/resident/me` |
| Catálogo | `GET /towers`, `/announcements` |
| Zonas sociales | `GET /common-areas` (+detail/availability); admin CRUD + schedules/blackouts/special-hours/images |
| Admin | dashboard, units, residents, facturas, anuncios, paz y salvo, contabilidad; reservas approve/reject/cancel/export; job maintenance |
| Residente | reservas (crear/pagar stub/reprogramar/cancelar/comprobante/access-pass/ical; rechaza solape y mora), facturas, pagos, visitantes |
| Integraciones (Fase 4) | Puertos `Payment` / `Notification` / `AccessControl` / `ImageStorage` / `Calendar` + adapters stub en `app/ports` + `app/adapters` |

Detalle de auth: [FlujoAutenticacion.md](./FlujoAutenticacion.md).

## Base de datos

PostgreSQL vía SQLAlchemy. Al arrancar:

1. `Base.metadata.create_all` (bootstrap)
2. `alembic upgrade head`
3. `seed_database` (crea demo o backfill `admin.complex_id`)

Modelo: [BaseDatos.md](./BaseDatos.md). Reservas: `app/services/reservations.py`.

## Docker

Imagen multi-stage / healthcheck documentados en [Docker.md](./Docker.md).

```bash
docker compose up back db
```

## Scripts

No hay Makefile ni CLI propia. Comandos útiles:

```bash
uvicorn app.main:app --reload --port 8000
pip install -r requirements.txt
```

## Despliegue

1. Definir secretos reales (`JWT_SECRET_KEY`, `DATABASE_URL`).
2. Deshabilitar o proteger registro admin abierto.
3. Usar `docker-compose.prod.yml`.
4. Introducir migraciones Alembic antes de cambios de schema en producción.

## Troubleshooting

| Síntoma | Causa probable | Acción |
|---------|----------------|--------|
| Error de conexión DB | PostgreSQL no listo / URL mala | Revisar `DATABASE_URL` y healthcheck de `db` |
| CORS en admin | Origen no listado | Ampliar `CORS_ORIGINS` |
| 401 | Token vencido o `aud` incorrecto | Re-login; verificar audiencia JWT |
| Seed no corre | Ya existe un conjunto | Vaciar DB o borrar filas de `residential_complexes` |

## Convenciones

- Prefijo API `/api/v1`.
- Schemas y modelos centralizados en `domain.py` (a modularizar).
- Passwords con bcrypt; JWT con claim `aud`.
- No commitear `.env` ni secretos.
