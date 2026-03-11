# Authentication

Mirlo uses **API Keys** to authenticate requests. Include your API key in the header of every request.

## Required header

```http
Authorization: Bearer <your_api_key>
```

## Getting your API Key

Contact your Mirlo account manager to obtain your access credentials.

## Example

```bash
curl https://api.mirlo.mx/api/v1/lines \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

> **Important:** Never expose your API key in client-side code. All calls must be made from your server.
