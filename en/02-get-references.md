# GET methods — References (9.1–9.20)

[← Back to contents](README.md)

All GET methods support pagination (`limit`, `page`), period and sort.

---

### 9.1. `getValyutaType` — Currency types

**Description:** Returns the list of currency types (settlement currency).

**Request:**
```json
{
    "method": "getValyutaType",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100, "page": 1 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "valyutaType": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "UZS",
                "short": "UZS",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 100, "total": 3, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Currency name |
| `short` | string | Short label |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.2. `getUnit` — Units of measure

**Description:** Returns the list of product units of measure.

**Request:**
```json
{
    "method": "getUnit",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100, "page": 1 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "unit": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "pc",
                "name": "Piece",
                "short": "pc",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 100, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Unit of measure name |
| `short` | string | Short label |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.3. `getClientCategory` — Client categories

**Description:** Returns the list of client categories.

**Filters:**

| Filter | Values | Description |
|--------|--------|-------------|
| `filter.include` | `sd`, `1c`, `all` | Filter by creation source |

**Request:**
```json
{
    "method": "getClientCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": { "include": "all" }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "clientCategory": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Wholesale",
                "description": "Wholesale buyer",
                "active": "Y",
                "sort": 500
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 10, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Category name |
| `description` | string | Category description |
| `active` | string | Active: `Y` — active, `N` — inactive |
| `sort` | integer | Sort order |

---

### 9.4. `getProduct` — Products

**Description:** Returns the list of products with full info: category, subcategory, unit of measure, brand and group.

**Filters:**

| Filter | Description |
|--------|-------------|
| `filter.products.SD_id` | Array of product IDs |
| `filter.products.code_1C` | Array of 1C codes |
| `filter.trade.CS_id` | Trade direction ID |
| `filter.trade.SD_id` | Trade direction SD_id |
| `filter.trade.code_1C` | Trade direction 1C code |

**Request (filter by product list):**
```json
{
    "method": "getProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "products": {
                "SD_id": ["d0_10", "d0_11"]
            }
        }
    }
}
```

> Alternative for `filter.products`: you can use **code_1C** (array, e.g. `["000001", "000002"]`) instead of SD_id.

**Request (filter by trade direction):**
```json
{
    "method": "getProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "trade": {
                "CS_id": "F1-d0_5"
            }
        }
    }
}
```

> Alternatives for `filter.trade`: you can use **SD_id** (e.g. `"d0_5"`) or **code_1C** (e.g. `"TD001"`) instead of CS_id.

**Response:**
```json
{
    "status": true,
    "result": {
        "product": [
            {
                "CS_id": "d0_15",
                "SD_id": "d0_15",
                "code_1C": "000000015",
                "name": "Coca-Cola 1.5L",
                "active": "Y",
                "sort": 100,
                "volume": 0.5,
                "packQuantity": 6,
                "barCode": "4780001234567",
                "weight": 1.5,
                "imageID": "d0_15_abc123.jpg",
                "imageUrl": "/upload/adtProduct/d0_15_abc123.jpg",
                "thumbUrl": "/upload/adtProduct/thumbs/150x150_d0_15_abc123.jpg",
                "sapCode": null,
                "ikpu": null,
                "part_number": null,
                "category": {
                    "CS_id": "d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003"
                },
                "subCategory": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "unit": {
                    "CS_id": "d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "pc"
                },
                "brand": {
                    "CS_id": "1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "trade": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "group": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "GRP001"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 1500, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Product name |
| `active` | string | Active: `Y` — active, `N` — inactive |
| `sort` | integer | Sort order |
| `volume` | float | Product volume |
| `packQuantity` | integer | Quantity per pack |
| `barCode` | string | Barcode |
| `weight` | float | Product weight |
| `imageID` | string | Image file name |
| `imageUrl` | string | Full image URL |
| `thumbUrl` | string | Thumbnail URL |
| `sapCode` | string\|null | SAP code |
| `ikpu` | string\|null | IKPU code |
| `part_number` | string\|null | Part number |
| **category** | object | Product category |
| `category.CS_id` | string | Category identifier |
| `category.SD_id` | string | Category server ID |
| `category.code_1C` | string | Category 1C code |
| **subCategory** | object | Product subcategory |
| `subCategory.CS_id` | string | Subcategory identifier |
| `subCategory.SD_id` | string | Subcategory server ID |
| `subCategory.code_1C` | string | Subcategory 1C code |
| **unit** | object | Unit of measure |
| `unit.CS_id` | string | Unit identifier |
| `unit.SD_id` | string | Unit server ID |
| `unit.code_1C` | string | Unit 1C code |
| **brand** | object | Brand |
| `brand.CS_id` | string | Brand identifier |
| `brand.SD_id` | string | Brand server ID |
| `brand.code_1C` | string\|null | Brand 1C code |
| **trade** | object | Trade direction |
| `trade.CS_id` | string | Trade direction identifier |
| `trade.SD_id` | string | Trade direction server ID |
| `trade.code_1C` | string\|null | Trade direction 1C code |
| **group** | object | Product group |
| `group.CS_id` | string | Group identifier |
| `group.SD_id` | string | Group server ID |
| `group.code_1C` | string | Group 1C code |

---

### 9.5. `getProductCategory` — Product categories

**Description:** Returns the list of product categories.

**Request:**
```json
{
    "method": "getProductCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "productCategory": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Beverages",
                "active": "Y",
                "unit": {
                    "CS_id": "d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "pc"
                }
            }
        ]
    },
    "pagination": { "limit": 100, "total": 20, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Category name |
| `active` | string | Active: `Y` — active, `N` — inactive |
| **unit** | object | Default unit of measure |
| `unit.CS_id` | string | Unit identifier |
| `unit.SD_id` | string | Unit server ID |
| `unit.code_1C` | string | Unit 1C code |

---

### 9.6. `getProductSubCategory` — Product subcategories

**Description:** Returns the list of product subcategories.

**Request:**
```json
{
    "method": "getProductSubCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "subCategory": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Carbonated",
                "active": "Y",
                "unit": {
                    "CS_id": "d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "pc"
                }
            }
        ]
    },
    "pagination": { "limit": 100, "total": 15, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Subcategory name |
| `active` | string | Active: `Y` — active, `N` — inactive |
| **unit** | object | Default unit of measure |
| `unit.CS_id` | string | Unit identifier |
| `unit.SD_id` | string | Unit server ID |
| `unit.code_1C` | string | Unit 1C code |

---

### 9.7. `getProductGroup` — Product groups

**Description:** Returns the list of product groups.

**Request:**
```json
{
    "method": "getProductGroup",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "productGroup": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "GRP001",
                "name": "Group A",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 100, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Group name |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.8. `getPriceType` — Price types

**Description:** Returns the list of price types with linked currencies and payment types.

**Request:**
```json
{
    "method": "getPriceType",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 50 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "priceType": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Retail price (UZS)",
                "active": "Y",
                "valyutaType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "UZS"
                },
                "paymentType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 6, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Price type name |
| `active` | string | Active: `Y` — active, `N` — inactive |
| **valyutaType** | object | Currency type |
| `valyutaType.CS_id` | string | Currency identifier |
| `valyutaType.SD_id` | string | Currency server ID |
| `valyutaType.code_1C` | string | Currency 1C code |
| **paymentType** | object | Payment type |
| `paymentType.CS_id` | string | Payment type identifier |
| `paymentType.SD_id` | string | Payment type server ID |
| `paymentType.code_1C` | string | Payment type 1C code |

---

### 9.9. `getPrice` — Prices

**Description:** Returns prices of all products for the given price type.

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|:--------:|-------------|
| `params.priceType.SD_id` | string | ✅* | Price type ID |
| `params.priceType.code_1C` | string | ✅* | Price type 1C code |

> *At least one of the identifiers must be specified.

**Request:**
```json
{
    "method": "getPrice",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "priceType": {
            "CS_id": "F1-d0_1",
            // or use SD_id
            // "SD_id": "d0_1",
            // or use code_1C
            // "code_1C": "000000001"
        }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": [
        {
            "product": {
                "CS_id": "d0_15",
                "SD_id": "d0_15",
                "code_1C": "000000015"
            },
            "price": 15000.00
        }
    ]
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| **product** | object | Product object |
| `product.CS_id` | string | Product identifier |
| `product.SD_id` | string | Product server ID |
| `product.code_1C` | string | Product 1C code |
| `price` | float | Product price |

---

### 9.10. `getClient` — Clients

**Description:** Returns the list of clients with full info: category, type, channel, territory, expeditor, agents with visit days.

**Filters:**

| Filter | Description |
|--------|-------------|
| `filter.period.update.from/to` | Period by update date |
| `filter.period.create.from/to` | Period by creation date |
| `sortBy.name` | Sort: `updatedAt` or `createdAt` |
| `sortBy.asc` | `true` — ASC, `false` — DESC |

**Conditional filtering:**

In the `filter` block you can specify conditions by `CS_id`, `SD_id`, `code_1C` and `active` to select specific clients.

**Filter examples:**

**Filter by update date (`filter.period.update`):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "period": {
                "update": {
                    "from": "2025-01-01",
                    "to": "2025-12-31"
                }
            }
        },
        "sortBy": {
            "name": "updatedAt",
            "asc": false
        }
    }
}
```

**Filter by creation date (`filter.period.create`):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "period": {
                "create": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        },
        "sortBy": {
            "name": "createdAt",
            "asc": true
        }
    }
}
```

**Sorting (`sortBy`):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "sortBy": {
            "name": "updatedAt",
            "asc": true
        }
    }
}
```
> `name` — sort field: `updatedAt` or `createdAt`. `asc` — `true` for ASC, `false` for DESC.

**Conditional filter (by specific client):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "CS_id": "F1-d0_1"
        }
    }
}
```

> Alternatives for client filter: you can use **SD_id** (e.g. `"d0_1"`) or **code_1C** (e.g. `"000000100"`) instead of CS_id.

**Filter by activity:**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "active": "Y"
        }
    }
}
```
> `active` — `Y` for active, `N` for inactive clients.

**Combining filters:**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 100,
        "page": 1,
        "filter": {
            "active": "Y",
            "period": {
                "update": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        },
        "sortBy": {
            "name": "updatedAt",
            "asc": false
        }
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "client": [
            {
                "CS_id": "F1-d0_100",
                "SD_id": "d0_100",
                "code_1C": "000000100",
                "name": "Star Shop",
                "firmName": "Star LLC",
                "address": "5 Amir Temur St",
                "waymark": "Opposite the bank",
                "tel": "+998901234567",
                "lon": 69.2401,
                "lat": 41.2995,
                "allowKredit": 1,
                "allowConsig": 0,
                "comment": "Key client",
                "photo": null,
                "active": "Y",
                "inn": "123456789",
                "contract": null,
                "updatedAt": "2025-06-15 10:30:00",
                "createdAt": "2024-01-10 08:00:00",
                "nspCode": null,
                "bankDetails": {
                    "bank": "Asaka Bank",
                    "account": "20208000900123456789",
                    "mfo": "00444"
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
                "expeditor": {
                    "CS_id": "F1-d0_5",
                    "SD_id": "d0_5",
                    "code_1C": "000000005"
                },
                "agents": [
                    {
                        "id": "d0_3",
                        "code": "000000003",
                        "days": [1, 3, 5]
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 1200, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Client external identifier (with branch prefix) |
| `SD_id` | string | Client server ID |
| `code_1C` | string | Client 1C code |
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
| `photo` | string\|null | Client photo |
| `active` | string | Active: `Y` — active, `N` — inactive |
| `inn` | string | Client INN |
| `contract` | string\|null | Contract number |
| `updatedAt` | string | Last update date |
| `createdAt` | string | Creation date |
| `nspCode` | string\|null | NSP code |
| **bankDetails** | object | Bank details |
| `bankDetails.bank` | string | Bank name |
| `bankDetails.account` | string | Account number |
| `bankDetails.mfo` | string | Bank MFO |
| **category** | object | Client category |
| `category.CS_id` | string | Category identifier |
| `category.SD_id` | string | Category server ID |
| `category.code_1C` | string | Category 1C code |
| **type** | object | Client type |
| `type.CS_id` | string | Type identifier |
| `type.SD_id` | string | Type server ID |
| `type.code_1C` | string | Type 1C code |
| **channel** | object | Sales channel |
| `channel.CS_id` | string | Channel identifier |
| `channel.SD_id` | string | Channel server ID |
| `channel.code_1C` | string | Channel 1C code |
| **city** | object | City |
| `city.CS_id` | string | City identifier |
| `city.SD_id` | string | City server ID |
| `city.code_1C` | string | City 1C code |
| **expeditor** | object | Expeditor |
| `expeditor.CS_id` | string | Expeditor identifier |
| `expeditor.SD_id` | string | Expeditor server ID |
| `expeditor.code_1C` | string | Expeditor 1C code |
| **agents** | array | Attached agents |
| `agents[].id` | string | Agent ID |
| `agents[].code` | string | Agent 1C code |
| `agents[].days` | array | Visit days (1=Mon, 2=Tue, ..., 7=Sun) |

---

### 9.11. `getAgent` — Agents

**Description:** Returns the list of agents with linked users.

**Request:**
```json
{
    "method": "getAgent",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 50 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "agent": [
            {
                "CS_id": "F1-d0_3",
                "SD_id": "d0_3",
                "code_1C": "000000003",
                "name": "Ivan Ivanov",
                "active": "Y",
                "user": [
                    {
                        "CS_id": "F1-d0_10",
                        "SD_id": "d0_10",
                        "code_1C": null,
                        "login": "tp-3-1700000000"
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 15, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | Agent 1C code |
| `name` | string | Agent name |
| `active` | string | Active: `Y` — active, `N` — inactive |
| **user** | array | Linked users |
| `user[].CS_id` | string | User identifier |
| `user[].SD_id` | string | User server ID |
| `user[].code_1C` | string\|null | User 1C code |
| `user[].login` | string | User login |

---

### 9.12. `getExpeditor` — Expeditors

**Description:** Returns the list of expeditors.

**Request:**
```json
{
    "method": "getExpeditor",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 50 }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "expeditor": [
            {
                "CS_id": "F1-d0_5",
                "SD_id": "d0_5",
                "code_1C": "000000005",
                "name": "Peter Petrov",
                "tel": "+998901112233",
                "address": "10 Navoi St",
                "active": "Y",
                "user": [
                    {
                        "CS_id": "F1-d0_20",
                        "SD_id": "d0_20",
                        "code_1C": null,
                        "login": "exp-5"
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 8, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | Expeditor 1C code |
| `name` | string | Expeditor name |
| `tel` | string | Phone |
| `address` | string | Address |
| `active` | string | Active: `Y` — active, `N` — inactive |
| **user** | array | Linked users |
| `user[].CS_id` | string | User identifier |
| `user[].SD_id` | string | User server ID |
| `user[].code_1C` | string\|null | User 1C code |
| `user[].login` | string | User login |

---

### 9.13. `getSupervisor` — Supervisors

**Description:** Returns the list of supervisors with attached agents.

**Request:**
```json
{
    "method": "getSupervisor",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "supervisor": [
            {
                "CS_id": "F1-d0_30",
                "SD_id": "d0_30",
                "code_1C": null,
                "name": "Alexey Sidorov",
                "active": "Y",
                "agents": [
                    {
                        "CS_id": "F1-d0_3",
                        "SD_id": "d0_3",
                        "code_1C": "000000003",
                        "name": "Ivan Ivanov"
                    }
                ]
            }
        ]
    }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string\|null | 1C code |
| `name` | string | Supervisor name |
| `active` | string | Active: `Y` — active, `N` — inactive |
| **agents** | array | Attached agents |
| `agents[].CS_id` | string | Agent identifier |
| `agents[].SD_id` | string | Agent server ID |
| `agents[].code_1C` | string | Agent 1C code |
| `agents[].name` | string | Agent name |

---

### 9.14. `getPaymentType` — Payment types

**Description:** Returns the list of payment types (settlement currencies).

**Request:**
```json
{
    "method": "getPaymentType",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "currency": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Cash (UZS)",
                "short": "cash",
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
| `CS_id` | string | Identifier (with prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Payment type name |
| `short` | string | Short label |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.15. `getPayment` — Payments

**Description:** Returns the list of financial operations (payments, balances, debts).

**Filters:**

| Filter | Description |
|--------|-------------|
| `filter.period.date.from/to` | Period by transaction date |
| `filter.period.dateUpdate.from/to` | Period by update date |
| `filter.trade` | Filter by trade direction (`CS_id` / `SD_id`) |
| `filter.paymentType` | Filter by payment type (`CS_id` / `SD_id` / `code_1C`) |
| `filter.transactionType` | Transaction type (see table below) |
| `filter.CS_id` / `filter.SD_id` / `filter.code_1C` | Filter by specific payment |

**Transaction types:**

| Code | Description |
|------|-------------|
| 1 | Order |
| 2 | Debt |
| 3 | Payment |
| 4 | Conversion |
| 6 | Opening balance |
| 7 | Payout to client |
| 8 | Debt write-off |
| 9 | Shelf return |

**Request (filter by transaction type and date period):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "transactionType": 3,
            "period": {
                "date": {
                    "from": "2025-01-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Request (filter by update period):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
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

**Request (filter by trade direction):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "trade": {
                "CS_id": "F1-1"
            }
        }
    }
}
```

> Alternatives for `filter.trade`: you can use **SD_id** (e.g. `"d0_1"`) or **code_1C** (e.g. `"TD001"`) instead of CS_id.

**Request (filter by payment type):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "paymentType": {
                "code_1C": "000000001"
            }
        }
    }
}
```

> Alternatives for `filter.paymentType`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **SD_id** (e.g. `"d0_1"`) instead of code_1C.

**Request (combining multiple filters):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "transactionType": 3,
            "trade": {
                "SD_id": "1"
            },
            "paymentType": {
                "code_1C": "000000001"
            },
            "period": {
                "date": {
                    "from": "2025-01-01",
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
        "payment": [
            {
                "CS_id": "F1-d0_500",
                "SD_id": "d0_500",
                "code_1C": "PAY000001",
                "paymentDate": "2025-03-15 14:30:00",
                "expirationDate": null,
                "amount": 5000000.00,
                "comment": "Payment for goods",
                "transactionType": 3,
                "paymentType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "client": {
                    "CS_id": "F1-d0_100",
                    "SD_id": "d0_100",
                    "code_1C": "000000100"
                },
                "agent": {
                    "CS_id": "F1-d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003"
                },
                "cashbox": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "trade": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "orders": [
                    {
                        "CS_id": "F1-d0_300",
                        "SD_id": "d0_300",
                        "amount": 5000000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 200, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `paymentDate` | string | Payment date and time |
| `expirationDate` | string\|null | Expiration date |
| `amount` | float | Payment amount |
| `comment` | string | Comment |
| `transactionType` | integer | Transaction type (1–9, see type table) |
| **paymentType** | object | Payment type |
| `paymentType.CS_id` | string | Payment type identifier |
| `paymentType.SD_id` | string | Payment type server ID |
| `paymentType.code_1C` | string | Payment type 1C code |
| **client** | object | Client |
| `client.CS_id` | string | Client identifier |
| `client.SD_id` | string | Client server ID |
| `client.code_1C` | string | Client 1C code |
| **agent** | object | Agent |
| `agent.CS_id` | string | Agent identifier |
| `agent.SD_id` | string | Agent server ID |
| `agent.code_1C` | string | Agent 1C code |
| **cashbox** | object | Cashbox |
| `cashbox.CS_id` | string | Cashbox identifier |
| `cashbox.SD_id` | string | Cashbox server ID |
| `cashbox.code_1C` | string\|null | Cashbox 1C code |
| **trade** | object | Trade direction |
| `trade.CS_id` | string | Trade direction identifier |
| `trade.SD_id` | string | Trade direction server ID |
| `trade.code_1C` | string\|null | Trade direction 1C code |
| **orders** | array | Linked orders |
| `orders[].CS_id` | string | Order identifier |
| `orders[].SD_id` | string | Order server ID |
| `orders[].amount` | float | Order payment amount |

---

### 9.16. `getTerritory` — Territories

**Description:** Returns regions and cities.

**Request:**
```json
{
    "method": "getTerritory",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "region": [
            {
                "CS_id": "F1-d0_13",
                "SD_id": "d0_13",
                "code_1C": "000000013",
                "name": "Tashkent",
                "active": "Y",
                "sort": 500
            }
        ],
        "city": [
            {
                "CS_id": "F1-d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Chilanzar",
                "active": "Y",
                "sort": 100,
                "region": {
                    "CS_id": "F1-d0_13",
                    "SD_id": "d0_13",
                    "code_1C": "000000013"
                }
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 50, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| **region** | array | Regions |
| `region[].CS_id` | string | Region identifier |
| `region[].SD_id` | string | Region server ID |
| `region[].code_1C` | string | Region 1C code |
| `region[].name` | string | Region name |
| `region[].active` | string | Active: `Y` — active, `N` — inactive |
| `region[].sort` | integer | Sort order |
| **city** | array | Cities |
| `city[].CS_id` | string | City identifier |
| `city[].SD_id` | string | City server ID |
| `city[].code_1C` | string | City 1C code |
| `city[].name` | string | City name |
| `city[].active` | string | Active: `Y` — active, `N` — inactive |
| `city[].sort` | integer | Sort order |
| `city[].region` | object | Parent region (`CS_id`, `SD_id`, `code_1C`) |

---

### 9.17. `getBrand` — Brands

**Description:** Returns the list of brands.

**Request:**
```json
{
    "method": "getBrand",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "brand": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Coca-Cola",
                "sort": 100,
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 10, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier |
| `SD_id` | string | Server ID |
| `name` | string | Brand name |
| `sort` | integer | Sort order |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.18. `getTradeDirection` — Trade directions

**Description:** Returns the list of trade directions.

**Request:**
```json
{
    "method": "getTradeDirection",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "tradeDirection": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Main direction",
                "desc": "Description",
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
| `name` | string | Direction name |
| `desc` | string | Description |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.19. `getClientChannel` — Sales channels

**Description:** Returns the list of client sales channels.

**Request:**
```json
{
    "method": "getClientChannel",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": [
        {
            "CS_id": "1",
            "SD_id": "1",
            "code_1C": "CH001",
            "name": "Retail"
        }
    ]
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Sales channel name |

---

### 9.20. `getClientType` — Client types

**Description:** Returns the list of client types.

**Request:**
```json
{
    "method": "getClientType",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": [
        {
            "CS_id": "1",
            "SD_id": "1",
            "code_1C": "TP001",
            "name": "VIP",
            "color": "#FF0000",
            "active": "Y"
        }
    ]
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Identifier |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `name` | string | Client type name |
| `color` | string | Color (HEX format, e.g. `#FF0000`) |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

[← Back to contents](README.md) | [Next: Visits, Warehouses, Stock →](03-get-visits-warehouse.md)

