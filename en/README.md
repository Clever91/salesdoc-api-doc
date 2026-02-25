# SalesDoc API V2 — Full documentation

**Русский:** [Полная документация](../ru/README.md)

> **Version:** 2.0  
> **Base URL:** `https://{domain}/api/v2`  
> **Data format:** JSON  
> **HTTP method:** POST (for all requests)  
> **Examples:** Request/response examples in Postman — [View examples](https://documenter.getpostman.com/view/140637/2sA2r3bSJ9)  
> **Integration log:** All API requests and responses are logged and viewable in the admin panel (**Profile → Integration log**)

---

## Quick find (for integrators)

| Task | Method(s) |
|------|-----------|
| Log in | [login](01-general.md#2-authentication) |
| Read orders | [getOrder](04-get-orders.md#926-getorder--orders), [getOrderDefect](04-get-orders.md#927-getorderdefect--returns), [getOrderReplace](04-get-orders.md#928-getorderreplace--exchanges) |
| Create/update order | [setOrder](08-set-warehouse-orders.md#121-setorder--createupdate-order), [setOrderDefect](08-set-warehouse-orders.md#125-setorderdefect--create-return), [setCode](08-set-warehouse-orders.md#124-setcode--set-1c-code-for-documents), [syncOrder](08-set-warehouse-orders.md#126-syncorder--order-sync) |
| References (clients, products, prices, units, etc.) | [02-get-references](02-get-references.md) (GET 9.1–9.20), [07-set-references](07-set-references.md) (SET 10.1–10.21) |
| Stock and warehouses | [getStock](03-get-visits-warehouse.md#924-getstock--stock), [getStockForDate](03-get-visits-warehouse.md#925-getstockfordate--stock-by-date), [setStock](08-set-warehouse-orders.md#112-setstock--set-warehouse-stock-inventory), [setPurchase](08-set-warehouse-orders.md#113-setpurchase--goods-receipt), [setMovement](08-set-warehouse-orders.md#114-setmovement--transfer), [setExcretion](08-set-warehouse-orders.md#118-setexcretion--write-off) |
| Payments and balance | [getPayment](02-get-references.md#915-getpayment--payments), [getBalance](05-get-finance.md#929-getbalance--client-balances), [setPayment](09-finance-photo-extra.md#131-setpayment--create-payment), [setBalance](09-finance-photo-extra.md#132-setbalance--set-opening-balance), [setCurrentBalance](09-finance-photo-extra.md#133-setcurrentbalance--set-current-balance), [setConsumption](09-finance-photo-extra.md#134-setconsumption--create-expense) |
| Product photo | [setPhoto](09-finance-photo-extra.md#141-setphoto--upload-product-photo), [getPhoto](09-finance-photo-extra.md#142-getphoto--get-product-photo) |
| Filials, client requests | [getFilials](09-finance-photo-extra.md#151-getfilials--list-filials), [getClientPending](09-finance-photo-extra.md#153-getclientpending--client-requests), [deleteClientPending](09-finance-photo-extra.md#154-deleteclientpending--delete-client-request) |
| Store operations log | [getStoreLog](09-finance-photo-extra.md#152-getstorelog--store-operations-log) (available 20:00–07:00) |

---

## Contents

### Basics

1. [API overview](01-general.md#1-api-overview)
2. [Authentication](01-general.md#2-authentication)
3. [Request structure](01-general.md#3-request-structure)
4. [Response structure](01-general.md#4-response-structure)
5. [Pagination](01-general.md#5-pagination)
6. [Filtering and period](01-general.md#6-filtering-and-period)
7. [Object identifiers](01-general.md#7-object-identifiers)
8. [Error codes](01-general.md#8-error-codes)
9. [Rate limits and access time](01-general.md#9-rate-limits-and-access-time-windows)

**File:** [01-general.md](01-general.md)

---

### GET methods (read)

**References (9.1–9.20):**

| #    | Method | Description        |
| ---- | ------ | ------------------- |
| 9.1  | [`getValyutaType`](02-get-references.md#91-getvalyutatype--currency-types) | Currency types      |
| 9.2  | [`getUnit`](02-get-references.md#92-getunit--units-of-measure) | Units of measure    |
| 9.3  | [`getClientCategory`](02-get-references.md#93-getclientcategory--client-categories) | Client categories   |
| 9.4  | [`getProduct`](02-get-references.md#94-getproduct--products) | Products            |
| 9.5  | [`getProductCategory`](02-get-references.md#95-getproductcategory--product-categories) | Product categories  |
| 9.6  | [`getProductSubCategory`](02-get-references.md#96-getproductsubcategory--product-subcategories) | Product subcategories |
| 9.7  | [`getProductGroup`](02-get-references.md#97-getproductgroup--product-groups) | Product groups      |
| 9.8  | [`getPriceType`](02-get-references.md#98-getpricetype--price-types) | Price types         |
| 9.9  | [`getPrice`](02-get-references.md#99-getprice--prices) | Prices              |
| 9.10 | [`getClient`](02-get-references.md#910-getclient--clients) | Clients             |
| 9.11 | [`getAgent`](02-get-references.md#911-getagent--agents) | Agents              |
| 9.12 | [`getExpeditor`](02-get-references.md#912-getexpeditor--expeditors) | Expeditors          |
| 9.13 | [`getSupervisor`](02-get-references.md#913-getsupervisor--supervisors) | Supervisors         |
| 9.14 | [`getPaymentType`](02-get-references.md#914-getpaymenttype--payment-types) | Payment types       |
| 9.15 | [`getPayment`](02-get-references.md#915-getpayment--payments) | Payments            |
| 9.16 | [`getTerritory`](02-get-references.md#916-getterritory--territories) | Territories         |
| 9.17 | [`getBrand`](02-get-references.md#917-getbrand--brands) | Brands              |
| 9.18 | [`getTradeDirection`](02-get-references.md#918-gettradedirection--trade-directions) | Trade directions    |
| 9.19 | [`getClientChannel`](02-get-references.md#919-getclientchannel--sales-channels) | Sales channels       |
| 9.20 | [`getClientType`](02-get-references.md#920-getclienttype--client-types) | Client types         |

**File:** [02-get-references.md](02-get-references.md)

---

**Visits, Warehouses, Stock (9.21–9.25):**

| #    | Method | Description     |
| ---- | ------ | --------------- |
| 9.21 | [`getVisit`](03-get-visits-warehouse.md#921-getvisit--visits) | Visits           |
| 9.22 | [`getWarehouse`](03-get-visits-warehouse.md#922-getwarehouse--warehouses) | Warehouses       |
| 9.23 | [`getDefectWarehouse`](03-get-visits-warehouse.md#923-getdefectwarehouse--return-warehouses) | Return warehouses |
| 9.24 | [`getStock`](03-get-visits-warehouse.md#924-getstock--stock) | Stock            |
| 9.25 | [`getStockForDate`](03-get-visits-warehouse.md#925-getstockfordate--stock-by-date) | Stock by date    |

**File:** [03-get-visits-warehouse.md](03-get-visits-warehouse.md)

---

**Orders (9.26–9.28):**

| #    | Method | Description |
| ---- | ------ | ------------ |
| 9.26 | [`getOrder`](04-get-orders.md#926-getorder--orders) | Orders   |
| 9.27 | [`getOrderDefect`](04-get-orders.md#927-getorderdefect--returns) | Returns  |
| 9.28 | [`getOrderReplace`](04-get-orders.md#928-getorderreplace--exchanges) | Exchanges |

**File:** [04-get-orders.md](04-get-orders.md)

---

**Finance (9.29–9.31):**

| #    | Method | Description     |
| ---- | ------ | ---------------- |
| 9.29 | [`getBalance`](05-get-finance.md#929-getbalance--client-balances) | Client balances |
| 9.30 | [`getConsumption`](05-get-finance.md#930-getconsumption--expense-income) | Expense / Income |
| 9.31 | [`getCashbox`](05-get-finance.md#931-getcashbox--cashbox) | Cashbox          |

**File:** [05-get-finance.md](05-get-finance.md)

---

**Extra GET methods (9.32–9.46):**

| #    | Method | Description                |
| ---- | ------ | -------------------------- |
| 9.32 | [`getInventory`](06-get-extra.md#932-getinventory--inventory) | Inventory           |
| 9.33 | [`getShipper`](06-get-extra.md#933-getshipper--suppliers) | Suppliers           |
| 9.34 | [`getPaymentConfirmation`](06-get-extra.md#934-getpaymentconfirmation--payment-confirmation) | Payment confirmation |
| 9.36 | [`getBonus`](06-get-extra.md#936-getbonus--bonuses) | Bonuses             |
| 9.37 | [`getPRCategory`](06-get-extra.md#937-getprcategory--photo-report-categories) | Photo report categories |
| 9.38 | [`getPhotoReport`](06-get-extra.md#938-getphotoreport--photo-reports) | Photo reports      |
| 9.39 | [`getFilials`](06-get-extra.md#939-getfilials--filials) | Filials             |
| 9.40 | [`getStoreLog`](06-get-extra.md#940-getstorelog--store-operations-log) | Store operations log |
| 9.41 | [`getClientPending`](06-get-extra.md#941-getclientpending--client-requests-pending) | Client requests     |
| 9.42 | [`getCorrection`](06-get-extra.md#942-getcorrection--store-corrections) | Store corrections   |
| 9.43 | [`getPurchase`](06-get-extra.md#943-getpurchase--purchases) | Purchases           |
| 9.44 | [`getMovement`](06-get-extra.md#944-getmovement--movements) | Movements           |
| 9.45 | [`getVsExchange`](06-get-extra.md#945-getvsexchange--warehouse-exchanges) | Warehouse exchanges |
| 9.46 | [`getMovementBetweenFilial`](06-get-extra.md#946-getmovementbetweenfilial--movements-between-filials) | Movements between filials |

**File:** [06-get-extra.md](06-get-extra.md)

---

### SET methods (write)

**References — SET (10.1–10.21):**

| #     | Method | Description                    |
| ----- | ------ | ------------------------------ |
| 10.1  | [`setValyutaType`](07-set-references.md#101-setvalyutatype--createupdate-currency-types) | Create/update currency types   |
| 10.2  | [`setUnit`](07-set-references.md#102-setunit--createupdate-units) | Create/update units            |
| 10.3  | [`setClientCategory`](07-set-references.md#103-setclientcategory--createupdate-client-categories) | Create/update client categories |
| 10.4  | [`setClientChannel`](07-set-references.md#104-setclientchannel--createupdate-sales-channels) | Create/update sales channels   |
| 10.5  | [`setClientType`](07-set-references.md#105-setclienttype--createupdate-client-types) | Create/update client types     |
| 10.6  | [`setProduct`](07-set-references.md#106-setproduct--createupdate-products) | Create/update products         |
| 10.7  | [`setProductCategory`](07-set-references.md#107-setproductcategory--createupdate-product-categories) | Create/update product categories |
| 10.8  | [`setProductSubCategory`](07-set-references.md#108-setproductsubcategory--createupdate-subcategories) | Create/update subcategories    |
| 10.9  | [`setProductGroup`](07-set-references.md#109-setproductgroup--createupdate-product-groups) | Create/update product groups   |
| 10.10 | [`setPaymentType`](07-set-references.md#1010-setpaymenttype--createupdate-payment-types) | Create/update payment types    |
| 10.11 | [`setPriceType`](07-set-references.md#1011-setpricetype--createupdate-price-types) | Create/update price types      |
| 10.13 | [`setPrice`](07-set-references.md#1013-setprice--set-prices) | Set prices                      |
| 10.15 | [`setAgent`](07-set-references.md#1015-setagent--createupdate-agents) | Create/update agents           |
| 10.16 | [`setExpeditor`](07-set-references.md#1016-setexpeditor--createupdate-expeditors) | Create/update expeditors        |
| 10.17 | [`setClient`](07-set-references.md#1017-setclient--createupdate-clients) | Create/update clients           |
| 10.18 | [`setClientPhone`](07-set-references.md#1018-setclientphone--update-client-phones) | Update client phones            |
| 10.19 | [`setTerritory`](07-set-references.md#1019-setterritory--createupdate-territories) | Create/update territories       |
| 10.20 | [`setBrand`](07-set-references.md#1020-setbrand--createupdate-brands) | Create/update brands            |
| 10.21 | [`setVisit`](07-set-references.md#1021-setvisit--createupdate-visits) | Create/update visits            |

**File:** [07-set-references.md](07-set-references.md)

---

**Warehouse and Orders — SET (11–12):**

| #    | Method | Description                         |
| ---- | ------ | ----------------------------------- |
| 11.1 | [`setWarehouse`](08-set-warehouse-orders.md#111-setwarehouse--createupdate-warehouses) | Create/update warehouses        |
| 11.2 | [`setStock`](08-set-warehouse-orders.md#112-setstock--set-warehouse-stock-inventory) | Set stock (inventory)          |
| 11.3 | [`setPurchase`](08-set-warehouse-orders.md#113-setpurchase--goods-receipt) | Goods receipt                   |
| 11.4 | [`setMovement`](08-set-warehouse-orders.md#114-setmovement--transfer) | Transfer                        |
| 11.5 | [`setVsExchange`](08-set-warehouse-orders.md#115-setvsexchange--exchange-vansel) | Exchange (Vansel)               |
| 11.6 | [`setMovingToFilial`](08-set-warehouse-orders.md#116-setmovingtofilial--move-to-filial) | Move to filial                  |
| 11.7 | [`setSupplierReturn`](08-set-warehouse-orders.md#117-setsupplierreturn--supplier-return) | Supplier return                 |
| 11.8 | [`setExcretion`](08-set-warehouse-orders.md#118-setexcretion--write-off) | Write-off                       |
| 11.9 | [`setCorrection`](08-set-warehouse-orders.md#119-setcorrection--stock-correction) | Stock correction                |
| 12.1 | [`setOrder`](08-set-warehouse-orders.md#121-setorder--createupdate-order) | Create/update order             |
| 12.2 | [`setDeletedOrder`](08-set-warehouse-orders.md#122-setdeletedorder--cancel-order) | Cancel order                    |
| 12.3 | [`setStatus`](08-set-warehouse-orders.md#123-setstatus--change-order-status) | Change order status             |
| 12.4 | [`setCode`](08-set-warehouse-orders.md#124-setcode--set-1c-code-for-documents) | Set 1C code for documents       |
| 12.5 | [`setOrderDefect`](08-set-warehouse-orders.md#125-setorderdefect--create-return) | Create return                   |
| 12.6 | [`syncOrder`](08-set-warehouse-orders.md#126-syncorder--order-sync) | Order sync                      |

**File:** [08-set-warehouse-orders.md](08-set-warehouse-orders.md)

---

**Finance, Photo, Extra (13–16):**

| #    | Method | Description               |
| ---- | ------ | -------------------------- |
| 13.1 | [`setPayment`](09-finance-photo-extra.md#131-setpayment--create-payment) | Create payment        |
| 13.2 | [`setBalance`](09-finance-photo-extra.md#132-setbalance--set-opening-balance) | Set opening balance   |
| 13.3 | [`setCurrentBalance`](09-finance-photo-extra.md#133-setcurrentbalance--set-current-balance) | Set current balance   |
| 13.4 | [`setConsumption`](09-finance-photo-extra.md#134-setconsumption--create-expense) | Create expense        |
| 14.1 | [`setPhoto`](09-finance-photo-extra.md#141-setphoto--upload-product-photo) | Upload product photo  |
| 14.2 | [`getPhoto`](09-finance-photo-extra.md#142-getphoto--get-product-photo) | Get product photo     |
| 15.1 | [`getFilials`](09-finance-photo-extra.md#151-getfilials--list-filials) | List filials          |
| 15.2 | [`getStoreLog`](09-finance-photo-extra.md#152-getstorelog--store-operations-log) | Store operations log  |
| 15.3 | [`getClientPending`](09-finance-photo-extra.md#153-getclientpending--client-requests) | Client requests       |
| 15.4 | [`deleteClientPending`](09-finance-photo-extra.md#154-deleteclientpending--delete-client-request) | Delete client request |

**File:** [09-finance-photo-extra.md](09-finance-photo-extra.md)

---
