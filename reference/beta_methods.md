# Beta Methods

## BetaIntro

Seller API Beta Methods is a section with methods that are still being tested.
Such methods may be unstable, and their requests and responses may change.
We provide one week's notice regarding changes in beta methods operations in the [Ozon for dev community](https://dev.ozon.ru/community?category_id=79).
There, you can also leave feedback on the beta methods performance and offer your ideas.

Beta methods are available by API keys depending on your role.

## BetaMethod

### `POST /v1/analytics/manage/stocks`

**Stock management**

Operation ID: `AnalyticsAPI_ManageStocks`

> **Note:** 
On January 22, 2026, the method will be disabled. Switch to the [/v1/analytics/stocks](#operation/AnalyticsAPI_AnalyticsStocks) method.

Use the method to find out how many product items are left in FBO stock.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1106-Razdel-upravleniia-ostatkami-analytics-manage-stocks) in the Ozon for dev community.

**Request body:**

- `filter` (object)
  - `skus` (array[string]) — Product identifiers in the Ozon system, SKU.
  - `stock_types` (array[string]) — The type of item in stock: - `STOCK_TYPE_VALID` : valid products. The stock of products available for sale.  - `STOCK_TYPE_WAITING_DOCS`: waiting for 
  - `warehouse_ids` (array[string]) — Warehouse identifiers.
- `limit` (integer(int32)) — Number of values in the response.
- `offset` (integer(int32)) — Number of elements to skip in the response.  For example, if `offset = 10`, the response starts with the 11th element found.

**Response 200:**

- `items` (array[object]) — Products.
  - `defect_stock_count` (integer(int64)) — Stock of defective products, pcs.
  - `expiring_stock_count` (integer(int64)) — Stock of near-expired products, pcs.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `valid_stock_count` (integer(int64)) — Stock of valid products.
  - `waitingdocs_stock_count` (integer(int64)) — Stock of products that waiting for documents
  - `warehouse_name` (string) — Warehouse name.

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

### `POST /v1/removal/from-supply/list`

**Report on removal and disposal from FBO supply**

Operation ID: `GetSupplyReturnsSummaryReport`

Matches the [**FBO → Removal and disposal**](https://seller.ozon.ru/app/fbo-operations/returns) section in your personal account.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1608-Novye-metody-po-vyvozu-i-utilizatsii) in the Ozon for dev community.

**Request body:**

- `date_from` (string) **(required)** — Period start date in the `YYYY-MM-DD` format.
- `date_to` (string) **(required)** — Period end date in the `YYYY-MM-DD` format.
- `last_id` (string) — Identifier of the last value on the page. To get the next values, specify `last_id` from the response of the previous request.
- `limit` (integer(int32)) **(required)** — Number of items in a response.

**Response 200:**

- `last_id` (string) — Identifier of the last value on the page..
- `returns_summary_report_rows` (array[object]) — Product details.
  - `barcode` (string) — Product barcode.
  - `box_barcode` (string) — Box barcode.
  - `box_height` (number(double)) — Box height in meters.
  - `box_id` (integer(int64)) — Box identifier.
  - `box_length` (number(double)) — Box length in meters.
  - `box_state` (string) — Box status: - `available for removal`; - `already in removal request`; - `preparing for removal`; - `lost`; - `on the way`; - `compensated to seller`;
  - `box_volume` (number(double)) — Box volume in liters.
  - `box_weight` (number(double)) — Box weight in kilograms.
  - `box_width` (number(double)) — Box width in meters.
  - `clearing_warehouse_name` (string) — Warehouse with products prepared for removal.
  - `delivery_date` (string) — Date of delivery to a pick-up-point, sorting center, or by courier.
  - `delivery_type` (string) — Removal method: - `self pickup`; - `pick-up point`; - `sorting center`; - `courier delivery`.
  - `destination_warehouse_address` (string) — Destination warehouse address.
  - `destination_warehouse_name` (string) — Destination warehouse name.
  - `given_out_date` (string) — Date when seller picked up products from Ozon warehouse.
  - `is_auto_return` (boolean) — Indication that removal request is created automatically.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product article code.
  - `preliminary_delivery_price` (number(double)) — Preliminary cost of product removal from the warehouse by Ozon.
  - `quant_count` (integer(int32)) — Number of quants in removal request.
  - `quantity_for_return` (integer(int32)) — Number of product items in box or removal request.
  - `return_created_at` (string) — Date of removal request creation.
  - `return_id` (integer(int64)) — Removal request identifier.
  - `return_state` (string) — Removal request status.  Possible values for pickup from stock: - `created`; - `being packaged at warehouse`; - `packed at warehouse`; - `completed`; 
  - `sku` (integer(int64)) — Product SKU in Ozon system.
  - `stock_type` (string) — Product stock type: - `available for sale`; - `labeled products awaiting actions`; - `expiring`; - `defective products available for removal from stoc
  - `utilization_date` (string) — Disposal date.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/removal/from-stock/list`

**Report on removal and disposal from FBO stock**

Operation ID: `GetSupplierReturnsSummaryReport`

Matches the [**FBO → Removal and disposal**](https://seller.ozon.ru/app/fbo-operations/returns) section in your personal account.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1608-Novye-metody-po-vyvozu-i-utilizatsii) in the Ozon for dev community.

**Request body:**

- `date_from` (string) **(required)** — Period start date in the `YYYY-MM-DD` format.
- `date_to` (string) **(required)** — Period end date in the `YYYY-MM-DD` format.
- `last_id` (string) — Identifier of the last value on the page. To get the next values, specify `last_id` from the response of the previous request.
- `limit` (integer(int32)) **(required)** — Number of items in a response.

**Response 200:**

- `last_id` (string) — Identifier of the last value on the page.
- `returns_summary_report_rows` (array[object]) — Product details.
  - `barcode` (string) — Product barcode.
  - `box_barcode` (string) — Box barcode.
  - `box_height` (number(double)) — Box height in meters.
  - `box_id` (integer(int64)) — Box identifier.
  - `box_length` (number(double)) — Box length in meters.
  - `box_state` (string) — Box status: - `available for removal`; - `already in removal request`; - `preparing for removal`; - `lost`; - `on the way`; - `compensated to seller`;
  - `box_volume` (number(double)) — Box volume in liters.
  - `box_weight` (number(double)) — Box weight in kilograms.
  - `box_width` (number(double)) — Box width in meters.
  - `clearing_warehouse_name` (string) — Warehouse with products prepared for removal.
  - `delivery_date` (string) — Date of delivery to a pick-up-point, sorting center, or by courier.
  - `delivery_type` (string) — Removal method: - `self pickup`; - `pick-up point`; - `sorting center`; - `courier delivery`.
  - `destination_warehouse_address` (string) — Destination warehouse address.
  - `destination_warehouse_name` (string) — Destination warehouse name.
  - `given_out_date` (string) — Date when seller picked up products from Ozon warehouse.
  - `is_auto_return` (boolean) — Indication that removal request is created automatically.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product article code.
  - `preliminary_delivery_price` (number(double)) — Preliminary cost of product removal from the warehouse by Ozon.
  - `quant_count` (integer(int32)) — Number of quants in removal request.
  - `quantity_for_return` (integer(int32)) — Number of product items in box or removal request.
  - `return_created_at` (string) — Date of removal request creation.
  - `return_id` (integer(int64)) — Removal request identifier.
  - `return_state` (string) — Removal request status.  Possible values for pickup from stock: - `created`; - `being packaged at warehouse`; - `packed at warehouse`; - `completed`; 
  - `sku` (integer(int64)) — Product SKU in Ozon system.
  - `stock_type` (string) — Product stock type: - `available for sale`; - `labeled products awaiting actions`; - `expiring`; - `defective products available for removal from stoc
  - `utilization_date` (string) — Disposal date.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/stairway-discount/by-quantity/set`

**Manage quantity discounts**

Operation ID: `ProductAPI_SetProductStairwayDiscountByQuantity`

Sets or removes a product discount based on its quantity in an order.
  
You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1719-Novye-metody-dlia-raboty-so-skidkoi-ot-kolichestva/) in the Ozon for dev community.

**Request body:**

- `stairways` (array[object]) **(required)** — Information about the quantity discount for products.
  - `enabled` (boolean) **(required)** — Pass `true` to enable discount.
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
  - `stairway` (object) **(required)**
    - `steps` (array[object]) **(required)** — Information about the quantity discount levels. Number of levels is from 1 to 4.
- `suppress_warnings` (boolean) — Pass `true` to ignore warnings and set the discount.

**Response 200:**

- `accepted` (boolean) — `true` if the request is accepted. Use the [/v1/product/stairway-discount/by-quantity/get](#operation/ProductAPI_GetProductStairwayDiscountByQuantity)
- `errors` (array[object]) — Error description.
  - `data` (array[object]) — Error or warning description.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
- `warnings` (array[object]) — Warning description.

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

### `POST /v1/product/stairway-discount/by-quantity/get`

**Get quantity discount information**

Operation ID: `ProductAPI_GetProductStairwayDiscountByQuantity`

Returns information about a product discount based on its quantity in an order.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1719-Novye-metody-dlia-raboty-so-skidkoi-ot-kolichestva/) in the Ozon for dev community.

**Request body:**

- `skus` (array[string]) **(required)** — Product identifiers in the Ozon system, SKU.

**Response 200:**

- `stairways` (array[object]) — Information about the quantity discount for a specific product.
  - `enabled` (boolean) — `true` if the quantity discount is enabled.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `stairway` (object)
  - `status` (enum) — Values: `IN_PROCESS, ERROR, SUCCESS`

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

### `POST /v1/finance/balance`

**Get balance report**

Operation ID: `GetFinanceBalanceV1`

Matches the **Finance → Balance** section in your seller account.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1732-Novyi-metod-polucheniia-dannykh-po-balansu/) in the Ozon for dev community.

**Request body:**

- `date_from` (string(date-time)) **(required)** — Start date of the reporting period in the `YYYY-MM-DD` format.
- `date_to` (string(date-time)) **(required)** — End date of the reporting period in the `YYYY-MM-DD` format. The maximum period between `date_from` and `date_to` is 30 days.

**Response 200:**

- `cashflows` (object)
  - `returns` (object)
  - `sales` (object)
  - `services` (array[object]) — Accruals for other services.
- `total` (object)
  - `accrued` (object)
  - `closing_balance` (object)
  - `opening_balance` (object)
  - `payments` (array[object]) — Payments for the period.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/actions/discounts-task/list`

**Get list of discount requests**

Operation ID: `GetDiscountTaskListV2`

Method for getting a list of products that customers want to buy with a discount.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1856-Novye-metody-dlia-raboty-s-polucheniem-Spiska-zaiavok-na-skidku/) in the Ozon for dev community.

**Request body:**

- `last_id` (integer(int64)) — Identifier of the last value on the page. Leave this field blank in the first request.
- `limit` (enum(int64)) — Values: `5, 10, 15, 20, 30, 50`. The maximum number of requests on a page.
- `status` (enum) — Values: `ALL, NEW, APPROVED, DECLINED`

**Response 200:**

- `tasks` (array[object]) — List of requests.
  - `approved_discount` (number(double)) — Discount in RUB approved by the seller. Pass `0` if the seller didn't approve the request.
  - `approved_price` (number(double)) — Approved price.
  - `approved_quantity_max` (integer(uint64)) — Approved maximum quantity of products.
  - `auto_moderated_info` (object) — Information about automatic moderation of the request.
  - `created_at` (string(date-time)) — Request creation date.
  - `edited_till` (string(date-time)) — Remaining time to change the decision.
  - `edited_till_duration` (integer(uint64)) — Remaining time to change the decision in seconds.
  - `email` (string) — Email of the user who processed the request.
  - `end_at` (string(date-time)) — End time of the request.
  - `end_at_duration` (integer(uint64)) — End time of the request in seconds.
  - `first_name` (string) — First name of user who processed the request.
  - `id` (integer(uint64)) — Request identifier.
  - `is_auto_moderated` (boolean) — `true` if the request was moderated automatically.
  - `last_name` (string) — Last name of user who processed the request.
  - `min_auto_price` (number(double)) — Minimum price after auto-application of discounts and special offers.
  - `moderated_at` (string(date-time)) — Moderation date: review, approval, or decline of the request.
  - `name` (string) — Product name.
  - `original_price` (number(double)) — Product price before applying discounts.
  - `patronymic` (string) — Patronymic of the user who processed the request.
  - `reduction_factor` (number(double)) — Difference between customer price and seller price at the time of request creation.
  - `requested_discount` (number(double)) — Discount percentage.
  - `requested_price` (number(double)) — Requested price.
  - `requested_quantity_max` (integer(uint64)) — Requested maximum number of products.
  - `sku` (integer(uint64)) — Product identifier in the Ozon system, SKU.
  - `status` (enum) — Values: `ALL, NEW, APPROVED, DECLINED`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/cluster/list`

**Get information about macrolocal clusters**

Operation ID: `DraftClusterList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1873-Novyi-metod-polucheniia-makrolokalnykh-klasterov/) in the Ozon for dev community.

**Response 200:**

- `result` (array[object]) — Cluster list.
  - `data` (object)
  - `macrolocal_cluster_id` (integer(int64)) — Cluster identifier.

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

### `POST /v1/description-category/tips`

**Get tips to identify product category**

Operation ID: `DescriptionCategoryTips`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1963-Novyi-metod-dlia-raboty-s-polucheniem-podskazok-v-Dereve-kategorii-i-tipov-tovarov/) in the Ozon for dev community.

**Request body:**

- `type_id` (array[string]) — Product type identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.

**Response 200:**

- `result` (array[object]) — List of tips.
  - `images_url` (array[string]) — Links to images of similar products.
  - `info_url` (string) — Link to Ozon product storefront with similar products and information about them.
  - `type_id` (integer(int64)) — Product type identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/visibility/set`

**Set product visibility on Ozon and Ozon Select storefronts**

Operation ID: `ProductVisibilitySet`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1951-Novyi-metod-upravleniia-vidimostiu-na-vitrinakh/) in the Ozon for dev community.

**Request body:**

- `item_placement` (array[object]) **(required)** — Product visibility details.
  - `placement` (enum) **(required)** — Values: `OZON, SELECT, OZON_SELECT`
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.

**Response 200:**

- `items` (array[object]) — Product visibility details.
  - `select_permission` (enum) — Values: `UNSPECIFIED, RESTRICTED, ALLOWED`
  - `seller_item_placement` (enum) — Values: `UNSPECIFIED, OZON, SELECT, OZON_SELECT`
  - `seller_item_placement_list` (array[object]) — List of visibility set by the seller:   - `UNSPECIFIED`: undefined;   - `OZON`: only on Ozon;   - `SELECT`: only on the Select.
  - `showcases_visibility` (enum) — Values: `UNSPECIFIED, OZON, SELECT, OZON_SELECT, NONE`
  - `showcases_visibility_list` (array[object]) — List of storefronts where the product is displayed:   - `UNSPECIFIED`: undefined;   - `OZON`: only on Ozon;   - `SELECT`: only on the Select.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `warnings` (array[string]) — Warnings.
- `items_errors` (array[object]) — Products with errors.
  - `code` (string) — Error code.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/posting/digital/list`

**Get shipment list**

Operation ID: `PostingDigitalList`

Returns a list of shipments for which digital product codes need to be uploaded. Method is available only to sellers working with digital products. 

To get a list of shipments in any status, use the [/v3/posting/fbo/list](#operation/PostingFboList) method.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/2050-Novyi-beta-metod-dlia-postingov-s-zagruzkoi-kodov-tsifrovykh-tovarov-v-Seller-API/) in the Ozon for dev community.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `order_numbers` (array[string]) — Order numbers to which the shipments belong.
  - `posting_numbers` (array[string]) — Shipment identifiers.
  - `since` (string(date-time)) — Start date.
  - `to` (string(date-time)) — End date.
- `limit` (integer(int64)) — Number of values in the response.
- `sort_dir` (enum) — Values: `ASC, DESC`
- `with` (object)
  - `analytics_data` (boolean) — `true` to add analytics data.
  - `financial_data` (boolean) — `true` to add financial data.
  - `legal_info` (boolean) — `true` to add legal information.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `has_next` (boolean) — `true` if the response doesn't contain all shipments.
- `postings` (array[object]) — Shipment list.
  - `additional_data` (array[object]) — Additional parameters.
  - `analytics_data` (object)
  - `cancel_reason_id` (integer(int64)) — Identifier of shipment cancellation reason.
  - `cancellation` (object)
  - `created_at` (string(date-time)) — Date and time of shipment creation.
  - `external_order` (object)
  - `financial_data` (object)
  - `in_process_at` (string(date-time)) — Start date and time of shipment processing.
  - `legal_info` (object)
  - `order_id` (integer(int64)) — Order identifier to which the shipment belongs.
  - `order_number` (string) — Order number to which the shipment belongs.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — List of products in the shipment.
  - `status` (string) — Shipment status:  - `awaiting_packaging`: awaiting packaging.
  - `waiting_deadline_for_digital_code` (string(date-time)) — Deadline for providing digital product codes. Upload codes using the [/v1/posting/digital/codes/upload](#operation/UploadPostingCodes) method.

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

## Quants

### `POST /v1/product/quant/list`

**Economy products list**

Operation ID: `QuantProductList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1084-Metody-po-tarifu-Ekonom) in the Ozon for dev community.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `limit` (integer(int64)) **(required)** — Maximum number of values in the response.
- `visibility` (enum) — Values: `ALL, VISIBLE, INVISIBLE, EMPTY_STOCK, NOT_MODERATED, MODERATED, DISABLED, STATE_FAILED`. Filter by product visibility: - `ALL`: all products except archived ones; - `VISIBLE`: products visible to customers; - `INVISIBLE`: products not visi

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `products` (object) — Economy products.
- `total_items` (integer(int32)) — Leftover stock in all warehouses.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/quant/info`

**Economy product information**

Operation ID: `QuantGetInfo`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1084-Metody-po-tarifu-Ekonom) in the Ozon for dev community.

**Request body:**

- `quant_code` (object) **(required)** — List of MOQs with products.

**Response 200:**

- `items` (array[object]) — Economy products.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `quant_info` (object)

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## ReviewAPI

### `POST /v1/review/comment/create`

**Leave a comment on the review**

Operation ID: `ReviewAPI_CommentCreate`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

**Request body:**

- `mark_review_as_processed` (boolean) — Review status update: - `true`: status changes to `Processed`. - `false`: status doesn't change.
- `parent_comment_id` (string) — Identifier of the parent comment you're replying to.
- `review_id` (string) **(required)** — Review identifier.
- `text` (string) **(required)** — Comment text.

**Response 200:**

- `comment_id` (string) — Comment identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/review/comment/delete`

**Delete a comment on a review**

Operation ID: `ReviewAPI_CommentDelete`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

**Request body:**

- `comment_id` (string) **(required)** — Comment identifier.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/review/comment/list`

**List of comments for the review**

Operation ID: `ReviewAPI_CommentList`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

Method returns information about comments on reviews that have passed moderation.

**Request body:**

- `limit` (integer(int32)) **(required)** — Limit of values in the response. Minimum is 20. Maximum is 100.
- `offset` (integer(int32)) — Number of elements that is skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `review_id` (string) **(required)** — Review identifier.
- `sort_dir` (enum) — Values: `ASC, DESC`

**Response 200:**

- `comments` (array[object]) — Comment details.
  - `id` (string) — Comment identifier.
  - `is_official` (boolean) — `true`, if the comment was left by an official, `false` if a customer left it.
  - `is_owner` (boolean) — `true`, if the comment was left by a seller, `false` if a customer left it.
  - `parent_comment_id` (string) — Identifier of the parent comment to reply to.
  - `published_at` (string(date-time)) — Date the comment was published.
  - `text` (string) — Comment text.
- `offset` (integer(int32)) — Number of elements in the response.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/review/change-status`

**Change review status**

Operation ID: `ReviewAPI_ReviewChangeStatus`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

**Request body:**

- `review_ids` (array[string]) **(required)** — Array with review identifiers from 1 to 100.
- `status` (string) **(required)** — Review status: - `PROCESSED`: processed, - `UNPROCESSED`: not processed.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/review/count`

**Number of reviews by status**

Operation ID: `ReviewAPI_ReviewCount`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

**Response 200:**

- `processed` (integer(int32)) — Number of processed reviews.
- `total` (integer(int32)) — Number of all reviews.
- `unprocessed` (integer(int32)) — Number of unprocessed reviews.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/review/info`

**Get review details**

Operation ID: `ReviewAPI_ReviewInfo`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

**Request body:**

- `review_id` (string) **(required)** — Review identifier.

**Response 200:**

- `comments_amount` (integer(int32)) — Number of comments on the review.
- `dislikes_amount` (integer(int32)) — Number of dislikes on the review.
- `id` (string) — Review identifier.
- `is_rating_participant` (boolean) — `true`, if the review affects the rating calculation.
- `likes_amount` (integer(int32)) — Number of likes on the review.
- `order_status` (string) — Status of the order for which the customer left a review: - `DELIVERED`: delivered, - `CANCELLED`: cancelled.
- `photos` (array[object]) — Image details.
  - `height` (integer(int32)) — Height.
  - `url` (string) — Link to image.
  - `width` (integer(int32)) — Width.
- `photos_amount` (integer(int32)) — Number of images in the review.
- `published_at` (string(date-time)) — Review publication date.
- `rating` (integer(int32)) — Review rating.
- `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
- `status` (string) — Review status: - `UNPROCESSED`: not processed, - `PROCESSED`: processed.
- `text` (string) — Review text.
- `videos` (array[object]) — Video details.
  - `height` (integer(int64)) — Height.
  - `preview_url` (string) — Link to video preview.
  - `short_video_preview_url` (string) — Link to short video.
  - `url` (string) — Video link.
  - `width` (integer(int64)) — Width.
- `videos_amount` (integer(int32)) — Number of videos for the review.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/review/list`

**Get a list of reviews**

Operation ID: `ReviewAPI_ReviewList`

Available to sellers with the [Review Management](https://docs.ozon.ru/global/en/work-with-customers/managing-reviews/managing-reviews-subscription/) or Premium Pro subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1089-Metody-sozdaniia-zaiavok-na-postavku)
in the Ozon for dev community.

Method doesn't return the “Advantages” and “Disadvantages” parameters if they are included in product reviews. The parameters are outdated and aren't included in new reviews.

**Request body:**

- `last_id` (string) — Identifier of the last review on the page.
- `limit` (integer(int32)) **(required)** — Number of reviews in the response. Minimum is 20, maximum is 100.
- `sort_dir` (string) — Sorting direction: - `ASC`: ascending, - `DESC`: descending.
- `status` (string) — Review statuses: - `ALL`: all statuses, - `UNPROCESSED`: not processed, - `PROCESSED`: processed.

**Response 200:**

- `has_next` (boolean) — `true`, if not all reviews were returned in the response.
- `last_id` (string) — Identifier of the last review on the page.
- `reviews` (array[object]) — Review details.
  - `comments_amount` (integer(int32)) — Number of comments on the review.
  - `id` (string) — Review identifier.
  - `is_rating_participant` (boolean) — `true`, if the review affects the rating calculation.
  - `order_status` (string) — Status of the order for which the customer left feedback: - `DELIVERED`: delivered, - `CANCELLED`: canceled.
  - `photos_amount` (integer(int32)) — Number of images in the review.
  - `published_at` (string(date-time)) — Review publication date.
  - `rating` (integer(int32)) — Review rating.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `status` (string) — Review status: - `UNPROCESSED`: not processed, - `PROCESSED`: processed.
  - `text` (string) — Review text.
  - `videos_amount` (integer(int32)) — Number of videos in the review.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## Questions&Answers

### `POST /v1/question/answer/create`

**Create answer to question**

Operation ID: `QuestionAnswer_Create`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `question_id` (string) **(required)** — Question identifier.
- `sku` (integer(int64)) **(required)** — Product SKU in Ozon system.
- `text` (string) **(required)** — Answer text from 2 to 3,000 characters long.

**Response 200:**

- `answer_id` (string) — Identifier of answer to question.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/answer/delete`

**Delete answer to question**

Operation ID: `QuestionAnswer_Delete`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `answer_id` (string) **(required)** — Answer identifier.
- `sku` (integer(int64)) **(required)** — Product SKU in Ozon system.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/answer/list`

**List of answers to question**

Operation ID: `QuestionAnswer_List`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `last_id` (object) — Identifier of the last value on the page.  Leave blank for the first request. For the next values, specify the `last_id` from the response of the prev
- `question_id` (string) **(required)** — Question identifier.
- `sku` (integer(int64)) **(required)** — Product SKU in Ozon system.

**Response 200:**

- `answers` (object) — Answers.
- `last_id` (string) — Identifier of the last value on the page.  To get the next values, specify the received value in the next request in the `last_id` parameter.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/change-status`

**Change question statuses**

Operation ID: `Question_ChangeStatus`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `question_ids` (object) **(required)** — Question identifiers.
- `status` (string) **(required)** — Question statuses: - `NEW`, - `VIEWED`, - `PROCESSED`.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/count`

**Number of questions by statuses**

Operation ID: `Question_Count`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Response 200:**

- `all` (integer(int64)) — Total number of questions.
- `new` (integer(int64)) — New questions.
- `processed` (integer(int64)) — Processed questions.
- `unprocessed` (integer(int64)) — Unprocessed questions.
- `viewed` (integer(int64)) — Viewed questions.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/info`

**Question details**

Operation ID: `Question_Info`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `question_id` (string) **(required)** — Question identifier.

**Response 200:**

- `answers_count` (integer(int64)) — Number of answers to question.
- `author_name` (string) — Question author.
- `id` (string) — Question identifier.
- `product_url` (string) — Product link.
- `published_at` (timestamp) — Question publication date.
- `question_link` (string) — Question link.
- `sku` (integer(int64)) — Product SKU in Ozon system.
- `status` (enum) — Question status: - `NEW`, - `ALL`: all questions, - `VIEWED`, - `PROCESSED`, - `UNPROCESSED`.
- `text` (string) — Question text.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/list`

**Question list**

Operation ID: `Question_List`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `filter` (object)
  - `date_from` (string(date-time)) — Start date.
  - `date_to` (string(date-time)) — End date.
  - `status` (string) — Question status: - `NEW`, - `ALL`: all questions, - `VIEWED`, - `PROCESSED`, - `UNPROCESSED`.
- `last_id` (string) — Identifier of the last value on the page.   Leave blank for the first request. For the next values, specify the `last_id` from the response of the pre
- `limit` (integer(int64)) — Number of values in the response.
- `sort_dir` (enum) — Values: `DESC, ASC`

**Response 200:**

- `questions` (object) — Questions.
- `last_id` (string) — Identifier of the last value on the page.  To get the next values, specify the received value in the next request in the `last_id` parameter.
- `has_next` (boolean) — `true` if not all questions are returned in the response.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

### `POST /v1/question/top-sku`

**Products with the most questions**

Operation ID: `Question_TopSku`

Available to sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions/premium-plus/) subscription.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1198-Metody-dlia-raboty-s-voprosami-otvetami) in the Ozon for dev community.

**Request body:**

- `limit` (integer(int64)) **(required)** — Number of values in the response.

**Response 200:**

- `sku` (object) — List of product SKUs in Ozon system.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (string) — Error details.
- `message` (string) — Error description.

---

## SellerActions

[How to work with seller special offers](#section/Working-with-seller-special-offers)

[Learn more about seller special offers in the Seller knowledge base](https://docs.ozon.ru/global/en/promotion/promotions/seller-promotions/)

### `POST /v1/seller-actions/create/discount`

**Create special offer with "Discount" mechanics**

Operation ID: `SellerActionsCreateDiscount`

> **Note:** 
Not available for sellers from CIS.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
- `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
- `min_action_percent` (number(double)) **(required)** — Minimum discount percentage.
- `title` (string) — Special offer name.

**Response 200:**

- `action_id` (integer(uint64)) — Special offer identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/create/discount-with-condition`

**Create special offer with "Discount of order amount" mechanics**

Operation ID: `SellerActionsCreateDiscountWithCondition`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
- `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
- `discount_type` (enum) **(required)** — Values: `PERCENT, CURRENCY`
- `discount_value` (number(float)) **(required)** — Discount size.
- `min_order_amount` (number(double)) **(required)** — Order amount from which the discount applies.
- `title` (string) — Special offer name.

**Response 200:**

- `action_id` (integer(uint64)) — Special offer identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/create/installment`

**Create special offer with "Interest-free installment" mechanics**

Operation ID: `SellerActionsCreateInstallment`

Installment period is 6 months.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
- `title` (string) **(required)** — Special offer name.

**Response 200:**

- `action_id` (integer(uint64)) — Special offer identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/create/multi-level-discount`

**Create special offer with "Multi-level discount from the amount" mechanics**

Operation ID: `SellerActionsCreateMultiLevelDiscount`

Products are added to the special offer automatically, you don't need to use the [/v1/seller-actions/products/add](#operation/SellerActionsProductsAdd) method.
 
You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
- `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
- `discount_levels` (array[object]) **(required)** — Discount levels.
  - `discount_value` (number(double)) **(required)** — Discount size.
  - `order_amount` (number(double)) **(required)** — Minimum order amount.
- `discount_type` (enum) **(required)** — Values: `PERCENT, CURRENCY`
- `is_legal_entities_segment` (boolean) — `true`, if the special offer is for legal entities only.
- `title` (string) — Special offer name.

**Response 200:**

- `action_id` (integer(uint64)) — Special offer identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/create/voucher`

**Create special offer with "Discount by promo code" mechanics**

Operation ID: `SellerActionsCreateVoucher`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `budget` (integer(int64)) **(required)** — Special offer budget. If the budget runs out, the special offer stops.
- `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
- `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
- `discount_type` (enum) **(required)** — Values: `PERCENT, CURRENCY`
- `discount_value` (number(double)) **(required)** — Discount size.
- `title` (string) **(required)** — Special offer name.
- `user_ids` (array[string]) — Identifiers of users who have access to the promo code.
- `voucher_parameters` (object) **(required)**
  - `count_codes` (integer(uint64)) **(required)** — Number of promo codes.
  - `is_private` (boolean) **(required)** — `true`, if the promo code is publicly available.
  - `type` (enum) **(required)** — Values: `ONE, MULTIPLE, UNIQUE`

**Response 200:**

- `action_id` (integer(uint64)) — Special offer identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/update/discount`

**Update special offer with "Discount" mechanic**

Operation ID: `SellerActionsUpdateDiscount`

> **Note:** 
Not available for sellers from CIS.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera/) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `action_parameters` (object)
  - `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
  - `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
  - `title` (string) **(required)** — Special offer name.

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

### `POST /v1/seller-actions/update/discount-with-condition`

**Update special offer with "Discount of order amount" mechanic**

Operation ID: `SellerActionsUpdateDiscountWithCondition`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera/) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `action_parameters` (object)
  - `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
  - `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
  - `discount_value` (number(double)) **(required)** — Discount amount.
  - `min_order_amount` (number(double)) **(required)** — Minimum order amount to get the discount.
  - `title` (string) **(required)** — Special offer name.

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

### `POST /v1/seller-actions/update/installment`

**Update special offer with "Interest-free installment" mechanic**

Operation ID: `SellerActionsUpdateInstallment`

The installment period is 6 months.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera/) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `action_parameters` (object)
  - `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
  - `title` (string) **(required)** — Special offer name.

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

### `POST /v1/seller-actions/update/multi-level-discount`

**Update special offer with "Multi-level discount from the total amount" mechanic**

Operation ID: `SellerActionsUpdateMultiLevelDiscount`

Products are added to the special offer automatically. You don't need to use the [/v1/seller-actions/products/add](#operation/SellerActionsProductsAdd) method.

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera/) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `action_parameters` (object)
  - `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
  - `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
  - `discount_levels` (array[object]) **(required)** — Discount levels.
    - `discount_value` (number(double)) **(required)** — Discount amount.
    - `order_amount` (number(double)) **(required)** — Minimum order amount for the discount.
  - `is_legal_entities_segment` (boolean) — `true` if the special offer is only for legal entities.
  - `title` (string) **(required)** — Special offer name.

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

### `POST /v1/seller-actions/update/voucher`

**Update special offer with "Discount by promo code" mechanic**

Operation ID: `SellerActionsUpdateVoucher`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera/) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `action_parameters` (object)
  - `budget` (integer(int64)) **(required)** — Special offer budget. Disable the special offer using the [/v1/seller-actions/change-activity](#operation/SellerActionsChangeActivity) method before c
  - `date_end` (string(date-time)) **(required)** — Date and time when the special offer ends.
  - `date_start` (string(date-time)) **(required)** — Date and time when the special offer starts.
  - `discount_value` (number(double)) **(required)** — Discount amount.
  - `title` (string) **(required)** — Special offer name.
  - `user_ids` (array[string]) — Identifiers of the customers with access to the promo code.

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

### `POST /v1/seller-actions/products/add`

**Add products to special offer**

Operation ID: `SellerActionsProductsAdd`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `products` (array[object]) **(required)** — Product details.
  - `currency` (enum) — Values: `RUB, BYN, KZT, EUR, USD, CNY`
  - `discount_percent` (number(float)) — Percentage discount amount. Pass the parameter if the special offer mechanics are "Discount".
  - `sku` (integer(uint64)) **(required)** — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/products/candidates`

**Get list of products that can participate in special offer**

Operation ID: `SellerActionsProductsCandidates`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `cursor` (integer(uint64)) — Cursor for the next data sample.
- `limit` (integer(int64)) **(required)** — Maximum number of values in the response.

**Response 200:**

- `cursor` (integer(uint64)) — Cursor for the next data sample.
- `has_next` (boolean) — Indication that only part of values was returned in the response: - `true`: make a request with a new `cursor` parameter value for getting the rest of
- `products` (array[object]) — Product details.
  - `action_price` (number(double)) — Product price, including the special offer.
  - `base_price` (number(double)) — The base price at which the product is sold on Ozon if it does not participate in the special offer.
  - `currency` (string) — Currency.
  - `discount_percent` (number(double)) — Discount percentage.
  - `is_active` (boolean) — `true` if product participates in the special offer.
  - `min_seller_price` (number(double)) — Minimum price to automatically add product to the special offer.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `price` (number(double)) — Product price for customer.
  - `product_id` (integer(uint64)) — Product identifier in the Ozon system, `product_id`.
  - `quant_size` (integer(uint64)) — Quant size.
  - `quant_type` (enum) — Values: `UNSPECIFIED, BOX, PALLET, GENERAL`
  - `sku` (array[string]) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/products/delete`

**Remove products from special offer**

Operation ID: `SellerActionsProductsDelete`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `skus` (array[string]) **(required)** — Product identifiers in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/products/list`

**Get list of products in special offer**

Operation ID: `SellerActionsProductsList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `cursor` (integer(uint64)) — Cursor for the next data sample.
- `limit` (integer(int64)) **(required)** — Maximum number of values in the response.

**Response 200:**

- `cursor` (integer(uint64)) — Cursor for the next data sample.
- `has_next` (boolean) — Indication that only part of values was returned in the response: - `true`: make a request with a new `cursor` parameter value for getting the rest of
- `products` (array[object]) — Product details.
  - `action_price` (number(double)) — Product price, including the special offer.
  - `base_price` (number(double)) — Base price at which the product is sold on Ozon if it doesn't participate in the special offer.
  - `currency` (string) — Currency.
  - `discount_percent` (number(double)) — Discount percentage.
  - `is_active` (boolean) — `true` if product participates in the special offer.
  - `min_seller_price` (number(double)) — Minimum price to automatically add product to the special offer.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `price` (number(double)) — Product price for customer.
  - `product_id` (integer(uint64)) — Product identifier in the Ozon system, `product_id`.
  - `quant_size` (integer(uint64)) — Quant size.
  - `quant_type` (enum) — Values: `UNSPECIFIED, BOX, PALLET, GENERAL`
  - `sku` (array[string]) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/archive`

**Archive special offer**

Operation ID: `SellerActionsArchive`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/change-activity`

**Enable or disable special offer**

Operation ID: `SellerActionsChangeActivity`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.
- `is_turn_on` (boolean) **(required)** — `true` to enable the special offer.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/list`

**Get list of special offers**

Operation ID: `SellerActionsList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_ids` (array[string]) — Special offer identifiers.
- `action_type` (array[object]) — Special offer mechanics:   - `DISCOUNT`: discount;   - `VOUCHER_DISCOUNT`: discount via promo code;   - `DISCOUNT_WITH_CONDITION`: discount from order
- `limit` (integer(uint64)) **(required)** — Number of values per page.
- `offset` (integer(uint64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `search` (string) — Search by special offer name.
- `status` (array[object]) — Special offer status.

**Response 200:**

- `actions` (array[object]) — List of special offers.
  - `action_id` (integer(uint64)) — Special offer identifier.
  - `action_parameters` (object)
  - `allow_delete` (boolean) — `true` if the special offer can be deleted.
  - `highlight_url` (string) — Highlight link.
  - `is_editable` (boolean) — `true` if the special offer can be edited.
  - `is_participated` (boolean) — `true` if at least one product is added to the special offer.
  - `is_turn_on` (boolean) — `true` if the special offer is enabled.
  - `sku_count` (integer(uint64)) — Total number of products in the special offer.
- `total` (integer(uint64)) — Total number of special offers.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller-actions/voucher/get`

**Get file with promo codes in CSV format**

Operation ID: `SellerActionsVoucherGet`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1872-Novye-metody-dlia-raboty-s-aktsiiami-sellera) in the Ozon for dev community.

**Request body:**

- `action_id` (integer(uint64)) **(required)** — Special offer identifier. Get the parameter value using the [/v1/seller-actions/list](#operation/SellerActionsList) method.

**Response 200:**

- `file` (string) — Link to the CSV file with promo codes.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## Notification

### `POST /v1/notification/set`

**Connect URL for notifications**

Operation ID: `SetNotification`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Request body:**

- `types` (array[object]) **(required)** — Notification types:  - `TYPE_NEW_MESSAGE`: new message in the chat; - `TYPE_UPDATE_MESSAGE`: message in the chat is edited; - `TYPE_MESSAGE_READ`: cus
- `url` (string) **(required)** — URL.

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

### `POST /v1/notification/update`

**Change URL for notifications**

Operation ID: `UpdateNotification`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Request body:**

- `id` (integer(int64)) **(required)** — URL identifier.
- `types` (array[object]) — Notification types:  - `TYPE_NEW_MESSAGE`: new message in the chat; - `TYPE_UPDATE_MESSAGE`: message in the chat is edited; - `TYPE_MESSAGE_READ`: cus
- `url` (string) — New URL.

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

### `POST /v1/notification/delete`

**Delete URL for notifications**

Operation ID: `DeleteNotification`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Request body:**

- `id` (integer(int64)) **(required)** — URL identifier.

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

### `POST /v1/notification/check`

**Check URL for notifications**

Operation ID: `CheckNotification`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Request body:**

- `url` (string) **(required)** — URL.

**Response 200:**

- `errors` (array[object]) — Verification errors.
  - `description` (string) — Error description.
  - `type` (enum) — Values: `REQUEST_ERROR, REQUEST_TIMEOUT, SERVER_FAULT, STATUS_CODE_NOT_OK, EMPTY_BODY, INVALID_BODY, INVALID_JSON, WRONG_RESULT_FIELD`
- `is_active` (boolean) **(required)** — `true` if the URL is active.

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

### `POST /v1/notification/enable`

**Enable or disable URL for notifications**

Operation ID: `EnableNotification`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Request body:**

- `enabled` (boolean) **(required)** — Pass:  - `true`: to enable notifications; - `false`: to disable notifications.
- `id` (integer(int64)) **(required)** — URL identifier.

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

### `POST /v1/notification/list`

**Get information about connected URLs**

Operation ID: `NotificationList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Response 200:**

- `urls` (array[object]) **(required)** — Connected URLs.
  - `created_at` (string(date-time)) **(required)** — URL connection date.
  - `enable` (boolean) **(required)** — `true` if the URL is enabled.
  - `id` (integer(int64)) **(required)** — URL identifier.
  - `types` (array[object]) **(required)** — Notification types.
  - `url` (string) **(required)** — URL.

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

### `POST /v1/notification/push-type/list`

**Get push notification types**

Operation ID: `GetNotificationPushTypeList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1978-Novye-beta-metody-dlia-upravleniia-podkliucheniiami-PUSH-uvedomlenii/) in the Ozon for dev community.

**Response 200:**

- `types` (array[object]) **(required)** — Push notification types.
  - `description` (string) **(required)** — Description.
  - `seller_endpoint` (object)
  - `type` (enum) **(required)** — Values: `TYPE_NEW_MESSAGE, TYPE_UPDATE_MESSAGE, TYPE_MESSAGE_READ, TYPE_CHAT_CLOSED, TYPE_NEW_POSTING, TYPE_POSTING_SHIPPED, TYPE_POSTING_CANCELLED, TYPE_STATE_CHANGED`

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

## DeliveryFBPDraft

### `POST /v1/fbp/warehouse/list`

**Get partner warehouses list**

Operation ID: `FbpWarehouseList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Response 200:**

- `warehouses` (array[object]) — List of warehouses.
  - `address_detailing` (object)
  - `id` (integer(int64)) — Warehouse identifier.
  - `is_bonded` (boolean) — `true` if warehouse is bonded.
  - `name` (string) — Warehouse name.
  - `partner_name` (string) — Partner's name.
  - `supply_types` (array[integer]) — Supply type.
  - `timezone_name` (string) — Warehouse time zone.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/get`

**Get information about supply draft**

Operation ID: `FbpAPI_FbpDraftGet`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `bundle_id` (string) — Identifier of the validated product list.
- `cancellation_state` (object)
  - `cancellation_error` (object)
  - `cancellation_status` (enum) — Values: `STATUS_UNSPECIFIED, CONFIRMATION, CANCELED, NOT_CANCELED`
- `created_at` (string(date-time)) — Date and time of draft creation.
- `decline_reason` (object)
  - `failed_sku_ids` (array[string]) — Invalid SKU identifiers.
  - `message` (string) — Message content.
- `deleted_at` (string(date-time)) — Date and time of deletion.
- `delivery_details` (object)
  - `direct_details` (object)
  - `drop_off_point` (object)
  - `pickup_details` (object)
  - `supply_type` (enum) — Values: `SUPPLY_TYPE_UNSPECIFIED, DIRECT_BY_SELLER, DIRECT_BY_TPL, DROP_OFF, PICK_UP`
- `editable` (boolean) — `true`, if draft is editable.
- `id` (integer(int64)) — Draft identifier.
- `is_cancelable` (boolean) — `true`, if draft is cancelable.
- `is_deletable` (boolean) — `true`, if draft is deletable.
- `is_registration_available` (boolean) — `true`, if registration is available.
- `locked` (boolean) — `true`, if draft is locked.
- `package_units_count` (integer(int32)) — Number of package units.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `status` (enum) — Values: `DRAFT_STATUS_UNSPECIFIED, NEW, SUPPLY_VARIANT_CONFIRMATION, SUPPLY_NOT_CONFIRMED`
- `supply_id` (string) — Supply identifier.
- `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/list`

**Supply drafts list**

Operation ID: `FbpAPI_FbpDraftList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `count` (integer(int32)) **(required)** — Number of elements in the response.
- `last_id` (integer(int64)) — Identifier of the last value on the page.  To get the next values, specify the recieved value in the next request in the `last_id` parameter.

**Response 200:**

- `has_next` (boolean) — `true`, if not all elements were returned in the response.
- `items` (array[object]) — Drafts.
  - `bundle_id` (string) — Identifier of the validated product list.
  - `cancellation_state` (object)
  - `created_at` (string(date-time)) — Date and time of draft creation.
  - `deleted_at` (string(date-time)) — Date and time of deletion.
  - `delivery_details` (object)
  - `editable` (boolean) — `true`, if draft is editable.
  - `id` (integer(int64)) — Draft identifier.
  - `is_cancelable` (boolean) — `true`, if draft is cancelable.
  - `is_deletable` (boolean) — `true`, if draft is deletable.
  - `locked` (boolean) — `true`, if draft is locked.
  - `package_units_count` (integer(int32)) — Number of package units.
  - `status` (enum) — Values: `DRAFT_STATUS_UNSPECIFIED, NEW, SUPPLY_VARIANT_CONFIRMATION, SUPPLY_NOT_CONFIRMED`
  - `supply_id` (string) — Supply identifier.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
- `last_id` (integer(int64)) — Identifier of the last value on the page.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DraftDirectFBP

### `POST /v1/fbp/draft/direct/seller-dlv/create`

**Create draft with delivery by seller**

Operation ID: `FbpDraftDirectSellerDlvCreate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `bundle_id` (string) **(required)** — Identifier of the validated product list.
- `delivery_details` (object) **(required)**
  - `driver_name` (string) **(required)** — Driver's full name.
  - `timeslot_start` (string(date-time)) **(required)** — Start of the time slot.
  - `vehicle_number` (string) **(required)** — Vehicle number.
  - `vehicle_type` (string) **(required)** — Vehicle type.
- `package_units_count` (integer(int32)) **(required)** — Number of package units.
- `warehouse_id` (integer(int64)) **(required)** — Seller warehouse identifier.

**Response 200:**

- `draft_id` (integer(int64)) — Draft identifier.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `supply_id` (string) — Supply request identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/seller-dlv/edit`

**Update draft with information about delivery by seller**

Operation ID: `FbpDraftDirectSellerDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `driver_name` (string) **(required)** — Driver's full name.
- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply request identifier.
- `vehicle_number` (string) **(required)** — Vehicle number.
- `vehicle_type` (string) **(required)** — Vehicle type.

**Response 200:**

- `error` (object)
  - `errors` (array[object]) — Error type:  - `ERROR_TYPE_UNSPECIFIED`: undefined;  - `ORDER_DRAFT_LOCKED`: draft is blocked;  - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: drive
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/timeslot/edit`

**Edit time slot in draft**

Operation ID: `FbpDraftDirectTimeslotEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply request identifier.
- `timeslot_start` (string(date-time)) **(required)** — Start of the time slot.

**Response 200:**

- `error_reasons` (array[object]) — Error reason:   - `RESERVE_FAILURE_TYPE_UNSPECIFIED`: undefined;   - `REQUEST_VALIDATION`: reservation date in the request is in the past;   - `INVALI
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/timeslot/get`

**Get list of time slots for direct supply**

Operation ID: `FbpDraftDirectGetTimeslot`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `bundle_id` (string) **(required)** — Identifier of the validated product list.
- `interval_end` (string(date-time)) **(required)** — End date of the required time slot period.
- `interval_start` (string(date-time)) **(required)** — Start date of the required time slot period.
- `warehouse_id` (integer(int64)) **(required)** — Seller warehouse identifier.

**Response 200:**

- `reasons` (array[object]) — Reasons for the lack of time slots: - `EMPTY_TIMESLOTS_REASON_UNSPECIFIED`: undefined; - `LOGISTICS_UNKNOWN`: unknown logistics error; - `NO_ROUTE`: n
- `timeslots` (array[object]) — List of available time slots.
  - `timeslot_end` (string(date-time)) — End date of the time slot.
  - `timeslot_start` (string(date-time)) — Start date of the time slot.
- `warehouse_timezone_name` (string) — Time zone of the seller's warehouse.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/create`

**Create supply request draft without specifying delivery method**

Operation ID: `FbpDraftDirectCreate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `bundle_id` (string) **(required)** — Identifier of the validated product list. To get it, use the [/v1/fbp/draft/direct/product/validate](#operation/FbpDraftDirectProductValidate) method.
- `delivery_details` (object) **(required)**
  - `timeslot_start` (array[string]) **(required)** — Start of delivery time slot.
- `package_units_count` (integer(int32)) **(required)** — Number of items per package.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `draft_id` (integer(int64)) — Draft identifier.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `supply_id` (string) — Supply identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/delete`

**Delete draft of supply request**

Operation ID: `FbpDraftDirectDelete`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `cancellation_state` (object)
  - `cancellation_error` (object)
  - `cancellation_status` (enum) — Values: `STATUS_UNSPECIFIED, CONFIRMATION, CANCELED, NOT_CANCELED`
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/product/validate`

**Validate product list for partner warehouse**

Operation ID: `FbpDraftDirectProductValidate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `skus` (array[object]) **(required)** — Product identifiers in the Ozon system, SKU.
  - `count` (integer(int64)) **(required)** — Number of product items per supply.
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `approved_items` (array[object]) — Confirmed products.
  - `barcode` (string) — Barcode.
  - `icon_name` (string) — Рroduct image link.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product article code in seller system.
  - `quantity` (integer(int32)) — Product quantity.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `volume` (number(double)) — Product volume.
- `bundle_generated` (boolean) — `true` if the validated product list is created.
- `bundle_id` (string) — Identifier of the validated product list.
- `rejected_items` (array[object]) — Rejected products.
  - `barcode` (string) — Barcode.
  - `icon_name` (string) — Рroduct image link.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product article code in seller system.
  - `quantity` (integer(int32)) — Product quantity.
  - `rejection_reasons` (array[object]) — Rejection reasons:    - `BUNDLE_ITEM_ERROR_UNSPECIFIED`: undefined;    - `OUT_OF_ASSORTMENT`: product isn't found;    - `INVALID`: product isn't creat
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `volume` (number(double)) — Product volume.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/registrate`

**Transfer draft to current supply**

Operation ID: `FbpDraftDirectRegistrate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `error` (object)
  - `bundle_errors` (array[object]) — Errors of the validated product list.
  - `order_error` (enum) — Values: `ORDER_ERROR_TYPE_UNSPECIFIED, INVALID_NUMBER_OF_PACKAGE_UNITS, MAXIMUM_NUMBER_OF_UNIQUE_SKU_REACHED, MAXIMUM_BUNDLE_VOLUME_REACHED, BUNDLE_ID_EMPTY, INVALID_SUPPLY_TYPE, INVALID_TIMESLOT, INVALID_WHC_NUMBER`
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/tpl-dlv/create`

**Create supply request for delivery by a third-party transport company**

Operation ID: `FbpAPI_FbpDraftDirectTplDlvCreate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `bundle_id` (string) **(required)** — Identifier of the validated product list.
- `delivery_details` (object) **(required)**
  - `timeslot_start` (string(date-time)) **(required)** — Start of delivery time slot.
  - `tracking_number` (string) **(required)** — Shipment tracking number.
  - `transport_company_name` (string) **(required)** — Logistics provider name.
- `package_units_count` (integer(int32)) **(required)** — Number of package units.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `draft_id` (integer(int64)) — Draft identifier.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `supply_id` (string) — Supply identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/direct/tpl-dlv/edit`

**Edit a draft of shipment with a third-party transport company**

Operation ID: `FbpAPI_FbpDraftDirectTplDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply identifier.
- `tracking_number` (string) **(required)** — Shipment tracking number.
- `transport_company_name` (string) **(required)** — Logistics provider name.

**Response 200:**

- `error` (object)
  - `errors` (array[object]) — Error type:  - `ERROR_TYPE_UNSPECIFIED`: undefined;  - `ORDER_DRAFT_LOCKED`: draft is blocked;  - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: drive
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DraftDropOffFBP

### `POST /v1/fbp/draft/drop-off/province/list`

**Get province list**

Operation ID: `FbpDraftDropOffProvinceList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `provinces` (array[object]) — Province list.
  - `name` (string) — Province name.
  - `points_count` (integer(int32)) — Number of points on the map.
  - `province_uuid` (string) — Unique province identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/point/list`

**Get list of drop-off points in province**

Operation ID: `FbpDraftDropOffPointList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `next_page_number` (integer(int32)) — Next page number.
- `page_size` (integer(int32)) **(required)** — Number of elements on the page.
- `province_uuid` (string) **(required)** — Unique province identifier.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `drop_off_points` (array[object]) — List of drop-off points.
  - `city` (string) — City.
  - `drop_off_point_id` (integer(int64)) — Drop-off point identifier.
  - `nearest_drop_off_date` (string(date-time)) — Nearest shipping date.
  - `point_address` (string) — Drop-off point address.
  - `province_uuid` (string) — Unique province identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/point/timetable`

**Get drop-off point schedule**

Operation ID: `FbpDraftDropOffPointTimetable`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `drop_off_point_id` (integer(int64)) **(required)** — Drop-off point identifier.
- `province_uuid` (string) **(required)** — Unique province identifier.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `calendar` (array[object]) — Work schedule of the drop-off point.
  - `calendar_item` (object)
  - `day_of_week` (enum) — Values: `DAY_OF_WEEK_UNSPECIFIED, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/product/validate`

**Check product list that partner's warehouse can accept**

Operation ID: `FbpDraftDropOffProductValidate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `skus` (array[object]) **(required)** — Product identifiers in the Ozon system, SKU.
  - `count` (integer(int32)) **(required)** — Quantity.
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `approved_items` (array[object]) — Accepted products.
  - `barcode` (string) — Product barcode.
  - `icon_name` (string) — Product image link.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `quantity` (integer(int32)) — Product quantity.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `volume` (number(double)) — Product volume.
- `bundle_generated` (boolean) — `true` if bundle is created.
- `bundle_id` (string) — Identifier of the validated product list.
- `rejected_items` (array[object]) — Rejected products.
  - `barcode` (string) — Product barcode.
  - `icon_name` (string) — Product image link.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `quantity` (integer(int32)) — Product quantity.
  - `rejection_reasons` (array[object]) — Rejection reasons: - `BUNDLE_ITEM_ERROR_UNSPECIFIED`: undefined; - `OUT_OF_ASSORTMENT`: product is out of the supply range; - `INVALID`: invalid statu
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `volume` (number(double)) — Product volume.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/create`

**Create draft for delivery to drop-off point**

Operation ID: `FbpDraftDropOffCreate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `bundle_id` (string) **(required)** — Identifier of the validated product list.
- `delivery_details` (object) **(required)**
  - `drop_off_date` (string) **(required)** — Delivery date.
  - `drop_off_point_id` (integer(int64)) **(required)** — Drop-off point identifier.
  - `drop_off_province_uuid` (string) **(required)** — Unique province identifier.
- `package_units_count` (integer(int32)) **(required)** — Number of package units.
- `warehouse_id` (integer(int64)) **(required)** — Seller warehouse identifier.

**Response 200:**

- `draft_id` (integer(int64)) — Draft identifier.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `supply_id` (string) — Supply request identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/delete`

**Delete draft for delivery to drop-off point**

Operation ID: `FbpDraftDropOffDelete`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `cancellation_state` (object)
  - `cancellation_error` (object)
  - `cancellation_status` (enum) — Values: `STATUS_UNSPECIFIED, CONFIRMATION, CANCELED, NOT_CANCELED`
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/dlv/edit`

**Edit delivery details for drop-off draft**

Operation ID: `FbpDraftDropOffDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `drop_off_date` (string) **(required)** — Delivery date.
- `drop_off_point_id` (integer(int64)) **(required)** — Drop-off point identifier.
- `drop_off_province_uuid` (string) **(required)** — Unique province identifier.
- `row_version` (integer(int64)) **(required)** — Draft identifier.
- `supply_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/drop-off/registrate`

**Transfer draft to current supply**

Operation ID: `FbpDraftDropOffRegistrate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `error` (object)
  - `bundle_errors` (array[object]) — Errors of the validated product list.
  - `order_error` (enum) — Values: `ORDER_ERROR_TYPE_UNSPECIFIED, INVALID_NUMBER_OF_PACKAGE_UNITS, MAXIMUM_NUMBER_OF_UNIQUE_SKU_REACHED, MAXIMUM_BUNDLE_VOLUME_REACHED, BUNDLE_ID_EMPTY, INVALID_SUPPLY_TYPE, INVALID_TIMESLOT, INVALID_WHC_NUMBER`
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DraftPickupFBP

### `POST /v1/fbp/draft/pick-up/registrate`

**Transfer draft to current supply**

Operation ID: `FbpDraftPickUpRegistrate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `error` (object)
  - `bundle_errors` (array[object]) — Errors of the validated product list.
  - `order_error` (enum) — Values: `ORDER_ERROR_TYPE_UNSPECIFIED, INVALID_NUMBER_OF_PACKAGE_UNITS, MAXIMUM_NUMBER_OF_UNIQUE_SKU_REACHED, MAXIMUM_BUNDLE_VOLUME_REACHED, BUNDLE_ID_EMPTY, INVALID_SUPPLY_TYPE, INVALID_TIMESLOT, INVALID_WHC_NUMBER`
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/pick-up/create`

**Create request draft for pick-up supply**

Operation ID: `FbpAPI_FbpDraftPickupCreate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `bundle_id` (string) **(required)** — Supply contents identifier.
- `delivery_details` (object) **(required)**
  - `address` (string) **(required)** — Address.
  - `comment` (string) **(required)** — Comment.
  - `date` (string(date-time)) **(required)** — Delivery date.
  - `sender_name` (string) **(required)** — Sender's full name.
  - `sender_phone` (string) **(required)** — Sender's phone number.
- `package_units_count` (integer(int32)) **(required)** — Number of package units.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `draft_id` (integer(int64)) — Identifier of the supply request draft.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `supply_id` (string) — Supply identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/pick-up/delete`

**Cancel request draft for pick-up supply**

Operation ID: `FbpAPI_FbpDraftPickUpDelete`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `cancellation_state` (object)
  - `cancellation_error` (object)
  - `cancellation_status` (enum) — Values: `STATUS_UNSPECIFIED, CONFIRMATION, CANCELED, NOT_CANCELED`
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/pick-up/dlv/edit`

**Update request draft for pick-up supply**

Operation ID: `FbpAPI_FbpDraftPickupDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `pickup_details` (object) **(required)**
  - `address` (string) **(required)** — Address.
  - `comment` (string) **(required)** — Comment.
  - `date` (string(date-time)) **(required)** — Delivery date.
  - `sender_name` (string) **(required)** — Sender's full name.
  - `sender_phone` (string) **(required)** — Sender's phone number.
- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/draft/pick-up/product/validate`

**Validate product list for pick-up supply**

Operation ID: `FbpAPI_FbpDraftPickUpProductValidate`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `skus` (array[object]) **(required)** — List of product SKUs.
  - `count` (integer(int32)) **(required)** — Number of product items per supply.
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `approved_items` (array[object]) — Confirmed products.
  - `barcode` (string) — Barcode.
  - `icon_name` (string) — Рroduct image link.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product article code in seller system.
  - `quantity` (integer(int32)) — Product quantity.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `volume` (number(double)) — Product volume.
- `bundle_generated` (boolean) — `true` if the validated product list is created.
- `bundle_id` (string) — Identifier of the validated product list.
- `rejected_items` (array[object]) — Rejected products.
  - `barcode` (string) — Barcode.
  - `icon_name` (string) — Рroduct image link.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product article code in seller system.
  - `quantity` (integer(int32)) — Product quantity.
  - `rejection_reasons` (array[object]) — Rejection reasons:    - `BUNDLE_ITEM_ERROR_UNSPECIFIED`: undefined;    - `OUT_OF_ASSORTMENT`: product isn't found;    - `INVALID`: product isn't creat
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `volume` (number(double)) — Product volume.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## OrderDirectFBP

### `POST /v1/fbp/order/direct/cancel`

**Cancel supply**

Operation ID: `FbpAPI_FbpOrderDirectCancel`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `error` (object)
  - `order_errors` (array[object]) — Error type: - `ERROR_TYPE_UNSPECIFIED`: undefined; - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: driver's full name length exceeds the limit; - `DE
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/direct/seller-dlv/edit`

**Update information about delivery by seller**

Operation ID: `FbpAPI_FbpOrderDirectSellerDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `driver_name` (string) **(required)** — Driver's full name.
- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply request identifier.
- `vehicle_number` (string) **(required)** — Vehicle number.
- `vehicle_type` (string) **(required)** — Vehicle type.

**Response 200:**

- `error` (object)
  - `order_errors` (array[object]) — Error type: - `ERROR_TYPE_UNSPECIFIED`: undefined; - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: driver's full name length exceeds the limit; - `DE
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/direct/timeslot/edit`

**Edit time slot in supply request**

Operation ID: `FbpAPI_FbpEditTimeslot`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply request identifier.
- `timeslot_start` (string(date-time)) **(required)** — Start of the time slot.

**Response 200:**

- `error_reasons` (array[object]) — Error reason: - `RESERVE_FAILURE_TYPE_UNSPECIFIED`: undefined; - `REQUEST_VALIDATION`: reservation date in the request is in the past; - `INVALID_RESE
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/direct/timeslot/list`

**Get list of time slots for supply**

Operation ID: `FbpAPI_FbpAvailableTimeslotList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `interval_end` (string(date-time)) **(required)** — End date of the required time slot period.
- `interval_start` (string(date-time)) **(required)** — Start date of the required time slot period.
- `supply_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `reasons` (array[object]) — Reasons for the lack of time slots: - `EMPTY_TIMESLOTS_REASON_UNSPECIFIED`: undefined; - `LOGISTICS_UNKNOWN`: unknown logistics error; - `NO_ROUTE`: n
- `timeslots` (array[object]) — List of available time slots.
  - `timeslot_end` (string(date-time)) — End date of the time slot.
  - `timeslot_start` (string(date-time)) — Start date of the time slot.
- `warehouse_timezone_name` (string) — Time zone of the seller's warehouse.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## OrderDropOffFBP

### `POST /v1/fbp/order/drop-off/cancel`

**Cancel supply to drop-off point**

Operation ID: `FbpAPI_FbpOrderDropOffCancel`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `error` (object)
  - `order_errors` (array[object]) — Error type: - `ERROR_TYPE_UNSPECIFIED`: undefined; - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: driver's full name length exceeds the limit; - `DE
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/drop-off/dlv/edit`

**Edit information about supply to drop-off point**

Operation ID: `FbpAPI_FbpOrderDropOffDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `drop_off_date` (string) **(required)** — Date of delivery to drop-off point.
- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/drop-off/timetable`

**Get drop-off point working schedule**

Operation ID: `FbpAPI_FbpOrderDropOffTimetable`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `drop_off_point_id` (integer(int64)) **(required)** — Drop-off point identifier.
- `province_uuid` (string) **(required)** — Unique province identifier.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `calendar` (array[object]) — Work schedule of the drop-off point.
  - `calendar_item` (object)
  - `day_of_week` (enum) — Values: `DAY_OF_WEEK_UNSPECIFIED, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## OrderPickupFBP

### `POST /v1/fbp/order/pick-up/cancel`

**Cancel pick-up supply**

Operation ID: `FbpAPI_FbpOrderPickUpCancel`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `error` (object)
  - `order_errors` (array[object]) — Error type: - `ERROR_TYPE_UNSPECIFIED`: undefined; - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: driver's full name length exceeds the limit; - `DE
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/pick-up/dlv/edit`

**Edit pick-up point details**

Operation ID: `FbpAPI_FbpOrderPickUpDlvEdit`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `pickup_details` (object) **(required)**
  - `sender_name` (string) **(required)** — Sender's full name.
  - `sender_phone` (string) **(required)** — Sender's phone number.
- `row_version` (integer(int64)) **(required)** — Identifier of the current draft version.
- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `error` (object)
  - `order_errors` (array[object]) — Error type: - `ERROR_TYPE_UNSPECIFIED`: undefined; - `DELIVERY_DRIVER_NAME_LENGTH_MAXIMUM_REACHED`: driver's full name length exceeds the limit; - `DE
- `is_error` (boolean) — `true` if there is an error.
- `row_version` (integer(int64)) — Identifier of the current draft version.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DeliveryFBP

### `POST /v1/fbp/act-from/create`

**Generate acceptance certificate**

Operation ID: `FbpAPI_FbpCreateAct`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `errors` (array[object]) — Error reason: - `CREATE_ACT_ERROR_REASON_UNSPECIFIED`: undefined; - `INVALID_ORDER_TYPE`: can't create certificate for specified identifier.
- `file_uuid` (string) — Acceptance certificate identifier.
- `is_success` (boolean) — `true` if the request doesn't contain errors.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/act-from/get`

**Get status of acceptance certificate generation**

Operation ID: `FbpAPI_FbpCheckActState`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `file_uuid` (string) **(required)** — Acceptance certificate identifier.

**Response 200:**

- `cdn_url` (string) — Link to acceptance certificate.
- `error` (enum) — Values: `ERROR_REASON_UNSPECIFIED, INVALID_COMPANY, FILE_NOT_FOUND, GENERATE_TIMEOUT_REACHED, GENERATION_ERROR`
- `status` (enum) — Values: `STATUS_UNSPECIFIED, NOT_EXIST, PROCESSING, EXIST, ERROR`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/act-to/create`

**Generate waybill**

Operation ID: `FbpAPI_FbpCreateConsignmentNote`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `code` (string) — Waybill identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/act-to/get`

**Get status of waybill generation**

Operation ID: `FbpAPI_FbpCheckConsignmentNoteState`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `code` (string) **(required)** — Waybill identifier.
- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `error_message` (string) — Error description.
- `label_url` (string) — Link to supply labels.
- `state` (enum) — Values: `STATE_TYPE_UNSPECIFIED, IN_PROGRESS, FINISHED, FAILED`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/archive/get`

**Get details on completed supply**

Operation ID: `FbpAPI_FbpArchiveGet`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `act_file_uuid` (string) — Acceptance certificate identifier.
- `bundle_id` (string) — Identifier of the validated product list.
- `bundle_sku_summary` (object)
  - `rounded_total_volume_in_litres` (number(double)) — Total volume of products in liters.
  - `total_items_count` (integer(int64)) — Quantity of SKUs in the supply.
  - `total_quantity` (integer(int64)) — Quantity of product items in the supply.
- `business_flow_type_id` (integer(int64)) — Supply type identifier.
- `created_date` (string(date-time)) — Date and time of the supply request creation.
- `decline_reason` (object)
  - `code` (enum) — Values: `DECLINE_REASON_CODE_UNSPECIFIED, CANNOT_CREATE_SUPPLY_ON_TPF, DROP_OFF_POINT_CLOSED, CODE_SUPPLY_LOST, COURIER_PICK_UP_REJECTED_BY_SELLER, BONDED_DOCUMENTS_REJECTED_BY_WAREHOUSE`. Code of the supply rejection reason:  - `DECLINE_REASON_CODE_UNSPECIFIED`: undefined;  - `CANNOT_CREATE_SUPPLY_ON_TPF`: failed to create a supply on t
  - `message` (string) — Rejection reason description.
- `delivery_details` (object)
  - `direct_details` (object)
  - `drop_off_point` (object)
  - `pickup_details` (object)
  - `supply_type` (enum) — Values: `SUPPLY_TYPE_UNSPECIFIED, DIRECT_BY_SELLER, DIRECT_BY_TPL, DROP_OFF, PICK_UP`
- `has_act` (boolean) — `true` if an acceptance certificate was generated.
- `has_label` (boolean) — `true` if labels were generated.
- `id` (integer(int64)) — Archive record number.
- `order_draft_id` (integer(int64)) — Supply draft identifier.
- `order_number` (string) — Completed supply identifier.
- `package_units_count` (integer(int32)) — Number of package units.
- `receive_date` (string(date-time)) — Date and time of the supply acceptance.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `status` (enum) — Values: `ARCHIVE_STATUS_UNSPECIFIED, COMPLETED, REJECTED_AT_SUPPLY_WAREHOUSE, CANCELLED_BY_SELLER`
- `supply_id` (string) — Supply identifier.
- `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/archive/list`

**Get list of completed supplies**

Operation ID: `FbpAPI_FbpArchiveList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `count` (string(int32)) **(required)** — Number of items in the response.
- `last_id` (string(int64)) — Identifier of the last value on the page. Leave this field empty for the initial request.  To get the next values, specify the `last_id` from the prev

**Response 200:**

- `has_next` (boolean) — `true` if not all values are returned in the response.
- `items` (array[object]) — Completed supplies.
  - `act_file_uuid` (string) — Acceptance certificate identifier.
  - `bundle_id` (string) — Identifier of the validated product list.
  - `bundle_sku_summary` (object)
  - `created_date` (string(date-time)) — Creation date of the supply request.
  - `decline_reason` (object)
  - `delivery_details` (object)
  - `external_order_id` (string) — Completed supply identifier at the partner warehouse with its own inventory system.
  - `has_act` (boolean) — `true` if an acceptance certificate was generated.
  - `has_label` (boolean) — `true` if labels were generated.
  - `order_draft_id` (integer(int64)) — Supply draft identifier.
  - `package_units_count` (integer(int32)) — Number of package units.
  - `receive_date` (string(date-time)) — Date and time of the supply acceptance.
  - `row_version` (integer(int64)) — Identifier of the current draft version.
  - `status` (enum) — Values: `ARCHIVE_STATUS_UNSPECIFIED, COMPLETED, REJECTED_AT_SUPPLY_WAREHOUSE, CANCELLED_BY_SELLER`
  - `supply_id` (string) — Supply identifier.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
  - `whc_order_id` (integer(int64)) — Completed supply identifier at the partner warehouse.
- `last_id` (integer(int64)) — Creation date of the supply request.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/label/create`

**Create label generation task**

Operation ID: `FbpAPI_FbpCreateLabel`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `code` (string) — Identifier of the label generation task.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/label/get`

**Get status of label generation task**

Operation ID: `FbpAPI_FbpGetLabel`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `code` (string) **(required)** — Identifier of the label generation task.
- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `label_url` (string) — Link to supply labels.
- `state` (enum) — Values: `UNSPECIFIED, IN_PROGRESS, FINISHED, FAILED`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/get`

**Get information about supply**

Operation ID: `FbpAPI_FbpOrderGet`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `supply_id` (string) **(required)** — Supply identifier.

**Response 200:**

- `attention_reasons` (array[object]) — Warning reasons: - `ORDER_ATTENTION_TYPE_UNSPECIFIED`: undefined; - `OLD`: expired request; - `TIME_SLOT_EXPIRED`: expired time slot.
- `bundle_uuid` (string) — Identifier of the validated product list.
- `can_be_cancelled` (boolean) — `true` if the supply request can be canceled.
- `cancellation_state` (object)
  - `cancellation_error` (object)
  - `cancellation_status` (enum) — Values: `STATUS_UNSPECIFIED, CONFIRMATION, CANCELED, NOT_CANCELED`
- `created_date` (string(date-time)) — Creation date of the supply request.
- `delivery_details` (object)
  - `direct_details` (object)
  - `drop_off_point` (object)
  - `pickup_details` (object)
  - `supply_type` (enum) — Values: `SUPPLY_TYPE_UNSPECIFIED, DIRECT_BY_SELLER, DIRECT_BY_TPL, DROP_OFF, PICK_UP`
- `draft_id` (integer(int64)) — Draft identifier.
- `has_consignment_note` (boolean) — `true` if order has consignment note.
- `has_label` (boolean) — `true` if order has labels.
- `id` (integer(int64)) — Supply request identifier.
- `locked` (boolean) — `true` if supply is locked from editing.
- `order_number` (string) — Order number.
- `package_units_count` (integer(int32)) — Number of package units.
- `receive_date` (string(date-time)) — Date and time of the supply acceptance.
- `row_version` (integer(int64)) — Identifier of the current draft version.
- `status` (enum) — Values: `ORDER_STATUS_UNSPECIFIED, READY_TO_SUPPLY, FILLING_DELIVERY_DETAILS, COURIER_ASSIGNED, COURIER_PICKED_UP, ACCEPTANCE_AT_DROP_OFF_POINT, IN_TRANSIT_TO_STORAGE_WAREHOUSE, ACCEPTANCE_AT_STORAGE_WAREHOUSE`
- `supply_id` (string) — Supply identifier.
- `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbp/order/list`

**Get list of supplies**

Operation ID: `FbpAPI_FbpOrderList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/1700-FBP-metody/) in the Ozon for dev community.

**Request body:**

- `count` (integer(int32)) **(required)** — Number of supplies in the response.
- `last_id` (integer(int64)) — Identifier of the last supply on the page. Leave this field blank in the first request.  To get the next values, specify `id` of the last supply from 

**Response 200:**

- `has_next` (boolean) — `true` if not all supplies were returned in the response.
- `items` (array[object]) — Supplies.
  - `attention_reasons` (array[object]) — Warning reasons: - `ORDER_ATTENTION_TYPE_UNSPECIFIED`: undefined; - `OLD`: expired request; - `TIME_SLOT_EXPIRED`: expired time slot.
  - `bundle_summary` (object)
  - `can_be_cancelled` (boolean) — `true` if the supply request can be canceled.
  - `cancellation_state` (object)
  - `created_date` (string(date-time)) — Creation date of the supply request.
  - `delivery_details` (object)
  - `has_consignment_note` (boolean) — `true` if order has consignment note.
  - `has_label` (boolean) — `true` if order has labels.
  - `id` (integer(int64)) — Supply request identifier.
  - `locked` (boolean) — `true` if supply is locked from editing.
  - `order_number` (string) — Order number.
  - `package_units_count` (integer(int32)) — Number of package units.
  - `receive_date` (string(date-time)) — Date and time of the supply acceptance.
  - `status` (enum) — Values: `ORDER_STATUS_UNSPECIFIED, READY_TO_SUPPLY, FILLING_DELIVERY_DETAILS, COURIER_ASSIGNED, COURIER_PICKED_UP, ACCEPTANCE_AT_DROP_OFF_POINT, IN_TRANSIT_TO_STORAGE_WAREHOUSE, ACCEPTANCE_AT_STORAGE_WAREHOUSE`
  - `supply_id` (string) — Supply identifier.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
- `last_id` (integer(int64)) — Identifier of the last order on the page.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/fbp/list`

**Get shipment list**

Operation ID: `PostingFbpList`

You can leave feedback on this method in the comments section to the [discussion](https://dev.ozon.ru/community/2054-Novyi-beta-metod-dlia-raboty-s-FBP-postingami-v-Seller-API/ ) in the Ozon for dev community.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller system.
  - `posting_numbers` (array[string]) — Shipment numbers.
  - `since` (string(date-time)) — Start date.
  - `statuses` (array[string]) — Shipment status.
  - `to` (string(date-time)) — End date.
- `limit` (integer(int64)) — Number of values in the response.
- `sort_by` (string) — Parameter by which shipments are sorted:  - `last_change_status_date`: date of the last status change; - `in_process_at`: processing start date.
- `sort_dir` (enum) — Values: `ASC, DESC`

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `postings` (array[object]) — Shipment list.
  - `financial_data` (object)
  - `in_process_at` (string(date-time)) — Start date and time of shipment processing.
  - `order_date` (string(date-time)) — Order creation date.
  - `order_id` (integer(int64)) — Order identifier to which the shipment belongs.
  - `order_number` (string) — Order number to which the shipment belongs.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — List of products in the shipment.
  - `provider_id` (integer(int64)) — Delivery service identifier.
  - `status` (string) — Shipment status.

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

