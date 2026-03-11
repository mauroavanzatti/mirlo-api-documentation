# Listar líneas

Obtiene todas las líneas asociadas a tu cuenta de partner.

## Endpoint

```http
GET /lines
```

## Parámetros de query

| Parámetro | Tipo    | Requerido | Descripción |
| --------- | ------- | --------- | ----------- |
| `status`  | string  | No        | Filtra por estado: `active`, `suspended`, `cancelled` |
| `page`    | integer | No        | Número de página (default: `1`) |
| `limit`   | integer | No        | Resultados por página (default: `20`, max: `100`) |

## Ejemplo

```bash
curl "https://api.mirlo.mx/api/v1/lines?status=active&page=1&limit=50" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": [
    {
      "msisdn": "5512345678",
      "status": "active",
      "plan": {
        "id": "offer_01HZ...",
        "name": "Plan Básico 5GB",
        "renewal_date": "2025-02-15T00:00:00Z"
      },
      "sim_type": "esim",
      "iccid": "8952140061234567890",
      "activated_at": "2025-01-15T12:00:00Z"
    },
    {
      "msisdn": "5598765432",
      "status": "active",
      "plan": {
        "id": "offer_02HZ...",
        "name": "Plan Pro 20GB",
        "renewal_date": "2025-02-20T00:00:00Z"
      },
      "sim_type": "sim",
      "iccid": "8952140069876543210",
      "activated_at": "2025-01-20T09:15:00Z"
    },
    {
      "msisdn": "5511223344",
      "status": "suspended",
      "plan": {
        "id": "offer_01HZ...",
        "name": "Plan Básico 5GB",
        "renewal_date": null
      },
      "sim_type": "sim",
      "iccid": "8952140061122334450",
      "activated_at": "2024-12-01T08:00:00Z"
    }
  ],
  "meta": {
    "total": 45,
    "page": 1,
    "limit": 20
  }
}
```

## Campos de la respuesta

| Campo              | Tipo   | Descripción |
| ------------------ | ------ | ----------- |
| `msisdn`           | string | Número de la línea |
| `status`           | string | Estado actual: `active`, `suspended`, `cancelled`, `pending` |
| `plan.id`          | string | ID del plan activo |
| `plan.name`        | string | Nombre del plan activo |
| `plan.renewal_date`| string | Fecha de renovación (`null` si suspendida o cancelada) |
| `sim_type`         | string | `sim` o `esim` |
| `iccid`            | string | Identificador de la SIM |
| `activated_at`     | string | Fecha de activación en ISO 8601 |
