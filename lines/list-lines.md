# List Lines

Returns all lines associated with your partner account.

## Endpoint

```http
GET /lines
```

## Query parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| `status`  | string  | No       | Filter by status: `active`, `suspended`, `cancelled` |
| `page`    | integer | No       | Page number (default: `1`) |
| `limit`   | integer | No       | Results per page (default: `20`, max: `100`) |

## Example

```bash
curl "{baseurl}/api/v1/lines?status=active&page=1&limit=50" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

```json
{
  "status": 200,
  "data": [
    {
      "msisdn": "5512345678",
      "status": "active",
      "plan": {
        "id": "offer_01HZ...",
        "name": "Basic Plan 5GB",
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
        "name": "Pro Plan 20GB",
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
        "name": "Basic Plan 5GB",
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

## Response fields

| Field               | Type   | Description |
| ------------------- | ------ | ----------- |
| `msisdn`            | string | Line number |
| `status`            | string | Current status: `active`, `suspended`, `cancelled`, `pending` |
| `plan.id`           | string | Active plan ID |
| `plan.name`         | string | Active plan name |
| `plan.renewal_date` | string | Renewal date (`null` if suspended or cancelled) |
| `sim_type`          | string | `sim` or `esim` |
| `iccid`             | string | SIM identifier |
| `activated_at`      | string | Activation date in ISO 8601 |
