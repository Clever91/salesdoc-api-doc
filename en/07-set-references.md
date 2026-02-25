# SET methods — References (10.1–10.21)

[← Back to contents](README.md)

All SET methods create or update records. Data is sent in the `data` block. The response includes the number of successfully processed (`completed`) and failed (`error`) records.

### Common SET response format:
```json
{
    "status": true,
    "result": {
        "completed": 3,
        "error": 0,
        "data": {
            "ключ": [
                {
                    "CS_id": "...",
                    "SD_id": "...",
                    "code_1C": "..."
                }
            ]
        }
    }
}
```

### Record identification in all SET methods

Во всех SET-методах для поиска существующей записи (или вложенных объектов: тип оплаты, валюта, категория и т.д.) можно указывать **CS_id**, **SD_id** или **code_1C**. Система ищет запись в таком порядке:

1. **CS_id** — уникальный идентификатор в нашей системе (приоритет поиска).
2. **SD_id** — внутренний серверный ID (если CS_id не указан или не найден).
3. **code_1C** — внешний код (XML_ID из 1С); **не является уникальным** в нашей системе.

Если ни одна запись по указанным идентификаторам не найдена, создаётся новая запись. Для вложенных объектов (например, `paymentType`, `valyutaType`, `category`) действует тот же порядок: CS_id → SD_id → code_1C.

---

### 10.1. `setValyutaType` — Create/update currency types

**Description:** Создание или обновление типов валют.

**Request:**
```json
{
    "method": "setValyutaType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "valyutaType": [
            {
                "CS_id": "",
                "SD_id": "",
                "code_1C": "000000001",
                "name": "Сум",
                "short": "UZS",
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название |
| `short` | string | ❌ | Краткое обозначение |
| `active` | bool/string | ❌ | Активность |

---

### 10.2. `setUnit` — Create/update units of measure

**Request:**
```json
{
    "method": "setUnit",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "unit": [
            {
                "code_1C": "шт",
                "name": "Штука",
                "short": "шт",
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название |
| `short` | string | ❌ | Краткое обозначение |
| `active` | bool/string | ❌ | Активность |

---

### 10.3. `setClientCategory` — Create/update client categories

**Request:**
```json
{
    "method": "setClientCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "clientCategory": [
            {
                "code_1C": "000000001",
                "name": "Оптовый",
                "description": "Оптовый покупатель",
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название категории |
| `description` | string | ❌ | Описание |
| `active` | bool/string | ❌ | Активность |

---

### 10.4. `setClientChannel` — Create/update sales channels

**Request:**
```json
{
    "method": "setClientChannel",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "clientChannel": [
            {
                "code_1C": "CH001",
                "name": "Розница"
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название канала сбыта |

---

### 10.5. `setClientType` — Create/update client types

**Request:**
```json
{
    "method": "setClientType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": [
        {
            "SD_id": "",
            "code_1C": "TP001",
            "name": "VIP",
            "color": "#FF0000",
            "active": true
        }
    ]
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название типа клиента |
| `color` | string | ❌ | Цвет (например, `#FF0000`) |
| `active` | bool/string | ❌ | Активность |

> ⚠️ **Внимание:** В этом методе данные передаются напрямую в `data` как массив (не вложенный объект).

---

### 10.6. `setProduct` — Create/update products

**Description:** Создание или обновление товаров. При создании можно автоматически создать категорию товара.

**Request:**
```json
{
    "method": "setProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": [
            {
                "code_1C": "000000015",
                "name": "Кока-Кола 1.5л",
                "active": true,
                "sort": 100,
                "volume": 0.5,
                "packQuantity": 6,
                "barCode": "4780001234567",
                "weight": 1.5,
                "part_number": null,
                "category": {
                    "code_1C": "000000003",
                    "name": "Напитки"
                },
                "unit": {
                    "code_1C": "CODE_000001"
                },
                "subCategory": {
                    "code_1C": "000000001"
                },
                "group": {
                    "code_1C": "GRP001"
                },
                "brand": {
                    "SD_id": "1"
                }
            }
        ]
    }
}
```

> Для вложенных объектов (`unit`, `category`, `subCategory`, `group`, `brand`) можно указывать **CS_id**, **SD_id** или **code_1C**. Примеры для `unit`:
> - по code_1C: `"unit": { "code_1C": "CODE_000001" }`
> - по SD_id: `"unit": { "SD_id": "d0_2" }`
> - по CS_id: `"unit": { "CS_id": "F1-d0_2" }`

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ (один из) | Идентификатор |
| `name` | string | ✅ | Название товара |
| `active` | bool | ❌ | Активность (по умолчанию `N` для новых) |
| `sort` | int | ❌ | Порядок сортировки |
| `volume` | float | ❌ | Объём |
| `packQuantity` | float | ❌ | Количество в упаковке |
| `barCode` | string | ❌ | Штрих-код |
| `weight` | float | ❌ | Вес |
| `part_number` | string | ❌ | Артикул |
| `category` | object | ❌ | Категория (если не найдена — создаётся или присваивается «Другие») |
| `unit` | object | ✅ | Единица измерения (обязательна; можно указать `CS_id` / `SD_id` / `code_1C`) |
| `subCategory` | object | ❌ | Подкатегория |
| `group` | object | ❌ | Группа товара |
| `brand` | object | ❌ | Бренд |

---

### 10.7. `setProductCategory` — Create/update product categories

**Request:**
```json
{
    "method": "setProductCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "productCategory": [
            {
                "code_1C": "000000003",
                "name": "Напитки",
                "active": true,
                "sort": 100,
                "unit": {
                    "code_1C": "CODE_000001"
                }
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название категории |
| `active` | bool/string | ❌ | Активность |
| `sort` | int | ❌ | Порядок сортировки |
| `unit` | object | ❌ | Единица измерения (`code_1C` / `SD_id`) |

---

### 10.8. `setProductSubCategory` — Create/update product subcategories

**Request:**
```json
{
    "method": "setProductSubCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "subCategory": [
            {
                "code_1C": "000000001",
                "name": "Газированные",
                "active": true,
                "sort": 100,
                "unit": {
                    "code_1C": "CODE_000001"
                }
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название подкатегории |
| `active` | bool/string | ❌ | Активность |
| `sort` | int | ❌ | Порядок сортировки |
| `unit` | object | ❌ | Единица измерения (`code_1C` / `SD_id`) |

---

### 10.9. `setProductGroup` — Create/update product groups

**Request:**
```json
{
    "method": "setProductGroup",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "productGroup": [
            {
                "code_1C": "GRP001",
                "name": "Группа А",
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название группы |
| `active` | bool/string | ❌ | Активность |

---

### 10.10. `setPaymentType` — Create/update payment types

**Request:**
```json
{
    "method": "setPaymentType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "paymentType": [
            {
                "code_1C": "000000001",
                "name": "Наличные (сум)",
                "short": "нал",
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название типа оплаты |
| `short` | string | ❌ | Краткое обозначение |
| `active` | bool/string | ❌ | Активность |

---

### 10.11. `setPriceType` — Create/update price types

**Description:** Создание типов цен с привязкой к валюте и типу оплаты. Метод также может создавать валюты.

**Request:**
```json
{
    "method": "setPriceType",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "valyutaType": [
            {
                "code_1C": "UZS",
                "name": "Сум",
                "short": "UZS",
                "active": true
            }
        ],
        "priceType": [
            {
                "code_1C": "000000001",
                "name": "Розничная цена",
                "active": true,
                "forClient": true,
                "paymentType": {
                    "code_1C": "000000001"
                },
                "valyutaType": {
                    "code_1C": "UZS"
                }
            }
        ]
    }
}
```

> Для вложенных объектов (`paymentType`, `valyutaType` и т.п.) также можно использовать **CS_id**, **SD_id** или **code_1C**. Порядок поиска: сначала CS_id (уникален в системе), затем SD_id, затем code_1C (не уникален в системе).

**Поля элемента (valyutaType):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название валюты |
| `short` | string | ❌ | Краткое обозначение |
| `active` | bool/string | ❌ | Активность |

**Поля элемента (priceType):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название типа цены |
| `active` | bool | ❌ | Активность |
| `forClient` | bool | ❌ | Доступен для клиента |
| `paymentType` | object | ✅ | Тип оплаты (валюта расчёта) |
| `valyutaType` | object | ✅ | Тип валюты |


### 10.13. `setPrice` — Set prices

**Description:** Установка или обновление цен товаров для конкретного типа цены.

**Request:**
```json
{
    "method": "setPrice",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": [
            {
                "code_1C": "000000015",
                "document_1C": "DOC001",
                "priceType": {
                    "code_1C": "000000001",
                    "price": 15000.00
                }
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | ID товара |
| `document_1C` | string | ❌ | Код документа 1С |
| `priceType` | object | ✅ | Тип цены + новая цена |
| `priceType.code_1C` / `priceType.SD_id` | string | ✅ | ID типа цены |
| `priceType.price` | float | ✅ | Цена |

---

### 10.15. `setAgent` — Create/update agents

**Description:** Создание или обновление агентов. При создании нового агента автоматически создаётся пользователь (ROLE=4).

**Request:**
```json
{
    "method": "setAgent",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "agent": [
            {
                "code_1C": "000000003",
                "name": "Иванов Иван",
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | ФИО агента |
| `active` | bool/string | ❌ | Активность |

---

### 10.16. `setExpeditor` — Create/update expeditors

**Description:** Создание или обновление экспедиторов. При создании нового — автоматически создаётся пользователь (ROLE=10).

**Request:**
```json
{
    "method": "setExpeditor",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "expeditor": [
            {
                "code_1C": "000000005",
                "name": "Петров Пётр",
                "active": true,
                "address": "ул. Навои, 10",
                "tel": "+998901112233",
                "email": "petrov@mail.com",
                "date_birth": "1990-05-15"
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | ФИО экспедитора |
| `active` | bool/string | ❌ | Активность |
| `address` | string | ❌ | Адрес |
| `tel` | string | ❌ | Телефон |
| `email` | string | ❌ | Email |
| `date_birth` | string | ❌ | Дата рождения |

---

### 10.17. `setClient` — Create/update clients

**Description:** Создание или обновление клиентов. Включает привязку агентов и дней посещения.

**Request:**
```json
{
    "method": "setClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "client": [
            {
                "code_1C": "000000100",
                "shortName": "Магазин Звезда",
                "firmName": "ООО Звезда",
                "address": "ул. Амира Темура, 5",
                "orentir": "Напротив банка",
                "tel": "+998901234567",
                "comment": "Ключевой клиент",
                "inn": "123456789",
                "pinfl": null,
                "nspCode": null,
                "active": true,
                "lat": 41.2995,
                "lon": 69.2401,
                "clientCategory": {
                    "code_1C": "000000001"
                },
                "clientType": {
                    "code_1C": "TP001"
                },
                "clientChannel": {
                    "code_1C": "CH001"
                },
                "territory": {
                    "code_1C": "000000001"
                },
                "priceTypeList": [
                    { "code_1C": "000000001" }
                ],
                "bankDetails": {
                    "bankName": "Асака банк",
                    "accountNumber": "20208000900123456789",
                    "bankCode": "00444"
                },
                "agent": [
                    {
                        "code_1C": "000000003",
                        "visitDays": [1, 3, 5]
                    }
                ],
                "deleteVisits": "false"
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Идентификатор |
| `shortName` | string | ✅ | Краткое имя |
| `firmName` | string | ❌ | Название фирмы |
| `address` | string | ❌ | Адрес |
| `orentir` | string | ❌ | Ориентир |
| `tel` | string | ❌ | Телефон |
| `comment` | string | ❌ | Комментарий |
| `inn` | string | ❌ | ИНН |
| `pinfl` | string | ❌ | ПИНФЛ |
| `nspCode` | string | ❌ | Код НСП |
| `active` | bool | ❌ | Активность |
| `lat` / `lon` | float | ❌ | GPS-координаты |
| `clientCategory` | object | ✅ | Категория клиента (обязательно) |
| `clientType` | object | ❌ | Тип клиента |
| `clientChannel` | object | ❌ | Канал сбыта |
| `territory` | object | ❌ | Территория (город) |
| `priceTypeList` | array | ❌ | Список типов цен для клиента |
| `bankDetails` | object | ❌ | Банковские реквизиты |
| `agent` | array/object | ❌ | Агент(ы) с днями посещения |
| `agent.visitDays` | array | ❌ | Дни посещения [1=Пн, ..., 7=Вс] |
| `deleteVisits` | string | ❌ | `"true"` — удалить старые визиты перед обновлением |

---

### 10.18. `setClientPhone` — Update client phones

**Description:** Установка дополнительных номеров телефона клиента (максимум 2). Все старые номера удаляются.

**Request:**
```json
{
    "method": "setClientPhone",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "client_phone": [
            {
                "code_1C": "000000100",
                "phone_numbers": ["+998901234567", "+998901234568"]
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор клиента |
| `phone_numbers` | array | ✅ | Массив телефонов (макс. 2 элемента) |

---

### 10.19. `setTerritory` — Create/update territories

**Request:**
```json
{
    "method": "setTerritory",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "city": [
            {
                "code_1C": "000000001",
                "name": "Чиланзар",
                "active": true,
                "lat": 41.2995,
                "lng": 69.2401
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название территории (города) |
| `active` | bool/string | ❌ | Активность |
| `lat` / `lng` | float | ❌ | GPS-координаты |

---

### 10.20. `setBrand` — Create/update brands

**Request:**
```json
{
    "method": "setBrand",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "brand": [
            {
                "CS_id": "",
                "SD_id": "",
                "name": "Coca-Cola",
                "sort": 100,
                "active": true
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор записи |
| `name` | string | ✅ | Название бренда |
| `sort` | int | ❌ | Порядок сортировки |
| `active` | bool/string | ❌ | Активность |

---

### 10.21. `setVisit` — Create/update visits

**Description:** Создание или обновление визитов агентов к клиентам. Включает проверку GPS-координат.

**Request:**
```json
{
    "method": "setVisit",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "visit": [
            {
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "date": "2025-06-15 10:00:00",
                "visited": 1,
                "planned": 1,
                "has_photo_report": 0,
                "has_order": 1,
                "start_datetime": "2025-06-15 10:00:00",
                "end_datetime": "2025-06-15 10:30:00",
                "lat": 41.2995,
                "lng": 69.2401
            }
        ]
    }
}
```

**Поля элемента:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `client` | object | ✅ | Клиент (`CS_id` / `SD_id` / `code_1C`) |
| `agent` | object | ✅ | Агент (`CS_id` / `SD_id` / `code_1C`) |
| `date` | string | ✅ | Дата и время визита |
| `visited` | int | ❌ | Фактический визит: `1` — да, `0` — нет |
| `planned` | int | ❌ | Плановый визит: `1` — да, `0` — нет |
| `has_photo_report` | int | ❌ | Наличие фотоотчёта |
| `has_order` | int | ❌ | Наличие заказа |
| `start_datetime` / `end_datetime` | string | ❌ | Время начала/окончания |
| `lat` / `lng` | float | ❌ | GPS-координаты |

---

[← Back to contents](README.md) | [Next: Warehouse and Orders — SET →](08-set-warehouse-orders.md)

