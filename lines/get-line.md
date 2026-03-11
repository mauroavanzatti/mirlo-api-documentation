# Get Line

Returns the status and data of a line by its MSISDN.

## Endpoint

```http
GET /line/{msisdn}
```

## Path parameters

| Parameter | Description |
| --------- | ----------- |
| `msisdn`  | Line number (10 digits, no country code) |

## Example

```bash
curl {baseurl}/api/v1/line/5512345678 \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

```json
{
  "status": 200,
  "data": {
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
  }
}
```

## Line statuses

| Status      | Description |
| ----------- | ----------- |
| `active`    | Line active and in service |
| `suspended` | Temporarily suspended |
| `cancelled` | Cancelled line |
| `pending`   | Pending activation |
