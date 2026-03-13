# List Purchased Data eSIMs

Returns the data eSIM purchase history for the authenticated organization, enriched with the real-time status of each SIM.

## Endpoint

```http
GET /travel_esim/purchases
```

No parameters required. The `organization_id` is automatically extracted from the authentication token.

## Example

```bash
curl {baseurl}/api/v1/travel_esim/purchases \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

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
      "offer_name": "Spain 7 days 1 GB",
      "country_code": "ES",
      "country_name": "Spain",
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

## Fields

| Field               | Type          | Description |
| ------------------- | ------------- | ----------- |
| `id`                | string (UUID) | Unique purchase ID in Mirlo |
| `provider`          | string        | Provider: `AIRALO` or `BONDIO` |
| `provider_order_id` | string        | Order ID at the provider |
| `offer_name`        | string        | Package name at the time of purchase |
| `country_code`      | string        | ISO country code of coverage |
| `country_name`      | string        | Country name |
| `operator`          | string        | Package operator |
| `data_gb`           | string        | Total GB of the package |
| `validity_days`     | integer       | Validity period in days |
| `is_unlimited`      | boolean       | `true` if data was unlimited |
| `plan_type`         | string        | `local`, `regional`, or `global` |
| `coverage_countries`| array         | Countries covered (for global/regional plans) |
| `price_mxn`         | string        | Price paid by the customer in MXN |
| `iccid`             | string        | eSIM ICCID |
| `lpa`               | string        | LPA string for manual activation |
| `qrcode_url`        | string        | Activation QR code URL |
| `status`            | string        | Persisted status: `PENDING`, `ACTIVE`, `EXPIRED`, `TERMINATED` |
| `live_status`       | string        | Real-time status queried from the provider (may differ from `status`) |
| `activation_date`   | string        | Activation date (ISO 8601) |
| `expiration_date`   | string        | Expiration date (ISO 8601) |
| `created_at`        | string        | Order creation date |

> `live_status` reflects the current status on the provider's network. If the provider query fails, this field is omitted and only `status` is returned.
