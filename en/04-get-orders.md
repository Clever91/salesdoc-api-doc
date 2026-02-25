# GET methods — Orders (9.26–9.28)

[← Back to contents](README.md)

---

### 9.26. `getOrder` — Orders

**Description:** Получение списка заказов с деталями (товары, цены, количество, бонусы).

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.agent` | `all` — все агенты, иначе только активные |
| `filter.include` | `sd`, `1c`, `all` — фильтр по источнику |
| `filter.period.date.from/to` | По дате заказа |
| `filter.period.dateLoad.from/to` | По дате документа |
| `filter.period.dateCreate.from/to` | По дате создания (с точным временем) |
| `filter.period.orderCreated.from/to` | По дате создания записи |
| `filter.period.dateUpdate.from/to` | По дате обновления (из истории) |
| `filter.status` | Массив статусов (по умолчанию `[1]`) |
| `filter.store` | По складу (`CS_id` / `SD_id` / `code_1C`) |
| `filter.trade` | По торговому направлению (`CS_id` / `SD_id`) |
| `filter.expeditors` | Массив экспедиторов |
| `filter.clientCategories` | Массив категорий клиентов |
| `filter.sort.category` | `asc` / `desc` — сортировка по категориям |
| `params.promotions` | `true` — включить данные об акциях |
| `filter.CS_id` / `filter.SD_id` / `filter.code_1C` | Фильтр по конкретному заказу |
| Фильтр по клиенту | `client.CS_id` / `client.SD_id` / `client.code_1C` |

**Примеры использования фильтров:**

**1. Фильтр по агентам (`filter.agent`):**
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
> Если `agent` = `all`, возвращаются заказы всех агентов. Если не указан — только активные агенты.

**2. Фильтр по источнику (`filter.include`):**
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
> Значения: `sd` — только из SalesDoc, `1c` — только из 1С, `all` — все записи.

**3. Фильтр по дате заказа (`filter.period.date`):**
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

**4. Фильтр по дате документа (`filter.period.dateLoad`):**
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

**5. Фильтр по дате создания (`filter.period.dateCreate`):**
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
> Используется точное время (формат `YYYY-MM-DD HH:MM:SS`).

**6. Фильтр по дате создания записи (`filter.period.orderCreated`):**
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

**7. Фильтр по дате обновления (`filter.period.dateUpdate`):**
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
> Дата берётся из истории изменений заказа.

**8. Фильтр по статусам (`filter.status`):**
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
> Передаётся массив статусов. По умолчанию `[1]` (только новые).  
> Статусы: `1` — новый, `2` — отправлен, `3` — доставлен, `4` — закрыт, `5` — отменён (см. таблицу «Статусы заказов» ниже).

**9. Фильтр по складу (`filter.store`):**
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

> Альтернативы для `filter.store`: можно указать **CS_id** (например `"F1-d0_1"`) или **SD_id** (например `"d0_1"`) вместо code_1C.

**10. Фильтр по торговому направлению (`filter.trade`):**
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

> Альтернативы для `filter.trade`: можно указать **CS_id** (например `"F1-1"`) или **code_1C** (например `"TD001"`) вместо SD_id.

**11. Фильтр по экспедиторам (`filter.expeditors`):**
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
> Массив экспедиторов, каждый может быть указан через `CS_id`, `SD_id` или `code_1C`.

**12. Фильтр по категориям клиентов (`filter.clientCategories`):**
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
> Массив категорий клиентов.

**13. Сортировка по категориям (`filter.sort.category`):**
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
> Значения: `asc` — по возрастанию, `desc` — по убыванию.

**14. Включить данные об акциях (`params.promotions`):**
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
> При `promotions: true` в ответ включаются данные об акциях.

**15. Фильтр по конкретному заказу (`filter.CS_id` / `filter.SD_id` / `filter.code_1C`):**
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

> Альтернативы для фильтра по заказу: можно указать **CS_id** (например `"F1-d0_300"`) или **code_1C** (например `"ORD000001"`) вместо SD_id.

**16. Фильтр по клиенту (`client`):**
```json
{
    "method": "getOrder",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "client": { "code_1C": "000000100" }
    }
}
```
> Можно указать `CS_id`, `SD_id` или `code_1C` клиента.

**17. Комбинирование нескольких фильтров:**
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
> Все фильтры можно комбинировать в одном запросе.

**Статусы заказов:**

| Статус | Описание |
|--------|----------|
| 1 | Новый |
| 2 | Отправлен |
| 3 | Доставлен |
| 4 | Закрыт |
| 5 | Отменён |

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
                "comment": "Срочный заказ",
                "consignment": false,
                "consigDate": null,
                "debtDateExp": "2025-07-15",
                "orderCreated": "2025-06-15 10:00:00",
                "client": {
                    "CS_id": "F1-d0_100",
                    "SD_id": "d0_100",
                    "code_1C": "000000100",
                    "nspCode": null,
                    "clientName": "Магазин Звезда",
                    "clientLegalName": "ООО Звезда"
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
                            "name": "Кока-Кола 1.5л"
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

**Описание полей ответа:**

| Field | Type | Description |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор заказа (с префиксом филиала) |
| `SD_id` | string | Внутренний идентификатор заказа в SalesDoc |
| `code_1C` | string | Код заказа в системе 1С |
| `status` | integer | Статус заказа (1 — новый, 2 — отправлен, 3 — доставлен, 4 — закрыт, 5 — отменён) |
| `dateCreate` | string | Дата и время создания заказа (`YYYY-MM-DD HH:MM:SS`) |
| `dateDocument` | string | Дата документа заказа (`YYYY-MM-DD HH:MM:SS`) |
| `totalSumma` | float | Общая сумма заказа до скидки |
| `discountSumma` | float | Сумма скидки |
| `totalSummaAfterDiscount` | float | Итоговая сумма заказа после скидки |
| `totalReturnsSumma` | float | Сумма возвратов по заказу |
| `totalReturnsCount` | integer | Количество возвратов по заказу |
| `comment` | string | Комментарий к заказу |
| `consignment` | boolean | Признак консигнации |
| `consigDate` | string\|null | Дата консигнации (если применимо) |
| `debtDateExp` | string\|null | Ожидаемая дата погашения задолженности |
| `orderCreated` | string | Дата и время фактического создания записи |
| `invoiceNumber` | string\|null | Номер накладной |
| **client** | object | Объект клиента |
| `client.CS_id` | string | Внешний идентификатор клиента |
| `client.SD_id` | string | Внутренний идентификатор клиента |
| `client.code_1C` | string | Код клиента в 1С |
| `client.nspCode` | string\|null | NSP-код клиента |
| `client.clientName` | string | Название клиента |
| `client.clientLegalName` | string | Юридическое название клиента |
| **agent** | object | Объект агента |
| `agent.CS_id` | string | Внешний идентификатор агента |
| `agent.SD_id` | string | Внутренний идентификатор агента |
| `agent.code_1C` | string | Код агента в 1С |
| **expeditor** | object | Объект экспедитора |
| `expeditor.CS_id` | string | Внешний идентификатор экспедитора |
| `expeditor.SD_id` | string | Внутренний идентификатор экспедитора |
| `expeditor.code_1C` | string | Код экспедитора в 1С |
| **priceType** | object | Объект типа цены |
| `priceType.CS_id` | string | Внешний идентификатор типа цены |
| `priceType.SD_id` | string | Внутренний идентификатор типа цены |
| `priceType.code_1C` | string | Код типа цены в 1С |
| **paymentType** | object | Объект типа оплаты |
| `paymentType.CS_id` | string | Внешний идентификатор типа оплаты |
| `paymentType.SD_id` | string | Внутренний идентификатор типа оплаты |
| `paymentType.code_1C` | string | Код типа оплаты в 1С |
| **note** | object | Объект примечания |
| `note.CS_id` | string\|null | Внешний идентификатор примечания |
| `note.SD_id` | string\|null | Внутренний идентификатор примечания |
| `note.code_1C` | string\|null | Код примечания в 1С |
| `note.title` | string\|null | Текст примечания |
| **store** | object | Объект склада |
| `store.CS_id` | string | Внешний идентификатор склада |
| `store.SD_id` | string | Внутренний идентификатор склада |
| `store.code_1C` | string | Код склада в 1С |
| **trade** | object | Объект торгового направления |
| `trade.CS_id` | string | Внешний идентификатор торгового направления |
| `trade.SD_id` | string | Внутренний идентификатор торгового направления |
| `trade.code_1C` | string\|null | Код торгового направления в 1С |
| **orderProducts** | array | Массив товаров в заказе |
| `orderProducts[].product.CS_id` | string | Внешний идентификатор товара |
| `orderProducts[].product.SD_id` | string | Внутренний идентификатор товара |
| `orderProducts[].product.code_1C` | string | Код товара в 1С |
| `orderProducts[].product.name` | string | Название товара |
| `orderProducts[].quantity` | float | Количество товара |
| `orderProducts[].price` | float | Цена за единицу |
| `orderProducts[].discount` | float | Процент скидки |
| `orderProducts[].summa` | float | Итоговая сумма по позиции (после скидки) |
| `orderProducts[].returned` | float | Количество возвращённого товара |
| `orderProducts[].block` | float | Количество в блоках |
| **bonus** | array | Массив бонусных товаров (структура аналогична `orderProducts`) |

---

### 9.27. `getOrderDefect` — Returns

**Description:** Получение списка возвратных заказов (тип 2).

**Filters:**

| Filter | Description |
|--------|----------|
| `filter.period.date.from/to` | По дате возврата |
| `filter.period.dateLoad.from/to` | По дате документа |
| `filter.period.dateUpdate.from/to` | По дате обновления (из истории) |
| `filter.status` | Массив статусов (по умолчанию `[1]`) |

**Примеры использования фильтров:**

**Фильтр по дате возврата и статусам:**
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

**Фильтр по дате документа (`filter.period.dateLoad`):**
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

**Фильтр по дате обновления (`filter.period.dateUpdate`):**
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
> Дата берётся из истории изменений документа.

**Комбинирование нескольких фильтров:**
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
                "comment": "Брак",
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
                "reason": "Истёк срок годности",
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
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `type` | integer | Тип документа (2 = возврат) |
| `status` | integer | Статус документа (1–5) |
| `date` | string | Дата и время возврата |
| `dateLoad` | string | Дата документа |
| `totalSumma` | float | Общая сумма до скидки |
| `discount` | float | Сумма скидки |
| `summa` | float | Итоговая сумма |
| `comment` | string | Комментарий |
| `reason` | string | Причина возврата |
| **client** | object | Клиент |
| `client.CS_id` | string | Идентификатор клиента |
| `client.SD_id` | string | Серверный ID клиента |
| `client.code_1C` | string | Код клиента в 1С |
| **agent** | object | Агент |
| `agent.CS_id` | string | Идентификатор агента |
| `agent.SD_id` | string | Серверный ID агента |
| `agent.code_1C` | string | Код агента в 1С |
| **expeditor** | object | Экспедитор |
| `expeditor.CS_id` | string | Идентификатор экспедитора |
| `expeditor.SD_id` | string | Серверный ID экспедитора |
| `expeditor.code_1C` | string | Код экспедитора в 1С |
| **priceType** | object | Тип цены |
| `priceType.CS_id` | string | Идентификатор типа цены |
| `priceType.SD_id` | string | Серверный ID типа цены |
| `priceType.code_1C` | string | Код типа цены в 1С |
| **store** | object | Склад возврата |
| `store.CS_id` | string | Идентификатор склада |
| `store.SD_id` | string | Серверный ID склада |
| `store.code_1C` | string | Код склада в 1С |
| **defectProducts** | array | Массив возвращённых товаров |
| `defectProducts[].product` | object | Товар (`CS_id`, `SD_id`, `code_1C`) |
| `defectProducts[].quantity` | float | Количество |
| `defectProducts[].price` | float | Цена за единицу |
| `defectProducts[].summa` | float | Сумма по позиции |
| `defectProducts[].exp_date` | string | Срок годности |

---

### 9.28. `getOrderReplace` — Exchanges

**Description:** Получение списка заказов на обмен (тип 3). Структура аналогична `getOrderDefect`.

**Filters:** Аналогичны `getOrderDefect` (period, status).

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

> Структура ответа аналогична `getOrderDefect` (см. выше), за исключением того, что `type` = `3` (обмен).

---

[← Back to contents](README.md) | [Next: Finance →](05-get-finance.md)

