# Autenticación

Mirlo usa **API Keys** para autenticar las peticiones. Incluye tu API key en el header de cada request.

## Header requerido

```http
Authorization: Bearer <tu_api_key>
```

## Obtener tu API Key

Contacta a tu account manager en Mirlo para obtener tus credenciales de acceso.

## Ejemplo

```bash
curl https://api.mirlo.mx/api/v1/lines \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

> **Importante:** Nunca expongas tu API key en código del lado del cliente. Todas las llamadas deben hacerse desde tu servidor.
