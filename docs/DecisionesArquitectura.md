# Decisiones de arquitectura — ConjunApp

## Decisiones actuales (as-is)

| Decisión | Motivo | Consecuencia |
|----------|--------|--------------|
| Monolito FastAPI | Velocidad de MVP | Acoplamiento en `routes.py` / `domain.py` |
| React + Vite para admin | SPA liviana, DX con HMR | Auth solo en cliente + JWT |
| Flutter para residentes | Multiplataforma móvil | Docker no cubre el ciclo móvil típico |
| JWT HS256 dual `aud` | Separar admin/residente sin OAuth | Sin roles finos ni refresh |
| SQLAlchemy `create_all` | Bootstrap rápido | Sin historial de migraciones |
| Seed en startup | Demo inmediata | Riesgo si se deja en prod |
| Un solo Postgres | Simplicidad | Punto único de fallo |

## Recomendaciones futuras (to-be) — no implementar aún

### Arquitectura modular

- Extraer módulos: `auth`, `billing`, `reservations`, `visitors`, `announcements`.
- Capas: routers → services → repositories → models.
- Mantener deploy monolítico hasta que el tamaño o los equipos justifiquen split.

### Microservicios

Solo cuando haya límites claros (pagos, notificaciones, acceso/vigilancia). Evitar microservicios prematuros.

### API Gateway

Introducir cuando haya más de un servicio o necesidad de rate limit / API keys unificadas (Kong, Traefik, AWS API GW).

### Cache

Redis para: sesiones/blacklist JWT, catálogo de torres/áreas, dashboards. Empezar por endpoints de lectura frecuente.

### Colas

Broker (Redis/Rabbit/SQS) para: emails, WhatsApp, generación masiva de facturas, webhooks de pago.

### Logging y observabilidad

- Logs JSON estructurados (correlation id).
- OpenTelemetry + traces.
- Métricas Prometheus + health/readiness.
- Error tracking (Sentry).

### Escalabilidad horizontal

- API stateless (ya casi lo es con JWT).
- Postgres con backups y read replicas si hace falta.
- Admin estático en CDN/nginx.
- Evitar estado en volumen de contenedores de app.

### Seguridad evolutiva

- OAuth2/OIDC opcional para admin.
- Roles RBAC + auditorías.
- Secrets manager (no env planos en prod).
- WAF / rate limiting en edge.

## Principio guía

**Modularizar el monolito primero; extraer servicios solo con métricas o equipos que lo justifiquen.**
