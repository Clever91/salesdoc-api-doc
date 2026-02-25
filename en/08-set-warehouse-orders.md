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
                "name": "Основной склад",
                "active": true
            }
        ]
    }
}
```

---

### 11.2. `setStock` — Set warehouse stock (inventory)

**Description:** Полная инвентаризация склада. Обновляет остатки всех переданных товаров. По умолчанию обнуляет остатки товаров, которые **не были** переданы в запросе.

> ⚠️ **Rate limit:** Максимум 5 запросов за 5 секунд.

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

**Description:** Создание или обновление документа прихода (закупки) товаров на склад. Для поиска существующего документа используется CS_id / SD_id / code_1C (см. порядок идентификации в 07-set-references).

**Request:**
```json
{
    "method": "setPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "purchase": [
            {
                "date": "2025-06-15 10:00:00",
                "comment": "Закупка от поставщика",
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

**Description:** Создание документа перемещения товаров между двумя складами. Существующие перемещения редактировать нельзя. Обязательно указывается уникальный `code_1C` для документа.

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
                "comment": "Перемещение на склад агента",
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
| `code_1C` | string | ✅ | Уникальный код документа (из 1С). Не должен совпадать с существующим перемещением |
| `date` | string | ✅ | Дата перемещения (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Комментарий |
| `storeFrom` | object | ✅ | Склад-источник. CS_id / SD_id / code_1C |
| `storeTo` | object | ✅ | Склад-назначение. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Товары и количество (на складе-источнике проверяется наличие) |

**Product fields in products:** `CS_id` / `SD_id` / `code_1C` — идентификатор товара; `quantity` — количество (float).

---

### 11.5. `setVsExchange` — Exchange (Vansel)

**Description:** Создание или обновление документа обмена товаров на складе (схема Vansel). Агент должен быть типа VanSelling; используется склад агента.

**Request:**
```json
{
    "method": "setVsExchange",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "vsExchange": [
            {
                "date": "2025-06-15 08:00:00",
                "comment": "Обмен товара",
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
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (один из для обновления) | Идентификатор документа обмена |
| `date` | string | ✅ | Дата (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Комментарий |
| `priceType` | object | ✅ | Тип цены (TYPE=2). CS_id / SD_id / code_1C |
| `store` | object | ✅ | Склад-источник. CS_id / SD_id / code_1C |
| `agent` | object | ✅ | Агент типа Vansel. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Товары и количество |

**Product fields in products:** идентификатор товара (CS_id / SD_id / code_1C), `quantity` (float, > 0). Цена может подставляться из прайса.

---

### 11.6. `setMovingToFilial` — Move to filial

**Description:** Перемещение товаров из склада текущего филиала на склад другого филиала. В запросе обязательно указываются `storeFrom` и `movingTo` (филиал и склад назначения) с **CS_id** (префикс филиала). Создаётся возврат поставщику в текущем филиале и приход в целевом филиале.

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
                "comment": "Перемещение в филиал F2",
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
| `date` | string | ✅ | Дата перемещения |
| `comment` | string | ❌ | Комментарий |
| `priceType` | object | ✅ | Тип цены (закупочный). CS_id / SD_id / code_1C |
| `storeFrom` | object | ✅ | Склад текущего филиала; **CS_id обязателен** (с префиксом филиала) |
| `movingTo` | object | ✅ | Филиал и склад назначения |
| `movingTo.filialId` | string | ✅ | Код филиала (xml_id) |
| `movingTo.storeTo` | object | ✅ | Склад филиала назначения; **CS_id обязателен** |
| `products` | array | ✅ | Товары: идентификатор, `quantity`, `price` |

---

### 11.7. `setSupplierReturn` — Supplier return

**Description:** Создание или обновление документа возврата товара поставщику со склада. Проверяется наличие товара на складе.

**Request:**
```json
{
    "method": "setSupplierReturn",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "return": [
            {
                "date": "2025-06-15 14:00:00",
                "comment": "Возврат брака",
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
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (один из для обновления) | Идентификатор документа возврата |
| `date` | string | ✅ | Дата (`Y-m-d H:i:s`) |
| `comment` | string | ❌ | Комментарий |
| `priceType` | object | ✅ | Тип цены. CS_id / SD_id / code_1C |
| `store` | object | ✅ | Склад. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Товары: идентификатор (CS_id / SD_id / code_1C), `quantity` (> 0), `price` |

---

### 11.8. `setExcretion` — Write-off

**Description:** Создание или обновление списания товара со склада. Уменьшает остатки на складе. Наличие товара проверяется.

**Request:**
```json
{
    "method": "setExcretion",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "excretion": [
            {
                "date": "2025-06-15 16:00:00",
                "comment": "Списание по акту",
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
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (один из для обновления) | Идентификатор документа списания |
| `date` | string | ✅ | Дата списания (формат `Y-m-d H:i:s` или ISO-8601) |
| `comment` | string | ❌ | Комментарий |
| `fromStore` | object | ✅ | Склад. CS_id / SD_id / code_1C |
| `products` | array | ✅ | Товары и количество (положительное) |

**Product fields in products:** идентификатор товара (CS_id / SD_id / code_1C), `quantity` (float, > 0).

---

### 11.9. `setCorrection` — Stock correction

**Description:** Ручная корректировка складских остатков. Поддерживается **только создание** новых документов; редактирование существующих (по SD_id или по уже существующему code_1C) запрещено. Данные передаются массивом напрямую в `data`. Склад должен быть привязан к созданному складу. `quantity` в позиции может быть положительным (увеличение) или отрицательным (уменьшение); итоговый остаток не должен стать отрицательным.

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

**Description:** Создание или обновление заказа (заявки). Включает проверку остатков, бронирование, бонусные товары.

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
                "comment": "Срочный заказ",
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

