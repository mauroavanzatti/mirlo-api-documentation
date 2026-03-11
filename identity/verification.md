# Identity Verification

Before activating a line, the end user must complete an identity verification (KYC) process. This ensures compliance with PROFECO and IFT regulations for SIM registration in Mexico.

## Full flow

```
1. Partner submits user data       → POST /identity/verifications
2. Mirlo returns a verification_id   and status "pending"
3. Partner uploads user documents  → POST /identity/verifications/{id}/documents
4. Mirlo processes the verification  (seconds to minutes)
5. Partner checks the result       → GET /identity/verifications/{id}
6. Status "approved"               → proceed to create the order
```

---

## 1. Create verification

Starts the identity verification process for a user.

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

### Parameters

**customer**

| Field        | Required | Description |
| ------------ | -------- | ----------- |
| `name`       | Yes      | Full name as it appears on the identity document |
| `curp`       | Yes      | 18-character CURP |
| `birth_date` | Yes      | Date of birth in `YYYY-MM-DD` format |
| `email`      | Yes      | User email address |
| `phone`      | Yes      | Contact phone number (10 digits) |

**address**

| Field          | Required | Description |
| -------------- | -------- | ----------- |
| `street`       | Yes      | Street name, exterior and interior number |
| `neighborhood` | No       | Neighborhood (colonia) |
| `city`         | Yes      | City or municipality |
| `state`        | Yes      | State |
| `zip`          | Yes      | ZIP code (5 digits) |

**document**

| Field         | Required | Description |
| ------------- | -------- | ----------- |
| `type`        | Yes      | Document type: `ine`, `pasaporte`, `cedula_profesional` |
| `front_image` | Yes      | Front of document in **base64** (JPEG or PNG, max 5MB) |
| `back_image`  | No       | Back of document — required for `ine`, optional for `pasaporte` |

**selfie_image**

Photo of the user's face in **base64** (JPEG or PNG, max 5MB). Biometrically compared against the document photo.

### Response

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

## 2. Check status

Returns the result of an identity verification.

### Endpoint

```http
GET /identity/verifications/{verification_id}
```

### Example

```bash
curl https://api.mirlo.mx/api/v1/identity/verifications/ver_01HZ... \
  -H "Authorization: Bearer sk_live_xxxxxxxxxxxx"
```

### Response — approved

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

### Response — rejected

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

### Verification statuses

| Status                  | Description |
| ----------------------- | ----------- |
| `pending`               | Processing — wait a few seconds and check again |
| `approved`              | Identity verified — you can proceed to create the order |
| `rejected`              | Verification failed — see `rejection_reasons` |
| `requires_resubmission` | Some data must be corrected — the user must restart the flow |
| `expired`               | Verification was not completed before `expires_at` |

### Rejection reasons (`rejection_reasons`)

| Code                  | Description |
| --------------------- | ----------- |
| `document_unreadable` | Document is not legible (blurry, cropped, flash glare) |
| `document_expired`    | Identity document is expired |
| `document_mismatch`   | Name on document does not match the CURP |
| `selfie_mismatch`     | Selfie does not match the document photo |
| `curp_invalid`        | CURP does not exist or is invalid in RENAPO |
| `duplicate_identity`  | A line already exists registered with this CURP |

---

## Using the verification when creating an order

Once approved, include the `verification_id` when creating the order:

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

> Verifications are valid for **7 days**. If the user does not complete the purchase within that period, a new verification must be initiated.
