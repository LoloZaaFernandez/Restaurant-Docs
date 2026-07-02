---
tags: [adr, decision, stack, fastapi, react, postgresql]
status: aceptado
fecha: 2026-07-02
---

# ADR-004 — Stack: FastAPI + React + PostgreSQL

## Estado

**Aceptado**

## Contexto

Necesitamos elegir las tecnologías base del proyecto. El equipo tiene experiencia en Python para backend y JavaScript/TypeScript para frontend.

## Decisión

- **Backend**: FastAPI (Python)
- **Frontend**: React + TypeScript + Vite
- **Base de datos**: PostgreSQL

## Razonamiento

### FastAPI
- Async nativo → ideal para WebSockets del KDS
- OpenAPI/Swagger auto-generado → documentación gratis
- Pydantic para validación → tipos en runtime
- Más rápido que Django para APIs puras

### React + TypeScript
- TypeScript da seguridad de tipos cross-capa (domain → presentation)
- Ecosistema maduro para dashboards complejos
- React Query simplifica el estado del servidor enormemente

### PostgreSQL
- Multi-schema nativo → estrategia de multi-tenancy sin hacks
- JSONB para configuraciones flexibles (features por plan)
- Vistas materializadas para reportes sin impacto en operación

## Alternativas descartadas

- **Django**: demasiado opinionated, WebSockets más complejos
- **NestJS**: el equipo no tiene experiencia en Node para backend
- **MySQL**: no tiene soporte multi-schema nativo
- **MongoDB**: schema-less complica las migraciones en multi-tenant
