---
tags: [adr, decision, arquitectura, clean-architecture]
status: aceptado
fecha: 2026-07-02
---

# ADR-002 — Clean Architecture como estándar del proyecto

## Estado

**Aceptado**

## Contexto

El proyecto es un SaaS complejo con múltiples módulos (auth, pedidos, facturación, inventario, etc.). Necesitamos una arquitectura que:
- Permita testear la lógica de negocio sin levantar FastAPI ni la DB
- Que sea extensible sin romper lo que ya funciona
- Que el equipo pueda entender dónde va cada cosa sin ambigüedad

## Decisión

Adoptar **Clean Architecture** de Robert C. Martin con cuatro capas en Backend y Frontend:

1. `domain` — entidades y reglas de negocio puras
2. `application` — use cases que orquestan el dominio
3. `infrastructure` — implementaciones concretas (DB, APIs externas)
4. `presentation` — interfaz con el mundo exterior (HTTP, WebSockets, UI)

**Regla de dependencia**: las capas internas nunca conocen a las externas.

## Consecuencias

### Positivas
- Los use cases son 100% testeables sin DB ni framework
- Cambiar de FastAPI a otro framework = solo tocar `presentation/`
- Cambiar de PostgreSQL a otro motor = solo tocar `infrastructure/`
- Onboarding de nuevos devs más rápido — siempre saben dónde buscar

### Negativas
- Más archivos y carpetas que un enfoque MVC simple
- Curva de aprendizaje si el equipo no conoce el patrón
- Riesgo de "anemia de dominio" si los use cases hacen todo y las entidades quedan vacías

## Ver también

- [[Clean Architecture]] — documentación detallada de cómo aplicamos el patrón
