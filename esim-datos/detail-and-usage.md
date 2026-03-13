# Data eSIM Detail & Usage

Two endpoints to check the installation status of a data eSIM (detail) and its real-time data consumption (usage).

---

## eSIM Detail

Returns the activation status and installation information for a data eSIM.

### Endpoint

```http
GET /travel_esim/sims/{iccid}
```

### Path Parameters

| Parameter | Description |
| --------- | ----------- |
| `iccid`   | eSIM ICCID (19–20 digits) |

### Example

```bash
curl {baseurl}/api/v1/travel_esim/sims/8952140061234567890 \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

### Response

```json
{
  "status": 200,
  "data": {
    "data": {
      "id": 456789,
      "created_at": "2025-01-15T10:30:00Z",
      "iccid": "8952140061234567890",
      "lpa": "LPA:1$sm-v4-059-a.es.prod.ondemandconnectivity.com$ABC123",
      "qrcode_installation": "https://cdn.airalo.com/qr/abc123.png",
      "manual_installation": "https://cdn.airalo.com/manual/abc123.pdf",
      "direct_apple_installation_url": "https://esimsetup.apple.com/esim_qrcode_provisioning?carddata=LPA:1$...",
      "apn_type": "automatic",
      "apn_value": "airalo",
      "msisdn": null,
      "matching_id": "ABC123",
      "confirmation_code": null,
      "is_roaming": false,
      "simable": {
        "id": 3000123,
        "package_id": "spain-7days-1gb",
        "quantity": 1,
        "type": "sim",
        "description": "eSIM trip to Spain",
        "esim_type": "travelSim",
        "validity": 7,
        "package": "Spain 7 days 1 GB",
        "data": "1 GB",
        "price": 7.0,
        "mirlo_price": 140.0,
        "text": null,
        "voice": null,
        "created_at": "2025-01-15T10:30:00Z"
      }
    }
  }
}
```

### Response Fields

| Field                              | Type    | Description |
| ---------------------------------- | ------- | ----------- |
| `iccid`                            | string  | eSIM ICCID |
| `lpa`                              | string  | LPA string for manual activation (`LPA:1$...`) |
| `qrcode_installation`              | string  | Activation QR code URL |
| `manual_installation`              | string  | Manual installation guide URL (PDF) |
| `direct_apple_installation_url`    | string  | Direct iOS installation URL (Apple Deep Link) |
| `apn_type`                         | string  | APN type: `automatic` or `manual` |
| `apn_value`                        | string  | APN value when manual |
| `msisdn`                           | string  | Assigned phone number (may be `null`) |
| `matching_id`                      | string  | eSIM matching ID |
| `is_roaming`                       | boolean | `true` if roaming is active |
| `simable.package_id`               | string  | Package ID |
| `simable.data`                     | string  | Formatted package data |
| `simable.validity`                 | integer | Validity in days |
| `simable.price`                    | number  | Provider price in USD |
| `simable.mirlo_price`              | number  | Price charged to the customer in MXN |

---

## eSIM Usage

Returns real-time data consumption, queried directly from the provider.

### Endpoint

```http
GET /travel_esim/sims/{iccid}/usage
```

### Path Parameters

| Parameter | Description |
| --------- | ----------- |
| `iccid`   | eSIM ICCID (19–20 digits) |

### Example

```bash
curl {baseurl}/api/v1/travel_esim/sims/8952140061234567890/usage \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

### Response

```json
{
  "status": 200,
  "data": {
    "data": {
      "remaining": 524288000,
      "total": 1073741824,
      "expired_at": "2025-01-22T12:00:00Z",
      "is_unlimited": false,
      "status": "ACTIVE",
      "remaining_voice": null,
      "remaining_text": null,
      "total_voice": null,
      "total_text": null
    },
    "meta": {
      "message": "success"
    }
  }
}
```

### Response Fields

| Field             | Type    | Description |
| ----------------- | ------- | ----------- |
| `remaining`       | integer | Remaining data in bytes |
| `total`           | integer | Total data bytes in the package |
| `expired_at`      | string  | Package expiration date (ISO 8601) |
| `is_unlimited`    | boolean | `true` if data is unlimited |
| `status`          | string  | eSIM status: `ACTIVE`, `NOT_ACTIVATED`, `EXPIRED`, `DEPLETED` |
| `remaining_voice` | integer | Remaining voice seconds (`null` if not applicable) |
| `remaining_text`  | integer | Remaining SMS (`null` if not applicable) |
| `total_voice`     | integer | Total voice seconds (`null` if not applicable) |
| `total_text`      | integer | Total SMS (`null` if not applicable) |

> `remaining` and `total` are in **bytes**. To convert to MB: `bytes / 1_048_576`.
