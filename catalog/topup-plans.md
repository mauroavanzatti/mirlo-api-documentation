# Top-up Plans

Returns the plans available to recharge an existing line. These plans are used exclusively in the [Top-ups](../orders/topup.md) endpoint.

## Difference from main plans

| | Main Plans | Top-up Plans |
|-|---|---|
| Use | New line purchase, portability | Recharge active line |
| Purchase endpoint | `POST /orders` with `type: new_line` or `type: portability` | `POST /orders` with `type: topup` |
| Requires customer data | Yes (CURP, address) | No |

## Endpoint

```http
GET /offers/catalogs/all
```

## Query parameters

| Parameter | Type    | Required | Description |
| --------- | ------- | -------- | ----------- |
| `type`    | string  | No       | Send `topups` to get only top-up plans |
| `page`    | integer | No       | Page number (default: `1`) |
| `limit`   | integer | No       | Results per page (default: `20`, max: `100`) |

## Example

```bash
curl "{baseurl}/api/v1/offers/catalogs/all?type=topups" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

```json
{
  "items": [
    {
      "id": "cat_2ec0449e-74ce-416f-98eb-47d14ba6350f",
      "name": "Top-ups",
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

## Top-up plan fields

| Field                                  | Type   | Description |
| -------------------------------------- | ------ | ----------- |
| `offerings[].id`                       | string | Plan ID — use as `plan_id` in the [Top-ups](../orders/topup.md) endpoint |
| `offerings[].name`                     | string | Plan name |
| `offerings[].price`                    | number | Price in MXN |
| `offerings[].recurrence`               | string | Validity in days from activation |
| `offerings[].details.mxOffer.dataGb`   | number | Data in GB |
| `offerings[].details.mxOffer.minutes`  | number | Voice minutes included |
| `offerings[].details.mxOffer.sms`      | number | SMS included |

> The offering `id` is the value you must send as `plan_id` when creating a [Top-up](../orders/topup.md).
