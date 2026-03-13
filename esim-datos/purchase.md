# Purchase Data eSIM

Creates a data eSIM purchase order. The system automatically selects the optimal provider (Airalo or Bondio) based on the requested package.

## Endpoint

```http
POST /travel_esim/orders
```

## Body

```json
{
  "package_id": "spain-7days-1gb",
  "quantity": 1,
  "customer_email": "juan@example.com",
  "description": "eSIM trip to Spain",
  "mirlo_offer_id": "mesm_01HZ...",
  "offer_snapshot": {
    "offer_name": "Spain 7 days 1 GB",
    "country_code": "ES",
    "country_name": "Spain",
    "operator": "Movistar ES",
    "data_amount_mb": 1024,
    "data_gb": 1,
    "validity_days": 7,
    "is_unlimited": false,
    "plan_type": "local",
    "price_mxn": 140.0,
    "cost_usd": 7.0,
    "image_url": "https://cdn.mirlo.mx/esim/spain.png"
  }
}
```

## Parameters

| Field                          | Required | Type    | Description |
| ------------------------------ | -------- | ------- | ----------- |
| `package_id`                   | Yes      | string  | Package ID obtained from the catalog |
| `quantity`                     | Yes      | integer | Number of eSIMs to purchase (minimum `1`) |
| `customer_email`               | No       | string  | Email to send activation instructions to |
| `description`                  | No       | string  | Free-form description of the order |
| `provider`                     | No       | string  | Force a provider: `AIRALO` or `BONDIO`. If omitted, selected automatically |
| `mirlo_offer_id`               | No       | string  | Offer ID in the Mirlo catalog (format `mesm_*`) |
| `offer_snapshot`               | No       | object  | Snapshot of the offer at purchase time (persisted in history) |
| `offer_snapshot.offer_name`    | No       | string  | Package name |
| `offer_snapshot.country_code`  | No       | string  | ISO country code of coverage |
| `offer_snapshot.country_name`  | No       | string  | Country name |
| `offer_snapshot.operator`      | No       | string  | Operator name |
| `offer_snapshot.data_amount_mb`| No       | integer | Total data in MB |
| `offer_snapshot.data_gb`       | No       | number  | Total data in GB |
| `offer_snapshot.validity_days` | No       | integer | Validity in days |
| `offer_snapshot.is_unlimited`  | No       | boolean | `true` if data is unlimited |
| `offer_snapshot.plan_type`     | No       | string  | `local`, `regional`, or `global` |
| `offer_snapshot.price_mxn`     | No       | number  | Price charged to the customer in MXN |
| `offer_snapshot.cost_usd`      | No       | number  | Provider cost in USD |
| `offer_snapshot.image_url`     | No       | string  | Package image URL |
| `offer_snapshot.coverage_countries` | No  | string[]| Countries covered (for global/regional packages) |

## Response

```json
{
  "status": 201,
  "data": {
    "order_id": "3000123",
    "provider": "AIRALO",
    "package_id": "spain-7days-1gb",
    "quantity": 1,
    "total_price": 140.0,
    "currency": "MXN",
    "status": "completed",
    "created_at": "2025-01-15T10:30:00Z",
    "sims": [
      {
        "iccid": "8952140061234567890",
        "provider": "AIRALO",
        "lpa": "LPA:1$sm-v4-059-a.es.prod.ondemandconnectivity.com$ABC123",
        "qrcode_url": "https://cdn.mirlo.mx/esim/qr_01HZ.png",
        "qrcode": "data:image/png;base64,...",
        "status": "NOT_ACTIVATED",
        "apn_type": "automatic",
        "apn_value": "airalo"
      }
    ]
  }
}
```

## Response Fields

| Field             | Type    | Description |
| ----------------- | ------- | ----------- |
| `order_id`        | string  | Order ID at the provider |
| `provider`        | string  | Provider that processed the order: `AIRALO` or `BONDIO` |
| `package_id`      | string  | ID of the purchased package |
| `quantity`        | integer | Number of eSIMs generated |
| `total_price`     | number  | Total price charged in MXN |
| `currency`        | string  | Always `MXN` |
| `status`          | string  | Order status |
| `sims`            | array   | List of generated eSIMs (one per `quantity`) |
| `sims[].iccid`    | string  | eSIM ICCID |
| `sims[].lpa`      | string  | LPA string for manual activation (`LPA:1$...`) |
| `sims[].qrcode_url` | string | Public URL of the activation QR code |
| `sims[].qrcode`   | string  | QR code in base64 (PNG) |
| `sims[].status`   | string  | Initial status: `NOT_ACTIVATED` |
| `sims[].apn_type` | string  | APN type (usually `automatic`) |
| `sims[].apn_value`| string  | APN value |

> Show the `qrcode_url` to the end user to scan from **Settings → Mobile → Add eSIM Plan**.
