---
tags: [base-de-datos, schema, postgresql, multi-tenant]
status: activo
---

# Schema Public — Gestión SaaS

El schema `public` pertenece a la plataforma, no a ningún restaurante. Acá vive todo lo relacionado con la gestión de tenants y suscripciones.

## Tablas

### tenants
```sql
id          UUID PRIMARY KEY
name        VARCHAR(255)       -- "Restaurante El Buen Sabor"
slug        VARCHAR(100) UNIQUE -- "el-buen-sabor" (define subdominio)
plan_id     UUID FK plans
status      ENUM(active, suspended, cancelled)
created_at  TIMESTAMP
```

### plans
```sql
id            UUID PRIMARY KEY
name          VARCHAR(100)   -- "Starter", "Pro", "Enterprise"
price         DECIMAL(10,2)
max_tables    INTEGER
max_users     INTEGER
max_products  INTEGER
features      JSONB          -- {"inventory": true, "reports": true}
```

### subscriptions
```sql
id                    UUID PRIMARY KEY
tenant_id             UUID FK tenants
plan_id               UUID FK plans
stripe_subscription_id VARCHAR(255)
status                ENUM(active, past_due, cancelled, trialing)
current_period_end    TIMESTAMP
```

### invoices
```sql
id                UUID PRIMARY KEY
tenant_id         UUID FK tenants
amount            DECIMAL(10,2)
status            ENUM(paid, open, void)
stripe_invoice_id VARCHAR(255)
created_at        TIMESTAMP
```

## Notas

- El `slug` del tenant es el subdominio: `slug.tuapp.com`
- Cuando `subscriptions.status = past_due` → el middleware bloquea el acceso al tenant
- Ver [[Multi-tenant Strategy]] para el flujo de provisioning
