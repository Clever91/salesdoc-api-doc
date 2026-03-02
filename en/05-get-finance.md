# GET methods — Finance (9.29–9.31)

[← Back to contents](README.md)

---

### 9.29. `getBalance` — Client balances

**Description:** Returns current client balances with breakdown by currency.

**Request:**
```json
{
    "method": "getBalance",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "balance": [
            {
                "CS_id": "F1-d0_100",
                "SD_id": "d0_100",
                "code_1C": "000000100",
                "name": "Star Shop",
                "active": true,
                "balance": -2500000.00,
                "by-currency": [
                    {
                        "currency_id": "d0_1",
                        "amount": -2500000.00
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
| `CS_id` | string | Client external identifier (with branch prefix) |
| `SD_id` | string | Client server ID |
| `code_1C` | string | Client 1C code |
| `name` | string | Client name |
| `active` | boolean | Client active flag |
| `balance` | float | Total client balance (negative = debt) |
| **by-currency** | array | Balance breakdown by currency |
| `by-currency[].currency_id` | string | Currency (payment type) ID |
| `by-currency[].amount` | float | Balance amount in that currency |

> **Note:** Negative balance means client debt.

---

### 9.30. `getConsumption` — Expense / Income

**Description:** Returns the list of cash operations (expense and income).

**Filters:**

| Filter | Description |
|--------|-------------|
| `filter.include` | `sd`, `1c`, `all` — by source |
| `filter.type` | `income` — income, `expense` — expense |
| `filter.paymentType` | By payment type (`CS_id` / `SD_id` / `code_1C`) |

**Filter examples:**

**Filter by operation type and source:**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "type": "expense",
            "include": "all"
        }
    }
}
```

**Filter by source (`filter.include`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "1c"
        }
    }
}
```
> Values: `sd` — from SalesDoc only, `1c` — from 1C only, `all` — all records.

**Filter by income only (`filter.type`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "type": "income"
        }
    }
}
```

**Filter by payment type (`filter.paymentType`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "paymentType": {
                "code_1C": "000000001"
            }
        }
    }
}
```

> Alternatives for `filter.paymentType`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **SD_id** (e.g. `"d0_1"`) instead of code_1C.

**Combining all filters:**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "type": "expense",
            "include": "sd",
            "paymentType": {
                "code_1C": "000000001"
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
        "consumption": [
            {
                "CS_id": "F1-d0_50",
                "SD_id": "d0_50",
                "code_1C": "CON000001",
                "summa": 500000.00,
                "date": "2025-06-15",
                "created_at": "2025-06-15 09:00:00",
                "type": "expense",
                "comment": "Stationery",
                "cashbox": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Main cashbox",
                    "code_1C": null
                },
                "paymentType": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "category_parent": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Administrative expenses",
                    "code_1C": "ADM001"
                },
                "category_child": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Office supplies",
                    "code_1C": "KAN001"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 30, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `summa` | float | Operation amount |
| `date` | string | Operation date |
| `created_at` | string | Record creation date and time |
| `type` | string | Operation type: `expense` — expense, `income` — income |
| `comment` | string | Comment |
| **cashbox** | object | Cashbox |
| `cashbox.CS_id` | string | Cashbox identifier |
| `cashbox.SD_id` | string | Cashbox server ID |
| `cashbox.name` | string | Cashbox name |
| `cashbox.code_1C` | string\|null | Cashbox 1C code |
| **paymentType** | object | Payment type |
| `paymentType.CS_id` | string | Payment type identifier |
| `paymentType.SD_id` | string | Payment type server ID |
| `paymentType.code_1C` | string | Payment type 1C code |
| **category_parent** | object | Parent expense category |
| `category_parent.CS_id` | string | Identifier |
| `category_parent.SD_id` | string | Server ID |
| `category_parent.name` | string | Name |
| `category_parent.code_1C` | string | 1C code |
| **category_child** | object | Child expense category |
| `category_child.CS_id` | string | Identifier |
| `category_child.SD_id` | string | Server ID |
| `category_child.name` | string | Name |
| `category_child.code_1C` | string | 1C code |

---

### 9.31. `getCashbox` — Cashbox

**Description:** Returns the list of cashboxes.

**Request:**
```json
{
    "method": "getCashbox",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "cashbox": [
            {
                "CS_id": "F1-1",
                "SD_id": "1",
                "code_1C": null,
                "name": "Main cashbox",
                "active": true,
                "created_at": "2024-01-01 00:00:00"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 2, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string\|null | Cashbox 1C code |
| `name` | string | Cashbox name |
| `active` | boolean | Cashbox active |
| `created_at` | string | Creation date and time |

---

[← Back to contents](README.md) | [Next: Extra GET methods →](06-get-extra.md)

