---
tags: [arquitectura, multi-tenant, postgresql]
status: activo
decision: [[ADR-001 Multi-tenant por Schema]]
---

# Multi-tenant Strategy

## Decisión

Un schema PostgreSQL por tenant. Ver [[ADR-001 Multi-tenant por Schema]].

## Estructura de schemas

```
PostgreSQL
├── public (schema compartido)
│   ├── tenants          → id, name, slug, plan_id, status
│   ├── plans            → id, name, price, max_tables, max_users
│   └── subscriptions    → tenant_id, plan_id, status, ends_at
│
├── acme (schema del tenant "acme")
│   ├── users
│   ├── products
│   ├── categories
│   ├── tables
│   ├── orders
│   ├── order_items
│   ├── bills
│   ├── reservations
│   └── ingredients
│
└── otro_restaurante (schema del tenant "otro_restaurante")
    └── (misma estructura)
```

## Resolución del tenant por request

```
Request entrante
  → Subdominio: acme.tuapp.com → slug = "acme"
  → Header fallback: X-Tenant-ID: acme (para desarrollo local)
  → TenantMiddleware busca tenant en public.tenants
  → Setea search_path = acme en la sesión DB
  → Todos los queries del request corren en ese schema
```

## Provisioning de nuevo tenant

Al registrarse un restaurante nuevo:
1. Insertar en `public.tenants`
2. `CREATE SCHEMA {slug}`
3. Correr migraciones Alembic en el nuevo schema
4. El restaurante ya puede operar

## Ventajas de esta estrategia

- Aislamiento real: un bug no expone datos de otro tenant
- Backup por cliente: `pg_dump -n acme`
- Fácil de auditar: `SET search_path = acme; SELECT * FROM orders`

## Consideración importante

Las migraciones de Alembic deben correrse en TODOS los schemas cuando hay un cambio de schema. Hay una tarea Celery para esto.
