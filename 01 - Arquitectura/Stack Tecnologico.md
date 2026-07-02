---
tags: [arquitectura, stack, tecnologia]
status: activo
---

# Stack Tecnológico

## Backend

| Tecnología | Rol | Por qué |
|---|---|---|
| **FastAPI** | Framework HTTP | Async nativo, OpenAPI auto-generado, tipado con Pydantic |
| **PostgreSQL** | Base de datos | Multi-schema para tenants, robusto, ACID |
| **SQLAlchemy** | ORM | Soporte multi-schema, async, madurez |
| **Alembic** | Migraciones | Integrado con SQLAlchemy |
| **Redis** | Cache + cola | Rápido, soporte para Celery |
| **Celery** | Tareas async | Recordatorios, reportes, alertas de stock |
| **JWT** | Auth tokens | Stateless, compatible con multi-tenant |

## Frontend

| Tecnología | Rol | Por qué |
|---|---|---|
| **React 18** | UI framework | Ecosistema, equipo lo conoce |
| **TypeScript** | Tipado | Seguridad, autocompletado, refactor seguro |
| **Vite** | Bundler | Rápido, HMR nativo |
| **Tailwind CSS** | Estilos | Utilidad, sin CSS custom |
| **React Query** | Estado servidor | Cache, loading states, refetch automático |
| **Zustand** | Estado cliente | Simple, sin boilerplate de Redux |
| **React Router v6** | Ruteo | Estándar actual |
| **axios** | HTTP client | Interceptores para JWT y tenant |
| **socket.io-client** | WebSockets | KDS y actualizaciones en tiempo real |

## Servicios externos

| Servicio | Para qué | Alternativa |
|---|---|---|
| **Stripe** | Cobro de suscripciones | MercadoPago (LATAM) |
| **Resend** | Email transaccional | SendGrid |
| **Twilio / 360dialog** | WhatsApp Business | |
| **AWS S3 / Cloudflare R2** | Imágenes de productos | Local en desarrollo |

## Infraestructura

| | MVP | Escala |
|---|---|---|
| **Hosting** | Railway | DigitalOcean / Hetzner |
| **DB managed** | Railway Postgres | DigitalOcean managed Postgres |
| **Deploy** | GitHub Actions | GitHub Actions |
| **Proxy** | Railway built-in | Nginx |
| **SSL** | Railway built-in | Let's Encrypt |

Ver [[ADR-003 Railway para MVP]].
