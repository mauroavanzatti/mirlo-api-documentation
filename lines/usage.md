# Line Usage

Returns the data, voice, and SMS consumption for a line in the active period.

## Endpoint

```http
GET /line/usage/{msisdn}
```

## Path parameters

| Parameter | Description |
| --------- | ----------- |
| `msisdn`  | Line number (10 digits, no country code) |

## Example

```bash
curl {baseurl}/api/v1/line/usage/5512345678 \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

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

> Values of `-1` in `total` and `remaining` indicate the resource is **unlimited**.
