# Deuda técnica — ConjunApp

Informe de hallazgos. **No eliminar código sin justificar** en PR/cambio.

## Resuelto (2026-07-15)

| Hallazgo | Resolución |
|----------|------------|
| Registro admin público | Flag `ALLOW_ADMIN_REGISTER` (off en prod compose) |
| JWT/DB defaults en producción | Fail-fast si `ENVIRONMENT=production` |
| Password inicial = documento | `ResidentCreate.initial_password` obligatorio |
| `MetricCard` / props dashboard | Contrato alineado (`title`, `trend`, `color`) |
| Spinner infinito en dashboard | `try/catch/finally` + UI de error/reintento |
| `resident_register` traga `HTTPException` | Se propagan 404 reales |
| Pagos sin tope vs saldo | Validación de saldo restante |
| Android release sin `INTERNET` | Permiso en `main/AndroidManifest.xml` |
| Startup legacy `on_event` | Migrado a `lifespan` |
| Seed siempre activo | `SEED_ON_STARTUP` |
| `.gitignore` sin `.env` (back) | Corregido |

## Abierto — seguridad

| Hallazgo | Ubicación | Severidad |
|----------|-----------|-----------|
| Defaults débiles aún válidos en `development` | `config.py` / `.env.example` | Media (intencional para demo) |
| Credenciales demo en UI login | admin LoginForm, app LoginScreen | Media |
| Seed con passwords conocidos | `seed.py` | Media (ok en demo) |

## Abierto — bugs / calidad

| Hallazgo | Repo | Severidad |
|----------|------|-----------|
| `is_super_admin` sin enforcement | back | Media |
| `datetime.utcnow` deprecado | back | Baja |
| Credenciales demo visibles en formularios de login | admin/app | Baja (demo) |

> **Resuelto (2026-07-15):** el dashboard admin llamaba `/invoices` y `/reservations` (rol residente) con JWT admin → `Sesion de residente invalida`. Ahora usa `/admin/invoices` y `/admin/reservations`.
>
> **Resuelto (2026-07-15):** eliminado stub `ApiClient` / `ResidentHomePage` legacy; home cableado a facturas, reservas, visitantes y comunicados.
>
> **Resuelto (2026-07-15):** tabs admin cableadas (Residentes, Facturas, Reservas, Anuncios) con alta de residente, generación de facturas, anuncios y paz y salvo.

## Código muerto / duplicado

- Modelos `Vehicle` / `Pet` sin endpoints.
- `AdminUser` tipado duplicado en store y api (admin).

## Dependencias / calidad

| Hallazgo | Repo |
|----------|------|
| Sin tests automatizados | back, admin |
| Vite/TS en `dependencies` (deberían ser dev) | admin |
| Sin Alembic | back |
| `python-multipart` sin uploads | back |

## Priorización sugerida

1. Alembic + tests.
2. CI (lint/typecheck/build).
3. RBAC con `is_super_admin`.
4. Vehículos/mascotas (API + UI).
