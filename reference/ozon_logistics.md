# Ozon Logistics

## OzonLogistics

Methods in the Ozon Logistics section require an OAuth token in a private or public application.

## CancelReasonAPI

### `POST /v1/cancel-reason/list`

**Cancellation reasons for shipments**

Operation ID: `CancelReasonList`

Returns possible cancellation reasons for shipments and orders.

**Response 200:**

- `reasons` (array[object]) — Information about cancellation reasons.
  - `id` (integer(int64)) — Cancellation reason identifier:   - `501`: "Ozon has postponed the delivery date";   - `502`: "Some products from the order have been cancelled";   - 
  - `name` (string) — Cancellation reason.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/cancel-reason/list-by-order`

**Order cancellation reasons**

Operation ID: `CancelReasonListByOrder`

Returns possible cancellation reasons for orders.

**Request body:**

- `order_number` (string) **(required)** — Order number.

**Response 200:**

- `reasons` (array[object]) — Information about cancellation reasons.
  - `id` (integer(int64)) — Cancellation reason identifier:   - `501`: "Ozon has postponed the delivery date";   - `502`: "Some products from the order have been cancelled";   - 
  - `name` (string) — Cancellation reason.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/cancel-reason/list-by-posting`

**Cancellation reasons for a shipment**

Operation ID: `CancelReasonAPI_CancelReasonListByPosting`

Returns possible cancellation reasons for shipments.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `reasons` (array[object]) — Information about cancellation reasons.
  - `id` (integer(int64)) — Cancellation reason identifier:  - `501`: "Ozon has postponed the delivery date";  - `502`: "Some products from the order have been cancelled";  - `50
  - `name` (string) — Cancellation reason.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DeliveryAPI

### `POST /v1/delivery/check`

**Check delivery availability for customer**

Operation ID: `DeliveryCheck`

Checks availability of Ozon delivery for customers. It doesn't take into account purchase amount limits, product categories, or geographic restrictions.

**Request body:**

- `client_phone` (string) **(required)** — Customer phone number.

**Response 200:**

- `is_possible` (boolean) — `true` if delivery is available.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/delivery/checkout`

**Get available delivery options**

Operation ID: `DeliveryCheckout`

Checks availability of product delivery to a specified address or pick-up point and displays delivery times.

Check product availability and routes during checkout to calculate delivery times.

**Request body:**

- `buyer_phone` (string) — Customer phone number.
- `delivery_schema` (enum) — Values: `MIX, FBO, FBS`
- `delivery_type` (object)
  - `courier` (object)
    - `coordinates` (object)
  - `pick_up` (object)
    - `map_point_id` (integer(int64)) — Pick-up point identifier.
- `items` (array[object]) — Product information.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `quantity` (integer(int64)) — Product quantity.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response 200:**

- `splits` (array[object]) — Request result.
  - `delivery_method` (object)
  - `delivery_schema` (enum) — Values: `UNSPECIFIED, FBO, FBS`
  - `items` (array[object]) — Product information.
  - `unavailable_reason` (enum) — Values: `UNSPECIFIED, UNKNOWN, OUT_OF_STOCK, BANNED_FOR_AREA, BANNED_FOR_LEGAL, BANNED, BANNED_FOR_NOT_PREMIUM, DELIVERY_UNAVAILABLE`
  - `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/delivery/map`

**Display points on map**

Operation ID: `DeliveryMap`

Returns pick-up points grouped into clusters within the area specified by the `viewport` parameter. 

Use the values from the `clusters.viewport` parameter to get a list of points or smaller clusters within a large cluster.

To get information about a specific pick-up point, use the [/v1/delivery/point/info](#operation/DeliveryPointInfo) method.

**Request body:**

- `viewport` (object)
  - `left_bottom` (object)
    - `lat` (number(double)) — Latitude.
    - `long` (number(double)) — Longitude.
  - `right_top` (object)
    - `lat` (number(double)) — Latitude.
    - `long` (number(double)) — Longitude.
- `zoom` (integer(int32)) — Map scale.

**Response 200:**

- `clusters` (array[object]) — Clusters.
  - `coordinate` (object)
  - `is_same_building` (boolean) — `true` if all points are in the same building.
  - `map_point_ids` (array[string]) — Identifiers of map points.
  - `points_count` (integer(int32)) — Number of points in the cluster.
  - `viewport` (object)

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/delivery/point/info`

**Get pick-up point information**

Operation ID: `DeliveryPointInfo`

Returns information about pick-up points.

**Request body:**

- `map_point_ids` (array[string]) — Identifiers of map points.

**Response 200:**

- `points` (array[object]) — Pick-up point information.
  - `delivery_method` (object)
  - `enabled` (boolean) — `true` if the pick-up point is available.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/delivery/point/list`

**Get pick-up point list**

Operation ID: `DeliveryAPI_DeliveryPointList`

Returns coordinates of all pick-up points without grouping them into clusters.

**Request body:**

**Response 200:**

- `points` (array[object]) — Pick-up points.
  - `coordinate` (object)
  - `map_point_id` (integer(int64)) — Map point identifier.

---

## OrderAPI

### `POST /v1/order/cancel`

**Cancel order**

Operation ID: `OrderAPI_OrderCancel`

Cancels an order with all shipments. Use the cancellation reason identifier `reasons.id` from the [/v1/cancel-reason/list-by-order](#operation/CancelReasonListByOrder) method.

**Request body:**

- `order_number` (string) **(required)** — Order number.
- `reason_id` (integer(int32)) **(required)** — Order cancellation reason identifier.
- `reason_message` (string) — Order cancellation reason.

**Response 200:**

- `message` (string) — Cancellation processing status.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/order/cancel/check`

**Check if order can be canceled**

Operation ID: `OrderAPI_OrderCancelCheck`

Returns possibility for a customer to cancel an order.

**Request body:**

- `order_number` (string) **(required)** — Order number.

**Response 200:**

- `cancellable` (boolean) — `true` if order can be canceled.
- `order_number` (string) — Order number.
- `posting_groups` (array[object]) — Shipment groups.
  - `posting_numbers` (array[string]) — List of shipments in group.
- `postings` (array[object]) — Cancellation availability details.
  - `cancellable` (boolean) — `true` if shipment can be canceled.
  - `posting_number` (string) — Shipment identifier.
  - `why_not_cancellable` (string) — Reason why shipment can't be canceled.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/order/cancel/status`

**Get order cancellation status**

Operation ID: `OrderAPI_OrderCancelStatus`

**Request body:**

- `order_number` (string) **(required)** — Order number.

**Response 200:**

- `order_number` (string) — Order number.
- `posting_number` (array[string]) — List of shipments in order.
- `state` (string) — Order cancellation status.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/order/create`

**Create order**

Operation ID: `OrderAPI_OrderCreate`

Creates an order for a customer and recipient in the Ozon system. Pass the delivery option from the [/v2/delivery/checkout](#operation/DeliveryCheckout) method response.

The response may include not all shipments. Get the list of all shipments by the `order_number` parameter using the methods:

  - [/v2/posting/fbo/list](#operation/PostingAPI_GetFboPostingList): for the FBO scheme;
  - [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3): for the FBS scheme.

**Request body:**

- `buyer` (object) **(required)**
  - `first_name` (string) **(required)** — Name.
  - `last_name` (string) **(required)** — Surname.
  - `middle_name` (string) — Patronymic.
  - `phone` (string) **(required)** — Phone number.
- `delivery` (object) **(required)**
  - `courier` (object)
    - `apartment` (string) — Apartment number.
    - `city` (string) **(required)** — City.
    - `comment` (string) — Comment.
    - `coordinates` (object) **(required)**
    - `country` (string) **(required)** — Country.
    - `entrance` (string) — Entrance number.
    - `floor` (string) — Floor number.
    - `house_number` (string) **(required)** — Building number.
    - `intercom` (string) — Intercom code.
    - `region` (string) — Region.
    - `street` (string) — Street.
    - `zip_code` (string) — Index.
  - `pick_up` (object)
    - `map_point_id` (integer(int64)) **(required)** — Pick-up point identifier on map.
- `delivery_schema` (enum) **(required)** — Values: `MIX, FBO, FBS`
- `recipient` (object) **(required)**
  - `recipient_first_name` (string) **(required)** — Name.
  - `recipient_last_name` (string) **(required)** — Surname.
  - `recipient_middle_name` (string) — Patronymic.
  - `recipient_phone` (string) **(required)** — Phone number.
- `splits` (array[object]) **(required)** — Details on shipments in order.
  - `delivery_method` (object) **(required)**
    - `delivery_method_id` (integer(int64)) **(required)** — Delivery method identifier.
    - `delivery_type` (enum) **(required)** — Values: `COURIER, PVZ, POSTAMAT`
    - `logistic_date_range` (object) **(required)**
    - `price` (object)
    - `timeslot_id` (integer(int64)) **(required)** — Time slot identifier.
  - `items` (array[object]) **(required)** — Products in shipment.
    - `offer_id` (string) — Product identifier in the seller's system.
    - `price` (object) **(required)**
    - `quantity` (integer(int64)) **(required)** — Product quantity.
    - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
  - `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `order_number` (string) — Order number.
- `postings` (array[string]) — Shipments.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## FboPostingAPI

### `POST /v1/posting/cancel`

**Cancel shipment from order**

Operation ID: `PostingAPI_PostingCancel`

Cancels a shipment within an order. Use the cancellation reason identifier `reasons.id` from the [/v1/cancel-reason/list-by-posting](#operation/CancelReasonAPI_CancelReasonListByPosting) method.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.
- `reason_id` (integer(int32)) **(required)** — Cancellation reason identifier.
- `reason_message` (string) — Additional cancellation details.

**Response 200:**

- `message` (string) — Message.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/cancel/status`

**Check shipment cancellation status**

Operation ID: `PostingAPI_PostingCancelStatus`

**Request body:**

- `posting_number` (string) — Shipment identifier.

**Response 200:**

- `order_number` (string) — Order number.
- `posting_number` (array[string]) — Shipment identifier.
- `state` (string) — Shipment cancellation status.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/marks`

**Get item labels from shipment**

Operation ID: `PostingAPI_PostingMarks`

Returns item giveout statuses and "Chestny ZNAK" labeling codes for each shipment.

The method returns labeling codes of received items in the `issued_exemplars` parameter. Specify them in the receipt and remove them from circulation.

**Request body:**

- `posting_numbers` (array[string]) — Shipment identifiers.

**Response 200:**

- `invalid_postings` (array[string]) — List of invalid shipment identifiers.
- `issued_exemplars` (array[object]) — List of product items given to customers.
  - `exemplar_id` (integer(int64)) — Item identifier.
  - `mandatory_marks` (array[string]) — Label list of items given to customers.
  - `posting_number` (string) — Shipment identifier.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
- `non_issued_exemplars` (array[object]) — List of product items not given to customers.
  - `exemplar_id` (integer(int64)) — Item identifier.
  - `posting_number` (string) — Shipment identifier.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

