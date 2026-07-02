---
tags: [modulo, pedidos, kds, websocket, fase-3]
status: pendiente
fase: 3
prioridad: critica
---

# Pedidos y KDS

## Responsabilidad

Toma de pedidos en mesa, ciclo de vida de cada ítem, y pantalla de cocina en tiempo real (Kitchen Display System).

## Estados de un pedido

```
open → in_progress → ready → closed
                           → cancelled
```

## Estados de un ítem

```
pending → in_kitchen → ready → served
                     → cancelled
```

## Flujo completo

```
1. Mozo abre pedido en una mesa (POST /orders)
2. Agrega ítems del menú (POST /orders/{id}/items)
3. WebSocket emite evento al KDS → cocina ve el pedido
4. Cocina marca ítems como "listo" (PATCH /orders/{id}/items/{item_id}/status)
5. WebSocket notifica al mozo → sirve el plato
6. Al cerrar cuenta → pedido pasa a "closed" → mesa queda libre
```

## WebSockets

| Canal | Quién escucha | Eventos |
|---|---|---|
| `/ws/kitchen` | KDS en cocina | `order.created`, `item.added` |
| `/ws/tables` | Tablet de mozo | `item.ready`, `order.status_changed` |

## Endpoints

| Método | Ruta | Descripción |
|---|---|---|
| POST | `/orders` | Abrir pedido en mesa |
| GET | `/orders` | Listar pedidos activos |
| GET | `/orders/{id}` | Detalle con ítems |
| POST | `/orders/{id}/items` | Agregar ítem |
| PATCH | `/orders/{id}/items/{item_id}` | Editar ítem |
| DELETE | `/orders/{id}/items/{item_id}` | Quitar ítem |
| PATCH | `/orders/{id}/status` | Cambiar estado del pedido |
| PATCH | `/orders/{id}/items/{item_id}/status` | KDS: marcar ítem |

## Archivos clave

- `src/domain/entities/order.py`
- `src/domain/entities/order_item.py`
- `src/application/use_cases/orders/`
- `src/presentation/websockets/kitchen.py`
- `src/presentation/websockets/tables.py`
- Frontend: `src/presentation/pages/kitchen/KitchenDisplayPage.tsx`
- Frontend: `src/presentation/hooks/useKitchenSocket.ts`
