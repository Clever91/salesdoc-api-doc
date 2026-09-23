# 📘 GET-методы — Финансы (9.29–9.31)

[← Назад к оглавлению](README.md)

---

### 9.29. `getBalance` — Баланс клиентов

**Описание:** Получение текущего баланса клиентов с разбивкой по валютам.

**Запрос:**
```json
{
    "method": "getBalance",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "balance": [
            {
                "CS_id": "F1-d0_100",
                "SD_id": "d0_100",
                "code_1C": "000000100",
                "name": "Магазин Звезда",
                "active": true,
                "balance": -2500000.00,
                "by-currency": [
                    {
                        "currency_id": "d0_1",
                        "amount": -2500000.00
                    }
                ]
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 500, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор клиента (с префиксом филиала) |
| `SD_id` | string | Серверный ID клиента |
| `code_1C` | string | Код клиента в 1С |
| `name` | string | Название клиента |
| `active` | boolean | Активность клиента |
| `balance` | float | Общий баланс клиента (отрицательный = задолженность) |
| **by-currency** | array | Разбивка баланса по валютам |
| `by-currency[].currency_id` | string | ID валюты (типа оплаты) |
| `by-currency[].amount` | float | Сумма баланса в данной валюте |

> **Примечание:** Отрицательный баланс означает задолженность клиента.

---

### 9.30. `getConsumption` — Расход / Приход

**Описание:** Получение списка кассовых операций (расход и приход).

**Фильтры:**

| Фильтр | Описание |
|--------|----------|
| `filter.include` | `sd`, `1c`, `all` — по источнику |
| `filter.type` | `income` — приход, `expense` — расход |
| `filter.paymentType` | По типу оплаты (`CS_id` / `SD_id` / `code_1C`) |
| `filter.date.from` | Начало периода, `YYYY-MM-DD` (включительно) |
| `filter.date.to` | Конец периода, `YYYY-MM-DD` (включительно) |
| `filter.dateType` | Дата, по которой применяется период: `date` — дата операции (по умолчанию), `created_at` — дата создания записи |

**Параметры фильтра по дате:**

| Параметр | Тип | Формат | Описание |
|------|-----|--------|----------|
| `filter.date.from` | string | `YYYY-MM-DD` | Начало периода, включительно (с `00:00:00`). Необязательный. |
| `filter.date.to` | string | `YYYY-MM-DD` | Конец периода, включительно (до `23:59:59`). Необязательный. |
| `filter.dateType` | string | `date` \| `created_at` | К какой дате применяется период. По умолчанию — `date`. |

| `dateType` | Дата | Поле в ответе |
|------------|------|---------------|
| `date` | Дата операции (дата расхода / прихода) | `date` |
| `created_at` | Дата создания записи в SalesDoc | `created_at` |

> Любую из границ можно не указывать: только `from` — «с этой даты», только `to` — «по эту дату».
> Если не указаны ни `from`, ни `to`, фильтр по дате не применяется, а `dateType` игнорируется.
>
> Фильтр работает так же, как фильтры **«Дата расхода»** и **«Дата создания»** в разделе
> **Касса → Расходы** веб-интерфейса SalesDoc: за один и тот же период API возвращает те же записи.
>
> `pagination.total` учитывает все применённые фильтры, включая фильтр по дате.

**Примеры использования фильтров:**

**Фильтр по типу операции и источнику:**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "type": "expense",
            "include": "all"
        }
    }
}
```

**Фильтр по источнику (`filter.include`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "1c"
        }
    }
}
```
> Значения: `sd` — только из SalesDoc, `1c` — только из 1С, `all` — все записи.

**Фильтр только по приходу (`filter.type`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "type": "income"
        }
    }
}
```

**Фильтр по типу оплаты (`filter.paymentType`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "paymentType": {
                "code_1C": "000000001"
            }
        }
    }
}
```

> Альтернативы для `filter.paymentType`: можно указать **CS_id** (например `"F1-d0_1"`) или **SD_id** (например `"d0_1"`) вместо code_1C.

**Фильтр по дате операции (`filter.date`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "date": { "from": "2025-06-01", "to": "2025-06-30" }
        }
    }
}
```
> `dateType` не указан, поэтому период применяется к дате операции (`date`).

**Фильтр по дате создания (`filter.dateType: created_at`):**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "filter": {
            "include": "sd",
            "dateType": "created_at",
            "date": { "from": "2025-06-15" }
        }
    }
}
```
> Возвращает записи SalesDoc, созданные 15.06.2025 и позже, независимо от даты операции.
> Этот режим удобен для регулярной догрузки: операция, введённая сегодня задним числом, всё равно будет выгружена.

**Комбинирование всех фильтров:**
```json
{
    "method": "getConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "limit": 50,
        "filter": {
            "type": "expense",
            "include": "sd",
            "paymentType": {
                "code_1C": "000000001"
            },
            "dateType": "date",
            "date": { "from": "2025-06-01", "to": "2025-06-30" }
        }
    }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "consumption": [
            {
                "CS_id": "F1-d0_50",
                "SD_id": "d0_50",
                "code_1C": "CON000001",
                "summa": 500000.00,
                "date": "2025-06-15",
                "created_at": "2025-06-15 09:00:00",
                "type": "expense",
                "comment": "Канцелярские товары",
                "cashbox": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Основная касса",
                    "code_1C": null
                },
                "paymentType": {
                    "CS_id": "F1-d0_1",
                    "SD_id": "d0_1",
                    "code_1C": "000000001"
                },
                "category_parent": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Административные расходы",
                    "code_1C": "ADM001"
                },
                "category_child": {
                    "CS_id": "F1-1",
                    "SD_id": "1",
                    "name": "Канцелярия",
                    "code_1C": "KAN001"
                }
            }
        ]
    },
    "pagination": { "limit": 50, "total": 30, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string | Код в 1С |
| `summa` | float | Сумма операции |
| `date` | string | Дата операции |
| `created_at` | string | Дата и время создания записи (используется при `filter.dateType: created_at`) |
| `type` | string | Тип операции: `expense` — расход, `income` — приход |
| `comment` | string | Комментарий |
| **cashbox** | object | Касса |
| `cashbox.CS_id` | string | Идентификатор кассы |
| `cashbox.SD_id` | string | Серверный ID кассы |
| `cashbox.name` | string | Название кассы |
| `cashbox.code_1C` | string\|null | Код кассы в 1С |
| **paymentType** | object | Тип оплаты |
| `paymentType.CS_id` | string | Идентификатор типа оплаты |
| `paymentType.SD_id` | string | Серверный ID типа оплаты |
| `paymentType.code_1C` | string | Код типа оплаты в 1С |
| **category_parent** | object | Родительская категория расхода |
| `category_parent.CS_id` | string | Идентификатор |
| `category_parent.SD_id` | string | Серверный ID |
| `category_parent.name` | string | Название |
| `category_parent.code_1C` | string | Код в 1С |
| **category_child** | object | Дочерняя категория расхода |
| `category_child.CS_id` | string | Идентификатор |
| `category_child.SD_id` | string | Серверный ID |
| `category_child.name` | string | Название |
| `category_child.code_1C` | string | Код в 1С |

**Ошибки валидации:**

| `error.message` | Причина |
|-----------------|---------|
| `Invalid date format` | `filter.date.from` или `filter.date.to` не является корректной датой |
| `Invalid date type` | `filter.dateType` имеет значение, отличное от `date` и `created_at` |
| `Invalid filter type` | `filter.type` имеет значение, отличное от `income` и `expense` |

```json
{
    "status": false,
    "result": [],
    "error": {
        "code": 400,
        "message": "Invalid date type",
        "data": []
    }
}
```

---

### 9.31. `getCashbox` — Касса

**Описание:** Получение списка касс.

**Запрос:**
```json
{
    "method": "getCashbox",
    "auth": { "userId": "d0_1", "token": "..." }
}
```

**Ответ:**
```json
{
    "status": true,
    "result": {
        "cashbox": [
            {
                "CS_id": "F1-1",
                "SD_id": "1",
                "code_1C": null,
                "name": "Основная касса",
                "active": true,
                "created_at": "2024-01-01 00:00:00"
            }
        ]
    },
    "pagination": { "limit": 1000, "total": 2, "page": 1 }
}
```

**Поля ответа:**

| Поле | Тип | Описание |
|------|-----|----------|
| `CS_id` | string | Внешний идентификатор (с префиксом филиала) |
| `SD_id` | string | Серверный ID |
| `code_1C` | string\|null | Код кассы в 1С |
| `name` | string | Название кассы |
| `active` | boolean | Активность кассы |
| `created_at` | string | Дата и время создания |

---

[← Назад к оглавлению](README.md) | [Далее: Дополнительные GET-методы →](06-get-extra.md)

