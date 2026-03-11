# Consultar línea

Obtiene el estado y datos de una línea por su MSISDN.

## Endpoint

```http
GET /line/{msisdn}
```

## Parámetros de ruta

| Parámetro | Descripción |
| --------- | ----------- |
| `msisdn`  | Número de la línea (10 dígitos, sin código de país) |

## Ejemplo

```bash
curl https://api.mirlo.mx/api/v1/line/5512345678 \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": {
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
  }
}
```

## Estados de línea

| Estado      | Descripción |
| ----------- | ----------- |
| `active`    | Línea activa y en servicio |
| `suspended` | Línea suspendida temporalmente |
| `cancelled` | Línea cancelada |
| `pending`   | Pendiente de activación |
