---
tags: [adr, decision, arquitectura, postgresql, multi-tenant]
status: aceptado
fecha: 2026-07-02
---

# ADR-001 — Multi-tenant por Schema PostgreSQL

## Estado

**Aceptado**

## Contexto

El sistema debe aislar los datos de cada restaurante. Hay tres estrategias principales:
1. **Base de datos separada por tenant** — máximo aislamiento, muy costoso de escalar
2. **Schema por tenant** — buen aislamiento, costo moderado
3. **tenant_id en cada tabla** — más simple, pero un bug puede exponer datos entre tenants

## Decisión

Usar **un schema PostgreSQL por tenant**.

Cada restaurante que se registra obtiene su propio schema (`CREATE SCHEMA {slug}`). Las migraciones de Alembic se corren en cada schema individualmente.

## Consecuencias

### Positivas
- Aislamiento real: un bug de filtrado no expone datos de otro restaurante
- Backup por cliente: `pg_dump -n acme` exporta solo ese restaurante
- Performance: índices por schema, sin filtros adicionales en cada query
- Auditoría simple: `SET search_path = acme; SELECT * FROM orders`

### Negativas
- Las migraciones deben correrse en todos los schemas (requiere automatización)
- Más schemas activos = más overhead de PostgreSQL (aceptable hasta ~1000 tenants)
- No se pueden hacer queries cross-tenant fácilmente (intencional)

## Alternativas descartadas

- **tenant_id en tablas**: desechado por riesgo de data leak si se olvida el filtro
- **DB separada**: descartado por costo operativo en etapa MVP
