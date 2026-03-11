# Top-ups

Recharges or changes the plan of an active line.

## Endpoint

```http
POST /orders
```

## Body

```json
{
  "type": "topup",
  "msisdn": "5512345678",
  "plan_id": "offr_f87c8752-f422-459d-b1f0-586ad784df99"
}
```

## Parameters

| Field     | Required | Type   | Description |
| --------- | -------- | ------ | ----------- |
| `type`    | Yes      | string | Always `topup` |
| `msisdn`  | Yes      | string | Line to recharge |
| `plan_id` | Yes      | string | Top-up plan ID from [Top-up Plans](../catalog/topup-plans.md) |

## Response

```json
{
  "status": 201,
  "data": {
    "order_id": "ord_01HZ...",
    "status": "completed",
    "msisdn": "5512345678",
    "plan": {
      "id": "offr_f87c8752-f422-459d-b1f0-586ad784df99",
      "name": "Plan 6 GB 7D"
    },
    "renewal_date": "2025-02-15T00:00:00Z",
    "created_at": "2025-01-15T10:30:00Z"
  }
}
```
