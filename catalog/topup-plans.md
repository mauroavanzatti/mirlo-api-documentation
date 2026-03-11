# Planes de recarga

Obtiene los planes disponibles para recargar una línea existente. Estos planes se usan exclusivamente en el endpoint [Recargas](../orders/topup.md).

## Diferencia con planes principales

| | Planes principales | Planes de recarga |
|-|---|---|
| Uso | Compra de línea nueva, portabilidad | Recargar línea activa |
| Endpoint de compra | `POST /orders` con `type: new_line` o `type: portability` | `POST /orders` con `type: topup` |
| Requiere datos del cliente | Sí (CURP, dirección) | No |

## Endpoint

```http
GET /offers/topup-plans
```

## Parámetros de query

| Parámetro | Tipo    | Requerido | Descripción |
| --------- | ------- | --------- | ----------- |
| `msisdn`  | string  | No        | Si se provee, filtra los planes compatibles con esa línea |
| `page`    | integer | No        | Número de página (default: `1`) |
| `limit`   | integer | No        | Resultados por página (default: `20`, max: `100`) |

## Ejemplo

```bash
# Todos los planes de recarga
curl https://api.mirlo.mx/api/v1/offers/topup-plans \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"

# Planes compatibles con una línea específica
curl "https://api.mirlo.mx/api/v1/offers/topup-plans?msisdn=5512345678" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": [
    {
      "id": "topup_01HZ...",
      "name": "Recarga 5GB — 30 días",
      "description": "5GB adicionales con vigencia de 30 días",
      "retail_price": 149.00,
      "currency": "MXN",
      "data_gb": 5,
      "validity_days": 30,
      "active": true
    },
    {
      "id": "topup_02HZ...",
      "name": "Recarga 10GB — 30 días",
      "description": "10GB adicionales con vigencia de 30 días",
      "retail_price": 249.00,
      "currency": "MXN",
      "data_gb": 10,
      "validity_days": 30,
      "active": true
    },
    {
      "id": "topup_03HZ...",
      "name": "Recarga ilimitada — 7 días",
      "description": "Datos ilimitados por 7 días",
      "retail_price": 99.00,
      "currency": "MXN",
      "data_gb": -1,
      "validity_days": 7,
      "active": true
    }
  ],
  "meta": {
    "total": 8,
    "page": 1,
    "limit": 20
  }
}
```

## Campos del plan de recarga

| Campo           | Tipo    | Descripción |
| --------------- | ------- | ----------- |
| `id`            | string  | ID del plan — usar como `plan_id` en el endpoint de Recargas |
| `name`          | string  | Nombre del plan |
| `retail_price`  | number  | Precio en MXN |
| `currency`      | string  | Siempre `MXN` |
| `data_gb`       | number  | GB incluidos (`-1` = ilimitado) |
| `validity_days` | number  | Días de vigencia desde la activación |
| `active`        | boolean | `true` si el plan está disponible |

> El `id` de un plan de recarga es el valor que debes enviar como `plan_id` al crear una [Recarga](../orders/topup.md).
