# Comprar eSIM de datos

Crea una orden de compra de eSIM de datos. El sistema selecciona automáticamente el proveedor óptimo (Airalo o Bondio) según el paquete solicitado.

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
  "description": "eSIM viaje España",
  "mirlo_offer_id": "mesm_01HZ...",
  "offer_snapshot": {
    "offer_name": "España 7 días 1 GB",
    "country_code": "ES",
    "country_name": "España",
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

## Parámetros

| Campo                          | Requerido | Tipo    | Descripción |
| ------------------------------ | --------- | ------- | ----------- |
| `package_id`                   | Sí        | string  | ID del paquete obtenido del catálogo |
| `quantity`                     | Sí        | integer | Número de eSIMs a comprar (mínimo `1`) |
| `customer_email`               | No        | string  | Email para enviar instrucciones de activación |
| `description`                  | No        | string  | Descripción libre de la orden |
| `provider`                     | No        | string  | Forzar proveedor: `AIRALO` o `BONDIO`. Si se omite, se selecciona automáticamente |
| `mirlo_offer_id`               | No        | string  | ID de la oferta en el catálogo Mirlo (formato `mesm_*`) |
| `offer_snapshot`               | No        | object  | Snapshot de la oferta al momento de compra (se persiste en el historial) |
| `offer_snapshot.offer_name`    | No        | string  | Nombre del paquete |
| `offer_snapshot.country_code`  | No        | string  | Código ISO del país de cobertura |
| `offer_snapshot.country_name`  | No        | string  | Nombre del país en español |
| `offer_snapshot.operator`      | No        | string  | Nombre del operador |
| `offer_snapshot.data_amount_mb`| No        | integer | Datos totales en MB |
| `offer_snapshot.data_gb`       | No        | number  | Datos totales en GB |
| `offer_snapshot.validity_days` | No        | integer | Vigencia en días |
| `offer_snapshot.is_unlimited`  | No        | boolean | `true` si los datos son ilimitados |
| `offer_snapshot.plan_type`     | No        | string  | `local`, `regional` o `global` |
| `offer_snapshot.price_mxn`     | No        | number  | Precio cobrado al cliente en MXN |
| `offer_snapshot.cost_usd`      | No        | number  | Costo al proveedor en USD |
| `offer_snapshot.image_url`     | No        | string  | URL de la imagen del paquete |
| `offer_snapshot.coverage_countries` | No   | string[]| Países cubiertos (aplica para paquetes globales/regionales) |

## Respuesta

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

## Campos de la respuesta

| Campo            | Tipo    | Descripción |
| ---------------- | ------- | ----------- |
| `order_id`       | string  | ID de la orden en el proveedor |
| `provider`       | string  | Proveedor que procesó la orden: `AIRALO` o `BONDIO` |
| `package_id`     | string  | ID del paquete comprado |
| `quantity`       | integer | Cantidad de eSIMs generadas |
| `total_price`    | number  | Precio total cobrado en MXN |
| `currency`       | string  | Siempre `MXN` |
| `status`         | string  | Estado de la orden |
| `sims`           | array   | Lista de eSIMs generadas (una por `quantity`) |
| `sims[].iccid`   | string  | ICCID de la eSIM |
| `sims[].lpa`     | string  | String LPA para activación manual (`LPA:1$...`) |
| `sims[].qrcode_url` | string | URL pública del QR de activación |
| `sims[].qrcode`  | string  | QR en base64 (PNG) |
| `sims[].status`  | string  | Estado inicial: `NOT_ACTIVATED` |
| `sims[].apn_type`| string  | Tipo de APN (generalmente `automatic`) |
| `sims[].apn_value`| string | Valor del APN |

> Muestra el `qrcode_url` al usuario para que escanee desde **Ajustes → Móvil → Agregar plan eSIM**.
