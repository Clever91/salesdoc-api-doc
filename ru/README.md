# 📘 SalesDoc API V2 — Полная документация

**English:** [Full documentation](../en/README.md)

> **Версия:** 2.0  
> **Базовый URL:** `https://{domain}/api/v2`  
> **Формат данных:** JSON  
> **Метод HTTP:** POST (для всех запросов)  
> **Документация с примерами:** Примеры запросов и ответов доступны в Postman — [Посмотреть примеры](https://documenter.getpostman.com/view/140637/2sA2r3bSJ9)  
> **Журнал интеграции:** Все запросы и ответы API логируются и доступны для просмотра в панели администратора (**Профиль → Журнал интеграции**)

---

## 🔍 Быстрый поиск (для интегратора)

| Задача | Метод(ы) |
|--------|----------|
| Войти в систему | [login](01-general.md#2-аутентификация) |
| Прочитать заказы | [getOrder](04-get-orders.md#926-getorder--заказы), [getOrderDefect](04-get-orders.md#927-getorderdefect--возвраты), [getOrderReplace](04-get-orders.md#928-getorderreplace--обмены) |
| Создать/обновить заказ | [setOrder](08-set-warehouse-orders.md#121-setorder--создатьобновить-заказ), [setOrderDefect](08-set-warehouse-orders.md#125-setorderdefect--создать-возврат), [setCode](08-set-warehouse-orders.md#124-setcode--установить-код-1с-для-документов), [syncOrder](08-set-warehouse-orders.md#126-syncorder--синхронизация-заказов) |
| Справочники (клиенты, товары, цены, единицы и т.д.) | [02-get-references](02-get-references.md) (GET 9.1–9.20), [07-set-references](07-set-references.md) (SET 10.1–10.21) |
| Остатки и склады | [getStock](03-get-visits-warehouse.md#924-getstock--остатки), [getStockForDate](03-get-visits-warehouse.md#925-getstockfordate--остатки-на-дату), [setStock](08-set-warehouse-orders.md#112-setstock--установить-складские-остатки-инвентаризация), [setPurchase](08-set-warehouse-orders.md#113-setpurchase--приход-товаров), [setMovement](08-set-warehouse-orders.md#114-setmovement--перемещение-товаров), [setMovementBetweenFilial](08-set-warehouse-orders.md#117-setmovementbetweenfilial--перемещения-между-филиалами-draft), [setMovementFilialPending](08-set-warehouse-orders.md#118-setmovementfilialpending--отправить-межфилиальное-перемещение-в-pending), [setExcretion](08-set-warehouse-orders.md#1110-setexcretion--списание-товара) |
| Оплаты и баланс | [getPayment](02-get-references.md#915-getpayment--оплаты), [getBalance](05-get-finance.md#929-getbalance--баланс-клиентов), [setPayment](09-finance-photo-extra.md#131-setpayment--создать-оплату), [setBalance](09-finance-photo-extra.md#132-setbalance--установить-начальный-баланс), [setCurrentBalance](09-finance-photo-extra.md#133-setcurrentbalance--установить-текущий-баланс), [setConsumption](09-finance-photo-extra.md#134-setconsumption--создать-расход) |
| Фото товара | [setPhoto](09-finance-photo-extra.md#141-setphoto--загрузить-фото-товара), [getPhoto](09-finance-photo-extra.md#142-getphoto--получить-фото-товара) |
| Филиалы, заявки на клиентов | [getFilials](09-finance-photo-extra.md#151-getfilials--получить-список-филиалов), [getClientPending](09-finance-photo-extra.md#153-getclientpending--заявки-на-клиентов), [deleteClientPending](09-finance-photo-extra.md#154-deleteclientpending--удалить-заявку-на-клиента) |
| Журнал складских операций | [getStoreLog](09-finance-photo-extra.md#152-getstorelog--журнал-складских-операций) (доступ 20:00–07:00) |

---

## 📑 Содержание

### Основы

1. [Общие сведения об API](01-general.md#1-общие-сведения-об-api)
2. [Аутентификация](01-general.md#2-аутентификация)
3. [Структура запроса](01-general.md#3-структура-запроса)
4. [Структура ответа](01-general.md#4-структура-ответа)
5. [Пагинация](01-general.md#5-пагинация)
6. [Фильтрация и период](01-general.md#6-фильтрация-и-период)
7. [Идентификаторы объектов](01-general.md#7-идентификаторы-объектов)
8. [Коды ошибок](01-general.md#8-коды-ошибок)
9. [Ограничения по частоте вызовов и времени доступа](01-general.md#9-ограничения-по-частоте-вызовов-rate-limit-и-времени-доступа)

📄 **Файл:** [01-general.md](01-general.md)

---

### GET-методы (Чтение данных)

**Справочники (9.1–9.20):**


| #    | Метод | Описание             |
| ---- | ----- | -------------------- |
| 9.1  | [`getValyutaType`](02-get-references.md#91-getvalyutatype--типы-валют) | Типы валют           |
| 9.2  | [`getUnit`](02-get-references.md#92-getunit--единицы-измерения) | Единицы измерения    |
| 9.3  | [`getClientCategory`](02-get-references.md#93-getclientcategory--категории-клиентов) | Категории клиентов   |
| 9.4  | [`getProduct`](02-get-references.md#94-getproduct--товары-продукты) | Товары               |
| 9.5  | [`getProductCategory`](02-get-references.md#95-getproductcategory--категории-товаров) | Категории товаров    |
| 9.6  | [`getProductSubCategory`](02-get-references.md#96-getproductsubcategory--подкатегории-товаров) | Подкатегории товаров |
| 9.7  | [`getProductGroup`](02-get-references.md#97-getproductgroup--группы-товаров) | Группы товаров       |
| 9.8  | [`getPriceType`](02-get-references.md#98-getpricetype--типы-цен) | Типы цен             |
| 9.9  | [`getPrice`](02-get-references.md#99-getprice--цены-товаров) | Цены товаров         |
| 9.10 | [`getClient`](02-get-references.md#910-getclient--клиенты) | Клиенты              |
| 9.11 | [`getAgent`](02-get-references.md#911-getagent--агенты-торговые-представители) | Агенты               |
| 9.12 | [`getExpeditor`](02-get-references.md#912-getexpeditor--экспедиторы) | Экспедиторы          |
| 9.13 | [`getSupervisor`](02-get-references.md#913-getsupervisor--супервайзеры) | Супервайзеры         |
| 9.14 | [`getPaymentType`](02-get-references.md#914-getpaymenttype--типы-оплат-валюты-расчёта) | Типы оплат           |
| 9.15 | [`getPayment`](02-get-references.md#915-getpayment--оплаты) | Оплаты               |
| 9.16 | [`getTerritory`](02-get-references.md#916-getterritory--территории) | Территории           |
| 9.17 | [`getBrand`](02-get-references.md#917-getbrand--бренды) | Бренды               |
| 9.18 | [`getTradeDirection`](02-get-references.md#918-gettradedirection--торговые-направления) | Торговые направления |
| 9.19 | [`getClientChannel`](02-get-references.md#919-getclientchannel--каналы-сбыта) | Каналы сбыта         |
| 9.20 | [`getClientType`](02-get-references.md#920-getclienttype--типы-клиентов) | Типы клиентов        |

📄 **Файл:** [02-get-references.md](02-get-references.md)

---

**Визиты, Склады, Остатки (9.21–9.25):**


| #    | Метод | Описание        |
| ---- | ----- | --------------- |
| 9.21 | [`getVisit`](03-get-visits-warehouse.md#921-getvisit--визиты) | Визиты          |
| 9.22 | [`getWarehouse`](03-get-visits-warehouse.md#922-getwarehouse--склады) | Склады          |
| 9.23 | [`getDefectWarehouse`](03-get-visits-warehouse.md#923-getdefectwarehouse--склады-возврата) | Склады возврата |
| 9.24 | [`getStock`](03-get-visits-warehouse.md#924-getstock--остатки) | Остатки         |
| 9.25 | [`getStockForDate`](03-get-visits-warehouse.md#925-getstockfordate--остатки-на-дату) | Остатки на дату |

📄 **Файл:** [03-get-visits-warehouse.md](03-get-visits-warehouse.md)

---

**Заказы (9.26–9.28):**


| #    | Метод | Описание |
| ---- | ----- | -------- |
| 9.26 | [`getOrder`](04-get-orders.md#926-getorder--заказы) | Заказы   |
| 9.27 | [`getOrderDefect`](04-get-orders.md#927-getorderdefect--возвраты) | Возвраты |
| 9.28 | [`getOrderReplace`](04-get-orders.md#928-getorderreplace--обмены) | Обмены   |

📄 **Файл:** [04-get-orders.md](04-get-orders.md)

---

**Финансы (9.29–9.31):**


| #    | Метод | Описание        |
| ---- | ----- | --------------- |
| 9.29 | [`getBalance`](05-get-finance.md#929-getbalance--баланс-клиентов) | Баланс клиентов |
| 9.30 | [`getConsumption`](05-get-finance.md#930-getconsumption--расход--приход) | Расход / Приход |
| 9.31 | [`getCashbox`](05-get-finance.md#931-getcashbox--касса) | Касса           |

📄 **Файл:** [05-get-finance.md](05-get-finance.md)

---

**Дополнительные GET-методы (9.32–9.47):**


| #    | Метод | Описание                         |
| ---- | ----- | -------------------------------- |
| 9.32 | [`getInventory`](06-get-extra.md#932-getinventory--инвентаризация) | Инвентаризация                   |
| 9.33 | [`getShipper`](06-get-extra.md#933-getshipper--поставщики) | Поставщики                       |
| 9.34 | [`getPaymentConfirmation`](06-get-extra.md#934-getpaymentconfirmation--подтверждение-оплат) | Подтверждение оплат              |
| 9.36 | [`getBonus`](06-get-extra.md#936-getbonus--бонусы) | Бонусы                           |
| 9.37 | [`getPRCategory`](06-get-extra.md#937-getprcategory--категории-фотоотчётов) | Категории фотоотчётов            |
| 9.38 | [`getPhotoReport`](06-get-extra.md#938-getphotoreport--фотоотчёты) | Фотоотчёты                       |
| 9.39 | [`getFilials`](06-get-extra.md#939-getfilials--филиалы) | Филиалы                          |
| 9.40 | [`getStoreLog`](06-get-extra.md#940-getstorelog--журнал-складских-операций) | Журнал складских операций        |
| 9.41 | [`getClientPending`](06-get-extra.md#941-getclientpending--заявки-на-клиентов-ожидающие) | Заявки на клиентов               |
| 9.42 | [`getCorrection`](06-get-extra.md#942-getcorrection--корректировки-склада) | Корректировки склада             |
| 9.43 | [`getPurchase`](06-get-extra.md#943-getpurchase--закупки) | Закупки                          |
| 9.44 | [`getMovement`](06-get-extra.md#944-getmovement--перемещения) | Перемещения                      |
| 9.45 | [`getVsExchange`](06-get-extra.md#945-getvsexchange--обмены-на-склад) | Обмены на склад                  |
| 9.46 | [`getMovementBetweenFilial`](06-get-extra.md#946-getmovementbetweenfilial--перемещения-между-филиалами) | Перемещения между филиалами      |
| 9.47 | [`getTag`](06-get-extra.md#947-gettag--теги) | Теги                             |

📄 **Файл:** [06-get-extra.md](06-get-extra.md)

---

### SET-методы (Запись данных)

**Справочники — SET (10.1–10.21):**


| #     | Метод | Описание                            |
| ----- | ----- | ----------------------------------- |
| 10.1  | [`setValyutaType`](07-set-references.md#101-setvalyutatype--создатьобновить-типы-валют) | Создать/обновить валюты             |
| 10.2  | [`setUnit`](07-set-references.md#102-setunit--создатьобновить-единицы-измерения) | Создать/обновить единицы измерения  |
| 10.3  | [`setClientCategory`](07-set-references.md#103-setclientcategory--создатьобновить-категории-клиентов) | Создать/обновить категории клиентов |
| 10.4  | [`setClientChannel`](07-set-references.md#104-setclientchannel--создатьобновить-каналы-сбыта) | Создать/обновить каналы сбыта       |
| 10.5  | [`setClientType`](07-set-references.md#105-setclienttype--создатьобновить-типы-клиентов) | Создать/обновить типы клиентов      |
| 10.6  | [`setProduct`](07-set-references.md#106-setproduct--создатьобновить-товары) | Создать/обновить товары             |
| 10.7  | [`setProductCategory`](07-set-references.md#107-setproductcategory--создатьобновить-категории-товаров) | Создать/обновить категории товаров  |
| 10.8  | [`setProductSubCategory`](07-set-references.md#108-setproductsubcategory--создатьобновить-подкатегории-товаров) | Создать/обновить подкатегории       |
| 10.9  | [`setProductGroup`](07-set-references.md#109-setproductgroup--создатьобновить-группы-товаров) | Создать/обновить группы товаров     |
| 10.10 | [`setPaymentType`](07-set-references.md#1010-setpaymenttype--создатьобновить-типы-оплат) | Создать/обновить типы оплат         |
| 10.11 | [`setPriceType`](07-set-references.md#1011-setpricetype--создатьобновить-типы-цен) | Создать/обновить типы цен           |
| 10.13 | [`setPrice`](07-set-references.md#1013-setprice--установить-цены-товаров) | Установить цены                     |
| 10.15 | [`setAgent`](07-set-references.md#1015-setagent--создатьобновить-агентов) | Создать/обновить агентов            |
| 10.16 | [`setExpeditor`](07-set-references.md#1016-setexpeditor--создатьобновить-экспедиторов) | Создать/обновить экспедиторов       |
| 10.17 | [`setClient`](07-set-references.md#1017-setclient--создатьобновить-клиентов) | Создать/обновить клиентов           |
| 10.18 | [`setClientPhone`](07-set-references.md#1018-setclientphone--обновить-телефоны-клиента) | Обновить телефоны клиента           |
| 10.19 | [`setTerritory`](07-set-references.md#1019-setterritory--создатьобновить-территории) | Создать/обновить территории         |
| 10.20 | [`setBrand`](07-set-references.md#1020-setbrand--создатьобновить-бренды) | Создать/обновить бренды             |
| 10.21 | [`setVisit`](07-set-references.md#1021-setvisit--создатьобновить-визиты) | Создать/обновить визиты             |

📄 **Файл:** [07-set-references.md](07-set-references.md)

---

**Склад и Заказы — SET (11–12):**


| #    | Метод | Описание                            |
| ---- | ----- | ----------------------------------- |
| 11.1 | [`setWarehouse`](08-set-warehouse-orders.md#111-setwarehouse--создатьобновить-склады) | Создать/обновить склады             |
| 11.2 | [`setStock`](08-set-warehouse-orders.md#112-setstock--установить-складские-остатки-инвентаризация) | Установить остатки (инвентаризация) |
| 11.3 | [`setPurchase`](08-set-warehouse-orders.md#113-setpurchase--приход-товаров) | Приход товаров                      |
| 11.4 | [`setMovement`](08-set-warehouse-orders.md#114-setmovement--перемещение-товаров) | Перемещение товаров                 |
| 11.5 | [`setVsExchange`](08-set-warehouse-orders.md#115-setvsexchange--обмен-товаров-vansel) | Обмен товаров                       |
| 11.6 | [`setMovingToFilial`](08-set-warehouse-orders.md#116-setmovingtofilial--перемещение-в-филиал) | Перемещение в филиал                |
| 11.7 | [`setMovementBetweenFilial`](08-set-warehouse-orders.md#117-setmovementbetweenfilial--перемещения-между-филиалами-draft) | Перемещения между филиалами (draft) |
| 11.8 | [`setMovementFilialPending`](08-set-warehouse-orders.md#118-setmovementfilialpending--отправить-межфилиальное-перемещение-в-pending) | Отправить межфилиальное перемещение в pending |
| 11.9 | [`setSupplierReturn`](08-set-warehouse-orders.md#119-setsupplierreturn--возврат-поставщику) | Возврат поставщику                  |
| 11.10 | [`setExcretion`](08-set-warehouse-orders.md#1110-setexcretion--списание-товара) | Списание товара                    |
| 11.11 | [`setCorrection`](08-set-warehouse-orders.md#1111-setcorrection--корректировка-остатков) | Корректировка остатков             |
| 12.1 | [`setOrder`](08-set-warehouse-orders.md#121-setorder--создатьобновить-заказ) | Создать/обновить заказ              |
| 12.2 | [`setDeletedOrder`](08-set-warehouse-orders.md#122-setdeletedorder--отменить-заказ) | Отменить заказ                      |
| 12.3 | [`setStatus`](08-set-warehouse-orders.md#123-setstatus--изменить-статус-заказа) | Изменить статус заказа              |
| 12.4 | [`setCode`](08-set-warehouse-orders.md#124-setcode--установить-код-1с-для-документов) | Установить код 1С для документов    |
| 12.5 | [`setOrderDefect`](08-set-warehouse-orders.md#125-setorderdefect--создать-возврат) | Создать возврат                     |
| 12.6 | [`syncOrder`](08-set-warehouse-orders.md#126-syncorder--синхронизация-заказов) | Синхронизация заказов               |

📄 **Файл:** [08-set-warehouse-orders.md](08-set-warehouse-orders.md)

---

**Касса, Фото, Дополнительные (13–16):**


| #    | Метод | Описание                    |
| ---- | ----- | --------------------------- |
| 13.1 | [`setPayment`](09-finance-photo-extra.md#131-setpayment--создать-оплату) | Создать оплату              |
| 13.2 | [`setBalance`](09-finance-photo-extra.md#132-setbalance--установить-начальный-баланс) | Установить начальный баланс |
| 13.3 | [`setCurrentBalance`](09-finance-photo-extra.md#133-setcurrentbalance--установить-текущий-баланс) | Установить текущий баланс   |
| 13.4 | [`setConsumption`](09-finance-photo-extra.md#134-setconsumption--создать-расход) | Создать расход              |
| 14.1 | [`setPhoto`](09-finance-photo-extra.md#141-setphoto--загрузить-фото-товара) | Загрузить фото товара       |
| 14.2 | [`getPhoto`](09-finance-photo-extra.md#142-getphoto--получить-фото-товара) | Получить фото товара        |
| 15.1 | [`getFilials`](09-finance-photo-extra.md#151-getfilials--получить-список-филиалов) | Список филиалов             |
| 15.2 | [`getStoreLog`](09-finance-photo-extra.md#152-getstorelog--журнал-складских-операций) | Журнал складских операций   |
| 15.3 | [`getClientPending`](09-finance-photo-extra.md#153-getclientpending--заявки-на-клиентов) | Заявки на клиентов          |
| 15.4 | [`deleteClientPending`](09-finance-photo-extra.md#154-deleteclientpending--удалить-заявку-на-клиента) | Удалить заявку на клиента   |

📄 **Файл:** [09-finance-photo-extra.md](09-finance-photo-extra.md)

---
