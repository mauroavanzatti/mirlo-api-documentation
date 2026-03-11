# Planes de recarga

Obtiene los planes disponibles para recargar una línea existente. Estos planes se usan exclusivamente en el endpoint [Recargas](../orders/topup.md).

## Diferencia con planes principales

| | Planes principales | Planes de recarga |
|-|---|---|
| Uso | Compra de línea nueva, portabilidad | Recargar línea activa |
| Endpoint de compra | `POST /orders` con `type: new_line` o `type: portability` | `POST /orders` con `type: topup` |
| Requiere datos del cliente | Sí (CURP, dirección) | No |

## Endpoint

```http
GET /offers/catalogs/all
```

## Parámetros de query

| Parámetro | Tipo    | Requerido | Descripción |
| --------- | ------- | --------- | ----------- |
| `type`    | string  | No        | Enviar `topups` para obtener solo planes de recarga |
| `page`    | integer | No        | Número de página (default: `1`) |
| `limit`   | integer | No        | Resultados por página (default: `20`, max: `100`) |

## Ejemplo

```bash
curl "{baseurl}/api/v1/offers/catalogs/all?type=topups" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "items": [
    {
      "id": "cat_2ec0449e-74ce-416f-98eb-47d14ba6350f",
      "name": "Recargas",
      "type": "topups",
      "offerings": [
        {
          "id": "offr_f87c8752-f422-459d-b1f0-586ad784df99",
          "name": "Plan 6 GB 7D",
          "price": 79,
          "recurrence": "7",
          "details": {
            "mxOffer": {
              "dataGb": 6,
              "minutes": 500,
              "sms": 125
            }
          }
        },
        {
          "id": "offr_34aab0c4-fdad-4d37-8945-f51a569c90ea",
          "name": "Plan 5 GB 15D",
          "price": 99,
          "recurrence": "15",
          "details": {
            "mxOffer": {
              "dataGb": 5,
              "minutes": 1000,
              "sms": 250
            }
          }
        },
        {
          "id": "offr_783736e0-d27e-4604-b764-f4dae7f11a54",
          "name": "Plan 12 GB 30D",
          "price": 349,
          "recurrence": "30",
          "details": {
            "mxOffer": {
              "dataGb": 12,
              "minutes": 1500,
              "sms": 500
            }
          }
        }
      ]
    }
  ],
  "count": 1,
  "page": 1,
  "limit": 20
}
```

## Campos del plan de recarga

| Campo                    | Tipo    | Descripción |
| ------------------------ | ------- | ----------- |
| `id`                     | string  | ID del catálogo |
| `name`                   | string  | Nombre del catálogo |
| `offerings[].id`         | string  | ID del plan — usar como `plan_id` en el endpoint de [Recargas](../orders/topup.md) |
| `offerings[].name`       | string  | Nombre del plan |
| `offerings[].price`      | number  | Precio en MXN |
| `offerings[].recurrence` | string  | Vigencia en días desde la activación |
| `offerings[].details.mxOffer.dataGb`   | number | GB de datos incluidos |
| `offerings[].details.mxOffer.minutes`  | number | Minutos de voz incluidos |
| `offerings[].details.mxOffer.sms`      | number | SMS incluidos |

> El `id` de un offering es el valor que debes enviar como `plan_id` al crear una [Recarga](../orders/topup.md).
