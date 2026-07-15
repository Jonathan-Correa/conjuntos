# Admin — conjunapp-admin

## Descripción

Panel web para administradores del conjunto: login, registro y dashboard con KPIs y listados parciales de residentes y facturas.

## Tecnologías

| Componente | Versión |
|------------|---------|
| React | 19 |
| Vite | 6 |
| TypeScript | 5.7 |
| React Router | 6 |
| Zustand | 4.5 |
| lucide-react | iconos |
| Node (Docker) | 20 |

## Arquitectura

```
src/
├── main.tsx, App.tsx, styles.css
├── lib/api.ts
├── store/auth.ts
├── hooks/useAuth.ts
├── pages/ Login, Signup, Dashboard
└── components/ ProtectedRoute, forms, MetricCard
```

Rutas: `/` login, `/signup` registro, `/dashboard` protegida.

## Instalación

```bash
cd conjunapp-admin
cp .env.example .env
npm install
npm run dev
```

Abre http://localhost:5173.

## Variables de entorno

| Variable | Default | Descripción |
|----------|---------|-------------|
| `VITE_API_BASE_URL` | `http://localhost:8000/api/v1` | Base de la API |

En Docker Compose se inyecta apuntando al servicio `back`.

## Ejecución con Docker

```bash
# Desde la raíz del monorepo
docker compose up admin
```

Dev: hot reload en puerto 5173. Prod: build estático + nginx (ver `docker-compose.prod.yml`).

## Scripts

| Script | Descripción |
|--------|-------------|
| `npm run dev` | Vite en :5173 |
| `npm run build` | Bundle a `dist/` |
| `npm run preview` | Preview del build |

## Despliegue

`VITE_*` se embebe en build-time. Rebuild la imagen al cambiar la URL de API.

## Troubleshooting

| Síntoma | Acción |
|---------|--------|
| Spinner infinito en dashboard | API caída o error sin catch; ver red y backend |
| CORS | Revisar `CORS_ORIGINS` en back |
| Login falla | Verificar seed y URL en `.env` |
| Métricas sin título | Bug conocido de props en `MetricCard` (ver DeudaTecnica) |

## Convenciones

- Token en `localStorage` vía Zustand persist (`auth-store`).
- Cliente HTTP nativo (`fetch`) en `lib/api.ts`.
- UI en español.
