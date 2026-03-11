# Number Portability

Port an existing number from another carrier to Mirlo.

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
  "verification_id": "ver_01HZ...",
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

## Additional parameters

| Field    | Required | Description |
| -------- | -------- | ----------- |
| `type`   | Yes      | Must be `portability` |
| `msisdn` | Yes      | Number to port (10 digits, no country code) |
| `nip`    | Yes      | 4-digit portability NIP — the user obtains it by sending `NIP` to `051` from their current phone |

All other fields (`plan_id`, `sim_type`, `verification_id`, `customer`, `address`) are the same as in [Purchase Line / SIM](new-line.md).

## Portability statuses

| Status      | Description |
| ----------- | ----------- |
| `pending`   | Request received |
| `submitted` | Sent to the donor carrier |
| `scheduled` | Portability date confirmed |
| `ported`    | Portability completed |
| `failed`    | Rejected by the donor carrier |

## Response

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
