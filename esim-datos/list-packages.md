# List Data eSIM Packages

Returns the catalog of available data eSIM packages for purchase, with support for filtering by coverage type (local or global) and destination country.

## Endpoint

```http
GET /travel_esim/packages
```

## Query Parameters

| Parameter         | Type    | Required | Description |
| ----------------- | ------- | -------- | ----------- |
| `filter[type]`    | string  | No       | Coverage type: `local` (single country) or `global` (multiple countries) |
| `filter[country]` | string  | No       | ISO 3166-1 alpha-2 country code (e.g. `MX`, `US`, `DE`). Only applies when `filter[type]` is `local` |
| `limit`           | integer | No       | Results per page (default: `20`) |
| `page`            | integer | No       | Page number (default: `1`) |
| `include`         | string  | No       | Include top-up packages: `topup` |

## Example — local packages for Spain

```bash
curl "{baseurl}/api/v1/travel_esim/packages?filter[type]=local&filter[country]=ES&limit=10" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Example — global packages

```bash
curl "{baseurl}/api/v1/travel_esim/packages?filter[type]=global&limit=20" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Response

```json
{
  "status": 200,
  "data": {
    "data": [
      {
        "slug": "spain-7days-1gb",
        "country_code": "ES",
        "country": "Spain",
        "operators": [
          {
            "id": 123,
            "name": "Movistar ES",
            "style": {
              "gradient": { "color": "#00ADEF", "direction": "to right" },
              "logo_url": "https://cdn.airalo.com/operators/123.png",
              "background_color": "#00ADEF"
            },
            "packages": [
              {
                "id": "spain-7days-1gb",
                "type": "local",
                "data": "1 GB",
                "validity": 7,
                "price": 7.0,
                "mirlo_price": 140.0,
                "is_unlimited": false,
                "net_price": null
              },
              {
                "id": "spain-30days-3gb",
                "type": "local",
                "data": "3 GB",
                "validity": 30,
                "price": 16.0,
                "mirlo_price": 320.0,
                "is_unlimited": false,
                "net_price": null
              }
            ]
          }
        ]
      }
    ],
    "meta": {
      "message": "success",
      "current_page": 1,
      "last_page": 3,
      "per_page": 20,
      "total": 54
    }
  }
}
```

## Package Fields

| Field          | Type    | Description |
| -------------- | ------- | ----------- |
| `id`           | string  | Package ID, used when placing a purchase order |
| `type`         | string  | `local` or `global` |
| `data`         | string  | Formatted data amount (e.g. `"1 GB"`) |
| `validity`     | integer | Validity period in days |
| `price`        | number  | Provider price in USD |
| `mirlo_price`  | number  | Retail price in MXN |
| `is_unlimited` | boolean | `true` if data is unlimited |
