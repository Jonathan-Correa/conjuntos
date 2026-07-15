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

## Crítico / inmediato

- [ ] Introducir Alembic y retirar dependencia exclusiva de `create_all`

## Backend

- [ ] Modularizar `routes.py` por dominio
- [ ] Endpoints para `Vehicle` y `Pet`
- [ ] Usar `is_super_admin` en autorización
- [ ] Transacciones / asientos al facturar y pagar
- [ ] Tests (pytest + httpx)
- [ ] Sustituir `datetime.utcnow`

## Admin

- [ ] Completar tabs (Residentes, Facturas, Reservas, Anuncios)
- [ ] Usar endpoints ya tipados (`generateInvoices`, paz y salvo, etc.)
- [ ] ESLint, typecheck en CI
- [ ] Separar `dependencies` / `devDependencies`
- [ ] Unificar diseño visual login/signup
- [ ] Ocultar/deshabilitar `/signup` cuando el registro admin esté off

## App Flutter

- [ ] Endpoints UI para vehículos / mascotas (cuando exista API)
- [ ] Refresh token o renovación explícita
- [ ] Cambiar application id `com.example.*`
- [ ] Documentar cleartext / network security por ambiente

## Infra / Docs

- [ ] CI (lint, test, build imágenes)
- [ ] Monitoreo / logs estructurados
- [ ] Mantener `/docs` actualizado en cada cambio relevante
