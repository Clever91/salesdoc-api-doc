# GET methods — Visits, Warehouses, Stock (9.21–9.25)

[← Back to contents](README.md)

---

### 9.21. `getVisit` — Visits

**Description:** Returns agent visit data for clients. Includes planned and actual visits, GPS status, orders and photo reports.

**Filters:**

| Filter | Description |
|--------|-------------|
| `filter.period.date.from/to` | Period by visit date |

**Request:**
```json
{
    "method": "getVisit",
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

**Response:**
```json
{
    "status": true,
    "result": {
        "visit": [
            {
                "agent_id": "d0_3",
                "agent_name": "Ivan Ivanov",
                "client_id": "d0_100",
                "client_name": "Star Shop",
                "category_name": "Wholesale",
                "date": "2025-06-15",
                "start_date": "2025-06-15 10:00:00",
                "end_date": "2025-06-15 10:30:00",
                "planned": 1,
                "visited": 1,
                "reject": "",
                "has_photo_report": 1,
                "gps_visit": 1,
                "has_order": 1,
                "order_summa": 5000000.00
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 150, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `agent_id` | string | Agent ID |
| `agent_name` | string | Agent name |
| `client_id` | string | Client ID |
| `client_name` | string | Client name |
| `category_name` | string | Client category |
| `date` | string | Visit date |
| `start_date` | string | Visit start time (check-in) |
| `end_date` | string | Visit end time (check-out) |
| `planned` | int | Planned visit: `1` — yes, `0` — no |
| `visited` | int | Actual visit: `1` — yes, `0` — no |
| `reject` | string | Rejection reason (if any, comma-separated) |
| `has_photo_report` | int | Photo report present: `1` — yes, `0` — no |
| `gps_visit` | int | GPS confirmed: `1` — yes, `0` — no |
| `has_order` | int | Order present: `1` — yes, `0` — no |
| `order_summa` | float | Order total for the visit |

---

### 9.22. `getWarehouse` — Warehouses

**Description:** Returns the list of warehouses (type: regular warehouse).

**Request:**
```json
{
    "method": "getWarehouse",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

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
| `CS_id` | string | External ID (with filial prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | Warehouse code in 1C |
| `name` | string | Warehouse name |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.23. `getDefectWarehouse` — Return warehouses

**Description:** Returns the list of return warehouses (defective goods).

**Request:**
```json
{
    "method": "getDefectWarehouse",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "warehouse": [
            {
                "CS_id": "F1-d0_10",
                "SD_id": "d0_10",
                "code_1C": "WHD001",
                "name": "Return warehouse",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 1, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | Return warehouse code in 1C |
| `name` | string | Return warehouse name |
| `active` | string | Active: `Y` — active, `N` — inactive |

---

### 9.24. `getStock` — Stock

**Description:** Returns current stock levels grouped by warehouse.

**Filters:**

| Filter | Description |
|--------|-------------|
| `params.store.SD_id` | Filter by warehouse ID |
| `params.category.SD_id` | Filter by product category ID |

**Request (filter by warehouse):**
```json
{
    "method": "getStock",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "store": { "SD_id": "d0_1" }
    }
}
```

> Alternatives for `params.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **code_1C** (e.g. `"WH001"`) instead of SD_id.

**Request (filter by product category):**
```json
{
    "method": "getStock",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "category": { "SD_id": "d0_3" }
    }
}
```

> Alternatives for `params.category`: you can use **CS_id** (e.g. `"d0_3"`) or **code_1C** (e.g. `"000000003"`) instead of SD_id.

**Request (combined filters: warehouse + category):**
```json
{
    "method": "getStock",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "store": { "SD_id": "d0_1" },
        "category": { "SD_id": "d0_3" }
    }
}
```

> For `store` and `category` you can use **CS_id**, **SD_id** or **code_1C** in each object.

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
                "active": "Y",
                "products": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "name": "Coca-Cola 1.5L",
                        "code_1C": "000000015",
                        "active": "Y",
                        "quantity": 100.0
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 500, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| **warehouse** | array | Array of warehouses |
| `warehouse[].CS_id` | string | Warehouse external ID |
| `warehouse[].SD_id` | string | Warehouse server ID |
| `warehouse[].code_1C` | string | Warehouse code in 1C |
| `warehouse[].name` | string | Warehouse name |
| `warehouse[].active` | string | Active: `Y` — active, `N` — inactive |
| **warehouse[].products** | array | Array of products in warehouse |
| `products[].CS_id` | string | Product ID |
| `products[].SD_id` | string | Product server ID |
| `products[].name` | string | Product name |
| `products[].code_1C` | string | Product code in 1C |
| `products[].active` | string | Product active flag |
| `products[].quantity` | float | Product quantity in warehouse |

---

### 9.25. `getStockForDate` — Stock by date

**Description:** Returns stock levels for a given date. Calculated from the store operations log.

> **Limitations:**
> - If parameter is enabled — limit 1 request per 5 minutes
> - If not enabled — method is available **only** from 20:00 to 08:00
> - Date must be in ISO-8601 format

**Required parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `params.date` | string | Date (ISO-8601) |
| `params.store` | object | Warehouse (`CS_id` / `SD_id` / `code_1C`) |

**Request:**
```json
{
    "method": "getStockForDate",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "date": "2025-06-15",
        "store": { "code_1C": "WH001" }
    }
}
```

> Alternatives for `params.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **SD_id** (e.g. `"d0_1"`) instead of code_1C.

**Response:**
```json
{
    "status": true,
    "result": {
        "date": "2025-06-15",
        "store": { "code_1C": "WH001" },
        "stock": [
            {
                "SD_id": "d0_15",
                "code_1C": "000000015",
                "amount": 85.0
            },
            {
                "SD_id": "d0_16",
                "code_1C": "000000016",
                "amount": 42.0
            }
        ]
    }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `date` | string | Requested date |
| **store** | object | Warehouse |
| `store.code_1C` | string | Warehouse code in 1C |
| **stock** | array | Array of product stock |
| `stock[].SD_id` | string | Product server ID |
| `stock[].code_1C` | string | Product code in 1C |
| `stock[].amount` | float | Product quantity on date |

---

[← Back to contents](README.md) | [Next: Orders →](04-get-orders.md)

