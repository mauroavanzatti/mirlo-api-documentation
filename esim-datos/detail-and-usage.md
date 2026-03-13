# Detalle y consumo de una eSIM de datos

Dos endpoints para consultar el estado de instalación de una eSIM (detalle) y su consumo de datos en tiempo real (usage).

---

## Detalle de la eSIM

Devuelve el estado de activación e información de instalación de la eSIM.

### Endpoint

```http
GET /travel_esim/sims/{iccid}
```

### Parámetros de ruta

| Parámetro | Descripción |
| --------- | ----------- |
| `iccid`   | ICCID de la eSIM (19–20 dígitos) |

### Ejemplo

```bash
curl {baseurl}/api/v1/travel_esim/sims/8952140061234567890 \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

### Respuesta

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
        "description": "eSIM viaje España",
        "esim_type": "travelSim",
        "validity": 7,
        "package": "España 7 días 1 GB",
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

### Campos de la respuesta

| Campo                              | Tipo    | Descripción |
| ---------------------------------- | ------- | ----------- |
| `iccid`                            | string  | ICCID de la eSIM |
| `lpa`                              | string  | String LPA para activación manual (`LPA:1$...`) |
| `qrcode_installation`              | string  | URL del QR de activación |
| `manual_installation`              | string  | URL con guía de instalación manual (PDF) |
| `direct_apple_installation_url`    | string  | URL de instalación directa en iOS (Deep Link Apple) |
| `apn_type`                         | string  | Tipo de APN: `automatic` o `manual` |
| `apn_value`                        | string  | Valor del APN cuando es manual |
| `msisdn`                           | string  | Número asignado (puede ser `null`) |
| `matching_id`                      | string  | Matching ID de la eSIM |
| `is_roaming`                       | boolean | `true` si tiene roaming activo |
| `simable.package_id`               | string  | ID del paquete |
| `simable.data`                     | string  | Datos del paquete formateados |
| `simable.validity`                 | integer | Vigencia en días |
| `simable.price`                    | number  | Precio USD del proveedor |
| `simable.mirlo_price`              | number  | Precio MXN cobrado al cliente |

---

## Consumo de la eSIM

Devuelve el consumo de datos en tiempo real, directamente consultado al proveedor.

### Endpoint

```http
GET /travel_esim/sims/{iccid}/usage
```

### Parámetros de ruta

| Parámetro | Descripción |
| --------- | ----------- |
| `iccid`   | ICCID de la eSIM (19–20 dígitos) |

### Ejemplo

```bash
curl {baseurl}/api/v1/travel_esim/sims/8952140061234567890/usage \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

### Respuesta

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

### Campos de la respuesta

| Campo                  | Tipo    | Descripción |
| ---------------------- | ------- | ----------- |
| `iccid`                | string  | ICCID de la eSIM |
| `provider`             | string  | Proveedor: `AIRALO` o `BONDIO` |
| `data_used_bytes`      | integer | Datos consumidos en bytes |
| `data_total_bytes`     | integer | Datos totales del paquete en bytes |
| `data_remaining_bytes` | integer | Datos restantes en bytes |
| `usage_percentage`     | number  | Porcentaje de datos consumidos (0–100) |
| `status`               | string  | Estado de la eSIM: `pending_activation`, `active`, `expired`, `depleted` |
| `last_updated`         | string  | Última actualización de datos por el proveedor (ISO 8601) |
| `provider_metadata`    | object  | Datos crudos del proveedor (varía según proveedor) |
| `provider_metadata.state` | string | Estado interno del proveedor (ej. `PENDING_FOR_FIRST_USE`) |
| `provider_metadata.activationAt` | string | Fecha de activación (`null` si aún no se activó) |
| `provider_metadata.expirationAt` | string | Fecha de vencimiento (`null` si aún no se activó) |
| `provider_metadata.plan` | object | Detalle del plan a nivel proveedor |
| `provider_metadata.plan.periodDays` | integer | Vigencia del plan en días |
| `provider_metadata.plan.dataMegaBytes` | integer | Datos totales del plan en MB |
| `provider_metadata.usedAllowance.dataBytes` | integer | Bytes consumidos según el proveedor |

> `data_used_bytes`, `data_total_bytes` y `data_remaining_bytes` están en **bytes**. Para convertir a MB: `bytes / 1_048_576`.
