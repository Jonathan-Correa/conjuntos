# Roadmap — ConjunApp

Estado actual: **MVP técnico** (auth dual, seed demo, dashboard admin parcial, app residente con login/home stub).

## Residentes

| Capacidad | Estado |
|-----------|--------|
| Auth login / registro | Funcional (API + app web/móvil) |
| Home / perfil | Funcional básico |
| Reservas (piscinas, salones, BBQ, canchas, gimnasios) | Operativo en app (API + UI) |
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
| Noticias / anuncios | Lectura operativa en app; create solo admin API |

## Administración

| Capacidad | Estado |
|-----------|--------|
| Gestión de conjuntos | Seed + modelo; UI limitada |
| Apartamentos / casas (unidades) | API listado |
| Residentes / propietarios / arrendatarios | API parcial; tipo owner/tenant en registro |
| Cartera / facturación | Generación y listado básico |
| Contabilidad | Reporte simple de asientos |
| Personal | Pendiente |
| Proveedores | Pendiente |
| Presupuestos | Pendiente |
| Reportes avanzados | Pendiente |
| Dashboard | KPIs básicos en admin |

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

1. **Estabilizar MVP**: arreglar bugs admin (MetricCard, error handling), cablear features residente, Alembic.
2. **Seguridad**: cerrar registro admin, secretos, roles, rate limit.
3. **Producto admin**: CRUD unidades, facturas, anuncios, paz y salvo.
4. **Producto app**: facturas, reservas, invitados operativos.
5. **Integraciones** de pago y notificaciones.
6. **Observabilidad y escala** según [DecisionesArquitectura.md](./DecisionesArquitectura.md).
