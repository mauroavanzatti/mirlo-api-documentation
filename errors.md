# Error Codes

Mirlo uses standard HTTP status codes. All errors include a human-readable `message` field and a business `code`.

## Error format

```json
{
  "status": 400,
  "message": "The msisdn field is required",
  "code": "VALIDATION_ERROR"
}
```

## HTTP codes

| Code  | Meaning |
| ----- | ------- |
| `200` | OK |
| `201` | Resource created |
| `400` | Bad request — check your parameters |
| `401` | Unauthenticated — invalid or missing API key |
| `403` | Forbidden — no permission for this resource |
| `404` | Resource not found |
| `409` | Conflict — e.g. number already ported, duplicate CURP |
| `422` | Business validation error |
| `429` | Rate limit exceeded |
| `500` | Internal server error |

## Business codes (`code`)

| Code                   | Description |
| ---------------------- | ----------- |
| `VALIDATION_ERROR`     | Required fields missing or invalid |
| `PLAN_NOT_FOUND`       | The `plan_id` does not exist or is not active |
| `LINE_NOT_FOUND`       | The `msisdn` does not exist in the system |
| `PORTABILITY_FAILED`   | The donor carrier rejected the portability request |
| `INVALID_NIP`          | The portability NIP is incorrect |
| `DUPLICATE_CURP`       | A user with this CURP already exists |
| `RATE_LIMIT_EXCEEDED`  | Too many requests — wait before retrying |

## Rate limits

| Endpoint       | Limit        |
| -------------- | ------------ |
| `GET /lines`   | 100 req/min  |
| `GET /line/*`  | 200 req/min  |
| `POST /orders` | 30 req/min   |

## Recommended retry strategy

Only retry on `5xx` or `429` errors. `4xx` errors are definitive and will not improve with retries.

```javascript
const MAX_RETRIES = 3;
const RETRY_DELAY_MS = 1000;

async function withRetry(fn, retries = MAX_RETRIES) {
  try {
    return await fn();
  } catch (err) {
    if (retries > 0 && (err.status >= 500 || err.status === 429)) {
      await new Promise(r => setTimeout(r, RETRY_DELAY_MS));
      return withRetry(fn, retries - 1);
    }
    throw err;
  }
}
```
