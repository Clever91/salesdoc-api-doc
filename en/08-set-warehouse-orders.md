# SET methods — Warehouse and Orders (11–12)

[← Back to contents](README.md)

---

## 11. Warehouse

### 11.1. `setWarehouse` — Create/update warehouses

**Request:**
```json
{
    "method": "setWarehouse",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "warehouse": [
            {
                "code_1C": "WH001",
                "name": "Main warehouse",
                "active": true
            }
        ]
    }
}
```

---

### 11.2. `setStock` — Set warehouse stock (inventory)

**Description:** Full warehouse inventory. Updates stock for all products sent in the request. By default zeroes stock for products that were **not** included in the request.

> ⚠️ **Rate limit:** Maximum 5 requests per 5 seconds.

**Request:**
```json
{
    "method": "setStock",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "warehouse": [
            {
                "code_1C": "WH001",
                "date": "2025-06-15 12:00:00",
                "dont_zero_others": false,
                "products": [
                    {
                        "code_1C": "000000015",
                        "quantity": 100,
                        "price": 15000.00
                    },
                    {
                        "code_1C": "000000016",
                        "quantity": 50,
                        "price": 12000.00
                    }
                ]
            }
        ]
    }
}
```

**Warehouse parameters:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Warehouse ID |
| `date` | string | ❌ | Inventory date (default: current) |
| `dont_zero_others` | bool | ❌ | `true` — do not zero stock for products not in the request |
| `products` | array | ✅ | List of products with quantity |

**Product parameters:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Product ID |
| `quantity` | float | ✅ | Quantity (≥ 0) |
| `price` | float | ❌ | Cost price |

---

### 11.3. `setPurchase` — Goods receipt

**Description:** Create or update a warehouse receipt (purchase) document. To find an existing document use CS_id / SD_id / code_1C (see identification order in 07-set-references).

**Request:**
```json
{
    "method": "setPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "purchase": [
            {
                "date": "2025-06-15 10:00:00",
                "comment": "Purchase from supplier",
                "priceType": { "code_1C": "000000001" },
                "store": { "code_1C": "WH001" },
                "shipper": { "code_1C": "SHIP001" },
                "products": [
                    {
                        "code_1C": "000000015",
                        "quantity": 100,
                        "price": 12000.00
                    },
                    {
                        "code_1C": "000000016",
                        "quantity": 50,
                        "price": 11500.00,
                        "mfg_date": "2025-01-10",
                        "exp_date": "2026-01-10"
                    }
                ]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (one of for update) | Receipt identifier (if empty — new is created) |
| `date` | string | ✅ | Receipt date (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Comment |
| `priceType` | object | ✅ | Price type (purchase, TYPE=1). CS_id / SD_id / code_1C |
| `store` | object | ✅ | Warehouse. CS_id / SD_id / code_1C |
| `shipper` | object | ❌ | Supplier. CS_id / SD_id / code_1C (if not set — default supplier) |
| `products` | array | ✅ | List of products with quantity and price |

**Product fields in products:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Product identifier |
| `quantity` | float | ✅ | Quantity (≥ 0) |
| `price` | float | ✅ | Price (≥ 0) |
| `mfg_date` | string | ❌ | Manufacturing date (Y-m-d) |
| `exp_date` | string | ❌ | Expiry date (Y-m-d) |

---

### 11.4. `setMovement` — Transfer

**Description:** Create a transfer document between two warehouses. Existing transfers cannot be edited. A unique `code_1C` for the document is required.

**Request:**
```json
{
    "method": "setMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "movement": [
            {
                "code_1C": "MOV000001",
                "date": "2025-06-15 07:00:00",
                "comment": "Transfer to agent warehouse",
                "storeFrom": { "code_1C": "WH001" },
                "storeTo": { "code_1C": "WH002" },
                "products": [
                    { "code_1C": "000000015", "quantity": 50 },
                    { "code_1C": "000000016", "quantity": 30 }
                ]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` | string | ✅ | Unique document code (from 1C). Must not match an existing transfer |
| `date` | string | ✅ | Transfer date (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Comment |
| `storeFrom` | object | ✅ | Source warehouse. CS_id / SD_id / code_1C |
| `storeTo` | object | ✅ | Destination warehouse. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Products and quantity (availability checked on source warehouse) |

**Product fields in products:** `CS_id` / `SD_id` / `code_1C` — product identifier; `quantity` — quantity (float).

---

### 11.5. `setVsExchange` — Exchange (Vansel)

**Description:** Create or update a warehouse exchange document (Vansel scheme). Agent must be VanSelling type; agent warehouse is used.

**Request:**
```json
{
    "method": "setVsExchange",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "vsExchange": [
            {
                "date": "2025-06-15 08:00:00",
                "comment": "Product exchange",
                "priceType": { "code_1C": "000000001" },
                "store": { "code_1C": "WH001" },
                "agent": { "code_1C": "000000003" },
                "products": [
                    { "code_1C": "000000015", "quantity": 20 },
                    { "code_1C": "000000016", "quantity": 10 }
                ]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (one of for update) | Exchange document identifier |
| `date` | string | ✅ | Date (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Comment |
| `priceType` | object | ✅ | Price type (TYPE=2). CS_id / SD_id / code_1C |
| `store` | object | ✅ | Source warehouse. CS_id / SD_id / code_1C |
| `agent` | object | ✅ | Vansel agent. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Products and quantity |

**Product fields in products:** product identifier (CS_id / SD_id / code_1C), `quantity` (float, > 0). Price may be taken from price list.

---

### 11.6. `setMovingToFilial` — Move to filial

**Description:** Transfer products from the current branch warehouse to another branch warehouse. The request must include `storeFrom` and `movingTo` (destination branch and warehouse) with **CS_id** (branch prefix). Creates a supplier return in the current branch and a receipt in the target branch.

**Request:**
```json
{
    "method": "setMovingToFilial",
    "auth": { "userId": "d0_1", "token": "..." },
    "filial": { "filial_id": "F1" },
    "data": {
        "moving": [
            {
                "date": "2025-06-15 12:00:00",
                "comment": "Transfer to branch F2",
                "priceType": { "code_1C": "000000001" },
                "storeFrom": { "CS_id": "F1-d0_1" },
                "movingTo": {
                    "filialId": "F2",
                    "storeTo": { "CS_id": "F2-d0_1" }
                },
                "products": [
                    { "code_1C": "000000015", "quantity": 100, "price": 12000.00 },
                    { "code_1C": "000000016", "quantity": 50, "price": 11500.00 }
                ]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `date` | string | ✅ | Transfer date |
| `comment` | string | ❌ | Comment |
| `priceType` | object | ✅ | Price type (purchase). CS_id / SD_id / code_1C |
| `storeFrom` | object | ✅ | Current branch warehouse; **CS_id required** (with branch prefix) |
| `movingTo` | object | ✅ | Destination branch and warehouse |
| `movingTo.filialId` | string | ✅ | Branch code (xml_id) |
| `movingTo.storeTo` | object | ✅ | Destination branch warehouse; **CS_id required** |
| `products` | array | ✅ | Products: identifier, `quantity`, `price` |

---

### 11.7. `setSupplierReturn` — Supplier return

**Description:** Create or update a product return-to-supplier document. Product availability on warehouse is checked.

**Request:**
```json
{
    "method": "setSupplierReturn",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "return": [
            {
                "date": "2025-06-15 14:00:00",
                "comment": "Defect return",
                "priceType": { "code_1C": "000000001" },
                "store": { "code_1C": "WH001" },
                "products": [
                    { "code_1C": "000000015", "quantity": 5, "price": 12000.00 },
                    { "code_1C": "000000016", "quantity": 3, "price": 11500.00 }
                ]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (one of for update) | Return document identifier |
| `date` | string | ✅ | Date (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Comment |
| `priceType` | object | ✅ | Price type. CS_id / SD_id / code_1C |
| `store` | object | ✅ | Warehouse. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Products: identifier (CS_id / SD_id / code_1C), `quantity` (> 0), `price` |

---

### 11.8. `setExcretion` — Write-off

**Description:** Create or update a warehouse write-off document. Decreases warehouse stock. Product availability is checked.

**Request:**
```json
{
    "method": "setExcretion",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "excretion": [
            {
                "date": "2025-06-15 16:00:00",
                "comment": "Write-off by act",
                "fromStore": { "code_1C": "WH001" },
                "products": [
                    { "code_1C": "000000015", "quantity": 10 },
                    { "code_1C": "000000016", "quantity": 5 }
                ]
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (one of for update) | Write-off document identifier |
| `date` | string | ✅ | Write-off date (format `Y-m-d H:i:s` or ISO-8601) |
| `comment` | string | ❌ | Comment |
| `fromStore` | object | ✅ | Warehouse. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Products and quantity (positive) |

**Product fields in products:** product identifier (CS_id / SD_id / code_1C), `quantity` (float, > 0).

---

### 11.9. `setCorrection` — Stock correction

**Description:** Manual warehouse stock correction. **Only creation** of new documents is supported; editing existing ones (by SD_id or existing code_1C) is not allowed. Data is passed as an array directly in `data`. Warehouse must be linked to the created warehouse. `quantity` in a line can be positive (increase) or negative (decrease); final stock must not become negative.

**Request:**
```json
{
    "method": "setCorrection",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": [
        {
            "code_1C": "COR000001",
            "date": "2025-06-15 18:00:00",
            "warehouse": { "code_1C": "WH001" },
            "products": [
                { "code_1C": "000000015", "quantity": 5.0 },
                { "code_1C": "000000016", "quantity": -2.0 }
            ]
        }
    ]
}
```

**Element fields (each item in `data` array):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` | string | ❌ | Document code in 1C (new only; if set and already exists — error) |
| `SD_id` | — | ❌ | Do not send; editing by SD_id is not allowed |
| `date` | string | ❌ | Correction date (default: current) |
| `warehouse` | object | ✅ | Warehouse. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Products and stock delta (+ increase, − decrease) |

**Product fields in products:** product identifier (CS_id / SD_id / code_1C), `quantity` (float; may be negative). New stock must be ≥ 0.

---

## 12. Orders

### 12.1. `setOrder` — Create/update order

**Description:** Create or update an order. Includes stock check, reservation, bonus products.

**Request:**
```json
{
    "method": "setOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "order": [
            {
                "code_1C": "ORD000001",
                "status": 1,
                "dateCreate": "2025-06-15 10:00:00",
                "dateShipment": "2025-06-16",
                "comment": "Urgent order",
                "consignment": false,
                "consigDate": null,
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "expeditor": { "code_1C": "000000005" },
                "priceType": { "code_1C": "000000001" },
                "warehouse": { "code_1C": "WH001" },
                "auto_attach_expeditor": false,
                "debtDateExp": null,
                "orderProducts": [
                    {
                        "product": { "code_1C": "000000015" },
                        "quantity": 10,
                        "price": 15000.00,
                        "discount": 5,
                        "returned": 0
                    }
                ],
                "bonusProducts": []
            }
        ]
    }
}
```

**Order fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Order identifier |
| `status` | int | ✅ | Status (1-5) |
| `dateCreate` | string | ❌ | Creation date |
| `dateShipment` | string | ❌ | Shipment date |
| `comment` | string | ❌ | Comment |
| `consignment` | bool | ❌ | Consignment |
| `consigDate` | string | ❌ | Consignment date |
| `client` | object | ✅ | Client |
| `agent` | object | ✅ | Agent |
| `expeditor` | object | ❌ | Expeditor |
| `priceType` | object | ✅ | Price type |
| `warehouse` | object | ✅ | Warehouse |
| `auto_attach_expeditor` | bool | ❌ | Auto-attach expeditor |
| `debtDateExp` | string | ❌ | Debt expiry date |
| `orderProducts` | array | ✅ | Order products |
| `bonusProducts` | array | ❌ | Bonus products |

**Order product fields:**

| Field | Type | Description |
|------|-----|----------|
| `product` | object | Product (`code_1C` / `SD_id`) |
| `quantity` | float | Quantity |
| `price` | float | Price (if not set — from price list) |
| `discount` | float | Discount percent (0-100) |
| `returned` | float | Return quantity |

**Order statuses:**

| Status | Description |
|--------|-------------|
| 1 | New |
| 2 | Sent |
| 3 | Delivered |
| 4 | Closed |
| 5 | Cancelled |

---

### 12.2. `setDeletedOrder` — Cancel order

**Description:** Cancel order (set status 5 — Cancelled).

**Request:**
```json
{
    "method": "setDeletedOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "deletedOrder": [
            {
                "code_1C": "ORD000001"
            }
        ]
    }
}
```

---

### 12.3. `setStatus` — Change order status

**Description:** Change status of one or more orders.

**Request:**
```json
{
    "method": "setStatus",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "order": [
            {
                "code_1C": "ORD000001",
                "status": 3,
                "dateShipment": "2025-06-16"
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Order identifier |
| `status` | int | ✅ | New status |
| `dateShipment` | string | ❌ | Shipment date (format: Y-m-d) |

---

### 12.4. `setCode` — Set 1C code for documents

**Description:** Set XML_ID (1C code) for various documents (orders, returns, exchanges, etc.).

**Request:**
```json
{
    "method": "setCode",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "document": "Order",
        "documents": [
            {
                "SD_id": "d0_300",
                "code_1C": "ORD000001"
            }
        ]
    }
}
```

**Supported document types:**
- `Order` — Order
- `OrderDefect` — Client return
- `OrderReplace` — Exchange
- `Purchase` — Receipt (purchase)
- `PurchaseRefund` — Supplier return
- `Exchange` — Transfer between warehouses
- `VsExchange` — Exchange (Vansel)
- `Excretion` — Write-off
- Any other existing model with code_1C field

---

### 12.5. `setOrderDefect` — Create return

**Description:** Create a client return document (in the web UI called **"Shelf return"**). The document records return of goods from client to warehouse: client, agent, warehouse, price type and list of returned products with quantity and price.

**Request:**
```json
{
    "method": "setOrderDefect",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "orderDefect": [
            {
                "code_1C": "DEF000001",
                "status": 1,
                "dateCreate": "2025-06-15 10:00:00",
                "dateDefect": "2025-06-15",
                "comment": "Defect",
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "expeditor": { "code_1C": "000000005" },
                "priceType": { "code_1C": "000000001" },
                "warehouse": { "code_1C": "WH001" },
                "defectProducts": [
                    {
                        "product": { "code_1C": "000000015" },
                        "quantity": 5,
                        "price": 15000.00
                    }
                ]
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ (one of) | Return document identifier |
| `status` | int | ✅ | Status (1–5) |
| `dateCreate` | string | ❌ | Creation date (`Y-m-d H:i:s`) |
| `dateDefect` | string | ❌ | Return date (Y-m-d) |
| `comment` | string | ❌ | Comment (return reason) |
| `client` | object | ✅ | Client. CS_id / SD_id / code_1C |
| `agent` | object | ✅ | Agent. CS_id / SD_id / code_1C |
| `expeditor` | object | ❌ | Expeditor. CS_id / SD_id / code_1C |
| `priceType` | object | ✅ | Price type. CS_id / SD_id / code_1C |
| `warehouse` | object | ✅ | Warehouse. CS_id / SD_id / code_1C |
| `defectProducts` | array | ✅ | List of returned products |

**Product fields in defectProducts:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `product` | object | ✅ | Product. CS_id / SD_id / code_1C |
| `quantity` | float | ✅ | Return quantity |
| `price` | float | ❌ | Price (if not set — from price list) |

---

### 12.6. `syncOrder` — Order sync

**Description:** Assign 1C code and optionally change status for orders, returns and exchanges. Used after the document is processed in 1C: the integrator sends the document's `SD_id` from SalesDoc and the code assigned in 1C (`code_order` for order, `code_return` for return, `code_replace` for exchange). Optionally pass new `status` (1–5).

**Typical integrator scenario:**

1. **Get unsynchronized documents** — in `getOrder`, `getOrderDefect`, `getOrderReplace` use filter **`filter.include`**:
   - `"include": "sd"` — only documents created in SalesDoc (usually without 1C code yet). Use to select only orders/returns/exchanges to send to 1C and then "confirm" via `syncOrder`.
   - `"include": "1c"` — only documents that already have a 1C code (synchronized).
   - `"include": "all"` — all documents, no source filter.
2. Send selected documents to 1C, get assigned codes from 1C.
3. Call **`syncOrder`**, passing for each document `SD_id` and the corresponding codes (`code_order`, `code_return`, `code_replace`) and optionally `status`.
4. After a successful call the document is considered synchronized; with subsequent requests using `include: "1c"` it will be included in the result.

**Request:**
```json
{
    "method": "syncOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": [
        {
            "SD_id": "d0_300",
            "code_order": "ORD000001",
            "status": 3
        },
        {
            "SD_id": "d0_301",
            "code_return": "RET000001",
            "code_replace": "REP000001",
            "status": 3
        }
    ]
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `SD_id` | string | ✅ | Order ID in SalesDoc |
| `code_order` | string | ❌* | Order code in 1C |
| `code_return` | string | ❌* | Return code in 1C |
| `code_replace` | string | ❌ | Exchange code in 1C |
| `status` | int | ❌ | New status (1–5). If 0 or not sent — status is not changed |

> *At least one of `code_order`, `code_return`, `code_replace` must be specified (for exchange — both `code_return` and `code_replace`).

**Response:** Unlike other SET methods, returns an object with arrays `completed` (successfully processed items from `data`) and `failed` (items with error and `error` field). No pagination.

---

[← Back to contents](README.md) | [Next: Finance, Photo, Extra →](09-finance-photo-extra.md)

