# Main Plans

Returns the catalog of available plans for purchasing a new line or porting a number.

## Endpoint

```http
GET /offers/catalogs/all
```

## Query parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| `type`    | string  | No       | Filter by type: `postpaid`, `prepaid` |
| `page`    | integer | No       | Page number (default: `1`) |
| `limit`   | integer | No       | Results per page (default: `20`, max: `100`) |

## Example

```bash
curl https://api.mirlo.mx/api/v1/offers/catalogs/all \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

```json
{
  "status": 200,
  "data": [
    {
      "id": "offer_01HZ...",
      "name": "Basic Plan 5GB",
      "description": "5GB data, unlimited calls",
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

## Plan fields

| Field           | Type    | Description |
| --------------- | ------- | ----------- |
| `id`            | string  | Unique plan ID |
| `name`          | string  | Plan name |
| `retail_price`  | number  | Sale price in MXN |
| `currency`      | string  | Always `MXN` |
| `data_gb`       | number  | Data in GB (`-1` = unlimited) |
| `voice_minutes` | number  | Voice minutes (`-1` = unlimited) |
| `sms`           | number  | SMS included (`-1` = unlimited) |
| `validity_days` | number  | Validity in days |
| `active`        | boolean | `true` if the plan is available for purchase |
