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
    "iccid": "8937103400004910314",
    "provider": "BONDIO",
    "data_used_bytes": 0,
    "data_total_bytes": 1073741824,
    "data_remaining_bytes": 1073741824,
    "usage_percentage": 0,
    "status": "pending_activation",
    "last_updated": "1970-01-01T00:00:00.000Z",
    "provider_metadata": {
      "attachmentId": "platt_xghvswcv",
      "state": "PENDING_FOR_FIRST_USE",
      "activationAt": null,
      "expirationAt": null,
      "plan": {
        "name": "Costco Europa 1GB 7d",
        "coverageProfileId": "cvpr_00357sly",
        "periodDays": 7,
        "dataMegaBytes": 1024,
        "label": "lambda",
        "periodIterations": 1,
        "throttledSpeedKbps": 0
      },
      "usedAllowance": {
        "dataBytes": 0,
        "smsMessages": null,
        "voiceSeconds": null
      }
    }
  }
}
```

### Response Fields

| Field                  | Type    | Description |
| ---------------------- | ------- | ----------- |
| `iccid`                | string  | eSIM ICCID |
| `provider`             | string  | Provider: `AIRALO` or `BONDIO` |
| `data_used_bytes`      | integer | Data consumed in bytes |
| `data_total_bytes`     | integer | Total data in the package in bytes |
| `data_remaining_bytes` | integer | Remaining data in bytes |
| `usage_percentage`     | number  | Percentage of data consumed (0–100) |
| `status`               | string  | eSIM status: `pending_activation`, `active`, `expired`, `depleted` |
| `last_updated`         | string  | Last time the provider updated the data (ISO 8601) |
| `provider_metadata`    | object  | Raw provider data (varies by provider) |
| `provider_metadata.state` | string | Internal provider state (e.g. `PENDING_FOR_FIRST_USE`) |
| `provider_metadata.activationAt` | string | Activation timestamp (`null` if not yet activated) |
| `provider_metadata.expirationAt` | string | Expiration timestamp (`null` if not yet activated) |
| `provider_metadata.plan` | object | Plan details at the provider level |
| `provider_metadata.plan.periodDays` | integer | Plan validity in days |
| `provider_metadata.plan.dataMegaBytes` | integer | Total plan data in MB |
| `provider_metadata.usedAllowance.dataBytes` | integer | Bytes consumed according to provider |

> `data_used_bytes`, `data_total_bytes`, and `data_remaining_bytes` are in **bytes**. To convert to MB: `bytes / 1_048_576`.
