# Códigos de error

Mirlo usa códigos HTTP estándar. Todos los errores incluyen un campo `message` con descripción legible y un `code` de negocio.

## Formato de error

```json
{
  "status": 400,
  "message": "El campo msisdn es requerido",
  "code": "VALIDATION_ERROR"
}
```

## Códigos HTTP

| Código | Significado |
| ------ | ----------- |
| `200`  | OK |
| `201`  | Recurso creado |
| `400`  | Petición inválida — revisa los parámetros |
| `401`  | No autenticado — API key inválida o ausente |
| `403`  | Sin permisos para este recurso |
| `404`  | Recurso no encontrado |
| `409`  | Conflicto — ej. número ya portado, CURP duplicado |
| `422`  | Error de validación de negocio |
| `429`  | Rate limit excedido |
| `500`  | Error interno del servidor |

## Códigos de negocio (`code`)

| Code | Descripción |
| ---- | ----------- |
| `VALIDATION_ERROR` | Campos requeridos faltantes o inválidos |
| `PLAN_NOT_FOUND` | El `plan_id` no existe o no está activo |
| `LINE_NOT_FOUND` | El `msisdn` no existe en el sistema |
| `PORTABILITY_FAILED` | El operador donante rechazó la portabilidad |
| `INVALID_NIP` | El NIP de portabilidad es incorrecto |
| `DUPLICATE_CURP` | Ya existe un usuario con ese CURP |
| `RATE_LIMIT_EXCEEDED` | Demasiadas peticiones — espera antes de reintentar |

## Rate Limits

| Endpoint | Límite |
| -------- | ------ |
| `GET /lines` | 100 req/min |
| `GET /line/*` | 200 req/min |
| `POST /orders` | 30 req/min |

## Reintentos recomendados

Solo reintenta en errores `5xx` o `429`. Los errores `4xx` son definitivos y no mejorarán con reintentos.

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
