---
tags: [modulo, facturacion, pagos, stripe, fase-4]
status: pendiente
fase: 4
prioridad: critica
---

# Facturación y Pagos

## Responsabilidad

Generación de cuentas, registro de pagos (en efectivo, tarjeta, QR), split de cuenta y cierre de mesa.

## Flujo de cierre de cuenta

```
1. Mozo solicita la cuenta (POST /bills — genera snapshot de ítems)
2. Cliente elige método de pago
3. Se registra el pago (POST /bills/{id}/payments)
4. Si el total queda saldado → bill.status = "paid"
5. Se cierra el pedido → mesa queda libre
```

## Métodos de pago

- `cash` → efectivo (manual)
- `card` → tarjeta (integración con terminal física externa)
- `transfer` → transferencia bancaria
- `qr` → pago online via Stripe/MercadoPago

## Split bill

La cuenta se puede dividir entre N personas. Cada persona paga su parte. La cuenta no se cierra hasta que todos los splits estén pagados.

## Integración Stripe / MercadoPago

Para cobros QR online:
1. Backend crea PaymentIntent en Stripe
2. Frontend muestra QR o link de pago
3. Cliente paga desde su celular
4. Stripe envía webhook → backend confirma pago → cierra cuenta

## Endpoints

| Método | Ruta | Descripción |
|---|---|---|
| POST | `/bills` | Generar cuenta desde pedido |
| GET | `/bills/{id}` | Detalle de cuenta |
| POST | `/bills/{id}/payments` | Registrar pago |
| POST | `/bills/{id}/split` | Dividir entre N personas |
| PATCH | `/bills/{id}/discount` | Aplicar descuento |
| GET | `/bills/{id}/pdf` | Generar PDF de ticket |

## Archivos clave

- `src/domain/entities/bill.py`
- `src/application/use_cases/billing/`
- `src/infrastructure/external/stripe/stripe_gateway.py`
- Frontend: `src/presentation/pages/billing/BillingPage.tsx`
