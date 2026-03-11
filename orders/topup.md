# Recargas / Topup

Recarga o cambia el plan de una línea activa.

## Endpoint

```http
POST /orders
```

## Body

```json
{
  "type": "topup",
  "msisdn": "5512345678",
  "plan_id": "offer_01HZ..."
}
```

## Parámetros

| Campo     | Requerido | Tipo   | Descripción |
| --------- | --------- | ------ | ----------- |
| `type`    | Sí        | string | Siempre `topup` |
| `msisdn`  | Sí        | string | Número a recargar |
| `plan_id` | Sí        | string | ID del plan a aplicar |

## Respuesta

```json
{
  "status": 201,
  "data": {
    "order_id": "ord_01HZ...",
    "status": "completed",
    "msisdn": "5512345678",
    "plan": {
      "id": "offer_01HZ...",
      "name": "Plan Básico 5GB"
    },
    "renewal_date": "2025-02-15T00:00:00Z",
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```
