# Finance, Photo, Extra methods, Limits (13–16)

[← Back to contents](README.md)

---

## 13. Cash and payments

### 13.1. `setPayment` — Create payment

**Description:** Create a client payment. Can be linked to a specific order.

**Request:**
```json
{
    "method": "setPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "payment": [
            {
                "code_1C": "PAY000001",
                "amount": 5000000,
                "paymentDate": "2025-06-15 14:30:00",
                "expirationDate": null,
                "comment": "Payment for goods",
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "paymentType": { "code_1C": "000000001" },
                "trade": { "SD_id": "1" },
                "order": { "SD_id": "d0_300" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Identifier |
| `amount` | float | ✅ | Amount (>0) |
| `paymentDate` | string | ✅ | Payment date |
| `expirationDate` | string | ❌ | Expiration date |
| `comment` | string | ❌ | Comment |
| `client` | object | ✅ | Client |
| `agent` | object | ❌ | Agent |
| `paymentType` | object | ✅ | Payment type |
| `trade` | object | ❌ | Trade direction |
| `order` | object | ❌ | Order to link (close debt) |

---

### 13.2. `setBalance` — Set opening balance

**Description:** Create or update a client's opening balance (transaction type "Opening balance"). You can use CS_id / SD_id / code_1C to identify the record.

**Request:**
```json
{
    "method": "setBalance",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "balance": [
            {
                "code_1C": "BAL000001",
                "amount": 1000000,
                "paymentDate": "2025-01-01 00:00:00",
                "comment": "Opening balance",
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "paymentType": { "code_1C": "000000001" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (one for update) | Balance record identifier |
| `amount` | float | ✅ | Amount |
| `paymentDate` | string | ✅ | Date (Y-m-d H:i:s) |
| `comment` | string | ❌ | Comment |
| `client` | object | ✅ | Client. CS_id / SD_id / code_1C |
| `agent` | object | ❌ | Agent. CS_id / SD_id / code_1C |
| `paymentType` | object | ✅ | Payment type (currency). CS_id / SD_id / code_1C |

**Response:** Standard SET response: `completed`, `error`, `data.balance` — array of objects with `CS_id`, `SD_id`, `code_1C` for each successfully processed record.

---

### 13.3. `setCurrentBalance` — Set current balance

**Description:** Sync the client's current balance. Calculates the difference between the sent and existing balance and creates a correcting transaction. If the difference is 0, no record is created.

> **Rate limit:** Maximum 3 requests per 5 minutes.

**Request:**
```json
{
    "method": "setCurrentBalance",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "currentBalance": [
            {
                "code_1C": "000000100",
                "balance": 500000,
                "paymentType": { "code_1C": "000000001" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Client identifier |
| `balance` | float | ✅ | Target current balance (number) |
| `paymentType` | object | ✅ | Payment type (currency). CS_id / SD_id / code_1C |

**Response:** `completed`, `error`, `data.currentBalance` — array of objects with `CS_id`, `SD_id`, `code_1C` for each processed client.

---

### 13.4. `setConsumption` — Create expense

**Description:** Create an expense or income record from the cashbox (consumption). Expense category is set by `name`; if missing, a new subcategory "Integration consumption" is created.

**Request:**
```json
{
    "method": "setConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "consumption": [
            {
                "name": "Transport expense",
                "summa": 500000,
                "date": "2025-06-15 16:00:00",
                "comment": "Delivery",
                "type": "debt",
                "paymentType": { "code_1C": "000000001" },
                "cashbox": { "SD_id": "d0_1" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (one for update) | Consumption record identifier |
| `name` | string | ✅ | Expense/income name (category) |
| `summa` | float | ✅ | Amount (positive number) |
| `date` | string | ❌ | Date (Y-m-d H:i:s), default current |
| `comment` | string | ❌ | Comment |
| `type` | string | ❌ | `"income"` — income, otherwise expense (debt) |
| `paymentType` | object | ✅ | Payment type (currency). CS_id / SD_id / code_1C |
| `cashbox` | object | ✅ | Cashbox. CS_id / SD_id / code_1C |

**Response:** `completed`, `error`, `data.consumption` — array with `CS_id`, `SD_id`, `code_1C` for each created record.

---

## 14. Photos

### 14.1. `setPhoto` — Upload product photo

**Description:** Upload or update a product photo. Supports image as base64.

**Request (with base64 data):**
```json
{
    "method": "setPhoto",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": { "code_1C": "000000015" },
        "imageData": "/9j/4AAQSkZJRgABAQ..."
    }
}
```

**Request (with image ID):**
```json
{
    "method": "setPhoto",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": { "code_1C": "000000015" },
        "imageId": "d0_15_abc123.jpg"
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `product` | object | ✅ | Product. CS_id / SD_id / code_1C |
| `imageData` | string | ✅* | Image in base64 |
| `imageId` | string | ✅* | ID of already uploaded file (for update/link) |

> *Specify one of: `imageData` or `imageId`.

**Limits:**
- Max size: 100KB
- Supported formats: JPEG, PNG, GIF

**Response:**
```json
{
    "status": true,
    "result": {
        "product": {
            "SD_id": "d0_15",
            "code_1C": "000000015"
        },
        "imageId": "d0_15_abc123.jpg"
    }
}
```

---

### 14.2. `getPhoto` — Get product photo

**Description:** Get product image by ID. Returns binary image data.

**Request:**
```json
{
    "method": "getPhoto",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "imageID": "d0_15_abc123.jpg"
    }
}
```

**Request fields (params):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `imageID` | string | ✅ | Image identifier (file name, e.g. from setPhoto response) |

**Response:** Binary image data with header `Content-Type: image/jpeg` (or other MIME type). No pagination.

---

## 15. Extra methods

### 15.1. `getFilials` — List filials

**Description:** Returns the list of all available filials (name, domain, prefix, filial_id for requests with filial).

**Request:**
```json
{
    "method": "getFilials",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "filials": [
            {
                "name": "Main office",
                "domain": "app.salesdoc.io",
                "prefix": "F1",
                "filial_id": "1"
            }
        ]
    }
}
```

**Response fields (filials[]):**

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Filial name |
| `domain` | string | Filial domain |
| `prefix` | string | Prefix (for CS_id) |
| `filial_id` | string | Filial code (pass in `filial.filial_id`) |

---

### 15.2. `getStoreLog` — Store operations log

**Description:** Returns the store operations log for a period. All identifiers in SD_id format only.

> **Limits:** Rate limit 5 requests per 5 seconds. Method available **only from 20:00 to 07:00** server time.

**Parameters:** `params.storeId` (warehouse SD_id), `params.from`, `params.to` (ISO-8601 dates). Optional: `params.documents` (array of types: Purchase, Order, OrderDefect, OrderReplace, Exchange, Excretion, StoreCorrector, BonusOrder), `params.products` (array of product SD_id).

**Request:**
```json
{
    "method": "getStoreLog",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "storeId": "d0_1",
        "from": "2025-06-01",
        "to": "2025-06-30",
        "documents": ["Purchase", "Order"]
    }
}
```

**Response:** Array of records in `result`: `product_id`, `document`, `document_id`, `date`, `quantity` (positive — receipt, negative — issue). See section 9.40 in [06-get-extra.md](06-get-extra.md#940-getstorelog--store-operations-log).

---

### 15.3. `getClientPending` — Client requests

**Description:** Returns the list of client create/update requests pending moderation. Supports pagination (`params.limit`, `params.page`).

**Request:**
```json
{
    "method": "getClientPending",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1
    }
}
```

**Response:** `result.clientPending` — array of requests (fields: CS_id, SD_id, code_1C, name, firmName, address, tel, category, type, channel, city, createdBy, createdAt, etc.). Response includes `pagination`. Full structure in [06-get-extra.md](06-get-extra.md#941-getclientpending--client-requests-pending).

---

### 15.4. `deleteClientPending` — Delete client request

**Description:** Delete one or more client requests from the pending list. The body contains the `clientPending` array (not inside `data`).

**Request:**
```json
{
    "method": "deleteClientPending",
    "auth": { "userId": "d0_1", "token": "..." },
    "clientPending": [
        { "SD_id": "d0_500" },
        { "code_1C": "PEND002" }
    ]
}
```

**Fields for each element in clientPending:**

| Field | Type | Required | Description |
|-------|------|:--------:|-------------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Client request identifier |

**Response:**
```json
{
    "status": true,
    "result": {
        "completed": 2,
        "error": 0,
        "data": [
            { "SD_id": "d0_500" },
            { "code_1C": "PEND002" }
        ]
    }
}
```

---

## 16. Limits and restrictions

### Default limits

| Parameter | Value | Description |
|-----------|:-----:|-------------|
| Response limit | 1000 | Max records in one response |
| Request limit | 250 | Max records in one SET request |

> Limits can be changed via server parameters.

### Rate limiting

Some methods have call frequency limits:

| Method | Interval | Max requests |
|--------|:--------:|:------------:|
| `setStock` | 5 sec | 5 |
| `setCurrentBalance` | 5 min | 3 |

When the limit is exceeded the request is rejected.

### Data requirements

- **JSON format:** Request must be valid JSON (UTF-8 encoding)
- **BOM:** UTF-8 BOM is automatically removed if present
- **Required fields:** `method` — always required
- **Empty request:** Returns error 400
- **Invalid JSON:** Returns error 400
- **active value:** Accepts: `true`, `false`, `"Y"`, `"N"`, `"True"`, `1`, `0`

---

[← Back to contents](README.md)

