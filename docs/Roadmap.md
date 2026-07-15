# Roadmap — ConjunApp

Estado actual: **MVP técnico** (auth dual, seed demo, admin con dashboard + tabs operativas, app residente web/móvil con facturas/reservas/visitantes/comunicados).

## Residentes

| Capacidad | Estado |
|-----------|--------|
| Auth login / registro | Funcional (API + app web/móvil) |
| Home / perfil | Funcional básico |
| Reservas (piscinas, salones, BBQ, canchas, gimnasios) | Operativo Fases 0–3: slots, aprobación, reprogramar, comprobante, completed |
| Pagos de administración | Operativo simulado (sin pasarela real) |
| PQRS | Pendiente |
| Correspondencia | Pendiente |
| Invitados / visitantes | Operativo en app (QR texto) |
| Vehículos | Modelo sin API |
| Mascotas | Modelo sin API |
| Documentos | Pendiente |
| Asambleas | Pendiente |
| Votaciones | Pendiente |
| Encuestas | Pendiente |
| Noticias / anuncios | Lectura en app; publicación desde admin UI |

## Administración

| Capacidad | Estado |
|-----------|--------|
| Gestión de conjuntos | Seed + modelo; UI limitada |
| Apartamentos / casas (unidades) | API listado |
| Zonas sociales / amenidades | CRUD + schedules/blackouts/special hours/docs; export CSV reservas; catálogo residente |
| Residentes / propietarios / arrendatarios | UI admin: listado, alta, paz y salvo |
| Cartera / facturación | UI admin: listado + generación por periodo |
| Contabilidad | Reporte API; sin UI aún |
| Personal | Pendiente |
| Proveedores | Pendiente |
| Presupuestos | Pendiente |
| Reportes avanzados | Pendiente |
| Dashboard | KPIs + tabs Residentes/Facturas/Reservas/Anuncios |

## Vigilancia

| Capacidad | Estado |
|-----------|--------|
| Control de acceso | Pendiente |
| Visitantes + QR | QR como string; sin validación de portería |
| Parqueaderos | Pendiente |
| Validación de residentes | Pendiente |
| PIN / RFID / biometría / controles remotos | Pendiente |

## Integraciones futuras

- Pasarelas de pago (PSE, tarjetas, etc.)
- WhatsApp, Email, SMS, Push
- Cámaras / IoT / reconocimiento facial
- Facturación electrónica (DIAN / proveedor)

## Hitos técnicos sugeridos

1. **Estabilizar MVP**: Alembic + tests; CI lint/typecheck.
2. **Seguridad**: roles (`is_super_admin`), rate limit, ocultar signup admin cuando el flag está off.
3. **Producto admin / Zonas Sociales**: Fases 0–3 hechas; Fase 4 puertos pagos/notificaciones/acceso; reporte contable UI.
4. **Producto app**: PQRS, documentos; vehículos/mascotas cuando exista API.
5. **Integraciones** de pago y notificaciones.
6. **Observabilidad y escala** según [DecisionesArquitectura.md](./DecisionesArquitectura.md).
