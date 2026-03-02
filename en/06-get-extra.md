# GET methods — Extra (9.32–9.46)

[← Back to contents](README.md)

---

### 9.32. `getInventory` — Inventory

**Description:** Returns equipment inventory data at clients.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By inventory start date |

**Request:**
```json
{
    "method": "getInventory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "date": {
                    "from": "2025-01-01",
                    "to": "2025-12-31"
                }
            }
        }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "inventory": [
            {
                "SD_id": "d0_1",
                "code_1C": "INV001",
                "name": "Samsung refrigerator",
                "model": "RT35K5440S8",
                "serial_no": "SN123456",
                "inventory_no": "INV-001",
                "production_date": "2023-01-15",
                "from_date": "2025-03-01",
                "to_date": null,
                "state": "good",
                "comment": "In good condition",
                "type": {
                    "SD_id": "1",
                    "name": "Refrigerator",
                    "code_1C": "FRIDGE"
                },
                "agents": [
                    {
                        "SD_id": "d0_3",
                        "name": "Ivan Ivanov",
                        "code_1C": "000000003"
                    }
                ],
                "client": {
                    "SD_id": "d0_100",
                    "name": "Star Shop",
                    "code_1C": "000000100"
                },
                "city": {
                    "SD_id": "d0_1",
                    "name": "Chilanzar",
                    "code_1C": "000000001"
                }
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 25, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Equipment name |
| `model` | string | Model |
| `serial_no` | string | Serial number |
| `inventory_no` | string | Inventory number |
| `production_date` | string | Production date |
| `from_date` | string | Placement start date |
| `to_date` | string\|null | Placement end date |
| `state` | string | State (e.g. `good`) |
| `comment` | string | Comment |
| **type** | object | Equipment type |
| `type.SD_id` | string | Type server ID |
| `type.name` | string | Type name |
| `type.code_1C` | string | Type 1C code |
| **agents** | array | Agents |
| `agents[].SD_id` | string | Agent ID |
| `agents[].name` | string | Agent name |
| `agents[].code_1C` | string | Agent 1C code |
| **client** | object | Client |
| `client.SD_id` | string | Client ID |
| `client.name` | string | Client name |
| `client.code_1C` | string | Client 1C code |
| **city** | object | City |
| `city.SD_id` | string | City ID |
| `city.name` | string | City name |
| `city.code_1C` | string | City 1C code |

---

### 9.33. `getShipper` — Suppliers

**Description:** Returns the list of suppliers.

**Request:**
```json
{
    "method": "getShipper",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "shipper": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Supplier LLC",
                "phone_number": "+998901112233",
                "address": "50 Navoi St",
                "active": true,
                "code_1C": "SHIP001"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier |
| `SD_id` | string | Server ID |
| `name` | string | Supplier name |
| `phone_number` | string | Phone |
| `address` | string | Address |
| `active` | boolean | Active |
| `code_1C` | string | 1C code |

---

### 9.34. `getPaymentConfirmation` — Payment confirmation

**Description:** Returns the list of payments sent by expeditors for confirmation.

**Request:**
```json
{
    "method": "getPaymentConfirmation",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "paymentConfirmation": [
            {
                "CS_id": "F1-d0_60",
                "SD_id": "d0_60",
                "code_1C": null,
                "client": {
                    "CS_id": "F1-d0_100",
                    "SD_id": "d0_100",
                    "name": "Star Shop",
                    "code_1C": "000000100"
                },
                "order": {
                    "CS_id": "F1-d0_300",
                    "SD_id": "d0_300"
                },
                "payment_type": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "amount": 5000000.00,
                "date": "2025-06-15 14:30:00",
                "comment": "Payment on delivery",
                "confirmed": true
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 15, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string\|null | 1C code |
| **client** | object | Client |
| `client.CS_id` | string | Client identifier |
| `client.SD_id` | string | Client server ID |
| `client.name` | string | Client name |
| `client.code_1C` | string | Client 1C code |
| **order** | object | Order |
| `order.CS_id` | string | Order identifier |
| `order.SD_id` | string | Order server ID |
| **payment_type** | object | Payment type |
| `payment_type.CS_id` | string | Payment type identifier |
| `payment_type.SD_id` | string | Payment type server ID |
| `payment_type.code_1C` | string | Payment type 1C code |
| `amount` | float | Payment amount |
| `date` | string | Date and time |
| `comment` | string | Comment |
| `confirmed` | boolean | Confirmed: `true` — yes, `false` — no |

---

### 9.36. `getBonus` — Bonuses

**Description:** Returns the list of bonus programs.

**Request:**
```json
{
    "method": "getBonus",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "data": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "name": "Buy 10 get 1",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 3, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier |
| `SD_id` | string | Server ID |
| `name` | string | Bonus program name |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.37. `getPRCategory` — Photo report categories

**Description:** Returns photo report categories.

**Request:**
```json
{
    "method": "getPRCategory",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "prCategory": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Product display",
                "active": "Y"
            },
            {
                "CS_id": "2",
                "SD_id": "2",
                "name": "Signboard",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 2, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier |
| `SD_id` | string | Server ID |
| `name` | string | Category name |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.38. `getPhotoReport` — Photo reports

**Description:** Returns agent photo reports **for the current day**.

> ⚠️ This method returns only photo reports for **today's date**.

**Request:**
```json
{
    "method": "getPhotoReport",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "photoReport": [
            {
                "CS_id": "F1-d0_200",
                "SD_id": "d0_200",
                "agent_id": "d0_3",
                "client_id": "d0_100",
                "cat_id": "1",
                "role": "4",
                "url": "https://example.com/upload/photoReport/photo1.jpg",
                "date": "2025-06-15 10:15:00"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 50, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `agent_id` | string | Agent ID |
| `client_id` | string | Client ID |
| `cat_id` | string | Photo report category ID |
| `role` | string | Role (user type) |
| `url` | string | Photo URL |
| `date` | string | Photo report date and time |

---

### 9.39. `getFilials` — Filials

**Description:** Returns the list of active branches.

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
                "name": "Tashkent",
                "domain": "tashkent.example.com",
                "prefix": "F1",
                "filial_id": "FIL001"
            },
            {
                "name": "Samarkand",
                "domain": "samarkand.example.com",
                "prefix": "F2",
                "filial_id": "FIL002"
            }
        ]
    }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `name` | string | Branch name |
| `domain` | string | Branch domain |
| `prefix` | string | Branch prefix (for CS_id) |
| `filial_id` | string | Branch ID |

---

### 9.40. `getStoreLog` — Store operations log

**Description:** Returns warehouse operations log for the specified period.

> ⚠️ **Limitations:**
> - Rate limit: 5 requests per 5 seconds
> - Method available **only** from 20:00 to 07:00
> - All identifiers — `SD_id` only

**Required parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `params.storeId` | string | Warehouse SD_id |
| `params.from` | string | Start date (ISO-8601) |
| `params.to` | string | End date (ISO-8601) |

**Additional filters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `params.documents` | array | Filter by document types: `Purchase`, `Order`, `OrderReplace`, `OrderDefect`, `BonusOrder`, `StoreCorrector`, `Exchange`, `Excretion` |
| `params.products` | array | Filter by product IDs |

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

**Response:**
```json
{
    "status": true,
    "result": [
        {
            "product_id": "d0_15",
            "document": "Purchase",
            "document_id": "d0_50",
            "date": "2025-06-15 09:00:00",
            "quantity": 100.0
        },
        {
            "product_id": "d0_15",
            "document": "Order",
            "document_id": "d0_300",
            "date": "2025-06-15 14:00:00",
            "quantity": -10.0
        }
    ]
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `product_id` | string | Product SD_id |
| `document` | string | Document type (`Purchase`, `Order`, `OrderReplace`, `OrderDefect`, `BonusOrder`, `StoreCorrector`, `Exchange`, `Excretion`) |
| `document_id` | string | Document SD_id |
| `date` | string | Operation date and time |
| `quantity` | float | Quantity: positive — receipt, negative — issue |

> **Note:** Positive quantity — receipt, negative — issue.

---

### 9.41. `getClientPending` — Client requests (pending)

**Description:** Returns client creation requests pending moderation.

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

**Response:**
```json
{
    "status": true,
    "result": {
        "clientPending": [
            {
                "CS_id": "F1-d0_500",
                "SD_id": "d0_500",
                "code_1C": null,
                "name": "New Shop",
                "firmName": "New LLC",
                "address": "15 Chilanzar St",
                "waymark": "Near the pharmacy",
                "tel": "+998901234567",
                "lon": 69.24,
                "lat": 41.30,
                "allowKredit": 0,
                "allowConsig": 0,
                "comment": "Request from agent",
                "photo": null,
                "active": "Y",
                "inn": "987654321",
                "contract": null,
                "createdAt": "2025-06-15 11:00:00",
                "bankDetails": {
                    "bank": null,
                    "account": null,
                    "mfo": null
                },
                "category": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "type": {
                    "CS_id": "1",
                    "SD_id": "1",
                    "code_1C": "TP001"
                },
                "channel": {
                    "CS_id": "1",
                    "SD_id": "1",
                    "code_1C": "CH001"
                },
                "city": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "createdBy": {
                    "CS_id": "F1-d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003",
                    "name": "Ivan Ivanov"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string\|null | 1C code |
| `name` | string | Client name |
| `firmName` | string | Legal name |
| `address` | string | Address |
| `waymark` | string | Landmark |
| `tel` | string | Phone |
| `lon` | float | Longitude (GPS) |
| `lat` | float | Latitude (GPS) |
| `allowKredit` | integer | Credit allowed: `1` — yes, `0` — no |
| `allowConsig` | integer | Consignment allowed: `1` — yes, `0` — no |
| `comment` | string | Comment |
| `photo` | string\|null | Photo |
| `active` | string | Active |
| `inn` | string | INN |
| `contract` | string\|null | Contract number |
| `createdAt` | string | Request creation date |
| **bankDetails** | object | Bank details |
| **category** | object | Client category (`CS_id`, `SD_id`, `code_1C`) |
| **type** | object | Client type (`CS_id`, `SD_id`, `code_1C`) |
| **channel** | object | Sales channel (`CS_id`, `SD_id`, `code_1C`) |
| **city** | object | City (`CS_id`, `SD_id`, `code_1C`) |
| **createdBy** | object | Request creator |
| `createdBy.CS_id` | string | Creator identifier |
| `createdBy.SD_id` | string | Creator server ID |
| `createdBy.code_1C` | string | 1C code |
| `createdBy.name` | string | Creator name |

---

### 9.42. `getCorrection` — Store corrections

**Description:** Returns the list of warehouse stock corrections.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By correction date |
| `filter.store` | By warehouse (`CS_id` / `SD_id` / `code_1C`) |

**Filter examples:**

**Filter by correction date:**
```json
{
    "method": "getCorrection",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Filter by warehouse (`filter.store`):**
```json
{
    "method": "getCorrection",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "code_1C": "WH001" }
        }
    }
}
```

> Alternatives for `filter.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **SD_id** (e.g. `"d0_1"`) instead of code_1C.

**Combining filters:**
```json
{
    "method": "getCorrection",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "SD_id": "d0_1" },
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

> Alternatives for `filter.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **code_1C** (e.g. `"WH001"`) instead of SD_id.

**Response:**
```json
{
    "status": true,
    "result": {
        "correction": [
            {
                "CS_id": "F1-d0_70",
                "SD_id": "d0_70",
                "code_1C": "COR000001",
                "date": "2025-06-15 08:00:00",
                "comment": "Recalculation",
                "type": "correction",
                "store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "WH001",
                    "name": "Main warehouse"
                },
                "items": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "product_name": "Coca-Cola 1.5L",
                        "quantity": 5.0,
                        "price": 15000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 10, "page": 1 }
}
```

**Correction types:**

| Type | Description |
|------|-------------|
| `correction` | Correction |
| `inventory` | Inventory |

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `date` | string | Correction date and time |
| `comment` | string | Comment |
| `type` | string | Type: `correction` or `inventory` |
| **store** | object | Warehouse |
| `store.CS_id` | string | Warehouse identifier |
| `store.SD_id` | string | Warehouse server ID |
| `store.code_1C` | string | Warehouse 1C code |
| `store.name` | string | Warehouse name |
| **items** | array | Products array |
| `items[].CS_id` | string | Product identifier |
| `items[].SD_id` | string | Product server ID |
| `items[].code_1C` | string | Product 1C code |
| `items[].product_name` | string | Product name |
| `items[].quantity` | float | Quantity |
| `items[].price` | float | Price |

---

### 9.43. `getPurchase` — Purchases

**Description:** Returns the list of purchase documents.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By purchase date |
| `filter.period.dateUpdate.from/to` | By update date |
| `filter.store` | By warehouse (`CS_id` / `SD_id` / `code_1C`) |

**Filter examples:**

**Filter by purchase date:**
```json
{
    "method": "getPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Filter by update date (`filter.period.dateUpdate`):**
```json
{
    "method": "getPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "dateUpdate": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Filter by warehouse (`filter.store`):**
```json
{
    "method": "getPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "code_1C": "WH001" }
        }
    }
}
```

> Alternatives for `filter.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **SD_id** (e.g. `"d0_1"`) instead of code_1C.

**Combining all filters:**
```json
{
    "method": "getPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "SD_id": "d0_1" },
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

> Alternatives for `filter.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **code_1C** (e.g. `"WH001"`) instead of SD_id.

**Response:**
```json
{
    "status": true,
    "result": {
        "warehouse": [
            {
                "CS_id": "F1-d0_1",
                "SD_id": "d0_1",
                "code_1C": "WH001",
                "name": "Main warehouse",
                "date": "2025-06-15 09:00:00",
                "updated": "2025-06-15 09:00:00",
                "purchase_id": "F1-d0_50",
                "priceType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "shipper": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Supplier LLC",
                    "code_1C": "SHIP001"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "name": "Coca-Cola 1.5L",
                        "quantity": 100.0,
                        "price": 12000.00,
                        "amount": 1200000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 20, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Warehouse external identifier (with prefix) |
| `SD_id` | string | Warehouse server ID |
| `code_1C` | string | Warehouse 1C code |
| `name` | string | Warehouse name |
| `date` | string | Purchase date |
| `updated` | string | Last update date |
| `purchase_id` | string | Purchase document ID |
| **priceType** | object | Price type |
| `priceType.CS_id` | string | Price type identifier |
| `priceType.SD_id` | string | Price type server ID |
| `priceType.code_1C` | string | Price type 1C code |
| **shipper** | object | Supplier |
| `shipper.CS_id` | string | Supplier identifier |
| `shipper.SD_id` | string | Supplier server ID |
| `shipper.name` | string | Supplier name |
| `shipper.code_1C` | string | Supplier 1C code |
| **detail** | array | Products array |
| `detail[].CS_id` | string | Product identifier |
| `detail[].SD_id` | string | Product server ID |
| `detail[].code_1C` | string | Product 1C code |
| `detail[].name` | string | Product name |
| `detail[].quantity` | float | Quantity |
| `detail[].price` | float | Price per unit |
| `detail[].amount` | float | Line total |

---

### 9.44. `getMovement` — Movements

**Description:** Returns the list of transfer documents between warehouses.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By transfer date |
| `filter.include` | `sd`, `1c`, `all` — by source |
| `filter.type` | Transfer type (see table below) |

**Transfer types (`filter.type`):**

| Value | Code | Description |
|-------|------|-------------|
| *(not set)* | 1 | Regular transfer between warehouses (default) |
| `vansel` | 2 | Van-sel type transfer |

> If `filter.type` is not set — only regular transfers are returned. To get van-sel transfers, set `"type": "vansel"`.

**Filter examples:**

**Filter by date and source:**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "all",
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Filter by source (`filter.include`):**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "1c"
        }
    }
}
```
> Values: `sd` — from SalesDoc only, `1c` — from 1C only, `all` — all records.

**Filter by transfer type (`filter.type`):**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "type": "vansel"
        }
    }
}
```
> `vansel` — returns only van-sel type transfers. If not set — regular transfers are returned.

**Combining all filters:**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "all",
            "type": "vansel",
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "movement": [
            {
                "CS_id": "F1-d0_80",
                "SD_id": "d0_80",
                "code_1C": "MOV000001",
                "date": "2025-06-15 07:00:00",
                "from_store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "name": "Main warehouse",
                    "code_1C": "WH001"
                },
                "to_store": {
                    "CS_id": "F1-d0_2",
                    "SD_id": "d0_2",
                    "name": "Agent warehouse",
                    "code_1C": "WH002"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "quantity": 50.0
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 15, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `date` | string | Transfer date and time |
| **from_store** | object | Source warehouse |
| `from_store.CS_id` | string | Warehouse identifier |
| `from_store.SD_id` | string | Warehouse server ID |
| `from_store.name` | string | Warehouse name |
| `from_store.code_1C` | string | Warehouse 1C code |
| **to_store** | object | Destination warehouse |
| `to_store.CS_id` | string | Warehouse identifier |
| `to_store.SD_id` | string | Warehouse server ID |
| `to_store.name` | string | Warehouse name |
| `to_store.code_1C` | string | Warehouse 1C code |
| **detail** | array | Products array |
| `detail[].CS_id` | string | Product identifier |
| `detail[].SD_id` | string | Product server ID |
| `detail[].code_1C` | string | Product 1C code |
| `detail[].quantity` | float | Quantity |

---

### 9.45. `getVsExchange` — Warehouse exchanges

**Description:** Returns the list of product exchange documents (Van-sel exchange).

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By exchange date |
| `filter.include` | `sd`, `1c`, `all` — by source |

**Request:**
```json
{
    "method": "getVsExchange",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "all",
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "vsExchange": [
            {
                "CS_id": "F1-d0_90",
                "SD_id": "d0_90",
                "code_1C": "VSE000001",
                "total_count": 20.0,
                "total_summa": 300000.00,
                "comment": "Product exchange",
                "date": "2025-06-15 08:00:00",
                "date_load": "2025-06-15",
                "from_store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "WH001"
                },
                "to_store": {
                    "CS_id": "F1-d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "WH002"
                },
                "agent": {
                    "CS_id": "F1-d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003"
                },
                "price_type": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "quantity": 20.0,
                        "price": 15000.00,
                        "summa": 300000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `total_count` | float | Total product count |
| `total_summa` | float | Total amount |
| `comment` | string | Comment |
| `date` | string | Exchange date and time |
| `date_load` | string | Document date |
| **from_store** | object | Source warehouse |
| `from_store.CS_id` | string | Warehouse identifier |
| `from_store.SD_id` | string | Warehouse server ID |
| `from_store.code_1C` | string | Warehouse 1C code |
| **to_store** | object | Destination warehouse |
| `to_store.CS_id` | string | Warehouse identifier |
| `to_store.SD_id` | string | Warehouse server ID |
| `to_store.code_1C` | string | Warehouse 1C code |
| **agent** | object | Agent |
| `agent.CS_id` | string | Agent identifier |
| `agent.SD_id` | string | Agent server ID |
| `agent.code_1C` | string | Agent 1C code |
| **price_type** | object | Price type |
| `price_type.CS_id` | string | Price type identifier |
| `price_type.SD_id` | string | Price type server ID |
| `price_type.code_1C` | string | Price type 1C code |
| **detail** | array | Products array |
| `detail[].CS_id` | string | Product identifier |
| `detail[].SD_id` | string | Product server ID |
| `detail[].code_1C` | string | Product 1C code |
| `detail[].quantity` | float | Quantity |
| `detail[].price` | float | Price per unit |
| `detail[].summa` | float | Line amount |

---

### 9.46. `getMovementBetweenFilial` — Movements between filials

**Description:** Returns product transfers between branches.

> ⚠️ For this method `filial_id` is **required** in the request.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By transfer date |

**Request:**
```json
{
    "method": "getMovementBetweenFilial",
    "auth": { "userId": "d0_1", "token": "..." },
    "filial": { "filial_id": "FIL001" },
    "params": {
        "filter": {
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "movement": [
            {
                "CS_id": "F1-d0_95",
                "SD_id": "d0_95",
                "code_1C": "MBF000001",
                "date": "2025-06-15 07:00:00",
                "count": 30,
                "amount": 450000.00,
                "comment": "Transfer to Samarkand",
                "from_store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "name": "Main warehouse",
                    "code_1C": "WH001"
                },
                "to_filial": {
                    "CS_id": "2",
                    "SD_id": "2",
                    "name": "Samarkand",
                    "domain": "samarkand.example.com"
                },
                "to_store": {
                    "CS_id": "F2-d0_1",
                    "SD_id": "d0_1",
                    "name": "Samarkand warehouse",
                    "code_1C": "WH001-S"
                },
                "shipper": {
                    "CS_id": "1",
                    "SD_id": "1",
                    "name": "Supplier LLC",
                    "code_1C": "SHIP001"
                },
                "priceType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "name": "Retail",
                    "code_1C": "000000001"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "quantity": 30.0,
                        "price": 15000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 3, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `date` | string | Transfer date and time |
| `count` | integer | Product count |
| `amount` | float | Total amount |
| `comment` | string | Comment |
| **from_store** | object | Source warehouse |
| `from_store.CS_id` | string | Warehouse identifier |
| `from_store.SD_id` | string | Warehouse server ID |
| `from_store.name` | string | Warehouse name |
| `from_store.code_1C` | string | Warehouse 1C code |
| **to_filial** | object | Destination branch |
| `to_filial.CS_id` | string | Branch identifier |
| `to_filial.SD_id` | string | Branch server ID |
| `to_filial.name` | string | Branch name |
| `to_filial.domain` | string | Branch domain |
| **to_store** | object | Destination warehouse in target branch |
| `to_store.CS_id` | string | Warehouse identifier |
| `to_store.SD_id` | string | Warehouse server ID |
| `to_store.name` | string | Warehouse name |
| `to_store.code_1C` | string | Warehouse 1C code |
| **shipper** | object | Supplier |
| `shipper.CS_id` | string | Supplier identifier |
| `shipper.SD_id` | string | Supplier server ID |
| `shipper.name` | string | Supplier name |
| `shipper.code_1C` | string | Supplier 1C code |
| **priceType** | object | Price type |
| `priceType.CS_id` | string | Price type identifier |
| `priceType.SD_id` | string | Price type server ID |
| `priceType.name` | string | Price type name |
| `priceType.code_1C` | string | Price type 1C code |
| **detail** | array | Products array |
| `detail[].CS_id` | string | Product identifier |
| `detail[].SD_id` | string | Product server ID |
| `detail[].code_1C` | string | Product 1C code |
| `detail[].quantity` | float | Quantity |
| `detail[].price` | float | Price per unit |

---

[← Back to contents](README.md) | [Next: SET methods — References →](07-set-references.md)

