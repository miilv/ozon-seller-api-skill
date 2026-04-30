# Ozon Seller API — Endpoints Index

Base URL: `https://api-seller.ozon.ru`

All requests use POST method with JSON body.

Auth headers: `Client-Id` (seller ID) + `Api-Key` (API key)


## Description

## Basic Methods


### APIkey

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/roles` | Get a list of roles and methods based on the API key | `AccessAPI_RolesByToken` |


### SellerInfo

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/seller/info` | Get information about seller account | `SellerAPI_SellerInfo` |
| `POST /v1/seller/ozon-logistics/info` | Get information about connecting to Ozon Delivery | `SellerAPI_SellerOzonLogisticsInfo` |


### CategoryAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/description-category/tree` | Tree of product category and type | `DescriptionCategoryAPI_GetTree` |
| `POST /v1/description-category/attribute` | Category characteristics list | `DescriptionCategoryAPI_GetAttributes` |
| `POST /v1/description-category/attribute/values` | Characteristics value directory | `DescriptionCategoryAPI_GetAttributeValues` |
| `POST /v1/description-category/attribute/values/search` | Search by reference values of a characteristic | `DescriptionCategoryAPI_SearchAttributeValues` |


### ProductAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v3/product/import` | Create or update a product | `ProductAPI_ImportProductsV3` |
| `POST /v1/product/import/info` | Get the product import status | `ProductAPI_GetImportProductsInfo` |
| `POST /v1/product/import-by-sku` | Create a product by SKU | `ProductAPI_ImportProductsBySKU` |
| `POST /v1/product/attributes/update` | Update product characteristics | `ProductAPI_ProductUpdateAttributes` |
| `POST /v1/product/pictures/import` | Upload and update product images | `ProductAPI_ProductImportPictures` |
| `POST /v3/product/list` | List of products | `ProductAPI_GetProductList` |
| `POST /v1/product/rating-by-sku` | Get products' content rating by SKU | `ProductAPI_GetProductRatingBySku` |
| `POST /v3/product/info/list` | Get a list of products by identifiers | `ProductAPI_GetProductInfoList` |
| `POST /v4/product/info/attributes` | Get a description of the product characteristics | `ProductAPI_GetProductAttributesV4` |
| `POST /v1/product/info/description` | Get product description | `ProductAPI_GetProductInfoDescription` |
| `POST /v4/product/info/limit` | Product range limit, limits on product creation and update | `ProductAPI_GetUploadQuota` |
| `POST /v1/product/update/offer-id` | Change product identifiers from the seller's system | `ProductAPI_ProductUpdateOfferID` |
| `POST /v1/product/archive` | Archive a product | `ProductAPI_ProductArchive` |
| `POST /v1/product/unarchive` | Unarchive a product | `ProductAPI_ProductUnarchive` |
| `POST /v2/products/delete` | Remove a product without an SKU from the archive | `ProductAPI_DeleteProducts` |
| `POST /v1/product/info/subscription` | Number of users subscribed to product availability alerts | `ProductAPI_GetProductInfoSubscription` |
| `POST /v1/product/related-sku/get` | Get related SKUs | `ProductAPI_ProductGetRelatedSKU` |
| `POST /v2/product/pictures/info` | Get products images | `ProductAPI_ProductInfoPicturesV2` |
| `POST /v1/product/info/wrong-volume` | List of products with incorrect VWC | `ProductAPI_ProductInfoWrongVolume` |


### BarcodeAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/barcode/add` | Bind barcodes to products | `add-barcode` |
| `POST /v1/barcode/generate` | Generate barcodes for products | `generate-barcode` |


### Prices&StocksAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/products/stocks` | Update the quantity of products in stock | `ProductAPI_ProductsStocksV2` |
| `POST /v4/product/info/stocks` | Information about product quantity | `ProductAPI_GetProductInfoStocks` |
| `POST /v1/product/info/warehouse/stocks` | Get information on stock in FBS and rFBS warehouse | `ProductInfoWarehouseStocks` |
| `POST /v1/product/info/stocks-by-warehouse/fbs` | Stocks in seller's warehouses (FBS и rFBS) | `ProductAPI_ProductStocksByWarehouseFbs` |
| `POST /v1/product/import/prices` | Update prices | `ProductAPI_ImportProductsPrices` |
| `POST /v1/product/action/timer/update` | Update the minimum price relevance timer | `ProductAPI_ActionTimerUpdate` |
| `POST /v1/product/action/timer/status` | Get status of timer you've set | `ProductAPI_ActionTimerStatus` |
| `POST /v5/product/info/prices` | Get product price information | `ProductAPI_GetProductInfoPrices` |
| `POST /v1/product/info/discounted` | Get information about the markdown and the main product by the markdown product SKU | `ProductAPI_GetProductInfoDiscounted` |
| `POST /v1/product/update/discount` | Set a discount on a markdown product | `ProductAPI_ProductUpdateDiscount` |


### Promos

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `GET /v1/actions` | Available special offers | `` |
| `POST /v1/actions/candidates` | Products that can participate in a special offer | `PromosCandidates` |
| `POST /v1/actions/products` | Products in a special offer | `PromosProducts` |
| `POST /v1/actions/products/activate` | Add products to special offer | `PromosProductsActivate` |
| `POST /v1/actions/products/deactivate` | Remove products from special offer | `PromosProductsDeactivate` |
| `POST /v1/actions/discounts-task/list` | List of discount requests | `promos_task_list` |
| `POST /v1/actions/discounts-task/approve` | Approve a discount request | `promos_task_approve` |
| `POST /v1/actions/discounts-task/decline` | Decline a discount request | `promos_task_decline` |


### PricingStrategyAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/pricing-strategy/competitors/list` | List of competitors | `pricing_competitors` |
| `POST /v1/pricing-strategy/list` | List of strategies | `pricing_list` |
| `POST /v1/pricing-strategy/create` | Create a pricing strategy | `pricing_create` |
| `POST /v1/pricing-strategy/info` | Strategy info | `pricing_info` |
| `POST /v1/pricing-strategy/update` | Update strategy | `pricing_update` |
| `POST /v1/pricing-strategy/products/add` | Bind products to a strategy | `pricing_items-add` |
| `POST /v1/pricing-strategy/strategy-ids-by-product-ids` | List of strategy identifiers | `pricing_ids` |
| `POST /v1/pricing-strategy/products/list` | List of products in a strategy | `pricing_items-list` |
| `POST /v1/pricing-strategy/product/info` | Competitor's product price | `pricing_items-info` |
| `POST /v1/pricing-strategy/products/delete` | Remove products from a strategy | `pricing_items-delete` |
| `POST /v1/pricing-strategy/status` | Change strategy status | `pricing_status` |
| `POST /v1/pricing-strategy/delete` | Delete a pricing strategy | `pricing_delete` |


### BrandAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/brand/company-certification/list` | List of certified brands | `BrandAPI_BrandCompanyCertificationList` |


### CertificationAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `GET /v1/product/certificate/accordance-types` | List of accordance types (version 1) | `ProductAPI_ProductCertificateAccordanceTypes` |
| `GET /v2/product/certificate/accordance-types/list` | List of accordance types (version 2) | `CertificateAccordanceTypes` |
| `GET /v1/product/certificate/types` | Directory of document types | `ProductAPI_ProductCertificateTypes` |
| `POST /v2/product/certification/list` | List of certified categories | `ProductAPI_ProductCertificationList` |
| `POST /v1/product/certification/list` | List of certified categories | `ProductAPI_V1ProductCertificationList` |
| `POST /v1/product/certificate/create` | Adding certificates for products | `ProductAPI_ProductCertificateCreate` |
| `POST /v1/product/certificate/bind` | Link the certificate to the product | `ProductAPI_ProductCertificateBind` |
| `POST /v1/product/certificate/delete` | Delete certificate | `CertificateDelete` |
| `POST /v1/product/certificate/info` | Certificate information | `CertificateInfo` |
| `POST /v1/product/certificate/list` | Certificates list | `CertificateList` |
| `POST /v1/product/certificate/product_status/list` | Product statuses list | `ProductStatusList` |
| `POST /v1/product/certificate/products/list` | List of products associated with the certificate | `CertificateProductsList` |
| `POST /v1/product/certificate/unbind` | Unbind products from a certificate | `CertificateUnbind` |
| `POST /v1/product/certificate/rejection_reasons/list` | Possible certificate rejection reasons | `RejectionReasonsList` |
| `POST /v1/product/certificate/status/list` | Possible certificate statuses | `CertificateStatusList` |


### WarehouseAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/warehouse/list` | List of warehouses | `WarehouseAPI_WarehouseList` |
| `POST /v1/delivery-method/list` | List of delivery methods for a warehouse | `WarehouseAPI_DeliveryMethodList` |
| `POST /v2/delivery-method/list` | List of delivery methods for realFBS warehouses | `WarehouseAPI_DeliveryMethodListV2` |
| `POST /v2/warehouse/list` | List of warehouses | `WarehouseListV2` |
| `POST /v1/warehouse/operation/status` | Get operation status | `GetWarehouseFBSOperationStatus` |
| `POST /v1/warehouse/archive` | Archive a warehouse | `ArchiveWarehouseFBS` |
| `POST /v1/warehouse/unarchive` | Remove warehouse from archive | `UnarchiveWarehouseFBS` |
| `POST /v1/warehouse/invalid-products/get` | Get list of products with FBS delivery restrictions. | `WarehouseInvalidProductsGet` |
| `POST /v1/warehouse/warehouses-with-invalid-products` | Get list of warehouses with products restricted for delivery | `WarehouseWithInvalidProducts` |


### FBSWarehouseSetup

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/warehouse/fbs/create/drop-off/list` | Get a list of drop-off points to create a warehouse | `WarehouseAPI_ListDropOffPointsForCreateFBSWarehouse` |
| `POST /v1/warehouse/fbs/update/drop-off/list` | Get a list of drop-off points for changing warehouse details | `WarehouseAPI_ListDropOffPointsForUpdateFBSWarehouse` |
| `POST /v1/warehouse/fbs/create/drop-off/timeslot/list` | Get list of time slots for creating warehouse with drop-off shipment | `WarehouseFbsCreateDropOffTimeslotList` |
| `POST /v1/warehouse/fbs/update/drop-off/timeslot/list` | Get list of time slots for updating warehouse with drop-off shipment | `WarehouseFbsUpdateDropOffTimeslotList` |
| `POST /v1/warehouse/fbs/create/pick-up/timeslot/list` | Get list of time slots for creating warehouse with pick-up shipment | `WarehouseFbsCreatePickUpTimeslotList` |
| `POST /v1/warehouse/fbs/update/pick-up/timeslot/list` | Get list of time slots for updating warehouse with pick-up shipment | `WarehouseFbsUpdatePickUpTimeslotList` |
| `POST /v1/warehouse/fbs/create` | Create a warehouse | `WarehouseAPI_CreateWarehouseFBS` |
| `POST /v1/warehouse/fbs/update` | Update warehouse | `UpdateWarehouseFBS` |
| `POST /v1/warehouse/fbs/pickup/courier/create` | Create courier request for pickup shipments | `WarehouseFbsPickUpCourierCreate` |
| `POST /v1/warehouse/fbs/pickup/courier/cancel` | Cancel courier request for pickup shipments | `WarehouseFbsPickUpCourierCancel` |
| `POST /v1/warehouse/fbs/first-mile/update` | Update first mile | `UpdateWarehouseFBSFirstMile` |
| `POST /v1/warehouse/fbs/pickup/history/list` | Get history of shippings to couriers | `WarehouseFbsPickUpHistoryList` |
| `POST /v1/warehouse/fbs/pickup/planning/list` | Get warehouse list for courier delivery planning | `WarehouseFbsPickUpPlanningList` |


### FBS

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v3/posting/fbs/unfulfilled/list` | List of unprocessed shipments | `PostingAPI_GetFbsPostingUnfulfilledList` |
| `POST /v4/posting/fbs/unfulfilled/list` | Get list of unprocessed shipments | `PostingFbsUnfulfilledList` |
| `POST /v3/posting/fbs/list` | Shipments list | `PostingAPI_GetFbsPostingListV3` |
| `POST /v4/posting/fbs/list` | Get shipment list | `PostingFbsList` |
| `POST /v3/posting/fbs/get` | Get shipment details by identifier (version 3) | `PostingAPI_GetFbsPostingV3` |
| `POST /v2/posting/fbs/get-by-barcode` | Get shipment data by barcode | `PostingAPI_GetFbsPostingByBarcode` |
| `POST /v3/posting/multiboxqty/set` | Specify number of boxes for multi-box shipments | `PostingAPI_PostingMultiBoxQtySetV3` |
| `POST /v2/posting/fbs/product/country/list` | List of manufacturing countries | `PostingAPI_ListCountryProductFbsPostingV2` |
| `POST /v2/posting/fbs/product/country/set` | Set the manufacturing country | `PostingAPI_SetCountryProductFbsPostingV2` |
| `POST /v1/posting/fbs/restrictions` | Get drop-off point restrictions | `PostingAPI_GetRestrictions` |
| `POST /v2/posting/fbs/package-label` | Print the labeling | `PostingAPI_PostingFBSPackageLabel` |
| `POST /v1/posting/fbs/package-label/create` | Create a task to generate labeling | `PostingAPI_CreateLabelBatch` |
| `POST /v2/posting/fbs/package-label/create` | Create a task to generate a label | `PostingAPI_CreateLabelBatchV2` |
| `POST /v1/posting/fbs/package-label/get` | Get a labeling file | `PostingAPI_GetLabelBatch` |
| `POST /v1/posting/fbs/cancel-reason` | Shipment cancellation reasons | `PostingAPI_GetPostingFbsCancelReasonV1` |
| `POST /v2/posting/fbs/cancel-reason/list` | Shipments cancellation reasons | `PostingAPI_GetPostingFbsCancelReasonList` |
| `POST /v2/posting/fbs/product/cancel` | Cancel sending some products in the shipment | `PostingAPI_CancelFbsPostingProduct` |
| `POST /v2/posting/fbs/cancel` | Cancel the shipment | `PostingAPI_CancelFbsPosting` |
| `POST /v2/posting/fbs/arbitration` | Open a dispute over a shipment | `PostingAPI_MoveFbsPostingToArbitration` |
| `POST /v2/posting/fbs/awaiting-delivery` | Pass the shipment to shipping | `PostingAPI_MoveFbsPostingToAwaitingDelivery` |
| `POST /v1/posting/fbs/pick-up-code/verify` | Verify courier code | `PostingAPI_PostingFBSPickupCodeVerify` |
| `POST /v1/posting/global/etgb` | ETGB customs declarations | `PostingAPI_GetEtgb` |
| `POST /v1/posting/unpaid-legal/product/list` | List of unpaid products from legal entities | `PostingAPI_UnpaidLegalProductList` |


### PolygonAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/polygon/create` | Create delivery polygon | `PolygonAPI_CreatePolygon` |
| `POST /v1/polygon/bind` | Link delivery method to a delivery polygon | `PolygonAPI_BindPolygon` |


### FBO

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/posting/fbo/list` | Shipments list | `PostingAPI_GetFboPostingList` |
| `POST /v3/posting/fbo/list` | Get shipment list | `PostingFboList` |
| `POST /v2/posting/fbo/get` | Shipment details | `PostingAPI_GetFboPosting` |
| `POST /v1/posting/fbo/cancel-reason/list` | Shipments cancellation reasons by FBO scheme | `PostingAPI_GetPostingFboCancelReasonList` |
| `POST /v1/supply-order/status/counter` | Number of supply requests by status | `SupplyOrderAPI_SupplyOrderStatusCounter` |
| `POST /v1/supply-order/bundle` | Supply or supply request contents | `SupplyOrderBundle` |
| `POST /v3/supply-order/list` | List of supply requests to the Ozon warehouse | `SupplyOrderList` |
| `POST /v3/supply-order/get` | Supply request details | `SupplyOrderGet` |
| `POST /v1/supply-order/timeslot/get` | Supply time slots | `SupplyOrderAPI_GetSupplyOrderTimeslots` |
| `POST /v1/supply-order/timeslot/update` | Update supply time slot | `SupplyOrderAPI_UpdateSupplyOrderTimeslot` |
| `POST /v1/supply-order/timeslot/status` | Supply time slot status | `SupplyOrderAPI_GetSupplyOrderTimeslotStatus` |
| `POST /v1/supply-order/pass/create` | Specify driver and vehicle details | `SupplyOrderAPI_SupplyOrderPassCreate` |
| `POST /v1/supply-order/pass/status` | Driver and vehicle details entry status | `SupplyOrderAPI_SupplyOrderPassStatus` |
| `GET /v1/supplier/available_warehouses` | Ozon warehouses workload | `SupplierAPI_SupplierAvailableWarehouses` |


### FboSupplyRequest

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/cluster/list` | Information about clusters and their warehouses | `SupplyDraftAPI_DraftClusterList` |
| `POST /v1/warehouse/fbo/list` | Finding points to ship the supply | `SupplyDraftAPI_DraftGetWarehouseFboList` |
| `POST /v1/draft/create` | Create a supply request draft | `SupplyDraftAPI_DraftCreate` |
| `POST /v1/draft/create/info` | Supply request draft details | `SupplyDraftAPI_DraftCreateInfo` |
| `POST /v1/draft/timeslot/info` | Available supply time slots | `SupplyDraftAPI_DraftTimeslotInfo` |
| `POST /v1/draft/supply/create` | Create a supply request from the draft | `SupplyDraftAPI_DraftSupplyCreate` |
| `POST /v1/draft/supply/create/status` | Supply request creating details | `SupplyDraftAPI_DraftSupplyCreateStatus` |
| `POST /v1/supply-order/cancel` | Cancel supply request | `SupplyOrderAPI_SupplyOrderCancel` |
| `POST /v1/supply-order/cancel/status` | Get status of canceled supply request | `SupplyOrderAPI_SupplyOrderCancelStatus` |


### FBS&rFBSMarks

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v4/posting/fbs/ship` | Pack the order (version 4) | `PostingAPI_ShipFbsPostingV4` |
| `POST /v4/posting/fbs/ship/package` | Shipment partial package (version 4) | `PostingAPI_ShipFbsPostingPackage` |
| `POST /v6/fbs/posting/product/exemplar/set` | Check and save items data | `PostingAPI_FbsPostingProductExemplarSetV6` |
| `POST /v6/fbs/posting/product/exemplar/create-or-get` | Get created items data | `PostingAPI_FbsPostingProductExemplarCreateOrGetV6` |
| `POST /v5/fbs/posting/product/exemplar/status` | Get statuses of product items check | `PostingAPI_FbsPostingProductExemplarStatusV5` |
| `POST /v5/fbs/posting/product/exemplar/validate` | Validate labeling codes | `PostingAPI_FbsPostingProductExemplarValidateV5` |
| `POST /v1/fbs/posting/product/exemplar/update` | Update items data | `PostingAPI_FbsPostingProductExemplarUpdate` |


### DeliveryFBS

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/carriage/create` | Create shipping | `CarriageAPI_CarriageCreate` |
| `POST /v1/carriage/approve` | Confirm shipping | `CarriageAPI_CarriageApprove` |
| `POST /v1/carriage/set-postings` | Change shipping composition | `CarriageAPI_SetPostings` |
| `POST /v1/carriage/cancel` | Delete shipping | `CarriageAPI_CarriageCancel` |
| `POST /v1/carriage/delivery/list` | List of delivery methods and shipments | `CarriageAPI_CarriageDeliveryList` |
| `POST /v2/carriage/delivery/list` | List of delivery methods and shippings | `CarriageDeliveryListV2` |
| `POST /v2/posting/fbs/act/create` | Create an acceptance and transfer certificate and a waybill | `PostingAPI_PostingFBSActCreate` |
| `POST /v1/posting/carriage-available/list` | List of available shippings | `PostingAPI_GetCarriageAvailableList` |
| `POST /v1/carriage/get` | Shipping details | `CarriageGet` |
| `POST /v1/posting/fbs/split` | Split the order into shipments without picking | `FbsSplit` |
| `POST /v2/posting/fbs/act/get-postings` | List of shipments in the certificate | `PostingAPI_ActPostingList` |
| `POST /v2/posting/fbs/act/get-container-labels` | Package unit labels | `PostingAPI_PostingFBSActGetContainerLabels` |
| `POST /v2/posting/fbs/act/get-barcode` | Barcode for product shipping | `PostingAPI_PostingFBSGetBarcode` |
| `POST /v2/posting/fbs/act/get-barcode/text` | Value of barcode for product shipping | `PostingAPI_PostingFBSGetBarcodeText` |
| `POST /v2/posting/fbs/digital/act/check-status` | Generating status of digital acceptance and transfer certificate and waybill | `PostingAPI_PostingFBSDigitalActCheckStatus` |
| `POST /v2/posting/fbs/act/get-pdf` | Get acceptance and transfer certificate and waybill | `PostingAPI_PostingFBSGetAct` |
| `POST /v2/posting/fbs/act/list` | List of shipping certificates | `PostingAPI_FbsActList` |
| `POST /v2/posting/fbs/digital/act/get-pdf` | Get digital shipping certificate | `PostingAPI_PostingFBSGetDigitalAct` |
| `POST /v2/posting/fbs/act/check-status` | Status of acceptance and transfer certificate and waybill | `PostingAPI_PostingFBSActCheckStatus` |
| `POST /v1/posting/fbs/traceable/split` | Split shipment with traceable products | `PostingFbsTraceableSplit` |
| `POST /v1/posting/fbs/product/traceable/attribute` | Get list of empty attributes for traceable products | `PostingFbsProductTraceableAttribute` |
| `POST /v1/carriage/ettn/status` | Get electronic waybill verification status for traceable FBS shipping | `CarriageEttnStatus` |
| `POST /v1/assembly/carriage/posting/list` | Get list of shipments in shipping | `AssemblyCarriagePostingList` |
| `POST /v1/assembly/carriage/product/list` | Get list of products in shipping | `AssemblyCarriageProductList` |
| `POST /v1/assembly/fbs/posting/list` | Get shipment list | `AssemblyFbsPostingList` |
| `POST /v1/assembly/fbs/product/list` | Get list of products in shipments | `AssemblyFbsProductList` |


### DeliveryrFBS

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/fbs/posting/tracking-number/set` | Add tracking numbers | `PostingAPI_FbsPostingTrackingNumberSet` |
| `POST /v2/fbs/posting/delivering` | Change the status to "Delivering" | `PostingAPI_FbsPostingDelivering` |
| `POST /v2/fbs/posting/last-mile` | Change the status to "Last Mile" | `PostingAPI_FbsPostingLastMile` |
| `POST /v2/fbs/posting/delivered` | Change the status to "Delivered" | `PostingAPI_FbsPostingDelivered` |
| `POST /v1/posting/fbs/timeslot/change-restrictions` | Dates available for delivery reschedule | `PostingAPI_PostingTimeslotChangeRestrictions` |
| `POST /v1/posting/fbs/timeslot/set` | Reschedule shipment delivery date | `PostingAPI_SetPostingTimeslot` |
| `POST /v1/posting/cutoff/set` | Specify shipping date | `PostingAPI_SetPostingCutoff` |


### Pass

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/pass/list` | List of passes | `PassList` |
| `POST /v1/carriage/pass/create` | Create a pass | `carriagePassCreate` |
| `POST /v1/carriage/pass/update` | Update pass | `carriagePassUpdate` |
| `POST /v1/carriage/pass/delete` | Delete pass | `carriagePassDelete` |
| `POST /v1/return/pass/create` | Create a return pass | `returnPassCreate` |
| `POST /v1/return/pass/update` | Update return pass | `returnPassUpdate` |
| `POST /v1/return/pass/delete` | Delete return pass | `returnPassDelete` |


### ReturnsAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/returns/list` | Information about FBO and FBS returns | `returnsList` |


### ReturnAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/returns/company/fbs/info` | FBS returns quantity | `returnsCompanyFBSInfo` |
| `POST /v1/return/giveout/is-enabled` | Check the ability to receive return shipments by barcode | `ReturnAPI_GiveoutIsEnabled` |
| `POST /v1/return/giveout/list` | Return shipments list | `ReturnAPI_GiveoutList` |
| `POST /v1/return/giveout/info` | Information on return shipment | `ReturnAPI_GiveoutInfo` |
| `POST /v1/return/giveout/barcode` | Value of barcode for return shipments | `ReturnAPI_GiveoutGetBarcode` |
| `POST /v1/return/giveout/get-pdf` | Barcode for return shipment in PDF format | `ReturnAPI_GiveoutGetPDF` |
| `POST /v1/return/giveout/get-png` | Barcode for return shipment in PNG format | `ReturnAPI_GiveoutGetPNG` |
| `POST /v1/return/giveout/barcode-reset` | Generate new barcode | `ReturnAPI_GiveoutBarcodeReset` |


### RFBSReturnsAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/returns/rfbs/list` | Get a list of return requests | `RFBSReturnsAPI_ReturnsRfbsListV2` |
| `POST /v2/returns/rfbs/get` | Get information about a return request | `RFBSReturnsAPI_ReturnsRfbsGetV2` |
| `POST /v2/returns/rfbs/reject` | Reject a return request | `RFBSReturnsAPI_ReturnsRfbsRejectV2` |
| `POST /v2/returns/rfbs/compensate` | Compensate partial cost | `RFBSReturnsAPI_ReturnsRfbsCompensateV2` |
| `POST /v2/returns/rfbs/verify` | Approve a return request | `RFBSReturnsAPI_ReturnsRfbsVerifyV2` |
| `POST /v2/returns/rfbs/receive-return` | Confirm receipt of a product for check | `RFBSReturnsAPI_ReturnsRfbsReceiveReturnV2` |
| `POST /v2/returns/rfbs/return-money` | Refund the customer | `RFBSReturnsAPI_ReturnsRfbsReturnMoneyV2` |
| `POST /v1/returns/rfbs/action/set` | Pass available actions for rFBS returns | `ReturnsAPI_ReturnsRfbsActionSet` |


### CancellationAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/conditional-cancellation/list` | Get a list of rFBS cancellation requests | `CancellationAPI_GetConditionalCancellationListV2` |
| `POST /v2/conditional-cancellation/approve` | Approve rFBS cancellation request | `CancellationAPI_ConditionalCancellationApproveV2` |
| `POST /v2/conditional-cancellation/reject` | Reject rFBS cancellation request | `CancellationAPI_ConditionalCancellationRejectV2` |


### ChatAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/chat/send/file` | Send file | `ChatAPI_ChatSendFile` |
| `POST /v2/chat/list` | Chats list | `ChatAPI_ChatListV2` |
| `POST /v3/chat/list` | Chats list | `ChatAPI_ChatListV3` |
| `POST /v3/chat/history` | Chat history | `ChatAPI_ChatHistoryV3` |


### SupplierAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/invoice/create-or-update` | Create or edit an invoice | `InvoiceAPI_InvoiceCreateOrUpdateV2` |
| `POST /v1/invoice/file/upload` | Invoice upload | `invoice_upload` |
| `POST /v2/invoice/get` | Get invoice information | `invoice_getV2` |
| `POST /v1/invoice/delete` | Delete invoice link | `invoice_delete` |


### ReportAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/report/info` | Report details | `ReportAPI_ReportInfo` |
| `POST /v1/report/list` | Reports list | `ReportAPI_ReportList` |
| `POST /v1/report/products/create` | Products report | `ReportAPI_CreateCompanyProductsReport` |
| `POST /v2/report/returns/create` | Report on returns | `ReportAPI_ReportReturnsCreate` |
| `POST /v1/report/postings/create` | Shipment report | `ReportAPI_CreateCompanyPostingsReport` |
| `POST /v1/finance/cash-flow-statement/list` | Financial report | `FinanceAPI_FinanceCashFlowStatementList` |
| `POST /v1/report/discounted/create` | Report on markdown products | `ReportAPI_CreateDiscountedReport` |
| `POST /v1/report/warehouse/stock` | Report on FBS warehouse stocks | `ReportAPI_CreateStockByWarehouseReport` |
| `POST /v1/report/placement/by-products/create` | Get report on storage cost by products | `CreatePlacementByProductsReport` |
| `POST /v1/report/placement/by-supplies/create` | Get report on storage cost by supplies | `CreatePlacementBySuppliesReport` |
| `POST /v1/report/marked-products-sales/create` | Generate sales report of labeled products | `CreateCompanyMarkedProductsSalesReport` |


### AnalyticsAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/analytics/stock_on_warehouses` | Stocks and products report | `AnalyticsAPI_AnalyticsGetStockOnWarehousesV2` |
| `POST /v1/analytics/turnover/stocks` | Product turnover | `AnalyticsAPI_StocksTurnover` |
| `POST /v1/analytics/average-delivery-time` | Get analytics of average delivery time | `AnalyticsAPI_AverageDeliveryTime` |
| `POST /v1/analytics/average-delivery-time/details` | Get detailed analytics of average delivery time | `AnalyticsAPI_AverageDeliveryTimeDetails` |
| `POST /v1/analytics/average-delivery-time/summary` | Get general analytics of average delivery time | `AverageDeliveryTimeSummary` |
| `POST /v1/analytics/stocks` | Get analytics on stock balances | `AnalyticsAPI_AnalyticsStocks` |


### FinanceAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v2/finance/realization` | Sales report (version 2) | `FinanceAPI_GetRealizationReportV2` |
| `POST /v1/finance/realization/posting` | Sales report by order | `FinanceAPI_GetRealizationReportV1` |
| `POST /v3/finance/transaction/list` | Transactions list | `FinanceAPI_FinanceTransactionListV3` |
| `POST /v3/finance/transaction/totals` | Total transactions sum | `FinanceAPI_FinanceTransactionTotalV3` |
| `POST /v1/finance/document-b2b-sales` | Legal entities sales register | `ReportAPI_CreateDocumentB2BSalesReport` |
| `POST /v1/finance/document-b2b-sales/json` | Legal entities sales register in JSON format | `ReportAPI_CreateDocumentB2BSalesJSONReport` |
| `POST /v1/finance/mutual-settlement` | Mutual settlements report | `ReportAPI_CreateMutualSettlementReport` |
| `POST /v1/finance/products/buyout` | Purchased product report | `GetFinanceProductsBuyout` |
| `POST /v1/finance/compensation` | Compensation report | `ReportAPI_GetCompensationReport` |
| `POST /v1/finance/decompensation` | Decompensation report | `ReportAPI_GetDecompensationReport` |


### Receipt

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/receipts/get` | Get receipt in PDF format | `GetReceipt` |
| `POST /v1/receipts/seller/list` | Get list of seller receipts | `ReceiptsSellerList` |
| `POST /v1/receipts/upload` | Upload receipt | `UploadReceipt` |


### SellerRating

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/rating/summary` | Get information on current seller ratings | `RatingAPI_RatingSummaryV1` |
| `POST /v1/rating/history` | Get information on seller ratings for the period | `RatingAPI_RatingHistoryV1` |
| `POST /v1/rating/index/fbs/info` | Get FBS and rFBS error index | `RatingAPI_GetFBSRatingIndexInfoV1` |
| `POST /v1/rating/index/fbs/posting/list` | List of shipments that affected FBS and rFBS error index | `RatingAPI_ListFBSRatingIndexPostingsV1` |


### Digital

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/posting/digital/codes/upload` | Upload digital product codes for shipping | `UploadPostingCodes` |
| `POST /v1/posting/digital/list` | Get shipments list | `ListPostingCodes` |
| `POST /v1/product/digital/stocks/import` | Update quantity of digital products | `DigitalProductAPI_StocksImport` |

## Beta Methods


### BetaMethod

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/analytics/manage/stocks` | Stock management | `AnalyticsAPI_ManageStocks` |
| `POST /v1/removal/from-supply/list` | Report on removal and disposal from FBO supply | `GetSupplyReturnsSummaryReport` |
| `POST /v1/removal/from-stock/list` | Report on removal and disposal from FBO stock | `GetSupplierReturnsSummaryReport` |
| `POST /v1/product/stairway-discount/by-quantity/set` | Manage quantity discounts | `ProductAPI_SetProductStairwayDiscountByQuantity` |
| `POST /v1/product/stairway-discount/by-quantity/get` | Get quantity discount information | `ProductAPI_GetProductStairwayDiscountByQuantity` |
| `POST /v1/finance/balance` | Get balance report | `GetFinanceBalanceV1` |
| `POST /v2/actions/discounts-task/list` | Get list of discount requests | `GetDiscountTaskListV2` |
| `POST /v2/cluster/list` | Get information about macrolocal clusters | `DraftClusterList` |
| `POST /v1/description-category/tips` | Get tips to identify product category | `DescriptionCategoryTips` |
| `POST /v1/product/visibility/set` | Set product visibility on Ozon and Ozon Select storefronts | `ProductVisibilitySet` |
| `POST /v2/posting/digital/list` | Get shipment list | `PostingDigitalList` |


### Quants

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/product/quant/list` | Economy products list | `QuantProductList` |
| `POST /v1/product/quant/info` | Economy product information | `QuantGetInfo` |


### ReviewAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/review/comment/create` | Leave a comment on the review | `ReviewAPI_CommentCreate` |
| `POST /v1/review/comment/delete` | Delete a comment on a review | `ReviewAPI_CommentDelete` |
| `POST /v1/review/comment/list` | List of comments for the review | `ReviewAPI_CommentList` |
| `POST /v1/review/change-status` | Change review status | `ReviewAPI_ReviewChangeStatus` |
| `POST /v1/review/count` | Number of reviews by status | `ReviewAPI_ReviewCount` |
| `POST /v1/review/info` | Get review details | `ReviewAPI_ReviewInfo` |
| `POST /v1/review/list` | Get a list of reviews | `ReviewAPI_ReviewList` |


### Questions&Answers

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/question/answer/create` | Create answer to question | `QuestionAnswer_Create` |
| `POST /v1/question/answer/delete` | Delete answer to question | `QuestionAnswer_Delete` |
| `POST /v1/question/answer/list` | List of answers to question | `QuestionAnswer_List` |
| `POST /v1/question/change-status` | Change question statuses | `Question_ChangeStatus` |
| `POST /v1/question/count` | Number of questions by statuses | `Question_Count` |
| `POST /v1/question/info` | Question details | `Question_Info` |
| `POST /v1/question/list` | Question list | `Question_List` |
| `POST /v1/question/top-sku` | Products with the most questions | `Question_TopSku` |


### SellerActions

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/seller-actions/create/discount` | Create special offer with "Discount" mechanics | `SellerActionsCreateDiscount` |
| `POST /v1/seller-actions/create/discount-with-condition` | Create special offer with "Discount of order amount" mechanics | `SellerActionsCreateDiscountWithCondition` |
| `POST /v1/seller-actions/create/installment` | Create special offer with "Interest-free installment" mechanics | `SellerActionsCreateInstallment` |
| `POST /v1/seller-actions/create/multi-level-discount` | Create special offer with "Multi-level discount from the amount" mechanics | `SellerActionsCreateMultiLevelDiscount` |
| `POST /v1/seller-actions/create/voucher` | Create special offer with "Discount by promo code" mechanics | `SellerActionsCreateVoucher` |
| `POST /v1/seller-actions/update/discount` | Update special offer with "Discount" mechanic | `SellerActionsUpdateDiscount` |
| `POST /v1/seller-actions/update/discount-with-condition` | Update special offer with "Discount of order amount" mechanic | `SellerActionsUpdateDiscountWithCondition` |
| `POST /v1/seller-actions/update/installment` | Update special offer with "Interest-free installment" mechanic | `SellerActionsUpdateInstallment` |
| `POST /v1/seller-actions/update/multi-level-discount` | Update special offer with "Multi-level discount from the total amount" mechanic | `SellerActionsUpdateMultiLevelDiscount` |
| `POST /v1/seller-actions/update/voucher` | Update special offer with "Discount by promo code" mechanic | `SellerActionsUpdateVoucher` |
| `POST /v1/seller-actions/products/add` | Add products to special offer | `SellerActionsProductsAdd` |
| `POST /v1/seller-actions/products/candidates` | Get list of products that can participate in special offer | `SellerActionsProductsCandidates` |
| `POST /v1/seller-actions/products/delete` | Remove products from special offer | `SellerActionsProductsDelete` |
| `POST /v1/seller-actions/products/list` | Get list of products in special offer | `SellerActionsProductsList` |
| `POST /v1/seller-actions/archive` | Archive special offer | `SellerActionsArchive` |
| `POST /v1/seller-actions/change-activity` | Enable or disable special offer | `SellerActionsChangeActivity` |
| `POST /v1/seller-actions/list` | Get list of special offers | `SellerActionsList` |
| `POST /v1/seller-actions/voucher/get` | Get file with promo codes in CSV format | `SellerActionsVoucherGet` |


### Notification

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/notification/set` | Connect URL for notifications | `SetNotification` |
| `POST /v1/notification/update` | Change URL for notifications | `UpdateNotification` |
| `POST /v1/notification/delete` | Delete URL for notifications | `DeleteNotification` |
| `POST /v1/notification/check` | Check URL for notifications | `CheckNotification` |
| `POST /v1/notification/enable` | Enable or disable URL for notifications | `EnableNotification` |
| `POST /v1/notification/list` | Get information about connected URLs | `NotificationList` |
| `POST /v1/notification/push-type/list` | Get push notification types | `GetNotificationPushTypeList` |


### DeliveryFBPDraft

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/warehouse/list` | Get partner warehouses list | `FbpWarehouseList` |
| `POST /v1/fbp/draft/get` | Get information about supply draft | `FbpAPI_FbpDraftGet` |
| `POST /v1/fbp/draft/list` | Supply drafts list | `FbpAPI_FbpDraftList` |


### DraftDirectFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/draft/direct/seller-dlv/create` | Create draft with delivery by seller | `FbpDraftDirectSellerDlvCreate` |
| `POST /v1/fbp/draft/direct/seller-dlv/edit` | Update draft with information about delivery by seller | `FbpDraftDirectSellerDlvEdit` |
| `POST /v1/fbp/draft/direct/timeslot/edit` | Edit time slot in draft | `FbpDraftDirectTimeslotEdit` |
| `POST /v1/fbp/draft/direct/timeslot/get` | Get list of time slots for direct supply | `FbpDraftDirectGetTimeslot` |
| `POST /v1/fbp/draft/direct/create` | Create supply request draft without specifying delivery method | `FbpDraftDirectCreate` |
| `POST /v1/fbp/draft/direct/delete` | Delete draft of supply request | `FbpDraftDirectDelete` |
| `POST /v1/fbp/draft/direct/product/validate` | Validate product list for partner warehouse | `FbpDraftDirectProductValidate` |
| `POST /v1/fbp/draft/direct/registrate` | Transfer draft to current supply | `FbpDraftDirectRegistrate` |
| `POST /v1/fbp/draft/direct/tpl-dlv/create` | Create supply request for delivery by a third-party transport company | `FbpAPI_FbpDraftDirectTplDlvCreate` |
| `POST /v1/fbp/draft/direct/tpl-dlv/edit` | Edit a draft of shipment with a third-party transport company | `FbpAPI_FbpDraftDirectTplDlvEdit` |


### DraftDropOffFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/draft/drop-off/province/list` | Get province list | `FbpDraftDropOffProvinceList` |
| `POST /v1/fbp/draft/drop-off/point/list` | Get list of drop-off points in province | `FbpDraftDropOffPointList` |
| `POST /v1/fbp/draft/drop-off/point/timetable` | Get drop-off point schedule | `FbpDraftDropOffPointTimetable` |
| `POST /v1/fbp/draft/drop-off/product/validate` | Check product list that partner's warehouse can accept | `FbpDraftDropOffProductValidate` |
| `POST /v1/fbp/draft/drop-off/create` | Create draft for delivery to drop-off point | `FbpDraftDropOffCreate` |
| `POST /v1/fbp/draft/drop-off/delete` | Delete draft for delivery to drop-off point | `FbpDraftDropOffDelete` |
| `POST /v1/fbp/draft/drop-off/dlv/edit` | Edit delivery details for drop-off draft | `FbpDraftDropOffDlvEdit` |
| `POST /v1/fbp/draft/drop-off/registrate` | Transfer draft to current supply | `FbpDraftDropOffRegistrate` |


### DraftPickupFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/draft/pick-up/registrate` | Transfer draft to current supply | `FbpDraftPickUpRegistrate` |
| `POST /v1/fbp/draft/pick-up/create` | Create request draft for pick-up supply | `FbpAPI_FbpDraftPickupCreate` |
| `POST /v1/fbp/draft/pick-up/delete` | Cancel request draft for pick-up supply | `FbpAPI_FbpDraftPickUpDelete` |
| `POST /v1/fbp/draft/pick-up/dlv/edit` | Update request draft for pick-up supply | `FbpAPI_FbpDraftPickupDlvEdit` |
| `POST /v1/fbp/draft/pick-up/product/validate` | Validate product list for pick-up supply | `FbpAPI_FbpDraftPickUpProductValidate` |


### OrderDirectFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/order/direct/cancel` | Cancel supply | `FbpAPI_FbpOrderDirectCancel` |
| `POST /v1/fbp/order/direct/seller-dlv/edit` | Update information about delivery by seller | `FbpAPI_FbpOrderDirectSellerDlvEdit` |
| `POST /v1/fbp/order/direct/timeslot/edit` | Edit time slot in supply request | `FbpAPI_FbpEditTimeslot` |
| `POST /v1/fbp/order/direct/timeslot/list` | Get list of time slots for supply | `FbpAPI_FbpAvailableTimeslotList` |


### OrderDropOffFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/order/drop-off/cancel` | Cancel supply to drop-off point | `FbpAPI_FbpOrderDropOffCancel` |
| `POST /v1/fbp/order/drop-off/dlv/edit` | Edit information about supply to drop-off point | `FbpAPI_FbpOrderDropOffDlvEdit` |
| `POST /v1/fbp/order/drop-off/timetable` | Get drop-off point working schedule | `FbpAPI_FbpOrderDropOffTimetable` |


### OrderPickupFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/order/pick-up/cancel` | Cancel pick-up supply | `FbpAPI_FbpOrderPickUpCancel` |
| `POST /v1/fbp/order/pick-up/dlv/edit` | Edit pick-up point details | `FbpAPI_FbpOrderPickUpDlvEdit` |


### DeliveryFBP

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/fbp/act-from/create` | Generate acceptance certificate | `FbpAPI_FbpCreateAct` |
| `POST /v1/fbp/act-from/get` | Get status of acceptance certificate generation | `FbpAPI_FbpCheckActState` |
| `POST /v1/fbp/act-to/create` | Generate waybill | `FbpAPI_FbpCreateConsignmentNote` |
| `POST /v1/fbp/act-to/get` | Get status of waybill generation | `FbpAPI_FbpCheckConsignmentNoteState` |
| `POST /v1/fbp/archive/get` | Get details on completed supply | `FbpAPI_FbpArchiveGet` |
| `POST /v1/fbp/archive/list` | Get list of completed supplies | `FbpAPI_FbpArchiveList` |
| `POST /v1/fbp/label/create` | Create label generation task | `FbpAPI_FbpCreateLabel` |
| `POST /v1/fbp/label/get` | Get status of label generation task | `FbpAPI_FbpGetLabel` |
| `POST /v1/fbp/order/get` | Get information about supply | `FbpAPI_FbpOrderGet` |
| `POST /v1/fbp/order/list` | Get list of supplies | `FbpAPI_FbpOrderList` |
| `POST /v1/posting/fbp/list` | Get shipment list | `PostingFbpList` |

## Premium Methods


### Premium

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/chat/send/message` | Send message | `ChatAPI_ChatSendMessage` |
| `POST /v1/chat/start` | Create a new chat | `ChatAPI_ChatStart` |
| `POST /v2/chat/read` | Mark messages as read | `ChatAPI_ChatReadV2` |
| `POST /v1/analytics/data` | Analytics data | `AnalyticsAPI_AnalyticsGetData` |
| `POST /v1/analytics/product-queries` | Get details on your product queries | `AnalyticsAPI_AnalyticsProductQueries` |
| `POST /v1/analytics/product-queries/details` | Get query details by product | `AnalyticsAPI_AnalyticsProductQueriesDetails` |
| `POST /v1/finance/realization/by-day` | Sales report per day | `FinanceAPI_GetRealizationByDayReportV1` |
| `POST /v1/search-queries/text` | Get list of search queries by text | `SearchQueriesAPI_SearchQueriesText` |
| `POST /v1/search-queries/top` | Get list of popular search queries | `SearchQueriesAPI_SearchQueriesTop` |
| `POST /v1/product/prices/details` | Get details on product prices | `ProductPricesDetails` |

## Ozon Delivery


### CancelReasonAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/cancel-reason/list` | Cancellation reasons for shipments | `CancelReasonList` |
| `POST /v1/cancel-reason/list-by-order` | Order cancellation reasons | `CancelReasonListByOrder` |
| `POST /v1/cancel-reason/list-by-posting` | Cancellation reasons for a shipment | `CancelReasonAPI_CancelReasonListByPosting` |


### DeliveryAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/delivery/check` | Check delivery availability for customer | `DeliveryCheck` |
| `POST /v2/delivery/checkout` | Get available delivery options | `DeliveryCheckout` |
| `POST /v1/delivery/map` | Display points on map | `DeliveryMap` |
| `POST /v1/delivery/point/info` | Get pick-up point information | `DeliveryPointInfo` |
| `POST /v1/delivery/point/list` | Get pick-up point list | `DeliveryAPI_DeliveryPointList` |


### OrderAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/order/cancel` | Cancel order | `OrderAPI_OrderCancel` |
| `POST /v1/order/cancel/check` | Check if order can be canceled | `OrderAPI_OrderCancelCheck` |
| `POST /v1/order/cancel/status` | Get order cancellation status | `OrderAPI_OrderCancelStatus` |
| `POST /v2/order/create` | Create order | `OrderAPI_OrderCreate` |


### FboPostingAPI

| Endpoint | Description | Operation ID |
|----------|-------------|--------------|
| `POST /v1/posting/cancel` | Cancel shipment from order | `PostingAPI_PostingCancel` |
| `POST /v1/posting/cancel/status` | Check shipment cancellation status | `PostingAPI_PostingCancelStatus` |
| `POST /v1/posting/marks` | Get item labels from shipment | `PostingAPI_PostingMarks` |

## Errors

## Push Notifications
