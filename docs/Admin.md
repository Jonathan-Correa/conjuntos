# Admin — conjunapp-admin

## Descripción

Panel web para administradores del conjunto: login, registro (si está permitido), dashboard con KPIs y pantallas operativas de residentes, facturas, reservas, anuncios y paz y salvo.

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
├── main.tsx, App.tsx, styles.css, vite-env.d.ts
├── lib/api.ts
├── store/auth.ts
├── hooks/useAuth.ts
├── pages/
│   ├── LoginPage, AdminSignupPage, DashboardPage
│   ├── ResidentsPage (alta + paz y salvo)
│   ├── InvoicesPage (listado + generate)
│   ├── ReservationsPage (solo lectura)
│   └── AnnouncementsPage (publicar)
└── components/ AdminLayout, ProtectedRoute, forms, MetricCard
```

Rutas (protegidas salvo login/signup):

| Ruta | Pantalla |
|------|----------|
| `/` | Login |
| `/signup` | Registro admin |
| `/dashboard` | KPIs |
| `/dashboard/residents` | Residentes + paz y salvo |
| `/dashboard/invoices` | Facturas |
| `/dashboard/common-areas` | Zonas Sociales (CRUD + horarios + bloqueos + imágenes URL) |
| `/dashboard/reservations` | Reservas (filtros, aprobar/rechazar/cancelar, nombre residente) |
| `/dashboard/announcements` | Anuncios |

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
| Error en dashboard / listados | Ver mensaje en UI; revisar red y JWT admin |
| CORS | Revisar `CORS_ORIGINS` en back |
| Login falla | Verificar seed y URL en `.env` |
| Paz y salvo deshabilitado | Unidad morosa (saldo pendiente); API responde 409 |
| Alta residente falla | `initial_password` mínimo 8 caracteres |

## Convenciones

- Token en `localStorage` vía Zustand persist (`auth-store`).
- Cliente HTTP nativo (`fetch`) en `lib/api.ts`.
- Shell común: `AdminLayout` con `NavLink` por sección.
- UI en español.
