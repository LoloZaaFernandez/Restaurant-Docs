---
tags: [modulo, auth, tenancy, fase-1]
status: pendiente
fase: 1
prioridad: critica
---

# Auth & Tenancy

## Responsabilidad

Gestión de identidad, autenticación y aislamiento entre restaurantes (tenants).

## Roles del sistema

| Rol | Permisos |
|---|---|
| `superadmin` | Acceso total — gestión de todos los tenants |
| `admin` | Gestión completa de su restaurante |
| `manager` | Sin acceso a configuración de suscripción |
| `waiter` | Solo pedidos y mesas |
| `kitchen` | Solo vista KDS |

## Flujo de registro

```
1. Restaurante se registra (nombre, email, password, slug)
2. Se crea tenant en public.tenants
3. Se provisiona schema PostgreSQL
4. Se corren migraciones en el nuevo schema
5. Se crea usuario admin para ese tenant
6. Se redirige al onboarding de plan/pago
```

## Flujo de login

```
1. POST /auth/login con email + password
2. Se valida contra el tenant correcto (via subdomain/header)
3. Se retorna access_token (JWT, 15min) + refresh_token (7 días)
4. Frontend guarda en TokenStorage
5. axios interceptor agrega Authorization: Bearer {token} en cada request
```

## Endpoints

| Método | Ruta | Descripción |
|---|---|---|
| POST | `/auth/register` | Crear nuevo restaurante |
| POST | `/auth/login` | Login con email y password |
| POST | `/auth/refresh` | Renovar access token |
| POST | `/auth/logout` | Invalidar refresh token |
| GET | `/me` | Perfil del usuario autenticado |

## Archivos clave

- `src/domain/entities/user.py`
- `src/domain/entities/tenant.py`
- `src/application/use_cases/auth/`
- `src/presentation/api/v1/routers/auth.py`
- `src/presentation/api/v1/dependencies/tenant.py`
- `src/infrastructure/database/models/user_model.py`
