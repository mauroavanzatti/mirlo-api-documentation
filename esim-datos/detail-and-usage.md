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

### Campos de la respuesta

| Campo             | Tipo    | Descripción |
| ----------------- | ------- | ----------- |
| `remaining`       | integer | Bytes de datos restantes |
| `total`           | integer | Bytes de datos totales del paquete |
| `expired_at`      | string  | Fecha de vencimiento del paquete (ISO 8601) |
| `is_unlimited`    | boolean | `true` si los datos son ilimitados |
| `status`          | string  | Estado de la eSIM: `ACTIVE`, `NOT_ACTIVATED`, `EXPIRED`, `DEPLETED` |
| `remaining_voice` | integer | Segundos de voz restantes (`null` si no aplica) |
| `remaining_text`  | integer | SMS restantes (`null` si no aplica) |
| `total_voice`     | integer | Segundos de voz totales (`null` si no aplica) |
| `total_text`      | integer | SMS totales (`null` si no aplica) |

> Los valores en `remaining` y `total` están en **bytes**. Para convertir a MB: `bytes / 1_048_576`.
