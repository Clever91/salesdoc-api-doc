# 📘 Contragents (17.1–17.2)

[← Back to contents](README.md)

---

## 17. Contragents

### Two server types

Each SalesDoc installation runs in one of two modes. **The server type is detected by the system automatically** — the integrator cannot choose or switch it through the API.

| Server type | Accounting model |
|-------------|------------------|
| **Ordinary server** | Classic single-level model: each **client** (salepoint / outlet) carries its own payments, balance and debt. |
| **Contragent server** | Two-level model: a **contragent** (payer) and its **salepoints** (clients). |

To find out which server you are working with, rely on the method behavior described below: if the `setContragent` / `getContragent` methods are available and a `clientPoint` object appears in payment and balance responses, this is a **contragent server**.

### Ordinary server

Everything works as described in the rest of the documentation. No extra fields or objects appear:

- sales, payments, balance and debt are tracked per client (salepoint) separately;
- the `setContragent` and `getContragent` methods are unavailable and return an error (see below);
- the `contragent` field in `setClient` is also unavailable.

### Contragent server

Here an additional level is introduced — the **contragent**.

- A **contragent** is a legal entity / payer, "the company on the invoice".
- **Clients** are the **salepoints** (`client`) of the contragent, where sales actually happen.
- **Sales** run through the salepoints, while **money, balance and debt** are tracked on the contragent.

**Illustration:**

```
Romashka LLC               ← contragent (payer, "the company on the invoice")
├── Store #1               ← salepoint (client / salepoint)
└── Store #2               ← salepoint (client / salepoint)
```

Orders are placed on "Store #1" and "Store #2", while the whole financial picture (payments, balance, debt) rolls up to Romashka LLC.

**What changes in method behavior on a contragent server:**

- **Payments and opening balances** sent for a client (salepoint) are automatically attributed to that client's contragent. In responses, the `client` object then identifies the **contragent**, while a separate `clientPoint` object identifies the outlet the operation belonged to.
- **`getBalance`** aggregates the balance **by contragent** (not by individual salepoints).
- **Orders** are automatically linked to the contragent of the corresponding salepoint.
- A contragent may have **no salepoints at all** (a standalone legal entity). Such contragents are still present in `getBalance` and `getPayment`.

> **Important:** the specific field changes are described in the relevant sections:
> - linking a client to a contragent — [`setClient`](07-set-references.md#1017-setclient--createupdate-clients) (the `contragent` field);
> - payment attribution and the `clientPoint` object — [`setPayment`](09-finance-photo-extra.md#131-setpayment--create-payment), [`getPayment`](02-get-references.md#915-getpayment--payments);
> - opening balance — [`setBalance`](09-finance-photo-extra.md#132-setbalance--set-opening-balance);
> - balance aggregation by contragent — [`getBalance`](05-get-finance.md#929-getbalance--client-balances);
> - linking an order to a contragent — [`setOrder`](08-set-warehouse-orders.md#121-setorder--createupdate-order).

### Method availability

The `setContragent` and `getContragent` methods and the `contragent` field of `setClient` work **only on a contragent server**. On an ordinary server they return an HTTP 400 error:

```json
{
    "status": false,
    "result": [],
    "error": {
        "code": 400,
        "message": "Contragent mode is not enabled",
        "data": []
    }
}
```

---

### 17.1. `setContragent` — Create/update contragents

**Description:** Create or update contragents (legal entities / payers). The method is available **only on a contragent server**; on an ordinary server it returns the error `Contragent mode is not enabled` (HTTP 400).

Data is sent in a batch in `data.contragent` (the same batch-size limit as in the other SET methods applies). Each element is created or updated independently; the response contains `completed` / `error` counters.

**Request:**
```json
{
    "method": "setContragent",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "contragent": [
            {
                "CS_id": "",
                "SD_id": "",
                "code_1C": "000000500",
                "shortName": "Romashka",
                "firmName": "Romashka LLC",
                "address": "5 Amir Temur St.",
                "orentir": "Opposite the bank",
                "tel": "+998901234567",
                "comment": "Key contragent",
                "inn": "123456789",
                "active": true,
                "lat": 41.2995,
                "lon": 69.2401,
                "category": {
                    "code_1C": "000000001"
                },
                "territory": {
                    "code_1C": "000000001"
                },
                "bankDetails": {
                    "bankName": "Asaka Bank",
                    "accountNumber": "20208000900123456789",
                    "bankCode": "00444"
                }
            }
        ]
    }
}
```

**Element fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (one of) | Contragent identifier. If the record is not found — a new one is created (by `code_1C`). |
| `shortName` | string | ❌ | Short name |
| `firmName` | string | ❌ | Full legal entity name |
| `address` | string | ❌ | Address |
| `orentir` | string | ❌ | Landmark |
| `tel` | string | ❌ | Phone |
| `comment` | string | ❌ | Comment |
| `inn` | string | ❌ | INN |
| `active` | bool/string | ❌ | Active flag |
| `lat` / `lon` | float | ❌ | GPS coordinates |
| `category` | object | ✅ | Client category — `CS_id` / `SD_id` / `code_1C` |
| `territory` | object | ❌ | Territory (city) — `CS_id` / `SD_id` / `code_1C`. If not set or not found, the default city is used. |
| `bankDetails` | object | ❌ | Bank details |
| `bankDetails.bankName` | string | ❌ | Bank name |
| `bankDetails.accountNumber` | string | ❌ | Account number |
| `bankDetails.bankCode` | string | ❌ | MFO / bank code |

> For nested objects (`category`, `territory`) the common lookup order applies: **CS_id → SD_id → code_1C**.

**Response:**
```json
{
    "status": true,
    "result": {
        "completed": 1,
        "error": 0,
        "data": {
            "contragent": [
                {
                    "CS_id": "F1-d0_500",
                    "SD_id": "d0_500",
                    "code_1C": "000000500"
                }
            ]
        }
    }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `completed` | int | Number of successfully processed contragents |
| `error` | int | Number of contragents with an error |
| `data.contragent[].CS_id` | string | Contragent external identifier (with filial prefix) |
| `data.contragent[].SD_id` | string | Contragent server ID |
| `data.contragent[].code_1C` | string | Contragent 1C code |

**Element errors:**

| Message | Reason |
|-----------|---------|
| `Contragent mode is not enabled` | Method called on an ordinary server (HTTP 400 for the whole request) |
| `Contragent is not found or cannot created` | Contragent not found and cannot be created from the given identifiers |
| `ClientCategory is not found` | Category (`category`) not found |
| `City is not found` | Territory (`territory`) not found and no default city exists |
| validation errors | The contragent model failed validation (an object with field errors is returned) |

> The element error format is the same as in the other SET methods: when errors are present, the response comes with `status: false`, an `error` block and populated `completed` / `error` counters.

---

### 17.2. `getContragent` — Contragents list

**Description:** Returns the list of contragents together with their salepoints (`client`). The method is available **only on a contragent server**; on an ordinary server it returns the error `Contragent mode is not enabled` (HTTP 400).

**Request:**
```json
{
    "method": "getContragent",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 100,
        "page": 1,
        "filter": {
            "contragent": {
                "code_1C": "000000500",
                "active": "Y"
            },
            "include": "all"
        }
    }
}
```

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.contragent.CS_id` / `SD_id` / `code_1C` | Identifier of a specific contragent |
| `filter.contragent.active` | Contragent active flag |
| `filter.include` | Record source: `sd`, `1c`, `all` |

**Values for `filter.include`:**

| Value | Which records are returned |
|----------|---------------------------|
| `sd` | Only contragents created in SalesDoc (without a 1C code) |
| `1c` | Only contragents loaded from 1C (with a 1C code) |
| `all` | All contragents |

**Response:**
```json
{
    "status": true,
    "result": {
        "contragent": [
            {
                "CS_id": "F1-d0_500",
                "SD_id": "d0_500",
                "code_1C": "000000500",
                "name": "Romashka",
                "firmName": "Romashka LLC",
                "address": "5 Amir Temur St.",
                "waymark": "Opposite the bank",
                "tel": "+998901234567",
                "lon": 69.2401,
                "lat": 41.2995,
                "allowKredit": 1,
                "allowConsig": 0,
                "comment": "Key contragent",
                "photo": null,
                "active": "Y",
                "inn": "123456789",
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
                "city": {
                    "CS_id": "d0_13",
                    "SD_id": "d0_13",
                    "code_1C": "000000001"
                },
                "client": [
                    {
                        "CS_id": "F1-d0_100",
                        "SD_id": "d0_100",
                        "code_1C": "000000100",
                        "name": "Store #1"
                    },
                    {
                        "CS_id": "F1-d0_101",
                        "SD_id": "d0_101",
                        "code_1C": "000000101",
                        "name": "Store #2"
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 100, "total": 1, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Contragent external identifier (with filial prefix) |
| `SD_id` | string | Contragent server ID |
| `code_1C` | string | Contragent 1C code |
| `name` | string | Short name |
| `firmName` | string | Full legal entity name |
| `address` | string | Address |
| `waymark` | string | Landmark |
| `tel` | string | Phone |
| `lon` / `lat` | float | GPS coordinates |
| `allowKredit` | int | Credit allowed (`1` / `0`) |
| `allowConsig` | int | Consignment allowed (`1` / `0`) |
| `comment` | string | Comment |
| `photo` | string\|null | Photo |
| `active` | string | Active flag |
| `inn` | string | INN |
| **bankDetails** | object | Bank details |
| `bankDetails.bank` | string | Bank name |
| `bankDetails.account` | string | Account number |
| `bankDetails.mfo` | string | MFO / bank code |
| **category** | object | Contragent category (`CS_id`, `SD_id`, `code_1C`) |
| **city** | object | Territory / city (`CS_id`, `SD_id`, `code_1C`) |
| **client** | array | Contragent salepoints |
| `client[].CS_id` | string | Salepoint external identifier |
| `client[].SD_id` | string | Salepoint server ID |
| `client[].code_1C` | string | Salepoint 1C code |
| `client[].name` | string | Salepoint name |

> For a contragent without salepoints the `client` array is returned empty (`[]`) — such a contragent is still present in the list.

---

[← Back to contents](README.md)
