# Base de datos — ConjunApp

## Motor

- **PostgreSQL 16** (Compose)
- Driver: `psycopg` 3 vía SQLAlchemy URL `postgresql+psycopg://...`
- ORM: SQLAlchemy 2 (`Mapped` / `mapped_column`)
- Migraciones: **ninguna** (solo `Base.metadata.create_all` en startup)

## Diagrama ER simplificado

```mermaid
erDiagram
  ResidentialComplex ||--o{ Tower : tiene
  Tower ||--o{ Unit : tiene
  Unit ||--o{ Resident : ocupa
  ResidentUser ||--|| Resident : perfil
  Unit ||--o{ Vehicle : tiene
  Unit ||--o{ Pet : tiene
  Unit ||--o{ Invoice : genera
  Invoice ||--o{ InvoiceItem : detalle
  Invoice ||--o{ Payment : recibe
  Unit ||--o{ PaymentAgreement : acuerda
  Resident ||--o{ Reservation : reserva
  CommonArea ||--o{ Reservation : zona
  Resident ||--o{ VisitorInvitation : invita
  ResidentialComplex ||--o{ Announcement : publica
  Unit ||--o{ PeaceClearance : emite
  ResidentialComplex ||--o{ AccountingEntry : asientos
  AdminUser }o--|| ResidentialComplex : administra
```

## Tablas principales

| Tabla | Modelo | Notas |
|-------|--------|-------|
| `residential_complexes` | ResidentialComplex | Conjunto |
| `towers` | Tower | Unique (complex, name) |
| `units` | Unit | Unique (tower, number); cuota, coeficiente |
| `admin_users` | AdminUser | Auth admin |
| `resident_users` | ResidentUser | Auth residente |
| `residents` | Resident | 1:1 user; FK unit |
| `vehicles` | Vehicle | Sin API aún |
| `pets` | Pet | Sin API aún |
| `common_areas` | CommonArea | Zonas comunes |
| `reservations` | Reservation | Enum status |
| `invoices` / `invoice_items` | Invoice* | Facturación |
| `payments` | Payment | Pagos |
| `payment_agreements` | PaymentAgreement | Acuerdos |
| `visitor_invitations` | VisitorInvitation | QR string |
| `announcements` | Announcement | Comunicados |
| `peace_clearances` | PeaceClearance | Paz y salvo |
| `accounting_entries` | AccountingEntry | Contabilidad simple |

## Seed

Archivo: `conjunapp-back/app/services/seed.py`

Si no existe ningún `ResidentialComplex`, crea:

- Conjunto “Reserva del Sol” (Bogotá)
- Torres A/B y unidades demo
- Admin `admin@conjunapp.com` / `admin123`
- Residentes `ana@example.com` y `carlos@example.com` / `residente123`
- Áreas comunes, facturas, pago, anuncios y asientos

## URL por defecto

```
postgresql+psycopg://postgres:root@localhost:5432/conjunapp
```

En Compose el host es el servicio `db`.

## Evolución recomendada

1. Introducir **Alembic** antes de alterar tablas en entornos compartidos.
2. Separar modelos por dominio.
3. Índices explícitos en búsquedas frecuentes (email, periodo factura, status).
