# 📘 GET-методы — Справочники (9.1–9.20)

[← Назад к оглавлению](README.md)

Все GET-методы поддерживают пагинацию (`limit`, `page`), периоды и сортировку.

---

### 9.1. `getValyutaType` — Типы валют

**Описание:** Получение списка типов валют (валюта расчёта).

**Запрос:**
```json
{
    "method": "getValyutaType",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100, "page": 1 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "valyutaType": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Сум",
                "short": "UZS",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 100, "total": 3, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название валюты |
| `short` | string | Краткое обозначение |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |

---

### 9.2. `getUnit` — Единицы измерения

**Описание:** Получение списка единиц измерения товаров.

**Запрос:**
```json
{
    "method": "getUnit",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100, "page": 1 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "unit": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "шт",
                "name": "Штука",
                "short": "шт",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 100, "total": 5, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название единицы измерения |
| `short` | string | Краткое обозначение |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |

---

### 9.3. `getClientCategory` — Категории клиентов

**Описание:** Получение списка категорий клиентов.

**Фильтры:**

| Фильтр | Значения | Описание |
|--------|----------|----------|
| `filter.include` | `sd`, `1c`, `all` | Фильтр по источнику создания |

**Запрос:**
```json
{
    "method": "getClientCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": { "include": "all" }
    }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "clientCategory": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Оптовый",
                "description": "Оптовый покупатель",
                "active": "Y",
                "sort": 500
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 10, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название категории |
| `description` | string | Описание категории |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |
| `sort` | integer | Порядок сортировки |

---

### 9.4. `getProduct` — Товары (Продукты)

**Описание:** Получение списка товаров с полной информацией, включая категорию, подкатегорию, единицу измерения, бренд и группу.

**Фильтры:**

| Фильтр | Описание |
|--------|----------|
| `filter.products.SD_id` | Массив ID продуктов |
| `filter.products.code_1C` | Массив кодов 1С |
| `filter.trade.CS_id` | ID торгового направления |
| `filter.trade.SD_id` | SD_id торгового направления |
| `filter.trade.code_1C` | Код 1С торгового направления |

**Запрос (фильтр по списку товаров):**
```json
{
    "method": "getProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "products": {
                "SD_id": ["d0_10", "d0_11"]
            }
        }
    }
}
```

> Альтернатива для `filter.products`: можно указать **code_1C** (массив, например `["000001", "000002"]`) вместо SD_id.

**Запрос (фильтр по торговому направлению):**
```json
{
    "method": "getProduct",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "trade": {
                "CS_id": "F1-d0_5"
            }
        }
    }
}
```

> Альтернативы для `filter.trade`: можно указать **SD_id** (например `"d0_5"`) или **code_1C** (например `"TD001"`) вместо CS_id.

**Ответ:**
```json
{
    "status": true,
    "result": {
        "product": [
            {
                "CS_id": "d0_15",
                "SD_id": "d0_15",
                "code_1C": "000000015",
                "name": "Кока-Кола 1.5л",
                "active": "Y",
                "sort": 100,
                "volume": 0.5,
                "packQuantity": 6,
                "barCode": "4780001234567",
                "weight": 1.5,
                "imageID": "d0_15_abc123.jpg",
                "imageUrl": "/upload/adtProduct/d0_15_abc123.jpg",
                "thumbUrl": "/upload/adtProduct/thumbs/150x150_d0_15_abc123.jpg",
                "sapCode": null,
                "ikpu": null,
                "part_number": null,
                "category": {
                    "CS_id": "d0_3",
                    "SD_id": "d0_3",
                    "code_1C": "000000003"
                },
                "subCategory": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "unit": {
                    "CS_id": "d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "шт"
                },
                "brand": {
                    "CS_id": "1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "trade": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "group": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "GRP001"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 1500, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название товара |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |
| `sort` | integer | Порядок сортировки |
| `volume` | float | Объём товара |
| `packQuantity` | integer | Количество штук в упаковке |
| `barCode` | string | Штрих-код |
| `weight` | float | Вес товара |
| `imageID` | string | Имя файла изображения |
| `imageUrl` | string | URL полного изображения |
| `thumbUrl` | string | URL миниатюры |
| `sapCode` | string\|null | SAP-код |
| `ikpu` | string\|null | Код ИКПУ |
| `part_number` | string\|null | Артикул |
| **category** | object | Категория товара |
| `category.CS_id` | string | Идентификатор категории |
| `category.SD_id` | string | Серверный ID категории |
| `category.code_1C` | string | Код категории в 1С |
| **subCategory** | object | Подкатегория товара |
| `subCategory.CS_id` | string | Идентификатор подкатегории |
| `subCategory.SD_id` | string | Серверный ID подкатегории |
| `subCategory.code_1C` | string | Код подкатегории в 1С |
| **unit** | object | Единица измерения |
| `unit.CS_id` | string | Идентификатор единицы |
| `unit.SD_id` | string | Серверный ID единицы |
| `unit.code_1C` | string | Код единицы в 1С |
| **brand** | object | Бренд |
| `brand.CS_id` | string | Идентификатор бренда |
| `brand.SD_id` | string | Серверный ID бренда |
| `brand.code_1C` | string\|null | Код бренда в 1С |
| **trade** | object | Торговое направление |
| `trade.CS_id` | string | Идентификатор торгового направления |
| `trade.SD_id` | string | Серверный ID торгового направления |
| `trade.code_1C` | string\|null | Код торгового направления в 1С |
| **group** | object | Группа товаров |
| `group.CS_id` | string | Идентификатор группы |
| `group.SD_id` | string | Серверный ID группы |
| `group.code_1C` | string | Код группы в 1С |

---

### 9.5. `getProductCategory` — Категории товаров

**Описание:** Получение списка категорий товаров.

**Запрос:**
```json
{
    "method": "getProductCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "productCategory": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Напитки",
                "active": "Y",
                "unit": {
                    "CS_id": "d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "шт"
                }
            }
        ]
    },
    "pagination": { "limit": 100, "total": 20, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название категории |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |
| **unit** | object | Единица измерения по умолчанию |
| `unit.CS_id` | string | Идентификатор единицы |
| `unit.SD_id` | string | Серверный ID единицы |
| `unit.code_1C` | string | Код единицы в 1С |

---

### 9.6. `getProductSubCategory` — Подкатегории товаров

**Описание:** Получение списка подкатегорий товаров.

**Запрос:**
```json
{
    "method": "getProductSubCategory",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "subCategory": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Газированные",
                "active": "Y",
                "unit": {
                    "CS_id": "d0_2",
                    "SD_id": "d0_2",
                    "code_1C": "шт"
                }
            }
        ]
    },
    "pagination": { "limit": 100, "total": 15, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название подкатегории |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |
| **unit** | object | Единица измерения по умолчанию |
| `unit.CS_id` | string | Идентификатор единицы |
| `unit.SD_id` | string | Серверный ID единицы |
| `unit.code_1C` | string | Код единицы в 1С |

---

### 9.7. `getProductGroup` — Группы товаров

**Описание:** Получение списка групп товаров.

**Запрос:**
```json
{
    "method": "getProductGroup",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 100 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "productGroup": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "GRP001",
                "name": "Группа А",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 100, "total": 5, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название группы |
| `active` | string | Активность: `Y` — активна, `N` — неактивна |

---

### 9.8. `getPriceType` — Типы цен

**Описание:** Получение списка типов цен с привязанными валютами и типами оплат.

**Запрос:**
```json
{
    "method": "getPriceType",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 50 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "priceType": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Розничная цена (Сум)",
                "active": "Y",
                "type": "sale",
                "valyutaType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "UZS"
                },
                "paymentType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 6, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название типа цены |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |
| `type` | string | Категория типа цены: `sale` — продажа (TYPE=2), `purchase` — закуп (TYPE=1) |
| **valyutaType** | object | Тип валюты |
| `valyutaType.CS_id` | string | Идентификатор валюты |
| `valyutaType.SD_id` | string | Серверный ID валюты |
| `valyutaType.code_1C` | string | Код валюты в 1С |
| **paymentType** | object | Тип оплаты |
| `paymentType.CS_id` | string | Идентификатор типа оплаты |
| `paymentType.SD_id` | string | Серверный ID типа оплаты |
| `paymentType.code_1C` | string | Код типа оплаты в 1С |

---

### 9.9. `getPrice` — Цены товаров

**Описание:** Получение цен всех товаров по указанному типу цены.

**Параметры:**

| Параметр | Тип | Обязательное | Описание |
|----------|-----|:---:|----------|
| `params.priceType.SD_id` | string | ✅* | ID типа цены |
| `params.priceType.code_1C` | string | ✅* | Код 1С типа цены |

> *Должен быть указан хотя бы один из идентификаторов.

**Запрос:**
```json
{
    "method": "getPrice",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "priceType": {
            "CS_id": "F1-d0_1",
            // or use SD_id
            // "SD_id": "d0_1",
            // or use code_1C
            // "code_1C": "000000001"
        }
    }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": [
        {
            "product": {
                "CS_id": "d0_15",
                "SD_id": "d0_15",
                "code_1C": "000000015"
            },
            "price": 15000.00
        }
    ]
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| **product** | object | Объект товара |
| `product.CS_id` | string | Идентификатор товара |
| `product.SD_id` | string | Серверный ID товара |
| `product.code_1C` | string | Код товара в 1С |
| `price` | float | Цена товара |

---

### 9.10. `getClient` — Клиенты

**Описание:** Получение списка клиентов с полной информацией: категория, тип, канал, территория, экспедитор, агенты с днями посещения.

**Фильтры:**

| Фильтр | Описание |
|--------|----------|
| `filter.period.update.from/to` | Период по дате обновления |
| `filter.period.create.from/to` | Период по дате создания |
| `sortBy.name` | Сортировка: `updatedAt` или `createdAt` |
| `sortBy.asc` | `true` — ASC, `false` — DESC |

**Условная фильтрация:**

В блоке `filter` можно указать условия по `CS_id`, `SD_id`, `code_1C` и `active` для выборки конкретных клиентов.

**Примеры использования фильтров:**

**Фильтр по дате обновления (`filter.period.update`):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "page": 1,
        "filter": {
            "period": {
                "update": {
                    "from": "2025-01-01",
                    "to": "2025-12-31"
                }
            }
        },
        "sortBy": {
            "name": "updatedAt",
            "asc": false
        }
    }
}
```

**Фильтр по дате создания (`filter.period.create`):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "period": {
                "create": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        },
        "sortBy": {
            "name": "createdAt",
            "asc": true
        }
    }
}
```

**Сортировка (`sortBy`):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "sortBy": {
            "name": "updatedAt",
            "asc": true
        }
    }
}
```
> `name` — поле сортировки: `updatedAt` или `createdAt`. `asc` — `true` для ASC, `false` для DESC.

**Условная фильтрация (по конкретному клиенту):**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "CS_id": "F1-d0_1"
        }
    }
}
```

> Альтернативы для фильтра по клиенту: можно указать **SD_id** (например `"d0_1"`) или **code_1C** (например `"000000100"`) вместо CS_id.

**Фильтр по активности:**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "active": "Y"
        }
    }
}
```
> `active` — `Y` для активных, `N` для неактивных клиентов.

**Комбинирование фильтров:**
```json
{
    "method": "getClient",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 100,
        "page": 1,
        "filter": {
            "active": "Y",
            "period": {
                "update": {
                    "from": "2025-06-01",
                    "to": "2025-06-30"
                }
            }
        },
        "sortBy": {
            "name": "updatedAt",
            "asc": false
        }
    }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "client": [
            {
                "CS_id": "F1-d0_100",
                "SD_id": "d0_100",
                "code_1C": "000000100",
                "name": "Магазин \"Звезда\"",
                "firmName": "ООО Звезда",
                "address": "ул. Амира Темура, 5",
                "waymark": "Напротив банка",
                "tel": "+998901234567",
                "lon": 69.2401,
                "lat": 41.2995,
                "allowKredit": 1,
                "allowConsig": 0,
                "comment": "Ключевой клиент",
                "photo": null,
                "active": "Y",
                "inn": "123456789",
                "contract": null,
                "updatedAt": "2025-06-15 10:30:00",
                "createdAt": "2024-01-10 08:00:00",
                "nspCode": null,
                "bankDetails": {
                    "bank": "Асака банк",
                    "account": "20208000900123456789",
                    "mfo": "00444"
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
                "expeditor": {
                    "CS_id": "F1-d0_5",
                    "SD_id": "d0_5",
                    "code_1C": "000000005"
                },
                "agents": [
                    {
                        "id": "d0_3",
                        "code": "000000003",
                        "days": [1, 3, 5]
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 1200, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор клиента (с префиксом филиала) |
| `SD_id` | string | Серверный ID клиента |
| `code_1C` | string | Код клиента в 1С |
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
| `photo` | string\|null | Фото клиента |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |
| `inn` | string | ИНН клиента |
| `contract` | string\|null | Номер договора |
| `updatedAt` | string | Дата последнего обновления |
| `createdAt` | string | Дата создания |
| `nspCode` | string\|null | NSP-код |
| **bankDetails** | object | Банковские реквизиты |
| `bankDetails.bank` | string | Название банка |
| `bankDetails.account` | string | Номер счёта |
| `bankDetails.mfo` | string | МФО банка |
| **category** | object | Категория клиента |
| `category.CS_id` | string | Идентификатор категории |
| `category.SD_id` | string | Серверный ID категории |
| `category.code_1C` | string | Код категории в 1С |
| **type** | object | Тип клиента |
| `type.CS_id` | string | Идентификатор типа |
| `type.SD_id` | string | Серверный ID типа |
| `type.code_1C` | string | Код типа в 1С |
| **channel** | object | Канал сбыта |
| `channel.CS_id` | string | Идентификатор канала |
| `channel.SD_id` | string | Серверный ID канала |
| `channel.code_1C` | string | Код канала в 1С |
| **city** | object | Город |
| `city.CS_id` | string | Идентификатор города |
| `city.SD_id` | string | Серверный ID города |
| `city.code_1C` | string | Код города в 1С |
| **expeditor** | object | Экспедитор |
| `expeditor.CS_id` | string | Идентификатор экспедитора |
| `expeditor.SD_id` | string | Серверный ID экспедитора |
| `expeditor.code_1C` | string | Код экспедитора в 1С |
| **agents** | array | Массив прикреплённых агентов |
| `agents[].id` | string | ID агента |
| `agents[].code` | string | Код агента в 1С |
| `agents[].days` | array | Дни посещения (1=Пн, 2=Вт, ..., 7=Вс) |

---

### 9.11. `getAgent` — Агенты (Торговые представители)

**Описание:** Получение списка агентов с привязанными пользователями.

**Запрос:**
```json
{
    "method": "getAgent",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 50 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "agent": [
            {
                "CS_id": "F1-d0_3",
                "SD_id": "d0_3",
                "code_1C": "000000003",
                "name": "Иванов Иван",
                "active": "Y",
                "user": [
                    {
                        "CS_id": "F1-d0_10",
                        "SD_id": "d0_10",
                        "code_1C": null,
                        "login": "tp-3-1700000000"
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 15, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код агента в 1С |
| `name` | string | Имя агента |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |
| **user** | array | Массив привязанных пользователей |
| `user[].CS_id` | string | Идентификатор пользователя |
| `user[].SD_id` | string | Серверный ID пользователя |
| `user[].code_1C` | string\|null | Код пользователя в 1С |
| `user[].login` | string | Логин пользователя |

---

### 9.12. `getExpeditor` — Экспедиторы

**Описание:** Получение списка экспедиторов.

**Запрос:**
```json
{
    "method": "getExpeditor",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": { "limit": 50 }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "expeditor": [
            {
                "CS_id": "F1-d0_5",
                "SD_id": "d0_5",
                "code_1C": "000000005",
                "name": "Петров Пётр",
                "tel": "+998901112233",
                "address": "ул. Навои, 10",
                "active": "Y",
                "user": [
                    {
                        "CS_id": "F1-d0_20",
                        "SD_id": "d0_20",
                        "code_1C": null,
                        "login": "exp-5"
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 8, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код экспедитора в 1С |
| `name` | string | Имя экспедитора |
| `tel` | string | Телефон |
| `address` | string | Адрес |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |
| **user** | array | Массив привязанных пользователей |
| `user[].CS_id` | string | Идентификатор пользователя |
| `user[].SD_id` | string | Серверный ID пользователя |
| `user[].code_1C` | string\|null | Код пользователя в 1С |
| `user[].login` | string | Логин пользователя |

---

### 9.13. `getSupervisor` — Супервайзеры

**Описание:** Получение списка супервайзеров с прикрепленными агентами.

**Запрос:**
```json
{
    "method": "getSupervisor",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "supervisor": [
            {
                "CS_id": "F1-d0_30",
                "SD_id": "d0_30",
                "code_1C": null,
                "name": "Сидоров Алексей",
                "active": "Y",
                "agents": [
                    {
                        "CS_id": "F1-d0_3",
                        "SD_id": "d0_3",
                        "code_1C": "000000003",
                        "name": "Иванов Иван"
                    }
                ]
            }
        ]
    }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string\|null | Код в 1С |
| `name` | string | Имя супервайзера |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |
| **agents** | array | Массив прикреплённых агентов |
| `agents[].CS_id` | string | Идентификатор агента |
| `agents[].SD_id` | string | Серверный ID агента |
| `agents[].code_1C` | string | Код агента в 1С |
| `agents[].name` | string | Имя агента |

---

### 9.14. `getPaymentType` — Типы оплат (Валюты расчёта)

**Описание:** Получение списка типов оплат (валют расчёта).

**Запрос:**
```json
{
    "method": "getPaymentType",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "currency": [
            {
                "CS_id": "d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Наличные (сум)",
                "short": "нал",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 3, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор (с префиксом) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название типа оплаты |
| `short` | string | Краткое обозначение |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |

---

### 9.15. `getPayment` — Оплаты

**Описание:** Получение списка финансовых операций (оплаты, балансы, долги).

**Фильтры:**

| Фильтр | Описание |
|--------|----------|
| `filter.period.date.from/to` | Период по дате транзакции |
| `filter.period.dateUpdate.from/to` | Период по дате обновления |
| `filter.trade` | Фильтр по торговому направлению (`CS_id` / `SD_id`) |
| `filter.paymentType` | Фильтр по типу оплаты (`CS_id` / `SD_id` / `code_1C`) |
| `filter.transactionType` | Тип транзакции (см. таблицу ниже) |
| `filter.CS_id` / `filter.SD_id` / `filter.code_1C` | Фильтр по конкретной оплате |

**Типы транзакций:**

| Код | Описание |
|-----|----------|
| 1 | Заказ |
| 2 | Долг |
| 3 | Оплата |
| 4 | Конверсия |
| 6 | Начальный остаток |
| 7 | Выплата клиенту |
| 8 | Списание долга |
| 9 | Возврат с полки |

**Запрос (фильтр по типу транзакции и периоду по дате):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "transactionType": 3,
            "period": {
                "date": {
                    "from": "2025-01-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Запрос (фильтр по периоду обновления):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
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

**Запрос (фильтр по торговому направлению):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "trade": {
                "CS_id": "F1-1"
            }
        }
    }
}
```

> Альтернативы для `filter.trade`: можно указать **SD_id** (например `"d0_1"`) или **code_1C** (например `"TD001"`) вместо CS_id.

**Запрос (фильтр по типу оплаты):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "paymentType": {
                "code_1C": "000000001"
            }
        }
    }
}
```

> Альтернативы для `filter.paymentType`: можно указать **CS_id** (например `"F1-d0_1"`) или **SD_id** (например `"d0_1"`) вместо code_1C.

**Запрос (комбинирование нескольких фильтров):**
```json
{
    "method": "getPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "transactionType": 3,
            "trade": {
                "SD_id": "1"
            },
            "paymentType": {
                "code_1C": "000000001"
            },
            "period": {
                "date": {
                    "from": "2025-01-01",
                    "to": "2025-06-30"
                }
            }
        }
    }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "payment": [
            {
                "CS_id": "F1-d0_500",
                "SD_id": "d0_500",
                "code_1C": "PAY000001",
                "paymentDate": "2025-03-15 14:30:00",
                "expirationDate": null,
                "amount": 5000000.00,
                "comment": "Оплата за товар",
                "transactionType": 3,
                "paymentType": {
                    "CS_id": "d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
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
                "cashbox": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "trade": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "code_1C": null
                },
                "orders": [
                    {
                        "CS_id": "F1-d0_300",
                        "SD_id": "d0_300",
                        "amount": 5000000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 50, "total": 200, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `paymentDate` | string | Дата и время оплаты |
| `expirationDate` | string\|null | Дата истечения |
| `amount` | float | Сумма оплаты |
| `comment` | string | Комментарий |
| `transactionType` | integer | Тип транзакции (1–9, см. таблицу типов) |
| **paymentType** | object | Тип оплаты |
| `paymentType.CS_id` | string | Идентификатор типа оплаты |
| `paymentType.SD_id` | string | Серверный ID типа оплаты |
| `paymentType.code_1C` | string | Код типа оплаты в 1С |
| **client** | object | Клиент |
| `client.CS_id` | string | Идентификатор клиента |
| `client.SD_id` | string | Серверный ID клиента |
| `client.code_1C` | string | Код клиента в 1С |
| **agent** | object | Агент |
| `agent.CS_id` | string | Идентификатор агента |
| `agent.SD_id` | string | Серверный ID агента |
| `agent.code_1C` | string | Код агента в 1С |
| **cashbox** | object | Касса |
| `cashbox.CS_id` | string | Идентификатор кассы |
| `cashbox.SD_id` | string | Серверный ID кассы |
| `cashbox.code_1C` | string\|null | Код кассы в 1С |
| **trade** | object | Торговое направление |
| `trade.CS_id` | string | Идентификатор торгового направления |
| `trade.SD_id` | string | Серверный ID торгового направления |
| `trade.code_1C` | string\|null | Код торгового направления в 1С |
| **orders** | array | Привязанные заказы |
| `orders[].CS_id` | string | Идентификатор заказа |
| `orders[].SD_id` | string | Серверный ID заказа |
| `orders[].amount` | float | Сумма оплаты по заказу |

#### На сервере контрагентов

На **сервере контрагентов** объект `client` в каждой оплате идентифицирует **контрагента** (в том числе контрагентов без торговых точек), а не отдельную торговую точку.

Дополнительно в каждую оплату добавляется объект `clientPoint`, идентифицирующий торговую точку, по которой была проведена оплата (`CS_id` / `SD_id` / `code_1C`). Если оплата не привязана к конкретной торговой точке (например, отправлена напрямую по контрагенту), поля объекта `clientPoint` возвращаются как `null`.

```json
{
    "CS_id": "F1-d0_500",
    "SD_id": "d0_500",
    "code_1C": "PAY000001",
    "amount": 5000000.00,
    "client": {
        "CS_id": "F1-d0_100",
        "SD_id": "d0_100",
        "code_1C": "CA000001"
    },
    "clientPoint": {
        "CS_id": "F1-d0_205",
        "SD_id": "d0_205",
        "code_1C": "000000100"
    }
}
```

| Поле | Тип | Описание |
|------|-----|----------|
| **clientPoint** | object | Торговая точка, по которой проведена оплата (только на сервере контрагентов) |
| `clientPoint.CS_id` | string\|null | Идентификатор торговой точки |
| `clientPoint.SD_id` | string\|null | Серверный ID торговой точки |
| `clientPoint.code_1C` | string\|null | Код торговой точки в 1С |

На обычном сервере объект `clientPoint` в ответе отсутствует, объект `client` идентифицирует клиента (торговую точку), и поведение метода не меняется.

[Подробнее о контрагентах](10-contragent.md)

---

### 9.16. `getTerritory` — Территории

**Описание:** Получение регионов и городов.

**Запрос:**
```json
{
    "method": "getTerritory",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "region": [
            {
                "CS_id": "F1-d0_13",
                "SD_id": "d0_13",
                "code_1C": "000000013",
                "name": "Ташкент",
                "active": "Y",
                "sort": 500
            }
        ],
        "city": [
            {
                "CS_id": "F1-d0_1",
                "SD_id": "d0_1",
                "code_1C": "000000001",
                "name": "Чиланзар",
                "active": "Y",
                "sort": 100,
                "region": {
                    "CS_id": "F1-d0_13",
                    "SD_id": "d0_13",
                    "code_1C": "000000013"
                }
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 50, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| **region** | array | Массив регионов |
| `region[].CS_id` | string | Идентификатор региона |
| `region[].SD_id` | string | Серверный ID региона |
| `region[].code_1C` | string | Код региона в 1С |
| `region[].name` | string | Название региона |
| `region[].active` | string | Активность: `Y` — активен, `N` — неактивен |
| `region[].sort` | integer | Порядок сортировки |
| **city** | array | Массив городов |
| `city[].CS_id` | string | Идентификатор города |
| `city[].SD_id` | string | Серверный ID города |
| `city[].code_1C` | string | Код города в 1С |
| `city[].name` | string | Название города |
| `city[].active` | string | Активность: `Y` — активен, `N` — неактивен |
| `city[].sort` | integer | Порядок сортировки |
| `city[].region` | object | Родительский регион (`CS_id`, `SD_id`, `code_1C`) |

---

### 9.17. `getBrand` — Бренды

**Описание:** Получение списка брендов.

**Запрос:**
```json
{
    "method": "getBrand",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "brand": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Coca-Cola",
                "sort": 100,
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 10, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `name` | string | Название бренда |
| `sort` | integer | Порядок сортировки |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |

---

### 9.18. `getTradeDirection` — Торговые направления

**Описание:** Получение списка торговых направлений.

**Запрос:**
```json
{
    "method": "getTradeDirection",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "tradeDirection": [
            {
                "CS_id": "1",
                "SD_id": "1",
                "name": "Основное направление",
                "desc": "Описание",
                "active": "Y"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 3, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `name` | string | Название направления |
| `desc` | string | Описание |
| `active` | string | Активность: `Y` — активно, `N` — неактивно |

---

### 9.19. `getClientChannel` — Каналы сбыта

**Описание:** Получение списка каналов сбыта клиентов.

**Запрос:**
```json
{
    "method": "getClientChannel",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": [
        {
            "CS_id": "1",
            "SD_id": "1",
            "code_1C": "CH001",
            "name": "Розница"
        }
    ]
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название канала сбыта |

---

### 9.20. `getClientType` — Типы клиентов

**Описание:** Получение списка типов клиентов.

**Запрос:**
```json
{
    "method": "getClientType",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": [
        {
            "CS_id": "1",
            "SD_id": "1",
            "code_1C": "TP001",
            "name": "VIP",
            "color": "#FF0000",
            "active": "Y"
        }
    ]
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Идентификатор |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код из 1С |
| `name` | string | Название типа клиента |
| `color` | string | Цвет (HEX-формат, например `#FF0000`) |
| `active` | string | Активность: `Y` — активен, `N` — неактивен |

---

[← Назад к оглавлению](README.md) | [Далее: Визиты, Склады, Остатки →](03-get-visits-warehouse.md)

