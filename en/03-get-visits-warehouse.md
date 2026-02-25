# GET methods — Visits, Warehouses, Stock (9.21–9.25)

[← Back to contents](README.md)

---

### 9.21. `getVisit` — Visits

**Description:** Получение данных о визитах агентов к клиентам. Включает информацию о плановых и фактических визитах, GPS-статус, наличие заказов и фотоотчётов.

**Filters:**

| Фильтр | Описание |
|--------|----------|
| `filter.period.date.from/to` | Период по дате визита |

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
                "agent_name": "Иванов Иван",
                "client_id": "d0_100",
                "client_name": "Магазин \"Звезда\"",
                "category_name": "Оптовый",
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
| `agent_id` | string | ID агента |
| `agent_name` | string | Имя агента |
| `client_id` | string | ID клиента |
| `client_name` | string | Название клиента |
| `category_name` | string | Категория клиента |
| `date` | string | Дата визита |
| `start_date` | string | Время начала визита (check-in) |
| `end_date` | string | Время окончания визита (check-out) |
| `planned` | int | Плановый визит: `1` — да, `0` — нет |
| `visited` | int | Фактический визит: `1` — да, `0` — нет |
| `reject` | string | Причина отказа (если есть, через запятую) |
| `has_photo_report` | int | Наличие фотоотчёта: `1` — да, `0` — нет |
| `gps_visit` | int | GPS-подтверждение: `1` — подтверждён, `0` — нет |
| `has_order` | int | Наличие заказа: `1` — да, `0` — нет |
| `order_summa` | float | Сумма заказов за визит |

---

### 9.22. `getWarehouse` — Warehouses

**Description:** Получение списка складов (тип: обычный склад).

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
                "name": "Основной склад",
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
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код склада в 1С |
| `name` | string | Название склада |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |

---

### 9.23. `getDefectWarehouse` — Return warehouses

**Description:** Получение списка складов для возврата (дефектных товаров).

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
                "name": "Склад возврата",
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
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код склада возврата в 1С |
| `name` | string | Название склада возврата |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |

---

### 9.24. `getStock` — Stock

**Description:** Получение текущих складских остатков, сгруппированных по складам.

**Filters:**

| Фильтр | Описание |
|--------|----------|
| `params.store.SD_id` | Фильтр по ID склада |
| `params.category.SD_id` | Фильтр по ID категории товара |

**Запрос (фильтр по складу):**
```json
{
    "method": "getStock",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "store": { "SD_id": "d0_1" }
    }
}
```

> Альтернативы для `params.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **code_1C** (например `"WH001"`) вместо SD_id.

**Запрос (фильтр по категории товара):**
```json
{
    "method": "getStock",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "category": { "SD_id": "d0_3" }
    }
}
```

> Альтернативы для `params.category`: можно указать **CS_id** (например `"d0_3"`) или **code_1C** (например `"000000003"`) вместо SD_id.

**Запрос (комбинирование фильтров: склад + категория):**
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

> Для `store` и `category` в каждом объекте можно использовать **CS_id**, **SD_id** или **code_1C**.

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
                "name": "Основной склад",
                "active": "Y",
                "products": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "name": "Кока-Кола 1.5л",
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
| **warehouse** | array | Массив складов |
| `warehouse[].CS_id` | string | Внешний идентификатор склада |
| `warehouse[].SD_id` | string | Серверный ID склада |
| `warehouse[].code_1C` | string | Код склада в 1С |
| `warehouse[].name` | string | Название склада |
| `warehouse[].active` | string | Активность: `Y` — активен, `N` — неактивен |
| **warehouse[].products** | array | Массив товаров на складе |
| `products[].CS_id` | string | Идентификатор товара |
| `products[].SD_id` | string | Серверный ID товара |
| `products[].name` | string | Название товара |
| `products[].code_1C` | string | Код товара в 1С |
| `products[].active` | string | Активность товара |
| `products[].quantity` | float | Остаток товара на складе |

---

### 9.25. `getStockForDate` — Stock by date

**Description:** Получение складских остатков на указанную дату. Рассчитывается на основе журнала складских операций.

> ⚠️ **Ограничения:**
> - Если включён параметр — лимит 1 запрос за 5 минут
> - Если не включён — метод доступен **только** с 20:00 до 08:00
> - Дата должна быть в формате ISO-8601

**Обязательные параметры:**

| Параметр | Тип | Описание |
|----------|-----|----------|
| `params.date` | string | Дата (ISO-8601) |
| `params.store` | object | Склад (`CS_id` / `SD_id` / `code_1C`) |

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

> Альтернативы для `params.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **SD_id** (например `"d0_1"`) вместо code_1C.

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
| `date` | string | Запрошенная дата |
| **store** | object | Склад |
| `store.code_1C` | string | Код склада в 1С |
| **stock** | array | Массив остатков товаров |
| `stock[].SD_id` | string | Серверный ID товара |
| `stock[].code_1C` | string | Код товара в 1С |
| `stock[].amount` | float | Количество товара на дату |

---

[← Back to contents](README.md) | [Next: Orders →](04-get-orders.md)

