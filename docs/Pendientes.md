# Pendientes — ConjunApp

Lista operativa de trabajo abierto. Complementa [Roadmap.md](./Roadmap.md) y [DeudaTecnica.md](./DeudaTecnica.md).

## Hecho recientemente

- [x] Gate de `POST /auth/admin/register` (`ALLOW_ADMIN_REGISTER`)
- [x] Fail-fast de secretos en `ENVIRONMENT=production`
- [x] Fix `MetricCard` + errores en `DashboardPage`
- [x] Permiso `INTERNET` en manifest main (Android)
- [x] Seed controlado por `SEED_ON_STARTUP`
- [x] Validación de pagos vs saldo
- [x] Password inicial explícito al crear residente (`initial_password`)
- [x] Flutter Web en Compose `:5174`
- [x] Limpiar stub `ApiClient` / `ResidentHomePage` legacy
- [x] App residente: facturas, reservas, visitantes, comunicados
- [x] Admin: tabs Residentes, Facturas, Reservas, Anuncios + paz y salvo
- [x] Zonas Sociales Fase 0: Alembic aditivo, servicio reservas, reject overlap, bloqueo mora, scoping `complex_id`
- [x] Zonas Sociales Fase 1: CRUD admin, schedules/blackouts/images, catálogo app
- [x] Zonas Sociales Fase 2: availability/slots, approve/reject/cancel admin, calendario reserva app

## Crítico / inmediato

- [x] Introducir Alembic (migración aditiva; `create_all` aún bootstrap en startup)
- [ ] Retirar por completo `create_all` (solo Alembic)

## Backend

- [ ] Modularizar `routes.py` por dominio
- [ ] Endpoints para `Vehicle` y `Pet`
- [ ] Usar `is_super_admin` en autorización
- [ ] Transacciones / asientos al facturar y pagar
- [x] Tests iniciales reservas (pytest)
- [ ] Sustituir `datetime.utcnow`
- [x] Zonas Sociales Fase 1: CRUD admin + schedules/blackouts + catálogo app
- [x] Zonas Sociales Fase 2: availability engine + approve/reject admin + booking calendar UI
- [ ] Zonas Sociales Fase 3: reschedule, reportes/export, completed, comprobantes

## Admin

- [x] Completar tabs (Residentes, Facturas, Reservas, Anuncios)
- [x] Usar endpoints ya tipados (`generateInvoices`, paz y salvo, etc.)
- [ ] ESLint, typecheck en CI
- [ ] Separar `dependencies` / `devDependencies`
- [ ] Unificar diseño visual login/signup
- [ ] Ocultar/deshabilitar `/signup` cuando el registro admin esté off
- [ ] UI de reporte contable (`/admin/accounting-report`)

## App Flutter

- [ ] Endpoints UI para vehículos / mascotas (cuando exista API)
- [ ] Refresh token o renovación explícita
- [ ] Cambiar application id `com.example.*`
- [ ] Documentar cleartext / network security por ambiente

## Infra / Docs

- [ ] CI (lint, test, build imágenes)
- [ ] Monitoreo / logs estructurados
- [ ] Mantener `/docs` actualizado en cada cambio relevante
