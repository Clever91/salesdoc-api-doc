# SET methods — References (10.1–10.21)

[← Back to contents](README.md)

All SET methods create or update records. Data is sent in the `data` block. The response includes the number of successfully processed (`completed`) and failed (`error`) records.

### Common SET response format:
```json
{
    "status": true,
    "result": {
        "completed": 3,
        "error": 0,
        "data": {
            "key": [
                {
                    "CS_id": "...",
                    "SD_id": "...",
                    "code_1C": "..."
                }
            ]
        }
    }
}
```

### Record identification in all SET methods

In all SET methods you can use **CS_id**, **SD_id** or **code_1C** to find an existing record (or nested objects: payment type, currency, category, etc.). The system looks up in this order:

1. **CS_id** — unique identifier in our system (search priority).
2. **SD_id** — internal server ID (if CS_id is not set or not found).
3. **code_1C** — external code (XML_ID from 1C); **not unique** in our system.

If no record is found by the given identifiers, a new record is created. For nested objects (e.g. `paymentType`, `valyutaType`, `category`) the same order applies: CS_id → SD_id → code_1C.

---

### 10.1. `setValyutaType` — Create/update currency types

**Description:** Create or update currency types.

**Request:**
```json
{
    "method": "setValyutaType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "valyutaType": [
            {
                "CS_id": "",
                "SD_id": "",
                "code_1C": "000000001",
                "name": "UZS",
                "short": "UZS",
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Name |
| `short` | string | ❌ | Short label |
| `active` | bool/string | ❌ | Active |

---

### 10.2. `setUnit` — Create/update units of measure

**Request:**
```json
{
    "method": "setUnit",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "unit": [
            {
                "code_1C": "pc",
                "name": "Piece",
                "short": "pc",
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Name |
| `short` | string | ❌ | Short label |
| `active` | bool/string | ❌ | Active |

---

### 10.3. `setClientCategory` — Create/update client categories

**Request:**
```json
{
    "method": "setClientCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "clientCategory": [
            {
                "code_1C": "000000001",
                "name": "Wholesale",
                "description": "Wholesale buyer",
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Category name |
| `description` | string | ❌ | Description |
| `active` | bool/string | ❌ | Active |

---

### 10.4. `setClientChannel` — Create/update sales channels

**Request:**
```json
{
    "method": "setClientChannel",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "clientChannel": [
            {
                "code_1C": "CH001",
                "name": "Retail"
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Sales channel name |

---

### 10.5. `setClientType` — Create/update client types

**Request:**
```json
{
    "method": "setClientType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": [
        {
            "SD_id": "",
            "code_1C": "TP001",
            "name": "VIP",
            "color": "#FF0000",
            "active": true
        }
    ]
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Client type name |
| `color` | string | ❌ | Color (e.g. `#FF0000`) |
| `active` | bool/string | ❌ | Active |

> **Note:** In this method data is passed directly in `data` as an array (not a nested object).

---

### 10.6. `setProduct` — Create/update products

**Description:** Create or update products. When creating, product category can be created automatically.

**Request:**
```json
{
    "method": "setProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": [
            {
                "code_1C": "000000015",
                "name": "Coca-Cola 1.5L",
                "active": true,
                "sort": 100,
                "volume": 0.5,
                "packQuantity": 6,
                "barCode": "4780001234567",
                "weight": 1.5,
                "part_number": null,
                "category": {
                    "code_1C": "000000003",
                    "name": "Beverages"
                },
                "unit": {
                    "code_1C": "CODE_000001"
                },
                "subCategory": {
                    "code_1C": "000000001"
                },
                "group": {
                    "code_1C": "GRP001"
                },
                "brand": {
                    "SD_id": "1"
                }
            }
        ]
    }
}
```

> For nested objects (`unit`, `category`, `subCategory`, `group`, `brand`) you can use **CS_id**, **SD_id** or **code_1C**. Examples for `unit`:
> - by code_1C: `"unit": { "code_1C": "CODE_000001" }`
> - by SD_id: `"unit": { "SD_id": "d0_2" }`
> - by CS_id: `"unit": { "CS_id": "F1-d0_2" }`

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ (one of) | Identifier |
| `name` | string | ✅ | Product name |
| `active` | bool | ❌ | Active (default `N` for new) |
| `sort` | int | ❌ | Sort order |
| `volume` | float | ❌ | Volume |
| `packQuantity` | float | ❌ | Quantity per pack |
| `barCode` | string | ❌ | Barcode |
| `weight` | float | ❌ | Weight |
| `part_number` | string | ❌ | Part number |
| `category` | object | ❌ | Category (if not found — created or set to "Other") |
| `unit` | object | ✅ | Unit of measure (required; use `CS_id` / `SD_id` / `code_1C`) |
| `subCategory` | object | ❌ | Subcategory |
| `group` | object | ❌ | Product group |
| `brand` | object | ❌ | Brand |

---

### 10.7. `setProductCategory` — Create/update product categories

**Request:**
```json
{
    "method": "setProductCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "productCategory": [
            {
                "code_1C": "000000003",
                "name": "Beverages",
                "active": true,
                "sort": 100,
                "unit": {
                    "code_1C": "CODE_000001"
                }
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Category name |
| `active` | bool/string | ❌ | Active |
| `sort` | int | ❌ | Sort order |
| `unit` | object | ❌ | Unit of measure (`code_1C` / `SD_id`) |

---

### 10.8. `setProductSubCategory` — Create/update product subcategories

**Request:**
```json
{
    "method": "setProductSubCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "subCategory": [
            {
                "code_1C": "000000001",
                "name": "Carbonated",
                "active": true,
                "sort": 100,
                "unit": {
                    "code_1C": "CODE_000001"
                }
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Subcategory name |
| `active` | bool/string | ❌ | Active |
| `sort` | int | ❌ | Sort order |
| `unit` | object | ❌ | Unit of measure (`code_1C` / `SD_id`) |

---

### 10.9. `setProductGroup` — Create/update product groups

**Request:**
```json
{
    "method": "setProductGroup",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "productGroup": [
            {
                "code_1C": "GRP001",
                "name": "Group A",
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Group name |
| `active` | bool/string | ❌ | Active |

---

### 10.10. `setPaymentType` — Create/update payment types

**Request:**
```json
{
    "method": "setPaymentType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "paymentType": [
            {
                "code_1C": "000000001",
                "name": "Cash (UZS)",
                "short": "cash",
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Payment type name |
| `short` | string | ❌ | Short label |
| `active` | bool/string | ❌ | Active |

---

### 10.11. `setPriceType` — Create/update price types

**Description:** Create price types linked to currency and payment type. The method can also create currencies.

**Request:**
```json
{
    "method": "setPriceType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "valyutaType": [
            {
                "code_1C": "UZS",
                "name": "UZS",
                "short": "UZS",
                "active": true
            }
        ],
        "priceType": [
            {
                "code_1C": "000000001",
                "name": "Retail price",
                "active": true,
                "forClient": true,
                "paymentType": {
                    "code_1C": "000000001"
                },
                "valyutaType": {
                    "code_1C": "UZS"
                }
            }
        ]
    }
}
```

> For nested objects (`paymentType`, `valyutaType`, etc.) you can also use **CS_id**, **SD_id** or **code_1C**. Search order: CS_id first (unique in system), then SD_id, then code_1C (not unique in system).

**Element fields (valyutaType):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Currency name |
| `short` | string | ❌ | Short label |
| `active` | bool/string | ❌ | Active |

**Element fields (priceType):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Price type name |
| `active` | bool | ❌ | Active |
| `forClient` | bool | ❌ | Available for client |
| `paymentType` | object | ✅ | Payment type (settlement currency) |
| `valyutaType` | object | ✅ | Currency type |


### 10.13. `setPrice` — Set prices

**Description:** Set or update product prices for a given price type.

**Request:**
```json
{
    "method": "setPrice",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": [
            {
                "code_1C": "000000015",
                "document_1C": "DOC001",
                "priceType": {
                    "code_1C": "000000001",
                    "price": 15000.00
                }
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Product ID |
| `document_1C` | string | ❌ | 1C document code |
| `priceType` | object | ✅ | Price type + new price |
| `priceType.code_1C` / `priceType.SD_id` | string | ✅ | Price type ID |
| `priceType.price` | float | ✅ | Price |

---

### 10.15. `setAgent` — Create/update agents

**Description:** Create or update agents. When creating a new agent a user is created automatically (ROLE=4).

**Request:**
```json
{
    "method": "setAgent",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "agent": [
            {
                "code_1C": "000000003",
                "name": "Ivan Ivanov",
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Agent full name |
| `active` | bool/string | ❌ | Active |

---

### 10.16. `setExpeditor` — Create/update expeditors

**Description:** Create or update expeditors. When creating a new one a user is created automatically (ROLE=10).

**Request:**
```json
{
    "method": "setExpeditor",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "expeditor": [
            {
                "code_1C": "000000005",
                "name": "Peter Petrov",
                "active": true,
                "address": "10 Navoi St",
                "tel": "+998901112233",
                "email": "petrov@mail.com",
                "date_birth": "1990-05-15"
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Expeditor full name |
| `active` | bool/string | ❌ | Active |
| `address` | string | ❌ | Address |
| `tel` | string | ❌ | Phone |
| `email` | string | ❌ | Email |
| `date_birth` | string | ❌ | Date of birth |

---

### 10.17. `setClient` — Create/update clients

**Description:** Create or update clients. Includes linking agents and visit days.

**Request:**
```json
{
    "method": "setClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "client": [
            {
                "code_1C": "000000100",
                "shortName": "Star Shop",
                "firmName": "Star LLC",
                "address": "5 Amir Temur St",
                "orentir": "Opposite the bank",
                "tel": "+998901234567",
                "comment": "Key client",
                "inn": "123456789",
                "pinfl": null,
                "nspCode": null,
                "active": true,
                "lat": 41.2995,
                "lon": 69.2401,
                "clientCategory": {
                    "code_1C": "000000001"
                },
                "clientType": {
                    "code_1C": "TP001"
                },
                "clientChannel": {
                    "code_1C": "CH001"
                },
                "territory": {
                    "code_1C": "000000001"
                },
                "priceTypeList": [
                    { "code_1C": "000000001" }
                ],
                "bankDetails": {
                    "bankName": "Asaka Bank",
                    "accountNumber": "20208000900123456789",
                    "bankCode": "00444"
                },
                "agent": [
                    {
                        "code_1C": "000000003",
                        "visitDays": [1, 3, 5]
                    }
                ],
                "deleteVisits": "false"
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Identifier |
| `shortName` | string | ✅ | Short name |
| `firmName` | string | ❌ | Company name |
| `address` | string | ❌ | Address |
| `orentir` | string | ❌ | Landmark |
| `tel` | string | ❌ | Phone |
| `comment` | string | ❌ | Comment |
| `inn` | string | ❌ | INN |
| `pinfl` | string | ❌ | PINFL |
| `nspCode` | string | ❌ | NSP code |
| `active` | bool | ❌ | Active |
| `lat` / `lon` | float | ❌ | GPS coordinates |
| `clientCategory` | object | ✅ | Client category (required) |
| `clientType` | object | ❌ | Client type |
| `clientChannel` | object | ❌ | Sales channel |
| `territory` | object | ❌ | Territory (city) |
| `priceTypeList` | array | ❌ | List of price types for client |
| `bankDetails` | object | ❌ | Bank details |
| `agent` | array/object | ❌ | Agent(s) with visit days |
| `agent.visitDays` | array | ❌ | Visit days [1=Mon, ..., 7=Sun] |
| `deleteVisits` | string | ❌ | `"true"` — delete old visits before update |

---

### 10.18. `setClientPhone` — Update client phones

**Description:** Set additional client phone numbers (max 2). All old numbers are removed.

**Request:**
```json
{
    "method": "setClientPhone",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "client_phone": [
            {
                "code_1C": "000000100",
                "phone_numbers": ["+998901234567", "+998901234568"]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Client identifier |
| `phone_numbers` | array | ✅ | Array of phones (max 2 items) |

---

### 10.19. `setTerritory` — Create/update territories

**Request:**
```json
{
    "method": "setTerritory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "city": [
            {
                "code_1C": "000000001",
                "name": "Chilanzar",
                "active": true,
                "lat": 41.2995,
                "lng": 69.2401
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Territory (city) name |
| `active` | bool/string | ❌ | Active |
| `lat` / `lng` | float | ❌ | GPS coordinates |

---

### 10.20. `setBrand` — Create/update brands

**Request:**
```json
{
    "method": "setBrand",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "brand": [
            {
                "CS_id": "",
                "SD_id": "",
                "name": "Coca-Cola",
                "sort": 100,
                "active": true
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Record identifier |
| `name` | string | ✅ | Brand name |
| `sort` | int | ❌ | Sort order |
| `active` | bool/string | ❌ | Active |

---

### 10.21. `setVisit` — Create/update visits

**Description:** Create or update agent visits to clients. Includes GPS check.

**Request:**
```json
{
    "method": "setVisit",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "visit": [
            {
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "date": "2025-06-15 10:00:00",
                "visited": 1,
                "planned": 1,
                "has_photo_report": 0,
                "has_order": 1,
                "start_datetime": "2025-06-15 10:00:00",
                "end_datetime": "2025-06-15 10:30:00",
                "lat": 41.2995,
                "lng": 69.2401
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `client` | object | ✅ | Client (`CS_id` / `SD_id` / `code_1C`) |
| `agent` | object | ✅ | Agent (`CS_id` / `SD_id` / `code_1C`) |
| `date` | string | ✅ | Visit date and time |
| `visited` | int | ❌ | Actual visit: `1` — yes, `0` — no |
| `planned` | int | ❌ | Planned visit: `1` — yes, `0` — no |
| `has_photo_report` | int | ❌ | Photo report present |
| `has_order` | int | ❌ | Order present |
| `start_datetime` / `end_datetime` | string | ❌ | Start/end time |
| `lat` / `lng` | float | ❌ | GPS coordinates |

---

[← Back to contents](README.md) | [Next: Warehouse and Orders — SET →](08-set-warehouse-orders.md)

