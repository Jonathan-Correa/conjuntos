# App — conjunapp-app

## Descripción

Cliente Flutter **ConjunApp Residentes** con autenticación, home, zonas sociales (catálogo/detalle + slots), facturas, reservas (reprogramar/comprobante), visitantes y comunicados. Disponible como Flutter Web en Compose (`:5174`) y como app nativa.

## Tecnologías

| Componente | Constraint / nota |
|------------|-------------------|
| Flutter / Dart | SDK `>=3.6.0 <4.0.0` |
| provider | estado auth |
| http | REST |
| flutter_secure_storage | token |
| intl | formato |

## Arquitectura

```
lib/
├── main.dart
└── src/
    ├── models/auth.dart
    ├── providers/auth_provider.dart
    ├── services/auth_service.dart, api_client.dart, resident_api.dart
    └── screens/ login, signup, home, common_areas, common_area_detail,
                 reservations, invoices, visitors, announcements
```

## Instalación

```bash
cd conjunapp-app
flutter pub get
flutter run --dart-define=API_BASE_URL=http://localhost:8000/api/v1
```

### Emulador Android

```bash
flutter run --dart-define=API_BASE_URL=http://10.0.2.2:8000/api/v1
```

## Variables / dart-define

| Define | Default | Descripción |
|--------|---------|-------------|
| `API_BASE_URL` | `http://localhost:8000/api/v1` | Base API |

No usa archivos `.env` de Flutter; el `.env.example` del repo documenta el dart-define.

## Docker / Web

Con Compose, el servicio `resident` publica Flutter Web en http://localhost:5174.

```bash
# desde la raíz del monorepo
docker compose up --build resident
```

Login demo: `ana@example.com` / `residente123`.

Build manual:

```bash
cd conjunapp-app
docker build -f Dockerfile.web -t conjunapp-app-web \
  --build-arg API_BASE_URL=http://localhost:8000/api/v1 .
```

## Plataformas

Android, iOS, Web, Windows, macOS, Linux (scaffolds Flutter). **Web vía Docker es la vía recomendada para demos sin móvil.**

## Features actuales

- Login / logout / registro residente
- Home con acciones rápidas
- Facturas (listado + pago simulado PSE)
- Reservas de zonas comunes (crear / cancelar)
- Visitantes (invitación + código QR texto)
- Comunicados / anuncios

## Troubleshooting

| Síntoma | Acción |
|---------|--------|
| Sin red en Android release | Añadir permiso `INTERNET` al manifest main |
| Cleartext HTTP bloqueado | HTTPS o permitir cleartext en debug |
| localhost no resuelve en emulador | Usar `10.0.2.2` |
| Test widget falla | Test obsoleto: falta `MultiProvider` |

## Convenciones

- Auth con `Provider` + `AuthService`.
- No mezclar el stub `ApiClient` de `main.dart` con el real de `services/`.
- Mantener package id distinto de `com.example.*` antes de release.
