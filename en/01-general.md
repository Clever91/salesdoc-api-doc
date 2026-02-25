# SalesDoc API V2 — Basics

[← Back to contents](README.md)

---

## 1. API overview

### What is SalesDoc API V2?

SalesDoc API V2 is a programmatic interface for integrating external systems (1C, ERP, mobile apps) with the SalesDoc platform. The API allows you to:

- **Sync reference data** (products, clients, agents, prices, etc.)
- **Manage stock** (receipts, transfers, inventory)
- **Work with orders** (create, update statuses, returns)
- **Manage finances** (payments, balances, cash operations)
- **Exchange product photos**

### Core principles

| Principle | Description |
|-----------|-------------|
| **Single endpoint** | All requests go to one URL `/api/v2` |
| **Method in body** | Operation type is determined by the `method` field in the JSON body |
| **POST only** | All requests use HTTP POST |
| **JSON format** | Request and response bodies are always JSON |
| **Token authentication** | All requests (except `login`) require `token` and `userId` |
| **Filial support** | The API supports multiple branches (filials) |

### Basic concepts

- **CS_id** — internal identifier in SalesDoc (may include filial prefix)
- **SD_id** — server-side identifier in SalesDoc (no prefix)
- **code_1C** — external identifier in the 1C system (XML_ID)
- **Filial** — company subdivision with its own database

---

## 2. Authentication

The API uses token-based authentication. You must obtain a token via the `login` method before making other requests.

### Method: `login`

**Description:** User authorization and access token retrieval.

**Request:**
```json
{
    "method": "login",
    "auth": {
        "login": "sd_user",
        "password": "your_password"
    }
}
```

**Success response:**
```json
{
    "status": true,
    "result": {
        "userId": "d0_1",
        "token": "a1b2c3d4e5f6g7h8i9j0..."
    }
}
```

**Errors:**
| Code | Message | Description |
|------|---------|-------------|
| 401 | User not found | User not found |
| 401 | Invalid login/password | Invalid login or password |

### Using the token

After receiving the token, all subsequent requests must include the `auth` block:

```json
{
    "method": "getProduct",
    "auth": {
        "userId": "d0_1",
        "token": "a1b2c3d4e5f6g7h8i9j0..."
    },
    "params": { ... }
}
```

> **Important:** The token is unique per device. On a new login, the previous token is invalidated.

---

## 3. Request structure

All API requests share the same structure:

```json
{
    "method": "method_name",
    "auth": {
        "userId": "d0_1",
        "token": "your_token"
    },
    "filial": {
        "filial_id": "filial_code"
    },
    "params": {
        "limit": 100,
        "page": 1,
        "period": {
            "from": "2025-01-01 00:00:00",
            "to": "2025-12-31 23:59:59",
            "by": "update"
        },
        "filter": { }
    },
    "data": { }
}
```

### Field description

| Field | Type | Required | Description |
|-------|------|:--------:|-------------|
| `method` | string | Yes | Name of the method to call |
| `auth` | object | Yes | Authentication data |
| `auth.userId` | string | Yes | User ID (from `login`) |
| `auth.token` | string | Yes | Authorization token (from `login`) |
| `filial` | object | No | Filial data (see note below) |
| `filial.filial_id` | string | No | Filial code (XML_ID) |
| `params` | object | No | Request parameters (for GET methods) |
| `params.limit` | int | No | Page size (default 1000) |
| `params.page` | int | No | Page number (default 1) |
| `params.period` | object | No | Date range filter |
| `params.sort` | object | No | Sort (`by`: create/update, `order`: ASC/DESC) |
| `params.filter` | object | No | Additional filters |
| `data` | object/array | No | Data for SET methods |

> **Filial note:** If your server **does not use a filial structure**, you can:
> - Omit the `filial` object entirely
> - Or set `filial_id` to `"0"`:
> ```json
> "filial": { "filial_id": "0" }
> ```
> In both cases the system will operate without filial binding.
>
> **Methods without filial:** the following methods do not use filial (you may omit `filial` or pass empty/zero): `setPrice`, `setUnit`, `getUnit`, `setValyutaType`, `getValyutaType`, `setProduct`, `getProduct`, `setProductCategory`, `getProductCategory`, `setClientCategory`, `getClientCategory`.

---

## 4. Response structure

### Success (GET methods — with pagination):
```json
{
    "status": true,
    "result": {
        "data_key": [ ... ]
    },
    "pagination": {
        "limit": 1000,
        "total": 5432,
        "page": 1
    }
}
```

### Success (SET methods — no pagination):
```json
{
    "status": true,
    "result": {
        "completed": 10,
        "error": 0,
        "data": {
            "data_key": [
                {
                    "CS_id": "d0_123",
                    "SD_id": "d0_123",
                    "code_1C": "CODE_1C"
                }
            ]
        }
    }
}
```

### Error response:
```json
{
    "status": false,
    "result": {
        "completed": 5,
        "error": 2,
        "data": { ... }
    },
    "error": {
        "code": 400,
        "message": "Error",
        "data": {
            "group_key": [
                {
                    "CS_id": "...",
                    "SD_id": "...",
                    "code_1C": "...",
                    "name": "...",
                    "index": 0,
                    "errors": ["Error description"]
                }
            ]
        }
    }
}
```

### Pre-parse errors

If the request body is empty, not valid JSON, or missing the `method` field, the server responds **before** checking authentication:

```json
{
    "status": false,
    "error": {
        "code": 400,
        "message": "Error text"
    }
}
```

Possible messages:

| Message | Cause |
|---------|-------|
| Empty request body | No body sent |
| Invalid request format. Request must be JSON | Body is not valid JSON |
| Required field "method" is missing | JSON has no `method` field |

---

## 5. Pagination

Pagination is supported only in GET methods. Parameters:

| Parameter | Type | Default | Description |
|-----------|------|:-------:|-------------|
| `limit` | int | 1000 | Maximum records per page |
| `page` | int | 1 | Current page number |

**Example:**
```json
{
    "method": "getProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 3
    }
}
```

**Offset formula:** `offset = (page - 1) * limit`

### Request and response limits

| Type | Default | Description |
|------|:-------:|-------------|
| Response limit (GET) | 1000 | Max records per page. Configurable via `response_limit_1c` on server |
| Request limit (SET) | 250 | Max records in one SET request (per group in `data`). Returns 407 if exceeded. Configurable via `request_limit_1c` on server |

---

## 6. Filtering and period

### Period

Filter by record creation or update date:

```json
"params": {
    "period": {
        "from": "2025-01-01 00:00:00",
        "to": "2025-06-30 23:59:59",
        "by": "update"
    }
}
```

| Field | Description |
|-------|-------------|
| `from` | Start date |
| `to` | End date |
| `by` | `create` — by creation date, `update` — by update date |

### Sort

```json
"params": {
    "sort": {
        "by": "create",
        "order": "ASC"
    }
}
```

| Field | Description |
|-------|-------------|
| `by` | `create` — by creation date, `update` — by update date |
| `order` | `ASC` — ascending, `DESC` — descending |

### Source filter (`include`)

Some GET methods support filtering by record source (SalesDoc-created, 1C-imported, or all).

```json
"params": {
    "filter": {
        "include": "all"
    }
}
```

| Value | Description |
|-------|-------------|
| `sd` | Only records created in SalesDoc |
| `1c` | Only records imported from 1C (with `code_1C`) |
| `all` | All records regardless of source |

> **Note:** The `include` filter works **only** in: `getClientCategory`, `getMovement`, `getVsExchange`, `getOrder`, `getConsumption`. It is ignored in other methods.

---

## 7. Object identifiers

Each object can be identified in three ways:

| ID | Description | Example |
|----|-------------|---------|
| `CS_id` | Client-side ID (may include filial prefix) | `F1-d0_123` |
| `SD_id` | Server-side ID | `d0_123` |
| `code_1C` | External 1C code | `000000001` |

When sending data (SET methods), **one** of these identifiers is enough to find an existing record. Search order:

1. `CS_id` → search by internal ID
2. `SD_id` → search by internal ID
3. `code_1C` → search by XML_ID

If none is found, a new record is created.

---

## 8. Error codes

| Code | Description |
|------|-------------|
| 400 | Request data error (invalid data, missing required fields) |
| 401 | Authentication error (invalid token, user not found or inactive) |
| 402 | Method not found (invalid method name) |
| 403 | Method access denied (server-configured list of disabled methods) |
| 407 | Request record limit exceeded. Message may include count and limit (e.g. `Limit error::> request items: 300 limit: 250`) |
| 429 | Rate limit exceeded. Wait; the server will lift the restriction after a period |

---

## 9. Rate limits and access time windows

Some methods have extra limits on request frequency or time of day:

| Method | Limit | Note |
|--------|-------|------|
| `setStock` | 5 requests per 5 seconds | — |
| `setCurrentBalance` | 3 requests per 5 minutes | — |
| `getStockForDate` | 1 request per 5 minutes | — |
| `getStoreLog` | 5 requests per 5 seconds; available **20:00–07:00** server time only | Returns error at other times |

---

[← Back to contents](README.md) | [Next: GET methods — References →](02-get-references.md)
