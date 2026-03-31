# Errors

## Errors

### All methods

| Error text                                                      | Error description                                                                                                                          |
|-----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| `Circle is open`                                                | If there are a lot of requests, the system temporarily blocks the method. The method starts working as usual in a few minutes.             |
| `Internal error`                                                | The server failed to process the request before a timeout.                                                                                 |
| `Invalid Api-Key, please check the key and try again`           | Check the key and try again.                                                                                                               |
| `Api-key is deactivated, use another one or generate a new one` | API key deactivated: use another key or generate a new one.                                                                                |
| `Api-Key is missing a required role for a method`               | API key doesn't have the required role for the method to work.                                                                             |
| `Api-Key is restricted to specific IP addresses`                | Access to API key is only allowed from certain IP addresses.                                                                               |
| `You have reached request rate limit per second`                | You have exceeded the request limit: maximum of 50 requests per second per Client ID. Method-specific limits are also taken into account.  |

###  [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5)

| Error text                                        | Error description                                                                                         |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| `GTD_MUST_BE_SPECIFIED_FOR_PRODUCT_COUNTRY`   | Сustoms cargo declaration (CCD) number isn't specified. If you don't have it, pass `is_gtd_absent: true`. |

### [/v2/products/stocks](#operation/ProductAPI_ProductsStocksV2)

| Error text                                                                                               | Error description                                                                                                                                                                                          |
|----------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `product_is_not_created`                                                                                 | The product didn't pass moderation, so you can't update the stocks yet. Wait for the `price_sent` status and try again.                                                                                    |
| `offer_id_not_found`                                                                                     | Product with such ID doesn't exist in your personal account.                                                                                                                                               |
| `FLAMMABLE_ONLY_ON_SELF_OR_PROVIDER_DELIVERY`                                                            | You can sell flammable products only from your warehouses, using your own delivery or a third-party service. Select another warehouse or create a new one and try again.                                   |
| `WAREHOUSE_NOT_FOUND`                                                                                    | Warehouse `warehouse_id` not found. Make sure that the warehouse is active and check for errors in its ID.                                                                                                 |
| `PRODUCT_HAS_NOT_BEEN_TAGGED_YET`                                                                        | The product isn't tagged with "КГТ" or "неКГТ" tags because its dimensions aren't specified or the tagging system hasn't processed it yet. "КГТ" stands for bulky products and "неКГТ" for non-bulky ones. |
| `NON_KGT_ON_KGT_WAREHOUSE`                                                                               | An attempt to set or update the non-bulky product stocks at a warehouse for bulky products.                                                                                                                |
| `PRICE_IS_NOT_SENT`                                                                                      | The product hasn't been created yet or is being updated.                                                                                                                                                   |
| `MP_DELIVERY_ONLY_3PL_ERROR`                                                                             | The product can't be placed at the warehouse that uses logistics by Ozon.                                                                                                                                  |
| `TOO_MANY_REQUESTS`                                                                                      | You've updated the stock for one product too often. You can update the stock of one product once in 30 seconds.                                                                                            |                                                     
| `Stock is updated too frequently`                                                                        | You're trying to update the stock of the same product too often. You can only update the stock of one product once in 30 seconds. Make sure your integration does not update the stock in the background.  |
| `MULTIBOX_NOT_ALLOWED_FOR_FBS`	                                                                          | You can't combine several boxes in one product if you work under the FBS scheme. Remove the value from the `stock` field and try again.                                                                    |
| `OVER_MAX_OVH_NON_KGT`	                                                                                  | You can't sell bulky products from the selected warehouse. Select another warehouse or create a new one and try again.                                                                                     |                                 
| `OVER_MAX_OVH_KGT`	                                                                                      | The product weight or dimensions exceed the maximum values for the selected warehouse. Edit the product characteristics or select another warehouse.                                                       |                                                              
| `SOURCE_TYPE_NOT_FOUND`	                                                                                 | The product has no SKU. Make sure that the product is created and set up correctly.                                                                                                                        |                                                                                                
| `Request validation error: invalid ProductsStocksRequest.Stocks[0]: embedded message failed validation`	 | There is no warehouse identifier in the request. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.                                                              |
| `STOCK_TOO_BIG`	                                                                                         | The value you've specified for the product stock is too high. Set a quantity less than a million and try again.                                                                                            | 
| `FLAMMABLE_ON_NON_KGT_WAREHOUSE`                                                                         | You can sell flammable products only from a warehouse for bulky products. Select another warehouse or create a new one.                                                                                    |
| `NOT_FOUND_ERROR`                                                                                        | Failed to find the product in your personal account.                                                                                                                                                       |
| `SIZE_REQUIRED_FOR_NOT_UNIQUE_OFFER_ID`                                                                  | Article code duplicates the code of another product. For a standard product, specify the parameter `quant_size = 1`, and for a quant, specify `quant_size = 2` or more.                                    |
| `CB_DELIVERY_ONLY_FBP`                                                                                   | This product is only available for sale from FBP warehouse.                                                                                                                                                |

### [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4)

| Error text                                 | Error description                                                                                                                                                                                                                            |
|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `TRANSITION_IS_NOT_POSSIBLE`               | You pass an incorrect order of rFBS order statuses. Get the current shipment status using the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method. The status change is asynchronous.                                        |
| `HAS_INCORRECT_TPL_INTEGRATION_TYPE`       | An attempt to set status to an rFBS order which is delivering by an integrated delivery service.                                                                                                                                             |
| `POSTING_NOT_FOUND`                        | The order isn't in the partner's personal seller account.                                                                                                                                                                                    |
| `POSTING_ALREADY_CANCELLED`                | The order is already canceled.                                                                                                                                                                                                               |
| `POSTING_ALREADY_SHIPPED`                  | The order is already packaged.                                                                                                                                                                                                               |
| `HAS_INCORRECT_STATUS`                     | The order has incorrect status.                                                                                                                                                                                                              |
| `HAS_INCORRECT_PRODUCT_QUANTITY`           | Incorrect number of products or incorrect SKU in the request.                                                                                                                                                                                |
| `UNKNOW_PRODUCT`/`UNKNOWN_PRODUCT_DEFINED` | You've specified an incorrect product identifier. Make sure that you specify product SKU in the `product_id` field.	                                                                                                                         |
| `EXEMPLAR_INFO_ALREADY_DEFINED`            | The information about the product items has already been updated. You don't need to pass the data again.	                                                                                                                                    |
| `MANDATORY_MARK_REDUNDANT`                 | You don't need to pass the labeling code for the product.	                                                                                                                                                                                   |
| `INCORRECT_OVH_FOR_POSTING`                | The volume and weight characteristics of the shipment don't meet the drop-off point restrictions. Get information about restrictions by shipment number in the [/v1/posting/fbs/restrictions](#operation/PostingAPI_GetRestrictions) method. |

### [/v2/posting/fbs/package-label](#operation/PostingAPI_PostingFBSPackageLabel)

| Error text                          | Error description                                                                                                                                                                                                                                  |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `The next postings aren't ready` | The postings aren't ready for label printing.                                                                                                                                                                                                      |
| `INVALID_ARGUMENT`               | You've passed invalid values in the request body. You can print the label only for orders with the "Awaiting shipment"—`awaiting_deliver` status. Make sure that the shipment has the correct status and you sent correct data. |

### [/v2/posting/fbs/package-label/create](#operation/PostingAPI_CreateLabelBatchV2)

| Error text                                   | Error description                                                                                                  |
|----------------------------------------------|---------------------------------------------------------------------------------------------------------------------|
| `POSTING_NUMBERS_IS_INCORRECT_FOR_COMPANY`   | Make sure that the shipment belongs to the Client ID from which the request is sent.                                |

### [/v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate)

| Error text                                                         | Error description                                                                                                                                                                                                          |
|--------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `can't create carriage`                                            | Shipping isn't yet ready for creation. Add shipping certificates.                                                                                                                                                          |
| `Company has no FBS-warehouses`                                    | You don't have any created FBS warehouses.                                                                                                                                                                                 |
| `DELIVERY_METHOD_NOT_FOUND`                                        | You've passed an invalid delivery method identifier: `delivery_method_id`.                                                                                                                                                 |
| `first_mile_absent`                                                | Shipping method isn't specified for the delivery method. [Learn more about FBS warehouse settings](https://seller-edu.ozon.ru/fbs/logistics-settings/sklady#как-активировать-склад)                                        |
| `first_mile_is_changing`                                           | Updating warehouse settings. Shipping will be available after the update.                                                                                                                                                  |
| `has_overflow`                                                     | Pick-up point is overloaded. Choose another one to avoid longer delivery times.                                                                                                                                            |
| `has_postings_with_registration_error`                             | Some shipments can't get into the report due to registration error in the delivery service.                                                                                                                                |
| `has_seller_returns_in_stock`                                      | Collect returns.                                                                                                                                                                                                           |
| `has_surge`                                                        | Pick-up point is overloaded and shipping costs are temporarily higher. To save money, select a less loaded pick-up point.                                                                                                  |
| `new_postings_are_possible`                                        | New shipments may arrive in shipping. Use the method when today's order packaging time is over.                                                                                                                            |
| `no_postings`                                                      | No shipments for the current shipping.                                                                                                                                                                                     |
| `non_carriageable`                                                 | Shipping isn't required.                                                                                                                                                                                                   |
| `not_accepted_on_sc`                                               | Process shipments on the [**Disputed**](https://seller.ozon.ru/app/postings/fbs?tab=arbitration) tab in your account. To add a shipment to an already created shipping, cancel the previous shipping and create a new one. |
| `not_packaged`                                                     | You haven't packaged all the shipments for this shipping.                                                                                                                                                                  |
| `not_registered`                                                   | Registering shipments in the delivery service. Generate a report when all shipments have a `awaiting_deliver` status.                                                                                                      |
| `other`                                                            | An error occurred. Contact the support service.                                                                                                                                                                            |
| `outdated`                                                         | You've specified a date that's in the past.                                                                                                                                                                                |
| `package_time_not_passed`                                          | The time to make a shipment hasn't come yet.                                                                                                                                                                               |
| `partial_carriage_formed`                                          | All partial shipments have been created.                                                                                                                                                                                   |
| `partial_carriage_in_proccess`                                     | Create some more partial shipments.                                                                                                                                                                                        |
| `posting_statuses_not_ready`                                       | Making a shipment. Retry the request in a few minutes.                                                                                                                                                                     |
| `there_are_incomplete_carriages`                                   | You have incomplete shipments.                                                                                                                                                                                             |
| `Trying set ContainersCount to not HasEntrustedAcceptance company` | Activate trusted acceptance to pass on the number of package units. [Learn more about trusted acceptance](https://seller-edu.ozon.ru/fbs/ozon-logistika/doveritel-naya-priemka-gruzovogo-mesta)                            |
| `will_be_partial_carriage`                                         | Create several partial shipments.                                                                                                                                                                                          |
| `Incorrect_carriage_status`                                        | The shipment hasn't been generated yet. The act must be in `sended` or `formed` status. To get the status, use the [/v2/posting/fbs/act/list](#operation/PostingAPI_FbsActList) method.                                    |

### [/v1/product/import/prices](#operation/ProductAPI_ImportProductsPrices)

| Error text                                                            | Error description                                                                                                                                                                          |
|-----------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `invalid_cat egory_price`                                             | An attempt to set too high or too low a price for a product.                                                                                                                               |
| `discount_for_average_price_is_too_small`                             | The discount is too small. If the price after the discount is from 400 to 10,000 rubles inclusive, the difference between the prices before and after the discount should be more than 5%. |
| `discount_for_low_price_is_too_small`                                 | The discount is too small. If the price after the discount is below 400 rubles inclusive, the difference between the prices before and after the discount must be more than 20 rubles.     |
| `discount_too_big`                                                    | The discount is too big. The difference between the prices before and after the discount must be less than 90%.                                                                            |
| `discount_for_top_price_is_too_small`                                 | The discount is too small. If the price after the discount is higher than 10,000 rubles, the difference between the prices before and after the discount must be more than 500 rubles.     |
| `price_negative`                                                      | An attempt to set a negative price.                                                                                                                                                        |
| `NOT_FOUND_ERROR`                                                     | There is no product with this identifier in your personal account.                                                                                                                         |
| `error limiting: acquire limit per item: items limit: limit exceeded` | You've updated the price for the product too often. You can update the price of one product 10 times in 1 hour.                                                                            |

### [/v3/product/import](#operation/ProductAPI_ImportProductsV3)

| Error text                                                     | Error description                                                                                                                                                                                                                                                                                                                                                                |
|----------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `SPU_already_exists`                                           | A product with these attributes already exists.                                                                                                                                                                                                                                                                                                                                  |
| `"Invalid_state" - Product is not ready to supply` | The product isn't ready for stock updating. Make sure that the product is created or the account is active.                                                                                                                                                                                                                                                                      |
| `Incorrect_density`                                            | The product hasn't passed the density verification. The density you specified is out of the acceptable range. The minimum density value is 0.001, the maximum is 13.55. 

 The density is calculated using the formula: weight × 1000 ÷ (height × width × depth). 

 Make sure that you are using the correct weight and volume values for your product. |
| `price_is_negative`                                            | The `price` parameter isn't specified.                                                                                                                                                                                                                                                                                                                                           |

### [/v1/draft/supply/create](#operation/SupplyDraftAPI_DraftSupplyCreate)

| Error text                                        | Error description                                                                                                                                                          |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `CREATE_ORDER_ERROR_REASON_INVALID_STORAGE_WAREHOUSE` | The final placement warehouse is incorrect. Get available warehouses using the [/v1/draft/create/info](#operation/SupplyDraftAPI_DraftCreateInfo) method. |

### [/v1/draft/timeslot/info](#operation/SupplyDraftAPI_DraftTimeslotInfo)

| Error text                                           | Error description                                                                                                                                                             |
|------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `WAREHOUSE_SCORING_INVALID_REASON_NOT_AVAILABLE_MATRIX` | This warehouse can't accept all products from the draft composition (`is_available = false`). Choose another placement warehouse.                                               |
| `ITEM_REJECTION_REASON_OUT_OF_ASSORTMENT`              | Can't find product composition. Make sure that the seller has these SKUs and you passed them correctly.                                                                    |

### [/v1/product/import/info](#operation/ProductAPI_GetImportProductsInfo)

| Error text              | Error description                                                           |
|---------------------|--------------------------------------------------------------------|
| `result: items:  0` | Make sure that the correct product category and VAT are set. |

### [/v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate)

| Error text                    | Error description                      |
|---------------------------|----------------------------------------|
| `can't create carriage`   | Shipping isn't yet ready for creation. |

### [v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting)

| Error text                         | Error description                                            |
|--------------------------------|--------------------------------------------------------------|
| `HAS_INCORRECT_CANCEL_REASON`  | You've specified an incorrect order cancellation identifier. |

### [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) and [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5)

| Error text                                           | Error description                                                                                                                                                                                                                        |
|------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `GTD_IS_REQUIRED_ONLY_FOR_LEGAL_CUSTOMER`            | The Cargo Customs Declaration should be handed over only to legal entities.                                                                                                                                                              |
| `EXEMPLAR_ID does not belong to product PRODUCT_ID`  | Item identifier `exemplar_id` doesn't match the product identifier `product_id`. Get the correct `exemplar_id` using the [/v6/fbs/posting/product/exemplar/create-or-get](#operation/PostingAPI_FbsPostingProductExemplarCreateOrGetV6). |
| `Posting must have 'awaiting_packaging' status`      | The order has an incorrect status. To transfer data or check the status, the shipment must be in the `awaiting_packaging` status.                                                                                                        |

### [/v1/product/unarchive](#operation/ProductAPI_ProductUnarchive)

| Error text               | Error description                                                                                                                                                                                                                        |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `restore limit exceeded` | You’ve exceeded the limit for restoring PDPs that were archived automatically. You can restore up to 100 PDPs per day. The limit is updated at 03:00 Moscow time.                                                                        |
| `total limit exceeded`   | You’ve exceeded the product range limit in your personal account. Edit your active PDPs or archive some of them. You can check your product range limit using the [/v4/product/info/limit](#operation/ProductAPI_GetUploadQuota) method. |

### [v1/carriage/create](#operation/CarriageAPI_CarriageCreate)

| Error text          | Error description                                                                                                                                                                                                                                                           |
|---------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `POSTING_NOT_FOUND` | There are no shipments to add to freight. Confirm previous freight using the [v1/carriage/approve](#operation/CarriageAPI_CarriageApprove) method or add new shipment to existing freight using the [/v1/carriage/set-postings](#operation/CarriageAPI_SetPostings) method. |

