# GET methods — Orders (9.26–9.28)

[← Back to contents](README.md)

---

### 9.26. `getOrder` — Orders

**Description:** Returns the list of orders with details (products, prices, quantity, bonuses).

**Filters:**

| Filter | Description |
|--------|-------------|
| `filter.agent` | `all` — all agents, otherwise only active |
| `filter.include` | `sd`, `1c`, `all` — filter by source |
| `filter.period.date.from/to` | By order date |
| `filter.period.dateLoad.from/to` | By document date |
| `filter.period.dateCreate.from/to` | By creation date (with exact time) |
| `filter.period.orderCreated.from/to` | By record creation date |
| `filter.period.dateUpdate.from/to` | By update date (from history) |
| `filter.status` | Array of statuses (default `[1]`) |
| `filter.store` | By warehouse (`CS_id` / `SD_id` / `code_1C`) |
| `filter.trade` | By trade direction (`CS_id` / `SD_id`) |
| `filter.expeditors` | Array of expeditors |
| `filter.clientCategories` | Array of client categories |
| `filter.sort.category` | `asc` / `desc` — sort by categories |
| `params.promotions` | `true` — include promotion data |
| `filter.CS_id` / `filter.SD_id` / `filter.code_1C` | Filter by specific order |
| Filter by client | `client.CS_id` / `client.SD_id` / `client.code_1C` |

**Filter examples:**

**1. Filter by agents (`filter.agent`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "agent": "all"
        }
    }
}
```
> If `agent` = `all`, orders of all agents are returned. If not specified — only active agents.

**2. Filter by source (`filter.include`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "1c"
        }
    }
}
```
> Values: `sd` — from SalesDoc only, `1c` — from 1C only, `all` — all records.

**3. Filter by order date (`filter.period.date`):**
```json
{
    "method": "getOrder",
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

**4. Filter by document date (`filter.period.dateLoad`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "dateLoad": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**5. Filter by creation date (`filter.period.dateCreate`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "dateCreate": {
                    "from": "2025-06-01 00:00:00",
                    "to": "2025-06-30 23:59:59"
                }
            }
        }
    }
}
```
> Uses exact time (format `YYYY-MM-DD HH:MM:SS`).

**6. Filter by record creation date (`filter.period.orderCreated`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "orderCreated": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**7. Filter by update date (`filter.period.dateUpdate`):**
```json
{
    "method": "getOrder",
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
> Date is taken from order change history.

**8. Filter by statuses (`filter.status`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "status": [1, 2, 3]
        }
    }
}
```
> Pass an array of statuses. Default `[1]` (new only).  
> Statuses: `1` — new, `2` — sent, `3` — delivered, `4` — closed, `5` — cancelled (see "Order statuses" table below).

**9. Filter by warehouse (`filter.store`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "code_1C": "WH001" }
        }
    }
}
```

> Alternatives for `filter.store`: you can use **CS_id** (e.g. `"F1-d0_1"`) or **SD_id** (e.g. `"d0_1"`) instead of code_1C.

**10. Filter by trade direction (`filter.trade`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "trade": { "SD_id": "1" }
        }
    }
}
```

> Alternatives for `filter.trade`: you can use **CS_id** (e.g. `"F1-1"`) or **code_1C** (e.g. `"TD001"`) instead of SD_id.

**11. Filter by expeditors (`filter.expeditors`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "expeditors": [
                { "SD_id": "d0_5" },
                { "code_1C": "000000006" }
            ]
        }
    }
}
```
> Array of expeditors; each can be specified by `CS_id`, `SD_id` or `code_1C`.

**12. Filter by client categories (`filter.clientCategories`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "clientCategories": [
                { "SD_id": "d0_2" },
                { "code_1C": "CAT001" }
            ]
        }
    }
}
```
> Array of client categories.

**13. Sort by categories (`filter.sort.category`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "sort": {
                "category": "asc"
            }
        }
    }
}
```
> Values: `asc` — ascending, `desc` — descending.

**14. Include promotion data (`params.promotions`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "promotions": true,
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
> When `promotions: true`, promotion data is included in the response.

**15. Filter by specific order (`filter.CS_id` / `filter.SD_id` / `filter.code_1C`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "SD_id": "d0_300"
        }
    }
}
```

> Alternatives for order filter: you can use **CS_id** (e.g. `"F1-d0_300"`) or **code_1C** (e.g. `"ORD000001"`) instead of SD_id.

**16. Filter by client (`client`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "client": { "code_1C": "000000100" }
    }
}
```
> You can specify client's `CS_id`, `SD_id` or `code_1C`.

**17. Combining multiple filters:**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "promotions": true,
        "client": { "code_1C": "000000100" },
        "filter": {
            "include": "all",
            "agent": "all",
            "status": [1, 2],
            "store": { "code_1C": "WH001" },
            "trade": { "SD_id": "1" },
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            },
            "sort": {
                "category": "asc"
            }
        }
    }
}
```
> All filters can be combined in a single request.

**Order statuses:**

| Status | Description |
|--------|-------------|
| 1 | New |
| 2 | Sent |
| 3 | Delivered |
| 4 | Closed |
| 5 | Cancelled |

**Request:**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "status": [1, 2, 3],
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
        "order": [
            {
                "CS_id": "F1-d0_300",
                "SD_id": "d0_300",
                "code_1C": "ORD000001",
                "status": 1,
                "dateCreate": "2025-06-15 10:00:00",
                "dateDocument": "2025-06-15 10:00:00",
                "totalSumma": 157500.00,
                "discountSumma": 7500.00,
                "totalSummaAfterDiscount": 150000.00,
                "totalReturnsSumma": 0,
                "totalReturnsCount": 0,
                "comment": "Urgent order",
                "consignment": false,
                "consigDate": null,
                "debtDateExp": "2025-07-15",
                "orderCreated": "2025-06-15 10:00:00",
                "client": {
                    "CS_id": "F1-d0_100",
                    "SD_id": "d0_100",
                    "code_1C": "000000100",
                    "nspCode": null,
                    "clientName": "Star Shop",
                    "clientLegalName": "Star LLC"
                },
                "agent": {
                    "CS_id": "F1-d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003"
                },
                "expeditor": {
                    "CS_id": "F1-d0_5",
                    "SD_id": "d0_5",
                    "code_1C": "000000005"
                },
                "priceType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "paymentType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "note": {
                    "CS_id": null,
                    "SD_id": null,
                    "code_1C": null,
                    "title": null
                },
                "store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "WH001"
                },
                "trade": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "orderProducts": [
                    {
                        "product": {
                            "CS_id": "d0_15",
                            "SD_id": "d0_15",
                            "code_1C": "000000015",
                            "name": "Coca-Cola 1.5L"
                        },
                        "quantity": 10.0,
                        "price": 15000.00,
                        "discount": 5.0,
                        "summa": 142500.00,
                        "returned": 0.0,
                        "block": 1.67
                    }
                ],
                "bonus": [],
                "invoiceNumber": null
            }
        ]
    },
    "pagination": { "limit": 50, "total": 120, "page": 1 }
}
```

**Response field descriptions:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Order external identifier (with branch prefix) |
| `SD_id` | string | Order internal identifier in SalesDoc |
| `code_1C` | string | Order code in 1C system |
| `status` | integer | Order status (1 — new, 2 — sent, 3 — delivered, 4 — closed, 5 — cancelled) |
| `dateCreate` | string | Order creation date and time (`YYYY-MM-DD HH:MM:SS`) |
| `dateDocument` | string | Order document date (`YYYY-MM-DD HH:MM:SS`) |
| `totalSumma` | float | Order total before discount |
| `discountSumma` | float | Discount amount |
| `totalSummaAfterDiscount` | float | Order total after discount |
| `totalReturnsSumma` | float | Total returns for the order |
| `totalReturnsCount` | integer | Number of returns for the order |
| `comment` | string | Order comment |
| `consignment` | boolean | Consignment flag |
| `consigDate` | string\|null | Consignment date (if applicable) |
| `debtDateExp` | string\|null | Expected debt repayment date |
| `orderCreated` | string | Actual record creation date and time |
| `invoiceNumber` | string\|null | Invoice number |
| **client** | object | Client object |
| `client.CS_id` | string | Client external identifier |
| `client.SD_id` | string | Client internal identifier |
| `client.code_1C` | string | Client 1C code |
| `client.nspCode` | string\|null | Client NSP code |
| `client.clientName` | string | Client name |
| `client.clientLegalName` | string | Client legal name |
| **agent** | object | Agent object |
| `agent.CS_id` | string | Agent external identifier |
| `agent.SD_id` | string | Agent internal identifier |
| `agent.code_1C` | string | Agent 1C code |
| **expeditor** | object | Expeditor object |
| `expeditor.CS_id` | string | Expeditor external identifier |
| `expeditor.SD_id` | string | Expeditor internal identifier |
| `expeditor.code_1C` | string | Expeditor 1C code |
| **priceType** | object | Price type object |
| `priceType.CS_id` | string | Price type external identifier |
| `priceType.SD_id` | string | Price type internal identifier |
| `priceType.code_1C` | string | Price type 1C code |
| **paymentType** | object | Payment type object |
| `paymentType.CS_id` | string | Payment type external identifier |
| `paymentType.SD_id` | string | Payment type internal identifier |
| `paymentType.code_1C` | string | Payment type 1C code |
| **note** | object | Note object |
| `note.CS_id` | string\|null | Note external identifier |
| `note.SD_id` | string\|null | Note internal identifier |
| `note.code_1C` | string\|null | Note 1C code |
| `note.title` | string\|null | Note text |
| **store** | object | Warehouse object |
| `store.CS_id` | string | Warehouse external identifier |
| `store.SD_id` | string | Warehouse internal identifier |
| `store.code_1C` | string | Warehouse 1C code |
| **trade** | object | Trade direction object |
| `trade.CS_id` | string | Trade direction external identifier |
| `trade.SD_id` | string | Trade direction internal identifier |
| `trade.code_1C` | string\|null | Trade direction 1C code |
| **orderProducts** | array | Order products array |
| `orderProducts[].product.CS_id` | string | Product external identifier |
| `orderProducts[].product.SD_id` | string | Product internal identifier |
| `orderProducts[].product.code_1C` | string | Product 1C code |
| `orderProducts[].product.name` | string | Product name |
| `orderProducts[].quantity` | float | Product quantity |
| `orderProducts[].price` | float | Price per unit |
| `orderProducts[].discount` | float | Discount percent |
| `orderProducts[].summa` | float | Line total (after discount) |
| `orderProducts[].returned` | float | Returned quantity |
| `orderProducts[].block` | float | Quantity in blocks |
| **bonus** | array | Bonus products array (structure same as `orderProducts`) |

---

### 9.27. `getOrderDefect` — Returns

**Description:** Returns the list of return orders (type 2).

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | By return date |
| `filter.period.dateLoad.from/to` | By document date |
| `filter.period.dateUpdate.from/to` | By update date (from history) |
| `filter.status` | Array of statuses (default `[1]`) |

**Filter examples:**

**Filter by return date and statuses:**
```json
{
    "method": "getOrderDefect",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "status": [1, 2, 3],
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

**Filter by document date (`filter.period.dateLoad`):**
```json
{
    "method": "getOrderDefect",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "dateLoad": {
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
    "method": "getOrderDefect",
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
> Date is taken from document change history.

**Combining multiple filters:**
```json
{
    "method": "getOrderDefect",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "status": [1, 2],
            "period": {
                "date": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                },
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
        "order": [
            {
                "CS_id": "F1-d0_400",
                "SD_id": "d0_400",
                "code_1C": "DEF000001",
                "type": 2,
                "status": 1,
                "date": "2025-06-15 10:00:00",
                "dateLoad": "2025-06-15",
                "totalSumma": 75000.00,
                "discount": 0.00,
                "summa": 75000.00,
                "comment": "Defect",
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
                "expeditor": {
                    "CS_id": "F1-d0_5",
                    "SD_id": "d0_5",
                    "code_1C": "000000005"
                },
                "priceType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "store": {
                    "CS_id": "F1-d0_10",
                    "SD_id": "d0_10",
                    "code_1C": "WHD001"
                },
                "reason": "Expired",
                "defectProducts": [
                    {
                        "product": {
                            "CS_id": "d0_15",
                            "SD_id": "d0_15",
                            "code_1C": "000000015"
                        },
                        "quantity": 5.0,
                        "price": 15000.00,
                        "summa": 75000.00,
                        "exp_date": "2025-06-01"
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 10, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | External identifier (with branch prefix) |
| `SD_id` | string | Server ID |
| `code_1C` | string | 1C code |
| `type` | integer | Document type (2 = return) |
| `status` | integer | Document status (1–5) |
| `date` | string | Return date and time |
| `dateLoad` | string | Document date |
| `totalSumma` | float | Total amount before discount |
| `discount` | float | Discount amount |
| `summa` | float | Final amount |
| `comment` | string | Comment |
| `reason` | string | Return reason |
| **client** | object | Client |
| `client.CS_id` | string | Client identifier |
| `client.SD_id` | string | Client server ID |
| `client.code_1C` | string | Client 1C code |
| **agent** | object | Agent |
| `agent.CS_id` | string | Agent identifier |
| `agent.SD_id` | string | Agent server ID |
| `agent.code_1C` | string | Agent 1C code |
| **expeditor** | object | Expeditor |
| `expeditor.CS_id` | string | Expeditor identifier |
| `expeditor.SD_id` | string | Expeditor server ID |
| `expeditor.code_1C` | string | Expeditor 1C code |
| **priceType** | object | Price type |
| `priceType.CS_id` | string | Price type identifier |
| `priceType.SD_id` | string | Price type server ID |
| `priceType.code_1C` | string | Price type 1C code |
| **store** | object | Return warehouse |
| `store.CS_id` | string | Warehouse identifier |
| `store.SD_id` | string | Warehouse server ID |
| `store.code_1C` | string | Warehouse 1C code |
| **defectProducts** | array | Returned products array |
| `defectProducts[].product` | object | Product (`CS_id`, `SD_id`, `code_1C`) |
| `defectProducts[].quantity` | float | Quantity |
| `defectProducts[].price` | float | Price per unit |
| `defectProducts[].summa` | float | Line amount |
| `defectProducts[].exp_date` | string | Expiry date |

---

### 9.28. `getOrderReplace` — Exchanges

**Description:** Returns the list of exchange orders (type 3). Structure is the same as `getOrderDefect`.

**Filters:** Same as `getOrderDefect` (period, status).

**Request:**
```json
{
    "method": "getOrderReplace",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "status": [1, 2, 3],
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

**Response fields:**

> Response structure is the same as `getOrderDefect` (see above), except `type` = `3` (exchange).

---

[← Back to contents](README.md) | [Next: Finance →](05-get-finance.md)

