# Listar planes

Obtiene el catálogo de planes disponibles para compra.

## Endpoint

```http
GET /offers/catalogs/all
```

## Parámetros de query

| Parámetro | Tipo    | Requerido | Descripción |
| --------- | ------- | --------- | ----------- |
| `type`    | string  | No        | Filtra por tipo: `postpaid`, `prepaid` |
| `page`    | integer | No        | Número de página (default: `1`) |
| `limit`   | integer | No        | Resultados por página (default: `20`, max: `100`) |

## Ejemplo

```bash
curl {baseurl}/api/v1/offers/catalogs/all \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": [
    {
      "id": "offer_01HZ...",
      "name": "Plan Básico 5GB",
      "description": "5GB de datos, llamadas ilimitadas",
      "type": "postpaid",
      "retail_price": 299.00,
      "currency": "MXN",
      "data_gb": 5,
      "voice_minutes": -1,
      "sms": -1,
      "validity_days": 30,
      "active": true
    }
  ],
  "meta": {
    "total": 12,
    "page": 1,
    "limit": 20
  }
}
```

## Campos del plan

| Campo           | Tipo    | Descripción |
| --------------- | ------- | ----------- |
| `id`            | string  | ID único del plan |
| `name`          | string  | Nombre del plan |
| `retail_price`  | number  | Precio de venta en MXN |
| `currency`      | string  | Siempre `MXN` |
| `data_gb`       | number  | GB de datos (`-1` = ilimitado) |
| `voice_minutes` | number  | Minutos de voz (`-1` = ilimitado) |
| `sms`           | number  | SMS incluidos (`-1` = ilimitado) |
| `validity_days` | number  | Vigencia en días |
| `active`        | boolean | `true` si el plan está disponible para compra |
