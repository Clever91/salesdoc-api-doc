# GET methods — Extra (9.32–9.46)

[← Back to contents](README.md)

---

### 9.32. `getInventory` — Inventory

**Description:** Получение данных инвентаризации оборудования у клиентов.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате начала инвентаризации |

**Request:**
```json
{
    "method": "getInventory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "period": {
                "date": {
                    "from": "2025-01-01",
                    "to": "2025-12-31"
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
        "inventory": [
            {
                "SD_id": "d0_1",
                "code_1C": "INV001",
                "name": "Холодильник Samsung",
                "model": "RT35K5440S8",
                "serial_no": "SN123456",
                "inventory_no": "INV-001",
                "production_date": "2023-01-15",
                "from_date": "2025-03-01",
                "to_date": null,
                "state": "good",
                "comment": "В хорошем состоянии",
                "type": {
                    "SD_id": "1",
                    "name": "Холодильник",
                    "code_1C": "FRIDGE"
                },
                "agents": [
                    {
                        "SD_id": "d0_3",
                        "name": "Иванов Иван",
                        "code_1C": "000000003"
                    }
                ],
                "client": {
                    "SD_id": "d0_100",
                    "name": "Магазин Звезда",
                    "code_1C": "000000100"
                },
                "city": {
                    "SD_id": "d0_1",
                    "name": "Чиланзар",
                    "code_1C": "000000001"
                }
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 25, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `name` | string | Название оборудования |
| `model` | string | Модель |
| `serial_no` | string | Серийный номер |
| `inventory_no` | string | Инвентарный номер |
| `production_date` | string | Дата производства |
| `from_date` | string | Дата начала размещения |
| `to_date` | string\|null | Дата окончания размещения |
| `state` | string | Состояние (например `good`) |
| `comment` | string | Комментарий |
| **type** | object | Тип оборудования |
| `type.SD_id` | string | Серверный ID типа |
| `type.name` | string | Название типа |
| `type.code_1C` | string | Код типа в 1С |
| **agents** | array | Массив агентов |
| `agents[].SD_id` | string | ID агента |
| `agents[].name` | string | Имя агента |
| `agents[].code_1C` | string | Код агента в 1С |
| **client** | object | Клиент |
| `client.SD_id` | string | ID клиента |
| `client.name` | string | Название клиента |
| `client.code_1C` | string | Код клиента в 1С |
| **city** | object | Город |
| `city.SD_id` | string | ID города |
| `city.name` | string | Название города |
| `city.code_1C` | string | Код города в 1С |

---

### 9.33. `getShipper` — Suppliers

**Description:** Получение списка поставщиков.

**Request:**
```json
{
    "method": "getShipper",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "shipper": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "ООО Поставщик",
                "phone_number": "+998901112233",
                "address": "ул. Навои, 50",
                "active": true,
                "code_1C": "SHIP001"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `name` | string | Название поставщика |
| `phone_number` | string | Телефон |
| `address` | string | Адрес |
| `active` | boolean | Активность |
| `code_1C` | string | Код в 1С |

---

### 9.34. `getPaymentConfirmation` — Payment confirmation

**Description:** Получение списка оплат, отправленных экспедиторами на подтверждение.

**Request:**
```json
{
    "method": "getPaymentConfirmation",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "paymentConfirmation": [
            {
                "CS_id": "F1-d0_60",
                "SD_id": "d0_60",
                "code_1C": null,
                "client": {
                    "CS_id": "F1-d0_100",
                    "SD_id": "d0_100",
                    "name": "Магазин Звезда",
                    "code_1C": "000000100"
                },
                "order": {
                    "CS_id": "F1-d0_300",
                    "SD_id": "d0_300"
                },
                "payment_type": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "amount": 5000000.00,
                "date": "2025-06-15 14:30:00",
                "comment": "Оплата при доставке",
                "confirmed": true
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 15, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string\|null | Код в 1С |
| **client** | object | Клиент |
| `client.CS_id` | string | Идентификатор клиента |
| `client.SD_id` | string | Серверный ID клиента |
| `client.name` | string | Название клиента |
| `client.code_1C` | string | Код клиента в 1С |
| **order** | object | Заказ |
| `order.CS_id` | string | Идентификатор заказа |
| `order.SD_id` | string | Серверный ID заказа |
| **payment_type** | object | Тип оплаты |
| `payment_type.CS_id` | string | Идентификатор типа оплаты |
| `payment_type.SD_id` | string | Серверный ID типа оплаты |
| `payment_type.code_1C` | string | Код типа оплаты в 1С |
| `amount` | float | Сумма оплаты |
| `date` | string | Дата и время |
| `comment` | string | Комментарий |
| `confirmed` | boolean | Подтверждена: `true` — да, `false` — нет |

---

### 9.36. `getBonus` — Bonuses

**Description:** Получение списка бонусных программ.

**Request:**
```json
{
    "method": "getBonus",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "data": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "name": "Купи 10 — получи 1",
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
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `name` | string | Название бонусной программы |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |

---

### 9.37. `getPRCategory` — Photo report categories

**Description:** Получение категорий фотоотчётов.

**Request:**
```json
{
    "method": "getPRCategory",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "prCategory": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Выкладка товара",
                "active": "Y"
            },
            {
                "CS_id": "2",
                "SD_id": "2",
                "name": "Вывеска",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 2, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `name` | string | Название категории |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |

---

### 9.38. `getPhotoReport` — Photo reports

**Description:** Получение фотоотчётов агентов **за текущий день**.

> ⚠️ Этот метод возвращает только фотоотчёты за **сегодняшнюю дату**.

**Request:**
```json
{
    "method": "getPhotoReport",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "photoReport": [
            {
                "CS_id": "F1-d0_200",
                "SD_id": "d0_200",
                "agent_id": "d0_3",
                "client_id": "d0_100",
                "cat_id": "1",
                "role": "4",
                "url": "https://example.com/upload/photoReport/photo1.jpg",
                "date": "2025-06-15 10:15:00"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 50, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `agent_id` | string | ID агента |
| `client_id` | string | ID клиента |
| `cat_id` | string | ID категории фотоотчёта |
| `role` | string | Роль (тип пользователя) |
| `url` | string | URL фотографии |
| `date` | string | Дата и время фотоотчёта |

---

### 9.39. `getFilials` — Filials

**Description:** Получение списка активных филиалов.

**Request:**
```json
{
    "method": "getFilials",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "filials": [
            {
                "name": "Ташкент",
                "domain": "tashkent.example.com",
                "prefix": "F1",
                "filial_id": "FIL001"
            },
            {
                "name": "Самарканд",
                "domain": "samarkand.example.com",
                "prefix": "F2",
                "filial_id": "FIL002"
            }
        ]
    }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `name` | string | Название филиала |
| `domain` | string | Домен филиала |
| `prefix` | string | Префикс филиала (для CS_id) |
| `filial_id` | string | ID филиала |

---

### 9.40. `getStoreLog` — Store operations log

**Description:** Получение журнала складских операций за указанный период.

> ⚠️ **Ограничения:**
> - Rate limit: 5 запросов за 5 секунд
> - Метод доступен **только** с 20:00 до 07:00
> - Все идентификаторы — только `SD_id`

**Обязательные параметры:**

| Параметр | Тип | Описание |
|----------|-----|----------|
| `params.storeId` | string | SD_id склада |
| `params.from` | string | Дата начала (ISO-8601) |
| `params.to` | string | Дата окончания (ISO-8601) |

**Дополнительные фильтры:**

| Параметр | Тип | Описание |
|----------|-----|----------|
| `params.documents` | array | Фильтр по типам документов: `Purchase`, `Order`, `OrderReplace`, `OrderDefect`, `BonusOrder`, `StoreCorrector`, `Exchange`, `Excretion` |
| `params.products` | array | Фильтр по ID продуктов |

**Request:**
```json
{
    "method": "getStoreLog",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "storeId": "d0_1",
        "from": "2025-06-01",
        "to": "2025-06-30",
        "documents": ["Purchase", "Order"]
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": [
        {
            "product_id": "d0_15",
            "document": "Purchase",
            "document_id": "d0_50",
            "date": "2025-06-15 09:00:00",
            "quantity": 100.0
        },
        {
            "product_id": "d0_15",
            "document": "Order",
            "document_id": "d0_300",
            "date": "2025-06-15 14:00:00",
            "quantity": -10.0
        }
    ]
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `product_id` | string | SD_id товара |
| `document` | string | Тип документа (`Purchase`, `Order`, `OrderReplace`, `OrderDefect`, `BonusOrder`, `StoreCorrector`, `Exchange`, `Excretion`) |
| `document_id` | string | SD_id документа |
| `date` | string | Дата и время операции |
| `quantity` | float | Количество: положительное — приход, отрицательное — расход |

> **Примечание:** Положительное quantity — приход, отрицательное — расход.

---

### 9.41. `getClientPending` — Client requests (pending)

**Description:** Получение заявок на создание клиентов, находящихся в состоянии ожидания модерации.

**Request:**
```json
{
    "method": "getClientPending",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1
    }
}
```

**Response:**
```json
{
    "status": true,
    "result": {
        "clientPending": [
            {
                "CS_id": "F1-d0_500",
                "SD_id": "d0_500",
                "code_1C": null,
                "name": "Новый магазин",
                "firmName": "ООО Новый",
                "address": "ул. Чилонзор, 15",
                "waymark": "Рядом с аптекой",
                "tel": "+998901234567",
                "lon": 69.24,
                "lat": 41.30,
                "allowKredit": 0,
                "allowConsig": 0,
                "comment": "Заявка от агента",
                "photo": null,
                "active": "Y",
                "inn": "987654321",
                "contract": null,
                "createdAt": "2025-06-15 11:00:00",
                "bankDetails": {
                    "bank": null,
                    "account": null,
                    "mfo": null
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
                "createdBy": {
                    "CS_id": "F1-d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003",
                    "name": "Иванов Иван"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string\|null | Код в 1С |
| `name` | string | Название клиента |
| `firmName` | string | Юридическое название |
| `address` | string | Адрес |
| `waymark` | string | Ориентир |
| `tel` | string | Телефон |
| `lon` | float | Долгота (GPS) |
| `lat` | float | Широта (GPS) |
| `allowKredit` | integer | Разрешён кредит: `1` — да, `0` — нет |
| `allowConsig` | integer | Разрешена консигнация: `1` — да, `0` — нет |
| `comment` | string | Комментарий |
| `photo` | string\|null | Фото |
| `active` | string | Активность |
| `inn` | string | ИНН |
| `contract` | string\|null | Номер договора |
| `createdAt` | string | Дата создания заявки |
| **bankDetails** | object | Банковские реквизиты |
| **category** | object | Категория клиента (`CS_id`, `SD_id`, `code_1C`) |
| **type** | object | Тип клиента (`CS_id`, `SD_id`, `code_1C`) |
| **channel** | object | Канал сбыта (`CS_id`, `SD_id`, `code_1C`) |
| **city** | object | Город (`CS_id`, `SD_id`, `code_1C`) |
| **createdBy** | object | Кем создана заявка |
| `createdBy.CS_id` | string | Идентификатор создателя |
| `createdBy.SD_id` | string | Серверный ID создателя |
| `createdBy.code_1C` | string | Код в 1С |
| `createdBy.name` | string | Имя создателя |

---

### 9.42. `getCorrection` — Store corrections

**Description:** Получение списка корректировок складских остатков.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате корректировки |
| `filter.store` | По складу (`CS_id` / `SD_id` / `code_1C`) |

**Примеры использования фильтров:**

**Фильтр по дате корректировки:**
```json
{
    "method": "getCorrection",
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

**Фильтр по складу (`filter.store`):**
```json
{
    "method": "getCorrection",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "code_1C": "WH001" }
        }
    }
}
```

> Альтернативы для `filter.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **SD_id** (например `"d0_1"`) вместо code_1C.

**Комбинирование фильтров:**
```json
{
    "method": "getCorrection",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "SD_id": "d0_1" },
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

> Альтернативы для `filter.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **code_1C** (например `"WH001"`) вместо SD_id.

**Response:**
```json
{
    "status": true,
    "result": {
        "correction": [
            {
                "CS_id": "F1-d0_70",
                "SD_id": "d0_70",
                "code_1C": "COR000001",
                "date": "2025-06-15 08:00:00",
                "comment": "Пересчёт",
                "type": "correction",
                "store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "WH001",
                    "name": "Основной склад"
                },
                "items": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "product_name": "Кока-Кола 1.5л",
                        "quantity": 5.0,
                        "price": 15000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 10, "page": 1 }
}
```

**Типы корректировок:**

| Тип | Описание |
|-----|----------|
| `correction` | Корректировка |
| `inventory` | Инвентаризация |

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `date` | string | Дата и время корректировки |
| `comment` | string | Комментарий |
| `type` | string | Тип: `correction` или `inventory` |
| **store** | object | Склад |
| `store.CS_id` | string | Идентификатор склада |
| `store.SD_id` | string | Серверный ID склада |
| `store.code_1C` | string | Код склада в 1С |
| `store.name` | string | Название склада |
| **items** | array | Массив товаров |
| `items[].CS_id` | string | Идентификатор товара |
| `items[].SD_id` | string | Серверный ID товара |
| `items[].code_1C` | string | Код товара в 1С |
| `items[].product_name` | string | Название товара |
| `items[].quantity` | float | Количество |
| `items[].price` | float | Цена |

---

### 9.43. `getPurchase` — Purchases

**Description:** Получение списка документов закупки.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате закупки |
| `filter.period.dateUpdate.from/to` | По дате обновления |
| `filter.store` | По складу (`CS_id` / `SD_id` / `code_1C`) |

**Примеры использования фильтров:**

**Фильтр по дате закупки:**
```json
{
    "method": "getPurchase",
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

**Фильтр по дате обновления (`filter.period.dateUpdate`):**
```json
{
    "method": "getPurchase",
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

**Фильтр по складу (`filter.store`):**
```json
{
    "method": "getPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "code_1C": "WH001" }
        }
    }
}
```

> Альтернативы для `filter.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **SD_id** (например `"d0_1"`) вместо code_1C.

**Комбинирование всех фильтров:**
```json
{
    "method": "getPurchase",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "store": { "SD_id": "d0_1" },
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

> Альтернативы для `filter.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **code_1C** (например `"WH001"`) вместо SD_id.

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
                "date": "2025-06-15 09:00:00",
                "updated": "2025-06-15 09:00:00",
                "purchase_id": "F1-d0_50",
                "priceType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "shipper": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "ООО Поставщик",
                    "code_1C": "SHIP001"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "name": "Кока-Кола 1.5л",
                        "quantity": 100.0,
                        "price": 12000.00,
                        "amount": 1200000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 20, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор склада (с префиксом) |
| `SD_id` | string | Серверный ID склада |
| `code_1C` | string | Код склада в 1С |
| `name` | string | Название склада |
| `date` | string | Дата закупки |
| `updated` | string | Дата последнего обновления |
| `purchase_id` | string | ID документа закупки |
| **priceType** | object | Тип цены |
| `priceType.CS_id` | string | Идентификатор типа цены |
| `priceType.SD_id` | string | Серверный ID типа цены |
| `priceType.code_1C` | string | Код типа цены в 1С |
| **shipper** | object | Поставщик |
| `shipper.CS_id` | string | Идентификатор поставщика |
| `shipper.SD_id` | string | Серверный ID поставщика |
| `shipper.name` | string | Название поставщика |
| `shipper.code_1C` | string | Код поставщика в 1С |
| **detail** | array | Массив товаров |
| `detail[].CS_id` | string | Идентификатор товара |
| `detail[].SD_id` | string | Серверный ID товара |
| `detail[].code_1C` | string | Код товара в 1С |
| `detail[].name` | string | Название товара |
| `detail[].quantity` | float | Количество |
| `detail[].price` | float | Цена за единицу |
| `detail[].amount` | float | Общая сумма по позиции |

---

### 9.44. `getMovement` — Movements

**Description:** Получение списка документов перемещения товаров между складами.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате перемещения |
| `filter.include` | `sd`, `1c`, `all` — по источнику |
| `filter.type` | Тип перемещения (см. таблицу ниже) |

**Типы перемещений (`filter.type`):**

| Значение | Код | Описание |
|----------|-----|----------|
| *(не указан)* | 1 | Обычное перемещение между складами (по умолчанию) |
| `vansel` | 2 | Перемещение типа «ван-сэл» |

> Если `filter.type` не указан — возвращаются только обычные перемещения. Чтобы получить ван-сэл перемещения, укажите `"type": "vansel"`.

**Примеры использования фильтров:**

**Фильтр по дате и источнику:**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "all",
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

**Фильтр по источнику (`filter.include`):**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "1c"
        }
    }
}
```
> Значения: `sd` — только из SalesDoc, `1c` — только из 1С, `all` — все записи.

**Фильтр по типу перемещения (`filter.type`):**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "type": "vansel"
        }
    }
}
```
> `vansel` — возвращает только перемещения типа «ван-сэл». Если не указан — возвращаются обычные перемещения.

**Комбинирование всех фильтров:**
```json
{
    "method": "getMovement",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "all",
            "type": "vansel",
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
        "movement": [
            {
                "CS_id": "F1-d0_80",
                "SD_id": "d0_80",
                "code_1C": "MOV000001",
                "date": "2025-06-15 07:00:00",
                "from_store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "name": "Основной склад",
                    "code_1C": "WH001"
                },
                "to_store": {
                    "CS_id": "F1-d0_2",
                    "SD_id": "d0_2",
                    "name": "Склад агента",
                    "code_1C": "WH002"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "quantity": 50.0
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 15, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `date` | string | Дата и время перемещения |
| **from_store** | object | Склад-отправитель |
| `from_store.CS_id` | string | Идентификатор склада |
| `from_store.SD_id` | string | Серверный ID склада |
| `from_store.name` | string | Название склада |
| `from_store.code_1C` | string | Код склада в 1С |
| **to_store** | object | Склад-получатель |
| `to_store.CS_id` | string | Идентификатор склада |
| `to_store.SD_id` | string | Серверный ID склада |
| `to_store.name` | string | Название склада |
| `to_store.code_1C` | string | Код склада в 1С |
| **detail** | array | Массив товаров |
| `detail[].CS_id` | string | Идентификатор товара |
| `detail[].SD_id` | string | Серверный ID товара |
| `detail[].code_1C` | string | Код товара в 1С |
| `detail[].quantity` | float | Количество |

---

### 9.45. `getVsExchange` — Warehouse exchanges

**Description:** Получение списка документов обмена товаров (Ван-сэл обмен).

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате обмена |
| `filter.include` | `sd`, `1c`, `all` — по источнику |

**Request:**
```json
{
    "method": "getVsExchange",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "all",
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
        "vsExchange": [
            {
                "CS_id": "F1-d0_90",
                "SD_id": "d0_90",
                "code_1C": "VSE000001",
                "total_count": 20.0,
                "total_summa": 300000.00,
                "comment": "Обмен товара",
                "date": "2025-06-15 08:00:00",
                "date_load": "2025-06-15",
                "from_store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "WH001"
                },
                "to_store": {
                    "CS_id": "F1-d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "WH002"
                },
                "agent": {
                    "CS_id": "F1-d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003"
                },
                "price_type": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "quantity": 20.0,
                        "price": 15000.00,
                        "summa": 300000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 5, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `total_count` | float | Общее количество товаров |
| `total_summa` | float | Общая сумма |
| `comment` | string | Комментарий |
| `date` | string | Дата и время обмена |
| `date_load` | string | Дата документа |
| **from_store** | object | Склад-отправитель |
| `from_store.CS_id` | string | Идентификатор склада |
| `from_store.SD_id` | string | Серверный ID склада |
| `from_store.code_1C` | string | Код склада в 1С |
| **to_store** | object | Склад-получатель |
| `to_store.CS_id` | string | Идентификатор склада |
| `to_store.SD_id` | string | Серверный ID склада |
| `to_store.code_1C` | string | Код склада в 1С |
| **agent** | object | Агент |
| `agent.CS_id` | string | Идентификатор агента |
| `agent.SD_id` | string | Серверный ID агента |
| `agent.code_1C` | string | Код агента в 1С |
| **price_type** | object | Тип цены |
| `price_type.CS_id` | string | Идентификатор типа цены |
| `price_type.SD_id` | string | Серверный ID типа цены |
| `price_type.code_1C` | string | Код типа цены в 1С |
| **detail** | array | Массив товаров |
| `detail[].CS_id` | string | Идентификатор товара |
| `detail[].SD_id` | string | Серверный ID товара |
| `detail[].code_1C` | string | Код товара в 1С |
| `detail[].quantity` | float | Количество |
| `detail[].price` | float | Цена за единицу |
| `detail[].summa` | float | Сумма по позиции |

---

### 9.46. `getMovementBetweenFilial` — Movements between filials

**Description:** Получение перемещений товаров между филиалами.

> ⚠️ Для этого метода **обязателен** `filial_id` в запросе.

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате перемещения |

**Request:**
```json
{
    "method": "getMovementBetweenFilial",
    "auth": { "userId": "d0_1", "token": "..." },
    "filial": { "filial_id": "FIL001" },
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
        "movement": [
            {
                "CS_id": "F1-d0_95",
                "SD_id": "d0_95",
                "code_1C": "MBF000001",
                "date": "2025-06-15 07:00:00",
                "count": 30,
                "amount": 450000.00,
                "comment": "Перемещение в Самарканд",
                "from_store": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "name": "Основной склад",
                    "code_1C": "WH001"
                },
                "to_filial": {
                    "CS_id": "2",
                    "SD_id": "2",
                    "name": "Самарканд",
                    "domain": "samarkand.example.com"
                },
                "to_store": {
                    "CS_id": "F2-d0_1",
                    "SD_id": "d0_1",
                    "name": "Склад Самарканд",
                    "code_1C": "WH001-S"
                },
                "shipper": {
                    "CS_id": "1",
                    "SD_id": "1",
                    "name": "ООО Поставщик",
                    "code_1C": "SHIP001"
                },
                "priceType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "name": "Розничная",
                    "code_1C": "000000001"
                },
                "detail": [
                    {
                        "CS_id": "d0_15",
                        "SD_id": "d0_15",
                        "code_1C": "000000015",
                        "quantity": 30.0,
                        "price": 15000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 3, "page": 1 }
}
```

**Response fields:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `date` | string | Дата и время перемещения |
| `count` | integer | Количество товаров |
| `amount` | float | Общая сумма |
| `comment` | string | Комментарий |
| **from_store** | object | Склад-отправитель |
| `from_store.CS_id` | string | Идентификатор склада |
| `from_store.SD_id` | string | Серверный ID склада |
| `from_store.name` | string | Название склада |
| `from_store.code_1C` | string | Код склада в 1С |
| **to_filial** | object | Филиал-получатель |
| `to_filial.CS_id` | string | Идентификатор филиала |
| `to_filial.SD_id` | string | Серверный ID филиала |
| `to_filial.name` | string | Название филиала |
| `to_filial.domain` | string | Домен филиала |
| **to_store** | object | Склад-получатель в целевом филиале |
| `to_store.CS_id` | string | Идентификатор склада |
| `to_store.SD_id` | string | Серверный ID склада |
| `to_store.name` | string | Название склада |
| `to_store.code_1C` | string | Код склада в 1С |
| **shipper** | object | Поставщик |
| `shipper.CS_id` | string | Идентификатор поставщика |
| `shipper.SD_id` | string | Серверный ID поставщика |
| `shipper.name` | string | Название поставщика |
| `shipper.code_1C` | string | Код поставщика в 1С |
| **priceType** | object | Тип цены |
| `priceType.CS_id` | string | Идентификатор типа цены |
| `priceType.SD_id` | string | Серверный ID типа цены |
| `priceType.name` | string | Название типа цены |
| `priceType.code_1C` | string | Код типа цены в 1С |
| **detail** | array | Массив товаров |
| `detail[].CS_id` | string | Идентификатор товара |
| `detail[].SD_id` | string | Серверный ID товара |
| `detail[].code_1C` | string | Код товара в 1С |
| `detail[].quantity` | float | Количество |
| `detail[].price` | float | Цена за единицу |

---

[← Back to contents](README.md) | [Next: SET methods — References →](07-set-references.md)

