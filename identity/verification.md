# Verificación de identidad

Antes de activar una línea, el usuario final debe pasar por un proceso de verificación de identidad (KYC). Este flujo asegura el cumplimiento con la regulación de la PROFECO y el IFT para registro de SIMs en México.

## Flujo completo

```
1. Partner envía datos del usuario → POST /identity/verifications
2. Mirlo retorna un verification_id y estado "pending"
3. Partner sube los documentos del usuario → POST /identity/verifications/{id}/documents
4. Mirlo procesa la verificación (segundos a minutos)
5. Partner consulta el resultado → GET /identity/verifications/{id}
6. Estado "approved" → puede proceder a crear la orden
```

---

## 1. Crear verificación

Inicia el proceso de verificación de identidad para un usuario.

### Endpoint

```http
POST /identity/verifications
```

### Body

```json
{
  "customer": {
    "name": "Juan Pérez García",
    "curp": "PERJ900101HDFRZN09",
    "birth_date": "1990-01-01",
    "email": "juan@example.com",
    "phone": "5512345678"
  },
  "address": {
    "street": "Av. Insurgentes Sur 1234 Int. 5",
    "neighborhood": "Del Valle",
    "city": "Ciudad de México",
    "state": "CDMX",
    "zip": "03810"
  },
  "document": {
    "type": "ine",
    "front_image": "<base64>",
    "back_image": "<base64>"
  },
  "selfie_image": "<base64>"
}
```

### Parámetros

**customer**

| Campo        | Requerido | Descripción |
| ------------ | --------- | ----------- |
| `name`       | Sí        | Nombre completo tal como aparece en el documento de identidad |
| `curp`       | Sí        | CURP de 18 caracteres |
| `birth_date` | Sí        | Fecha de nacimiento en formato `YYYY-MM-DD` |
| `email`      | Sí        | Correo electrónico del usuario |
| `phone`      | Sí        | Teléfono de contacto (10 dígitos) |

**address**

| Campo          | Requerido | Descripción |
| -------------- | --------- | ----------- |
| `street`       | Sí        | Calle, número exterior e interior |
| `neighborhood` | No        | Colonia |
| `city`         | Sí        | Ciudad o municipio |
| `state`        | Sí        | Estado de la república |
| `zip`          | Sí        | Código postal (5 dígitos) |

**document**

| Campo         | Requerido | Descripción |
| ------------- | --------- | ----------- |
| `type`        | Sí        | Tipo de documento: `ine`, `pasaporte`, `cedula_profesional` |
| `front_image` | Sí        | Imagen frontal del documento en **base64** (JPEG o PNG, max 5MB) |
| `back_image`  | No        | Imagen trasera — requerida para `ine`, opcional para `pasaporte` |

**selfie_image**

Foto del rostro del usuario en **base64** (JPEG o PNG, max 5MB). Se compara biométricamente contra la foto del documento.

### Respuesta

```json
{
  "status": 201,
  "data": {
    "verification_id": "ver_01HZ...",
    "status": "pending",
    "created_at": "2025-01-15T10:30:00Z",
    "expires_at": "2025-01-22T10:30:00Z"
  }
}
```

---

## 2. Consultar estado

Obtiene el resultado de una verificación de identidad.

### Endpoint

```http
GET /identity/verifications/{verification_id}
```

### Ejemplo

```bash
curl {baseurl}/api/v1/identity/verifications/ver_01HZ... \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

### Respuesta — aprobada

```json
{
  "status": 200,
  "data": {
    "verification_id": "ver_01HZ...",
    "status": "approved",
    "customer": {
      "name": "Juan Pérez García",
      "curp": "PERJ900101HDFRZN09"
    },
    "approved_at": "2025-01-15T10:32:00Z",
    "expires_at": "2025-01-22T10:30:00Z"
  }
}
```

### Respuesta — rechazada

```json
{
  "status": 200,
  "data": {
    "verification_id": "ver_01HZ...",
    "status": "rejected",
    "rejection_reasons": [
      "document_unreadable",
      "selfie_mismatch"
    ],
    "rejected_at": "2025-01-15T10:33:00Z"
  }
}
```

### Estados de verificación

| Estado                  | Descripción |
| ----------------------- | ----------- |
| `pending`               | En proceso — espera unos segundos y vuelve a consultar |
| `approved`              | Identidad verificada — puedes proceder a crear la orden |
| `rejected`              | Verificación rechazada — ver `rejection_reasons` |
| `requires_resubmission` | Algunos datos deben corregirse — el usuario debe reiniciar el flujo |
| `expired`               | La verificación no fue completada antes de `expires_at` |

### Razones de rechazo (`rejection_reasons`)

| Código                  | Descripción |
| ----------------------- | ----------- |
| `document_unreadable`   | El documento no es legible (borroso, cortado, con flash) |
| `document_expired`      | El documento de identidad está vencido |
| `document_mismatch`     | El nombre en el documento no coincide con el CURP |
| `selfie_mismatch`       | La selfie no coincide con la foto del documento |
| `curp_invalid`          | El CURP no existe o no es válido en el RENAPO |
| `duplicate_identity`    | Ya existe una línea registrada con este CURP |

---

## Usar la verificación al crear una orden

Una vez aprobada, incluye el `verification_id` al crear la orden:

```json
{
  "type": "new_line",
  "plan_id": "offer_01HZ...",
  "sim_type": "esim",
  "verification_id": "ver_01HZ...",
  "customer": {
    "name": "Juan Pérez García",
    "email": "juan@example.com",
    "phone": "5512345678",
    "curp": "PERJ900101HDFRZN09"
  },
  "address": {
    "street": "Av. Insurgentes Sur 1234",
    "city": "Ciudad de México",
    "state": "CDMX",
    "zip": "03810"
  }
}
```

> Las verificaciones tienen una vigencia de **7 días**. Si el usuario no completa la compra en ese plazo, deberá iniciar una nueva verificación.
