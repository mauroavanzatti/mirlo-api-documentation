# Purchase Line / SIM

Creates a new mobile line and generates a SIM (physical or eSIM).

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
  "verification_id": "ver_01HZ...",
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

## Parameters

| Field                | Required | Type   | Description |
| -------------------- | -------- | ------ | ----------- |
| `type`               | Yes      | string | Always `new_line` |
| `plan_id`            | Yes      | string | Plan ID from the catalog |
| `sim_type`           | Yes      | string | `sim` or `esim` |
| `verification_id`    | Yes      | string | Identity verification ID (see [Verification Flow](../identity/verification.md)) |
| `customer.name`      | Yes      | string | Full name as it appears on the ID document |
| `customer.email`     | Yes      | string | Customer email |
| `customer.phone`     | Yes      | string | Contact phone number (10 digits) |
| `customer.curp`      | Yes      | string | CURP (18 characters) |
| `customer.rfc`       | No       | string | RFC |
| `address.street`     | Yes      | string | Street and number |
| `address.city`       | Yes      | string | City |
| `address.state`      | Yes      | string | State |
| `address.zip`        | Yes      | string | ZIP code (5 digits) |

## Response

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

> For eSIM, show the `qr_url` to the end user to scan from **Settings → Mobile → Add Plan**.
