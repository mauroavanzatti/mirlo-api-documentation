# Portabilidad

Porta un número existente de otro operador a Mirlo.

## Endpoint

```http
POST /orders
```

## Body

```json
{
  "type": "portability",
  "plan_id": "offer_01HZ...",
  "sim_type": "esim",
  "msisdn": "5512345678",
  "nip": "1234",
  "customer": {
    "name": "Juan Pérez",
    "email": "juan@example.com",
    "phone": "5512345678",
    "curp": "PERJ900101HDFRZN09"
  },
  "address": {
    "street": "Av. Insurgentes Sur 1234",
    "city": "Ciudad de México",
    "state": "CDMX",
    "zip": "03810"
  }
}
```

## Parámetros adicionales

| Campo    | Requerido | Descripción |
| -------- | --------- | ----------- |
| `type`   | Sí        | Debe ser `portability` |
| `msisdn` | Sí        | Número a portar (10 dígitos, sin código de país) |
| `nip`    | Sí        | NIP de portabilidad de 4 dígitos — el usuario lo obtiene enviando `NIP` al `051` desde su teléfono actual |

El resto de los campos (`plan_id`, `sim_type`, `customer`, `address`) son los mismos que en [Comprar línea / SIM](new-line.md).

## Estados de portabilidad

| Estado      | Descripción |
| ----------- | ----------- |
| `pending`   | Solicitud recibida |
| `submitted` | Enviada al operador donante |
| `scheduled` | Fecha de portabilidad confirmada |
| `ported`    | Portabilidad completada |
| `failed`    | Portabilidad rechazada por el operador |

## Respuesta

```json
{
  "status": 201,
  "data": {
    "order_id": "ord_01HZ...",
    "status": "submitted",
    "msisdn": "5512345678",
    "portability_date": "2025-01-17T10:00:00Z",
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```
