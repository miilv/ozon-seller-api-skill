# Premium Methods

## Premium

### `POST /v1/chat/send/message`

**Send message**

Operation ID: `ChatAPI_ChatSendMessage`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription.

Sends a message to an existing chat by its identifier.

**Request body:**

- `chat_id` (string) **(required)** — Chat identifier.
- `text` (string) **(required)** — Message text in the plain text format from 1 to 1000 symbols.

**Response 200:**

- `result` (string) — Method result.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/chat/start`

**Create a new chat**

Operation ID: `ChatAPI_ChatStart`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription.

Creates a new chat on the shipment with the customer. For example, to clarify the address or the product model.

**Request body:**

- `posting_number` (string) **(required)** — Shipment identifier.

**Response 200:**

- `result` (object)
  - `chat_id` (string) — Chat identifier.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/chat/read`

**Mark messages as read**

Operation ID: `ChatAPI_ChatReadV2`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription.

A method for marking the selected message and messages before it as read.

**Request body:**

- `chat_id` (string) **(required)** — Chat identifier.
- `from_message_id` (integer(uint64)) — Message identifier.

**Response 200:**

- `unread_count` (integer(int64)) — Number of unread messages in the chat.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/analytics/data`

**Analytics data**

Operation ID: `AnalyticsAPI_AnalyticsGetData`

Specify the period and metrics that are required. The response will contain analytical data grouped by the `dimensions` parameter.

There are restrictions for sellers without the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription:
- data is available for the last 3 months,
- some of the data grouping methods and metrics aren't available.

There are no restrictions for sellers with [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription.

You can use the method for no more than 1 request per minute.
Match...

**Request body:**

- `date_from` (string) **(required)** — Date from which the data will be in the report.
- `date_to` (string) **(required)** — Date up to which the data will be in the report.
- `dimension` (array[object]) **(required)** — Data grouping in the report.  Data grouping available to all sellers:   - `unknownDimension`—unknown,   - `sku`—product identifier,   - `spu`—identifi
- `filters` (array[object]) — Filters.
  - `key` (string) — Sorting parameter. You can pass any attribute from the `dimension` and `metric` parameters except the `brand` attribute.
  - `op` (enum) — Values: `EQ, GT, GTE, LT, LTE`
  - `value` (string) — Value for comparison.
- `limit` (integer(int64)) **(required)** — Number of items in the response:   - maximum is 1000,   - minimum is 1.
- `metrics` (array[object]) **(required)** — Specify up to 14 metrics. If there are more, you will get an error with the `InvalidArgument` code.  The list of metrics for which the report will be 
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `sort` (array[object]) — Report sorting settings.
  - `key` (string) — Metric by which the method result will be sorted.
  - `order` (enum) — Values: `ASC, DESC`

**Response 200:**

- `result` (object)
  - `data` (array[object]) — Data array.
  - `totals` (array[number]) — Total and average metrics values.
- `timestamp` (string) — Report creation time.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/analytics/product-queries`

**Get details on your product queries**

Operation ID: `AnalyticsAPI_AnalyticsProductQueries`

Use the method to get data about your product queries. Full analytics is available with the [Premium](https://docs.ozon.ru/global/en/promotion/subscriptions/premium/), [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/), or Premium Pro subscription. Without subscription, you can see a part of the metrics. The method is similar to the **Products in Search → Queries for my product** tab in your personal account.

You can view analytics by queries for certain dates. To do this, specify the interval in the `date_from` and `date_to` fields. Data for the last month a...

**Request body:**

- `date_from` (string(date-time)) **(required)** — Date when analytics generation starts.
- `date_to` (string(date-time)) — Date when analytics generation ends.
- `page` (integer(int32)) — Number of the page returned in the request.
- `page_size` (integer(int32)) **(required)** — Number of elements on the page.
- `skus` (array[string]) **(required)** — List of SKUs—product identifiers in the Ozon system. Analytics on requests is returned for them. Maximum value is 1,000 SKUs.
- `sort_by` (enum) — Values: `BY_SEARCHES, BY_VIEWS, BY_POSITION, BY_CONVERSION, BY_GMV`
- `sort_dir` (enum) — Values: `DESCENDING, ASCENDING`

**Response 200:**

- `analytics_period` (object)
  - `date_from` (string) — Date when analytics generation starts.
  - `date_to` (string) — Date when analytics generation ends.
- `items` (array[object]) — Product list.
  - `category` (string) — Category name.
  - `currency` (string) — Currency.
  - `gmv` (number(float)) — Sales by queries.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `position` (number(float)) — Average product position. Available only with the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premium-program) or [Premium Plus](h
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `unique_search_users` (integer(int64)) — Number of customers who searched for your product on Ozon.
  - `unique_view_users` (integer(int64)) — Number of customers who have seen your product on Ozon. Available only with the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premiu
  - `view_conversion` (number(float)) — Conversion from product views. Available only with the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premium-program) or [Premium Pl
- `page_count` (integer(int64)) — Number of pages.
- `total` (integer(int64)) — Total number of queries.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/analytics/product-queries/details`

**Get query details by product**

Operation ID: `AnalyticsAPI_AnalyticsProductQueriesDetails`

Use the method to get details on product-specific queries. Full analytics is available with [Premium](https://docs.ozon.ru/global/en/promotion/subscriptions/premium/), [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/), or Premium Pro subscription. Without a subscription, you can view a part of the metrics. Method is similar to viewing product details in the **Products in search → Requests for my product** tab in your personal account.

You can view analytics by queries for certain dates. To do this, specify the interval in the `date_from` and `date_to` fields...

**Request body:**

- `date_from` (string(date-time)) **(required)** — Date when analytics generation starts.
- `date_to` (string(date-time)) — Date when analytics generation ends.
- `limit_by_sku` (integer(int32)) **(required)** — Available number of queries per SKU. Maximum is 15 queries.
- `page` (integer(int32)) — Number of the page returned in the request.
- `page_size` (integer(int32)) **(required)** — Number of elements on the page.
- `skus` (array[string]) **(required)** — Product identifiers in the Ozon system, SKU. They will return analytics on the queries. Maximum is 1 000 SKUs.
- `sort_by` (enum) — Values: `BY_SEARCHES, BY_VIEWS, BY_POSITION, BY_CONVERSION, BY_GMV`
- `sort_dir` (enum) — Values: `DESCENDING, ASCENDING`

**Response 200:**

- `analytics_period` (object)
  - `date_from` (string) — Date when analytics generation starts.
  - `date_to` (string) — Date when analytics generation ends.
- `page_count` (integer(int64)) — Number of pages.
- `queries` (array[object]) — List of queries.
  - `currency` (string) — Currency.
  - `gmv` (number(float)) — Sales by queries.
  - `order_count` (integer(int64)) — Number of orders by queries.
  - `position` (number(float)) — Average product position. Available only with the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premium-program) or [Premium Plus](h
  - `query` (string) — Query text.
  - `query_index` (integer(int64)) — Query order number.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `unique_search_users` (integer(int64)) — Number of customers who searched for your product on Ozon.
  - `unique_view_users` (integer(int64)) — Number of customers who have seen your product on Ozon. Available only with the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premiu
  - `view_conversion` (number(float)) — Conversion from product views. Available only with the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premium-program) or [Premium Pl
- `total` (integer(int64)) — Total number of queries.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/finance/realization/by-day`

**Sales report per day**

Operation ID: `FinanceAPI_GetRealizationByDayReportV1`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription.

Returns sales amount data from the [sales report](#operation/FinanceAPI_GetRealizationReportV2) per day. Canceled or non-purchased products aren't included. Data is available for a maximum of 32 calendar days from the current date.

**Request body:**

- `day` (integer(int32)) **(required)** — Day.
- `month` (integer(int32)) **(required)** — Month.
- `year` (integer(int32)) **(required)** — Year.

**Response 200:**

- `rows` (array[object]) — Report table.
  - `commission_ratio` (number(double)) — Percentage of sales commission by category.
  - `delivery_commission` (object)
  - `item` (object)
  - `return_commission` (object)
  - `rowNumber` (integer(int32)) — Row number.
  - `seller_price_per_instance` (number(double)) — Seller's discounted price.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/search-queries/text`

**Get list of search queries by text**

Operation ID: `SearchQueriesAPI_SearchQueriesText`

Available for sellers with the Premium Pro subscription.

**Request body:**

- `limit` (string(int64)) **(required)** — Number of values per page.
- `offset` (string(int64)) **(required)** — Number of elements to be skipped in the response.
- `sort_by` (enum) — Values: `CLIENT_COUNT, ADD_TO_CART, CONVERSION_TO_CART, AVG_PRICE`
- `sort_dir` (enum) — Values: `ASC, DESC`
- `text` (string) **(required)** — Text search.

**Response 200:**

- `offset` (string(int64)) — Number of search queries per page.
- `search_queries` (array[object]) — Search query details.
  - `add_to_cart` (number(float)) — Number of customers who added at least 1 product to the cart.
  - `avg_price` (number(float)) — Average price of products in RUB.
  - `client_count` (number(float)) — Number of customers who searched for the product by this query.
  - `conversion_to_cart` (number(float)) — Percentage of customers who added at least 1 product to the cart.
  - `items_views` (number(float)) — Number of product views.
  - `query` (string) — Search query.
  - `sellers_count` (number(float)) — Average number of sellers whose products were viewed by customers by this query.
- `total` (string(int64)) — Total number of search queries.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/search-queries/top`

**Get list of popular search queries**

Operation ID: `SearchQueriesAPI_SearchQueriesTop`

Available for sellers with the Premium Pro subscription.

**Request body:**

- `limit` (string(int64)) **(required)** — Number of values per page.
- `offset` (string(int64)) **(required)** — Number of elements to be skipped in the response.

**Response 200:**

- `offset` (string(int64)) — Number of search queries per page.
- `search_queries` (array[object]) — Search query details.
  - `add_to_cart` (number(float)) — Number of customers who added at least 1 product to the cart.
  - `avg_price` (number(float)) — Average price of products in RUB.
  - `client_count` (number(float)) — Number of customers who searched for the product by this query.
  - `conversion_to_cart` (number(float)) — Percentage of customers who added at least 1 product to the cart.
  - `items_views` (number(float)) — Number of product views.
  - `query` (string) — Search query.
  - `sellers_count` (number(float)) — Average number of sellers whose products were viewed by customers by this query.
- `total` (string(int64)) — Total number of search queries.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/prices/details`

**Get details on product prices**

Operation ID: `ProductPricesDetails`

Available for sellers with the Premium Pro subscription.

**Request body:**

- `skus` (array[string]) **(required)** — List of SKUs.

**Response 200:**

- `prices` (array[object]) — Product prices.
  - `customer_price` (object)
  - `discount_percent` (number(float)) — Percentage of discount from Ozon.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `price` (object)
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 403:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 404:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 409:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

**Response 500:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

