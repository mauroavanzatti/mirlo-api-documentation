# Consumo de la línea

Consulta el consumo de datos, voz y SMS de una línea en el período activo.

## Endpoint

```http
GET /line/usage/{msisdn}
```

## Parámetros de ruta

| Parámetro | Descripción |
| --------- | ----------- |
| `msisdn`  | Número de la línea (10 dígitos, sin código de país) |

## Ejemplo

```bash
curl {baseurl}/api/v1/line/usage/5512345678 \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": {
    "msisdn": "5512345678",
    "status": "active",
    "consumption": {
      "data": {
        "used_mb": 1024,
        "total_mb": 5120,
        "remaining_mb": 4096,
        "percent_used": 20
      },
      "voice": {
        "used_minutes": 32,
        "total_minutes": -1,
        "remaining_minutes": -1
      },
      "sms": {
        "used": 5,
        "total": -1,
        "remaining": -1
      }
    },
    "period": {
      "start": "2025-01-15T00:00:00Z",
      "end": "2025-02-15T00:00:00Z"
    }
  }
}
```

> Los valores `-1` en `total` y `remaining` indican que el recurso es **ilimitado**.
