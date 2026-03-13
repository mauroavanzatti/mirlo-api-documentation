# Listar eSIMs de datos compradas

Devuelve el historial de compras de eSIM de datos de la organización autenticada, enriquecido con el estado en tiempo real de cada SIM.

## Endpoint

```http
GET /travel_esim/purchases
```

No requiere parámetros. El `organization_id` se obtiene automáticamente del token de autenticación.

## Ejemplo

```bash
curl {baseurl}/api/v1/travel_esim/purchases \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": [
    {
      "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
      "organization_id": "f1e2d3c4-b5a6-7890-abcd-ef1234567890",
      "provider": "AIRALO",
      "provider_order_id": "3000123",
      "provider_package_id": "spain-7days-1gb",
      "mirlo_offer_id": "mesm_01HZ...",
      "offer_name": "España 7 días 1 GB",
      "country_code": "ES",
      "country_name": "España",
      "operator": "Movistar ES",
      "data_amount_mb": 1024,
      "data_gb": "1.00",
      "validity_days": 7,
      "is_unlimited": false,
      "plan_type": "local",
      "coverage_countries": null,
      "price_mxn": "140.00",
      "cost_usd": "7.00",
      "image_url": "https://cdn.mirlo.mx/esim/spain.png",
      "iccid": "8952140061234567890",
      "lpa": "LPA:1$sm-v4-059-a.es.prod.ondemandconnectivity.com$ABC123",
      "qrcode_url": "https://cdn.mirlo.mx/esim/qr_01HZ.png",
      "apn_type": "automatic",
      "apn_value": "airalo",
      "msisdn": null,
      "status": "ACTIVE",
      "live_status": "ACTIVE",
      "activation_date": "2025-01-15T12:00:00Z",
      "expiration_date": "2025-01-22T12:00:00Z",
      "created_at": "2025-01-15T10:30:00Z",
      "updated_at": "2025-01-15T12:00:00Z"
    }
  ]
}
```

## Campos de cada compra

| Campo               | Tipo    | Descripción |
| ------------------- | ------- | ----------- |
| `id`                | string (UUID) | ID único de la compra en Mirlo |
| `provider`          | string  | Proveedor: `AIRALO` o `BONDIO` |
| `provider_order_id` | string  | ID de la orden en el proveedor |
| `offer_name`        | string  | Nombre del paquete al momento de la compra |
| `country_code`      | string  | Código ISO del país de cobertura |
| `country_name`      | string  | Nombre del país |
| `operator`          | string  | Operador del paquete |
| `data_gb`           | string  | GB totales del paquete |
| `validity_days`     | integer | Vigencia en días |
| `is_unlimited`      | boolean | `true` si los datos eran ilimitados |
| `plan_type`         | string  | `local`, `regional` o `global` |
| `coverage_countries`| array   | Países cubiertos (para planes globales/regionales) |
| `price_mxn`         | string  | Precio pagado por el cliente en MXN |
| `iccid`             | string  | ICCID de la eSIM |
| `lpa`               | string  | String LPA para activación manual |
| `qrcode_url`        | string  | URL del QR de activación |
| `status`            | string  | Estado persistido: `PENDING`, `ACTIVE`, `EXPIRED`, `TERMINATED` |
| `live_status`       | string  | Estado en tiempo real consultado al proveedor (puede diferir de `status`) |
| `activation_date`   | string  | Fecha de activación (ISO 8601) |
| `expiration_date`   | string  | Fecha de vencimiento (ISO 8601) |
| `created_at`        | string  | Fecha de creación de la orden |

> `live_status` refleja el estado actual en la red del proveedor. Si la consulta al proveedor falla, se omite y solo aparece `status`.
