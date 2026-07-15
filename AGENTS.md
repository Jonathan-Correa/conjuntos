# AGENTS.md — Estándares permanentes ConjunApp

Instrucciones para agentes de IA y desarrolladores que modifiquen este monorepo.

## Alcance

Repositorios: `conjunapp-back`, `conjunapp-admin`, `conjunapp-app`, más raíz (`docs/`, Compose).

## Stack real (no asumir otro)

- Backend: **Python / FastAPI / SQLAlchemy / PostgreSQL**
- Admin: **React + Vite + TypeScript + Zustand**
- App: **Flutter / Dart + Provider**

No introducir NestJS, Angular ni cambios de stack sin decisión documentada en `docs/DecisionesArquitectura.md`.

## Forma de trabajo obligatoria

1. Analizar el código afectado
2. Documentar impacto en `/docs` si cambia arquitectura, env, Docker, auth o datos
3. Justificar el cambio (PR o comentario claro)
4. Implementar el cambio mínimo necesario
5. Validar (compose, tests, smoke manual)
6. Actualizar documentación

Nunca realizar cambios silenciosos de comportamiento o de contratos API.

## Documentación

- Mantener sincronizados: README raíz, README de cada repo, `/docs/*`, `.env.example`.
- Toda variable de entorno nueva debe aparecer en `docs/VariablesEntorno.md` y en los `.env.example`.
- Actualizar `docs/Pendientes.md` / `docs/Roadmap.md` cuando se complete o abra trabajo relevante.
- Registrar deuda nueva en `docs/DeudaTecnica.md`.

## Docker

- El flujo canónico es `docker compose up` desde la raíz.
- Preservar healthchecks, red `conjunapp-net` y volumen `pgdata`.
- No commitear secretos reales; solo `.env.example`.
- Admin producción: multi-stage + nginx. Dev: Vite HMR.
- Flutter no se asume dentro del compose diario; documentar `dart-define`.

## Backend

- Prefijo API estable: `/api/v1`.
- No romper contratos JSON sin versionar o avisar en docs.
- Preferir lifespan FastAPI sobre `@app.on_event` en código nuevo.
- No dejar secretos hardcodeados; leer de settings/env.
- Antes de alterar el schema en entornos compartidos, introducir/usar Alembic.
- No eliminar endpoints o modelos “huérfanos” sin justificar (pueden ser roadmap).

## Frontend admin

- Variables públicas solo con prefijo `VITE_`.
- Auth: JWT Bearer; no almacenar passwords.
- Mantener TypeScript `strict`.
- No añadir UI kits pesados sin necesidad.

## App Flutter

- Config de API vía `--dart-define=API_BASE_URL=...`.
- No reactivar el stub `ApiClient` de `main.dart` para features nuevas; usar `src/services/api_client.dart`.
- Antes de borrar `ResidentHomePage`, migrar o documentar la decisión.

## Seguridad

- No exponer registro privilegiado en producción (`ALLOW_ADMIN_REGISTER=false`, `ENVIRONMENT=production`).
- No loguear tokens ni passwords.
- Revisar CORS al añadir orígenes.
- Credenciales demo solo para desarrollo (`SEED_ON_STARTUP`).
- En producción el backend debe fallar al arrancar si `JWT_SECRET_KEY` o la DB usan defaults débiles.

## Commits y PRs

- Commits solo si el usuario lo pide.
- Mensajes centrados en el “por qué”.
- No incluir `.env`, credenciales ni artefactos de build.

## Idioma

- Documentación y comunicación del equipo: **español**.
- Código: identificadores en inglés (código existente).
