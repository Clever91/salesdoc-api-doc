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

**Параметры склада:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | ID склада |
| `date` | string | ❌ | Дата инвентаризации (по умолчанию текущая) |
| `dont_zero_others` | bool | ❌ | `true` — не обнулять остатки товаров, не указанных в запросе |
| `products` | array | ✅ | Список товаров с количеством |

**Параметры товара:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | ID товара |
| `quantity` | float | ✅ | Количество (≥ 0) |
| `price` | float | ❌ | Себестоимость |

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

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (один из для обновления) | Идентификатор прихода (если пусто — создаётся новый) |
| `date` | string | ✅ | Дата прихода (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Комментарий |
| `priceType` | object | ✅ | Тип цены (закупочный, TYPE=1). CS_id / SD_id / code_1C |
| `store` | object | ✅ | Склад. CS_id / SD_id / code_1C |
| `shipper` | object | ❌ | Поставщик. CS_id / SD_id / code_1C (если не указан — основной поставщик) |
| `products` | array | ✅ | Список товаров с количеством и ценой |

**Product fields in products:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор товара |
| `quantity` | float | ✅ | Количество (≥ 0) |
| `price` | float | ✅ | Цена (≥ 0) |
| `mfg_date` | string | ❌ | Дата изготовления (Y-m-d) |
| `exp_date` | string | ❌ | Срок годности (Y-m-d) |

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

**Поля элемента:**

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

**Поля элемента:**

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

**Поля элемента:**

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

**Поля элемента:**

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

**Поля элемента:**

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

**Поля элемента (каждый элемент массива `data`):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` | string | ❌ | Код документа в 1С (только для нового; если указан и уже есть — ошибка) |
| `SD_id` | — | ❌ | Не передавать; редактирование по SD_id запрещено |
| `date` | string | ❌ | Дата корректировки (по умолчанию текущая) |
| `warehouse` | object | ✅ | Склад. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Товары и разница остатка (+ увеличение, − уменьшение) |

**Product fields in products:** идентификатор товара (CS_id / SD_id / code_1C), `quantity` (float; может быть отрицательным). Проверяется, что новый остаток ≥ 0.

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

**Поля заказа:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Идентификатор заказа |
| `status` | int | ✅ | Статус (1-5) |
| `dateCreate` | string | ❌ | Дата создания |
| `dateShipment` | string | ❌ | Дата отгрузки |
| `comment` | string | ❌ | Комментарий |
| `consignment` | bool | ❌ | Консигнация |
| `consigDate` | string | ❌ | Дата консигнации |
| `client` | object | ✅ | Клиент |
| `agent` | object | ✅ | Агент |
| `expeditor` | object | ❌ | Экспедитор |
| `priceType` | object | ✅ | Тип цены |
| `warehouse` | object | ✅ | Склад |
| `auto_attach_expeditor` | bool | ❌ | Автопривязка экспедитора |
| `debtDateExp` | string | ❌ | Дата истечения долга |
| `orderProducts` | array | ✅ | Товары заказа |
| `bonusProducts` | array | ❌ | Бонусные товары |

**Поля продукта заказа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `product` | object | Товар (`code_1C` / `SD_id`) |
| `quantity` | float | Количество |
| `price` | float | Цена (если не указано — берётся из прайса) |
| `discount` | float | Скидка в процентах (0-100) |
| `returned` | float | Количество возврата |

**Статусы заказов:**

| Статус | Описание |
|--------|----------|
| 1 | Новый |
| 2 | Отправлен |
| 3 | Доставлен |
| 4 | Закрыт |
| 5 | Отменён |

---

### 12.2. `setDeletedOrder` — Cancel order

**Description:** Отмена заказа (установка статуса 5 — Отменён).

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

**Description:** Изменение статуса одного или нескольких заказов.

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
| `code_1C` / `SD_id` | string | ✅ | Идентификатор заказа |
| `status` | int | ✅ | Новый статус |
| `dateShipment` | string | ❌ | Дата отгрузки (формат: Y-m-d) |

---

### 12.4. `setCode` — Set 1C code for documents

**Description:** Установка XML_ID (кода 1С) для различных документов (заказы, возвраты, обмены и др.).

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

**Поддерживаемые типы документов:**
- `Order` — Заказ
- `OrderDefect` — Возврат от клиента
- `OrderReplace` — Обмен
- `Purchase` — Приход (закупка)
- `PurchaseRefund` — Возврат поставщику
- `Exchange` — Перемещение между складами
- `VsExchange` — Обмен (Vansel)
- `Excretion` — Списание
- Любая другая существующая модель с полем code_1C

---

### 12.5. `setOrderDefect` — Create return

**Description:** Создание документа возврата товара от клиента (в веб-интерфейсе называется **«Возврат с полки»**). Документ фиксирует возврат товара от клиента на склад: клиент, агент, склад, тип цены и список возвращаемых товаров с количеством и ценой.

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
                "comment": "Брак",
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
| `code_1C` / `SD_id` | string | ✅ (один из) | Идентификатор документа возврата |
| `status` | int | ✅ | Статус (1–5) |
| `dateCreate` | string | ❌ | Дата создания (`Y-m-d H:i:s`) |
| `dateDefect` | string | ❌ | Дата возврата (Y-m-d) |
| `comment` | string | ❌ | Комментарий (причина возврата) |
| `client` | object | ✅ | Клиент. CS_id / SD_id / code_1C |
| `agent` | object | ✅ | Агент. CS_id / SD_id / code_1C |
| `expeditor` | object | ❌ | Экспедитор. CS_id / SD_id / code_1C |
| `priceType` | object | ✅ | Тип цены. CS_id / SD_id / code_1C |
| `warehouse` | object | ✅ | Склад. CS_id / SD_id / code_1C |
| `defectProducts` | array | ✅ | Список возвращаемых товаров |

**Product fields in defectProducts:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `product` | object | ✅ | Товар. CS_id / SD_id / code_1C |
| `quantity` | float | ✅ | Количество возврата |
| `price` | float | ❌ | Цена (если не указано — из прайса) |

---

### 12.6. `syncOrder` — Order sync

**Description:** Присвоение кода 1С и при необходимости изменение статуса заказов, возвратов и обменов. Используется после того, как документ обработан в 1С: интегратор передаёт `SD_id` документа из SalesDoc и присвоенный в 1С код (`code_order` для заказа, `code_return` для возврата, `code_replace` для обмена). Опционально можно передать новый `status` (1–5).

**Типичный сценарий для интегратора:**

1. **Получить несинхронизированные документы** — в методах `getOrder`, `getOrderDefect`, `getOrderReplace` использовать фильтр **`filter.include`**:
   - `"include": "sd"` — только документы, созданные в SalesDoc (как правило, ещё без кода 1С). Удобно, чтобы выбирать только те заказы/возвраты/обмены, которые нужно отправить в 1С и затем «подтвердить» через `syncOrder`.
   - `"include": "1c"` — только документы, уже имеющие код 1С (синхронизированные).
   - `"include": "all"` — все документы без фильтра по источнику.
2. Передать выбранные документы в 1С, получить от 1С присвоенные коды.
3. Вызвать **`syncOrder`**, передав для каждого документа `SD_id` и соответствующие коды (`code_order`, `code_return`, `code_replace`) и при необходимости `status`.
4. После успешного вызова документ считается синхронизированным; при следующих запросах с `include: "1c"` он будет попадать в выборку.

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
| `SD_id` | string | ✅ | ID заказа в SalesDoc |
| `code_order` | string | ❌* | Код заказа в 1С |
| `code_return` | string | ❌* | Код возврата в 1С |
| `code_replace` | string | ❌ | Код обмена в 1С |
| `status` | int | ❌ | Новый статус (1–5). Если 0 или не передан — статус не меняется |

> *Должно быть указано хотя бы одно из полей: `code_order`, `code_return`, `code_replace` (для обмена — оба `code_return` и `code_replace`).

**Response:** В отличие от других SET-методов, возвращается объект с массивами `completed` (успешно обработанные элементы из `data`) и `failed` (элементы с ошибкой и полем `error`). Пагинации нет.

---

[← Back to contents](README.md) | [Next: Finance, Photo, Extra →](09-finance-photo-extra.md)

