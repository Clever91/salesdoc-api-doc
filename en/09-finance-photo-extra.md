# Finance, Photo, Extra methods, Limits (13–16)

[← Back to contents](README.md)

---

## 13. Cash and payments

### 13.1. `setPayment` — Create payment

**Description:** Создание оплаты клиента. Можно привязать к конкретному заказу.

**Request:**
```json
{
    "method": "setPayment",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "payment": [
            {
                "code_1C": "PAY000001",
                "amount": 5000000,
                "paymentDate": "2025-06-15 14:30:00",
                "expirationDate": null,
                "comment": "Оплата за товар",
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "paymentType": { "code_1C": "000000001" },
                "trade": { "SD_id": "1" },
                "order": { "SD_id": "d0_300" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `code_1C` / `SD_id` | string | ✅ | Идентификатор |
| `amount` | float | ✅ | Сумма (>0) |
| `paymentDate` | string | ✅ | Дата оплаты |
| `expirationDate` | string | ❌ | Дата истечения |
| `comment` | string | ❌ | Комментарий |
| `client` | object | ✅ | Клиент |
| `agent` | object | ❌ | Агент |
| `paymentType` | object | ✅ | Тип оплаты |
| `trade` | object | ❌ | Торговое направление |
| `order` | object | ❌ | Заказ для привязки (закрытие долга) |

---

### 13.2. `setBalance` — Set opening balance

**Description:** Создание или обновление начального баланса клиента (транзакция типа «Начальный остаток»). Для идентификации записи можно использовать CS_id / SD_id / code_1C.

**Request:**
```json
{
    "method": "setBalance",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "balance": [
            {
                "code_1C": "BAL000001",
                "amount": 1000000,
                "paymentDate": "2025-01-01 00:00:00",
                "comment": "Начальный баланс",
                "client": { "code_1C": "000000100" },
                "agent": { "code_1C": "000000003" },
                "paymentType": { "code_1C": "000000001" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (один из для обновления) | Идентификатор записи баланса |
| `amount` | float | ✅ | Сумма |
| `paymentDate` | string | ✅ | Дата (Y-m-d H:i:s) |
| `comment` | string | ❌ | Комментарий |
| `client` | object | ✅ | Клиент. CS_id / SD_id / code_1C |
| `agent` | object | ❌ | Агент. CS_id / SD_id / code_1C |
| `paymentType` | object | ✅ | Тип оплаты (валюта). CS_id / SD_id / code_1C |

**Response:** Стандартный ответ SET-метода: `completed`, `error`, `data.balance` — массив объектов с `CS_id`, `SD_id`, `code_1C` по каждой успешно обработанной записи.

---

### 13.3. `setCurrentBalance` — Set current balance

**Description:** Синхронизация текущего баланса клиента. Рассчитывает разницу между переданным и существующим балансом и создаёт корректирующую транзакцию. Если разница равна 0, запись не создаётся.

> ⚠️ **Rate limit:** Максимум 3 запроса за 5 минут.

**Request:**
```json
{
    "method": "setCurrentBalance",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "currentBalance": [
            {
                "code_1C": "000000100",
                "balance": 500000,
                "paymentType": { "code_1C": "000000001" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор клиента |
| `balance` | float | ✅ | Целевой текущий баланс (число) |
| `paymentType` | object | ✅ | Тип оплаты (валюта). CS_id / SD_id / code_1C |

**Response:** `completed`, `error`, `data.currentBalance` — массив объектов с `CS_id`, `SD_id`, `code_1C` по каждому обработанному клиенту.

---

### 13.4. `setConsumption` — Create expense

**Description:** Создание записи о расходе или приходе из кассы (потребление). Категория расхода задаётся полем `name`; при отсутствии создаётся новая подкатегория «Потребление от интеграции».

**Request:**
```json
{
    "method": "setConsumption",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "consumption": [
            {
                "name": "Расход на транспорт",
                "summa": 500000,
                "date": "2025-06-15 16:00:00",
                "comment": "Доставка",
                "type": "debt",
                "paymentType": { "code_1C": "000000001" },
                "cashbox": { "SD_id": "d0_1" }
            }
        ]
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ❌ (один из для обновления) | Идентификатор записи потребления |
| `name` | string | ✅ | Название расхода/прихода (категория) |
| `summa` | float | ✅ | Сумма (положительное число) |
| `date` | string | ❌ | Дата (Y-m-d H:i:s), по умолчанию текущая |
| `comment` | string | ❌ | Комментарий |
| `type` | string | ❌ | `"income"` — приход, иначе расход (debt) |
| `paymentType` | object | ✅ | Тип оплаты (валюта). CS_id / SD_id / code_1C |
| `cashbox` | object | ✅ | Касса. CS_id / SD_id / code_1C |

**Response:** `completed`, `error`, `data.consumption` — массив с `CS_id`, `SD_id`, `code_1C` по каждой созданной записи.

---

## 14. Фото

### 14.1. `setPhoto` — Upload product photo

**Description:** Загрузка или обновление фотографии товара. Поддерживает передачу изображения в формате base64.

**Запрос (с base64 данными):**
```json
{
    "method": "setPhoto",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": { "code_1C": "000000015" },
        "imageData": "/9j/4AAQSkZJRgABAQ..."
    }
}
```

**Запрос (с ID изображения):**
```json
{
    "method": "setPhoto",
    "auth": { "userId": "d0_1", "token": "..." },
    "data": {
        "product": { "code_1C": "000000015" },
        "imageId": "d0_15_abc123.jpg"
    }
}
```

**Fields:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `product` | object | ✅ | Товар. CS_id / SD_id / code_1C |
| `imageData` | string | ✅* | Изображение в base64 |
| `imageId` | string | ✅* | Идентификатор уже загруженного файла (для обновления/привязки) |

> *Нужно указать одно из полей: `imageData` или `imageId`.

**Ограничения:**
- Максимальный размер: 100KB
- Поддерживаемые форматы: JPEG, PNG, GIF

**Response:**
```json
{
    "status": true,
    "result": {
        "product": {
            "SD_id": "d0_15",
            "code_1C": "000000015"
        },
        "imageId": "d0_15_abc123.jpg"
    }
}
```

---

### 14.2. `getPhoto` — Get product photo

**Description:** Получение изображения товара по ID. Возвращает бинарные данные изображения.

**Request:**
```json
{
    "method": "getPhoto",
    "auth": { "userId": "d0_1", "token": "..." },
    "params": {
        "imageID": "d0_15_abc123.jpg"
    }
}
```

**Request fields (params):**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `imageID` | string | ✅ | Идентификатор изображения (имя файла, например из ответа setPhoto) |

**Response:** Бинарные данные изображения с заголовком `Content-Type: image/jpeg` (или другой MIME-тип). Пагинации нет.

---

## 15. Дополнительные методы

### 15.1. `getFilials` — List filials

**Description:** Получение списка всех доступных филиалов (название, домен, префикс, filial_id для запросов с филиалом).

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
                "name": "Главный офис",
                "domain": "app.salesdoc.io",
                "prefix": "F1",
                "filial_id": "1"
            }
        ]
    }
}
```

**Поля ответа (filials[]):**

| Поле | Тип | Описание |
|------|-----|----------|
| `name` | string | Название филиала |
| `domain` | string | Домен филиала |
| `prefix` | string | Префикс (для CS_id) |
| `filial_id` | string | Код филиала (передавать в `filial.filial_id`) |

---

### 15.2. `getStoreLog` — Store operations log

**Description:** Получение журнала складских операций за период. Все идентификаторы только в формате SD_id.

> ⚠️ **Ограничения:** Rate limit 5 запросов за 5 секунд. Метод доступен **только с 20:00 до 07:00** по времени сервера.

**Параметры:** `params.storeId` (SD_id склада), `params.from`, `params.to` (даты ISO-8601). Опционально: `params.documents` (массив типов: Purchase, Order, OrderDefect, OrderReplace, Exchange, Excretion, StoreCorrector, BonusOrder), `params.products` (массив SD_id товаров).

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

**Response:** Массив записей в `result`: `product_id`, `document`, `document_id`, `date`, `quantity` (положительное — приход, отрицательное — расход). Подробнее см. раздел 9.40 в [06-get-extra.md](06-get-extra.md#940-getstorelog--журнал-складских-операций).

---

### 15.3. `getClientPending` — Client requests

**Description:** Получение списка заявок на создание/обновление клиентов, ожидающих модерации. Поддерживаются пагинация (`params.limit`, `params.page`).

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

**Response:** `result.clientPending` — массив заявок (поля: CS_id, SD_id, code_1C, name, firmName, address, tel, category, type, channel, city, createdBy, createdAt и др.). В ответе есть `pagination`. Подробная структура — в [06-get-extra.md](06-get-extra.md#941-getclientpending--заявки-на-клиентов-ожидающие).

---

### 15.4. `deleteClientPending` — Delete client request

**Description:** Удаление одной или нескольких заявок на клиента из списка ожидающих. В теле передаётся массив `clientPending` (не внутри `data`).

**Request:**
```json
{
    "method": "deleteClientPending",
    "auth": { "userId": "d0_1", "token": "..." },
    "clientPending": [
        { "SD_id": "d0_500" },
        { "code_1C": "PEND002" }
    ]
}
```

**Поля элемента в clientPending:**

| Field | Type | Required | Description |
|------|-----|:---:|----------|
| `CS_id` / `SD_id` / `code_1C` | string | ✅ (один из) | Идентификатор заявки на клиента |

**Response:**
```json
{
    "status": true,
    "result": {
        "completed": 2,
        "error": 0,
        "data": [
            { "SD_id": "d0_500" },
            { "code_1C": "PEND002" }
        ]
    }
}
```

---

## 16. Лимиты и ограничения

### Лимиты по умолчанию

| Параметр | Значение | Описание |
|----------|:--------:|----------|
| Лимит ответа | 1000 | Максимальное количество записей в одном ответе |
| Лимит запроса | 250 | Максимальное количество записей в одном SET-запросе |

> Лимиты могут быть изменены через серверные параметры.

### Rate Limiting (Ограничение частоты)

Некоторые методы имеют ограничения по частоте вызовов:

| Метод | Интервал | Макс. запросов |
|-------|:--------:|:--------------:|
| `setStock` | 5 сек | 5 |
| `setCurrentBalance` | 5 мин | 3 |

При превышении лимита запрос будет отклонён.

### Требования к данным

- **JSON формат:** Запрос должен быть валидным JSON (кодировка UTF-8)
- **BOM:** Автоматически удаляется UTF-8 BOM если присутствует
- **Обязательные поля:** `method` — всегда обязательно
- **Пустой запрос:** Возвращает ошибку 400
- **Невалидный JSON:** Возвращает ошибку 400
- **Значение active:** Принимает: `true`, `false`, `"Y"`, `"N"`, `"Истина"`, `1`, `0`

---

[← Back to contents](README.md)

