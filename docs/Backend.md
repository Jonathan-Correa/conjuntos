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
| PostgreSQL | 16 (Compose) |
| pydantic-settings | 2.7.1 |
| passlib + bcrypt | passwords |
| python-jose | JWT HS256 |

## Estructura

```
conjunapp-back/
├── Dockerfile
├── requirements.txt
├── .env.example
└── app/
    ├── main.py
    ├── core/config.py, security.py
    ├── db/session.py
    ├── models/domain.py
    ├── schemas/domain.py
    ├── api/auth.py, routes.py
    └── services/seed.py
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
| Catálogo | `GET /towers`, `/common-areas`, `/announcements` |
| Admin | `/admin/dashboard`, `/admin/units`, `/admin/residents`, facturas, anuncios, paz y salvo, contabilidad |
| Residente | reservas, facturas, pagos, visitantes |

Detalle de auth: [FlujoAutenticacion.md](./FlujoAutenticacion.md).

## Base de datos

PostgreSQL vía SQLAlchemy. Al arrancar:

1. `Base.metadata.create_all`
2. `seed_database` si no hay conjuntos

Modelo: [BaseDatos.md](./BaseDatos.md).

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
