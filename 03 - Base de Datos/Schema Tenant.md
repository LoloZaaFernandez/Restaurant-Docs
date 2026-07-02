---
tags: [base-de-datos, schema, postgresql, tenant]
status: activo
---

# Schema por Tenant

Cada restaurante tiene su propio schema PostgreSQL con esta estructura. El schema se crea al registrarse y se migra con Alembic.

## Tablas principales

### users
```sql
id         UUID PRIMARY KEY
email      VARCHAR(255) UNIQUE
password   VARCHAR(255)      -- bcrypt hash
name       VARCHAR(255)
role       ENUM(admin, manager, waiter, kitchen)
is_active  BOOLEAN
created_at TIMESTAMP
```

### categories / products
```sql
-- categories
id         UUID PRIMARY KEY
name       VARCHAR(255)
sort_order INTEGER
is_active  BOOLEAN

-- products
id           UUID PRIMARY KEY
category_id  UUID FK categories
name         VARCHAR(255)
description  TEXT
price        DECIMAL(10,2)
image_url    VARCHAR(500)
is_available BOOLEAN
```

### areas / tables
```sql
-- areas (salón, terraza, barra)
id    UUID PRIMARY KEY
name  VARCHAR(100)

-- tables
id         UUID PRIMARY KEY
area_id    UUID FK areas
number     INTEGER
capacity   INTEGER
status     ENUM(free, occupied, reserved)
position_x FLOAT
position_y FLOAT
```

### orders / order_items
```sql
-- orders
id         UUID PRIMARY KEY
table_id   UUID FK tables
waiter_id  UUID FK users
status     ENUM(open, in_progress, ready, closed, cancelled)
notes      TEXT
created_at TIMESTAMP
closed_at  TIMESTAMP

-- order_items
id         UUID PRIMARY KEY
order_id   UUID FK orders
product_id UUID FK products
variant_id UUID FK product_variants NULLABLE
quantity   INTEGER
unit_price DECIMAL(10,2)
status     ENUM(pending, in_kitchen, ready, served, cancelled)
notes      TEXT
```

### bills / payments
```sql
-- bills
id         UUID PRIMARY KEY
order_id   UUID FK orders
subtotal   DECIMAL(10,2)
tax        DECIMAL(10,2)
tip        DECIMAL(10,2)
discount   DECIMAL(10,2)
total      DECIMAL(10,2)
status     ENUM(pending, paid, cancelled, split)
created_at TIMESTAMP

-- payments
id         UUID PRIMARY KEY
bill_id    UUID FK bills
method     ENUM(cash, card, transfer, qr)
amount     DECIMAL(10,2)
reference  VARCHAR(255)
created_at TIMESTAMP
```

## Relaciones clave

```
areas ──< tables ──< orders ──< order_items >── products
                          │
                          └──< bills ──< payments
```

## Ver también

- [[Schema Public]] para tenants y planes
- [[Multi-tenant Strategy]] para cómo se aislan los schemas
