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
      "plan_name": "Plan Básico 5GB",
      "sim_type": "esim",
      "activated_at": "2025-01-15T12:00:00Z"
    }
  ],
  "meta": {
    "total": 45,
    "page": 1,
    "limit": 20
  }
}
```
