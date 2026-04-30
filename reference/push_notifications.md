# Push Notifications

## push_intro

The section describes how to turn push notifications on to get information about the following events from Ozon to your service:

- creating a new shipment,
- shipment cancellation,
- shipment status change,
- shipment delivery or shipping date change.

You can also get information about messages and notifications that hadn't been delivered because your service was unavailable.

## push_start

> **Note:** Your service should respond according to the REST API standards and with the error codes specified in the documentation.

If your service's responses differ from the required structure, notifications can be suspended.

**IP addresses from which notifications are sent**

- 195.34.21.0/24,
- 185.73.192.0/22,
- 91.223.93.0/24.

## Enable push notification

1. In your personal account, go to the [**Settings → Notifications**](https://seller.ozon.ru/app/settings/notifications-api) section.
2. On the **Push notifications** tab, enable push notifications.
3. Click **Enable**.
4. Enter the URL of the service to which notifications will be sent. For example, https://www.example.com/api/method.
5. Click **Check**. Ozon will send a [request](#section/Request-for-the-connection-check) to check the connection, to which your service should respond. If the connection is established, the message "This URL available for connection" will appear.
6. Click **Save**.
7. In the **Connection settings** section, in the **Notification types** drop-down list, select the necessary push notification types.
[Notifications descriptions](#tag/push_types)

You can disable notifications in the [**Settings → Notifications**](https://seller.ozon.ru/app/settings/notifications-api) section, on the **Push notifications** tab.

### Error codes when enabling push notifications

| Error                   | Description                                                                        | Solution                                                                                                                                 |
|-------------------------|------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| REQUEST_ERROR           | Failed to send the request as the service is unavailable at the specified address. | Make sure that your service is available.                                                                                                |
| REQUEST_TIMEOUT         | Request timed out.                                                                 | Increase the request timeout.                                                                                                            |
| SERVER_FAULT            | Your service returned an internal error.                                           | Check the server logs, update the server software, increase the allocated resources, or contact the server administrator.                |
| STATUS_CODE_NOT_OK      | HTTP service response status is other that 200.                                    | Check the passed status code.                                                                                                            |
| EMPTY_BODY              | Response body is empty.                                                            | Check that the response on the server is formed correctly and that the data is being passed.                                             |
| INVALID_BODY            | Incorrect response body format.                                                    | Check the response format and make sure that the `Content-Type` header is `application/json`.                                            |
| INVALID_JSON            | Failed to parse or validate JSON data.                                             | Make sure that the JSON data are correct and fix syntax errors.                                                                          |
| WRONG_RESULT_FIELD      | Your service's response is out of pattern.                                         | Make sure that the response format matches the template. 
 [Learn more about the template](#section/Request-for-the-connection-check) |
| WRONG_RESULT_TIME_FIELD | Invalid `time` field in response body.                                             | Check the time format in the response.                                                                                                   |

## Change service URL

1. In your personal account, go to the [**Settings → Integrations**](https://seller.ozon.ru/app/settings/integrations) section.
2. On the **Push notifications** tab, click **Edit**.
3. Enter the URL of the service to which notifications will be sent.
4. Click **Check**. Ozon will send a [request](#section/Request-for-the-connection-check) to check the connection, to which your service should respond. If the connection is established, the message "This URL available for connection" will appear.
5. Click **Save**.

## Request for the connection check

### Message that Ozon sends

```json5
{
   "message_type": "string",
   "time": "2019-08-24T14:15:22Z"
}
```

| Parameter | Type   | Format    | Description                                                 |
|------------------------------------------|--------|-----------|-------------------------------------------------------------|
| `message_type`                           | string | —         | Notification type: `TYPE_PING`.                             |
| `time`                                   | string | date-time | Date and time when the notification was sent in UTC format. |

### Your service response

#### If the notification was received successfully

If the notification is processed successfully, the service should respond with an HTTP 200 code:

```json5
{
   "version": "string",
   "name": "string",
   "time": "2019-08-24T14:15:22Z"
}
```

| Parameter | Type   | Format    | Description                                                       |
|------------------------------------------|--------|-----------|-------------------------------------------------------------------|
| `version`                                | string | —         | Application version.                                              |
| `name`                                   | string | —         | Application name.                                                 |
| `time`                                   | string | date-time | Date and time when notification processing started in UTC format. |

#### If an error occurs

If there is an error when processing notification, the service should respond with an HTTP code from 4xx or 5xx groups:

```json5
{
   "error": {
      "code": "ERROR_UNKNOWN",
      "message": "error",
      "details": null
   }
}
```

| Parameter | Type    | Format | Description                                                                                                                                                            |
|------------------------------------------|---------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `error`                                  | object  | —      | Information about the error.                                                                                                                                           |
| `code`                                   | string  | —      | Error code:
• `ERROR_UNKNOWN`.
• `ERROR_PARAMETER_VALUE_MISSED` — one or more parameters values are missing.
• `ERROR_REQUEST_DUPLICATED` — duplicate request. |
| `message`                                | string  | —      | Detailed error description.                                                                                                                                            |
| `details`                                | string  | —      | Additional information.                                                                                                                                                |

## push_resending

### Notification resending intervals

If the notification is not delivered, after a few seconds the system will resend the request several times.
The interval between attempts will gradually increase. 
When it reaches the maximum of 10 minutes, there will be 5 more attempts every 10 minutes.

### Automatic suspension of notifications

If the message still fails to be delivered, attempts to send the request will stop.

All notifications are paused if at least one condition is met:
- service is unavailable;
- service returns errors during the last 24 hours;
- less than half of all notifications are `200` responses;
- notification processing time is longer than 5 seconds.

To resume notifications, [confirm service URL](#tag/push_start) in your personal account.

## push_types

> **Note:** New order notification may be delayed.
To receive up-to-date information, periodically request the list of unprocessed shipments using the [POST /v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) method.

For each notification type, Ozon sends REST requests to your service address.
Your service should [respond](#tag/service_response) according to REST API standards.

| Type                                                                   | Purpose                                                                          |
|------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| [TYPE_PING](#section/Request-for-the-connection-check)                 | Checking if the service is ready at initial connection and periodically after it |
| [TYPE_NEW_POSTING](#section/New-shipment)                              | New shipment                                                                     |
| [TYPE_POSTING_CANCELLED](#section/Shipment-cancellation)               | Shipment cancellation                                                            |
| [TYPE_STATE_CHANGED](#section/Shipment-status-change)                  | Shipment status change                                                           |
| [TYPE_CUTOFF_DATE_CHANGED](#section/Shipment-shipping-date-change)     | Shipment shipping date change                                                    | 
| [TYPE_DELIVERY_DATE_CHANGED](#section/Shipment-delivery-date-change)   | Shipment delivery date change                                                    |
| [TYPE_CREATE_OR_UPDATE_ITEM](#section/Product-created-or-updated)      | Product creation and update, or process error                                    |
| [TYPE_CREATE_ITEM](#section/Product-creation)                          | Product creation or an error when creating it                                    |
| [TYPE_UPDATE_ITEM](#section/Product-update)                            | Product update or an error when updating it                                      |
| [TYPE_STOCKS_CHANGED](#section/Stock-change-at-the-seller's-warehouse) | Stock change at the seller's warehouse                                           |
| [TYPE_NEW_MESSAGE](#section/New-message-in-chat)                       | New message in chat                                                              |
| [TYPE_UPDATE_MESSAGE](#section/Message-update)                         | Message in chat has changed                                                      |
| [TYPE_MESSAGE_READ](#section/Message-was-read)                         | Customer or support read your message                                            |
| [TYPE_CHAT_CLOSED](#section/Chat-is-closed)                            | Chat is closed                                                                   |

## New shipment

> **Note:** 
    If the order was paid late, the in_process_at field may be empty. You can check the shipping date using the result.in_process_at field in the [POST /v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method. 

You'll receive this notification if products in the shipment are sold under the FBS and rFBS schemas:

```
{
  "message_type": "TYPE_NEW_POSTING",
  "posting_number": "24319409-0021-1",
  "products": [
    {
      "sku": 147451939,
      "offer_id": "",
      "quantity": 1
    }
  ],
  "in_process_at": "2021-01-26T06:56:36.294Z",
  "warehouse_id": 12850503335000,
  "shipment_date": "2021-01-26T06:56:36.294Z",
  "tpl_integration_type": "3pl_tracking",
  "is_express": false,
  "tracking_number": "ZZV-23",
  "delivery_date_begin": "2025-01-26T06:56:36.294Z",
  "delivery_date_end": "2025-01-26T06:56:36.294Z",
  "seller_id": 1
}
```

| Parameter | Type     | Format    | Description                                                           |
|-----------------------------------------|---------|-----------|-----------------------------------------------------------------------|
| `message_type`                          | string  | —         | Notification type: `TYPE_NEW_POSTING`.                                |
| `posting_number`                        | string  | —         | Shipment number.                                                      |
| `products`                              | array   | —         | Products information.                                                 |
| `sku`                                   | integer | int64     | Product identifier in the Ozon system, SKU.                                                          |
| `quantity`                              | integer | int64     | Product quantity.                                                     |
| `in_process_at`                         | string  | date-time | Start date and time of shipment processing. |
| `warehouse_id`                          | integer | int64     | Warehouse identifier. |
| `shipment_date`                         | string  | date-time | Date and time before which the shipment must be packaged.|
| `tpl_integration_type`                  | string  | —         | Type of integration with the delivery service:
`ozon`: elivery by the Ozon service;`3pl_tracking`: delivery by the integrated service;`non_integrated`: delivery by a third-party service;`aggregator`: delivery by Ozon partner delivery;`hybryd`: Russian Post delivery scheme. |
| `is_express`                            | boolean | —         | `true` if express delivery is enabled. |
| `tracking_number`                       | string  | —         | Shipment tracking number. |
| `delivery_date_begin`                   | string  | date-time | Delivery start date and time. |
| `delivery_date_end`                     | string  | date-time | Delivery end date and time. |
| `seller_id`                             | integer | int64     | Seller identifier.                                                    |

## Shipment cancellation

You'll receive this notification if products in the shipment are sold under the FBS and rFBS schemas:

```
{
  "message_type": "TYPE_POSTING_CANCELLED",
  "posting_number": "24219509-0020-1",
  "products": [
    {
      "sku": 147451959,
      "quantity": 1
    }
  ],
  "old_state": "posting_transferred_to_courier_service",
  "new_state": "posting_canceled",
  "changed_state_date": "2021-01-26T06:56:36.294Z",
  "reason": {
    "id": 0,
    "message": "string"
  },
  "warehouse_id": 0,
  "seller_id": 15
}
```

| Parameter | Type      | Format     | Description                                                           |
|------------------------------------------|-----------|------------|-----------------------------------------------------------------------|
| `message_type`                           | string    | —          | Notification type: `TYPE_POSTING_CANCELLED`.                          |
| `posting_number`                         | string    | —          | Shipment number.                                                      |
| `products`                               | array     | —          | Products information.                                                 |
| `sku`                                    | integer   | int64      | Product identifier in the Ozon system, SKU.                                                          |
| `quantity`                               | integer   | int64      | Product quantity.                                                     |
| `old_state`                              | string    | —          | Previous shipment status.                                             |
| `new_state`                              | string    | —          | New shipment status: `posting_canceled`—canceled.                     |
| `changed_state_date`                     | string    | date-time  | Date and time when the shipment status was changed in UTC format.     |
| `reason`                                 | object    | —          | Information about cancellation reason.                                | 
| `id`                                     | integer   | int64      | Cancellation reason identifier.                                       |
| `message`                                | string    | —          | Cancellation reason.                                                  |
| `warehouse_id`                           | integer   | int64      | Warehouse identifier where the products for this shipment are stored. |
| `seller_id`                              | integer   | int64      | Seller identifier.                                                    |

### Shipment statuses

- `posting_acceptance_in_progress`: acceptance in progress;
- `posting_created`: created;
- `posting_transferring_to_delivery`: is handed over to the delivery service;
- `posting_in_carriage`: in shipping;
- `posting_not_in_carriage`: not added to shipping;
- `posting_in_client_arbitration`: customer delivery arbitrage;
- `posting_on_way_to_city`: on the way to the city;
- `posting_transferred_to_courier_service`: is handed over to the courier;
- `posting_in_courier_service`: courier on the way;
- `posting_on_way_to_pickup_point`: on the way to the pick-up point;
- `posting_in_pickup_point`: at the pick-up point;
- `posting_conditionally_delivered`: presumably delivered;
- `posting_driver_pick_up`: handed over to the driver;
- `posting_not_in_sort_center`: not accepted at the sorting center.

## Shipment status change

Correspondence of Seller API and push model statuses.

| Seller API               |                                     | Push model                               |                                         |
|--------------------------|-------------------------------------|------------------------------------------|-----------------------------------------|
| **Status**               | **Description**                     | **Status**                               | **Description**                         |
| `acceptance_in_progress` | Acceptance in progress.             | `posting_acceptance_in_progress`         | Acceptance in progress.                 |
| `awaiting_approve`       | Awaiting confirmation.              | `posting_created`                        | Created.                                |
| `awaiting_packaging`     | Awaiting packaging.                 | `posting_created`                        | Created.                                |
| `awaiting_registration`  | Awaiting registration.              | `posting_awaiting_registration`          | Awaiting registration.                  |
| `awaiting_deliver`       | Awaiting shipping.                  | `posting_transferring_to_delivery`       | Is handed over to the delivery service. |
|                          |                                     | `posting_in_carriage`                    | In shipping.                            |
|                          |                                     | `posting_not_in_carriage`                | Not added to shipping.       |
| `arbitration`            | Arbitrage.                          | `posting_in_arbitration`                 | Arbitrage.                              |
| `client_arbitration`     | Customer delivery arbitrage.        | `posting_in_client_arbitration`          | Customer delivery arbitrage.            |
| `delivering`             | Delivering.                         | `posting_on_way_to_city`                 | On the way to your city.                |
|                          |                                     | `posting_transferred_to_courier_service` | Is handed over to the courier.          |
|                          |                                     | `posting_in_courier_service`             | Courier on the way.                     |
|                          |                                     | `posting_on_way_to_pickup_point`         | On the way to the pick-up point.        |
|                          |                                     | `posting_in_pickup_point`                | At the pick-up point.                   |
|                          |                                     | `posting_conditionally_delivered`        | Presumably delivered.                   |
| `driver_pickup`          | Handed over to the driver.          | `posting_driver_pick_up`                 | Handed over to the driver.              |
| `delivered`              | Delivered.                          | `posting_delivered`                      | Delivered.                              |
|                          |                                     | `posting_received`                       | Recieved.                               |
| `cancelled`              | Canceled.                           | `posting_canceled`                       | Canceled.                               |
| `not_accepted`           | Not accepted at the sorting center. | `posting_not_in_sort_center`             | Not accepted at the sorting center.     |

You'll receive this notification if products in the shipment are sold under the FBS and rFBS schemas:

```
{
  "message_type": "TYPE_STATE_CHANGED",
  "posting_number": "24219509-0020-2",
  "new_state": "posting_delivered",
  "changed_state_date": "2021-02-02T15:07:46.765Z",
  "warehouse_id": 0,
  "seller_id": 15
}
```

| Parameter | Type      | Format    | Description                                                           |
|------------------------------------------|-----------|-----------|-----------------------------------------------------------------------|
| `message_type`                           | string    | —         | Notification type: `TYPE_STATE_CHANGED`.                              |
| `posting_number`                         | string    | —         | Shipment number.                                                      |
| `new_state`                              | string    | —         | New shipment status.                                                  |
| `changed_state_date`                     | string    | date-time | Date and time when the shipment status was changed in UTC format.     |
| `warehouse_id`                           | integer   | int64     | Warehouse identifier where the products for this shipment are stored. |
| `seller_id`                              | integer   | int64     | Seller identifier.                                                    |

### Shipment statuses

- `posting_acceptance_in_progress`: acceptance in progress;
- `posting_transferring_to_delivery`: is handed over to the delivery service;
- `posting_in_carriage`: in shipping;
- `posting_not_in_carriage`: not added to shipping;
- `posting_in_arbitration`: arbitrage;
- `posting_in_client_arbitration`: customer delivery arbitrage;
- `posting_on_way_to_city`: on the way to the city;
- `posting_transferred_to_courier_service`: is handed over to the courier;
- `posting_in_courier_service`: courier on the way;
- `posting_on_way_to_pickup_point`: on the way to the pick-up point;
- `posting_in_pickup_point`: at the pick-up point;
- `posting_conditionally_delivered`: presumably delivered;
- `posting_driver_pick_up`: handed over to the driver;
- `posting_delivered`: delivered;
- `posting_not_in_sort_center`: not accepted at the sorting center.

## Shipment shipping date change

> **Note:** 
    The notification is working in test mode. We recommend checking the shipping date using the [POST /v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method, the result.shipment_date field.

The new_cutoff_date field may be empty because the delivery interval has been deleted. Wait until a new date is set: you will get a new notification.

> **Note:** 
    Sometimes notifications of this type might be sent after the order has been collected. Please ignore them.

You'll receive this notification if products in the shipment are sold under the FBS and rFBS schemas:

```
{
  "message_type": "TYPE_CUTOFF_DATE_CHANGED",
  "posting_number": "24219509-0020-2",
  "new_cutoff_date": "2021-11-24T07:00:00Z",
  "old_cutoff_date": "2021-11-21T10:00:00Z",
  "warehouse_id": 0,
  "seller_id": 15
}
```

| Parameter | Type     | Format    | Description                                                           |
|------------------------------------------|----------|-----------|-----------------------------------------------------------------------|
| `message_type`                           | string   | —         | Notification type: `TYPE_CUTOFF_DATE_CHANGED`.                        |
| `posting_number`                         | string   | —         | Shipment number.                                                      |
| `new_cutoff_date`                        | string   | date-time | New shipping date and time in UTC format.                             |
| `old_cutoff_date`                        | string   | date-time | Previous shipping date and time in UTC format.                        |
| `warehouse_id`                           | integer  | int64     | Warehouse identifier where the products for this shipment are stored. |
| `seller_id`                              | integer  | int64     | Seller identifier.                                                    |

## Shipment delivery date change

Message that Ozon sends:

> **Note:** 
You'll receive this notification if products in the shipment are sold under the FBS and rFBS schemas.

The new_delivery_date_begin and new_delivery_date_end fields may be empty because the delivery interval has been deleted. Wait until a new date is set: you will get a new notification.

```
{
  "message_type": "TYPE_DELIVERY_DATE_CHANGED",
  "posting_number": "24219509-0020-2",
  "new_delivery_date_begin": "2021-11-24T07:00:00Z",
  "new_delivery_date_end": "2021-11-24T16:00:00Z",
  "old_delivery_date_begin": "2021-11-21T10:00:00Z",
  "old_delivery_date_end": "2021-11-21T19:00:00Z",
  "warehouse_id": 0,
  "seller_id": 15
}
```

| Parameter | Type     | Format    | Description                                               |
|------------------------------------------|----------|-----------|-----------------------------------------------------------|
| `message_type`                           | string   | —         | Notification type: `TYPE_DELIVERY_DATE_CHANGED`.          |
| `posting_number`                         | string   | —         | Shipment number.                                          |
| `new_delivery_date_begin`                | string   | date-time | New delivery start date and time in UTC format.           |
| `new_delivery_date_end`                  | string   | date-time | New delivery end date and time in UTC format.             |
| `old_delivery_date_begin`                | string   | date-time | Previous delivery start date and time in UTC format.      |
| `old_delivery_date_end`                  | string   | date-time | Previous delivery end date and time in UTC format.        |
| `warehouse_id`                           | integer  | int64     | Warehouse identifier where the products for this shipment are stored. |
| `seller_id`                              | integer  | int64     | Seller identifier.                                        |

## Product created or updated

Message that Ozon sends:

```
{
    "message_type": "TYPE_CREATE_OR_UPDATE_ITEM",
    "seller_id": 0,
    "offer_id": "string",
    "product_id": 0,
    "is_error": false,
    "changed_at": "2022-09-01T14:15:22Z"
}
```

| Parameter | Type         | Format    | Description                                                                                                                                                                                                       |
|-----------------------------------------|-------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `seller_id`                             | integer     | int64     | Seller identifier.                                                                                                                                                                                                |
| `message_type`                          | string      | —         | Notification type: `TYPE_CREATE_OR_UPDATE_ITEM`.                                                                                                                                                                  |
| `offer_id`                              | string      | —         | Product identifier in the seller's system.                                                                                                                                                                        |
| `product_id`                            | integer     | int64     | Product identifier in the Ozon system, `product_id`.                                                                                                                                                                           |
| `is_error`                              | boolean     | —         | An indication that errors occurred during the product creation or update:
• `true`: there were errors, the product wasn't created or updated;
• `false`: the product was created or updated without errors. |
| `changed_at`                            | string      | date-time | Update date and time.                                                                                                                                                                                             |

## Product creation

> **Note:** 
TYPE_CREATE_ITEM push notifications will be suspended on July 15, 2023.

Set up your service to receive [TYPE_CREATE_OR_UPDATE_ITEM](#section/Sozdanie-ili-obnovlenie-tovara) notifications.

Message that Ozon sends:

```
{
    "message_type": "TYPE_CREATE_ITEM",
    "seller_id": 0,
    "offer_id": "string",
    "product_id": 0,
    "is_error": false,
    "changed_at": "2021-09-01T14:15:22Z"
}
```

| Parameter | Type         | Format    | Description                                                                                                                                                                       |
|-----------------------------------------|-------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `seller_id`                             | integer     | int64     | Seller identifier.                                                                                                                                                                |
| `message_type`                          | string      | —         | Notification type: `TYPE_CREATE_ITEM`.                                                                                                                                            |
| `offer_id`                              | string      | —         | Product identifier in the seller's system.                                                                                                                                        |
| `product_id`                            | integer     | int64     | Product identifier in the Ozon system, `product_id`.                                                                                                                                           |
| `is_error`                              | boolean     | —         | An indication that errors occurred during the product creation:
• `true`: there were errors, the product wasn't created;
• `false`: the product was created without errors. |
| `changed_at`                            | string      | date-time | Creation date and time.                                                                                                                                                           |

## Product update

> **Note:** 
TYPE_UPDATE_ITEM push notifications will be suspended on July 15, 2023.

Set up your service to receive [TYPE_CREATE_OR_UPDATE_ITEM](#section/Sozdanie-ili-obnovlenie-tovara) notifications.

Message that Ozon sends:

```
{
    "message_type": "TYPE_UPDATE_ITEM",
    "seller_id": 0,
    "offer_id": "string",
    "product_id": 0,
    "is_error": false, 
    "changed_at": "2021-09-01T14:15:22Z"
}
```

| Parameter | Type         | Format    | Description                                                                                                                                                                     |
|-----------------------------------------|-------------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `seller_id`                             | integer     | int64     | Seller identifier.                                                                                                                                                              |
| `message_type`                          | string      | —         | Notification type: `TYPE_UPDATE_ITEM`.                                                                                                                                          |
| `offer_id`                              | string      | —         | Product identifier in the seller's system.                                                                                                                                      |
| `product_id`                            | integer     | int64     | Product identifier in the Ozon system, `product_id`.                                                                                                                                         |
| `is_error`                              | boolean     | —         | An indication that errors occurred during the product update:
• `true`: there were errors, the product wasn't updated;
• `false`: the product was updated without errors. |
| `changed_at`                            | string      | date-time | Update date and time.                                                                                                                                                           |

## Stock change at the seller's warehouse

Message that Ozon sends:

```
{
  "message_type": "string",
  "seller_id": 0,
  "items": [
    {
      "product_id": 0,
      "sku": 0,
      "updated_at": "2021-09-01T14:15:22Z",
      "stocks": [
        {
          "warehouse_id": 0,
          "present": 0,
          "reserved": 0
        }
      ]
    }
  ]
}
```

| Parameter | Type     | Format    | Description                                             |
|-----------------------------------------|---------|-----------|---------------------------------------------------------|
| `seller_id`                             | integer | int64     | Seller identifier.                                      |
| `message_type`                          | string  | —         | Notification type: `TYPE_STOCKS_CHANGED`.               |
| `items`                                 | array   | —         | Array with products data.                               |
| `updated_at`                            | string  | date-time | Update date and time.                                   |
| `sku`                                   | integer | int64     | Product SKU when working under the FBS or rFBS schemes. |
| `product_id`                            | integer | int64     | Product identifier in the Ozon system, `product_id`.    |
| `stocks`                                | array   | —         | Array with product stocks data.                         |
| `warehouse_id`                          | integer | int64     | Warehouse identifier.                                   |
| `present`                               | integer | int64     | Total product stocks at the warehouse.                  |
| `reserved`                              | integer | int64     | Number of reserved products at the warehouse.           |

## New message in chat

```
{  
    "message_type": "TYPE_NEW_MESSAGE",
    "chat_id": "b646d975-0c9c-4872-9f41-8b1e57181063",
    "chat_type": "Buyer_Seller",
    "message_id": "3000000000817031942",
    "created_at": "2022-07-18T20:58:04.528Z",
    "user": {
        "id": "115568",
        "type": "Сustomer"
    },
    "data": [
        "Message text"
    ],  
    "seller_id": "7"
}
```

| Parameter | Type     | Format    | Description                                                                                                                                                                                                                                                                                                                                                                                                                            |
|------------------------------------------|-----------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `message_type`                           | string          | —         | Notification type: `TYPE_NEW_MESSAGE`.                                                                                                                                                                                                                                                                                                                                                                                                 |
| `chat_id`                                | string          | —         | Chat identifier.                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `chat_type`                              | string          | —         | Chat type:
• `Seller_Support`: support chat;
• `Buyer_Seller`: chat with a customer;
• `Seller_Notification`: notifications from Ozon;
• `Seller_API_Updates`: Seller API updates;
• `Seller_API_Notifications`: Seller API notifications;
• `Seller_Notification_Logistics`: Ozon Delivery notifications;
• `Buyer_Seller_Select`: chat with a Select customer. |
| `message_id`                             | string          | —         | Message identifier.                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `created_at`                             | string          | date-time | Message creation date.                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `user`                                   | object          | —         | Information about message sender.                                                                                                                                                                                                                                                                                                                                                                                                      |
| `id`                                     | string          | —         | Sender identifier.                                                                                                                                                                                                                                                                                                                                                                                                                     |
| `type`                                   | string          | —         | Sender type:
• `Customer`: customer;
• `Support`: support;
• `NotificationUser`: Ozon.                                                                                                                                                                                                                                                                                                                                        |
| `data`                                   | array of string | —         | Array with message content in Markdown format.                                                                                                                                                                                                                                                                                                                                                                                         |
| `seller_id`                              | integer         | int64     | Seller identifier.                                                                                                                                                                                                                                                                                                                                                                                                                     |

## Message update

```
{  
    "message_type": "TYPE_UPDATE_MESSAGE",
    "chat_id": "b646d975-0c9c-4872-9f41-8b1e57181063",
    "chat_type": "Buyer_Seller",
    "message_id": "3000000000817031942",
    "created_at": "2022-07-18T20:58:04.528Z",
    "updated_at": "2022-07-18T20:59:04.528Z",
    "user": {
        "id": "115568",
        "type": "Сustomer"
    },
    "data": [
        "Message text"
    ], 
    "seller_id": "7"
}
```

| Parameter | Type     | Format    | Description                                                                                                                                     |
|-----------------------------------------|-----------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `message_type`                          | string          | —         | Notification type: `TYPE_UPDATE_MESSAGE`.                                                                                                       |
| `chat_id`                               | string          | —         | Chat identifier.                                                                                                                                |
| `chat_type`                             | string          | —         | Chat type:
• `Seller_Support`: support chat;
• `Buyer_Seller`: chat with a customer;
• `Seller_Notification`: notifications from Ozon;
• `Seller_API_Updates`: Seller API updates;
• `Seller_API_Notifications`: Seller API notifications;
• `Seller_Notification_Logistics`: Ozon Delivery notifications;
• `Buyer_Seller_Select`: chat with a Select customer. |
| `message_id`                            | string          | —         | Message identifier.                                                                                                                             |
| `created_at`                            | string          | date-time | Message creation date.                                                                                                                          |
| `updated_at`                            | string          | date-time | Message update date.                                                                                                                            |
| `user`                                  | object          | —         | Information about message sender.                                                                                                               |
| `id`                                    | string          | —         | Sender identifier.                                                                                                                              |
| `type`                                  | string          | —         | Sender type:
• `Customer`: customer;
• `Support`: support;
• `NotificationUser`: Ozon.                                                 |
| `data`                                  | array of string | —         | Array with message content in Markdown format.                                                                                                  |
| `seller_id`                             | integer         | int64     | Seller identifier.                                                                                                                              |

## Message was read

```
{  
    "message_type": "TYPE_MESSAGE_READ",
    "chat_id": "b646d975-0c9c-4872-9f41-8b1e57181063",
    "chat_type": "Buyer_Seller",
    "message_id": "3000000000817031942",
    "created_at": "2022-07-18T20:58:04.528Z",    
    "user": {
        "id": "115568",
        "type": "Сustomer"
    },
    "last_read_message_id": "3000000000817031942",
    "seller_id": "7"
}
```

| Parameter | Type     | Format    | Description                                                                                                                                                                                                                                                                                                                                                                           |
|-----------------------------------------|---------|-----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `message_type`                          | string  | —         | Notification type: `TYPE_MESSAGE_READ`.                                                                                                                                                                                                                                                                                                                                               |
| `chat_id`                               | string  | —         | Chat identifier.                                                                                                                                                                                                                                                                                                                                                                      |
| `chat_type`                             | string  | —         | Chat type:
• `Seller_Support`: support chat;
• `Buyer_Seller`: chat with a customer;
• `Seller_Notification`: notifications from Ozon;
• `Seller_API_Updates`: Seller API updates;
• `Seller_API_Notifications`: Seller API notifications;
• `Seller_Notification_Logistics`: Ozon Delivery notifications;
• `Buyer_Seller_Select`: chat with a Select customer. |
| `message_id`                            | string  | —         | Message identifier.                                                                                                                                                                                                                                                                                                                                                                   |
| `created_at`                            | string  | date-time | Message creation date.                                                                                                                                                                                                                                                                                                                                                                |
| `user`                                  | object  | —         | Information about the user who read the message.                                                                                                                                                                                                                                                                                                                                      |
| `id`                                    | string  | —         | User identifier.                                                                                                                                                                                                                                                                                                                                                                      |
| `type`                                  | string  | —         | User type:
• `Customer`: customer;
• `Support`: support;
• `NotificationUser`: Ozon.                                                                                                                                                                                                                                                                                         |
| `last_read_message_id`                  | string  | —         | Last read message identifier.                                                                                                                                                                                                                                                                                                                                                         |
| `seller_id`                             | integer | int64     | Seller identifier.                                                                                                                                                                                                                                                                                                                                                                    |

## Chat is closed

```
{  
    "message_type": "TYPE_CHAT_CLOSED",
    "chat_id": "b646d975-0c9c-4872-9f41-8b1e57181063",
    "chat_type": "Buyer_Seller",
    "user": {
        "id": "115568",
        "type": "Сustomer"
    },
    "seller_id": "7"
}
```

| Parameter | Type     | Format    | Description                                                                                                                                     |
|-----------------------------------------|---------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| `message_type`                          | string  | —         | Notification type: `TYPE_CHAT_CLOSED`.                                                                                                          |
| `chat_id`                               | string  | —         | Chat identifier.                                                                                                                                |
| `chat_type`                             | string  | —         | Chat type:
• `Seller_Support`: support chat;
• `Buyer_Seller`: chat with a customer;
• `Seller_Notification`: notifications from Ozon;
• `Seller_API_Updates`: Seller API updates;
• `Seller_API_Notifications`: Seller API notifications;
• `Seller_Notification_Logistics`: Ozon Delivery notifications;
• `Buyer_Seller_Select`: chat with a Select customer. |
| `user`                                  | object  | —         | Information about the user who closed the chat.                                                                                                 |
| `id`                                    | string  | —         | User identifier.                                                                                                                                |
| `type`                                  | string  | —         | User type:
• `Customer`: customer;
• `Support`: support;
• `NotificationUser`: Ozon.                                                   |
| `seller_id`                             | integer | int64     | Seller identifier.                                                                                                                              |

## service_response

#### If the notification was received successfully

If the notification is processed successfully, the service should respond with an HTTP 200 code:

```json5
{
  "result": true
}
```

| Parameter   | Type    | Format  | Description               |
|--------------------------------------------|---------|---------|---------------------------|
| `result`                                   | boolean | —       | Notification is received. |

#### If an error occurs

If there is an error when processing notification, the service should respond with an HTTP code from 4xx or 5xx groups:

```json5
{
   "error": {
      "code": "ERROR_UNKNOWN",
      "message": "error",
      "details": null
   }
}
```

| Parameter | Type    | Format | Description                                                                                                                                                            |
|------------------------------------------|---------|--------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `error`                                  | object  | —      | Information about the error.                                                                                                                                           |
| `code`                                   | string  | —      | Error code:
• `ERROR_UNKNOWN`.
• `ERROR_PARAMETER_VALUE_MISSED` — one or more parameters values are missing.
• `ERROR_REQUEST_DUPLICATED` — duplicate request. |
| `message`                                | string  | —      | Detailed error description.                                                                                                                                            |
| `details`                                | string  | —      | Additional information.                                                                                                                                                |

