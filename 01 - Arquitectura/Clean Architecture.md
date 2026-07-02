---
tags: [arquitectura, clean-architecture]
status: activo
decision: [[ADR-002 Clean Architecture]]
---

# Clean Architecture

## Por qué la elegimos

Ver [[ADR-002 Clean Architecture]].

## Las 4 capas

```
┌─────────────────────────────────────┐
│           PRESENTATION              │  FastAPI routers / React pages
│  ┌───────────────────────────────┐  │
│  │       INFRASTRUCTURE         │  │  SQLAlchemy / axios / Redis
│  │  ┌─────────────────────────┐ │  │
│  │  │      APPLICATION        │ │  │  Use Cases / DTOs
│  │  │  ┌───────────────────┐  │ │  │
│  │  │  │      DOMAIN       │  │ │  │  Entities / Repositories (interfaces)
│  │  │  └───────────────────┘  │ │  │
│  │  └─────────────────────────┘ │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
```

**La regla de dependencia**: las flechas solo apuntan hacia adentro. Domain no conoce a nadie. Application solo conoce Domain. Nunca al revés.

## Qué va en cada capa

### Domain (núcleo)
- Entidades de negocio (`Order`, `Table`, `Bill`)
- Value objects (`Money`, `Email`)
- Interfaces de repositorios (`IOrderRepository`)
- Excepciones de dominio (`OrderAlreadyClosedError`)
- **Cero dependencias de frameworks**

### Application
- Use cases: una clase por operación (`OpenOrderUseCase`, `RegisterPaymentUseCase`)
- DTOs: contratos de entrada/salida entre capas
- Ports: interfaces de servicios externos (`IPaymentGateway`, `IEmailService`)

### Infrastructure
- Implementaciones de repositorios con SQLAlchemy
- Adaptadores externos: Stripe, WhatsApp, S3, Redis
- Tareas Celery

### Presentation
- **Backend**: routers FastAPI, schemas Pydantic, middleware, WebSockets
- **Frontend**: páginas React, componentes, hooks, stores Zustand

## Estructura de carpetas

Ver [[Backend/README]] y [[Frontend/README]] para la estructura completa.

## Ejemplo de flujo

```
POST /orders
  → TenantMiddleware (resolves tenant)
  → OrderRouter (validates OpenOrderSchema)
  → OpenOrderUseCase (business logic)
  → IOrderRepository (interface)
  → SqlOrderRepository (SQLAlchemy → PostgreSQL tenant schema)
```
