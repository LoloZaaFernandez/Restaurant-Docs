---
tags: [adr, decision, infraestructura, hosting, railway]
status: aceptado
fecha: 2026-07-02
---

# ADR-003 — Railway como hosting para el MVP

## Estado

**Aceptado** (revisar al superar 10 clientes activos)

## Contexto

Necesitamos un lugar donde desplegar Backend (FastAPI), Frontend (React), PostgreSQL y Redis sin configurar servidores desde cero. El MVP debe estar funcionando rápido.

## Decisión

Usar **Railway** para el MVP:
- PostgreSQL managed
- Redis managed
- Deploy automático desde GitHub (main branch)
- Variables de entorno por ambiente

## Criterio de migración

Cuando el costo mensual supere **$80 USD** o tengamos más de **10 restaurantes activos**, migrar a:
- **Hetzner** (VPS CPX31: 4 vCPU / 8GB RAM / €15/mes) + Docker Compose
- **DigitalOcean** si preferimos managed services

## Consecuencias

### Positivas
- Deploy en minutos, sin devops inicial
- PostgreSQL y Redis incluidos
- Preview environments por PR (útil para QA)
- Precio predecible al inicio (~$20-40/mes para MVP)

### Negativas
- Menos control que un VPS propio
- Precio escala más rápido que Hetzner a largo plazo
- No ideal para wildcard subdomains (requiere configuración extra)

## Ver también

- [[Stack Tecnologico]]
