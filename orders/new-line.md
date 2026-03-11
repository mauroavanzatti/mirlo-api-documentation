# Comprar línea / SIM

Crea una nueva línea móvil y genera una SIM (física o eSIM).

## Endpoint

```http
POST /orders
```

## Body

```json
{
  "type": "new_line",
  "plan_id": "offer_01HZ...",
  "sim_type": "esim",
  "customer": {
    "name": "Juan Pérez",
    "email": "juan@example.com",
    "phone": "5512345678",
    "curp": "PERJ900101HDFRZN09",
    "rfc": "PERJ900101AB1"
  },
  "address": {
    "street": "Av. Insurgentes Sur 1234",
    "city": "Ciudad de México",
    "state": "CDMX",
    "zip": "03810"
  }
}
```

## Parámetros

| Campo            | Requerido | Tipo   | Descripción |
| ---------------- | --------- | ------ | ----------- |
| `type`           | Sí        | string | Siempre `new_line` |
| `plan_id`        | Sí        | string | ID del plan del catálogo |
| `sim_type`       | Sí        | string | `sim` o `esim` |
| `customer.name`  | Sí        | string | Nombre completo del usuario |
| `customer.email` | Sí        | string | Email del usuario |
| `customer.phone` | Sí        | string | Teléfono de contacto (10 dígitos) |
| `customer.curp`  | Sí        | string | CURP (18 caracteres) |
| `customer.rfc`   | No        | string | RFC del usuario |
| `address.street` | Sí        | string | Calle y número |
| `address.city`   | Sí        | string | Ciudad |
| `address.state`  | Sí        | string | Estado |
| `address.zip`    | Sí        | string | Código postal (5 dígitos) |

## Respuesta

```json
{
  "status": 201,
  "data": {
    "order_id": "ord_01HZ...",
    "status": "pending",
    "msisdn": "5212345678901",
    "sim_type": "esim",
    "esim": {
      "iccid": "8952140061234567890",
      "qr_url": "https://cdn.mirlo.mx/esim/qr_01HZ.png",
      "lpa": "LPA:1$sm-v4-059-a.es.prod.ondemandconnectivity.com$ABC123"
    },
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```

> Para eSIM, muestra el `qr_url` al usuario final para que lo escanee desde **Ajustes → Móvil → Agregar plan**.
