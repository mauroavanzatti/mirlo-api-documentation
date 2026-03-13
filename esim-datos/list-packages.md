# Listar paquetes de eSIM de datos

Obtiene el catálogo de paquetes de eSIM de datos disponibles para compra, con soporte para filtrar por tipo (local o global) y por país de destino.

## Endpoint

```http
GET /travel_esim/packages
```

## Parámetros de query

| Parámetro        | Tipo    | Requerido | Descripción |
| ---------------- | ------- | --------- | ----------- |
| `filter[type]`   | string  | No        | Tipo de cobertura: `local` (un país) o `global` (múltiples países) |
| `filter[country]`| string  | No        | Código de país ISO 3166-1 alpha-2 (ej. `MX`, `US`, `DE`). Solo aplica cuando `filter[type]` es `local` |
| `limit`          | integer | No        | Resultados por página (default: `20`) |
| `page`           | integer | No        | Número de página (default: `1`) |
| `include`        | string  | No        | Incluir paquetes de recarga: `topup` |

## Ejemplo — paquetes locales para España

```bash
curl "{baseurl}/api/v1/travel_esim/packages?filter[type]=local&filter[country]=ES&limit=10" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Ejemplo — paquetes globales

```bash
curl "{baseurl}/api/v1/travel_esim/packages?filter[type]=global&limit=20" \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

## Respuesta

```json
{
  "status": 200,
  "data": {
    "data": [
      {
        "slug": "spain-7days-1gb",
        "country_code": "ES",
        "country": "España",
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

## Campos del paquete

| Campo          | Tipo    | Descripción |
| -------------- | ------- | ----------- |
| `id`           | string  | ID del paquete, se usa en la orden de compra |
| `type`         | string  | `local` o `global` |
| `data`         | string  | Cantidad de datos formateada (ej. `"1 GB"`) |
| `validity`     | integer | Vigencia en días |
| `price`        | number  | Precio en USD del proveedor |
| `mirlo_price`  | number  | Precio de venta en MXN |
| `is_unlimited` | boolean | `true` si los datos son ilimitados |
