# Description

## Introduction

### What is Ozon Seller API

Ozon Seller API is a software interface for working with the Ozon marketplace and exchanging information between the seller's system and Ozon.

Seller API methods allow you to change store data, such as product stocks and prices, and retrieve data, such as information about returns or a list of warehouses.

Working with the API consists of sending a request and receiving a response. To do this, you can use [Postman](https://www.postman.com/), the **Console** tab above the method description, or set up integration with accounting systems, such as 1C, ERP or MoySklad.

### When the Seller API is useful

Once you integrate the seller's accounting system with the Seller API, you can update and receive data automatically. This helps to keep prices and stocks information up-to-date, process more orders in less time, and eliminate errors caused by manual data updates.

For example, you need to update stocks or prices information for 100 products. To do this using your personal account,
you need to manually change the data on each product card.

However, you can update stock information for 100 products with one request using the [/v2/products/stocks](#operation/ProductAPI_ProductsStocksV2) method, and the [/v1/product/import/prices](#operation/ProductAPI_ImportProductsPrices) method allows you to update their prices.

### What you can do using Seller API features

- Upload and update products.
- Manage prices and product stocks.
- Get information about product returns.
- Manage FBO, FBS and rFBS orders.
- Manage chats.
- Work with invoices.
- Receive financial and analytical information.
- Download Ozon attributes and characteristics.

## Getting started

> **Note:** 
From May 16, 2025, you can only access Seller API back-to-back. Direct browser requests will be prohibited and blocked with the CORS policy error.

> **Note:** 
If the offer is outdated, access to the Seller API is restricted. You'll receive the 403 Offer not signed error.
To regain access, accept the offer.

[Learn more about accepting the offer](https://docs.ozon.ru/global/en/launch/steps/step-2/)

Before you start working with the Ozon Seller API:

1. Explore Ozon processes in the [Help Center](https://docs.ozon.ru/global/en/) and your [personal account](https://seller.ozon.ru/app/signin?locale=en).
2. Choose the processes you want to automate and compare them to your company's processes.
3. Read the Seller API documentation and develop an integration scheme.

To work with the Seller API, you need an API key. Learn more in the [Authorization](#tag/Auth) section.

Seller API works in UTC time. Keep this in mind when sending requests and receiving responses with dates and times.

The process of working with the Seller API can be divided into the following blocks:
- working with the attribute tree,
- uploading and updating products,
- managing prices,
- managing product stocks,
- managing FBO orders,
- managing FBS and rFBS orders,
- working with invoices,
- getting financial information,
- getting analytical information,
- managing returns,
- managing chats.

## Auth

> **Note:** API keys are stored in your personal account in a concealed form. You will see the API key only when creating it. Save it in a safe place or pass it to your systems immediately.

You cannot access unsaved API keys. Create a new key to continue working with the API.

Ozon can revoke the key if it detects suspicious activity or excessive load on its systems.

 

## How to access the Seller API

1. Sign up at the [Ozon Seller](https://seller.ozon.ru/app/signup?locale=en). Learn more about signing up in the [Help Center](https://docs.ozon.ru/global/en/launch/steps/step-1/).
2. Accept the offer.
3. Get an API key.

## How to get an API key

> **Note:** 
Keep your API key confidential  

API queries change the data in your store on Ozon. If any third party has access to the key, they will be able to manage your store.

To get an API key:
1. In your personal account go to [**Settings → Seller API**](https://seller.ozon.ru/app/settings/api-keys?locale=en).
2. Click **Generate key**.
3. Add a name for the key and select its access level.
4. Select the key purpose:
   - for personal use;
   - for an external service or application: enter the service name for which the key is issued.
5. Click **Generate**.

> **Note:** 
The key is valid for 6 months. When it expires, generate a new key in your personal account.

Get the key expiration date in the `expires_at` parameter of the [/v1/roles](#operation/AccessAPI_RolesByToken) method.

You can create several API keys, for example, if you have several users with different access levels.

### Edit API key

If you want to change allowed networks for the key, select it from the [API keys list](https://seller.ozon.ru/app/settings/api-keys) and click .
Make your changes and click **Refresh**.

## If your API key is compromised

1. In your personal account go to [**Settings → Seller API**](https://seller.ozon.ru/app/settings/api-keys?locale=en).
2. Delete the current API key and create a new one.

## OAuth-token

To obtain an OAuth token for Seller API, create a [private](https://docs.ozon.ru/api/applications/#section/Rabota-s-chastnym-prilozheniem) 
or [public](https://docs.ozon.ru/api/applications/#section/Rabota-s-publichnym-prilozheniem) application and grant it 
access to your personal account. Then you get an OAuth token to work with Seller API methods.

Use the OAuth token with Seller API methods according to the access levels assigned to your application.

Example request:

```json
POST https://api-seller.ozon.ru/{Seller API method endpoint}
Authorization: Bearer ACCESS_TOKEN
```

## Environment

## What is the production environment

The production environment is your store at Ozon. All requests sent, except for informational ones, can change the data in your personal account and on the Ozon website.

- **Production environment**: api-seller.ozon.ru

You can see the created products at the link of the following format: https://www.ozon.ru/context/detail/id/SKU, where “SKU” should be replaced with the value for the created product.

## Request format

```http
 GET / HTTP/1.1
 Host: api-seller.ozon.ru
 Client-Id: 
 Api-Key: 
 Content-Type: application/json
```

To make sure that the request format is correct, use [Postman](http://postman.com) or the **Console** tab above the method description.

## Process

> **Note:** If you or someone under your name performs identical or erroneous requests,
we can restrict access to Seller API without warning. 

 You can send a maximum of 50 requests per second to all methods from a single Client ID. 

# Downloading Ozon attributes and characteristics

> **Note:** To make the methods for working with products function correctly, compare your and Ozon categories and characteristics.

If in the [/v3/product/import](#operation/ProductAPI_ImportProductsV3) method you pass values not from the Ozon directory, the product won't be created or updated.

1. Get the list of categories and types in a tree view and use the value from the last level of the selected category: [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree).

2. Get characteristics for the selected category and type: [/v1/description-category/attribute](#operation/DescriptionCategoryAPI_GetAttributes).

3. Get a list of values for the selected characteristic: [/v1/description-category/attribute/values](#operation/DescriptionCategoryAPI_GetAttributeValues).

# Uploading and updating products

After comparing your attributes and characteristics with the Ozon attribute model, you can start uploading products:

1. Upload products and services: [/v3/product/import](#operation/ProductAPI_ImportProductsV3). This method also
   allows you to update already uploaded products. The request sets the primary price and uploads product images.
   You can send up to 100 products in one request. Images should be uploaded as direct links to the cloud storage, where they are stored.

   The method output is a `task_id`, the product upload task identifier.

2. Check the `task_id` that you received while uploading products: [/v1/product/import/info](#operation/ProductAPI_GetImportProductsInfo). The method will return information whether the products were uploaded successfully or there was an error during import.

    If the response contains the `moderating` status, wait for the moderation results and check the product status again. Moderation usually takes less than one day.

3. Get a list of products created after uploading products: [/v3/product/list](#operation/ProductAPI_GetProductList).

   The method allows you to use filters to divide products into groups by visibility status or track their status changes
   using the product identifier.

   The method returns a pair of `offer_id` and `product_id` values. They are needed in almost all queries for identifying the product with which the action will be performed. If you have uploaded products via template, use this method to
   get the `offer_id` and `product_id` in order to work with products via API in the future.

## Uploading and updating product images

To add product images or replace existing ones, use:

1. [/v1/product/pictures/import](#operation/ProductAPI_ProductImportPictures): upload or update product images. Pass direct links to images uploaded to the cloud storage.
2. [/v2/product/pictures/info](#operation/ProductAPI_ProductInfoPicturesV2): check the uploading status.

## Updating products

To update both product information and its characteristics, use the [/v3/product/import](#operation/ProductAPI_ImportProductsV3) method.

If you only need to update the product characteristics, use the [/v1/product/attributes/update](#operation/ProductAPI_ProductUpdateAttributes) method.

## Getting products information

- Get product details, for example barcode, main offer price, category identifier, commission, or moderation errors: 
  [/v3/product/info/list](#operation/ProductAPI_GetProductInfoList). Use filters from the [/v3/product/list](#operation/ProductAPI_GetProductList) method to get a list for all products 
  in bulk or by category.

- Get a product characteristics description: [/v4/product/info/attributes](#operation/ProductAPI_GetProductAttributesV4).
  This method allows you to add extra information about the product to make the product card more complete.

- Get a product description that can be used for creating a similar product: [/v1/product/info/description](#operation/ProductAPI_GetProductInfoDescription).

- Get information about the markdown and the main product by the markdown product SKU: [/v1/product/info/discounted](#operation/ProductAPI_GetProductInfoDiscounted).

## Deleting or archiving products

1. Delete a product if it was uploaded with an error and got into the archive without an SKU: [/v2/products/delete](#operation/ProductAPI_DeleteProducts). 
   Products that have successfully passed moderation and received an SKU cannot be deleted from the archive.

2. Move a product into the archive: [/v1/product/archive](#operation/ProductAPI_ProductArchive). Reset the product stocks to zero before archiving it.

3. Get a product back from the archive: [/v1/product/unarchive](#operation/ProductAPI_ProductUnarchive).

The product will go on sale only when you set its stocks.

## Services management

Upload activation codes for services and digital products: [/v1/product/upload_digital_codes](#operation/ProductAPI_UploadDigitalCode).

# Uploading quality certificates

## Information about certificates

- Get a certificate types list: [/v1/product/certificate/types](#operation/ProductAPI_ProductCertificateTypes).
- Get the list of categories subject to certification: [/v2/product/certification/list](#operation/ProductAPI_ProductCertificationList).
- Get a certificate statuses list: [/v1/product/certificate/status/list](#operation/CertificateStatusList).
- Get a accordance types list: [/v2/product/certificate/accordance-types/list](#operation/CertificateAccordanceTypes).
- Get a list of certificate rejection reasons: [v1/product/certificate/rejection_reasons/list](#operation/RejectionReasonsList).
- Get your certificates: [v1/product/certificate/list](#operation/CertificateList), using filters:
   - certificate type `type`—value `value` from the [/v1/product/certificate/types](#operation/ProductAPI_ProductCertificateTypes) method response;
   - certificate status `status`—value `code` from the [v1/product/certificate/status/list](#operation/CertificateStatusList) method response.

## Working with products associated with a certificate

For binding a certificate to a product:
1. Get a list of brands that require a certificate: [/v1/brand/company-certification/list](#operation/ProductAPI_ProductCertificateBind).
   The response contains brands whose products are added in your seller account.
   The brands list may change if Ozon receives a requirement from the brand to provide a certificate.
2. Add a product certificate: [/v1/product/certificate/create](#operation/ProductAPI_ProductCertificateCreate).
3. Bind a certificate to a product: [/v1/product/certificate/bind](#operation/ProductAPI_ProductCertificateBind).

To get a list of products associated with a certificate, use the [v1/product/certificate/products/list](#operation/CertificateProductsList) method.
If you need to get a list of products with a certain status, in the `status` parameter, pass the `code` value from the [v1/product/certificate/product_status/list](#operation/ProductStatusList) method response.

To unbind a product from a certificate, use the [/v1/product/certificate/products/unbind](#operation/CertificateUnbind) method.

## Certificate status management

To get the attributes for managing a certificate:
1. Get a certificate types list: [/v1/product/certificate/types](#operation/ProductAPI_ProductCertificateTypes).
2. Get a accordance types list: [/v2/product/certificate/accordance-types/list](#operation/CertificateAccordanceTypes).

To create a certificate, use the [/v1/product/certificate/create](#operation/ProductAPI_ProductCertificateCreate) method, by passing in the request:
- certificate type `type_code`—value `value` from the [/v1/product/certificate/types](#operation/ProductAPI_ProductCertificateTypes) method response;
- accordance type `accordance_type_code`—value `code` from the [/v2/product/certificate/accordance-types/list](#operation/CertificateAccordanceTypes) method response.

To delete a certificate, use the [/v1/product/certificate/delete](#operation/CertificateDelete) method.

# Updating prices and product stocks

After uploading the products for the FBS and rFBS schemes you can proceed to updating the stocks. For the FBO scheme, the stocks are updated
automatically once the product is sold.

To update the stocks, use the [/v2/products/stocks](#operation/ProductAPI_ProductsStocksV2) method. 
When using this method, specify the identifier of the warehouse for which you want to change the stocks.

To get information on the stock quantity for FBO, FBS, rFBS, and FBP, use the [/v4/product/info/stocks](#operation/ProductAPI_GetProductInfoStocks) method.

To get information on the stock quantity for FBS and rFBS, use the [v1/product/info/stocks-by-warehouse/fbs](#operation/ProductAPI_ProductStocksByWarehouseFbs) method.

To update prices by products without making changes to the product cards, use the [/v1/product/import/prices](#operation/ProductAPI_ImportProductsPrices) method.

The method allows you to update the price:

- before discounts,
- for customers with an Ozon Premium subscription,
- on the product card, taking discounts into account,
- minimum product price with all special offers applied.

> **Note:** 
The product price can become lower than the minimum price if you apply different types of special offers to the same product.

To get information on product prices, commissions, and discounts, use the [/v5/product/info/prices](#operation/ProductAPI_GetProductInfoPrices) method.

## Reserve products

If legal entities order products, payment may not arrive immediately. Such products are automatically reserved.
To check the stock, use [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3), [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3), or [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.
If `is_legal = true` is in the response, there are reserved products in the stock.
You can update the stock so that the new number of products is greater than the available stock and reserved products in total.
The system will deduct the old stock and calculate the new one.

To check if a product is reserved, use the [/v1/product/info/stocks-by-warehouse/fbs](#operation/ProductAPI_ProductStocksByWarehouseFbs) or [/v4/product/info/stocks](#operation/ProductAPI_GetProductInfoStocks) methods.

[Learn more about stock management](https://docs.ozon.ru/global/en/fulfillment/stock/)

# Participate in Ozon special offers

To promote products, participate in special offers that Ozon runs for customers:

- Get a list of available special offers: [/v1/actions](#operation/Promos).

- Get a list of products that can participate in a special offer: [/v1/actions/candidates](#operation/PromosCandidates).

- Add products to a special offer: [/v1/actions/products/activate](#operation/PromosProductsActivate).

- Get a list of eligible products: [/v1/actions/products](#operation/PromosProducts).

- Remove products from a special offer: [/v1/actions/products/deactivate](#operation/PromosProductsDeactivate).

Customers can ask you for a discount on a product.
To get a list of products that customers want to buy at a discount, use the method
[/v1/actions/discounts-task/list](#operation/promos_task_list).
Requests in statuses `NEW` or `SEEN` you can:
- approve—use the [/v1/actions/discounts-task/approve](#operation/promos_task_approve) method,
- decline—use the [/v1/actions/discounts-task/decline](#operation/promos_task_decline) method.

[Learn more about Ozon special offers in the Seller knowedge base](https://global.ozon.ru/en/docs/promotion/promo.html).

# Working with seller special offers

- Create a special offer:
  - [/v1/seller-actions/create/discount](#operation/SellerActionsCreateDiscount): with the "Discount" mechanics;
  - [/v1/seller-actions/create/discount-with-condition](#operation/SellerActionsCreateDiscountWithCondition): with the "Discount of order amount" mechanics;
  - [/v1/seller-actions/create/installment](#operation/SellerActionsCreateInstallment): with the "Interest-free installment" mechanics;
  - [/v1/seller-actions/create/multi-level-discount](#operation/SellerActionsCreateMultiLevelDiscount): with the "Multi-level discount from the amount" mechanics;
  - [/v1/seller-actions/create/ozon-card-discount](#operation/SellerActionsCreateOzonCardDiscount): with the "Increased discount with Ozon Bank card" mechanics;
  - [/v1/seller-actions/create/voucher](#operation/SellerActionsCreateVoucher): with the "Discount by promo code" mechanics.
- Edit the special offer:
  - [/v1/seller-actions/update/discount](#operation/SellerActionsUpdateDiscount): with the "Discount" mechanics;
  - [/v1/seller-actions/update/discount-with-condition](#operation/SellerActionsUpdateDiscountWithCondition): with the "Discount of order amount" mechanics;
  - [/v1/seller-actions/update/installment](#operation/SellerActionsUpdateInstallment): with the "Interest-free installment" mechanics;
  - [/v1/seller-actions/update/multi-level-discount](#operation/SellerActionsUpdateMultiLevelDiscount): with the "Multi-level discount from the amount" mechanics;
  - [/v1/seller-actions/update/ozon-card-discount](#operation/SellerActionsUpdateOzonCardDiscount): with the "Increased discount with Ozon Bank card" mechanics;
  - [/v1/seller-actions/update/voucher](#operation/SellerActionsUpdateVoucher): with the "Discount by promo code" mechanics.
- [/v1/seller-actions/list](#operation/SellerActionsList): get a list of created special offers.
- [/v1/seller-actions/products/candidates](#operation/SellerActionsProductsCandidates): get a list of products that can participate in the special offer.
- [/v1/seller-actions/products/add](#operation/SellerActionsProductsAdd): add products to the special offer.
- [/v1/seller-actions/products/list](#operation/SellerActionsProductsList): get a list of products that participate in the special offer.
- [/v1/seller-actions/products/delete](#operation/SellerActionsProductsDelete): remove products from the special offer.
- [/v1/seller-actions/voucher/get](#operation/SellerActionsVoucherGet): get a list of promo codes for the special offer with the "Discount by promo code" mechanics.
- [/v1/seller-actions/change-activity](#operation/SellerActionsChangeActivity): enable or disable the special offer.
- [/v1/seller-actions/archive](#operation/SellerActionsArchive): archive the special offer.

[Learn more about seller special offers in the Seller knowledge base](https://docs.ozon.ru/global/en/promotion/promotions/seller-promotions/)

# Setting pricing strategies

Pricing strategies is a tool for automatic change of product price according to the prices of similar products in other online stores and marketplaces.

To set pricing strategies:

1. Get a list of competitors: [/v1/pricing-strategy/competitors/list](#operation/pricing_competitors).

2. Get a list of pricing strategies: [/v1/pricing-strategy/list](#operation/pricing_list).

3. Create your own strategy: [/v1/pricing-strategy/create](#operation/pricing_create) and set coefficients to increase or decrease product price compared to other online stores and marketplaces.
   To get information about a strategy, use the [/v1/pricing-strategy/info](#operation/pricing_info) method.

4. Bind products to a strategy: [/v1/pricing-strategy/products/add](#operation/pricing_items-add).

   You can bind products:
   - If you set a minimum price for them via [/v1/product/import/prices](#operation/ProductAPI_ImportProductsPrices) method.
     To check the price, use the [/v5/product/info/prices](#operation/ProductAPI_GetProductInfoPrices) method.
   - If they aren't bounded to another strategy.
     To check product binding, use the [/v1/pricing-strategy/strategy-ids-by-product-ids](#operation/pricing_ids) method.

   Use the [/v1/pricing-strategy/products/list](#operation/pricing_items-list) method to get a list of products that are bounded to the strategy, and the [/v1/pricing-strategy/products/delete](#operation/pricing_items-delete) method to remove products from the strategy.

7. Enable or disable the strategy: [/v1/pricing-strategy/status](#operation/pricing_status).

To change a list of selected competitors and a strategy name, use the [/v1/pricing-strategy/update](#operation/pricing_update) method.

To delete a strategy, use the [/v1/pricing-strategy/delete](#operation/pricing_delete) method.

# Getting warehouses details

- Get a rFBS warehouse methods list: [/v2/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodListV2).

- Get a warehouse list: [/v2/warehouse/list](#operation/WarehouseListV2).

# Working with FBS warehouses

## Create new warehouse

1. Get a list of drop-off points and select one to create a warehouse: [/v1/warehouse/fbs/create/drop-off/list](#operation/WarehouseAPI_ListDropOffPointsForCreateFBSWarehouse).
2. Send a request to create a new warehouse: [/v1/warehouse/fbs/create](#operation/WarehouseAPI_CreateWarehouseFBS).
3. Get the operation status: [/v1/warehouse/operation/status](#operation/WarehouseAPI_GetWarehouseFBSOperationStatus).
4. Get details about the warehouse by the `warehouse_id` parameter: [/v2/warehouse/list](#operation/WarehouseListV2).

## Update warehouse settings

1. Send a request to update the warehouse settings: [/v1/warehouse/fbs/update](#operation/WarehouseAPI_UpdateWarehouseFBS).
2. Get the operation status: [/v1/warehouse/operation/status](#operation/WarehouseAPI_GetWarehouseFBSOperationStatus).

## Update warehouse first mile

1. Get a list of drop-off points and select one to update details about the warehouse: [/v1/warehouse/fbs/update/drop-off/list](#operation/WarehouseAPI_ListDropOffPointsForUpdateFBSWarehouse).
2. Send a request to update the first mile: [/v1/warehouse/fbs/first-mile/update](#operation/WarehouseAPI_UpdateWarehouseFBSFirstMile).
3. Get the first mile update status: [/v1/warehouse/operation/status](#operation/WarehouseAPI_GetWarehouseFBSOperationStatus).

## Archive warehouse

1. Move the warehouse to archive: [/v1/warehouse/archive](#operation/WarehouseAPI_ArchiveWarehouseFBS).
2. Get the warehouse archiving status: [/v1/warehouse/operation/status](#operation/WarehouseAPI_GetWarehouseFBSOperationStatus).

## Remove warehouse from archive

1. Remove the warehouse from the archive: [/v1/warehouse/unarchive](#operation/WarehouseAPI_UnarchiveWarehouseFBS).
2. Get the warehouse unarchiving status: [/v1/warehouse/operation/status](#operation/WarehouseAPI_GetWarehouseFBSOperationStatus).

# Managing FBO, FBS, rFBS, and FBP orders

Manage orders depending on your work scheme:
- [FBO scheme](#section/Managing-FBO-FBS-and-rFBS-orders/FBO-scheme);
- [FBS Standard scheme](#section/Managing-FBO-FBS-and-rFBS-orders/FBS-Standard-scheme);
- [FBS PickUp scheme with trusted acceptance enabled](#section/Managing-FBO-FBS-and-rFBS-orders/FBS-PickUp-scheme-with-trusted-acceptance-enabled);
- [Ozon Logistics](#section/Managing-FBO-FBS-and-rFBS-orders/Ozon-Logistics);
- [rFBS Standard scheme](#section/Managing-FBO-FBS-and-rFBS-orders/rFBS-Standard-scheme);
- [rFBS Express scheme with delivery to a pick-up point](#section/Managing-FBO-FBS-and-rFBS-orders/rFBS-Express-scheme-with-delivery-to-a-pick-up-point);
- [rFBS scheme with delivery by the integrated service](#section/Managing-FBO-FBS-and-rFBS-orders/rFBS-scheme-with-delivery-by-integrated-service).

If you sell products from abroad, manage orders according to one of the schemes:
- [rFBS Crossborder](#section/Managing-FBO-FBS-and-rFBS-orders/rFBS-Crossborder-scheme);
- [rFBS Crossborder with delivery by the integrated service](#section/Managing-FBO-FBS-and-rFBS-orders/rFBS-Crossborder-scheme-with-delivery-by-integrated-service);
- [rFBS Aggregator](#section/Managing-FBO-FBS-and-rFBS-orders/rFBS-Aggregator-scheme).

[Learn more how to manage rFBS orders with products sold by weight](#section/Managing-FBO-FBS-and-rFBS-orders/How-to-work-with-products-sold-by-weight-(rFBS))

## FBO scheme

Check Ozon warehouses workload before creating a supply request: [/v1/supplier/available_warehouses](#operation/SupplierAPI_SupplierAvailableWarehouses).

To get a list of shipments, financial and analytical information, use the [/v2/posting/fbo/list](#operation/PostingAPI_GetFboPostingList) method.
This method also returns information about the sold activation codes linked to the shipment number.

To get information about a shipment, use the [/v2/posting/fbo/get](#operation/PostingAPI_GetFboPosting) method.

### Create a supply request

1. Get information about clusters and their warehouses: [/v1/cluster/list](#operation/SupplyDraftAPI_DraftClusterList).
2. Get identifiers of warehouses, pick-up points and sorting centers for cross-docking shipping: [/v1/warehouse/fbo/list](#operation/SupplyDraftAPI_DraftGetWarehouseFboList).
3. Create a supply request draft. To do this, pass the product list, supply type and, if delivered by cross-docking, the shipping point: [/v1/draft/create](#operation/SupplyDraftAPI_DraftCreate).
4. Get information about creating a supply request from a draft: [/v1/draft/create/info](#operation/SupplyDraftAPI_DraftCreateInfo).
5. Get available supply time slots for final shipping warehouses: [/v1/draft/timeslot/info](#operation/SupplyDraftAPI_DraftTimeslotInfo). The maximum period is 28 days from the current date.
6. Create the supply request from the draft: [/v1/draft/supply/create](#operation/SupplyDraftAPI_DraftSupplyCreate).
7. Get the status of the supply request creating process: [/v1/supply/create/status](#operation/SupplyDraftAPI_DraftSupplyCreateStatus). When the request is created, the method returns the supply requests identifiers.

### Get supply request information

To supply products to Ozon warehouses, you need a supply request. It specifies what products and in what quantities you will supply.

1. Get a list of supply requests: [/v3/supply-order/list](#operation/SupplyOrderList).
2. Get the status of a supply request and the number of shipments in that status: [/v1/supply-order/status/counter](#operation/SupplyOrderAPI_SupplyOrderStatusCounter).
3. Get information on a supply request: [/v3/supply-order/get](#operation/SupplyOrderGet).
4. Get a list of products in the supply or request: [/v1/supply-order/bundle](#operation/SupplyOrderBundle).

To get to a warehouse, select a supply time slot and issue a driver and vehicle pass. To do this:
1. Get a list of available supply time slots: [/v1/supply-order/timeslot/get](#operation/SupplyOrderAPI_GetSupplyOrderTimeslots).
2. Change the selected time slot, if necessary: [/v1/supply-order/timeslot/update](#operation/SupplyOrderAPI_UpdateSupplyOrderTimeslot).
3. Get the status of the assigned time slot: [/v1/supply-order/timeslot/status](#operation/SupplyOrderAPI_GetSupplyOrderTimeslotStatus).
4. Add driver and vehicle details: [/v1/supply-order/pass/create](#operation/SupplyOrderAPI_SupplyOrderPassCreate).
5. Get the status of driver and vehicle data entry: [/v1/supply-order/pass/status](#operation/SupplyOrderAPI_SupplyOrderPassStatus).

## FBS Standard scheme

1. Before you start working with shipments, get a list
   of unprocessed shipments: [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

    If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
    Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
    Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
    
    The `requirements` block contains the following information:
    - which products are subject to mandatory labeling;
    - whether the cargo customs declaration and product batch registration number should be passed: this information
   can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system: [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Get the `exemplar_id` identifiers using the [/v6/fbs/posting/product/exemplar/create-or-get](#operation/PostingAPI_FbsPostingProductExemplarCreateOrGetV6) method.

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations and product batch registration numbers or indicate that there is no such data.

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

4. Before packaging, make sure that the shipment meets the drop-off point restrictions. Get them by the shipment number using the [/v1/posting/fbs/restrictions](#operation/PostingAPI_GetRestrictions) method.

5. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

6. Confirm shipping and generate the waybill using [/v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) or create shipping with [/v1/carriage/create](#operation/CarriageAPI_CarriageCreate), and confirm it with [/v1/carriage/approve](#operation/CarriageAPI_CarriageApprove). The methods output is a document generation task identifier.

7. Get the list of shipments for which acceptance, transfer certificate, and waybill are required: [/v1/posting/carriage-available/list](#operation/PostingAPI_GetCarriageAvailableList) or [/v2/carriage/delivery/list](#operation/CarriageDeliveryListV2).

8. Get information about the created shipping using the [/v1/carriage/get](#operation/CarriageGet) method.
   The `available_actions` array contains information about available actions with the supply and the necessity to create a pass for access to the Ozon warehouse.

   To create a pass, use the [/v1/carriage/pass/create](#operation/carriagePassCreate) method.
   To pick up returns on the same car after the products delivery, pass the `with_returns = true` value.
   You need a new pass for each supply.

   To update or delete the pass, use the [/v1/carriage/pass/update](#operation/carriagePassUpdate) and [/v1/carriage/pass/delete](#operation/carriagePassDelete) methods.

   To get the list of all passes, use the [/v1/pass/list](#operation/PassList) method.

   [Learn more about issuing passes](https://seller-edu.ozon.ru/fbs/ozon-logistika/oformlenie-propuska)

9. Check that the documents are created: [/v2/posting/fbs/act/check-status](#operation/PostingAPI_PostingFBSActCheckStatus).

10. Once the document status changes to `ready`, get the acceptance and transfer certificate and waybill files: [/v2/posting/fbs/act/get-pdf](#operation/PostingAPI_PostingFBSGetAct).

11. Print out a sticker for each shipment to identify it in the Ozon system: [/v2/posting/fbs/package-label](#operation/PostingAPI_PostingFBSPackageLabel).

12. Get PDF documents: [/v2/posting/fbs/act/get-pdf](#operation/PostingAPI_PostingFBSGetAct).

After that, you can take the shipments and documents to the pick-up point.

If the shipment has been handed over for delivery but has not been scanned at the sorting center, you can open a dispute: [/v2/posting/fbs/arbitration](#operation/PostingAPI_MoveFbsPostingToArbitration).
An open dispute will transfer the shipment to the `arbitration` status.

If customer opens a dispute, the shipment status will change to `client_arbitration`.

To track the status change once the shipment is found, use the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method with the necessary filters applied.

To handle dispute orders over to shipping, use the [/v2/posting/fbs/awaiting-delivery](#operation/PostingAPI_MoveFbsPostingToAwaitingDelivery) method.
The shipment status will change to `awaiting_deliver`.

### Working with economy products

1. Create economy products in your [personal account](https://seller.ozon.ru/app/econom/products).
2. Get identifiers of MOQs with created products: [/v1/product/quant/list](#operation/QuantProductList).

[Learn more about economy products in the Seller Knowledge Base](https://seller-edu.ozon.ru/tarif-ekonom)

### How to cancel a shipment

1. Use the [/v2/posting/fbs/cancel-reason/list](#operation/PostingAPI_GetPostingFbsCancelReasonList) method at any stage
   of shipment processing to get a list of shipment cancellation reasons.

2. Pass this list and the shipment number: [/v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting).

To cancel selected products from a shipment, use the [/v2/posting/fbs/product/cancel](#operation/PostingAPI_CancelFbsPostingProduct) method.

If customer cancels shipments, their status will change to `canceled`.

## FBS scheme with electronic waybills

### Working with shipments

1. Get the list of unprocessed shipments: [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) or [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList). 
   In the response, check:
   - `is_blr_traceable`: product traceability attribute;
   - `require_blr_traceable_attrs = true`: if you have to fill out traceability attributes;
   - `split_before_ship = true`: if you have to split the order.
   
2. Filter traceable shipments using the `is_blr_traceable = true` filter: [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3).

3. If the order is traceable, split it: [/v1/posting/fbs/traceable/split](#operation/PostingFbsTraceableSplit).

4. Check if you need to fill out traceability attributes: [/v1/posting/fbs/product/traceable/attribute](#operation/PostingFbsProductTraceableAttribute).

5. If attributes are required, specify them in the PDP in your account or via API:
   - product quantity in measurement units;
   - GTIN barcode.

6. Pass product items and pack the shipment: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4).

### Working with shippings

1. Get a list of zero shippings: [/v2/carriage/delivery/list](#operation/CarriageDeliveryListV2). Traceable shippings will have the `all_blr_traceable` parameter.

2. Create shipping with traceable products: [/v1/carriage/create](#operation/CarriageAPI_CarriageCreate). Specify the `is_blr_traceable = true` in the request.
   
   If a shipping point doesn't accept electronic waybills, you get the `BLR_TRACEABLE_PICKUP_NOT_ALLOWED` error.

3. Get information about the created shipping: [/v1/carriage/get](#operation/CarriageGet). In the response, get the `all_blr_traceable` parameter.

4. Create and upload an electronic waybill for verification via electronic document circulation (EDC).

5. Get the electronic waybill status: [/v1/carriage/ettn/status](#operation/CarriageEttnStatus).

6. Confirm shipping: [/v1/carriage/approve](#operation/CarriageAPI_CarriageApprove). If electronic waybill verification fails, you get the `INCORRECT_E_WAYBILL_STATUS` error.

7. Update product composition in shipping if needed: [/v1/carriage/set-postings](#operation/CarriageAPI_SetPostings). If electronic waybill verification fails, you get the `INCORRECT_E_WAYBILL_STATUS` error.

## FBS PickUp scheme with trusted acceptance enabled

1. Before you start working with shipments, get a list
   of unprocessed shipments: [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).

   The `requirements` block contains the following information:
    - which products are subject to mandatory labeling;
    - whether the cargo customs declaration and product batch registration number should be passed: this information
      can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
      [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system: [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations or indicate that there is no such data.

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

4. Before packaging, make sure that the shipment meets the drop-off point restrictions. Get them by the shipment number using the [/v1/posting/fbs/restrictions](#operation/PostingAPI_GetRestrictions) method.

5. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

6. Confirm shipping and generate the waybill using [/v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) or create shipping with [/v1/carriage/create](#operation/CarriageAPI_CarriageCreate), and confirm it with [/v1/carriage/approve](#operation/CarriageAPI_CarriageApprove). The methods output is a document generation task identifier.

7. Get the list of shipments for which acceptance, transfer certificate, and waybill are required: [/v1/posting/carriage-available/list](#operation/PostingAPI_GetCarriageAvailableList) or [/v2/carriage/delivery/list](#operation/CarriageDeliveryListV2).

8. Get PDF documents: [/v2/posting/fbs/act/get-pdf](#operation/PostingAPI_PostingFBSGetAct).

9. When the document status has changed to `ready`, get the acceptance and transfer certificate and waybill files: [/v2/posting/fbs/act/get-pdf](#operation/PostingAPI_PostingFBSGetAct).

10. Print out a sticker for each shipment to identify it in the Ozon system: [/v2/posting/fbs/package-label](#operation/PostingAPI_PostingFBSPackageLabel).

11. Once you've packed all your shipments according to the [Trusted package unit acceptance requirements](https://seller-edu.ozon.ru/docs/fbs/ozon-logistika/doveritel-naya-priemka-gruzovogo-mesta.html#какие-требования-к-грузовому-месту), get labels for each package unit: [/v2/posting/fbs/act/get-container-labels](#operation/PostingAPI_PostingFBSActGetContainerLabels).

After that, you can transfer the package unit to the pick-up point.

If the shipment has been handed over for delivery but has not been scanned at the sorting center, you can open a dispute: [/v2/posting/fbs/arbitration](#operation/PostingAPI_MoveFbsPostingToArbitration).
An open dispute will transfer the shipment to the `arbitration` status.

If customer opens a dispute, the shipment status will change to `client_arbitration`.

To track the status change once the shipment is found, use the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method with the necessary filters applied.

To handle dispute orders over to shipping, use the [/v2/posting/fbs/awaiting-delivery](#operation/PostingAPI_MoveFbsPostingToAwaitingDelivery) method.
The shipment status will change to `awaiting_deliver`.

## Ozon Logistics

### Delivery

1. Check if the customer can receive orders via Ozon: [v1/delivery/check](#operation/DeliveryCheck). Delivery methods for Ozon Logistics:
   - Pick-up if the customer receives products at a pick-up point.
   - Courier delivery if Ozon delivers the products to the specified address.

   If the delivery method is pick-up, pass the pick-up point identifier to the `pickup` parameter.
   For courier delivery, pass the receiving address and coordinates to the `courier` parameter.

2. Mark the points on the map from which customers can pick up orders: [v1/delivery/map](#operation/DeliveryMap).
3. Get a list of pick-up points: [v1/delivery/point/list](#operation/DeliveryAPI_DeliveryPointList).
4. Get pick-up point details: [/v1/delivery/point/info](#operation/DeliveryPointInfo).
5. Check product availability and delivery time: [/v2/delivery/checkout](#operation/DeliveryCheckout).

Allow customers to choose the delivery method and delivery point at the checkout or in the cart.
If it's impossible, cache the data on your side to reduce the load and optimize the number of API requests.

### Order processing

Create an order in the Ozon system only after the customer pays for it.

1. Create an order and start packing it: [v2/order/create](#operation/OrderAPI_OrderCreate). You can't change the order after calling the method.
2. Track all shipments or a specific one: [v2/posting/fbo/list](#operation/PostingAPI_GetFboPostingList) and [v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3).
3. Get the item labels from the shipment: [v1/posting/marks](#operation/PostingAPI_PostingMarks). Pass only those shipments that you created using Ozon Logistics methods.

### Shipment tracking

For the FBO scheme:
- Get a list of all shipments: [v2/posting/fbo/list](#operation/PostingAPI_GetFboPostingList).
- Get details about a specific shipment: [v2/posting/fbo/get](#operation/PostingAPI_GetFboPosting).

For the FBS scheme:
- Get a list of unprocessed shipments: [v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).
- Get shipment list: [v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3).
- Get shipment details: [v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).

If the customer rejects one of the products upon receipt of the shipment, it remains in the "Delivered" status. Use [v1/returns/list](#operation/returnsList) to get details on a return with an unpurchased product. The type of this return is `type: PartialReturn`.

### Order or shipment cancellation

You can cancel orders that were created using the Ozon Logistics methods if:
- Ozon canceled the order before it arrived at the pick-up point or before delivery to the customer.
- The customer refused the whole order or its part upon receipt.
- The customer requested order cancellation through the seller's website or app. The customer can't cancel the order via Ozon account.

> **Note:** 
Refund the money to the customer only when the cancellation is confirmed.

To cancel an order or its part:
- Get possible reasons for order cancellation: [/v1/cancel-reason/list-by-order](#operation/CancelReasonListByOrder).
- Get a list of available reasons for shipment cancellation: [/v1/cancel-reason/list-by-posting](#operation/CancelReasonAPI_CancelReasonListByPosting). Use this method only if the customer requested order cancellation.
- Check if the order can be canceled: [v1/order/cancel/check](#operation/OrderAPI_OrderCancelCheck).
- Cancel the order: [v1/order/cancel](#operation/OrderAPI_OrderCancel).
- Cancel the shipment from the order: [v1/posting/cancel](#operation/PostingAPI_PostingCancel).
- Check order cancellation status: [v1/order/cancel/status](#operation/OrderAPI_OrderCancelStatus).
- Check shipment cancellation status: [v1/posting/cancel/status](#operation/PostingAPI_PostingCancelStatus).

To cancel FBS shipments:
- Cancel shipping of some products in the shipment: [v2/posting/fbs/product/cancel](#operation/PostingAPI_CancelFbsPostingProduct).
- Cancel the shipment: [v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting).

### Product return

Track the product return status: [v1/returns/list](#operation/returnsList).
Use the method if the customer canceled the order during shipping or refused it upon receipt. Seller manages returns from customers on their own.
If the customer picked up the product and then decided to return it, they should contact the seller.

If a customer refuses one of the products upon shipment receipt, it remains in the "Delivered" status.
The system creates a new shipment with the "Canceled" status and the `PartialRefund` cancellation type.
To get information about it, use the [v1/returns/list](#operation/returnsList) method.

### Stock

- Get a report on the stock in Ozon warehouses to plan the product supply: [v1/analytics/stocks](#operation/AnalyticsAPI_AnalyticsStocks).
- Show customers the quantity of products in Ozon warehouses: [v4/product/info/stocks](#operation/ProductAPI_GetProductInfoStocks).

If there aren't enough products in Ozon warehouses, you can't create an order using the Ozon Logistics methods.

## rFBS Standard scheme

> **Note:** 
Starting March 11, 2024, the maximum delivery time to the customer is 30 days.
Update the delivery period in your method settings. 
If the period exceeds the limit, it'll be updated automatically. 

1. Before you start working with shipments, get a list of unprocessed orders (shipments):
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).
   
   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

   If `set_cutoff` is specified in the `available_actions` parameter, specify the shipping date of the shipment using the [/v1/posting/cutoff/set](#operation/PostingAPI_SetPostingCutoff) method.
   Do this no later than the date specified in the `shipment_date` parameter in the methods:
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList), [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) or [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   After the `shipment_date` passes, you can't specify the shipping date and pack the shipment.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   For example, a delivery address.

   The `requirements` block contains the following information:
    - which products are subject to mandatory labeling;
    - whether the cargo customs declaration and product batch registration number should be passed: this information
      can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
      [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   If you sell products by weight and the `result.products.is_weight_needed` parameter for a product is `true`,
   pass its weight information via the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system: [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations or indicate that there is no such data.

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

4. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

After you packaged the order, contact the customer to agree on a delivery date.
If the customer asks to reschedule the delivery, you can do it: [/v1/posting/fbs/timeslot/set](#operation/PostingAPI_SetPostingTimeslot).
Get information about the dates and the number of times available for delivery reschedule: [/v1/posting/fbs/timeslot/change-restrictions](#operation/PostingAPI_PostingTimeslotChangeRestrictions).

Hand the shipment over to the courier:

1. Once the courier has picked up the shipment, change the shipment status to `delivering`: [/v2/fbs/posting/delivering](#operation/PostingAPI_FbsPostingDelivering).

    
> **Note:** 
    If you don't make a route schedule, and the courier heads straight to the customer, you can change the shipment 
    status from delivering to last mile straight away: [/v2/fbs/posting/last-mile](#operation/PostingAPI_FbsPostingLastMile).
    

   Along with this pass the shipment track number: [/v2/fbs/posting/tracking-number/set](#operation/PostingAPI_FbsPostingTrackingNumberSet).

2. When courier is on the way to the customer, change the shipment status to `last mile`: [/v2/fbs/posting/last-mile](#operation/PostingAPI_FbsPostingLastMile).

3. Once courier has handed the shipment over to the customer, change the status to `delivered`: [/v2/fbs/posting/delivered](#operation/PostingAPI_FbsPostingDelivered).

### How to cancel a shipment

1. Use the [/v2/posting/fbs/cancel-reason/list](#operation/PostingAPI_GetPostingFbsCancelReasonList) method at any stage
   of shipment processing to get a list of shipment cancellation reasons.

2. Pass this list and the shipment number: [/v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting).

To cancel selected products in a shipment, use the [/v2/posting/fbs/product/cancel](#operation/PostingAPI_CancelFbsPostingProduct) method.

If customer cancels shipments, their status will change to `cancelled`.

## rFBS Express scheme with delivery to a pick-up point

1. Before you start working with shipments, get a list of unprocessed orders (shipments):
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).

   The `requirements` block contains the following information:
   - which products are subject to mandatory labeling;
   - whether the cargo customs declaration and product batch registration number should be passed: this information
     can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
     [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system: [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. 
   If necessary, pass the numbers of customs declarations or indicate that there is no such data.

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: 
   - to get the product item statuses use [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).
   - to get data about created items use [/v6/fbs/posting/product/exemplar/create-or-get](#operation/PostingAPI_FbsPostingProductExemplarCreateOrGetV6).

4. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). 
   You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. 
   For example, if there are several products in an order, and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

5. Print out a sticker for the shipment to identify it in the Ozon system: [/v2/posting/fbs/package-label](#operation/PostingAPI_PostingFBSPackageLabel).

6. Hand the shipment over to the courier.

Shipment statuses will change automatically:
1. `on_way_to_city`—the courier picked up the shipment.
2. `on_way_to_pickup_point`—the courier takes the order to the pick-up point.
3. `in_pickup_point`—the shipment is at the pick-up point.
4. `delivered`—the customer picked up the shipment from the pick-up point.

To get a shipment status, use the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.

### How to cancel a shipment

1. Use the [/v2/posting/fbs/cancel-reason/list](#operation/PostingAPI_GetPostingFbsCancelReasonList) method at any stage
   of shipment processing to get a list of shipment cancellation reasons.

2. Pass this list and the shipment number: [/v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting).

To cancel selected products in a shipment, use the [/v2/posting/fbs/product/cancel](#operation/PostingAPI_CancelFbsPostingProduct) method.

If customer cancels shipments, their status will change to `cancelled`.

## rFBS scheme with delivery by integrated service

1. Before you start working with shipments, get a list of unprocessed orders (shipments):
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   For example, a delivery address.

   The `requirements` block contains the following information:
    - which products are subject to mandatory labeling;
    - whether the cargo customs declaration and product batch registration number should be passed: this information
      can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
      [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system:  [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations or indicate that there is no such data.

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

4. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

Hand the shipment over to delivery service. All statuses from "Delivering" to "Delivered" will be passed by the delivery service.

### How to cancel a shipment

1. Use the [/v2/posting/fbs/cancel-reason/list](#operation/PostingAPI_GetPostingFbsCancelReasonList) method at any stage
   of shipment processing to get a list of shipment cancellation reasons.

2. Pass this list and the shipment number: [/v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting).

To cancel selected products in a shipment, use the [/v2/posting/fbs/product/cancel](#operation/PostingAPI_CancelFbsPostingProduct) method.

If customer cancels shipments, their status will change to `canceled`.

## rFBS Aggregator scheme

1. Before you start working with shipments, get a list of unprocessed orders (shipments):
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   For example, a delivery address.

   The `requirements` block contains the following information:
   - which products are subject to mandatory labeling;
   - whether the cargo customs declaration and product batch registration number should be passed: this information
     can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
     [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system:  [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations or indicate that there is no such data.

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

4. If the product in the shipment is packed in several boxes, pass the number of them: [/v3/posting/multiboxqty/set](#operation/PostingAPI_PostingMultiBoxQtySetV3).

5. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_registration`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

6. When the carrier processes the shipment, its status will change from `awaiting_registration` to `awaiting_delivery`. A tracking number will then be assigned for the shipment.

   Print out a sticker for each shipment to identify it in the Ozon system: [/v2/posting/fbs/package-label](#operation/PostingAPI_PostingFBSPackageLabel).

Hand the shipment over to delivery service. All statuses from "Delivering" to "Delivered" will be passed by the delivery service.

If you sell from Turkey and need the Elektronik Ticaret Gümrük Beyannamesi (ETGB) declarations for tax refund,
get declaration forms: [/v1/posting/global/etgb](#operation/PostingAPI_GetEtgb).

### How to cancel a shipment

1. Use the [/v2/posting/fbs/cancel-reason/list](#operation/PostingAPI_GetPostingFbsCancelReasonList) method at any stage
   of shipment processing to get a list of shipment cancellation reasons.

2. Pass this list and the shipment number: [/v2/posting/fbs/cancel](#operation/PostingAPI_CancelFbsPosting).

To cancel selected products in a shipment, use the [/v2/posting/fbs/product/cancel](#operation/PostingAPI_CancelFbsPostingProduct) method.

If customer cancels shipments, their status will change to `cancelled`.

## rFBS Crossborder scheme

1. Before you start working with shipments, get a list of unprocessed orders (shipments):
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

   If `set_cutoff` is specified in the `available_actions` parameter, specify the shipping date of the shipment using the [/v1/posting/cutoff/set](#operation/PostingAPI_SetPostingCutoff) method.
   Do this no later than the date specified in the `shipment_date` parameter in the methods:
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList), [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) or [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   After the `shipment_date` passes, you can't specify the shipping date and pack the shipment.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   For example, a delivery address.

   The `requirements` block contains the following information:
    - which products are subject to mandatory labeling;
    - whether the cargo customs declaration and product batch registration number should be passed: this information
      can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
      [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system: [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations or indicate that there is no such data.
   
   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

4. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

5. After the shipment has been transferred to the delivery service, change the status of the shipment to `delivering`: [/v2/fbs/posting/delivering](#operation/PostingAPI_FbsPostingDelivering).

6. At the same time, if the shipment has a tracking number, pass it to the system: [/v2/fbs/posting/tracking-number/set](#operation/PostingAPI_FbsPostingTrackingNumberSet). 

7. When courier is on the way to the customer, change the shipment status to `last mile`: [/v2/fbs/posting/last-mile](#operation/PostingAPI_FbsPostingLastMile).

8. Once courier has handed the shipment over to the customer, change the status to `delivered`: [/v2/fbs/posting/delivered](#operation/PostingAPI_FbsPostingDelivered).

## rFBS Crossborder scheme with delivery by integrated service

1. Before you start working with shipments, get a list of unprocessed orders (shipments):
   [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList).

   If the customer is a legal entity, the `requirements` block contains information about the necessity to pass manufacturer country for all items in the order for which it is not specified.
   Get a list of available countries: [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2).
   Pass the information about the manufacturer country: [/v2/posting/fbs/product/country/set](#operation/PostingAPI_SetCountryProductFbsPostingV2).

   You can also get a list of orders (shipments) using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) method.
   It allows you to get all orders using filters with different statuses. You can also get analytics data
   if you send the `with` field  with the `analytics_data` value.

   Shipments may come in `awaiting_packaging`, `awaiting_approve`, or `awaiting_verification` statuses.

2. Get more information on orders if necessary: [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3).
   For example, a delivery address.

   The `requirements` block contains the following information:
    - which products are subject to mandatory labeling;
    - whether the cargo customs declaration and product batch registration number should be passed: this information
      can also be obtained using the [/v3/posting/fbs/list](#operation/PostingAPI_GetFbsPostingListV3) and
      [/v3/posting/fbs/unfulfilled/list](#operation/PostingAPI_GetFbsPostingUnfulfilledList) methods.

   You also can get extra information by barcode: [/v2/posting/fbs/get-by-barcode](#operation/PostingAPI_GetFbsPostingByBarcode).

3. Check that the labeling codes meet the length and character requirements of the “Chestny ZNAK” system:  [/v5/fbs/posting/product/exemplar/validate](#operation/PostingAPI_FbsPostingProductExemplarValidateV5).

   Using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method, add a label for each exemplar that you'll submit to the
   “Chestny ZNAK” system. If necessary, pass the numbers of customs declarations or indicate that there is no such data.

   [Learn more about “Chestny ZNAK” label in the Seller Knowledge Base](https://seller-edu.ozon.ru/work-with-goods/trebovaniya-k-kartochkam-tovarov/product-information/obyazatelnaya-markirovka-tovarov)

   Get labeling transfer statuses: [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5).

4. Once you've packaged your order, confirm it before the packaging time ends: [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4). You can't package the order if:
   - its status differs from `awaiting_packaging`;
   - you haven't specified labeling codes for products that are subject to a mandatory labeling.

   If necessary, use this method to divide the order into several shipments. For example, if there are several products in an order,
   and they need to be packed in different boxes, since in combination they do not meet the packaging requirements.

   After applying this method, the shipment status will change to `awaiting_deliver`.

   You can use the partial order package method: [/v4/posting/fbs/ship/package](#operation/PostingAPI_ShipFbsPostingPackage).

5. Print out a sticker for each shipment to identify it in the Ozon system: [/v2/posting/fbs/package-label](#operation/PostingAPI_PostingFBSPackageLabel).

Hand the shipment over to delivery service. All statuses from "Delivering" to "Delivered" will be passed by the delivery service.

## How to work with products sold by weight (rFBS)

Before moving an order with products sold by weight to the `awaiting_delivery` status, pass the product weight data.
To do this, after weighing the products prior to packing, pass the weight information for each product item via the
[/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method.

The [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method
returns the following errors:
- `WEIGHT_IS_OUT_OF_RANGE`: if the passed weight is out of the acceptable range;
- `WEIGHT_IS_REQUIRED`: if you haven't provided the product weight.

If the `result.products.is_weight_needed` parameter
in the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method is `true` for a product, weigh it.

After passing the weight for each product item sold by weight,
you can move the order to the `awaiting_delivery` status.

If an order with products sold by weight contains unweighted products,
the [/v4/posting/fbs/ship](#operation/PostingAPI_ShipFbsPostingV4) method  
returns the `WEIGHT_IS_REQUIRED` error.

## FBP Scheme

### Direct Supply

1. Get a list of partner warehouses: [/v1/fbp/warehouse/list](#operation/FbpWarehouseList).
2. Validate products for partner warehouse acceptance: [/v1/fbp/draft/direct/product/validate](#operation/FbpDraftDirectProductValidate).
3. Get time slots for direct supply: [/v1/fbp/draft/direct/timeslot/get](#operation/FbpDraftDirectGetTimeslot).
4. Select the delivery method:

   
   Delivery by seller

   • Create a supply request draft: [/v1/fbp/draft/direct/seller-dlv/create](#operation/FbpDraftDirectSellerDlvCreate).

   • Edit the time slot in the draft: [/v1/fbp/draft/direct/timeslot/edit](#operation/FbpDraftDirectTimeslotEdit).

   • Update seller delivery details in the draft: [/v1/fbp/draft/direct/seller-dlv/edit](#operation/FbpDraftDirectSellerDlvEdit).
   

   
   Delivery by third-party

   • Create a supply request draft: [/v1/fbp/draft/direct/tpl-dlv/create](#operation/FbpAPI_FbpDraftDirectTplDlvCreate).

   • Edit the time slot in the draft: [/v1/fbp/draft/direct/timeslot/edit](#operation/FbpDraftDirectTimeslotEdit).

   • Update third-party delivery details in the draft: [/v1/fbp/draft/direct/tpl-dlv/edit](#operation/FbpAPI_FbpDraftDirectTplDlvEdit).
   

details {
  margin-bottom: 10px;
  border: 1px solid #ddd;
  padding: 10px;
  border-radius: 5px;
}
summary {
  cursor: pointer;
  font-weight: bold;
}

5. Get a list of drafts: [/v1/fbp/draft/list](#operation/FbpAPI_FbpDraftList).
6. Get draft details: [/v1/fbp/draft/get](#operation/FbpAPI_FbpDraftGet). If the draft has errors, delete it: [/v1/fbp/draft/direct/delete](#operation/FbpDraftDirectDelete).
7. Get information about a supply's contents using `bundle_id`: [/v1/supply-order/bundle](#operation/SupplyOrderBundle).
8. Register a draft request, so it becomes active: [/v1/fbp/draft/direct/register](#operation/FbpDraftDirectRegistrate).
9. Get current warehouse time slots: [/v1/fbp/order/direct/timeslot/list](#operation/FbpAPI_FbpAvailableTimeslotList).
10. Edit the supply time slot: [/v1/fbp/order/direct/timeslot/edit](#operation/FbpAPI_FbpEditTimeslot).
11. Update seller or third-party delivery details: [/v1/fbp/draft/direct/seller-dlv/edit](#operation/FbpDraftDirectSellerDlvEdit) or [/v1/fbp/draft/direct/tpl-dlv/edit](#operation/FbpAPI_FbpDraftDirectTplDlvEdit).
12. Get a list of supplies: [/v1/fbp/order/list](#operation/FbpAPI_FbpOrderList).
13. Get details of a specific supply: [/v1/fbp/order/get](#operation/FbpAPI_FbpOrderGet). If the supply request has errors, cancel it: [/v1/fbp/order/direct/cancel](#operation/FbpAPI_FbpOrderDirectCancel).
14. Create a shipping label generation task: [/v1/fbp/label/create](#operation/FbpAPI_FbpCreateLabel).
15. Get labels and their generation status: [/v1/fbp/label/get](#operation/FbpAPI_FbpGetLabel).
16. Create a waybill generation task: [/v1/fbp/act-to/create](#operation/FbpAPI_FbpCreateConsignmentNote).
17. Get the waybill and its generation status: [/v1/fbp/act-to/get](#operation/FbpAPI_FbpCheckConsignmentNoteState).
18. Create an acceptance certificate generation task: [/v1/fbp/act-from/create](#operation/FbpAPI_FbpCreateAct).
19. Get the certificate and its generation status: [/v1/fbp/act-from/get](#operation/FbpAPI_FbpCheckActState).
20. Get a list of completed supplies: [/v1/fbp/archive/list](#operation/FbpAPI_FbpArchiveList).
21. Get details of a completed supply: [/v1/fbp/archive/get](#operation/FbpAPI_FbpArchiveGet).

### Drop-off Supply

1. Get a list of partner warehouses: [/v1/fbp/warehouse/list](#operation/FbpWarehouseList).
2. Validate products for partner warehouse acceptance: [/v1/fbp/draft/drop-off/product/validate](#operation/FbpDraftDropOffProductValidate).
3. Get a list of provinces: [/v1/fbp/draft/drop-off/province/list](#operation/FbpDraftDropOffProvinceList).
4. Get a list of drop-off points in a province: [/v1/fbp/draft/drop-off/point/list](#operation/FbpDraftDropOffPointList).
5. Get the working schedule of a drop-off point: [/v1/fbp/draft/drop-off/point/timetable](#operation/FbpDraftDropOffPointTimetable).
6. Create a draft for drop-off delivery: [/v1/fbp/draft/drop-off/create](#operation/FbpDraftDropOffCreate).
7. Edit drop-off delivery details in the draft: [/v1/fbp/draft/drop-off/dlv/edit](#operation/FbpDraftDropOffDlvEdit).
8. Get a list of drafts: [/v1/fbp/draft/list](#operation/FbpAPI_FbpDraftList).
9. Get details of a specific draft: [/v1/fbp/draft/get](#operation/FbpAPI_FbpDraftGet). If the draft has errors, delete it: [/v1/fbp/draft/drop-off/delete](#operation/FbpDraftDropOffDelete).
10. Get information about a supply's contents using `bundle_id`: [/v1/supply-order/bundle](#operation/SupplyOrderBundle).
11. Register a draft supply request, so it becomes active: [/v1/fbp/draft/drop-off/registrate](#operation/FbpDraftDropOffRegistrate).
12. Get the working schedule of a drop-off point: [/v1/fbp/order/drop-off/timetable](#operation/FbpAPI_FbpOrderDropOffTimetable).
13. Edit and pass drop-off delivery details: [/v1/fbp/order/drop-off/dlv/edit](#operation/FbpAPI_FbpOrderDropOffDlvEdit).
14. Get a list of supplies: [/v1/fbp/order/list](#operation/FbpAPI_FbpOrderList).
15. Get details of a specific supply: [/v1/fbp/order/get](#operation/FbpAPI_FbpOrderGet). If the supply request has errors, cancel it: [/v1/fbp/order/drop-off/cancel](#operation/FbpAPI_FbpOrderDropOffCancel).
16. Create a shipping label generation task: [/v1/fbp/label/create](#operation/FbpAPI_FbpCreateLabel).
17. Get labels and their generation status: [/v1/fbp/label/get](#operation/FbpAPI_FbpGetLabel).
18. Create a waybill generation task: [/v1/fbp/act-to/create](#operation/FbpAPI_FbpCreateConsignmentNote).
19. Get the waybill and its generation status: [/v1/fbp/act-to/get](#operation/FbpAPI_FbpCheckConsignmentNoteState).
20. Create an acceptance certificate generation task: [/v1/fbp/act-from/create](#operation/FbpAPI_FbpCreateAct).
21. Get the certificate and its generation status: [/v1/fbp/act-from/get](#operation/FbpAPI_FbpCheckActState).
22. Get a list of completed supplies: [/v1/fbp/archive/list](#operation/FbpAPI_FbpArchiveList).
23. Get details of a completed supply: [/v1/fbp/archive/get](#operation/FbpAPI_FbpArchiveGet).

### Pick-up Supply

1. Get a list of partner warehouses: [/v1/fbp/warehouse/list](#operation/FbpWarehouseList).
2. Validate products for partner warehouse acceptance: [/v1/fbp/draft/pick-up/product/validate](#operation/FbpAPI_FbpDraftPickUpProductValidate).
3. Create a pick-up delivery draft: [/v1/fbp/draft/pick-up/dlv/edit](#operation/FbpAPI_FbpOrderPickUpDlvEdit).
4. Edit pick-up delivery details in the draft: [/v1/fbp/draft/pick-up/dlv/edit](#operation/FbpAPI_FbpOrderPickUpDlvEdit).
5. Get a list of drafts: [/v1/fbp/draft/list](#operation/FbpAPI_FbpDraftList).
6. Get details of a specific draft: [/v1/fbp/draft/get](#operation/FbpAPI_FbpDraftGet). If the draft has errors, delete it: [/v1/fbp/draft/pick-up/delete](#operation/FbpAPI_FbpDraftPickUpDelete).
7. Get information about a supply's contents using `bundle_id`: [/v1/supply-order/bundle](#operation/SupplyOrderBundle).
8. Register a draft supply request, so it becomes active: [/v1/fbp/draft/pick-up/registrate](#operation/FbpDraftPickUpRegistrate).
9. Edit pick-up point details: [/v1/fbp/order/pick-up/dlv/edit](#operation/FbpAPI_FbpOrderPickUpDlvEdit).
10. Get a list of supplies: [/v1/fbp/order/list](#operation/FbpAPI_FbpOrderList).
11. Get details of a specific supply: [/v1/fbp/order/get](#operation/FbpAPI_FbpOrderGet). If the supply request has errors, cancel it: [/v1/fbp/order/pick-up/cancel](FbpAPI_FbpOrderPickUpCancel).
12. Create a shipping label generation task: [/v1/fbp/label/create](#operation/FbpAPI_FbpCreateLabel).
13. Get labels and their generation status: [/v1/fbp/label/get](#operation/FbpAPI_FbpGetLabel).
14. Create a waybill generation task: [/v1/fbp/act-to/create](#operation/FbpAPI_FbpCreateConsignmentNote).
15. Get the waybill and its generation status: [/v1/fbp/act-to/get](#operation/FbpAPI_FbpCheckConsignmentNoteState).
16. Create an acceptance certificate generation task: [/v1/fbp/act-from/create](#operation/FbpAPI_FbpCreateAct).
17. Get the certificate and its generation status: [/v1/fbp/act-from/get](#operation/FbpAPI_FbpCheckActState).
18. Get a list of completed supplies: [/v1/fbp/archive/list](#operation/FbpAPI_FbpArchiveList).
19. Get details of a completed supply: [/v1/fbp/archive/get](#operation/FbpAPI_FbpArchiveGet).

# Getting information about product returns

Get information about returned products: [/v1/returns/list](#operation/returnsList).

# Get return shipments by barcode

Check if you can pick up return shipments by barcode: [/v1/return/giveout/is-enabled](#operation/ReturnAPI_GiveoutIsEnabled). If you can, the `enabled` parameter will be `true`.

- To receive a return shipment, get the barcode:
   - in PDF format: [/v1/return/giveout/get-pdf](#operation/ReturnAPI_GiveoutGetPDF);
   - in PNG format: [/v1/return/giveout/get-png](#operation/ReturnAPI_GiveoutGetPNG);
   - in text format: [/v1/return/giveout/barcode](#operation/ReturnAPI_GiveoutGetBarcode).
- Get a list of return shipments: [/v1/return/giveout/list](#operation/ReturnAPI_GiveoutList).
- Get information on the selected shipment: [/v1/return/giveout/info](#operation/ReturnAPI_GiveoutInfo).

You can pick up returns apart from the product supply. If you need a pass to get to Ozon warehouse, create it using the [/v1/return/pass/create](#operation/returnPassCreate) method.
You need a new pass for each visit to pick up returns.

To update or delete the pass, use the [/v1/return/pass/update](#operation/returnPassUpdate) and [/v1/return/pass/delete](#operation/returnPassDelete) methods.

You can get a list of all passes using the [/v1/pass/list](#operation/PassList) method.

[Learn more about issuing passes](https://seller-edu.ozon.ru/fbs/ozon-logistika/oformlenie-propuska)

# Managing cancellation requests

Get information about cancellation requests: [/v2/conditional-cancellation/list](#operation/CancellationAPI_GetConditionalCancellationListV2).

Make a decision on the request:
- Approve the request: [/v2/conditional-cancellation/approve](#operation/CancellationAPI_ConditionalCancellationApproveV2). The order will be canceled and the money will be refunded to the customer.
- Reject the request: [/v2/conditional-cancellation/reject](#operation/CancellationAPI_ConditionalCancellationRejectV2). The order will remain in the same status and must be delivered to the customer.

# Managing rFBS return requests

Get information about return requests:
- Get a list of rFBS return requests: [/v2/returns/rfbs/list](#operation/RFBSReturnsAPI_ReturnsRfbsListV2).
- Get information about an rFBS return request: [/v2/returns/rfbs/get](#operation/RFBSReturnsAPI_ReturnsRfbsGetV2).

You can approve, compensate, confirm a refund, request a product for inspection and reject a request using the [/v1/returns/rfbs/action/set](#operation/ReturnsAPI_ReturnsRfbsActionSet) method.
To pass the correct `action`, get the list of available actions for a particular return using the [/v2/returns/returns/rfbs/get](#operation/RFBSReturnsAPI_ReturnsRfbsGetV2) method.

# Managing chats

Use the [/v3/chat/list](#operation/ChatAPI_ChatListV3) method to get a chats list. The response will contain the  
current chats and recent messages identifiers.

To send messages by the chat identifiers, use the following methods:

- To send a text message: [/v1/chat/send/message](#operation/ChatAPI_ChatSendMessage).

- To send a file or an image: [/v1/chat/send/file](#operation/ChatAPI_ChatSendFile).

To get the chat history by the chat or message identifier, use the [/v3/chat/history](#operation/ChatAPI_ChatHistoryV3) method. The default sorting direction is from new messages to old ones.

If you specify the message identifier, the history will be shown starting with this message.

To create a new chat with the customer by shipment number, use the [/v1/chat/start](#operation/ChatAPI_ChatStart) method.

To mark a message and all messages before it as read, use the [/v2/chat/read](#operation/ChatAPI_ChatReadV2) method.

# Creating and getting reports

When requesting any of the reports, you get the document creation code first. Use it in the  
[/v1/report/info](#operation/ReportAPI_ReportInfo) method request. You will get a report file with additional information in the response.

To get a list of reports generated earlier, use the [/v1/report/list](#operation/ReportAPI_ReportList) method.

Use the following methods to get reports:

- A report with the product data, for example, Ozon ID, product description, price, commission, or package 
  dimensions: [/v1/report/products/create](#operation/ReportAPI_CreateCompanyProductsReport).

- A transactions report, which is available in the seller account: [/v3/finance/transaction/list](#operation/FinanceAPI_FinanceTransactionListV3).

- A price report: [/v4/product/info/price](#operation/ProductAPI_GetProductInfoPricesV4).

- A stock report: [/v1/report/warehouse/stock](#operation/ReportAPI_CreateStockByWarehouseReport).

- A returned products report for FBS and rFBS: [/v2/report/returns/create](#operation/ReportAPI_ReportReturnsCreate). 
  The report contains the details on the products accepted from customers or handed over to the seller.

- A shipments report: [/v1/report/postings/create](#operation/ReportAPI_CreateCompanyPostingsReport).

- A financial report: [/v1/finance/cash-flow-statement/list](#operation/FinanceAPI_FinanceCashFlowStatementList).

- A report on markdown products: [/v1/report/discounted/create](#operation/ReportAPI_CreateDiscountedReport).

# Getting analytical reports

- Get analytics data: [/v1/analytics/data](#operation/AnalyticsAPI_AnalyticsGetData).

  If you specify the period and metrics to be calculated, the response will contain analytics grouped by the  
  `dimensions` parameter.

- A report on stocks and products in transfer in Ozon warehouses: [/v2/analytics/stock_on_warehouses](#operation/AnalyticsAPI_AnalyticsGetStockOnWarehousesV2).

- Get analytics on stock balances: [/v1/analytics/stocks](#operation/AnalyticsAPI_AnalyticsStocks).

To get an FBO turnover report, request it [in your personal account](https://seller.ozon.ru/app/analytics/fulfillment-reports/turnover).

# Getting financial reports

- Get detailed information on transactions by shipments: [/v3/finance/transaction/list](#operation/FinanceAPI_FinanceTransactionListV3).

- Get detailed information on the total transactions amounts for the specified period: [/v3/finance/transaction/totals](#operation/FinanceAPI_FinanceTransactionTotalV3).

# Getting information on ratings

- Get the current seller ratings: [/v1/rating/summary](#operation/RatingAPI_RatingSummaryV1).

- Get information on seller ratings for the period and the number of penalty points received in the Premium program: [/v1/rating/history](#operation/RatingAPI_RatingHistoryV1).

