# Basic Methods

## APIkey

### `POST /v1/roles`

**Get a list of roles and methods based on the API key**

Operation ID: `AccessAPI_RolesByToken`

Method for getting information about roles and methods available for your API key.

**Response 200:**

- `expires_at` (string(date-time)) — Date and time when the key expires.
- `roles` (array[object]) — Information about available roles and methods.
  - `name` (string) — Role name.
  - `methods` (array[string]) — Methods available for the role.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## SellerInfo

### `POST /v1/seller/info`

**Get information about seller account**

Operation ID: `SellerAPI_SellerInfo`

**Response 200:**

- `company` (object)
  - `country` (string) — Country.
  - `currency` (string) — Currency.
  - `inn` (string) — Taxpayer identification number (INN).
  - `legal_name` (string) — Company legal name.
  - `name` (string) — Company name on Ozon.
  - `ogrn` (string) — Primary state registration number (OGRN).
  - `ownership_form` (string) — Ownership form.
  - `tax_system` (enum) — Values: `UNKNOWN, UNSPECIFIED, OSNO, USN, NPD, AUSN, PSN`
- `ratings` (array[object]) — Rating list.
  - `current_value` (object)
  - `name` (string) — Rating name.
  - `past_value` (object)
  - `rating` (string) — Rating name in system.
  - `status` (enum) — Values: `UNKNOWN, OK, WARNING, CRITICAL`
  - `value_type` (enum) — Values: `UNKNOWN, INDEX, PERCENT, TIME, RATIO, REVIEW_SCORE, COUNT`
- `subscription` (object)
  - `is_premium` (boolean) — `true` if subscription is available.
  - `type` (enum) — Values: `UNKNOWN, UNSPECIFIED, PREMIUM, PREMIUM_LITE, PREMIUM_PLUS, PREMIUM_PRO`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/seller/ozon-logistics/info`

**Get information about connecting to Ozon Delivery**

Operation ID: `SellerAPI_SellerOzonLogisticsInfo`

**Response 200:**

- `available_schemas` (array[object]) — Available scheme type.
- `ozon_logistics_enabled` (boolean) — `true` if Ozon Delivery is connected.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## CategoryAPI

### `POST /v1/description-category/tree`

**Tree of product category and type**

Operation ID: `DescriptionCategoryAPI_GetTree`

Returns product categories in the tree view. 

New products can be created in the last level categories only.
This means that you need to match these particular categories with the categories of your site.
We don't create new categories by user request.

> **Note:** 
   Choose the category for the product carefully: there are different commission fees for different categories.

**Request body:**

- `language` (enum) — Values: `DEFAULT, RU, EN, TR, ZH_HANS`

**Response 200:**

- `result` (array[object]) — Categories list.
  - `description_category_id` (integer(int64)) — Category identifier.
  - `category_name` (string) — Category name.
  - `children` (array[object]) — Subcategory tree.
  - `disabled` (boolean) — `true`, if you can't create products in the category. `false`, if you can.
  - `type_id` (integer(int64)) — Product type identifier.
  - `type_name` (string) — Product type name.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/description-category/attribute`

**Category characteristics list**

Operation ID: `DescriptionCategoryAPI_GetAttributes`

Getting characteristics for specified product category and type.

If the `dictionary_id` value is `0`, there is no directory.
If the value is different, there are directories. 
Get them using the [/v1/description-category/attribute/values](#operation/DescriptionCategoryAPI_GetAttributeValues) method.

**Request body:**

- `description_category_id` (integer(int64)) **(required)** — Category identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.
- `language` (enum) — Values: `DEFAULT, RU, EN, TR, ZH_HANS`
- `type_id` (integer(int64)) **(required)** — Product type identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.

**Response 200:**

- `result` (array[object]) — Method result.
  - `category_dependent` (boolean) — Indication that the dictionary attribute values depend on the category: - `true`—the attribute has its own set of values for each category. - `false`—
  - `description` (string) — Characteristic description.
  - `dictionary_id` (integer(int64)) — Directory identifier.
  - `group_id` (integer(int64)) — Characteristics group identifier.
  - `group_name` (string) — Characteristics group name.
  - `id` (integer(int64)) — Characteristic identifier.
  - `is_aspect` (boolean) — Indicates that the attribute is aspect. An aspect attribute is a characteristic that distinguishes products of the same model.  For example, clothes o
  - `is_collection` (boolean) — Indicates that the characteristic is a set of values:  - `true`—the characteristic is a set of values,  - `false`—the characteristic consists of a sin
  - `is_required` (boolean) — Indicates that the characteristic is mandatory:  - `true`—a mandatory characteristic,  - `false`—an optional characteristic.
  - `name` (string) — Name.
  - `type` (string) — Characteristic type.
  - `attribute_complex_id` (integer(int64)) — Complex attribute identifier.
  - `max_value_count` (integer(int64)) — Maximum number of values for attribute.
  - `complex_is_collection` (boolean) — Indicates that the complex characteristic is a set of values:  - `true`—the complex characteristic is a set of values,  - `false`—the complex characte

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/description-category/attribute/values`

**Characteristics value directory**

Operation ID: `DescriptionCategoryAPI_GetAttributeValues`

Returns characteristics value directory.

To check if an attribute has a nested directory, use the [/v1/description-category/attribute](#operation/DescriptionCategoryAPI_GetAttributes) method.

**Request body:**

- `attribute_id` (integer(int64)) **(required)** — Characteristics identifier. You can get it using the [/v1/description-category/attribute](#operation/DescriptionCategoryAPI_GetAttributes) method.
- `description_category_id` (integer(int64)) **(required)** — Category identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.
- `language` (enum) — Values: `DEFAULT, RU, EN, TR, ZH_HANS`
- `last_value_id` (integer(int64)) — Identifier of the directory to start the response with. If `last_value_id` is 10, the response will contain directories starting from the 11th.
- `limit` (integer(int64)) **(required)** — Number of values in the response:  - maximum—2000,  - minimum—1.
- `type_id` (integer(int64)) **(required)** — Product type identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.

**Response 200:**

- `has_next` (boolean) — Indication that only part of characteristic values was returned in the response: - `true`—make a request with a new `last_value_id` parameter value fo
- `result` (array[object]) — Characteristic values.
  - `id` (integer(int64)) — Characteristic value identifier.
  - `info` (string) — Additional description.
  - `picture` (string) — Image link.
  - `value` (string) — Product characteristic value.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/description-category/attribute/values/search`

**Search by reference values of a characteristic**

Operation ID: `DescriptionCategoryAPI_SearchAttributeValues`

Returns characteristic reference values for the specified `value` in the request.

To check if an attribute has a nested directory, use the [/v1/description-category/attribute](#operation/DescriptionCategoryAPI_GetAttributes) method.

**Request body:**

- `attribute_id` (integer(int64)) **(required)** — Characteristic identifier. You can get it using the [/v1/description-category/attribute](#operation/DescriptionCategoryAPI_GetAttributes) method.
- `description_category_id` (integer(int64)) **(required)** — Category identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.
- `limit` (integer(int64)) **(required)** — Number of values in the response. The minimum value is 1, the maximum is 100.
- `type_id` (integer(int64)) **(required)** — Product type identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.
- `value` (string) **(required)** — By this value the system searches for reference values. It must be at least 2 characters.

**Response 200:**

- `result` (array[object]) — Characteristic values.
  - `id` (integer(int64)) — Characteristic value identifier.
  - `info` (string) — Additional information.
  - `picture` (string) — Image link.
  - `value` (string) — Product characteristic value.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## ProductAPI

### `POST /v3/product/import`

**Create or update a product**

Operation ID: `ProductAPI_ImportProductsV3`

> **Note:** 
If you're selling from China or Hong Kong under the FBP scheme, generate a barcode for each product item. Partner warehouses don't accept products without barcodes.

This method allows you to create products and update their details.

You can add or update a certain number of products per day. To find out the limit, use
[/v4/product/info/limit](#operation/ProductAPI_GetUploadQuota). If the number of product creations
and updates exceeds the limit, an `item_limit_exceeded` error will appear.

You can pass up to 100 products in one request.
Each product is a separate element in the ...

**Request body:**

- `items` (array[object]) — Data array.
  - `attributes` (array[object]) — Array with the product characteristics. The characteristics depend on category. You can view them in [Help Center](https://global.ozon.ru/en/docs/prod
    - `complex_id` (integer(int64)) — Identifier of the characteristic that supports nested properties. For example, the "Processor" characteristic has nested characteristics "Manufacturer
    - `id` (integer(int64)) — Characteristic identifier.
    - `values` (array[object]) — Array of nested characteristic values.
  - `barcode` (string) — Product barcode.
  - `color_image` (string) — Marketing color.  Pass the link to the image in the public cloud storage. The image format is JPG.
  - `complex_attributes` (array[object]) — Array of characteristics that have nested attributes.
    - `attributes` (array[object])
  - `currency_code` (string) — Currency of your prices. The passed value must be the same as the one set in the personal account settings.  By default, the passed value is `RUB`, Ru
  - `depth` (integer(int32)) — Package depth.
  - `description_category_id` (integer(int64)) **(required)** — Category identifier. You can get it using the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTree) method.
  - `new_description_category_id` (integer(int64)) — New category identifier. Specify it if you want to change the current product category.
  - `dimension_unit` (string) — Dimensions measurement units:  - `mm`—millimeters,  - `cm`—centimeters,  - `in`—inches.
  - `geo_names` (array[string]) — Geo-restrictions — fill in the parameter in your personal account when creating or editing a product.  Optional parameter.
  - `height` (integer(int32)) — Package height.
  - `images` (array[string]) — Array of images, up to 30 files. The images are displayed on the site in the same order as they are in the array.  The first one will be set as the ma
  - `images360` (array[string]) — Array of 360 images—up to 70 files.  Pass links to images in the public cloud storage. The image format is JPG.
  - `name` (string) — Product name. Up to 500 characters.  [Learn more about product name requirements](https://docs.ozon.ru/global/en/products/requirements/product-info/na
  - `offer_id` (string) — Product identifier in the seller's system.  The maximum length of a string is 50 characters.
  - `old_price` (string) — Price before discounts. Displayed strikethrough on the product description page. Specified in rubles. The fractional part is separated by decimal poin
  - `pdf_list` (array[object]) — List of PDF files.
    - `index` (integer(int64)) — Storage order index.
    - `name` (string) — File name.
    - `src_url` (string) — File address.
  - `price` (string) **(required)** — Product price including discounts. This value is shown on the product description card. If there are no discounts on the product, specify the `old_pri
  - `primary_image` (string) — Link to main product image.
  - `promotions` (array[object]) — Special offers.
    - `operation` (enum) — Values: `UNKNOWN, ENABLE, DISABLE`. Attribute for action with special offer: - `ENABLE`—enable, - `DISABLE`—disable, - `UNKNOWN`—no changes, passed by default.
    - `type` (enum) — Values: `REVIEWS_PROMO`. Special offer type: - `REVIEWS_PROMO`—"Points for reviews" special offer.
  - `service_type` (enum) — Values: `IS_CODE_SERVICE, IS_NO_CODE_SERVICE`
  - `type_id` (integer(int64)) **(required)** — Product type identifier. You can get values from the type_id parameter in the [/v1/description-category/tree](#operation/DescriptionCategoryAPI_GetTre
  - `vat` (string) — VAT rate for the product:  - `0`—not subject to VAT,  - `0.05`—5%,  - `0.07`—7%,  - `0.1`—10%,  - `0.2`—20%,  - `0.22`—22%.  Pass the rate value that 
  - `weight` (integer(int32)) — Product weight with the package. The limit value is 1000 kilograms or a corresponding converted value in other measurement units.
  - `weight_unit` (string) — Weight measurement units: - `g`—grams, - `kg`—kilograms, - `lb`—pounds.
  - `width` (integer(int32)) — Package width.

**Response 200:**

- `result` (object)
  - `task_id` (integer(int64)) — Identifier of the task for uploading products. Check the product creation or update status using the [/v1/product/import/info](#operation/ProductAPI_G

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

### `POST /v1/product/import/info`

**Get the product import status**

Operation ID: `ProductAPI_GetImportProductsInfo`

Allows you to get the status of a product description page creation or update process.

**Request body:**

- `task_id` (integer(int64)) **(required)** — Importing products task code. You can get it using the [/v3/product/import](#operation/ProductAPI_ImportProductsV3) method.

**Response 200:**

- `result` (object)
  - `items` (array[object]) — Product details.
  - `total` (integer(int32)) — Product identifier in the seller's system.

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

### `POST /v1/product/import-by-sku`

**Create a product by SKU**

Operation ID: `ProductAPI_ImportProductsBySKU`

The method creates a [copy of the product description page](https://docs.ozon.ru/global/en/products/upload/upload-types/copying/?country=OTHER) with the specified SKU.

The method creates a copy of PDP from other seller. You cannot create a copy of your product.

You cannot create a copy if the seller has prohibited the copying of their PDPs.

It's not possible to update products using SKU.

**Request body:**

- `items` (array[object]) — Products details.
  - `name` (string) — Product name. Up to 500 characters.
  - `offer_id` (string) — Product identifier in the seller's system.  The maximum length of a string is 50 characters.
  - `old_price` (string) — Price before discounts. Displayed strikethrough on the product description page. Specified in rubles. The fractional part is separated by decimal poin
  - `price` (string) — Product price including discounts. This value is shown on the product description page. If there are no discounts, pass the `old_price` value in this 
  - `currency_code` (string) — Currency of your prices. The passed value must be the same as the one set in the personal account settings.  By default, the passed value is `RUB`, Ru
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
  - `vat` (string) — VAT rate for the product:  - `0`—not subject to VAT,  - `0.05`—5%,  - `0.07`—7%,  - `0.1`—10%,  - `0.2`—20%,  - `0.22`—22%.  Pass the rate value that 

**Response 200:**

- `result` (object)
  - `task_id` (integer(int64)) — Products import task code.
  - `unmatched_sku_list` (array[integer]) — Products identifiers list.

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

### `POST /v1/product/attributes/update`

**Update product characteristics**

Operation ID: `ProductAPI_ProductUpdateAttributes`

This method allows you to add characteristics and change their values. You can't delete characteristics that are already filled out. To completely update characteristics, use [/v3/product/import](#operation/ProductAPI_ImportProductsV3).

**Request body:**

- `items` (object) — Products and characteristics to be updated.

**Response 200:**

- `task_id` (integer(int64)) — Products update task code.  To check the update status, pass the received value to the [/v1/product/import/info](#operation/ProductAPI_GetImportProduc

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

### `POST /v1/product/pictures/import`

**Upload and update product images**

Operation ID: `ProductAPI_ProductImportPictures`

The method for uploading and updating product images.

Each time you call the method, pass all the images that should be on the product description page. For example, if you call a method and upload 10 images, and then call the method a second time and load one imahe,
then all 10 previous ones will be erased.

To upload image, pass a link to it in a public cloud storage. The image format is JPG or PNG.

Arrange the pictures in the `images` array as you want to see them on the site. The first picture in the array will
be the main one for the product.

You can upload up to 30 pictures for each p...

**Request body:**

- `color_image` (string) — Marketing color.
- `images` (object) — Array of links to images, up to 30 links. The images in the array are arranged in the order of their arrangement on the site. The first image in the l
- `images360` (object) — Array of 360 images—up to 70 files.
- `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, `product_id`.

**Response 200:**

- `result` (object)
  - `pictures` (object)

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

### `POST /v3/product/list`

**List of products**

Operation ID: `ProductAPI_GetProductList`

Method for getting a list of all products.

When using the filter by `offer_id` or `product_id` identifiers, other parameters aren't required.
At a time you can use only one identifier group with 1000 products or less.

If you don't use identifiers for display, specify the `limit` and `last_id` parameters in subsequent requests.

**Request body:**

- `filter` (object)
  - `offer_id` (object) — Filter by the `offer_id` parameter. You can pass a list of values in this parameter.
  - `product_id` (object) — Filter by the `product_id` parameter. You can pass a list of values in this parameter.
  - `visibility` (enum) — Values: `ALL, VISIBLE, INVISIBLE, EMPTY_STOCK, NOT_MODERATED, MODERATED, DISABLED, STATE_FAILED`
- `last_id` (string) — Identifier of the last value on the page. Leave this field blank in the first request.  To get the next values, specify `last_id` from the response of
- `limit` (integer(int64)) — Number of values per page. Minimum is 1, maximum is 1000.

**Response 200:**

- `result` (object)
  - `items` (object) — Product list.
  - `last_id` (string) — Identifier of the last value on the page.  To get the next values, specify the recieved value in the next request in the `last_id` parameter.
  - `total` (integer(int32)) — Total number of products.

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

### `POST /v1/product/rating-by-sku`

**Get products' content rating by SKU**

Operation ID: `ProductAPI_GetProductRatingBySku`

Method for getting products' content rating and recommendations on how to increase it.

[Learn more about content rating](https://docs.ozon.ru/global/en/products/general-information/content-rating/)

**Request body:**

- `skus` (object) **(required)** — Product identifiers in the Ozon system, SKUs, for which content rating should be returned.

**Response 200:**

- `products` (object) — Products' content rating.

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

### `POST /v3/product/info/list`

**Get a list of products by identifiers**

Operation ID: `ProductAPI_GetProductInfoList`

Method for getting an array of products by their identifiers.

Request body must contain an array of identifiers of the same type. The response contains an `items` array.

In one request, you can pass up to 1,000 products by `offer_id`, `product_id`, and `sku` parameters in total.

**Request body:**

- `offer_id` (array[string]) — Product identifier in the seller's system.
- `product_id` (array[string]) — Product identifier in the Ozon system, `product_id`.
- `sku` (array[string]) — Product identifier in the Ozon system, SKU.

**Response 200:**

- `items` (array[object]) — Data array.
  - `availabilities` (array[object]) — Product availability information.
  - `barcodes` (array[string]) — All product barcodes.
  - `color_image` (array[string]) — Product color image.
  - `commissions` (array[object]) — Commission fees details.
  - `created_at` (string(date-time)) — Date and time when the product was created.
  - `currency_code` (string) — Currency.
  - `description_category_id` (integer(int64)) — Category identifier. Use it with the [/v1/description-category/attribute](#operation/DescriptionCategoryAPI_GetAttributes) and [/v1/description-catego
  - `discounted_fbo_stocks` (integer(int32)) — Markdown product stocks at the Ozon warehouse.
  - `errors` (array[object]) — Details on errors when creating or validating a product.
  - `has_discounted_fbo_item` (boolean) — Indication that the product has similar markdown products at the Ozon warehouse.
  - `id` (integer(int64)) — Product identifier.
  - `images` (array[string]) — Array of links to images. The images in the array are arranged in the order of their arrangement on the site. If the `primary_image` parameter isn't s
  - `images360` (array[string]) — Array of 360 images.
  - `is_archived` (boolean) — `true` if the product is archived manually.
  - `is_autoarchived` (boolean) — `true` if the product is archived automatically.
  - `is_discounted` (boolean) — Indication of a markdown product:    - `true` if the product was created by the seller as a markdown.    - `false` if the product isn't markdown or wa
  - `is_kgt` (boolean) — `true` if the product is bulky. Only for the FBS scheme.
  - `is_prepayment_allowed` (boolean) — `true` if prepayment is possible.
  - `is_super` (boolean) — Indication of a super product.  [Read more about super products in Help center](https://seller-edu.ozon.ru/fbo/rabota-so-stokom/super-tovary)
  - `min_price` (string) — Minimum product price with all special offers applied.
  - `model_info` (object)
  - `name` (string) — Name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `old_price` (string) — Price before discounts. Displayed strikethrough on the product description page.
  - `price` (string) — Product price including discounts. This value is shown on the product description page.
  - `price_indexes` (object)
  - `primary_image` (array[string]) — Main product image.
  - `promotions` (array[object]) — Special offers.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `sources` (array[object]) — Details on the sources of product creation.
  - `statuses` (object)
  - `stocks` (object)
  - `type_id` (integer(int64)) — Product type identifier.
  - `updated_at` (string(date-time)) — Date of the last product update.
  - `vat` (string) — Product VAT rate.
  - `visibility_details` (object)
  - `volume_weight` (number(double)) — Product volume weight.

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

### `POST /v4/product/info/attributes`

**Get a description of the product characteristics**

Operation ID: `ProductAPI_GetProductAttributesV4`

Returns a product characteristics description by product identifier or visibility. You can search for the product by `offer_id`, `product_id` or `sku`.

**Request body:**

- `filter` (object)
  - `offer_id` (object) — Filter by the `offer_id` parameter. It is possible to pass a list of values.
  - `product_id` (object) — Filter by the `product_id` parameter. It is possible to pass a list of up to 1000 values.
  - `sku` (array[string]) — Product identifier in the Ozon system, SKU.
  - `visibility` (enum) — Values: `ALL, VISIBLE, INVISIBLE, EMPTY_STOCK, NOT_MODERATED, MODERATED, DISABLED, STATE_FAILED`
- `last_id` (string) — Identifier of the last value on the page. Leave this field blank in the first request.  To get the next values, specify `last_id` from the response of
- `limit` (integer(int32)) — Number of values per page.
- `sort_by` (string) — Parameter by which you can sort the products: - `sku`: sorting by product identifier in Ozon system; - `offer_id`: sorting by product article code; - 
- `sort_dir` (string) — Sorting direction: - `asc`—ascending, - `desc`—descending.

**Response 200:**

- `result` (object) — Request results.
- `last_id` (string) — Identifier of the last value on the page.  To get the next values, specify the recieved value in the next request in the `last_id` parameter.
- `total` (integer) — Number of products in the list.

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

### `POST /v1/product/info/description`

**Get product description**

Operation ID: `ProductAPI_GetProductInfoDescription`

**Request body:**

- `offer_id` (string) — Product identifier in the seller's system.
- `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.

**Response 200:**

- `result` (object)
  - `description` (string) — Description.
  - `id` (integer(int64)) — Identifier.
  - `name` (string) — Name.
  - `offer_id` (string) — Product identifier in the seller's system.

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

### `POST /v4/product/info/limit`

**Product range limit, limits on product creation and update**

Operation ID: `ProductAPI_GetUploadQuota`

Method for getting information about the following limits:
- Product range limit: how many products you can create in your personal account.
- Products creation limit: how many products you can create per day.
- Products update limit: how many products you can update per day.

If you have a product range limit and you exceed it, you won't be able to create new products.

**Response 200:**

- `daily_create` (object)
  - `limit` (integer(int64)) — Number of products you can create daily. If the value is `-1`, there is no limit.
  - `reset_at` (string(date-time)) — Counter reset time for the current day in UTC format.
  - `usage` (integer(int64)) — How many products you've created in the current day.
- `daily_update` (object)
  - `limit` (integer(int64)) — Number of products you can update daily. If the value is `-1`, there is no limit.
  - `reset_at` (string(date-time)) — Counter reset time for the current day in UTC format.
  - `usage` (integer(int64)) — How many products you've updated in the current day.
- `total` (object)
  - `limit` (integer(int64)) — Number of products you can create in your personal account. If the value is `-1`, there is no limit.
  - `usage` (integer(int64)) — How many products you've already created.

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

### `POST /v1/product/update/offer-id`

**Change product identifiers from the seller's system**

Operation ID: `ProductAPI_ProductUpdateOfferID`

Method for changing the `offer_id` linked to products. You can change multiple `offer_id` in this method.

**Request body:**

- `update_offer_id` (object) **(required)** — List of pairs with new and old values of product identifiers

**Response 200:**

- `errors` (object) — Errors list.

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

### `POST /v1/product/archive`

**Archive a product**

Operation ID: `ProductAPI_ProductArchive`

**Request body:**

- `product_id` (array[integer]) **(required)** — Product identifiers. You can pass up to 100 identifiers at a time.

**Response 200:**

- `result` (boolean) — The result of processing the request. `true` if the request was executed without errors.

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

### `POST /v1/product/unarchive`

**Unarchive a product**

Operation ID: `ProductAPI_ProductUnarchive`

Method is available to sellers from China and Turkey. You can restore up to 10 archived products per day via the `product_id` parameter.

**Request body:**

- `product_id` (array[integer]) **(required)** — Product identifiers in the Ozon system. You can pass up to 100 identifiers at a time.  You can restore up to 100 automatically archived products per d

**Response 200:**

- `result` (boolean) — The result of processing the request. `true` if the request was executed without errors.

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

### `POST /v2/products/delete`

**Remove a product without an SKU from the archive**

Operation ID: `ProductAPI_DeleteProducts`

You can pass up to 500 identifiers in one request.

**Request body:**

- `products` (array[object]) **(required)** — Product identifier.
  - `offer_id` (string) **(required)** — Product identifier in the seller's system.

**Response 200:**

- `status` (array[object]) — Product processing status.
  - `error` (string) — Reason of the error that occurred while processing the request.
  - `is_deleted` (boolean) — If the request was executed without errors and the products were deleted, the value is `true`.
  - `offer_id` (string) — Product identifier in the seller's system.

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

### `POST /v1/product/info/subscription`

**Number of users subscribed to product availability alerts**

Operation ID: `ProductAPI_GetProductInfoSubscription`

You can pass multiple products in a request.

**Request body:**

- `skus` (array[string]) **(required)** — List of SKUs, product identifiers in the Ozon system.

**Response 200:**

- `result` (array[object]) — Method result.
  - `count` (integer(int64)) — Number of subscribed users.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/related-sku/get`

**Get related SKUs**

Operation ID: `ProductAPI_ProductGetRelatedSKU`

Method for getting a single SKU based on the old SKU FBS and SKU FBO identifiers.
The response will contain all SKUs related to the passed ones.

The method can handle any SKU, even hidden or deleted.

In one request, you can pass up to 200 SKUs.

**Request body:**

- `sku` (object) **(required)** — List of SKUs.

**Response 200:**

- `items` (object) — Related SKUs information.
- `errors` (object) — Errors.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/product/pictures/info`

**Get products images**

Operation ID: `ProductAPI_ProductInfoPicturesV2`

**Request body:**

- `product_id` (object) **(required)** — Product identifier in the Ozon system, `product_id`.

**Response 200:**

- `items` (array[object]) — Product images.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `primary_photo` (array[string]) — Main image link.
  - `photo` (array[string]) — Links to product photos.
  - `color_photo` (array[string]) — Links to uploaded color samples.
  - `photo_360` (array[string]) — 360 images links.
  - `errors` (array[object]) — List of product image errors.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/info/wrong-volume`

**List of products with incorrect VWC**

Operation ID: `ProductAPI_ProductInfoWrongVolume`

Returns a list of products with incorrect volume and weight characteristics (VWC). If you specified them correctly, please contact Ozon support.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `limit` (integer(int64)) **(required)** — Maximum number of values in the response.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `products` (object) — Product list.

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

## BarcodeAPI

### `POST /v1/barcode/add`

**Bind barcodes to products**

Operation ID: `add-barcode`

If a product has a barcode that isn't specified in your account, bind it using this method.
If a product doesn't have a barcode, you can create it using the [/v1/barcode/generate](#operation/generate-barcode) method.

Each product can have up to 100 barcodes.
You can use the method for no more than 20 requests per minute in one account.

**Request body:**

- `barcodes` (array[object]) **(required)** — List of barcodes and products.
  - `barcode` (string) **(required)** — Barcode. Maximum 100 characters.
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.

**Response 200:**

- `errors` (array[object]) — Errors while binding barcodes.
  - `code` (string) — Error code.
  - `error` (string) — Error details.
  - `barcode` (string) — Barcode that is failed to bind.
  - `sku` (integer(int64)) — SKU of the product for which the barcode binding failed.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/barcode/generate`

**Generate barcodes for products**

Operation ID: `generate-barcode`

If a product doesn't have a barcode, you can create it using this method.
If a barcode already exists, but it isn't specified in your account, you can bind it using the [/v1/barcode/add](#operation/add-barcode) method.

You can't generate barcodes for more than 100 products per request. 
You can use the method for no more than 20 requests per minute in one account.

**Request body:**

- `product_ids` (array[string]) **(required)** — List of products for which you want to generate barcodes.

**Response 200:**

- `errors` (array[object]) — Errors while generating barcodes.
  - `code` (string) — Error code.
  - `error` (string) — Error details.
  - `barcode` (string) — Barcode that is failed to generate.
  - `product_id` (integer(int64)) — Product identifier for which the barcode generation failed.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## Prices&StocksAPI

### `POST /v2/products/stocks`

**Update the quantity of products in stock**

Operation ID: `ProductAPI_ProductsStocksV2`

Allows you to change the products in stock quantity.

> **Note:** 
 Passed stock is the number of products available for sale without taking reserved products into account. Before updating the stock, check the number of reserved products using the /v1/product/info/stocks-by-warehouse/fbs method.  

With one request you can change the availability for 100 product-warehouse pairs. You can send up to 80 requests per minute in one account.

> **Note:** You can update the stock of one product-warehouse pair only once in 30 seconds, otherwise you get the TOO_MANY_REQUESTS error in the result.errors ...

**Request body:**

- `stocks` (array[object]) **(required)** — Information about the products at the warehouses.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, `product_id`.
  - `stock` (integer(int64)) **(required)** — Quantity of items in stock excluding reserved products.
  - `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier derived from the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.

**Response 200:**

- `result` (array[object])
  - `errors` (array[object]) — An array of errors that occurred while processing the request.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `updated` (boolean) — If the request was completed successfully and the stocks are updated—`true`.
  - `warehouse_id` (integer(int64)) — Warehouse identifier derived from the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.

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

### `POST /v4/product/info/stocks`

**Information about product quantity**

Operation ID: `ProductAPI_GetProductInfoStocks`

Returns information about the quantity of products under the FBS, rFBS, and FBP schemes:
  - how many items are available,
  - how many are reserved by customers.

To get stock information under the FBO scheme, use the [/v1/analytics/stocks](#operation/AnalyticsAPI_AnalyticsStocks) method.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `offer_id` (array[string]) — Filter by the `offer_id` parameter. It is possible to pass a list of values.
  - `product_id` (array[string]) — Filter by the `product_id` parameter. It is possible to pass a list of values.
  - `visibility` (enum) — Values: `ALL, VISIBLE, INVISIBLE, EMPTY_STOCK, NOT_MODERATED, MODERATED, DISABLED, STATE_FAILED`
  - `with_quant` (object)
    - `created` (boolean) — Active economy products.
    - `exists` (boolean) — Economy products in all statuses.
- `limit` (integer(int32)) **(required)** — Limit on number of entries in a reply. Default value is 1000. Maximum value is 1000.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `items` (array[object]) — Product details.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `product_id` (integer(int64)) — Product identifier.
  - `stocks` (array[object]) — Stock details.
- `total` (integer(int32)) — The number of unique products for which information about stocks is displayed.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/info/warehouse/stocks`

**Get information on stock in FBS and rFBS warehouse**

Operation ID: `ProductInfoWarehouseStocks`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `limit` (integer(int64)) **(required)** — Number of values per page.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample. If the parameter is empty, there is no more data.
- `has_next` (boolean) — Indicates that the response returned not all products: - `true` — make a new request with a different `cursor` value to get the remaining values; - `f
- `stocks` (array[object]) — Product stock information.
  - `free_stock` (integer(int64)) — Product quantity in the warehouse available for order.
  - `offer_id` (string) — Product identifier in the seller's system — `offer_id`.
  - `present` (integer(int64)) — Total product quantity in the warehouse.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `reserved` (integer(int64)) — Reserved product quantity in the warehouse.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `updated_at` (string(date-time)) — Date of the recent product update.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/info/stocks-by-warehouse/fbs`

**Stocks in seller's warehouses (FBS и rFBS)**

Operation ID: `ProductAPI_ProductStocksByWarehouseFbs`

Pass `offer_id` or `sku` in the request. If you specify both parameters, only `sku` is used.

**Request body:**

- `sku` (object) — Product identifier in the Ozon system, SKU.
- `offer_id` (object) — Product identifier in the seller's system.

**Response 200:**

- `result` (object) — Method result.

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

### `POST /v1/product/import/prices`

**Update prices**

Operation ID: `ProductAPI_ImportProductsPrices`

Allows you to change a price of one or more products.
The price of each product can be updated no more than 10 times per hour.
To reset `old_price`, set `0` for this parameter.

If the request contains both the `offer_id` and `product_id` parameters, the changes will be applied to the product with the `offer_id`. To avoid ambiguity, use only one of the parameters.

**Request body:**

- `prices` (array[object]) — Product prices details.
  - `auto_action_enabled` (enum) — Values: `UNKNOWN, ENABLED, DISABLED`. Attribute for special offer auto-application: - `ENABLED`—enable. - `DISABLED`—disable. - `UNKNOWN`—don't change anything. Default value.  If you've p
  - `currency_code` (string) — Currency of your prices. The passed value must be the same as the one set in the personal account settings.  By default, the passed value is `RUB`, Ru
  - `manage_elastic_boosting_through_price` (boolean) — Manage participation in the "Elastic Boosting" special offer:  - `true`: automatically adds the product to the special offer or increases its discount
  - `min_price` (string) — Minimum product price with all special offers applied.
  - `min_price_for_auto_actions_enabled` (boolean) — `true`, if Ozon takes into account the minimum price when creating special offers. If you don't pass anything, the status of the price accounting rema
  - `net_price` (string) — Product cost price.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `old_price` (string) — Price before discounts. Displayed strikethrough on the product description page. Specified in rubles. The fractional part is separated by decimal poin
  - `price` (string) — Product price including discounts. This value is displayed on the product description page.  If the `old_price` parameter value is greater than 0, the
  - `price_strategy_enabled` (string) — Attribute for enabling and disabling pricing strategies auto-application: - `ENABLED`—enable. - `DISABLED`—disable. - `UNKNOWN`—don't change anything.
  - `product_id` (integer(int64)) — Product identifier.
  - `quant_size` (integer(int64)) — Use parameter if the regular and economy products have the same article code—`offer_id = quant_id`. To update price of the:  - regular product, pass t
  - `vat` (string) — VAT rate for the product:  - `0`—not subject to VAT,  - `0.05`—5%,  - `0.07`—7%,  - `0.1`—10%,  - `0.2`—20%,  - `0.22`—22%.  Pass the rate value that 

**Response 200:**

- `result` (array[object])
  - `errors` (array[object]) — An array of errors that occurred while processing the request.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `updated` (boolean) — If the product details have been successfully updated—`true`.

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

### `POST /v1/product/action/timer/update`

**Update the minimum price relevance timer**

Operation ID: `ProductAPI_ActionTimerUpdate`

The minimum price is valid for 30 days after setting. 
After that, the setting turns off. You can extend it: use the method again and specify `product_ids`.

**Request body:**

- `product_ids` (array[string]) — List of product identifiers.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/action/timer/status`

**Get status of timer you've set**

Operation ID: `ProductAPI_ActionTimerStatus`

**Request body:**

- `product_ids` (object) — List of product identifiers.

**Response 200:**

- `statuses` (object)

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v5/product/info/prices`

**Get product price information**

Operation ID: `ProductAPI_GetProductInfoPrices`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `offer_id` (object) — Filter by the `offer_id` parameter. You can pass a list of up to 1000 values.
  - `product_id` (object) — Filter by the `product_id` parameter. You can pass a list of up to 1000 values.
  - `visibility` (enum) — Values: `ALL, VISIBLE, INVISIBLE, EMPTY_STOCK, NOT_MODERATED, MODERATED, DISABLED, STATE_FAILED`
- `limit` (integer(int32)) **(required)** — Number of values per page.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `items` (object) — Product list.
- `total` (integer(int32)) — Products number in the list.

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

### `POST /v1/product/info/discounted`

**Get information about the markdown and the main product by the markdown product SKU**

Operation ID: `ProductAPI_GetProductInfoDiscounted`

A method for getting information about the condition and defects of a markdown product by its SKU. Works only with discounted products under the FBO scheme. The method also returns the SKU of the main product.

**Request body:**

- `discounted_skus` (object) **(required)** — Markdown products SKUs list.

**Response 200:**

- `items` (object) — Information about the markdown and the main product.

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

### `POST /v1/product/update/discount`

**Set a discount on a markdown product**

Operation ID: `ProductAPI_ProductUpdateDiscount`

A method for setting the discount percentage on markdown products sold under the FBS scheme.

**Request body:**

- `discount` (integer(int32)) **(required)** — Discount amount: from 3 to 99 percents.
- `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, `product_id`.

**Response 200:**

- `result` (boolean) — Method result. `true` if the query was executed without errors.

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

## Promos

To promote your products, participate in special offers that Ozon holds for customers.

Learn more about special offers in [Help Center](https://docs.ozon.ru/global/en/promotion/marketing/promo/).

### `GET /v1/actions`

**Available special offers**

A method for getting a list of Ozon special offers that you can participate in.

[Learn more about Ozon special offers](https://docs.ozon.ru/global/en/promotion/big-promotions/rasprodazha/)

**Response 200:**

- `result` (array[object]) — Method result.
  - `id` (number(double)) — Special offer identifier.
  - `title` (string) — Special offer name.
  - `action_type` (string) — Special offer type.
  - `description` (string) — Special offer description.
  - `date_start` (string) — Special offer start date.
  - `date_end` (string) — Special offer end date.
  - `freeze_date` (string) — Special offer freeze date.  If the field is filled, the seller can't increase prices, change the list of products, or decrease the number of product u
  - `potential_products_count` (number(double)) — Number of products that can participate in the special offer.
  - `participating_products_count` (number(double)) — Number of products that participate in the special offer.
  - `is_participating` (boolean) — Whether or not you participate in the special offer.
  - `is_voucher_action` (boolean) — Indication that customers need a promo code to participate in the special offer.
  - `banned_products_count` (number(double)) — Number of blocked products.
  - `with_targeting` (boolean) — Indication of the special offer is with the target audience.
  - `order_amount` (number(double)) — Order amount.
  - `discount_type` (string) — Discount type.
  - `discount_value` (number(double)) — Discount size.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/candidates`

**Products that can participate in a special offer**

Operation ID: `PromosCandidates`

A method for getting a list of products that can participate in the special offer by the special offer identifier.

**Request body:**

- `action_id` (number(double)) — Special offer identifier. You can get it using the [/v1/actions](#operation/Promos) method.
- `limit` (number(double)) — Number of values in the response. The default value is 100.
- `last_id` (number(double)) — Identifier of the last value on the page. Leave this field blank in the first request.

**Response 200:**

- `result` (object)
  - `products` (array[object]) — Product list.
  - `total` (number(double)) — Total number of products that can participate in the special offer.
  - `last_id` (number(double)) — Identifier of the last drop-off point on the page. Leave this field blank in the first request.  To get the next values, specify the received value in

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/products`

**Products in a special offer**

Operation ID: `PromosProducts`

A method for getting the list of products participating in the special offer by its identifier.

**Request body:**

- `action_id` (number(double)) — Special offer identifier. You can get it using the [/v1/actions](#operation/Promos) method.
- `limit` (number(double)) — Number of values in the response. The default value is 100.
- `last_id` (number(double)) — Identifier of the last value on the page. Leave this field blank in the first request.

**Response 200:**

- `result` (object)
  - `products` (array[object]) — Product list.
  - `total` (number(double)) — Total number of products that can participate in the special offer.
  - `last_id` (number(double)) — Identifier of the last drop-off point on the page. Leave this field blank in the first request.  To get the next values, specify the received value in

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/products/activate`

**Add products to special offer**

Operation ID: `PromosProductsActivate`

A method for adding products to an available special offer.

**Request body:**

- `action_id` (number(double)) **(required)** — Special offer identifier. You can get it using the [/v1/actions](#operation/Promos) method.
- `products` (array[object]) **(required)** — Product list.
  - `product_id` (number(double)) **(required)** — Product identifier.
  - `action_price` (number(double)) **(required)** — Special offer product price.
  - `stock` (number(double)) — Number of product units in a stock discount type special offer.

**Response 200:**

- `result` (object)
  - `product_ids` (array[number]) — List of product identifiers that were added to the special offer.
  - `rejected` (array[object]) — List of products that weren't added to the special offer.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/products/deactivate`

**Remove products from special offer**

Operation ID: `PromosProductsDeactivate`

A method for removing products from the special offer.

**Request body:**

- `action_id` (number(double)) **(required)** — Special offer identifier. You can get it using the [/v1/actions](#operation/Promos) method.
- `product_ids` (array[number]) **(required)** — List of products identifiers.

**Response 200:**

- `result` (object)
  - `product_ids` (array[number]) — List of product identifiers that were removed from the special offer.
  - `rejected` (array[object]) — List of product identifiers that weren't removed from the special offer.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/discounts-task/list`

**List of discount requests**

Operation ID: `promos_task_list`

> **Note:** 
 Method is deprecated and will be disabled. Switch to the new version [/v2/actions/discounts-task/list](#operation/GetDiscountTaskListV2).

 

 Method for getting a list of products that customers want to buy with discount.

**Request body:**

- `status` (enum) **(required)** — Values: `NEW, SEEN, APPROVED, PARTLY_APPROVED, DECLINED, AUTO_DECLINED, DECLINED_BY_USER, COUPON`
- `page` (integer(uint64)) **(required)** — Page number from which you want to download the list of discount requests.
- `limit` (integer(uint64)) **(required)** — The maximum number of requests on a page.

**Response 200:**

- `result` (array[object]) — List of requests.
  - `id` (integer(uint64)) — Request ID.
  - `created_at` (string(date-time)) — Request created date.
  - `end_at` (string(date-time)) — End time of the request.
  - `edited_till` (string(date-time)) — Time to change the decision.
  - `status` (string) — Request status.
  - `customer_name` (string) — Customer's name.
  - `sku` (integer(uint64)) — Product identifier in the Ozon system, SKU.
  - `user_comment` (string) — Customer's comment on the request.
  - `seller_comment` (string) — Seller's comment on the request.
  - `requested_price` (number(double)) — Requested price.
  - `approved_price` (number(double)) — Approved price.
  - `original_price` (number(double)) — Product price before all discounts.
  - `discount` (number(double)) — Discount in rubles.
  - `discount_percent` (number(double)) — Discount percentage.
  - `base_price` (number(double)) — Base price at which a product is selling on Ozon, if not eligible for a special offer.
  - `min_auto_price` (number(double)) — The minimum price after auto-application of discounts and special offers.
  - `prev_task_id` (integer(uint64)) — ID of the previous customer request for this product.
  - `is_damaged` (boolean) — If product is damaged — `true`.
  - `moderated_at` (string(date-time)) — Moderation date: review, approval or decline of the request.
  - `approved_discount` (number(double)) — Discount in rubles approved by the seller. Pass the value `0` if the seller did not approve the request.
  - `approved_discount_percent` (number(double)) — Discount percentage approved by the seller. Pass the value `0` if the seller did not approve the request.
  - `is_purchased` (boolean) — Whether the customer has purchased the product. `true` if purchased.
  - `is_auto_moderated` (boolean) — Whether the request was moderated automatically. `true` if moderation was automatic.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `email` (string) — Email of the user who processed the request.
  - `last_name` (string) — Last name of the user who processed the request.
  - `first_name` (string) — First name of the user who processed the request.
  - `patronymic` (string) — Patronymic of the user who processed the request.
  - `approved_quantity_min` (integer(uint64)) — Approved minimum quantity of products.
  - `approved_quantity_max` (integer(uint64)) — Approved maximum quantity of products.
  - `requested_quantity_min` (integer(uint64)) — Requested minimum number of products.
  - `requested_quantity_max` (integer(uint64)) — Requested maximum number of products.
  - `requested_price_with_fee` (number(double)) — Requested price with fee.
  - `approved_price_with_fee` (number(double)) — Approved price with fee.
  - `approved_price_fee_percent` (number(double)) — Approved price fee percent.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/discounts-task/approve`

**Approve a discount request**

Operation ID: `promos_task_approve`

You can approve applications in statuses:
- `NEW`—new,
- `SEEN`—viewed.

**Request body:**

- `tasks` (array[object]) **(required)** — List of discount requests.
  - `id` (integer(uint64)) **(required)** — Request ID. You can get it using the [/v1/actions/discounts-task/list](#operation/promos_task_list) method.
  - `approved_price` (number(double)) **(required)** — Approved price.
  - `seller_comment` (string) — Seller's comment on the request.
  - `approved_quantity_min` (integer(uint64)) **(required)** — Approved minimum quantity of products.
  - `approved_quantity_max` (integer(uint64)) **(required)** — Approved maximum quantity of products.

**Response 200:**

- `result` (object)
  - `fail_details` (array[object]) — Errors when creating a request.
  - `success_count` (integer(int32)) — The number of requests with a successful status change.
  - `fail_count` (integer(int32)) — The number of requests that failed to change their status.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/actions/discounts-task/decline`

**Decline a discount request**

Operation ID: `promos_task_decline`

You can decline applications in statuses:
- `NEW`—new,
- `SEEN`—viewed.

**Request body:**

- `tasks` (array[object]) **(required)** — List of discount requests.
  - `id` (integer(uint64)) **(required)** — Request ID.
  - `seller_comment` (string) — Seller's comment on the request.

**Response 200:**

- `result` (object)
  - `fail_details` (array[object]) — Errors when creating a request.
  - `success_count` (integer(int32)) — The number of requests with a successful status change.
  - `fail_count` (integer(int32)) — The number of requests that failed to change their status.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## PricingStrategyAPI

### `POST /v1/pricing-strategy/competitors/list`

**List of competitors**

Operation ID: `pricing_competitors`

Method for getting a list of competitors—sellers with similar products in other online stores and marketplaces.

**Request body:**

- `page` (integer(int64)) **(required)** — Page number from which you want to download the list of competitors. The minimum value is `1`.
- `limit` (integer(int64)) **(required)** — Maximum number of competitors on the page. Allowed values: 1–50.

**Response 200:**

- `competitor` (array[object]) — List of competitors.
  - `name` (string) — Competitor's name.
  - `id` (integer(int64)) — Competitor identifier.
- `total` (integer(int32)) — Total number of competitors.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/list`

**List of strategies**

Operation ID: `pricing_list`

**Request body:**

- `page` (integer(int64)) **(required)** — Page number from which you want to download the list of competitors. The minimum value is `1`.
- `limit` (integer(int64)) **(required)** — Maximum number of competitors on the page. Allowed values: 1–50.

**Response 200:**

- `strategies` (array[object]) — List of strategies.
  - `id` (string) — Strategy identifier.
  - `name` (string) — Strategy name.
  - `type` (string) — Strategy type: - `MIN_EXT_PRICE`—system strategy, - `COMP_PRICE`—user strategy.
  - `update_type` (string) — Type of the last strategy change: - `strategyEnabled`—resumed, - `strategyDisabled`—stopped, - `strategyChanged`—updated, - `strategyCreated`—created,
  - `updated_at` (string) — Date of last change.
  - `products_count` (integer(int64)) — Number of products in the strategy.
  - `competitors_count` (integer(int64)) — Number of selected competitors.
  - `enabled` (boolean) — Strategy status: - `true`—enabled, - `false`—disabled.
- `total` (integer(int32)) — Total number of strategies.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/create`

**Create a pricing strategy**

Operation ID: `pricing_create`

**Request body:**

- `competitors` (array[object]) **(required)** — List of competitors.
  - `coefficient` (number(float)) **(required)** — Coefficient by which the minimum price among competitors will be multiplied. The allowed range is from `0.5` to `1.2`.
  - `competitor_id` (integer(int64)) **(required)** — Competitor identifier.
- `strategy_name` (string) **(required)** — Strategy name.

**Response 200:**

- `result` (object)
  - `strategy_id` (string) — Strategy identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/info`

**Strategy info**

Operation ID: `pricing_info`

**Request body:**

- `strategy_id` (string) **(required)** — Strategy identifier.

**Response 200:**

- `result` (object)
  - `competitors` (array[object]) — List of competitors.
  - `enabled` (boolean) — Strategy status: - `true`—enabled, - `false`—disabled.
  - `name` (string) — Strategy name.
  - `type` (string) — Strategy type: - `MIN_EXT_PRICE`—system strategy, - `COMP_PRICE`—user strategy.
  - `update_type` (string) — Type of the last strategy change: - `strategyEnabled`—resumed, - `strategyDisabled`—stopped, - `strategyChanged`—updated, - `strategyCreated`—created,

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/update`

**Update strategy**

Operation ID: `pricing_update`

You can update all strategies except the system one.

**Request body:**

- `competitors` (array[object]) **(required)** — List of competitors.
  - `coefficient` (number(float)) **(required)** — Coefficient by which the minimum price among competitors will be multiplied. The allowed range is from `0.5` to `1.2`.
  - `competitor_id` (integer(int64)) **(required)** — Competitor identifier.
- `strategy_id` (string) **(required)** — Product identifier.
- `strategy_name` (string) **(required)** — Strategy name.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/products/add`

**Bind products to a strategy**

Operation ID: `pricing_items-add`

**Request body:**

- `product_id` (array[string]) **(required)** — List of product identifiers. The maximum number is 50.
- `strategy_id` (string) **(required)** — Product identifier.

**Response 200:**

- `result` (object)
  - `errors` (array[object]) — Products with errors.
  - `failed_product_count` (integer(int32)) — Number of products with errors.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/strategy-ids-by-product-ids`

**List of strategy identifiers**

Operation ID: `pricing_ids`

**Request body:**

- `product_id` (array[string]) **(required)** — List of product identifiers. The maximum number is 50.

**Response 200:**

- `result` (object)
  - `products_info` (array[object]) — Product information.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/products/list`

**List of products in a strategy**

Operation ID: `pricing_items-list`

**Request body:**

- `strategy_id` (string) **(required)** — Strategy identifier.

**Response 200:**

- `result` (object)
  - `product_id` (array[string]) — Product identifier in the Ozon system, `product_id`.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/product/info`

**Competitor's product price**

Operation ID: `pricing_items-info`

If you add a product to your pricing strategy, the method returns you the price and a link to the competitor's product.

**Request body:**

- `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, `product_id`.

**Response 200:**

- `result` (object)
  - `strategy_id` (string) — Product identifier.
  - `is_enabled` (boolean) — `true` if the product is in the pricing strategy.
  - `strategy_product_price` (integer(int32)) — Price of product in the strategy.
  - `price_downloaded_at` (string) — Price setting date.
  - `strategy_competitor_id` (integer(int64)) — Competitor identifier.
  - `strategy_competitor_product_url` (string) — Link to a competitor's product.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/products/delete`

**Remove products from a strategy**

Operation ID: `pricing_items-delete`

**Request body:**

- `product_id` (array[string]) **(required)** — List of product identifiers. The maximum number is 50.

**Response 200:**

- `result` (object)
  - `failed_product_count` (integer(int32)) — Number of products with errors.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/status`

**Change strategy status**

Operation ID: `pricing_status`

You can change the status of any strategy except the system one.

**Request body:**

- `enabled` (boolean) — Strategy status: - `true`—enabled, - `false`—disabled.
- `strategy_id` (string) **(required)** — Product identifier.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/pricing-strategy/delete`

**Delete a pricing strategy**

Operation ID: `pricing_delete`

You can delete any strategy except the system one.

**Request body:**

- `strategy_id` (string) **(required)** — Strategy identifier.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## BrandAPI

### `POST /v1/brand/company-certification/list`

**List of certified brands**

Operation ID: `BrandAPI_BrandCompanyCertificationList`

**Request body:**

- `page` (integer(int32)) **(required)** — Number of the page returned in the request.
- `page_size` (integer(int32)) **(required)** — Number of elements on the page.

**Response 200:**

- `result` (object)
  - `brand_certification` (array[object]) — Certified brands details.
  - `total` (integer(int64)) — Total number of brands.

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

## CertificationAPI

### `GET /v1/product/certificate/accordance-types`

**List of accordance types (version 1)**

Operation ID: `ProductAPI_ProductCertificateAccordanceTypes`

**Response 200:**

- `result` (array[object]) — Certificate types and names list.
  - `name` (string) — Certificate name.
  - `value` (string) — Certificate type.

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

### `GET /v2/product/certificate/accordance-types/list`

**List of accordance types (version 2)**

Operation ID: `CertificateAccordanceTypes`

**Response 200:**

- `result` (object)
  - `base` (array[object]) — Main accordance types.
  - `hazard` (array[object]) — Main accordance types related to dangerous products.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `GET /v1/product/certificate/types`

**Directory of document types**

Operation ID: `ProductAPI_ProductCertificateTypes`

**Response 200:**

- `result` (array[object]) — List of certificate types and names.
  - `name` (string) — Certificate name.
  - `value` (string) — Certificate type.

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

### `POST /v2/product/certification/list`

**List of certified categories**

Operation ID: `ProductAPI_ProductCertificationList`

**Request body:**

- `page` (integer(int64)) **(required)** — Page number.
- `page_size` (integer(int64)) **(required)** — Number of elements on the page.

**Response 200:**

- `certification` (array[object]) — Certified categories details.
  - `category_id` (integer(int64)) — Identifier of the certified category.
  - `category_name` (string) — Name of certified category.
  - `is_required` (boolean) — Indication of a mandatory category.
  - `type_id` (integer(int64)) — Type identifier of the certified category.
  - `type_name` (string) — Name of the type of certified category.
- `total` (integer(int64)) — Total number of categories.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certification/list`

**List of certified categories**

Operation ID: `ProductAPI_V1ProductCertificationList`

> **Note:** 
  From April 14, 2025, method will be disabled. Switch to the [/v2/product/certification/list](#operation/ProductAPI_ProductCertificationList) method.

**Request body:**

- `page` (integer(int32)) — Number of the page returned in the query.
- `page_size` (integer(int32)) — Number of elements on the page.

**Response 200:**

- `result` (object)
  - `certification` (array[object]) — Certified categories details.
  - `total` (integer(int64)) — Total number of categories.

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

### `POST /v1/product/certificate/create`

**Adding certificates for products**

Operation ID: `ProductAPI_ProductCertificateCreate`

**Response 200:**

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

### `POST /v1/product/certificate/bind`

**Link the certificate to the product**

Operation ID: `ProductAPI_ProductCertificateBind`

**Request body:**

- `certificate_id` (integer(int64)) **(required)** — Certificate identifier that was assigned when it was uploaded.
- `product_id` (array[integer]) **(required)** — An array of product identifiers that this certificate applies to.

**Response 200:**

- `result` (boolean) — The result of processing the request. `true` if the request was executed without errors.

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

### `POST /v1/product/certificate/delete`

**Delete certificate**

Operation ID: `CertificateDelete`

**Request body:**

- `certificate_id` (integer(int32)) **(required)** — Certificate identifier.

**Response 200:**

- `result` (object)
  - `is_delete` (boolean) — Indication that a certificate has been deleted: - `true`—deleted, - `false`—not deleted.
  - `error_message` (string) — Description of errors during certificate deletion.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/info`

**Certificate information**

Operation ID: `CertificateInfo`

**Request body:**

- `certificate_number` (string) **(required)** — Certificate identifier.

**Response 200:**

- `result` (object)
  - `certificate_id` (integer(int32)) — Identifier.
  - `certificate_number` (string) — Number.
  - `certificate_name` (string) — Name.
  - `type_code` (string) — Type.
  - `status_code` (string) — Status.
  - `accordance_type_code` (string) — Accordance type.
  - `rejection_reason_code` (string) — Certificate rejection reason.
  - `verification_comment` (string) — Moderator's comment.
  - `issue_date` (string(date-time)) — Issue date.
  - `expire_date` (string(date-time)) — Expire date.
  - `products_count` (integer(int32)) — Number of products associated with a certificate.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/list`

**Certificates list**

Operation ID: `CertificateList`

**Request body:**

- `offer_id` (string) — Product identifier associated with the certificate. Pass the parameter if you need certificates that certain products are associated with.
- `status` (string) — Certificate status. Pass the parameter if you need certificates with a certain status.
- `type` (string) — Certificate type. Pass the parameter if you need certificates with a certain type.
- `page` (integer(int32)) **(required)** — Page from which the list should be displayed. The minimum value is 1.
- `page_size` (integer(int32)) **(required)** — Number of objects on the page. The value is from 1 to 1000.

**Response 200:**

- `result` (object)
  - `certificates` (array[object]) — Сertificate information.
  - `page_count` (integer(int32)) — Number of pages.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/product_status/list`

**Product statuses list**

Operation ID: `ProductStatusList`

A method for getting a list of possible statuses of products when binding them to a certificate.

**Response 200:**

- `result` (array[object]) — Product statuses.
  - `code` (string) — Product status code when linking it to the certificate.
  - `name` (string) — Status description.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/products/list`

**List of products associated with the certificate**

Operation ID: `CertificateProductsList`

**Request body:**

- `certificate_id` (integer(int32)) **(required)** — Certificate identifier.
- `product_status_code` (string) — Status of the product verification when binding to a certificate.
- `page` (integer(int32)) **(required)** — Page from which the list should be displayed. The minimum value is 1.
- `page_size` (integer(int32)) **(required)** — Number of objects on the page. The value is from 1 to 1000.

**Response 200:**

- `result` (object)
  - `items` (array[object]) — List of products.
  - `count` (integer(int64)) — Number of products found.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/unbind`

**Unbind products from a certificate**

Operation ID: `CertificateUnbind`

**Request body:**

- `certificate_id` (integer(int32)) **(required)** — Certificate identifier.
- `product_id` (array[string]) **(required)** — List of product identifiers that you want to unbind from a certificate.

**Response 200:**

- `result` (array[object]) — Method result.
  - `error` (string) — Error message when unbinding a product.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `updated` (boolean) — Indication that the product was unbound from a certificate: - `true`—it was unbound, - `false`—it is still bound.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/rejection_reasons/list`

**Possible certificate rejection reasons**

Operation ID: `RejectionReasonsList`

**Response 200:**

- `result` (array[object]) — Certificate rejection reasons.
  - `code` (string) — Сode of a certificate rejection reason.
  - `name` (string) — Description of a certificate rejection reason.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/certificate/status/list`

**Possible certificate statuses**

Operation ID: `CertificateStatusList`

**Response 200:**

- `result` (array[object]) — Possible certificate statuses.
  - `code` (string) — Certificate status code.
  - `name` (string) — Status description.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## WarehouseAPI

### `POST /v1/warehouse/list`

**List of warehouses**

Operation ID: `WarehouseAPI_WarehouseList`

> **Note:** 
  Method is deprecated and will be disabled on April 7, 2026. Switch to the new version [/v2/warehouse/list](#operation/WarehouseListV2).

Returns the list of FBS and rFBS warehouses. To get the list of FBO warehouses, use the [/v1/cluster/list](#operation/SupplyDraftAPI_DraftClusterList) method.

You can use the method once per minute.

**Request body:**

- `limit` (integer(int64)) **(required)** — Number of values in the response.
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.

**Response 200:**

- `result` (array[object]) — Warehouses list.
  - `has_entrusted_acceptance` (boolean) — Trusted acceptance attribute. `true` if trusted acceptance is enabled in the warehouse.
  - `is_rfbs` (boolean) — Indication that the warehouse works under the rFBS scheme: - `true`—the warehouse works under the rFBS scheme; - `false`—the warehouse doesn't work un
  - `name` (string) — Warehouse name.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
  - `can_print_act_in_advance` (boolean) — Possibility to print an acceptance certificate in advance. `true` if printing in advance is possible.
  - `first_mile_type` (object)
  - `has_postings_limit` (boolean) — Indication if there is a limit on the minimum number of orders. `true` if there is such a limit.
  - `is_karantin` (boolean) — Indication that the warehouse is not working due to quarantine.
  - `is_kgt` (boolean) — Indication that the warehouse accepts bulky products.
  - `is_economy` (boolean) — `true` if the warehouse handles economy products.
  - `is_timetable_editable` (boolean) — Indication that warehouse schedule can be changed.
  - `min_postings_limit` (integer(int32)) — Minimum limit value: the number of orders that can be brought in one shipment.
  - `postings_limit` (integer(int32)) — Limit value. `-1` if there is no limit.
  - `min_working_days` (integer(int64)) — Number of warehouse working days.
  - `status` (string) — Warehouse status.  How warehouse statuses match with the statuses in the personal account:  | Seller&nbsp;API status | Status in personal account | |-
  - `working_days` (object) — Warehouse working days.

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

### `POST /v1/delivery-method/list`

**List of delivery methods for a warehouse**

Operation ID: `WarehouseAPI_DeliveryMethodList`

> **Note:** 
  Method is deprecated and will be disabled on April 7, 2026. Switch to the [/v2/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodListV2) method.

**Request body:**

- `filter` (object)
  - `provider_id` (integer(int64)) — Delivery service identifier.
  - `status` (string) — Delivery method status: - `NEW`—created, - `EDITED`—being edited, - `ACTIVE`—active, - `DISABLED`—inactive.
  - `warehouse_id` (integer(int64)) — Warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
- `limit` (integer(int64)) **(required)** — Number of items in a response. Maximum is 50, minimum is 1.
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.

**Response 200:**

- `has_next` (boolean) — Indication that only part of delivery methods was returned in the response: - `true`—make a request with a new `offset` parameter value for getting th
- `result` (array[object]) — Method result.
  - `company_id` (integer(int64)) — Company identifier.
  - `created_at` (string(date-time)) — Date and time of delivery method creation.
  - `cutoff` (string) — Time before an order must be packed.
  - `id` (integer(int64)) — Delivery method identifier.
  - `name` (string) — Delivery method name.
  - `provider_id` (integer(int64)) — Delivery service identifier.
  - `sla_cut_in` (integer(int64)) — Minimum time to package an order in minutes according to warehouse settings.
  - `status` (string) — Delivery method status:   - `NEW`—created,   - `EDITED`—being edited,   - `ACTIVE`—active,   - `DISABLED`—inactive.
  - `template_id` (integer(int64)) — Order delivery service identifier.
  - `updated_at` (string(date-time)) — Date and time when the delivery method was last updated.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.

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

### `POST /v2/delivery-method/list`

**List of delivery methods for realFBS warehouses**

Operation ID: `WarehouseAPI_DeliveryMethodListV2`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `delivery_method_ids` (array[string]) — Delivery method identifiers.
  - `provider_ids` (array[string]) — Delivery service identifiers.
  - `status` (array[object]) — Delivery method status: - `NEW`: created, - `EDITED`: being edited, - `ACTIVE`: active, - `DISABLED`: inactive, - `WAITING`: under review, - `BROKEN`:
  - `warehouse_ids` (array[string]) — Warehouse identifiers. Get them using the [/v2/warehouse/list](#operation/WarehouseListV2) method.
- `limit` (integer(int64)) **(required)** — Number of values in the response.
- `sort_dir` (enum) — Values: `ASC, DESC`

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `has_next` (boolean) — `true` if not all delivery methods are returned in the response.
- `delivery_methods` (array[object]) — Delivery methods.
  - `created_at` (string(date-time)) — Date and time of delivery method creation.
  - `cutoff` (string) — Time before an order must be packed.
  - `id` (integer(int64)) — Delivery method identifier.
  - `is_express` (boolean) — `true` if Ozon Express fast delivery is available.
  - `name` (string) — Delivery method name.
  - `provider_id` (integer(int64)) — Delivery service identifier.
  - `sla_cut_in` (integer(int64)) — Minimum time to pack an order in minutes according to warehouse settings.
  - `status` (string) — Delivery method status: - `NEW`: created, - `EDITED`: being edited, - `ACTIVE`: active, - `DISABLED`: inactive, - `WAITING`: under review, - `BROKEN`:
  - `template_id` (integer(int64)) — Identifier of the order delivery service.
  - `tpl_integration_type` (string) — Type of integration with the delivery service: - `aggregator`: delivery by a third-party service, Ozon registers the order; - `3pl_tracking`: delivery
  - `updated_at` (string(date-time)) — Date and time when the delivery method was last updated.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.

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

### `POST /v2/warehouse/list`

**List of warehouses**

Operation ID: `WarehouseListV2`

Method returns a list of FBS and rFBS warehouses. To get a list of FBO warehouses, use the [/v1/warehouse/fbo/list](#operation/SupplyDraftAPI_DraftGetWarehouseFboList) method.

**Request body:**

- `limit` (integer) **(required)** — Number of values in the response.
- `cursor` (string) — Cursor for the next data sample.
- `warehouse_ids` (array[string]) — Warehouse identifiers.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `warehouses` (array[object]) — List of warehouses.
  - `address_info` (object)
  - `carriage_label_type` (enum) — Values: `UNSPECIFIED, BIG, SMALL`
  - `courier_comment` (string) — Comment for courier.
  - `courier_phones` (array[string]) — Phone numbers to contact courier.
  - `created_at` (string(date-time)) — Warehouse creation date and time.
  - `cut_in_time` (integer(int64)) — Order shipment time in minutes.
  - `first_mile` (object)
  - `has_entrusted_acceptance` (boolean) — Indicates if entrusted acceptance is enabled.
  - `has_postings_limit` (boolean) — Indicates if there is a limit on the minimum number of orders. `true` if a limit exists.
  - `is_auto_assembly` (boolean) — Indicates if auto-assembly is enabled.
  - `is_comfort` (boolean) — `true` if comfort delivery is enabled. Delivery time to the customer is more than 60 minutes.
  - `is_express` (boolean) — `true` if express delivery is enabled. Delivery time to the customer is less than 60 minutes.
  - `is_kgt` (boolean) — Indicates if the warehouse accepts bulky products.
  - `is_rfbs` (boolean) — Indicates if the warehouse works under the rFBS scheme.
  - `is_waybill_enabled` (boolean) — Indicates if waybill printing is enabled.
  - `min_postings_limit` (integer(int32)) — Minimum number of orders that can be delivered in one shipment.
  - `name` (string) — Warehouse name.
  - `phone` (string) — Warehouse phone number.
  - `postings_limit` (integer(int32)) — Limit of orders. `-1` if there is no limit.
  - `sla_cut_in` (integer(int64)) — Minimum order assembly time in minutes.
  - `status` (string) — Warehouse status.
  - `timetable` (object)
  - `updated_at` (string(date-time)) — Date and time of the last warehouse information update.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
  - `warehouse_type` (string) — Warehouse type.
  - `with_item_list` (boolean) — Indicates if product list printing is enabled.
  - `working_days` (array[object]) — Warehouse working days.
- `has_next` (boolean) — `true` if not all values are returned in the response.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/operation/status`

**Get operation status**

Operation ID: `GetWarehouseFBSOperationStatus`

**Request body:**

- `operation_id` (string) **(required)** — Operation identifier.

**Response 200:**

- `error` (object)
  - `code` (string) — Error code.
  - `message` (string) — Error description.
- `result` (object)
  - `entity_id` (integer(int64)) — Processed entity identifier. If operation is `CREATE_FBS_WAREHOUSE`, warehouse identifier is returned.
- `status` (enum) — Values: `UNSPECIFIED, IN_PROGRESS, SUCCESS, ERROR`
- `type` (enum) — Values: `UNSPECIFIED, CREATE_FBS_WAREHOUSE, UPDATE_FBS_WAREHOUSE, SET_FIRST_MILE, WAREHOUSE_ENABLE_DISABLE`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/archive`

**Archive a warehouse**

Operation ID: `ArchiveWarehouseFBS`

**Request body:**

- `reason` (string) **(required)** — Archiving reason.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `operation_id` (string) — Operation identifier. Get the operation status using the [/v1/warehouse/operation/status](#operation/GetWarehouseFBSOperationStatus) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/unarchive`

**Remove warehouse from archive**

Operation ID: `UnarchiveWarehouseFBS`

**Request body:**

- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `operation_id` (string) — Operation identifier. Get the operation status using the [/v1/warehouse/operation/status](#operation/GetWarehouseFBSOperationStatus) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/invalid-products/get`

**Get list of products with FBS delivery restrictions.**

Operation ID: `WarehouseInvalidProductsGet`

**Request body:**

- `last_id` (integer(int64)) — Identifier of the last value on the page. Leave this field blank in the first request.  To get the next values, specify the `last_id` from the previou
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier. Get the parameter value using the [/v1/warehouse/warehouses-with-invalid-products](#operation/WarehouseWithInvalidProducts) meth

**Response 200:**

- `has_next` (boolean) — `true` if not all products are returned in the response.
- `last_id` (integer(int64)) — Identifier of the last value on the page. To get the next values, specify the received value in the next request in the `last_id` parameter.
- `validation_results` (array[object]) — Result of verification.
  - `item` (object)
  - `state` (enum) — Values: `UNSPECIFIED, NOT_VALID`
  - `validation_errors` (array[object]) — Error details.
- `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/warehouses-with-invalid-products`

**Get list of warehouses with products restricted for delivery**

Operation ID: `WarehouseWithInvalidProducts`

Returns the identifiers of warehouses that have restricted products. Such products aren't available for delivery from the warehouse.

**Response 200:**

- `warehouse_ids` (array[string]) — List of warehouses identifiers. There should be at least 1 product at the warehouse that isn't available for delivery from it. To get a list of restri

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## FBSWarehouseSetup

### `POST /v1/warehouse/fbs/create/drop-off/list`

**Get a list of drop-off points to create a warehouse**

Operation ID: `WarehouseAPI_ListDropOffPointsForCreateFBSWarehouse`

**Request body:**

- `coordinates` (object)
  - `latitude` (number(double)) **(required)** — Latitude.
  - `longitude` (number(double)) **(required)** — Longitude.
- `country_code` (string) **(required)** — Country code in the ISO 2 format.
- `is_kgt` (boolean) **(required)** — `true` if the product is bulky.
- `search` (object)
  - `address` (string) — Address of the drop-off point.
  - `types` (array[object]) — Type of drop-off point: - `PVZ`: order pick-up point, - `PPZ': order acceptance point, - 'SC`: sorting center.

**Response 200:**

- `points` (array[object]) — Drop-off points list.
  - `address` (string) — Address of the drop-off point.
  - `coordinates` (object)
  - `discount_percent` (number(float)) — Discount percentage for dropping off the shipment.
  - `id` (string) — Drop-off point identifier.
  - `last_transit_time_local` (object)
  - `type` (enum) — Values: `PVZ, PPZ, SC`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/update/drop-off/list`

**Get a list of drop-off points for changing warehouse details**

Operation ID: `WarehouseAPI_ListDropOffPointsForUpdateFBSWarehouse`

**Request body:**

- `search` (object)
  - `address` (string) — Search by drop-off point address.
  - `types` (array[object]) — Drop-off point type: - `PVZ`: order pick-up point, - `PPZ`: order acceptance point, - `SC`: sorting center.
- `warehouse_id` (integer(int64)) **(required)** — Filter by existing FBS warehouse.

**Response 200:**

- `points` (array[object]) — List of drop-off points.
  - `address` (string) — Address of the drop-off point.
  - `coordinates` (object)
  - `discount_percent` (number(float)) — Discount percentage for dropping off the shipment.
  - `id` (string) — Drop-off point identifier.
  - `last_transit_time_local` (object)
  - `type` (enum) — Values: `PVZ, PPZ, SC`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/create/drop-off/timeslot/list`

**Get list of time slots for creating warehouse with drop-off shipment**

Operation ID: `WarehouseFbsCreateDropOffTimeslotList`

**Request body:**

- `drop_off_point_id` (integer(int64)) **(required)** — Drop-off point identifier.

**Response 200:**

- `timeslots` (array[object]) — List of time slots.
  - `acceptance_end_time_local` (string) — Order acceptance end, local time.
  - `acceptance_start_time_local` (string) — Order acceptance start, local time.
  - `from` (string) — Time slot start time.
  - `id` (integer(int64)) — Time slot identifier.
  - `to` (string) — Time slot end time.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/update/drop-off/timeslot/list`

**Get list of time slots for updating warehouse with drop-off shipment**

Operation ID: `WarehouseFbsUpdateDropOffTimeslotList`

**Request body:**

- `drop_off_point_id` (integer(int64)) **(required)** — Drop-off point identifier.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `timeslots` (array[object]) — List of time slots.
  - `acceptance_end_time_local` (string) — Order acceptance end, local time.
  - `acceptance_start_time_local` (string) — Order acceptance start, local time.
  - `from` (string) — Time slot start time.
  - `id` (integer(int64)) — Time slot identifier.
  - `to` (string) — Time slot end time.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/create/pick-up/timeslot/list`

**Get list of time slots for creating warehouse with pick-up shipment**

Operation ID: `WarehouseFbsCreatePickUpTimeslotList`

**Request body:**

- `address_coordinates` (object) **(required)**
  - `latitude` (number(double)) **(required)** — Latitude.
  - `longitude` (number(double)) **(required)** — Longitude.
- `is_kgt` (boolean) **(required)** — `true` if the product is bulky.

**Response 200:**

- `is_pickup_supported` (boolean) — `true` if pick-up shipment is supported.
- `timeslots` (array[object]) — List of time slots.
  - `from` (string) — Time slot start time.
  - `id` (integer(int64)) — Time slot identifier.
  - `to` (string) — Time slot end time.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/update/pick-up/timeslot/list`

**Get list of time slots for updating warehouse with pick-up shipment**

Operation ID: `WarehouseFbsUpdatePickUpTimeslotList`

**Request body:**

- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `timeslots` (array[object]) — List of time slots.
  - `from` (string) — Time slot start time.
  - `id` (integer(int64)) — Time slot identifier.
  - `to` (string) — Time slot end time.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/create`

**Create a warehouse**

Operation ID: `WarehouseAPI_CreateWarehouseFBS`

If you create a warehouse with delivery to a drop-off point, use the [/v1/warehouse/fbs/create/drop-off/list](#operation/WarehouseAPI_ListDropOffPointsForCreateFBSWarehouse) method to get a list of points.

**Request body:**

- `address_coordinates` (object) **(required)**
  - `latitude` (number(double)) **(required)** — Latitude.
  - `longitude` (number(double)) **(required)** — Longitude.
- `cut_in_time` (integer(int64)) **(required)** — Order acceptance time in minutes. For example, if you pass `3000`, order acceptance finishes in 50 hours from the moment of submission.
- `drop_off_point_id` (integer(int64)) — Drop-off point identifier.
- `first_mile_type` (enum) **(required)** — Values: `PICK_UP, DROP_OFF`
- `is_kgt` (boolean) **(required)** — `true` if the product is bulky.
- `name` (string) **(required)** — Warehouse name.
- `options` (object)
  - `comment` (string) — Comment for the courier when shipping with the `PICK_UP` type.
  - `courier_phones` (array[string]) — Phone numbers for the courier during shipping with the `PICK_UP' type. Specify in the +7(XXX)XXX-XX-XX format.
  - `is_auto_assembly` (boolean) — `true` if auto-packaging is enabled.
  - `is_waybill_enabled` (boolean) — `true` if the waybill printing is enabled.
- `phone` (string) **(required)** — Warehouse phone number. Specify in the +7(XXX)XXX-XX-XX format.
- `timeslot_id` (integer(int64)) **(required)** — Time slot identifier.
- `working_days` (array[object]) — Warehouse working days: - `MONDAY`, - `TUESDAY`, - `WEDNESDAY`, - `THURSDAY`, - `FRIDAY`, - `SATURDAY`, - `SUNDAY`.

**Response 200:**

- `operation_id` (string) — Operation identifier for creating an FBS warehouse. To get the operation status, use the [/v1/warehouse/operation/status](#operation/GetWarehouseFBSOp

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/update`

**Update warehouse**

Operation ID: `UpdateWarehouseFBS`

**Request body:**

- `address_coordinates` (object) **(required)**
  - `latitude` (number(double)) **(required)** — Latitude.
  - `longitude` (number(double)) **(required)** — Longitude.
- `name` (string) — Warehouse name.
- `options` (object)
  - `comment` (string) — Comment for courier when shipment type is `PICK_UP`.
  - `courier_phones` (array[string]) — Phone numbers for courier when shipment type is `PICK_UP`.
  - `is_auto_assembly` (boolean) — Indicates if auto-assembly is enabled.
  - `is_waybill_enabled` (boolean) — Indicates if waybill printing is enabled.
- `phone` (string) — Warehouse phone number.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.
- `working_days` (array[object]) — Warehouse working days.

**Response 200:**

- `operation_id` (string) — Operation identifier. You can get the operation status using the [/v1/warehouse/operation/status](#operation/GetWarehouseFBSOperationStatus) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/pickup/courier/create`

**Create courier request for pickup shipments**

Operation ID: `WarehouseFbsPickUpCourierCreate`

The method allows you to schedule a courier pickup for shipments.

[Learn more about courier pickup under the FBS scheme in the Seller Knowledge Base](https://seller-edu.ozon.ru/fbs/ozon-logistika/otgruzka-kyruery)

**Request body:**

- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.  To get a list of warehouses for pickup scheduling, use [/v1/warehouse/fbs/pickup/planning/list](#operation/WarehouseFbsPickUpPl

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/pickup/courier/cancel`

**Cancel courier request for pickup shipments**

Operation ID: `WarehouseFbsPickUpCourierCancel`

The method allows you to cancel a scheduled courier arrival.

[Learn more about courier pickup under the FBS scheme in the Seller Knowledge Base](https://seller-edu.ozon.ru/fbs/ozon-logistika/otgruzka-kyruery)

**Request body:**

- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/first-mile/update`

**Update first mile**

Operation ID: `UpdateWarehouseFBSFirstMile`

**Request body:**

- `cut_in_time` (integer(int64)) **(required)** — Order acceptance time in minutes. For example, if you pass `3000`, order acceptance finishes in 50 hours from the moment of submission.
- `drop_off_point_id` (integer(int64)) — Order drop-off point identifier. Required if `first_mile_type = DROP_OFF`.
- `first_mile_type` (enum) **(required)** — Values: `PICK_UP, DROP_OFF`
- `timeslot_id` (integer(int64)) **(required)** — Time slot identifier.
- `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier.

**Response 200:**

- `operation_id` (string) — Operation identifier. You can get the operation status using the [/v1/warehouse/operation/status](#operation/GetWarehouseFBSOperationStatus) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbs/pickup/history/list`

**Get history of shippings to couriers**

Operation ID: `WarehouseFbsPickUpHistoryList`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `planned_date` (string) — Shipping date.
  - `warehouse_id` (array[string]) — Warehouse identifiers.
  - `was_planned` (boolean) — `true` if shipping is planned.
- `limit` (integer(int64)) **(required)** — Number of values per page.

**Response 200:**

- `result` (object)
  - `cursor` (string) — Cursor for the next data sample.
  - `history` (array[object]) — Shipping history.

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

### `POST /v1/warehouse/fbs/pickup/planning/list`

**Get warehouse list for courier delivery planning**

Operation ID: `WarehouseFbsPickUpPlanningList`

To create a shipment, use the [/v1/warehouse/fbs/pickup/courier/create](#operation/WarehouseFbsPickUpCourierCreate) method.

**Response 200:**

- `result` (object)
  - `warehouses` (array[object]) — Warehouse details.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## FBS

### `POST /v3/posting/fbs/unfulfilled/list`

**List of unprocessed shipments**

Operation ID: `PostingAPI_GetFbsPostingUnfulfilledList`

> **Note:** 
Method is deprecated and disabled on June 1, 2026. Switch to the new version [/v4/posting/fbs/unfulfilled/list](#operation/PostingFbsUnfulfilledList).

Returns a list of unprocessed shipments for the specified time period: it shouldn't be longer than one year.

Possible statuses:
- `awaiting_registration`—awaiting registration,
- `acceptance_in_progress`—acceptance is in progress,
- `awaiting_approve`—awaiting approval,
- `awaiting_packaging`—awaiting packaging,
- `awaiting_deliver`—awaiting shipping,
- `arbitration`—arbitration,
- `client_arbitration`—customer delivery arbitratio...

**Request body:**

- `dir` (string) — Sorting direction:  - `asc`—ascending,  - `desc`—descending.
- `filter` (object) **(required)**
  - `cutoff_from` (string(date-time)) **(required)** — Filter by the time by which the seller should pack the order. Period start.  Format: YYYY-MM-DDThh: mm:ss. mcsZ. Example: 2020-03-18T07:34:50.359 Z.
  - `cutoff_to` (string(date-time)) **(required)** — Filter by the time by which the seller should pack the order. Period end.  Format: YYYY-MM-DDThh: mm:ss. mcsZ. Example: 2020-03-18T07:34:50.359 Z.
  - `delivering_date_from` (string(date-time)) — Minimum date when shipment should be handed over for delivery.
  - `delivering_date_to` (string(date-time)) — Maximum date when shipment should be handed over for delivery.
  - `delivery_method_id` (array[integer]) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `is_quantum` (boolean) — Specify `true` to get only MOQ shipments.  The default value is `false`, the response contains all shipments.
  - `fbpFilter` (string) — Filter for shipments delivered from partner warehouse (FBP). You can pass one of the following values:  - `ALL`—all shipments matching other filters w
  - `provider_id` (array[integer]) — Delivery service identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `status` (string) — Shipment status: - `acceptance_in_progress`—acceptance is in progress, - `awaiting_approve`—awaiting approval, - `awaiting_packaging`—awaiting packagi
  - `warehouse_id` (array[integer]) — Warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
- `limit` (integer(int64)) **(required)** — Number of values in the response:  - maximum—1000,  - minimum—1.
- `offset` (integer(int64)) **(required)** — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `with` (object)
  - `analytics_data` (boolean) — Add analytics data to the response.
  - `barcodes` (boolean) — Add the shipment barcodes to the response.
  - `financial_data` (boolean) — Add financial data to the response.
  - `legal_info` (boolean) — Add legal details to the response.
  - `translit` (boolean) — Transliterate the return values.

**Response 200:**

- `result` (object)
  - `count` (integer(int64)) — Element counter in the response.
  - `postings` (array[object]) — List of shipments and detailed information on each one.

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

### `POST /v4/posting/fbs/unfulfilled/list`

**Get list of unprocessed shipments**

Operation ID: `PostingFbsUnfulfilledList`

Returns a list of shipments for the specified time period. The period shouldn't be longer than one year. 

Possible shipment statuses:
- `awaiting registration`: awaiting registration; 
- `acceptance_in_progress`: acceptance in progress;
- `awaiting_approve`: awaiting approval;
- `awaiting_packaging`: awaiting packaging;
- `awaiting_deliver`: awaiting shipping;
- `arbitration`: arbitration;
- `client_arbitration`: customer delivery arbitration;
- `delivering`: delivery in progress;
- `driver_pickup`: picked up by driver;
- `cancelled`: canceled; 
- `not_accepted`: not accepted at the sorting c...

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `cutoff_from` (string(date-time)) — Time before the order must be packed. Start date.
  - `cutoff_to` (string(date-time)) — Time before the order must be packed. End date.
  - `delivering_date_from` (string(date-time)) — Minimum date when shipment should be handed over for delivery.
  - `delivering_date_to` (string(date-time)) — Maximum date when shipment should be handed over for delivery.
  - `delivery_method_ids` (array[string]) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `last_changed_status_date` (object)
    - `from` (string(date-time)) — Start date of the period.
    - `to` (string(date-time)) — End date of the period.
  - `provider_ids` (array[string]) — Delivery service identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `statuses` (array[string]) — Shipment status:  - `acceptance_in_progress`: acceptance in progress; - `awaiting_approve`: awaiting approval; - `awaiting_packaging`: awaiting packag
  - `warehouse_ids` (array[string]) — Warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
- `limit` (integer(int64)) — Number of values in the response.
- `sort_dir` (enum) — Values: `ASC, DESC`
- `translit` (boolean) — `true` to enable the address transliteration from Cyrillic to Latin.
- `with` (object)
  - `analytics_data` (boolean) — `true` to add analytics data.
  - `barcodes` (boolean) — `true` to add the shipment barcodes to the response.
  - `financial_data` (boolean) — `true` to add financial data.
  - `legal_info` (boolean) — `true` to add legal information.

**Response 200:**

- `count` (integer(int64)) — Number of shipments in the response.
- `cursor` (string) — Cursor for the next data sample.
- `has_next` (boolean) — `true` if the response doesn't contain all shipments.
- `postings` (object) — Shipment list.

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

### `POST /v3/posting/fbs/list`

**Shipments list**

Operation ID: `PostingAPI_GetFbsPostingListV3`

> **Note:** 
Method is deprecated and disabled on June 1, 2026. Switch to the new version [/v4/posting/fbs/list](#operation/PostingFbsList).

Returns a list of shipments for the specified time period: it shouldn't be longer than one year.

You can filter shipments by their status. The list of available statuses is specified in the description of the `filter.status` parameter.

The `true` value of the `has_next` parameter in the response means there is not the entire array of shipments in the response. To get information on the remaining shipments, make a new request with a different `offset` v...

**Request body:**

- `dir` (string) — Sorting direction:  - `asc`—ascending,  - `desc`—descending.
- `filter` (object) **(required)**
  - `delivery_method_id` (array[integer]) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `fbpFilter` (string) — Filter for shipments delivered from partner warehouse (FBP). You can pass one of the following values:  - `ALL`—all shipments matching other filters w
  - `order_id` (integer(int64)) — Order identifier.
  - `is_blr_traceable` (boolean) — `true` if product is traceable.
  - `is_quantum` (boolean) — Specify `true` to get only MOQ shipments.  The default value is `false`, the response contains all shipments.
  - `provider_id` (array[integer]) — Delivery service identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `since` (string(date-time)) **(required)** — Start date of the period for which a list of shipments should be generated.  Format: YYYYY-MM-DDTHH:MM:SSZ.  Example: 2019-08-24T14:15:22Z.
  - `to` (string(date-time)) **(required)** — End date of the period for which a list of shipments should be generated.  Format: YYYYY-MM-DDTHH:MM:SSZ.  Example: 2019-08-24T14:15:22Z.
  - `status` (string) — Shipment status: - `awaiting_registration`—awaiting registration, - `acceptance_in_progress`—acceptance is in progress, - `awaiting_approve`—awaiting 
  - `warehouse_id` (array[string]) — Warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
  - `last_changed_status_date` (object)
    - `from` (string(date-time)) — Period start date.
    - `to` (string(date-time)) — Period end date.
- `limit` (integer(int64)) **(required)** — Number of shipments in the response:   - maximum is 50,   - minimum is 1.
- `offset` (integer(int64)) **(required)** — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `with` (object)
  - `analytics_data` (boolean) — Add analytics data to the response.
  - `barcodes` (boolean) — Add the shipment barcodes to the response.
  - `financial_data` (boolean) — Add financial data to the response.
  - `legal_info` (boolean) — Add legal details to the response.
  - `translit` (boolean) — Transliterate the return values.

**Response 200:**

- `result` (object)
  - `has_next` (boolean) — Indicates that the response returned not the entire array of shipments: - `true`—make a new request with a different `offset` value to get information
  - `postings` (array[object]) — Shipment details.

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

### `POST /v4/posting/fbs/list`

**Get shipment list**

Operation ID: `PostingFbsList`

Returns a list of shipments for the specified time period. The period shouldn't be longer than one year. 

To get an up-to-date shipment date, update information about shipments regularly or enable [push notifications](#tag/push_start).

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `delivery_method_ids` (array[string]) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `is_blr_traceable` (boolean) — `true` if the product is tracked.
  - `last_changed_status_date` (object)
    - `from` (string(date-time)) — Start date of the period.
    - `to` (string(date-time)) — End date of the period.
  - `order_id` (integer(int64)) — Order identifier.
  - `order_numbers` (array[string]) — Order numbers to which the shipments belong.
  - `provider_ids` (array[string]) — Delivery service identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `since` (string(date-time)) **(required)** — Start date of the period for which a list of shipments should be generated.
  - `statuses` (array[string]) — Shipment status:  - `awaiting registration`: awaiting registration; - `acceptance_in_progress`: acceptance in progress; - `awaiting_approve`: awaiting
  - `to` (string(date-time)) **(required)** — End date of the period for which a list of shipments should be generated.
  - `warehouse_ids` (array[string]) — Warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
- `limit` (integer(int64)) **(required)** — Number of values in the response.
- `sort_dir` (enum) — Values: `ASC, DESC`
- `translit` (boolean) — `true` to enable the address transliteration from Cyrillic to Latin.
- `with` (object)
  - `analytics_data` (boolean) — `true` to add analytics data.
  - `barcodes` (boolean) — `true` to add the shipment barcodes to the response.
  - `financial_data` (boolean) — `true` to add financial data.
  - `legal_info` (boolean) — `true` to add legal information.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `has_next` (boolean) — `true` if the response doesn't contain all shipments.
- `postings` (object) — Shipment list.

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

### `POST /v3/posting/fbs/get`

**Get shipment details by identifier (version 3)**

Operation ID: `PostingAPI_GetFbsPostingV3`

**Request body:**

- `posting_number` (string) **(required)** — Shipment identifier.
- `with` (object)
  - `analytics_data` (boolean) — Add analytics data to the response.
  - `barcodes` (boolean) — Add the shipment barcodes to the response.
  - `financial_data` (boolean) — Add financial data to the response.
  - `legal_info` (boolean) — Add legal details to the response.
  - `product_exemplars` (boolean) — Add data on products and their instances to the response.
  - `related_postings` (boolean) — Add related shipment numbers to the response. Related shipments are ones into which the parent shipment was split during packaging.
  - `translit` (boolean) — Transliterate the return values.

**Response 200:**

- `result` (object)
  - `additional_data` (array[object])
  - `addressee` (object)
  - `analytics_data` (object)
  - `available_actions` (object) — Available actions and shipment information: - `arbitration`: open a dispute; - `awaiting_delivery`: set to the "Awaiting shipping" status; - `can_crea
  - `barcodes` (object)
  - `cancellation` (object)
  - `courier` (object)
  - `customer` (object)
  - `delivering_date` (string(date-time)) — Date when the shipment was transferred for delivery.
  - `delivery_method` (object)
  - `delivery_price` (string) — Delivery cost.
  - `fact_delivery_date` (string(date-time)) — Actual date of shipment transfer to delivery.
  - `financial_data` (object)
  - `in_process_at` (string(date-time)) — Start date and time of shipment processing.
  - `is_express` (boolean) — If Ozon Express fast delivery was used—`true`.
  - `is_multibox` (boolean) — Indication that there is a multi-box product in the shipment and you need to pass the number of boxes for it:  - `true`: before packaging pass the num
  - `legal_info` (object)
  - `multi_box_qty` (integer(int32)) — Number of boxes in which the product is packed.
  - `optional` (object)
  - `order_id` (integer(int64)) — Order identifier to which the shipment belongs.
  - `order_number` (string) — Order number to which the shipment belongs.
  - `parent_posting_number` (string) — Number of the parent shipment which split resulted in the current shipment.
  - `pickup_code_verified_at` (string(date-time)) — Date and time of successful verification of the courier code. Use the [/v1/posting/fbs/pick-up-code/verify](#operation/PostingAPI_PostingFBSPickupCode
  - `posting_number` (string) — Shipment number.
  - `product_exemplars` (object)
  - `products` (array[object]) — Array of products in the shipment.
  - `provider_status` (string) — Delivery service status.
  - `prr_option` (object)
  - `related_postings` (object)
  - `related_weight_postings` (array[string]) — List with numbers of related shipments sold by weight.
  - `require_blr_traceable_attrs` (boolean) — `true` if you have to fill out traceability attributes.
  - `requirements` (object)
  - `shipment_date` (string(date-time)) — Date and time before which the shipment must be packaged. We show the recommended time of shipment. After this time a new rate will be applied, please
  - `shipment_date_without_delay` (string(date-time)) — Date and time of shipment without overdue.
  - `status` (string) — Shipment status:       - `acceptance_in_progress`: acceptance is in progress; - `arbitration`: arbitration; - `awaiting_approve`: awaiting confirmatio
  - `substatus` (string) — Shipment substatus: - `posting_acceptance_in_progress`: acceptance in progress; - `posting_in_arbitration`: arbitrage; - `posting_created`: created; -
  - `previous_substatus` (string) — Previous sub-status of the shipment. Possible values: - `posting_acceptance_in_progress`: acceptance in progress; - `posting_in_arbitration`: arbitrag
  - `tpl_integration_type` (string) — Type of integration with the delivery service:  - `ozon`: delivery by the Ozon logistics;  - `aggregator`: delivery by a third-party service, Ozon reg
  - `tracking_number` (string) — Shipment tracking number.
  - `tariffication` (object)
  - `tariffication_steps` (array[object]) — Shipping rate stages.

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

### `POST /v2/posting/fbs/get-by-barcode`

**Get shipment data by barcode**

Operation ID: `PostingAPI_GetFbsPostingByBarcode`

**Request body:**

- `barcode` (string) **(required)** — Shipment barcode. You can get it in the `barcodes` array of the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3), [/v3/posting/fbs/list](#

**Response 200:**

- `result` (object)
  - `barcodes` (object)
  - `cancel_reason_id` (integer(int64)) — Cancellation reason identifier.
  - `created_at` (string(date-time)) — Date and time when the shipment was created.
  - `in_process_at` (string(date-time)) — Start date and time of shipment processing.
  - `order_id` (integer(int64)) — Order identifier to which the shipment belongs.
  - `order_number` (string) — Order number to which the shipment belongs.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — List of products in the shipment.
  - `shipment_date` (string(date-time)) — Date and time before which the shipment must be packaged. If the shipment is not packaged by this date, it will be canceled automatically.
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

### `POST /v3/posting/multiboxqty/set`

**Specify number of boxes for multi-box shipments**

Operation ID: `PostingAPI_PostingMultiBoxQtySetV3`

Method for passing the number of boxes for multi-box shipments when working under the rFBS Aggregator scheme (using the Ozon partner delivery).

**Request body:**

- `posting_number` (string) **(required)** — Multi-box shipment identifier.
- `multi_box_qty` (integer(int64)) **(required)** — Number of boxes in which the product is packed.

**Response 200:**

- `result` (object)
  - `result` (boolean) — Possible values: - `true`—the number is successfully passed. - `false`—an error occurred while passing the number. Please try again.

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

### `POST /v2/posting/fbs/product/country/list`

**List of manufacturing countries**

Operation ID: `PostingAPI_ListCountryProductFbsPostingV2`

Method for getting a list of available manufacturing countries and their ISO codes.

**Request body:**

- `name_search` (string) — Filtering by line.

**Response 200:**

- `result` (array[object]) — List of manufacturing countries and their ISO codes.
  - `name` (string) — Country name in Russian.
  - `country_iso_code` (string) — Country ISO code.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/posting/fbs/product/country/set`

**Set the manufacturing country**

Operation ID: `PostingAPI_SetCountryProductFbsPostingV2`

The method to set the manufacturing country to the product if it hasn't been specified.

**Request body:**

- `posting_number` (string) **(required)** — Shipment identifier.
- `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, `product_id`.
- `country_iso_code` (string) **(required)** — Country ISO code from the [/v2/posting/fbs/product/country/list](#operation/PostingAPI_ListCountryProductFbsPostingV2) method response.

**Response 200:**

- `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
- `is_gtd_needed` (boolean) — Indication that you need to pass the сustoms cargo declaration (CCD) number for the product and shipment.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/fbs/restrictions`

**Get drop-off point restrictions**

Operation ID: `PostingAPI_GetRestrictions`

Method for getting dimensions, weight, and other restrictions of the drop-off point by the shipment number. The method is applicable only for the FBS scheme.

**Request body:**

- `posting_number` (string) **(required)** — The number of shipment for which you want to determine the restrictions.

**Response 200:**

- `result` (object)
  - `posting_number` (string) — Shipment number.
  - `max_posting_weight` (number(double)) — Maximum weight limit in grams.
  - `min_posting_weight` (number(double)) — Minimum weight limit in grams.
  - `width` (number(double)) — Width limit in centimeters.
  - `length` (number(double)) — Length limit in centimeters.
  - `height` (number(double)) — Height limit in centimeters.
  - `max_posting_price` (number(double)) — Maximum shipment cost limit in rubles.
  - `min_posting_price` (number(double)) — Minimum shipment cost limit in rubles.

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

### `POST /v2/posting/fbs/package-label`

**Print the labeling**

Operation ID: `PostingAPI_PostingFBSPackageLabel`

> **Note:** 
If you work under the rFBS or rFBS Express scheme, learn more about printing labels in the [Seller knowledge base](https://docs.ozon.ru/global/en/fulfillment/rfbs/logistic-settings/order-packaging-requirements/).

Generates a PDF file with a labeling for the specified shipments in the "Awaiting shipment" status: `awaiting_deliver`. You can pass a maximum of 20 identifiers in one request. If an error occurs for at least one shipment, the labeling isn't generated for all shipments in the request.

We recommend you to request labels 45–60 seconds after packing the shipments.

`The ne...

**Request body:**

- `posting_number` (array[string]) **(required)** — Shipment identifier.

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

### `POST /v1/posting/fbs/package-label/create`

**Create a task to generate labeling**

Operation ID: `PostingAPI_CreateLabelBatch`

> **Note:** 
Method is deprecated and will be disabled. We'll give you one month's notice. Switch to the new version [/v2/posting/fbs/package-label/create](#operation/PostingAPI_CreateLabelBatchV2).

Method for creating a task for asynchronous labeling generation.

To get labels created as a result of the method, 
use the [/v1/posting/fbs/package-label/get](#operation/PostingAPI_GetLabelBatch) method.

**Request body:**

- `posting_number` (object) **(required)** — Numbers of shipments that need labeling.

**Response 200:**

- `result` (object)
  - `task_id` (integer(int64)) — Task identifier for labeling generation.

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

### `POST /v2/posting/fbs/package-label/create`

**Create a task to generate a label**

Operation ID: `PostingAPI_CreateLabelBatchV2`

> **Note:** 
If you work under the rFBS or rFBS Express scheme, learn more about printing labels in the [Seller knowledge base](https://docs.ozon.ru/global/en/fulfillment/rfbs/logistic-settings/order-packaging-requirements/).

Method for creating a task for asynchronous label generation for shipments in the "Awaiting shipment" status: `awaiting_deliver`.
The method may return several tasks: to generate a small label and a regular label.

We recommend you to request labels 45–60 seconds after packing the shipments.

To get created labels, use the [/v1/posting/fbs/package-label/get](#operation/P...

**Request body:**

- `posting_number` (object) **(required)** — Numbers of shipments that need labeling.

**Response 200:**

- `result` (object)
  - `tasks` (array[object]) — Task list.

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

### `POST /v1/posting/fbs/package-label/get`

**Get a labeling file**

Operation ID: `PostingAPI_GetLabelBatch`

Method for getting labeling after using the [/v1/posting/fbs/package-label/create](#operation/PostingAPI_CreateLabelBatch) method.

**Request body:**

- `task_id` (integer(int64)) **(required)** — Task identifier for labeling generation from the [/v1/posting/fbs/package-label/create](#operation/PostingAPI_CreateLabelBatch) method response.

**Response 200:**

- `result` (object)
  - `error` (string) — Error code.
  - `file_url` (string) — Link to a labeling file.
  - `printed_postings_count` (integer(int32)) — Number of printed labels.
  - `status` (string) — Status of labeling generation: - `pending`: task is in the queue. - `in_progress`: being generated. - `completed`: labeling file is ready. - `error`: 
  - `unprinted_postings` (array[object]) — Reasons why labels can't be printed.
  - `unprinted_postings_count` (integer(int32)) — Number of labels that can't be printed.

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

### `POST /v1/posting/fbs/cancel-reason`

**Shipment cancellation reasons**

Operation ID: `PostingAPI_GetPostingFbsCancelReasonV1`

Returns a list of cancellation reasons for particular shipments.

**Request body:**

- `related_posting_numbers` (array[string]) **(required)** — Shipment numbers.

**Response 200:**

- `result` (array[object]) — Request result.
  - `posting_number` (string) — Shipment number.
  - `reasons` (array[object]) — Information about cancellation reasons.

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

### `POST /v2/posting/fbs/cancel-reason/list`

**Shipments cancellation reasons**

Operation ID: `PostingAPI_GetPostingFbsCancelReasonList`

**Response 200:**

- `result` (array[object]) — Method result.
  - `id` (integer(int64)) — Cancellation reasons.
  - `is_available_for_cancellation` (boolean) — Shipment cancellation result. `true` if the request is available for cancellation.
  - `title` (string) — Category name.
  - `type_id` (string) — Shipment cancellation initiator:   - `buyer`,   - `seller`.

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

### `POST /v2/posting/fbs/product/cancel`

**Cancel sending some products in the shipment**

Operation ID: `PostingAPI_CancelFbsPostingProduct`

Use this method if you cannot send some of the products from the shipment.

To get the `cancel_reason_id` cancellation reason identifiers when working with the FBS or rFBS schemes, use the [/v2/posting/fbs/cancel-reason/list](#operation/PostingAPI_GetPostingFbsCancelReasonList) method.

You can't cancel presumably delivered orders.

**Request body:**

- `cancel_reason_id` (integer(int64)) **(required)** — Product shipping cancellation reason identifier.
- `cancel_reason_message` (string) **(required)** — Additional information on cancellation. Required parameter.
- `items` (array[object]) **(required)** — Products information.
  - `quantity` (integer(int32)) **(required)** — Number of products in the shipment.
  - `sku` (integer(int64)) **(required)** — Product identifier in the seller's system.
- `posting_number` (string) **(required)** — Shipment identifier.

**Response 200:**

- `result` (string) — Shipment number.

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

### `POST /v2/posting/fbs/cancel`

**Cancel the shipment**

Operation ID: `PostingAPI_CancelFbsPosting`

Change shipment status to `cancelled`.

If you are using the rFBS scheme, you have the following cancellation reason identifiers (`cancel_reason_id`) available:

- `352`—product is out of stock;
- `400`—only defective products left;
- `401`—cancellation from arbitration;
- `402`—other reason;
- `665`—the customer didn't pick the order;
- `666`—delivery isn't available in the region;
- `667`—order was lost by the delivery service.

The last 4 reasons are available for shipments in the "Delivering" and "Courier on the way" statuses.

You can't cancel presumably delivered orders.

If `cancel_reas...

**Request body:**

- `cancel_reason_id` (integer(int64)) **(required)** — Shipment cancellation reason identifier.
- `cancel_reason_message` (string) — Additional information on cancellation. If `cancel_reason_id = 402`, the parameter is required.
- `posting_number` (string) **(required)** — Shipment identifier.

**Response 200:**

- `result` (boolean) — Request processing result. `true`, if the request was executed without errors.

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

### `POST /v2/posting/fbs/arbitration`

**Open a dispute over a shipment**

Operation ID: `PostingAPI_MoveFbsPostingToArbitration`

If the shipment has been handed over for delivery, but has not been scanned at the sorting center, you can open a dispute. Opened dispute will put the shipment into the `arbitration` status.

**Request body:**

- `posting_number` (array[string]) **(required)** — Shipment identifier.

**Response 200:**

- `result` (boolean) — Request processing result. `true`, if the request was executed without errors.

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

### `POST /v2/posting/fbs/awaiting-delivery`

**Pass the shipment to shipping**

Operation ID: `PostingAPI_MoveFbsPostingToAwaitingDelivery`

Transfers disputed orders to shipping. The shipment status will change to `awaiting_deliver`.

**Request body:**

- `posting_number` (array[string]) **(required)** — Shipment identifier. The maximum number of values in one request is 100.

**Response 200:**

- `result` (boolean) — Request processing result. `true`, if the request was executed without errors.

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

### `POST /v1/posting/fbs/pick-up-code/verify`

**Verify courier code**

Operation ID: `PostingAPI_PostingFBSPickupCodeVerify`

Use this method to verify the courier code when handing over realFBS Express shipments. Learn more about handing shipments over in the [Seller Knowledge Base](https://seller-edu.ozon.ru/contract-for-sellers/regulations-fbs-realfbs/reglament-prodaji-so-svoego-sklada-fbs-express#7-порядок-передачи-отправлении-через-партнёров-ozon-при-экспресс-доставке).

**Request body:**

- `pickup_code` (string) **(required)** — Courier code.
- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `valid` (boolean) — `true`, if the code is correct.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/global/etgb`

**ETGB customs declarations**

Operation ID: `PostingAPI_GetEtgb`

Method for getting Elektronik Ticaret Gümrük Beyannamesi (ETGB) customs declarations for sellers from Turkey.

**Request body:**

- `date` (object) **(required)**
  - `from` (string(date-time)) **(required)** — Start date.
  - `to` (string(date-time)) **(required)** — End date.

**Response 200:**

- `result` (array[object]) — Request result.
  - `posting_number` (string) — Shipment number.
  - `etgb` (object)

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

### `POST /v1/posting/unpaid-legal/product/list`

**List of unpaid products from legal entities**

Operation ID: `PostingAPI_UnpaidLegalProductList`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `limit` (integer(int32)) **(required)** — Number of values in the response.

**Response 200:**

- `products` (array[object]) — Product list.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `quantity` (integer(int32)) — Product quantity, pcs.
  - `name` (string) — Product name.
  - `image_url` (string) — Link to product image.
- `cursor` (string) — Cursor for the next data sample.

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

## PolygonAPI

### `POST /v1/polygon/create`

**Create delivery polygon**

Operation ID: `PolygonAPI_CreatePolygon`

You can link a polygon to the delivery method.

Create a polygon getting its coordinates on https://geojson.io: mark at least 3 points on the map and connect them.

**Request body:**

- `coordinates` (string) **(required)** — Delivery polygon coordinates in `[[[lat long]]]` format.

**Response 200:**

- `polygon_id` (integer(int64)) — Polygon identifier.

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error message:    - `coordinates not provided`—you didn't pass coordinates;   - `invalid coordinates, must have two points in coordinate`—there is onl

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

### `POST /v1/polygon/bind`

**Link delivery method to a delivery polygon**

Operation ID: `PolygonAPI_BindPolygon`

**Request body:**

- `delivery_method_id` (integer(int32)) **(required)** — Delivery method identifier.
- `polygons` (array[object]) **(required)** — Polygons list.
  - `polygon_id` (integer(int64)) **(required)** — Polygon identifier.
  - `time` (integer(int64)) **(required)** — Delivery time within polygon in minutes.
- `warehouse_location` (object) **(required)**
  - `lat` (string) **(required)** — Warehouse location latitude.
  - `lon` (string) **(required)** — Warehouse location longitude.

**Response 200:**

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

## FBO

### `POST /v2/posting/fbo/list`

**Shipments list**

Operation ID: `PostingAPI_GetFboPostingList`

> **Note:** 
Method is deprecated and disabled on June 1, 2026. Switch to the new version [/v3/posting/fbo/list](#operation/PostingFboList).

 

Returns a list of shipments for a specified period of time. You can additionally filter the shipments by their status.

**Request body:**

- `dir` (string) — Sorting direction: - `ASC`—ascending, - `DESC`—descending.
- `filter` (object) **(required)**
  - `since` (string(date-time)) **(required)** — Period start date.
  - `status` (string) — Shipment status:   - `awaiting_packaging`—awaiting packaging,   - `awaiting_deliver`—awaiting shipping,   - `delivering`—delivery is in progress,   - 
  - `to` (string(date-time)) **(required)** — Period end date.
- `limit` (integer(int64)) **(required)** — Number of values in the response. Maximum is 1000, minimum is 1.
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `translit` (boolean) — `true` if the address transliteration from Cyrillic to Latin is enabled.
- `with` (object)
  - `analytics_data` (boolean) — Specify `true` to add analytics data to the response.
  - `financial_data` (boolean) — Specify `true` to add financial data to the response.
  - `legal_info` (boolean) — Pass `true` to add legal details to the response.

**Response 200:**

- `result` (array[object]) — Shipment list.
  - `additional_data` (array[object])
  - `analytics_data` (object)
  - `cancel_reason_id` (integer(int64)) — Shipment cancellation reason identifier.
  - `created_at` (string(date-time)) — Date and time of shipment creation.
  - `financial_data` (object)
  - `in_process_at` (string(date-time)) — Date and time of shipment processing start.
  - `legal_info` (object)
  - `order_id` (integer(int64)) — Identifier of the order to which the shipment belongs.
  - `order_number` (string) — Number of the order to which the shipment belongs.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — Number of products in the shipment.
  - `status` (string) — Shipment status:   - `awaiting_packaging`—awaiting packaging,   - `awaiting_deliver`—awaiting shipping,   - `delivering`—delivery is in progress,   - 
  - `substatus` (string) — Shipment substatus: - `posting_split_pending`, `posting_created`: created; - `posting_packing`: packaging; - `posting_transferring_to_delivery`: hande

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

### `POST /v3/posting/fbo/list`

**Get shipment list**

Operation ID: `PostingFboList`

Returns a list of shipments for a specified period of time. 
If the period is more than a year, you receive the `PERIOD_IS_TOO_LONG` error.

You can additionally filter the shipments by their status.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `order_numbers` (array[string]) — Order numbers to which the shipments belong.
  - `posting_numbers` (array[string]) — Shipment identifiers.
  - `since` (string(date-time)) — Start date.
  - `statuses` (array[string]) — Shipment status: - `awaiting_packaging`: awaiting packaging;  - `awaiting_deliver`: awaiting shipping;  - `delivering`: delivery in progress;  - `deli
  - `to` (string(date-time)) — End date.
- `limit` (integer(int64)) — Number of values in the response.
- `sort_dir` (enum) — Values: `ASC, DESC`
- `translit` (boolean) — `true` to enable the address transliteration from Cyrillic to Latin.
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
  - `status` (string) — Shipment status:  - `awaiting_packaging`: awaiting packaging;  - `awaiting_deliver`: awaiting shipping; - `delivering`: delivery in progress;  - `deli
  - `substatus` (string) — Shipment substatus:  - `posting_split_pending`, `posting_created`: created; - `posting_packing`: packaging; - `posting_transferring_to_delivery`: hand

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

### `POST /v2/posting/fbo/get`

**Shipment details**

Operation ID: `PostingAPI_GetFboPosting`

Returns information about the shipment by its identifier.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.
- `translit` (boolean) — `true` if the address transliteration from Cyrillic to Latin is enabled.
- `with` (object)
  - `analytics_data` (boolean) — Specify `true` to add analytics data to the response.
  - `financial_data` (boolean) — Specify `true` to add financial data to the response.
  - `legal_info` (boolean) — Pass `true` to add legal details to the response.

**Response 200:**

- `result` (object)
  - `additional_data` (array[object])
  - `analytics_data` (object)
  - `cancel_reason_id` (integer(int64)) — Shipment cancellation reason identifier.
  - `created_at` (string(date-time)) — Date and time of shipment creation.
  - `fact_delivery_date` (string(date-time)) — Actual date of shipment transfer to delivery.
  - `financial_data` (object)
  - `in_process_at` (string(date-time)) — Date and time of shipment processing start.
  - `legal_info` (object)
  - `order_id` (integer(int64)) — Identifier of the order to which the shipment belongs.
  - `order_number` (string) — Number of the order to which the shipment belongs.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — Number of products in the shipment.
  - `status` (string) — Shipment status:   - `awaiting_packaging`—awaiting packaging,   - `awaiting_deliver`—awaiting shipping,   - `delivering`—delivery is in progress,   - 
  - `substatus` (string) — Shipment substatus: - `posting_split_pending`, `posting_created`: created; - `posting_packing`: packaging; - `posting_transferring_to_delivery`: hande

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

### `POST /v1/posting/fbo/cancel-reason/list`

**Shipments cancellation reasons by FBO scheme**

Operation ID: `PostingAPI_GetPostingFboCancelReasonList`

**Response 200:**

- `reasons` (array[object]) — Cancellation reason information.
  - `id` (integer(int64)) — Cancellation reason identifier.
  - `name` (string) — Cancellation reason.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/status/counter`

**Number of supply requests by status**

Operation ID: `SupplyOrderAPI_SupplyOrderStatusCounter`

Returns the number of supply requests in a specific status.

**Request body:**

**Response 200:**

- `items` (array[object])
  - `count` (integer(int32)) — Number of supply requests in this status.
  - `order_state` (enum) — Values: `ORDER_STATE_UNSPECIFIED, ORDER_STATE_DATA_FILLING, ORDER_STATE_READY_TO_SUPPLY, ORDER_STATE_ACCEPTED_AT_SUPPLY_WAREHOUSE, ORDER_STATE_IN_TRANSIT, ORDER_STATE_ACCEPTANCE_AT_STORAGE_WAREHOUSE, ORDER_STATE_REPORTS_CONFIRMATION_AWAITING, ORDER_STATE_REPORT_REJECTED`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/bundle`

**Supply or supply request contents**

Operation ID: `SupplyOrderBundle`

Use the method for getting the contents of a supply or draft supply request. A single call to the method can get the contents of one supply or one draft supply request.

**Request body:**

- `bundle_ids` (array[string]) **(required)** — Identifiers of supply contents. You can get them using the[/v3/supply-order/get](#operation/SupplyOrderGet) method.
- `is_asc` (boolean) — `true`, to sort in ascending order.
- `item_tags_calculation` (object)
  - `dropoff_warehouse_id` (string) **(required)** — Indentifier of the supply shipping warehouse.
  - `storage_warehouse_ids` (array[string]) **(required)** — List of supply warehouse identifiers, no more than 25 values.
- `last_id` (string) — Identifier of the last SKU value on the page.
- `limit` (integer(int32)) **(required)** — Number of products on the page.
- `query` (string) — Search query, for example: by name, article code, or SKU.
- `sort_field` (enum) — Values: `SKU, NAME, QUANTITY, TOTAL_VOLUME_IN_LITRES`

**Response 200:**

- `items` (array[object]) — List of products in the supply request.
  - `icon_path` (string) — Link to product image.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `quantity` (integer(int32)) — Quantity of product items.
  - `barcode` (string) — Barcode.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `quant` (integer(int32)) — Quantity of products in one package.
  - `is_quant_editable` (boolean) — `true` if the quantity of products in one package can be edited.
  - `volume_in_litres` (number(double)) — Volume of products in liters.
  - `total_volume_in_litres` (number(double)) — Volume of all products in liters.
  - `contractor_item_code` (string) — Product article code.
  - `sfbo_attribute` (enum) — Values: `ITEM_SFBO_ATTRIBUTE_NONE, ITEM_SFBO_ATTRIBUTE_SUPER_FBO, ITEM_SFBO_ATTRIBUTE_ANTI_FBO`
  - `shipment_type` (enum) — Values: `BUNDLE_ITEM_SHIPMENT_TYPE_GENERAL, BUNDLE_ITEM_SHIPMENT_TYPE_BOX, BUNDLE_ITEM_SHIPMENT_TYPE_PALLET`
  - `tags` (array[string]) — Product tags from a supply or a supply request.  Possible values: - `EVSD_REQUIRED`: product with certification in the Mercury system; - `MARKING_REQU
- `total_count` (integer(int32)) — Quantity of products in the request.
- `has_next` (boolean) — Indication that the response hasn't returned all products: - `true`: create another request with a different `last_id` value to get the remaining valu
- `last_id` (string) — Identifier of the last value on the page.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v3/supply-order/list`

**List of supply requests to the Ozon warehouse**

Operation ID: `SupplyOrderList`

Supply requests to a specific warehouse and through a [virtual distribution center (vDC)](https://seller-edu.ozon.ru/fbo/scheme-of-work/about#чем-отличаются-процессы-при-заявках-через-врц-и-напрямую-на-склад) are taken into account.

**Request body:**

- `filter` (object) **(required)**
  - `dropoff_warehouse_ids` (array[string]) — Drop-off point identifiers.
  - `order_number_search` (string) — Supply request number.
  - `states` (array[object]) **(required)** — Supply status:  - `DATA_FILLING`: filling in the data;  - `READY_TO_SUPPLY`: ready for shipping;  - `ACCEPTED_AT_SUPPLY_WAREHOUSE`: accepted at the sh
  - `timeslot_from_range` (object)
    - `from` (string(date-time)) — Start date.
    - `timeslot_filter_type` (enum) — Values: `BY_LOCAL_TIME, BY_UTC_TIME`
    - `to` (string(date-time)) — End date.
- `last_id` (string) — Last value identifier on the page. Leave this field empty for the first request.  To get the next values, specify the `last_id` from the previous requ
- `limit` (integer(int32)) **(required)** — Number of values per page.
- `sort_by` (enum) **(required)** — Values: `ORDER_CREATION, ORDER_STATE_UPDATED_AT, TIMESLOT_FROM_UTC, TIMESLOT_FROM_LOCAL`
- `sort_dir` (enum) — Values: `ASC, DESC`

**Response 200:**

- `last_id` (string) — Last value identifier on the page.  To get the next values, specify the received value in the `last_id` parameter of the next request.
- `order_ids` (array[string]) — Supply request identifiers.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v3/supply-order/get`

**Supply request details**

Operation ID: `SupplyOrderGet`

Supply requests to a specific warehouse and through a [virtual distribution center (vDC)](https://seller-edu.ozon.ru/fbo/scheme-of-work/about#чем-отличаются-процессы-при-заявках-через-врц-и-напрямую-на-склад) are taken into account.

**Request body:**

- `order_ids` (array[string]) **(required)** — Supply request identifiers.

**Response 200:**

- `orders` (array[object]) — List of supply requests.
  - `created_date` (string(date-time)) — Supply request creation date.
  - `data_filling_deadline_utc` (string(date-time)) — Time in seconds remaining to complete the supply data entry. Only for requests with vDC.
  - `dropoff_warehouse` (object)
  - `order_id` (integer(int64)) — Supply request identifier.
  - `order_number` (string) — Supply request number.
  - `order_tags` (object)
  - `state` (enum) — Values: `UNSPECIFIED, DATA_FILLING, READY_TO_SUPPLY, ACCEPTED_AT_SUPPLY_WAREHOUSE, IN_TRANSIT, ACCEPTANCE_AT_STORAGE_WAREHOUSE, REPORTS_CONFIRMATION_AWAITING, REPORT_REJECTED`
  - `state_updated_date` (string(date-time)) — Supply request status update date.
  - `supplies` (array[object]) — Supply information.
  - `timeslot` (object)

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/timeslot/get`

**Supply time slots**

Operation ID: `SupplyOrderAPI_GetSupplyOrderTimeslots`

**Request body:**

- `supply_order_id` (integer(int64)) **(required)** — Supply request identifier.

**Response 200:**

- `timeslots` (array[object]) — Supply time slot.
  - `from` (string(date-time)) **(required)** — Supply time slot start. Displayed in your local time.
  - `to` (string(date-time)) **(required)** — Supply time slot end. Displayed in your local time.
- `timezone` (object) — Time zone.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/timeslot/update`

**Update supply time slot**

Operation ID: `SupplyOrderAPI_UpdateSupplyOrderTimeslot`

**Request body:**

- `supply_order_id` (integer(int64)) **(required)** — Supply request identifier.
- `timeslot` (object) **(required)**
  - `from` (string(date-time)) **(required)** — Supply time slot start. Displayed in your local time.
  - `to` (string(date-time)) **(required)** — Supply time slot end. Displayed in your local time.

**Response 200:**

- `errors` (array[object]) — Possible errors:    - `UNSPECIFIED`: no status specified;   - `INVALID_ORDER_STATE`: incorrect order status;   - `INCOMPATIBLE_ORDER_FLOW`: incorrect 
- `operation_id` (string) — Operation identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/timeslot/status`

**Supply time slot status**

Operation ID: `SupplyOrderAPI_GetSupplyOrderTimeslotStatus`

**Request body:**

- `operation_id` (string) **(required)** — Operation identifier.

**Response 200:**

- `errors` (array[object]) — Possible errors:    - `UNSPECIFIED`: no status specified;   - `INVALID_ORDER_STATE`: incorrect order status;   - `INCOMPATIBLE_ORDER_FLOW`: incorrect 
- `status` (enum) — Values: `STATUS_UNSPECIFIED, STATUS_ERROR, STATUS_IN_PROGRESS, STATUS_SUCCESS`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/pass/create`

**Specify driver and vehicle details**

Operation ID: `SupplyOrderAPI_SupplyOrderPassCreate`

**Request body:**

- `supply_order_id` (integer(int64)) **(required)** — Supply request identifier.
- `vehicle` (object) **(required)**
  - `driver_name` (string) **(required)** — Driver full name.
  - `driver_phone` (string) **(required)** — Driver phone number.
  - `vehicle_model` (string) **(required)** — Car model.
  - `vehicle_number` (string) **(required)** — Car number.

**Response 200:**

- `error_reasons` (array[object]) — Possible errors:   - `UNSPECIFIED`: no status specified;   - `INVALID_ORDER_STATE`: incorrect order status;   - `VEHICLE_NOT_REQUIRED`: vehicle detail
- `operation_id` (string) — Operation identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/pass/status`

**Driver and vehicle details entry status**

Operation ID: `SupplyOrderAPI_SupplyOrderPassStatus`

**Request body:**

- `operation_id` (string) **(required)** — Operation identifier.

**Response 200:**

- `errors` (array[object]) — Possible errors: - `UNSPECIFIED`: no status specified; - `INVALID_ORDER_STATE`: incorrect order status; - `VEHICLE_NOT_REQUIRED`: vehicle details aren
- `result` (enum) — Values: `Unknown, Success, InProgress, Failed`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `GET /v1/supplier/available_warehouses`

**Ozon warehouses workload**

Operation ID: `SupplierAPI_SupplierAvailableWarehouses`

Method returns a list of active Ozon warehouses with information about their average workload in the nearest future.

**Response 200:**

- `result` (object) — Method result.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## FboSupplyRequest

### `POST /v1/cluster/list`

**Information about clusters and their warehouses**

Operation ID: `SupplyDraftAPI_DraftClusterList`

**Request body:**

- `cluster_ids` (array[string]) — Clusters identifiers.
- `cluster_type` (enum) **(required)** — Values: `CLUSTER_TYPE_OZON, CLUSTER_TYPE_CIS`

**Response 200:**

- `clusters` (array[object]) — Cluster details.
  - `id` (integer(int64)) — Cluster identifier.
  - `logistic_clusters` (array[object]) — Cluster warehouse details.
  - `name` (string) — Cluster name.
  - `type` (enum) — Values: `CLUSTER_TYPE_OZON, CLUSTER_TYPE_CIS`. Cluster type: - `CLUSTER_TYPE_OZON`—cluster in Russia, - `CLUSTER_TYPE_CIS`—CIS cluster.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/warehouse/fbo/list`

**Finding points to ship the supply**

Operation ID: `SupplyDraftAPI_DraftGetWarehouseFboList`

Use the method to find shipping points for cross-docking and direct supplies. 

You can view the addresses of all points on the map and in a table in the [Knowledge Base](https://seller-edu.ozon.ru/fbo/warehouses/adresa-skladov-fbo).

**Request body:**

- `filter_by_supply_type` (array[object]) **(required)** — Supply type: - `CREATE_TYPE_CROSSDOCK`—cross-docking, - `CREATE_TYPE_DIRECT`—direct.
- `search` (string) **(required)** — Search by warehouse name. To search for pick-up points, specify the full name.

**Response 200:**

- `search` (array[object]) — Warehouse search result.
  - `address` (string) — Warehouse address.
  - `coordinates` (object)
  - `name` (string) — Warehouse name.
  - `warehouse_id` (integer(int64)) — Identifier of the warehouse, pick-up point, or sorting center.
  - `warehouse_type` (enum) — Values: `WAREHOUSE_TYPE_DELIVERY_POINT, WAREHOUSE_TYPE_ORDERS_RECEIVING_POINT, WAREHOUSE_TYPE_SORTING_CENTER, WAREHOUSE_TYPE_FULL_FILLMENT, WAREHOUSE_TYPE_CROSS_DOCK`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/draft/create`

**Create a supply request draft**

Operation ID: `SupplyDraftAPI_DraftCreate`

> **Note:** 
  Method is deprecated and disabled on March 16, 2026. Switch to the [/v1/draft/crossdock/create](https://docs.ozon.ru/api/seller/#operation/DraftCrossdockCreate), [/v1/draft/direct/create](https://docs.ozon.ru/api/seller/#operation/DraftDirectCreate), or [/v1/draft/multi-cluster/create](https://docs.ozon.ru/api/seller/#operation/DraftMultiClusterCreate) methods.

> **Note:** A supply request draft is available for 30 minutes.

You can create supply request drafts twice per minute and 50 times per hour.
The maximum number of drafts per day is 500.

If you reach the limit, you get ...

**Request body:**

- `cluster_ids` (array[string]) — Cluster identifiers for the supply request. You can get them using the [/v1/cluster/list](#operation/SupplyDraftAPI_DraftClusterList) method.
- `drop_off_point_warehouse_id` (integer(int64)) — Shipping point identifier: pick-up point or sorting center. You can get it using the [/v1/warehouse/fbo/list](#operation/SupplyDraftAPI_DraftGetWareho
- `items` (array[object]) **(required)** — Products.
  - `quantity` (integer(int32)) **(required)** — Product quantity.
  - `sku` (integer(int64)) **(required)** — Product identifier.
- `type` (enum) **(required)** — Values: `CREATE_TYPE_CROSSDOCK, CREATE_TYPE_DIRECT`

**Response 200:**

- `operation_id` (string) — Identifier of the supply request draft.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/draft/create/info`

**Supply request draft details**

Operation ID: `SupplyDraftAPI_DraftCreateInfo`

> **Note:** 
  Method is deprecated and disabled on March 16, 2026. Switch to the [/v2/draft/create/info](https://docs.ozon.ru/api/seller/#operation/DraftCreateInfo) method.

> **Note:** 
You can create supply request drafts twice per minute and 50 times per hour.
If you reach the limit, you get the 429 error.

Returns information about the created supply request draft. The response lists the placement warehouses in each selected cluster that accept all products.

**Request body:**

- `operation_id` (string) **(required)** — Unique identifier for generation of the supply request draft.

**Response 200:**

- `clusters` (array[object]) — Clusters.
  - `cluster_id` (integer(int64)) — Cluster identifier.
  - `cluster_name` (string) — Cluster name.
  - `warehouses` (array[object]) — Placement warehouses.
- `draft_id` (integer(int64)) — Identifier of the supply request draft.
- `errors` (array[object]) — Errors.
  - `error_message` (string) — Possible errors: - `vdc_is_not_supported`—vDC (virtual distribution center) supply type isn't supported; - `drop_off_point_warehouse_is_required`—`dro
  - `items_validation` (array[object]) — Validation errors.
  - `unknown_cluster_ids` (array[string]) — Unknown clusters identifiers.
- `status` (enum) — Values: `CALCULATION_STATUS_FAILED, CALCULATION_STATUS_SUCCESS, CALCULATION_STATUS_IN_PROGRESS, CALCULATION_STATUS_EXPIRED`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/draft/timeslot/info`

**Available supply time slots**

Operation ID: `SupplyDraftAPI_DraftTimeslotInfo`

> **Note:** 
  Method is deprecated and disabled on March 16, 2026. Switch to the [/v2/draft/timeslot/info](https://docs.ozon.ru/api/seller/#operation/DraftTimeslotInfo) method.

> **Note:** The supply request draft is available for 30 minutes.

Returns available supply time slots at final shipping warehouses. For cross-docking supplies, the response returns time slots of the shipping warehouse passed when creating the draft.

**Request body:**

- `date_from` (string(date-time)) **(required)** — Start date of the available supply time slots period.
- `date_to` (string(date-time)) **(required)** — End date of the available supply time slots period.  The maximum period is 28 days from the current date.
- `draft_id` (integer(int64)) **(required)** — Identifier of the supply request draft. You can get it using the [/v1/draft/create/info](#operation/SupplyDraftAPI_DraftCreateInfo) method.
- `warehouse_ids` (array[string]) **(required)** — Placement warehouse identifiers.

**Response 200:**

- `drop_off_warehouse_timeslots` (array[object]) — Warehouses supply time slots.
  - `current_time_in_timezone` (string(date-time)) — Current time in the warehouse time zone.
  - `days` (array[object]) — Supply time slots by dates.
  - `drop_off_warehouse_id` (integer(int64)) — Warehouse identifier.
  - `warehouse_timezone` (string) — Warehouse time zone.
- `requested_date_from` (string(date-time)) — Start date of the necessary period.
- `requested_date_to` (string(date-time)) — End date of the necessary period.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/draft/supply/create`

**Create a supply request from the draft**

Operation ID: `SupplyDraftAPI_DraftSupplyCreate`

> **Note:** 
  Method is deprecated and disabled on March 16, 2026. Switch to the [/v2/draft/supply/create](https://docs.ozon.ru/api/seller/#operation/DraftSupplyCreate) method.

**Request body:**

- `draft_id` (integer(int64)) **(required)** — Identifier of the supply request draft.
- `timeslot` (object)
  - `from_in_timezone` (string(date-time)) — Supply time slot start date.
  - `to_in_timezone` (string(date-time)) — Supply time slot end date.
- `warehouse_id` (integer(int64)) **(required)** — Placement warehouse identifier. You can get it using the [/v1/draft/create/info](#operation/SupplyDraftAPI_DraftCreateInfo) method.

**Response 200:**

- `operation_id` (string) — Supply request identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/draft/supply/create/status`

**Supply request creating details**

Operation ID: `SupplyDraftAPI_DraftSupplyCreateStatus`

> **Note:** 
  Method is deprecated and disabled on March 16, 2026. Switch to the [/v2/draft/supply/create/status](https://docs.ozon.ru/api/seller/#operation/DraftSupplyCreateStatus) method.

**Request body:**

- `operation_id` (string) **(required)** — Supply request identifier.

**Response 200:**

- `error_messages` (array[string]) — Requests creation errors.
- `result` (object)
  - `order_ids` (array[string]) — Supply requests identifiers.
- `status` (enum) — Values: `DraftSupplyCreateStatusUnknown, DraftSupplyCreateStatusSuccess, DraftSupplyCreateStatusFailed, DraftSupplyCreateStatusInProgress`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/cancel`

**Cancel supply request**

Operation ID: `SupplyOrderAPI_SupplyOrderCancel`

**Request body:**

- `order_id` (integer(int64)) **(required)** — Supply request identifier.

**Response 200:**

- `operation_id` (string) — Operation identifier for canceling the request.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/supply-order/cancel/status`

**Get status of canceled supply request**

Operation ID: `SupplyOrderAPI_SupplyOrderCancelStatus`

**Request body:**

- `operation_id` (string) **(required)** — Operation identifier for canceling the supply request.

**Response 200:**

- `error_reasons` (array[object]) — Reason why the supply request can't be canceled:   - `INVALID_ORDER_STATE`: incorrect status of the supply request.   - `ORDER_IS_VIRTUAL`: request is
- `result` (object)
  - `is_order_cancelled` (boolean) — `true`, if supply request is canceled.
  - `supplies` (array[object]) — List of canceled supplies.
- `status` (enum) — Values: `SUCCESS, IN_PROGRESS, ERROR`

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## FBS&rFBSMarks

### `POST /v4/posting/fbs/ship`

**Pack the order (version 4)**

Operation ID: `PostingAPI_ShipFbsPostingV4`

> **Note:** 
Response with the 200 code doesn't guarantee that order is packaged successfully. Use the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method to check if order is packaged. If you get result.substatus = ship_failed, repeat order package.

Divides the order into shipments and changes its status to `awaiting_deliver`.

Each element of the `packages` may contain several instances of the `products`. One instance of the `products` is one shipment.
Each element of the `products` is a product included into the shipment.

Divide the order if the products don't fit into one...

**Request body:**

- `packages` (object) **(required)** — List of packages. Each package contains a list of shipments that the order was divided into.
- `posting_number` (string) **(required)** — Shipment number.
- `with` (object)
  - `additional_data` (boolean) — Pass `true` to get additional information.

**Response 200:**

- `additional_data` (object) — Additional information about shipments.
- `result` (object) — Order packaging result.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v4/posting/fbs/ship/package`

**Shipment partial package (version 4)**

Operation ID: `PostingAPI_ShipFbsPostingPackage`

> **Note:** 
Response with the 200 code doesn't guarantee that shipment is packaged successfully. Use the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method to check if shipment is packaged. If you get result.substatus = ship_failed, repeat shipment package.

If you pass some of the shipped products through the request, the primary shipment will split into two parts.
The primary unassembled shipment will contain some of the products that weren't passed to the request.

Default status of created shipments is `awaiting_packaging`, which indicates that the shipment is awaiting as...

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.
- `products` (array[object]) — List of products in the shipment.
  - `exemplarsIds` (array[string]) — Product item identifiers.
  - `product_id` (integer(int64)) **(required)** — Product identifier in the seller's system: SKU.
  - `quantity` (integer(int32)) **(required)** — Items quantity.

**Response 200:**

- `result` (string) — Shipments numbers formed after packaging.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v6/fbs/posting/product/exemplar/set`

**Check and save items data**

Operation ID: `PostingAPI_FbsPostingProductExemplarSetV6`

Asynchronous method:

 - for checking the availability of product items in the “Chestny ZNAK” labeling system;
 - for saving product items data.

To get the checks results, use the [/v5/fbs/posting/product/exemplar/status](#operation/PostingAPI_FbsPostingProductExemplarStatusV5) method.
To get data about created items, use the [/v6/fbs/posting/product/exemplar/create-or-get](#operation/PostingAPI_FbsPostingProductExemplarCreateOrGetV6) method.

If you have multiple identical products in a shipment, specify one `product_id` and `exemplars` array for each product in the shipment.

Always pass a ...

**Request body:**

- `multi_box_qty` (integer(int32)) — Quantity of boxes the product is packed in.
- `posting_number` (string) **(required)** — Shipment number.
- `products` (array[object]) **(required)** — Product list.
  - `exemplars` (array[object]) **(required)** — Data about items.
    - `exemplar_id` (integer(int64)) **(required)** — Item identifier.
    - `gtd` (string) — Customs cargo declaration (CCD) number.
    - `is_gtd_absent` (boolean) — Indication that the customs cargo declaration (CCD) number isn't specified.
    - `is_rnpt_absent` (boolean) — Indication that the product batch registration number isn't specified.
    - `marks` (array[object]) — List of Control Identification Marks and other labelings in one copy.
    - `rnpt` (string) — Product batch registration number.
    - `weight` (number(float)) — Actual item weight.
  - `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v6/fbs/posting/product/exemplar/create-or-get`

**Get created items data**

Operation ID: `PostingAPI_FbsPostingProductExemplarCreateOrGetV6`

Method for getting data about product items from the shipment passed in the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method.

Use this method to get the `exemplar_id`.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `multi_box_qty` (integer(int32)) — Quantity of boxes the product is packed in.
- `posting_number` (string) — Shipment number.
- `products` (array[object]) — Product list.
  - `exemplars` (array[object]) — Data about items.
  - `has_imei` (boolean) — IMEI presence attribute.  If the product has IMEI, the value is `true`.
  - `is_gtd_needed` (boolean) — Indication that you need to pass the сustoms cargo declaration (CCD) number for the product and shipment.
  - `is_jw_uin_needed` (boolean) — Indication that you need to pass the unique identifier of charges of the jewelry.
  - `is_mandatory_mark_needed` (boolean) — Indication that you need to pass the "Chestny ZNAK" labeling.
  - `is_mandatory_mark_possible` (boolean) — Indication that you can pass the "Chestny ZNAK" labeling, but it's not mandatory.
  - `is_rnpt_needed` (boolean) — Indication that you need to pass the product batch registration number.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `quantity` (integer(int32)) — Quantity of items.
  - `is_weight_needed` (boolean) — `true` if the product is sold by weight.
  - `weight_max` (number(float)) — Maximum item weight.
  - `weight_min` (number(float)) — Minimum item weight.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v5/fbs/posting/product/exemplar/status`

**Get statuses of product items check**

Operation ID: `PostingAPI_FbsPostingProductExemplarStatusV5`

Method for getting product items addition statuses that were passed in the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method. Also returns data on these product items.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `posting_number` (string) — Shipment number.
- `products` (array[object]) — Product list.
  - `exemplars` (array[object]) — Product items data.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, SKU.
- `status` (string) — Verification status for product items and order packaging availability: - `ship_available`: order packaging is available; - `ship_not_available`: orde

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v5/fbs/posting/product/exemplar/validate`

**Validate labeling codes**

Operation ID: `PostingAPI_FbsPostingProductExemplarValidateV5`

Method for checking whether labeling codes meet the "Chestny ZNAK" system requirements on length and symbols and other labelings.

If you don't have the customs cargo declaration (CCD) number, you don't have to specify it.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.
- `products` (array[object]) **(required)** — Product list.
  - `exemplars` (array[object]) **(required)** — Product items data.
    - `gtd` (string) — Сustoms cargo declaration (CCD) number.
    - `marks` (array[object]) — List of Control Identification Marks and other labelings in one copy.
    - `rnpt` (string) — Product batch registration number.
    - `weight` (number(float)) — Actual item weight.
  - `product_id` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.

**Response 200:**

- `products` (array[object]) — Product list.
  - `error` (string) — Error code.
  - `exemplars` (array[object]) — Product items data.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `valid` (boolean) — Check result. `true` if the labeling codes of all product items meet the requirements.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/fbs/posting/product/exemplar/update`

**Update items data**

Operation ID: `PostingAPI_FbsPostingProductExemplarUpdate`

Use the method after passing item data using the [/v6/fbs/posting/product/exemplar/set](#operation/PostingAPI_FbsPostingProductExemplarSetV6) method to save updated item data for shipments in the “Awaiting shipment” status.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DeliveryFBS

### `POST /v1/carriage/create`

**Create shipping**

Operation ID: `CarriageAPI_CarriageCreate`

> **Note:** 
If you're a seller outside Russia, please check the availability of the [recommended shipping time](https://seller-edu.ozon.ru/fbs/ozon-logistika/sobrat-zakazy#шаг-2-сформируите-отгрузку) in your personal account.
If it's not available, create shipping using the [/v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) method.
You don't need to confirm shipping created using this method. Once the shipping is created, you can't edit its contents.

Use the method to create the first FBS shipping. It includes all shipments in the `awaiting_deliver` status. The created s...

**Request body:**

- `all_blr_traceable` (boolean) — `true` if you have to create shipping with traceable products.
- `delivery_method_id` (integer(int64)) — Delivery method identifier.
- `departure_date` (string(date-time)) — Shipping date. The default value is current date.

**Response 200:**

- `carriage_id` (integer(int64)) — Shipping identifier.

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

### `POST /v1/carriage/approve`

**Confirm shipping**

Operation ID: `CarriageAPI_CarriageApprove`

Use the method to confirm shipping after creation.
After confirmation, the shipping receives the `FORMED` status.

Once the shipping is confirmed, you can get the shipping list using the [/v2/posting/fbs/act/get-pdf](#operation/PostingAPI_PostingFBSGetAct) method and the shipping barcode using the [/v2/posting/fbs/act/get-barcode](#operation/PostingAPI_PostingFBSGetBarcode) method.

**Request body:**

- `carriage_id` (integer(int64)) — Shipping identifier.
- `containers_count` (integer(int32)) — Number of package units.  Use this parameter if you have trusted acceptance enabled and ship orders by package units. If you don't have trusted accept

**Response 200:**

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

### `POST /v1/carriage/set-postings`

**Change shipping composition**

Operation ID: `CarriageAPI_SetPostings`

> **Note:** 
The method isn't available for sellers from CIS.

Overwrites the list of orders in shipping. Pass only orders in the awaiting_deliver status and ones which are ready for shipping.       

> **Note:** 
To return to the list of orders, delete the shipping using [/v1/carriage/cancel](#operation/CarriageAPI_CarriageCancel) and create a new one.

**Request body:**

- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.
- `posting_numbers` (array[string]) **(required)** — Current list of shipments.

**Response 200:**

- `result` (object)

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

### `POST /v1/carriage/cancel`

**Delete shipping**

Operation ID: `CarriageAPI_CarriageCancel`

**Request body:**

- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

- `error` (string) — Error message.
- `carriage_status` (string) — Shipping status.

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

### `POST /v1/carriage/delivery/list`

**List of delivery methods and shipments**

Operation ID: `CarriageAPI_CarriageDeliveryList`

> **Note:** 
  Method doesn't return information for delivery methods that don't have shipments.

Use the method to get a list of created shipments for a delivery method and their statuses.  

> **Note:** 
  On March 20, 2026 we disable the method. Switch to the new version [/v2/carriage/delivery/list](#operation/CarriageDeliveryListV2).

**Request body:**

- `delivery_method_id` (integer(int64)) — Delivery method identifier.
- `departure_date` (string(date-time)) — Shipping date. The default value is current date.

**Response 200:**

- `result` (array[object])
  - `assembly_list_availability` (boolean) — `true` if the product list is available.
  - `can_create_another_carriage` (boolean) — `true` if another shipping can be created.
  - `carriage_postings_count` (integer(int32)) — Number of shipments in shipping.
  - `carriage_quantum_count` (integer(int32)) — Number of quants in shipping.
  - `carriages` (array[string]) — Shipping list.
  - `cut_in` (string(date-time)) — Assembly start time and time zone of the warehouse.
  - `delivery_method_id` (integer) — Delivery method identifier.
  - `delivery_method_name` (string) — Delivery method name.
  - `delivery_method_status` (string) — Delivery method status.
  - `departure_date` (string(date-time)) — Shipping date.
  - `dropoff_address` (string) — Drop-off point address.
  - `dropoff_change_availability` (string) — Status of the ability to change the drop-off point.
  - `dropoff_point_id` (integer(int64)) — Drop-off point identifier.
  - `dropoff_point_type` (string) — Drop-off point type.
  - `errors` (array[object]) — Array of errors occurred while processing the request.
  - `first_mile_changing` (boolean) — `true` if a drop-off point is changed.
  - `first_mile_type` (string) — First mile type.
  - `has_entrusted_acceptance` (boolean) — Trusted acceptance attribute. `true` if trusted acceptance is enabled in the warehouse.
  - `integration_type` (string) — Type of integration with the delivery service.
  - `is_presort` (boolean) — `true` if the shipping is presorted.
  - `is_rfbs` (boolean) — `true` if the warehouse works under the rFBS scheme.
  - `recommended_time_local` (string) — Recommended local time of shipping at the drop-off point.
  - `recommended_time_utc_offset_in_minutes` (number(int32)) — Time zone offset of the recommended shipping time from UTC-0 in minutes.
  - `cutoff_at` (string(date-time)) — Date and time by which the shipment must be packed.
  - `mandatory_packaged_count` (integer(int32)) — Number of already packed mandatory shipments.
  - `mandatory_packaged_quantum_count` (integer(int32)) — Number of already packed mandatory quants.
  - `mandatory_postings_count` (integer(int32)) — Number of shipments to be packed.
  - `mandatory_quantum_count` (integer(int32)) — Number of quants to be packed.
  - `optional_packaged_count` (integer(int32)) — Number of already packed optional shipments.
  - `postings_for_another_carriage_count` (integer(int32)) — Number of shipments that can be included in the next shipping.
  - `quantum_for_another_carriage_count` (integer(int32)) — Number of quants that can be included in the next shipping.
  - `timeslot_from` (string(date-time)) — Start of the timeslot at the drop-off point.
  - `timeslot_to` (string(date-time)) — End of the timeslot at the drop-off point.
  - `tpl_provider_icon_url` (string) — Link to the delivery service icon.
  - `tpl_provider_name` (string) — Delivery service name.
  - `warehouse_city` (string) — Warehouse city.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
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

### `POST /v2/carriage/delivery/list`

**List of delivery methods and shippings**

Operation ID: `CarriageDeliveryListV2`

> **Note:** 
  Doesn't return information about delivery methods that don't have shipments.

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `delivery_method_id` (integer(int64)) — Delivery method identifier. For realFBS warehouses, get it using the [/v2/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodListV2) method. 
  - `departure_date` (string) — Shipping date. The default value is the current date.
- `limit` (integer(int64)) **(required)** — Number of values per page.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `has_next` (boolean) — `true` if not all delivery methods are returned in the response.
- `methods` (array[object]) — List of delivery methods.
  - `carriage_postings_count` (integer(int32)) — Number of shipments in all shippings.
  - `carriages` (array[object]) — Shipping list.
  - `cut_in` (string) — Packing start time and warehouse time zone.
  - `cutoff_at` (string) — Date and time by which the shipment must be packed.
  - `delivery_method_id` (integer(int64)) — Delivery method identifier.
  - `delivery_method_name` (string) — Delivery method name.
  - `delivery_method_status` (string) — Delivery method status.
  - `departure_date` (string) — Shipping date.
  - `dropoff_address` (string) — Drop-off point address.
  - `dropoff_change_availability` (string) — Status of the ability to change the drop-off point.
  - `dropoff_point_id` (integer(int64)) — Drop-off point identifier.
  - `dropoff_point_type` (string) — Shipping method.
  - `errors` (array[object]) — List of errors that occurred while processing the request.
  - `first_mile_changing` (boolean) — `true` if the drop-off point changed.
  - `first_mile_type` (string) — First mile type.
  - `has_entrusted_acceptance` (boolean) — `true` if trusted acceptance is enabled at the warehouse.
  - `integration_type` (string) — Type of integration with the delivery service.
  - `is_optional_carriage` (boolean) — `true` if the shipping isn't mandatory.
  - `is_presort` (boolean) — `true` if the shipping is presorted.
  - `is_rfbs` (boolean) — `true` if the warehouse works under the rFBS scheme.
  - `mandatory_packaged_count` (integer(int32)) — Number of already packed mandatory shipments.
  - `mandatory_postings_count` (integer(int32)) — Number of shipments to be packed.
  - `optional_packaged_count` (integer(int32)) — Number of already packed optional shipments.
  - `recommended_time_local` (string) — Recommended local time of shipping to the drop-off point.
  - `recommended_time_utc_offset_in_minutes` (integer(int32)) — Time zone offset of the recommended shipping time from UTC-0 in minutes.
  - `timeslot_from` (string) — Start of the timeslot at the drop-off point.
  - `timeslot_to` (string) — End of the timeslot at the drop-off point.
  - `tpl_provider_icon_url` (string) — Link to the delivery service icon.
  - `tpl_provider_name` (string) — Delivery service name.
  - `warehouse_city` (string) — Warehouse city.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
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

### `POST /v2/posting/fbs/act/create`

**Create an acceptance and transfer certificate and a waybill**

Operation ID: `PostingAPI_PostingFBSActCreate`

Launches the procedure for generating the transfer documents: acceptance and transfer certificate and the waybill.

To generate and receive transfer documents, transfer the shipment to the `awaiting_deliver` status.

**Request body:**

- `containers_count` (integer(int32)) — Number of package units.   Use this parameter  if you have trusted acceptance enabled and ship orders by package units. If you don't have trusted acce
- `delivery_method_id` (integer(int64)) **(required)** — Delivery method identifier. For realFBS warehouses, get it using the [/v2/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodListV2) method. 
- `departure_date` (string(date-time)) — Shipping date.  To make documents printing available before the shipping day, enable **Printing the acceptance certificate in advance** in your person

**Response 200:**

- `result` (object)
  - `id` (integer(int64)) — Number of document generation task.

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

### `POST /v1/posting/carriage-available/list`

**List of available shippings**

Operation ID: `PostingAPI_GetCarriageAvailableList`

> **Note:** 
On March 20, 2026 we disable the method. Switch to the new version [/v2/carriage/delivery/list](#operation/CarriageDeliveryListV2).

Method for getting shipments that require printing acceptance and transfer certificates and a waybill.

**Request body:**

- `delivery_method_id` (integer(int64)) **(required)** — Filter by delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
- `departure_date` (string(date-time)) — Shipping date. The default value is current date.

**Response 200:**

- `result` (object) — Method result.

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

### `POST /v1/carriage/get`

**Shipping details**

Operation ID: `CarriageGet`

**Request body:**

- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

- `act_type` (string) — Acceptance certificate type for FBS sellers.
- `all_blr_traceable` (boolean) — `true` if shipping contains traceable products.
- `is_waybill_enabled` (boolean) — `true` if it's possible to print the waybill.
- `is_econom` (boolean) — `true` if the shipping contains Super Economy products.
- `arrival_pass_ids` (array[string]) — Pass identifiers for the shipping.
- `available_actions` (array[string]) — Available actions with the shipping: - `get_shipping_list`: get the shipping list; - `get_act_of_acceptance`: get the acceptance certificate; - `get_w
- `cancel_availability` (object)
  - `is_cancel_available` (boolean) — `true` if the shipping can be canceled.
  - `reason` (string) — Reason why shipping can't be canceled.
- `carriage_id` (integer(int64)) — Shipping identifier.
- `company_id` (integer(int64)) — Company identifier.
- `containers_count` (integer(int32)) — Number of package units.
- `created_at` (string(date-time)) — Date and time of shipping creation.
- `delivery_method_id` (integer(int64)) — Delivery method identifier.
- `departure_date` (string) — Shipping date.
- `first_mile_type` (string) — First mile type.
- `has_postings_for_next_carriage` (boolean) — `true` if there are shipments subject to shipping that are not in the current shipping.
- `integration_type` (string) — Delivery service integration type.
- `is_container_label_printed` (boolean) — `true` if you already printed shipping labels.
- `is_partial` (boolean) — `true` if the shipping is partial.
- `partial_num` (integer(int64)) — Serial number of the partial shipping.
- `retry_count` (integer(int32)) — The number of retries to create shipping.
- `status` (string) — Shipping status: - `received`: acceptance in progress; - `closed`: closed after acceptance; - `sended`: sent; - `cancelled`: canceled.
- `tpl_provider_id` (integer(int64)) — Delivery method identifier.
- `updated_at` (string(date-time)) — Date and time when the shipping was last updated.
- `warehouse_id` (integer(int64)) — Warehouse identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/fbs/split`

**Split the order into shipments without picking**

Operation ID: `FbsSplit`

**Response 200:**

- `parent_posting` (object)
  - `posting_number` (string) — Original shipment number.
  - `products` (array[object]) — List of products in the shipment.
- `postings` (array[object]) — List of shipments the order is split into.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — List of products in the shipment.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/posting/fbs/act/get-postings`

**List of shipments in the certificate**

Operation ID: `PostingAPI_ActPostingList`

Returns a list of shipments in the certificate by certificate identifier.

**Request body:**

- `id` (int(int64)) **(required)** — Certificate identifier. You can get it using the [/v2/posting/fbs/act/list](#operation/PostingAPI_FbsActList) method. The required value is in the `re

**Response 200:**

- `result` (array[object]) — Information about shipments.
  - `id` (integer(int64)) — Certificate identifier.
  - `multi_box_qty` (integer(int32)) — Number of boxes in which the product is packed.
  - `posting_number` (string) — Shipment number.
  - `status` (string) — Shipment status.
  - `seller_error` (string) — Error code explanation.
  - `updated_at` (string(date-time)) — Shipment record update date and time.
  - `created_at` (string(date-time)) — Shipment record creation date and time.
  - `products` (array[object]) — List of products in the shipment.

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

### `POST /v2/posting/fbs/act/get-container-labels`

**Package unit labels**

Operation ID: `PostingAPI_PostingFBSActGetContainerLabels`

Method creates package unit labels.

**Request body:**

- `id` (integer(int64)) **(required)** — Document generation task number (shipping identifier) received from the [POST /v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) m

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

### `POST /v2/posting/fbs/act/get-barcode`

**Barcode for product shipping**

Operation ID: `PostingAPI_PostingFBSGetBarcode`

Method for getting a barcode to show at a pick-up point or sorting center during shipping.

**Request body:**

- `id` (integer(int64)) **(required)** — Shipping identifier.

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

### `POST /v2/posting/fbs/act/get-barcode/text`

**Value of barcode for product shipping**

Operation ID: `PostingAPI_PostingFBSGetBarcodeText`

Use this method to get the barcode
from the [/v2/posting/fbs/act/get-barcode](#operation/PostingAPI_PostingFBSGetBarcode) response in text format.

**Request body:**

- `id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

- `result` (string) — Barcode in text format.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/posting/fbs/digital/act/check-status`

**Generating status of digital acceptance and transfer certificate and waybill**

Operation ID: `PostingAPI_PostingFBSDigitalActCheckStatus`

> **Note:** 
The method is deprecated and will be disabled on March 22, 2026. Switch to the [/v2/posting/fbs/act/check-status](#operation/PostingAPI_PostingFBSActCheckStatus) method.

The method is available only for sellers who are connected to electronic document circulation.

Get current status of generating digital acceptance and transfer certificate and waybill.

**Request body:**

- `id` (integer(int64)) **(required)** — Document generation task number (shipping identifier) received from the [POST /v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) m

**Response 200:**

- `id` (integer(int64)) — Number of document generation task.
- `status` (string) — Documents generation status: - `FORMING`—in process, - `FORMED`—generated successfully, - `CONFIRMED`—signed by Ozon, - `CONFIRMED_WITH_MISMATCH`—sign

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

### `POST /v2/posting/fbs/act/get-pdf`

**Get acceptance and transfer certificate and waybill**

Operation ID: `PostingAPI_PostingFBSGetAct`

Get the generated transfer documents in PDF format: an acceptance and transfer certificate and a waybill.

If you are not connected to electronic document circulation (EDC), the method returns an acceptance and transfer certificate and a waybill.

If you are connected to EDC, the method returns a waybill only.

Get the list of available documents for the shipping in the `available_actions` parameter of the [/v1/carriage/get](#operation/CarriageGet) method.

**Request body:**

- `id` (integer(int64)) **(required)** — Number of the document generation task (shipping identifier) received in the [/v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) o

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

### `POST /v2/posting/fbs/act/list`

**List of shipping certificates**

Operation ID: `PostingAPI_FbsActList`

Returns a list of shipping certificates allowing to filter them by time period, status, and integration type.

**Request body:**

- `filter` (object)
  - `date_from` (string) **(required)** — Initial date of shipping creation.
  - `date_to` (string) **(required)** — Final date of shipping creation.
  - `integration_type` (string) — Type of integration with the delivery service:  - `ozon`: delivery by the Ozon service;  - `3pl_tracking`: delivery by the integrated service;  - `non
  - `status` (array[string]) — Shipping statuses:   - `new`: new;   - `awaiting_retry`: retry creation;   - `in_process`: is being packaged;   - `success`: created;   - `error`: cre
- `limit` (integer(int64)) **(required)** — Maximum number of certificates in the response.

**Response 200:**

- `result` (array[object]) — Request result.
  - `id` (int(int64)) — Shipping identifier.
  - `delivery_method_id` (int(int64)) — Delivery method identifier.
  - `delivery_method_name` (string) — Delivery method name.
  - `integration_type` (string) — Type of integration with the delivery service:  - `ozon`: delivery by the Ozon service;  - `3pl`: delivery by the integrated service.
  - `containers_count` (int(int32)) — Number of package units.
  - `status` (string) — Shipping status.
  - `departure_date` (string) — Shipping date.
  - `created_at` (string(date-time)) — Shipping record creation date.
  - `updated_at` (string(date-time)) — Shipping record update date.
  - `act_type` (string) — Acceptance certificate type for FBS sellers.
  - `is_partial` (boolean) — `true` if the shipping is partial.  Shipping is partial when it was split into several parts and you need to generate separate acts for each of them.
  - `has_postings_for_next_carriage` (boolean) — `true` if there are shipments subject to shipping that are not in the current shipping.
  - `partial_num` (integer(int64)) — Serial number of the partial shipping.
  - `related_docs` (object)

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

### `POST /v2/posting/fbs/digital/act/get-pdf`

**Get digital shipping certificate**

Operation ID: `PostingAPI_PostingFBSGetDigitalAct`

> **Note:** 
The method is deprecated and will be disabled on March 22, 2026. Switch to the [/v2/posting/fbs/act/get-pdf](#operation/PostingAPI_PostingFBSGetAct) method.
The method is available only for sellers who are connected to electronic document circulation.

You can get digital documents, if the [/v2/posting/fbs/digital/act/check-status](#operation/PostingAPI_PostingFBSDigitalActCheckStatus) method returned one of the following statuses:
- `FORMED`: generated successfully;
- `CONFIRMED`: signed by Ozon;
- `CONFIRMED_WITH_MISMATCH`: signed by Ozon with mismatches.

**Request body:**

- `id` (integer(int64)) **(required)** — Document generation task number (shipping identifier) received from the [POST /v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) m
- `doc_type` (string) — Type of shipment certificate: - `act_of_acceptance`: acceptance certificate, - `act_of_mismatch`: discrepancy certificate, - `act_of_excess`: surplus 

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

### `POST /v2/posting/fbs/act/check-status`

**Status of acceptance and transfer certificate and waybill**

Operation ID: `PostingAPI_PostingFBSActCheckStatus`

If you are not connected to electronic document circulation (EDC), the method returns status of generating an acceptance and transfer certificate and a waybill.

If you are connected to EDC, the method returns status of generating a waybill only.

**Request body:**

- `id` (integer(int64)) **(required)** — Document generation task number (shipping identifier) received from the [POST /v2/posting/fbs/act/create](#operation/PostingAPI_PostingFBSActCreate) m

**Response 200:**

- `result` (object)
  - `act_type` (string) — Document type.
  - `added_to_act` (array[string]) — List with numbers of shipments that are included in the acceptance and transfer certificate. You should hand these shipments over today.
  - `removed_from_act` (array[string]) — List with numbers of shipments that are not included in the acceptance and transfer certificate. You should hand these shipments over in the next ship
  - `status` (string) — Request status:  - `in_process`—documents generation in process, please wait.  - `ready`—documents are ready for downloading.  - `error`—error occured
  - `is_partial` (boolean) — Indication of a partial shipping. `true` if the shipping is partial.  Shipping is partial when it was split into several parts and you need to generat
  - `has_postings_for_next_carriage` (boolean) — `true` if there are shipments subject to shipping that are not in the current shipping.  If there are such shipments, create a new acceptance and tran
  - `partial_num` (integer(int64)) — Serial number of the partial shipping.

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

### `POST /v1/posting/fbs/traceable/split`

**Split shipment with traceable products**

Operation ID: `PostingFbsTraceableSplit`

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `postings` (array[object]) — Shipment details.
  - `posting_number` (string) — Shipment number.
  - `potential_blr_traceable` (boolean) — Indication that the product is potentially traceable:   - `true`: shipment is considered traceable at the moment. The status may change during packing
  - `products` (array[object]) — List of products in the shipment.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/fbs/product/traceable/attribute`

**Get list of empty attributes for traceable products**

Operation ID: `PostingFbsProductTraceableAttribute`

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `products` (array[object]) — List of products in the shipment.
  - `required_attributes` (array[string]) — Required attributes.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/carriage/ettn/status`

**Get electronic waybill verification status for traceable FBS shipping**

Operation ID: `CarriageEttnStatus`

**Request body:**

- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

- `errors` (array[string]) — Errors in checking the electronic waybill of traceable shipping.
- `status` (enum) — Values: `NOT_UPLOADED, PROCESSING, SUCCESS, FAILED`. Status of electronic waybill of traceable shipping.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/assembly/carriage/posting/list`

**Get list of shipments in shipping**

Operation ID: `AssemblyCarriagePostingList`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `carriage_id` (integer(int64)) **(required)** — Shipping identifier.
  - `cutoff_from` (string(date-time)) — Filter by the time by which the seller should pack the order. Period start.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `cutoff_to` (string(date-time)) — Filter by the time by which the seller should pack the order. Period end.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `delivery_method_id` (integer(int64)) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
- `limit` (integer(int64)) **(required)** — Number of values per page.

**Response 200:**

- `can_print_mass_label` (boolean) — `true` if you can print labels in bulky.
- `cursor` (string) — Cursor for the next data sample. If the parameter is empty, there is no more data.
- `postings` (array[object]) — Shipment list.
  - `assembly_code` (string) — Product list code.
  - `can_print_label` (boolean) — `true` if you can print label.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — Product list.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/assembly/carriage/product/list`

**Get list of products in shipping**

Operation ID: `AssemblyCarriageProductList`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `carriage_id` (integer(int64)) **(required)** — Shipping identifier.
  - `cutoff_from` (string(date-time)) — Filter by the time by which the seller should pack the order. Period start.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `cutoff_to` (string(date-time)) — Filter by the time by which the seller should pack the order. Period end.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `delivery_method_id` (integer(int64)) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
- `limit` (integer(int64)) **(required)** — Number of values per page.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample. If the parameter is empty, there is no more data.
- `products` (array[object]) — Product list.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `picture_url` (string) — Link to product image.
  - `posting_numbers` (array[string]) — Shipment numbers.
  - `product_name` (string) — Product name.
  - `quantity` (integer(int64)) — Quantity of product items.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/assembly/fbs/posting/list`

**Get shipment list**

Operation ID: `AssemblyFbsPostingList`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `cutoff_from` (string(date-time)) **(required)** — Filter by the time by which the seller should pack the order. Period start.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `cutoff_to` (string(date-time)) **(required)** — Filter by the time by which the seller should pack the order. Period end.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `delivery_method_id` (integer(int64)) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
- `limit` (integer(int64)) **(required)** — Number of values per page.
- `sort_dir` (enum) **(required)** — Values: `ASC, DESC`

**Response 200:**

- `cursor` (string) — Cursor for the next data sample. If the parameter is empty, there is no more data.
- `cutoff` (string(date-time)) — Time before an order must be packed.
- `postings` (array[object]) — Shipment list.
  - `assembly_code` (string) — Product list code.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — Product list.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/assembly/fbs/product/list`

**Get list of products in shipments**

Operation ID: `AssemblyFbsProductList`

**Request body:**

- `filter` (object) **(required)**
  - `cutoff_from` (string(date-time)) **(required)** — Filter by the time by which the seller should pack the order. Period start.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `cutoff_to` (string(date-time)) **(required)** — Filter by the time by which the seller should pack the order. Period end.  Format: `YYYY-MM-DDThh:mm:ss.mcsZ`. Example: `2020-03-18T07:34:50.359Z`.
  - `delivery_method_id` (integer(int64)) — Delivery method identifier. You can get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
- `limit` (integer(int64)) **(required)** — Number of values per page.
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `sort_dir` (enum) — Values: `ASC, DESC`

**Response 200:**

- `has_next` (boolean) — Indicates that the response returned not all products:  - `true`: make a new request with a different `offset` value to get the remaining values; - `f
- `products` (array[object]) — Product list.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `picture_url` (string) — Link to product image.
  - `postings` (array[object]) — Shipment list.
  - `product_name` (string) — Product name.
  - `quantity` (integer(int32)) — Quantity of product items.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
- `products_count` (integer(int32)) — Number of products.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## DeliveryrFBS

### `POST /v2/fbs/posting/tracking-number/set`

**Add tracking numbers**

Operation ID: `PostingAPI_FbsPostingTrackingNumberSet`

Add tracking numbers to shipments. You can add up to 20 tracking numbers at a time.

**Request body:**

- `tracking_numbers` (array[object]) **(required)** — An array with shipment identifier—tracking number pairs.
  - `posting_number` (string) **(required)** — Shipment identifier.
  - `tracking_number` (string) **(required)** — Shipment tracking number.

**Response 200:**

- `result` (array[object]) — Method result.
  - `error` (string) — Error when processing the request.
  - `posting_number` (string) — Shipment number.
  - `result` (boolean) — If the request is executed without errors—`true`.

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

### `POST /v2/fbs/posting/delivering`

**Change the status to "Delivering"**

Operation ID: `PostingAPI_FbsPostingDelivering`

> **Note:** Before changing the status, check the current shipment status using the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method. The status change is asynchronous.

Changes the shipment status to "Delivering" if a third-party delivery service is being used.

**Request body:**

- `posting_number` (array[string]) **(required)** — Shipment identifier.

**Response 200:**

- `result` (array[object]) — Method result.
  - `error` (string) — Error when processing the request.
  - `posting_number` (string) — Shipment number.
  - `result` (boolean) — If the request is executed without errors—`true`.

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

### `POST /v2/fbs/posting/last-mile`

**Change the status to "Last Mile"**

Operation ID: `PostingAPI_FbsPostingLastMile`

> **Note:** Before changing the status, check the current shipment status using the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method. The status change is asynchronous.

Changes the shipment status to "Last mile" if a third-party delivery service is being used.

**Request body:**

- `posting_number` (array[string]) **(required)** — Shipment identifier.

**Response 200:**

- `result` (array[object]) — Method result.
  - `error` (string) — Error when processing the request.
  - `posting_number` (string) — Shipment number.
  - `result` (boolean) — If the request is executed without errors—`true`.

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

### `POST /v2/fbs/posting/delivered`

**Change the status to "Delivered"**

Operation ID: `PostingAPI_FbsPostingDelivered`

> **Note:** Before changing the status, check the current shipment status using the [/v3/posting/fbs/get](#operation/PostingAPI_GetFbsPostingV3) method. The status change is asynchronous.

Changes the shipment status to "Delivered" if a third-party delivery service is being used.

**Request body:**

- `posting_number` (array[string]) **(required)** — Shipment identifier.

**Response 200:**

- `result` (array[object]) — Method result.
  - `error` (string) — Error when processing the request.
  - `posting_number` (string) — Shipment number.
  - `result` (boolean) — If the request is executed without errors—`true`.

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

### `POST /v1/posting/fbs/timeslot/change-restrictions`

**Dates available for delivery reschedule**

Operation ID: `PostingAPI_PostingTimeslotChangeRestrictions`

Method for getting the dates and number of times available for delivery reschedule.

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `delivery_interval` (object)
  - `begin` (string(date-time)) — Period start date.   Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.
  - `end` (string(date-time)) — Period end date.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.
- `remaining_changes_count` (integer(int64)) — Number of delivery date reschedules left.

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

### `POST /v1/posting/fbs/timeslot/set`

**Reschedule shipment delivery date**

Operation ID: `PostingAPI_SetPostingTimeslot`

You can change the delivery date of a shipment up to two times.

**Request body:**

- `new_timeslot` (object) **(required)**
  - `from` (string(date-time)) **(required)** — Period start date.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.
  - `to` (string(date-time)) **(required)** — Period end date.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.
- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `result` (boolean) — `true`, if the date was changed.

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

### `POST /v1/posting/cutoff/set`

**Specify shipping date**

Operation ID: `PostingAPI_SetPostingCutoff`

Method for shipments delivered by the seller or a non-integrated shipping provider.

**Request body:**

- `new_cutoff_date` (string(date-time)) **(required)** — New shipping date.
- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `result` (boolean) — `true` if the new date is set.

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

## Pass

### `POST /v1/pass/list`

**List of passes**

Operation ID: `PassList`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object)
  - `arrival_pass_ids` (array[string]) — Filter by pass identifier.
  - `arrival_reason` (string) — Filter by purpose of arrival: - `FBS_DELIVERY`—delivery. - `FBS_RETURN`—take out returns.  If the parameter isn't specified, both purposes are conside
  - `dropoff_point_ids` (array[string]) — Filter by drop-off points identifier.
  - `only_active_passes` (boolean) — `true` to get only active pass requests.
  - `warehouse_ids` (array[string]) — Filter by seller's warehouses identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
- `limit` (integer(int32)) **(required)** — Limit on number of entries in a reply. Default value is 1000. Maximum value is 1000.

**Response 200:**

- `arrival_passes` (array[object]) — List of passes.
  - `arrival_pass_id` (integer(int64)) — Pass identifier.
  - `arrival_reasons` (array[string]) — Arrival purpose.
  - `arrival_time` (string(date-time)) — Date and time of arrival in UTC format.
  - `driver_name` (string) — Driver full name.
  - `driver_phone` (string) — Driver phone number.
  - `dropoff_point_id` (integer(int64)) — Drop-off point identifier.
  - `is_active` (boolean) — `true` if the request is active.
  - `vehicle_license_plate` (string) — Car license plate.
  - `vehicle_model` (string) — Car model.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
- `cursor` (string) — Cursor for the next data sample. If the parameter is empty, there is no more data.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/carriage/pass/create`

**Create a pass**

Operation ID: `carriagePassCreate`

The identifier of the created pass will be added to the shipment.

**Request body:**

- `arrival_passes` (array[object]) **(required)** — List of passes.
  - `driver_name` (string) **(required)** — Driver full name.
  - `driver_phone` (string) **(required)** — Driver phone number.
  - `vehicle_license_plate` (string) **(required)** — Car license plate.
  - `vehicle_model` (string) **(required)** — Car model.
  - `with_returns` (boolean) — `true` if you will export returns. Default is `false`.
- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

- `arrival_pass_ids` (array[string]) — Pass identifiers.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/carriage/pass/update`

**Update pass**

Operation ID: `carriagePassUpdate`

**Request body:**

- `arrival_passes` (array[object]) **(required)** — List of passes.
  - `driver_name` (string) **(required)** — Driver full name.
  - `driver_phone` (string) **(required)** — Driver phone number.
  - `id` (integer(int64)) **(required)** — Pass identifier.
  - `vehicle_license_plate` (string) **(required)** — Car license plate.
  - `vehicle_model` (string) **(required)** — Car model.
  - `with_returns` (boolean) — `true` if you will export returns. Default is `false`.
- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/carriage/pass/delete`

**Delete pass**

Operation ID: `carriagePassDelete`

**Request body:**

- `arrival_pass_ids` (array[string]) **(required)** — Pass identifiers.
- `carriage_id` (integer(int64)) **(required)** — Shipping identifier.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/pass/create`

**Create a return pass**

Operation ID: `returnPassCreate`

**Request body:**

- `arrival_passes` (array[object]) **(required)** — Array of passes.
  - `arrival_time` (string(date-time)) **(required)** — Date and time of arrival in UTC format. At that time, the pass will become valid.
  - `driver_name` (string) **(required)** — Driver full name.
  - `driver_phone` (string) **(required)** — Driver phone number.
  - `dropoff_point_id` (integer(int64)) **(required)** — Drop-off point identifier for which the pass is issued.
  - `vehicle_license_plate` (string) **(required)** — Car license plate.
  - `vehicle_model` (string) **(required)** — Car model.
  - `warehouse_id` (integer(int64)) **(required)** — Warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.

**Response 200:**

- `arrival_pass_ids` (array[string]) — Pass identifiers.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/pass/update`

**Update return pass**

Operation ID: `returnPassUpdate`

**Request body:**

- `arrival_passes` (array[object]) **(required)** — Array of passes.
  - `arrival_pass_id` (integer(int64)) **(required)** — Pass identifier.
  - `arrival_time` (string(date-time)) **(required)** — Date and time of arrival in UTC format. At this time the pass will begin to be valid.  To change your arrival time, use the [/v1/carriage/pass/update]
  - `driver_name` (string) **(required)** — Driver full name.
  - `driver_phone` (string) **(required)** — Driver phone number.
  - `vehicle_license_plate` (string) **(required)** — Car license plate.
  - `vehicle_model` (string) **(required)** — Car model.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/pass/delete`

**Delete return pass**

Operation ID: `returnPassDelete`

**Request body:**

- `arrival_pass_ids` (array[string]) **(required)** — Pass identifiers.

**Response 200:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## ReturnsAPI

### `POST /v1/returns/list`

**Information about FBO and FBS returns**

Operation ID: `returnsList`

A method to retrieve information about FBO and FBS returns.

**Request body:**

- `filter` (object)
  - `logistic_return_date` (object)
    - `time_from` (string(date-time)) — Beginning of the period.
    - `time_to` (string(date-time)) — End of the period.
  - `storage_tariffication_start_date` (object)
    - `time_from` (string(date-time)) — Beginning of the period.
    - `time_to` (string(date-time)) — End of period.
  - `visual_status_change_moment` (object)
    - `time_from` (string(date-time)) — Beginning of the period.
    - `time_to` (string(date-time)) — End of period.
  - `order_id` (integer(int64)) — Filter by order identifier.
  - `posting_numbers` (array[string]) — Filter by shipment number. Pass no more than 50 postings.
  - `product_name` (string) — Filter by product name.
  - `offer_id` (string) — Filter by product identifier in the seller's system.
  - `visual_status_name` (string) — Filter by return status: - `DisputeOpened`: dispute with the customer has been opened; - `OnSellerApproval`: pending with the seller; - `ArrivedAtRetu
  - `warehouse_id` (integer(int64)) — Filter by warehouse identifier. You can get it using the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.
  - `barcode` (string) — Filter by return label barcode.
  - `return_schema` (string) — Filter by delivery scheme: `FBS` or `FBO`.
  - `compensation_status_id` (integer(int32)) — Filter by compensation status: - `1`: sent; - `2`: received; - `3`: canceled; - `4`: decompensation done.
- `limit` (integer(int32)) **(required)** — Number of loaded returns. The maximum value is 500.
- `last_id` (integer(int64)) — Identifier of the last loaded return.

**Response 200:**

- `returns` (array[object]) — Returns details.
  - `exemplars` (array[object]) — Product items data.
  - `id` (integer(int64)) — Return identifier.
  - `company_id` (integer(int64)) — Company identifier.
  - `return_reason_name` (string) — Return reason.
  - `type` (string) — Return type: `Cancellation` - cancellation (before shipping); `FullReturn` - full return on shipping; `PartialReturn` - partial return on shipping; `C
  - `schema` (string) — Return scheme: `FBS`; `FBO`.
  - `order_id` (integer(int64)) — Order identifier.
  - `order_number` (string) — Order number.
  - `place` (object)
  - `target_place` (object)
  - `storage` (object)
  - `product` (object)
  - `logistic` (object)
  - `visual` (object)
  - `additional_info` (object)
  - `source_id` (integer(int64)) — Previous return identifier.
  - `posting_number` (string) — Shipment number.
  - `clearing_id` (integer(int64)) — Original shipment barcode.
  - `return_clearing_id` (integer(int64)) — Package unit identifier in the Ozon logistics system.
  - `compensation_status` (object)
- `has_next` (boolean) — `true`, if the seller has other returns.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## ReturnAPI

### `POST /v1/returns/company/fbs/info`

**FBS returns quantity**

Operation ID: `returnsCompanyFBSInfo`

The method to get information about FBS returns and their quantity.

**Request body:**

- `filter` (object)
  - `place_id` (integer(int64)) — Filter by drop-off point identifier.
- `pagination` (object) **(required)**
  - `last_id` (integer(int64)) — Identifier of the last drop-off point on the page. Leave this field blank in the first request.  To get the next values, specify `id` of the last drop
  - `limit` (integer(int32)) **(required)** — Number of drop-off points per page. Maximum is 500.

**Response 200:**

- `drop_off_points` (array[object]) — Information about drop-off points.
  - `address` (string) — Drop-off point address.
  - `box_count` (integer(int32)) — Number of boxes in drop-off point.
  - `id` (integer(int64)) — Drop-off point identifier.
  - `name` (string) — Drop-off point name.
  - `pass_info` (object)
  - `place_id` (integer(int64)) — The warehouse identifier to which the shipment will arrive.
  - `returns_count` (integer(int32)) — Quantity of returns at the drop-off point.
  - `utc_offset` (string) — Time zone offset of the shipping time from UTC-0.
  - `warehouses_ids` (array[string]) — Seller's warehouses identifiers.
- `has_next` (boolean) — `true` if there are any other points where sellers have orders waiting.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/is-enabled`

**Check the ability to receive return shipments by barcode**

Operation ID: `ReturnAPI_GiveoutIsEnabled`

The `enabled` parameter is `true` if you can pick up return shipments by barcode.

**Request body:**

**Response 200:**

- `enabled` (boolean) — `true` if you can pick up a return shipment by barcode.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/list`

**Return shipments list**

Operation ID: `ReturnAPI_GiveoutList`

Method for getting a list of active returns.
A return shipment becomes active after scan a barcode. 
After the barcode is scanned for a second time, the status of an active return shipment switches to inactive.

**Request body:**

- `last_id` (integer(int64)) — Identifier of the last value on the page.
- `limit` (integer(int64)) **(required)** — Number of values in the response.

**Response 200:**

- `giveouts` (array[object]) — Shipment identifier.
  - `approved_articles_count` (integer(int32)) — Number of products in shipment.
  - `created_at` (string(date-time)) — Creation date and time.
  - `giveout_id` (integer(int64)) — Shipment identifier.
  - `giveout_status` (object)
  - `total_articles_count` (integer(int32)) — Total number of products to be picked up from the warehouse.
  - `warehouse_address` (string) — Warehouse address.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
  - `warehouse_name` (string) — Warehouse name.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/info`

**Information on return shipment**

Operation ID: `ReturnAPI_GiveoutInfo`

Method for getting information about return shipment. 
The value received in the [/v1/return/giveout/list](#operation/ReturnAPI_GiveoutList) method is passed to the `giveout_id` parameter.

**Request body:**

- `giveout_id` (integer(int64)) **(required)** — Shipment identifier.

**Response 200:**

- `articles` (array[object]) — Product IDs.
  - `approved` (boolean) — `true` if the shipment is confirmed.
  - `delivery_schema` (object)
  - `name` (string) — Product name.
  - `seller_id` (integer(int64)) — Seller identifier.
- `giveout_id` (integer(int64)) — Shipment identifier.
- `giveout_status` (object)
- `warehouse_address` (string) — Warehouse address.
- `warehouse_name` (string) — Warehouse name.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/barcode`

**Value of barcode for return shipments**

Operation ID: `ReturnAPI_GiveoutGetBarcode`

Use this method to get the barcode from the response of the [/v1/return/giveout/get-png](#operation/ReturnAPI_GiveoutGetPNG) and [/v1/return/giveout/get-pdf](#operation/ReturnAPI_GiveoutGetPDF) methods in text format.

**Request body:**

**Response 200:**

- `barcode` (string) — Barcode value in text format.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/get-pdf`

**Barcode for return shipment in PDF format**

Operation ID: `ReturnAPI_GiveoutGetPDF`

Returns a PDF file with a barcode. The method works only for the FBS scheme.

**Request body:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/get-png`

**Barcode for return shipment in PNG format**

Operation ID: `ReturnAPI_GiveoutGetPNG`

Returns a PNG file with a barcode.

**Request body:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/return/giveout/barcode-reset`

**Generate new barcode**

Operation ID: `ReturnAPI_GiveoutBarcodeReset`

Use this method if an unauthorized person has gained access to your barcode.

The method returns a PNG file with the new barcode. Once the method is used, you won't be able to get a return shipment using the old barcodes.
To get a new barcode in PDF format, use the [/v1/return/giveout/get-pdf](#operation/ReturnAPI_GiveoutGetPDF) method.

**Request body:**

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## RFBSReturnsAPI

### `POST /v2/returns/rfbs/list`

**Get a list of return requests**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsListV2`

**Request body:**

- `filter` (object)
  - `offer_id` (string) — Product identifier in the seller's system.
  - `posting_number` (string) — Shipment number.
  - `group_state` (array[string]) — Filter by request statuses: - `All`—all requests. - `New`—new. - `Delivering`—returned product is on the way for check. - `Checkout`— returned product
  - `created_at` (object)
    - `from` (string(date-time)) — Period start date.
    - `to` (string(date-time)) — Period end date.
- `last_id` (integer(int32)) — Identifier of the last value on the page: `return_id`. Leave this field blank in the first request.
- `limit` (integer(int32)) **(required)** — Number of values per page.

**Response 200:**

- `returns` (object)
  - `client_name` (string) — Customer name.
  - `created_at` (string(date-time)) — Request creation date.
  - `order_number` (string) — Order number.
  - `posting_number` (string) — Shipment number.
  - `product` (object)
  - `return_id` (integer(int64)) — Return request identifier.
  - `return_number` (string) — Return request number.
  - `state` (object)

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

### `POST /v2/returns/rfbs/get`

**Get information about a return request**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsGetV2`

**Request body:**

- `return_id` (integer(int64)) **(required)** — Return request identifier. You can get it using the [/v2/returns/rfbs/list](#operation/RFBSReturnsAPI_ReturnsRfbsListV2) method.

**Response 200:**

- `returns` (object)
  - `available_actions` (array[object]) — List of available actions on the request.
  - `client_name` (string) — Customer name.
  - `client_photo` (array[string]) — Links to product images.
  - `client_return_method_type` (object)
  - `comment` (string) — Customer comment.
  - `created_at` (string(date-time)) — Request creation date.
  - `order_number` (string) — Order number.
  - `posting_number` (string) — Shipment number.
  - `product` (object)
  - `rejection_comment` (string) — Comment on request rejection.
  - `rejection_reason` (array[object]) — Information on rejection reason.
  - `return_method_description` (string) — Method of product return.
  - `return_number` (string) — Return request number.
  - `return_reason` (object)
  - `ru_post_tracking_number` (string) — Postal tracking number.
  - `state` (object)
  - `warehouse_id` (integer(int64)) — Warehouse identifier.

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

### `POST /v2/returns/rfbs/reject`

**Reject a return request**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsRejectV2`

> **Note:** 
This method will be disabled. Switch to the [/v1/returns/rfbs/action/set](#operation/ReturnsAPI_ReturnsRfbsActionSet) method.

The method rejects an rFBS return request. Explain your decision in the `comment` parameter.

**Request body:**

- `return_id` (integer(int64)) **(required)** — Return request identifier.
- `comment` (string) — Comment.  The comment is required if the `rejection_reason.is_comment_required` parameter is `true` in the response of the [/v2/returns/rfbs/get](#ope
- `rejection_reason_id` (integer(int64)) **(required)** — Rejection reason identifier.  Pass the value from the list of reasons received in the response of the [/v2/returns/rfbs/get](#operation/RFBSReturnsAPI

**Response 200:**

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

### `POST /v2/returns/rfbs/compensate`

**Compensate partial cost**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsCompensateV2`

> **Note:** 
This method will be disabled. Switch to the [/v1/returns/rfbs/action/set](#operation/ReturnsAPI_ReturnsRfbsActionSet) method.

Using this method you can confirm the partial compensation and agree to keep the product with the customer.

**Request body:**

- `compensation_amount` (string) — Compensation amount.
- `return_id` (integer(int64)) **(required)** — Return request identifier.

**Response 200:**

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

### `POST /v2/returns/rfbs/verify`

**Approve a return request**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsVerifyV2`

> **Note:** 
This method will be disabled. Switch to the [/v1/returns/rfbs/action/set](#operation/ReturnsAPI_ReturnsRfbsActionSet) method.

The method allows to approve an rFBS return request and agree to receive products for verification.

Confirm that you've received the product using the [/v2/returns/rfbs/receive-return](#operation/RFBSReturnsAPI_ReturnsRfbsReceiveReturnV2) method.

**Request body:**

- `return_id` (integer(int64)) **(required)** — Return request identifier.
- `return_method_description` (string) — Method of product return.

**Response 200:**

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

### `POST /v2/returns/rfbs/receive-return`

**Confirm receipt of a product for check**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsReceiveReturnV2`

> **Note:** 
  This method will be disabled. Switch to the [/v1/returns/rfbs/action/set](#operation/ReturnsAPI_ReturnsRfbsActionSet) method.

**Request body:**

- `return_id` (integer(int64)) **(required)** — Return request identifier.

**Response 200:**

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

### `POST /v2/returns/rfbs/return-money`

**Refund the customer**

Operation ID: `RFBSReturnsAPI_ReturnsRfbsReturnMoneyV2`

> **Note:** 
This method will be disabled. Switch to the [/v1/returns/rfbs/action/set](#operation/ReturnsAPI_ReturnsRfbsActionSet) method.

The method confirms the refund of the full product cost.
Use the method if you agree to refund the customer:
  - Immediately without receiving the product.
  - After you received and checked the product. If the product is defective or damaged, you also refund its return shipment cost.

**Request body:**

- `return_id` (integer(int64)) **(required)** — Return request identifier.
- `return_for_back_way` (integer(int64)) — Refund amount for shipping the product.

**Response 200:**

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

### `POST /v1/returns/rfbs/action/set`

**Pass available actions for rFBS returns**

Operation ID: `ReturnsAPI_ReturnsRfbsActionSet`

Method for passing actions for rFBS return.

**Request body:**

- `comment` (string) — Seller comment.  Required for `id: -1` and `id: -10`.
- `compensation_amount` (number(double)) — Compensation amount.  Required for `id: 1020`.
- `id` (integer(int32)) — Action identifier.   Get available actions via the `returns.available_actions` parameter using the [/v2/returns/rfbs/get](#operation/RFBSReturnsAPI_Re
- `rejection_reason_id` (integer(int32)) — Rejection reason identifier.  Required for `id: -1` and `id: -10`.  Get available rejection reasons via the `returns.rejection_reason` parameter using
- `return_for_back_way` (number(double)) — Amount refunded to the customer for shipping the product.  Negative values are treated as `0`.
- `return_id` (integer(int64)) **(required)** — Return request identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## CancellationAPI

### `POST /v2/conditional-cancellation/list`

**Get a list of rFBS cancellation requests**

Operation ID: `CancellationAPI_GetConditionalCancellationListV2`

Method for getting a list of rFBS cancellation requests.

**Request body:**

- `filters` (object)
  - `cancellation_initiator` (array[object]) — Cancellation initiator: - `SELLER`—seller, - `CLIENT`—customer, - `OZON`—Ozon,  - `SYSTEM`—system, - `DELIVERY`—shipping provider.
  - `posting_number` (array[string]) — Filter by shipment number.
  - `state` (enum) — Values: `ALL, ON_APPROVAL, APPROVED, REJECTED`. Filter by cancellation request status: - `ALL`—any status requests, - `ON_APPROVAL`—requests under review, - `APPROVED`—approved requests, - `REJECTED
- `last_id` (integer(int64)) — Identifier of the last value on the page. Leave this field blank in the first request.  To get the next values, specify `last_id` from the response of
- `limit` (integer(int32)) **(required)** — Number of cancellation requests in the response.
- `with` (object)
  - `counter` (boolean) — Indication that the counter of requests in the `ON_APPROVAL` status should be displayed in the response.

**Response 200:**

- `counter` (integer(int64)) — Counter of requests in the `ON_APPROVAL` status.
- `last_id` (integer(int64)) — Identifier of the last value on the page.  To get the next values, specify `last_id` from the response of the previous request.
- `result` (array[object]) — Information of cancellation requests.
  - `approve_comment` (string) — Comment submitted on the cancellation request approval or rejection.
  - `approve_date` (string(date-time)) — Cancellation request approval or rejection date.
  - `auto_approve_date` (string(date-time)) — Date after which the request will be automatically approved.
  - `cancellation_id` (integer(int64)) — Cancellation request identifier.
  - `cancellation_initiator` (enum) — Values: `OZON, SELLER, CLIENT, SYSTEM, DELIVERY`
  - `cancellation_reason` (object)
  - `cancellation_reason_message` (string) — Comment to cancellation request entered by cancellation initiator manually.
  - `cancelled_at` (string(date-time)) — Cancellation request creation date.
  - `order_date` (string(date-time)) — Order creation date.
  - `posting_number` (string) — Shipment number.
  - `source_id` (integer(int64)) — Previous cancellation request identifier.  Used to maintain backward compatibility.
  - `state` (object)
  - `tpl_integration_type` (string) — Type of integration with the delivery service.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/conditional-cancellation/approve`

**Approve rFBS cancellation request**

Operation ID: `CancellationAPI_ConditionalCancellationApproveV2`

Use the method to approve rFBS cancellation requests in the `ON_APPROVAL` status. The order will be canceled and the money will be returned to the customer.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/conditional-cancellation/reject`

**Reject rFBS cancellation request**

Operation ID: `CancellationAPI_ConditionalCancellationRejectV2`

Use the method to reject rFBS cancellation requests in the `ON_APPROVAL` status. Provide a reason in the `comment` parameter. The order will remain in the same status and should be delivered to the customer.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## ChatAPI

More methods in the [**Premium Methods**](#tag/Premium) section.

### `POST /v1/chat/send/file`

**Send file**

Operation ID: `ChatAPI_ChatSendFile`

Sends a file to an existing chat by its identifier.

Only sellers with the [Premium Plus](https://docs.ozon.ru/global/en/promotion/subscriptions) or Premium Pro subscription can send files to chats with customers.

**Request body:**

- `base64_content` (string) **(required)** — File as a base64 string.
- `chat_id` (string) **(required)** — Chat identifier.
- `name` (string) **(required)** — File name with extension.

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

### `POST /v2/chat/list`

**Chats list**

Operation ID: `ChatAPI_ChatListV2`

Returns information about chats by specified filters. 
> **Note:**  This method will be disabled. Switch to the [/v3/chat/list](#operation/ChatAPI_ChatListV3) method.

**Request body:**

- `filter` (object)
  - `chat_status` (string) — Filter by chat status: - `All`, - `Opened`, - `Closed`.  Default value is `All`.
  - `unread_only` (boolean) — Filter by chats with unread messages.
- `limit` (integer(int64)) **(required)** — Number of values in the response. The default value is 30. The maximum value is 100.
- `cursor` (string) — Cursor for the next data sample.

**Response 200:**

- `chats` (object) — Chats data.
- `total_chats_count` (integer(int64)) — Total number of chats.
- `total_unread_count` (integer(int64)) — Total number of unread messages.

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

### `POST /v3/chat/list`

**Chats list**

Operation ID: `ChatAPI_ChatListV3`

Returns information about chats by specified filters.

**Request body:**

- `filter` (object)
  - `chat_status` (string) — Filter by chat status: - `All`, - `Opened`, - `Closed`.  Default value is `All`.
  - `unread_only` (boolean) — Filter by chats with unread messages.
- `limit` (integer(int64)) **(required)** — Number of values in the response. The default value is 30. The maximum value is 100.
- `cursor` (string) — Cursor for the next data sample.

**Response 200:**

- `chats` (object) — Chats data.
- `total_unread_count` (integer(int64)) — Total number of unread messages.
- `cursor` (string) — Cursor for the next data sample.
- `has_next` (boolean) — Indicates that the response does not contain all chats:   - `true`: send another request with a new cursor value to get the remaining chats. - `false`

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

### `POST /v3/chat/history`

**Chat history**

Operation ID: `ChatAPI_ChatHistoryV3`

Returns the history of chat messages. By default messages are shown from newest to oldest.

**Request body:**

- `chat_id` (string) **(required)** — Chat identifier.
- `direction` (string) — Direction of message sorting: - `Forward`: from old to new. - `Backward`: from new to old.  The default value is `Backward`. You can set the number of
- `filter` (object)
  - `message_ids` (array[string]) — Messages identifiers.
- `from_message_id` (integer(uint64)) — Identifier of the message from which the chat history is displayed. Default value is the last visible message.  The `from_message_id` parameter is req
- `limit` (integer(int64)) — Number of messages in the response. The default value is 50. The maximum value is 1000.

**Response 200:**

- `has_next` (boolean) — `true`, if not all messages were returned in the response.
- `messages` (array[object]) — Array of messages sorted according to the `direction` parameter in the request body.
  - `context` (object)
  - `created_at` (string(date-time)) — Message creation date.
  - `data` (array[string]) — Array with message content in Markdown format.
  - `is_image` (boolean) — `true`, if the message has an image.
  - `is_read` (boolean) — `true`, if the message is read.
  - `message_id` (integer(uint64)) — Message identifier.
  - `moderate_image_status` (enum) — Values: `SUCCESS, MODERATION, FAILED`. Image moderation status.
  - `user` (object)

**Response 400:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## SupplierAPI

### `POST /v2/invoice/create-or-update`

**Create or edit an invoice**

Operation ID: `InvoiceAPI_InvoiceCreateOrUpdateV2`

Create or edit an invoice for VAT refund to Turkey sellers.

**Request body:**

- `date` (string(date-time)) **(required)** — Invoice date.
- `hs_codes` (array[object]) — Product HS-codes.
  - `code` (string) — Product HS code.
  - `sku` (string) — Product identifier in the Ozon system, SKU.
- `number` (string) — Invoice number. The number can contain letters and digits, maximum length is 50 characters.
- `posting_number` (string) **(required)** — Shipment number.
- `price` (number(double)) — Cost stated in the invoice. The fractional part is separated by decimal point, up to two digits after the decimal point.
- `price_currency` (string) — Invoice currency: - `USD`—dollar,  - `EUR`—euro,  - `TRY`—Turkish lira,  - `CNY`—yuan,  - `RUB`—ruble,  - `GBP`—pound sterling.  Default value is `USD
- `url` (string) **(required)** — Invoice link. Use the [v1/invoice/file/upload](#operation/invoice_upload) method to create a link.

**Response 200:**

- `result` (boolean) — Method result.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/invoice/file/upload`

**Invoice upload**

Operation ID: `invoice_upload`

Available file types: JPEG and PDF. Maximum file size: 10 MB.

**Request body:**

- `base64_content` (string) **(required)** — Base64 encoded invoice.
- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `url` (string) — Link to invoice.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v2/invoice/get`

**Get invoice information**

Operation ID: `invoice_getV2`

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `result` (object)
  - `date` (string(date-time)) — Invoice uploading date
  - `file_url` (string) — Invoice link.
  - `hs_codes` (array[object]) — Product HS-codes.
  - `number` (string) — Invoice number.
  - `price` (number(double)) — Cost stated in the invoice. The fractional part is separated by decimal point, up to two digits after the decimal point. Example: `199.99`.
  - `price_currency` (string) — Invoice currency: - `USD`—dollar,  - `EUR`—euro,  - `TRY`—Turkish lira,  - `CNY`—yuan,  - `RUB`—ruble,  - `GBP`—pound sterling.  Default value is `USD

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/invoice/delete`

**Delete invoice link**

Operation ID: `invoice_delete`

**Request body:**

- `posting_number` (string) **(required)** — Shipment number.

**Response 200:**

- `result` (boolean) — Method result.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## ReportAPI

### `POST /v1/report/info`

**Report details**

Operation ID: `ReportAPI_ReportInfo`

Returns information about a created report by its identifier.

**Request body:**

- `code` (string) **(required)** — Unique report identifier.

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier.
  - `created_at` (string(date-time)) — Report creation date.
  - `error` (string) — Error code when generating the report.
  - `expires_at` (string(date-time)) — Date and time until the report is available via the link. The field returns an empty value if the report is generated before October 14, 2025.
  - `file` (string) — Link to the XLSX file.  For a report with the `SELLER_RETURNS` type, the link is available within 5 minutes after making a request.
  - `params` (object) — Array with the filters specified when the seller created the report.
  - `report_type` (string) — Report type:   - `SELLER_PRODUCTS`: products report;   - `SELLER_STOCK`: stocks report;   - `SELLER_RETURNS`: returns report;   - `SELLER_POSTINGS`: s
  - `status` (string) — Report generation status:   - `waiting`—pending,   - `processing`—being processed,   - `success`,   - `failed`.

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

### `POST /v1/report/list`

**Reports list**

Operation ID: `ReportAPI_ReportList`

Returns the list of reports that have been generated before.

**Request body:**

- `page` (integer(int32)) **(required)** — Page number.
- `page_size` (integer(int32)) **(required)** — The number of values on the page:   - default value is 100,   - maximum value is 1000.
- `report_type` (object)

**Response 200:**

- `result` (object)
  - `reports` (array[object]) — Array with generated reports.
  - `total` (integer(int32)) — Total number of reports.

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

### `POST /v1/report/products/create`

**Products report**

Operation ID: `ReportAPI_CreateCompanyProductsReport`

Method for getting a report with products data. For example, Ozon ID, number of products, prices, status.
Matches the **Products and prices → Product list → Download → Products CSV** action in your personal account.

Explanation of some fields in the report:
  - __Ozon Product ID__—product identifier in the Ozon system. For example, if you sell product from the Ozon warehouse and from your own warehouse, the Ozon Product ID will be the same for them.
  - __FBO Ozon SKU ID__—identifier of the product that is sold from the Ozon warehouse.
  - __FBS Ozon SKU ID__—identifier of the product that is...

**Request body:**

- `language` (object)
- `offer_id` (array[string]) — Product identifier in the seller's system.
- `search` (string) — Search by record content, checks for availability.
- `sku` (array[integer]) — Product identifier in the Ozon system, SKU.
- `visibility` (enum) — Values: `ALL, VALIDATION_STATE_FAIL, TO_SUPPLY, IN_SALE, REMOVED_FROM_SALE, PARTIAL_APPROVED, IMAGE_ABSENT, ARCHIVED`

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

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

### `POST /v2/report/returns/create`

**Report on returns**

Operation ID: `ReportAPI_ReportReturnsCreate`

Method for getting a report on FBO and FBS returns.

**Request body:**

- `filter` (object) **(required)** — Filter.
  - `delivery_schema` (enum) — Values: `FBS, FBO, ALL`. Filter by scheme of operation:  - `FBS`—returns to your warehouse.  - `FBO`—returns to marketplace warehouse. - `ALL`—all returns.
  - `date_from` (string(date-time)) **(required)** — Date from which the data is displayed in the report.  Available for the last three months only.
  - `date_to` (string(date-time)) **(required)** — Date up to which the data is displayed in the report.  Available for the last three months only.
  - `status` (enum) **(required)** — Values: `DisputeOpened, OnSellerApproval, ArrivedAtReturnPlace, OnSellerClarification, OnSellerClarificationAfterPartialCompensation, OfferedPartialCompensation, ReturnMoneyApproved, PartialCompensationReturned`. Filter by return status: - `DisputeOpened`: dispute with the customer has been opened; - `OnSellerApproval`: pending with the seller; - `ArrivedAtRetu
- `language` (object)

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

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

### `POST /v1/report/postings/create`

**Shipment report**

Operation ID: `ReportAPI_CreateCompanyPostingsReport`

Shipment report with orders details:
  - order statuses,
  - processing start date,
  - order numbers,
  - shipment numbers,
  - shipment costs,
  - shipments contents.
Matches the **FBO → Orders from Ozon warehouse** and **FBS → Orders from my warehouses → CSV** sections in your personal account.

**Request body:**

- `filter` (object) **(required)**
  - `cancel_reason_id` (array[integer]) — Cancellation reason identifier
  - `delivery_schema` (array[string]) — The scheme of operation is FBO or FBS.  Only one of the parameters can be passed to the array per query: * `fbo` - to get a report by FBO scheme, * `f
  - `offer_id` (string) — Product identifier.
  - `processed_at_from` (string(date-time)) **(required)** — Order processing start date and time.
  - `processed_at_to` (string(date-time)) **(required)** — Time when the order appeared in your personal account.
  - `sku` (array[integer]) — Product identifier in the Ozon system, SKU.
  - `status_alias` (array[string]) — Status text.
  - `statuses` (array[integer]) — Numerical status.
  - `title` (string) — Product name.
  - `warehouse_id` (array[integer]) — Warehouse identifier.
  - `delivery_method_id` (array[integer]) — Delivery method identifier. Get it using the [/v1/delivery-method/list](#operation/WarehouseAPI_DeliveryMethodList) method.
  - `is_express` (bool) — Express delivery: - `true`: only shipments with Ozon Express delivery; - `false`: only shipments without Ozon Express delivery.  If nothing is passed,
- `language` (object)
- `with` (object)
  - `additional_data` (boolean) — Pass `true` to get additional data in the response.
  - `analytics_data` (boolean) — Pass `true` to get analytics in the response. Pass the `filter.delivery_schema = fbs` value, otherwise you get an error.
  - `customer_data` (boolean) — Pass `true` to get customer data in the response.
  - `jewelry_codes` (boolean) — Pass `true` to get data on jewelry products in the response.

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

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

### `POST /v1/finance/cash-flow-statement/list`

**Financial report**

Operation ID: `FinanceAPI_FinanceCashFlowStatementList`

Method for getting a financial report for the 1st to 15th day and the 16th to 31st day of the month.
Requesting a report for specific days is not an option.
Matches the **Finance → Payments** section in your seller account.

**Request body:**

- `date` (object) **(required)**
  - `from` (string(date-time)) **(required)** — Date from which the report is calculated.
  - `to` (string(date-time)) **(required)** — Date up to which the report is calculated.
- `page` (integer(int64)) **(required)** — Number of the page returned in the request.
- `with_details` (boolean) — `true`, if you need to add additional parameters to the response.
- `page_size` (integer(int64)) **(required)** — Number of items on the page.

**Response 200:**

- `result` (object)
  - `cash_flows` (object) — Reports list.
  - `details` (object) — Detailed information.
  - `page_count` (integer(int64)) — Number of pages with reports.

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

### `POST /v1/report/discounted/create`

**Report on markdown products**

Operation ID: `ReportAPI_CreateDiscountedReport`

Generates a report on markdown products at Ozon warehouses.
For example, Ozon can markdown a product due to damage when delivering.

The method returns a report identifier.
To get the report, send the identifier in the request of the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

You can send 1 request per minute in one account.
Matches the **Analytics → Reports → Sales from Ozon warehouse → Marked down by Ozon** section in your personal account.

**Request body:**

**Response 200:**

- `code` (string) — Unique report identifier. To get a report, pass the value to the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

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

### `POST /v1/report/warehouse/stock`

**Report on FBS warehouse stocks**

Operation ID: `ReportAPI_CreateStockByWarehouseReport`

Report with information about the number of available and reserved products in stock.
Matches the **FBS → Logistics management → Stock management → Download in XLS** action in your personal account. 

The method returns a report identifier.
To get the report, send the identifier in the request of the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

**Request body:**

- `language` (object)
- `warehouseId` (string) **(required)** — Warehouses identifiers. Limit of values in the request. Maximum is 50.

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

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

### `POST /v1/report/placement/by-products/create`

**Get report on storage cost by products**

Operation ID: `CreatePlacementByProductsReport`

Corresponds to the **FBO → Storage cost** section in the personal account.

You can get the report no more than 5 times per day.

**Request body:**

- `date_from` (string) **(required)** — Start date of the reporting period in the `YYYY-MM-DD` format.
- `date_to` (string) **(required)** — End date of the reporting period in the `YYYY-MM-DD` format.  The maximum period is 31 days.

**Response 200:**

- `code` (string) — Unique report identifier. To get a report, pass the value to the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/report/placement/by-supplies/create`

**Get report on storage cost by supplies**

Operation ID: `CreatePlacementBySuppliesReport`

Corresponds to the **FBO → Storage cost** section in the personal account.

You can get the report no more than 5 times per day.

**Request body:**

- `date_from` (string) **(required)** — Start date of the reporting period in the `YYYY-MM-DD` format.
- `date_to` (string) **(required)** — End date of the reporting period in the `YYYY-MM-DD` format.  The maximum period is 31 days.

**Response 200:**

- `code` (string) — Unique report identifier. To get a report, pass the value to the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/report/marked-products-sales/create`

**Generate sales report of labeled products**

Operation ID: `CreateCompanyMarkedProductsSalesReport`

You can get no more than 50,000 labeling codes in one report. To get the remaining data, reduce the report generation period.

**Request body:**

- `date` (object)
  - `from` (string) **(required)** — Start date of the reporting period in the `YYYY-MM-DD` format.
  - `to` (string) **(required)** — End date of the reporting period in the `YYYY-MM-DD` format.

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## AnalyticsAPI

More methods in the [**Premium Methods**](#tag/Premium) section.

### `POST /v2/analytics/stock_on_warehouses`

**Stocks and products report**

Operation ID: `AnalyticsAPI_AnalyticsGetStockOnWarehousesV2`

> **Note:** 
This method will be disabled. Switch to the [/v1/analytics/stocks](#operation/AnalyticsAPI_AnalyticsStocks) method.

Method for getting a report on leftover stocks and products movement at Ozon warehouses.

> **Note:** 
Different from the report in the Analytics → Reports → Report on stocks and products on the way to Ozon warehouses section in your personal account.

**Request body:**

- `limit` (integer(int64)) **(required)** — Number of values per page. Default is 100.
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `warehouse_type` (enum) — Values: `ALL, EXPRESS_DARK_STORE, NOT_EXPRESS_DARK_STORE`

**Response 200:**

- `result` (object)
  - `rows` (array[object]) — Information about products and stocks.

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

### `POST /v1/analytics/turnover/stocks`

**Product turnover**

Operation ID: `AnalyticsAPI_StocksTurnover`

Use the method to get the product turnover rate and the number of days the current stock will last.

 The method corresponds to the [**FBO → Residuals management**](https://seller.ozon.ru/app/supply/stocks-management) section in the personal account.
 You can make no more than 1 request per minute per `Client-Id` account.

If you request a list of products by `sku`, the `limit` and `offset` parameters are optional.

**Request body:**

- `limit` (integer(int32)) — Number of values in the response.
- `offset` (integer(int32)) — Number of elements to skip in the response.  For example, if `offset = 10`, the response starts with the 11th element found.
- `sku` (array[string]) — Product identifiers in the Ozon system, SKU.

**Response 200:**

- `items` (array[object]) — Products.
  - `ads` (number(double)) — Average daily number of product items sold over the last 60 days.
  - `current_stock` (integer(int64)) — Product stock, pcs.
  - `idc` (number(double)) — Number of days the stock will last based on your average daily sales.
  - `idc_grade` (enum) — Values: `GRADES_NONE, GRADES_NOSALES, GRADES_GREEN, GRADES_YELLOW, GRADES_RED, GRADES_CRITICAL`. Product stock level: - `GRADES_NONE` — awaiting supply; - `GRADES_NOSALES` — no sales; - `GRADES_GREEN` — green, "good"; - `GRADES_YELLOW` — yellow, "

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

### `POST /v1/analytics/average-delivery-time`

**Get analytics of average delivery time**

Operation ID: `AnalyticsAPI_AverageDeliveryTime`

Method has the same functionality as **Analytics → Sales location → Average delivery time** tab in the seller's personal account. Get detailed analytics for each cluster using the [/v1/analytics/average-delivery-time/details](#operation/AnalyticsAPI_AverageDeliveryTimeDetails) method.

**Request body:**

- `delivery_schema` (enum) **(required)** — Values: `All, FBO, FBS, UNKNOWN`
- `sku` (array[string]) — Product identifier in the Ozon system, SKU.
- `supply_period` (enum) **(required)** — Values: `FOUR_WEEKS, ONE_WEEK, TWO_WEEKS, EIGHT_WEEKS`

**Response 200:**

- `data` (array[object]) — Cluster information.
  - `clusters_data` (array[object]) — Shipping cluster data.
  - `delivery_cluster_id` (integer(int64)) — Delivery cluster identifier.
  - `metrics` (object)
- `total` (object)
  - `attention_level` (enum) — Values: `UNKNOWN, LOW, MEDIUM, HI`
  - `average_delivery_time` (integer(int32)) — Average delivery time to the customer.
  - `average_delivery_time_status` (enum) — Values: `UNKNOWN, FAST, MEDIUM, LONG`
  - `impact_share` (integer(int32)) — Percentage of effect on general metrics.
  - `exact_impact_share` (string) — Percentage of effect on general metrics to 4 decimal places.
  - `lost_profit` (integer(int32)) — Overpayment for logistics.
  - `orders_count` (object)
  - `recommended_supply` (integer(int32)) — Recommended supply in pcs.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/analytics/average-delivery-time/details`

**Get detailed analytics of average delivery time**

Operation ID: `AnalyticsAPI_AverageDeliveryTimeDetails`

Method has the same functionality as **Analytics → Sales location → Average delivery time** tab in the seller's personal account.

Get general analytics by cluster using the [/v1/analytics/average-delivery-time](#operation/AnalyticsAPI_AverageDeliveryTime) method.

**Request body:**

- `cluster_id` (integer(int64)) **(required)** — Clusters identifier.
- `filters` (object)
  - `delivery_schema` (enum) — Values: `ALL, FBO, FBS`
  - `supply_period` (enum) — Values: `ONE_WEEK, TWO_WEEKS, FOUR_WEEKS, EIGHT_WEEKS`
- `limit` (integer(int32)) **(required)** — Number of elements in the response.
- `offset` (integer(int32)) **(required)** — Number of elements that are skipped in the response.  For example, if `offset=10`, the response starts with the 11th element found.

**Response 200:**

- `data` (array[object]) — Cluster data.
  - `clusters_data` (array[object]) — Shipping cluster data.
  - `item` (object)
  - `metrics` (object)
- `total_rows` (integer(int64)) — Total number of rows.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/analytics/average-delivery-time/summary`

**Get general analytics of average delivery time**

Operation ID: `AverageDeliveryTimeSummary`

Method has the same functionality as **Analytics → Sales location → Average delivery time** tab in the seller's personal account.

Get detailed analytics for each cluster using the [/v1/analytics/average-delivery-time/details](#operation/AnalyticsAPI_AverageDeliveryTimeDetails) method.
To get analytics of average delivery time use the [/v1/analytics/average-delivery-time](#operation/AnalyticsAPI_AverageDeliveryTime) method.

**Response 200:**

- `average_delivery_time` (integer(int32)) — Average delivery time to the customer.
- `current_tariff` (object)
  - `fee` (number(float)) — Percentage of product price.
  - `start` (integer(int32)) — Average value of delivery time metric.
  - `tariff_status` (enum) — Values: `TariffStatus_Unspecified, GOOD, MEDIUM, BAD`
  - `tariff_value` (number(float)) — Percentage to the base FBO logistics tariff.
- `lost_profit` (number(double)) — Overpayment for FBO logistics.
- `perfect_delivery_time` (integer(int32)) — Recommended average delivery time to the customer.
- `updated_at` (string(date-time)) — Date and time of the last data update.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/analytics/stocks`

**Get analytics on stock balances**

Operation ID: `AnalyticsAPI_AnalyticsStocks`

Use the method to get analytics on product stock balances. The method corresponds to the [**FBO → Stock management**](https://seller.ozon.ru/app/fbo-stocks/stocks-management/) section in your personal account. Analytics is updated twice a day: around 07:00 and 16:00 UTC. 

In the request, use only one of the parameters: `cluster_ids` or `macrolocal_cluster_ids`, otherwise an error is returned.

**Request body:**

- `cluster_ids` (array[string]) — Filter by cluster identifiers. To get identifiers, use the [/v1/cluster/list](#operation/SupplyDraftAPI_DraftClusterList) method.
- `item_tags` (array[string]) — Filter by product tags:       - `ITEM_ATTRIBUTE_NONE`: no tag; - `ECONOM`: economy product; - `NOVEL`: new product; - `DISCOUNT`: discounted product; 
- `macrolocal_cluster_ids` (array[string]) — Filter by macrolocal cluster identifiers. You can get cluster identifiers in the `macrolocal_cluster_ids` parameter of the [/v1/cluster/list](#operati
- `skus` (array[string]) **(required)** — Filter by product identifiers in the Ozon system, SKU.
- `turnover_grades` (array[string]) — Filter by product liquidity status:       - `TURNOVER_GRADE_NONE`: product has no liquidity status.       - `DEFICIT`: product in short supply. There'
- `warehouse_ids` (array[string]) — Filter by warehouse identifiers. To get identifiers, use the [/v1/warehouse/list](#operation/WarehouseAPI_WarehouseList) method.

**Response 200:**

- `items` (array[object]) — Product details.
  - `ads` (number(double)) — Average daily number of product items sold over the last 28 days in all clusters.
  - `ads_cluster` (number(double)) — Average daily number of product items sold over the last 28 days in the cluster.
  - `available_stock_count` (integer(int32)) — Number of products available for sale. Matches the "Available for sale" column.
  - `cluster_id` (integer(int64)) — Cluster identifier. To get information on the cluster, use the [/v1/cluster/list](#operation/SupplyDraftAPI_DraftClusterList) method.
  - `cluster_name` (string) — Cluster name.
  - `days_without_sales` (integer(int32)) — Number of days without sales in all clusters.
  - `days_without_sales_cluster` (integer(int32)) — Number of days without sales in the cluster.
  - `excess_stock_count` (integer(int32)) — Number of surplus products from supply that are available for removal.
  - `expiring_stock_count` (integer(int32)) — Number of expiring product items.
  - `idc` (number(double)) — Number of days for which the product stock is enough, taking into account average daily sales for the last 28 days.
  - `item_tags` (array[string]) — Product tags:       - `ITEM_ATTRIBUTE_NONE`: no tag; - `ECONOM`: economy product; - `NOVEL`: new product; - `DISCOUNT`: discounted product; - `FBS_RET
  - `macrolocal_cluster_id` (integer(int64)) — Macrolocal cluster identifier. Get cluster information using the [/v1/cluster/list](#operation/SupplyDraftAPI_DraftClusterList) or [/v2/cluster/list](
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `other_stock_count` (integer(int32)) — Number of product items under checking.
  - `requested_stock_count` (integer(int32)) — Number of product items in supply requests.
  - `return_from_customer_stock_count` (integer(int32)) — Number of product items being returned from customers.
  - `return_to_seller_stock_count` (integer(int32)) — Number of product items ready for removal from stock at your request.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `stock_defect_stock_count` (integer(int32)) — Number of defective products available for removal from stock.
  - `transit_defect_stock_count` (integer(int32)) — Number of defective products available for removal from supply.
  - `transit_stock_count` (integer(int32)) — Number of product items in transit for supply.
  - `turnover_grade` (enum) — Values: `UNSPECIFIED, TURNOVER_GRADE_NONE, DEFICIT, POPULAR, ACTUAL, SURPLUS, NO_SALES, WAS_NO_SALES`. Product liquidity status in all clusters:       - `TURNOVER_GRADE_NONE`: product has no liquidity status.       - `DEFICIT`: product in short supply. 
  - `turnover_grade_cluster` (enum) — Values: `UNSPECIFIED, TURNOVER_GRADE_NONE, DEFICIT, POPULAR, ACTUAL, SURPLUS, NO_SALES, WAS_NO_SALES`. Product liquidity status in the cluster:       - `TURNOVER_GRADE_NONE`: product has no liquidity status.       - `DEFICIT`: product in short supply. T
  - `valid_stock_count` (integer(int32)) — Number of products being prepared for sale. Matches the "Preparing for sale" column.
  - `waiting_docs_stock_count` (integer(int32)) — Number of labeled products awaiting your actions.
  - `warehouse_id` (integer(int64)) — Warehouse identifier.
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

## FinanceAPI

More methods in the [**Premium Methods**](#tag/Premium) section.

### `POST /v2/finance/realization`

**Sales report (version 2)**

Operation ID: `FinanceAPI_GetRealizationReportV2`

Returns information on products sold and returned within a month. Canceled or non-purchased products are not included.
Matches the **Finance → Documents → Sales reports → Product sales report** section in your personal account.

Report is returned no later than the 5th day of the next month.

**Request body:**

- `month` (integer(int32)) **(required)** — Month.
- `year` (integer(int32)) **(required)** — Year.

**Response 200:**

- `result` (object)
  - `header` (object)
  - `rows` (array[object]) — Report table.

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

### `POST /v1/finance/realization/posting`

**Sales report by order**

Operation ID: `FinanceAPI_GetRealizationReportV1`

Returns a report on delivered and returned products with details by order. Canceled or non-purchased products aren't included. Report is available from the current date until August, 2023 inclusive.

**Request body:**

- `month` (integer(int32)) **(required)** — Month.
- `year` (integer(int32)) **(required)** — Year.

**Response 200:**

- `header` (object)
  - `contract_date` (string) — Date of contract conclusion.
  - `contract_number` (string) — Contract number.
  - `currency_sys_name` (string) — Currency.
  - `doc_date` (string) — Document generation date.
  - `number` (string) — Report ID.
  - `payer_inn` (string) — Payer's Tax Identification Number.
  - `payer_kpp` (string) — Payer's Tax Registration Reason Code.
  - `payer_name` (string) — Payer's name.
  - `receiver_inn` (string) — Recipient's Tax Identification Number.
  - `receiver_kpp` (string) — Recipient's Tax Registration Reason Code.
  - `receiver_name` (string) — Recipient's name.
  - `start_date` (string) — Start date.
  - `stop_date` (string) — End date.
- `rows` (array[object]) — Report table.
  - `commission_ratio` (number(double)) — Percentage of sales commission by category.
  - `delivery_commission` (object)
  - `item` (object)
  - `return_commission` (object)
  - `row_number` (integer(int32)) — Row number.
  - `seller_price_per_instance` (number(double)) — Seller's discounted price.
  - `order` (object)
  - `legal_entity_document` (object)

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

### `POST /v3/finance/transaction/list`

**Transactions list**

Operation ID: `FinanceAPI_FinanceTransactionListV3`

> **Note:** 
Use the method with sequential sending of requests.

The data may not match the information in your personal account.

Returns detailed information on all accruals. The maximum period for which you can get information in one request is 1 month.

If you don't specify the `posting_number` in request, the response contains all shipments for the specified period or shipments of a certain type.

**Request body:**

- `filter` (object)
  - `date` (object)
    - `from` (string(date-time)) — Period start.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.  Example: `2019-11-25T10:43:06.51`.
    - `to` (string(date-time)) — Period end.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.  Example: `2019-11-25T10:43:06.51`.
  - `operation_type` (array[string]) — Operation type:   - `ClientReturnAgentOperation`—getting a returned, cancelled, or unredeemed order from the customer;   - `MarketplaceMarketingAction
  - `posting_number` (string) — Shipment number.
  - `transaction_type` (string) — Transaction type:   - `all`—all,   - `orders`—orders,   - `returns`—returns and cancellations,   - `services`—service fees,   - `compensation`—compens
- `page` (integer(int64)) **(required)** — Number of the page returned in the request.
- `page_size` (integer(int64)) **(required)** — Number of items on the page.

**Response 200:**

- `result` (object)
  - `operations` (array[object]) — Transactions infromation.
  - `page_count` (integer(int64)) — Number of pages. If 0, there are no more pages.
  - `row_count` (integer(int64)) — Number of transactions on all pages. If 0, there are no more transactions.

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

### `POST /v3/finance/transaction/totals`

**Total transactions sum**

Operation ID: `FinanceAPI_FinanceTransactionTotalV3`

> **Note:** 
The data may not match the information in your personal account.

Returns the transaction totals for the specified period.

**Request body:**

- `date` (object)
  - `from` (string(date-time)) — Period start.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.  Example: `2019-11-25T10:43:06.51`.
  - `to` (string(date-time)) — Period end.  Format: `YYYY-MM-DDTHH:mm:ss.sssZ`.  Example: `2019-11-25T10:43:06.51`.
- `posting_number` (string) — Shipment number.
- `transaction_type` (string) — Transaction type:   - `all`—all,   - `orders`—orders,   - `returns`—returns and cancellations,   - `services`—service fees,   - `compensation`—compens

**Response 200:**

- `result` (object)
  - `accruals_for_sale` (number(double)) — Total cost of products and returns for specified period.
  - `compensation_amount` (number(double)) — Compensations.
  - `money_transfer` (number(double)) — Charges for delivery and returns when working under rFBS scheme.
  - `others_amount` (number(double)) — Other accurals.
  - `processing_and_delivery` (number(double)) — Cost of shipment processing, orders packaging, pipeline and last mile services, and delivery cost before the new commissions and rates applied from Fe
  - `refunds_and_cancellations` (number(double)) — Cost of reverse pipeline, returned, canceled and unredeemed orders processing, and return cost before the new commissions and rates applied from Febru
  - `sale_commission` (number(double)) — The commission withheld when the product was sold and refunded when the product was returned.
  - `services_amount` (number(double)) — The  additional services cost that are not directly related to deliveries and returns. For example, promotion or product placement.

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

### `POST /v1/finance/document-b2b-sales`

**Legal entities sales register**

Operation ID: `ReportAPI_CreateDocumentB2BSalesReport`

Use the method to get sales to legal entities report. Matches the **Finance → Documents → Legal entities sales register** section in your personal account.

**Request body:**

- `date` (string) **(required)** — Time period in the `YYYY-MM` format.
- `language` (object)

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/finance/document-b2b-sales/json`

**Legal entities sales register in JSON format**

Operation ID: `ReportAPI_CreateDocumentB2BSalesJSONReport`

Use the method to get sales to legal entities register in JSON format. Matches the **Finance → Documents → Legal entities sales register section** in your personal account.

**Request body:**

- `date` (string) **(required)** — Reporting period in the `YYYY-MM` format. The report is available up to and including January 2019.

**Response 200:**

- `date_from` (string) — Start date of the reporting period in the `YYYYYY-MM-DD` format.
- `date_to` (string) — End date of the reporting period in the `YYYYY-MM-DD` format.
- `invoices` (array[object]) — Invoice list.
  - `buyer_info` (object)
  - `currency` (string) — Currency.
  - `currency_code` (integer(int32)) — Currency code.
  - `info` (object) — Invoice details.
  - `offer_id` (string) — Product identifier in the seller's system, article code.
  - `operations` (array[object]) — Operations list.
  - `product_name` (string) — Product name.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `unit_code` (integer(int32)) — Unit code.
  - `unit_name` (string) — Unit designation.
- `seller_info` (object)
  - `company_name` (string) — Company name.
  - `inn` (string) — Taxpayer identification number (INN).
  - `kpp` (string) — Tax registration reason code (KPP).

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

### `POST /v1/finance/mutual-settlement`

**Mutual settlements report**

Operation ID: `ReportAPI_CreateMutualSettlementReport`

Use the method to get mutual settlements report. Matches the **Finance → Documents → Analytical reports → Mutual settlements report** section in your personal account.

**Request body:**

- `date` (string) **(required)** — Time period in the `YYYY-MM` format.
- `language` (object)

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. Allows you to get the report within 3 days of the request. To get a report, pass the value to the [/v1/report/info](#operati

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/finance/products/buyout`

**Purchased product report**

Operation ID: `GetFinanceProductsBuyout`

Returns a report on products purchased by Ozon for resale in the EAEU and other countries. The method corresponds to the **Finance → Documents → UTDs on transactions with legal entities → UTD for purchased products** section in your personal account.

**Request body:**

- `date_from` (string) **(required)** — Date from which the data will be in the report.
- `date_to` (string) **(required)** — Date up to which the data will be in the report.  The maximum period is 31 days.

**Response 200:**

- `products` (array[object]) — Purchased product list
  - `amount` (number(float)) — Accrual amount.
  - `buyout_price` (number(float)) — Purchase of products price, VAT included.
  - `deduction_by_category_percent` (number(float)) — Category discount percentage.
  - `name` (string) — Product name.
  - `offer_id` (string) — Product identifier in the seller's system.
  - `posting_number` (string) — Posting number.
  - `quantity` (integer(int32)) — Number of products.
  - `seller_price_per_instance` (number(float)) — Seller's discounted price.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `vat_percent` (integer(int32)) — Product VAT rate percentage.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/finance/compensation`

**Compensation report**

Operation ID: `ReportAPI_GetCompensationReport`

Use the method to get а compensation report. It is the same as the report from the **Finance → Documents → Compensation and other accruals** section in your personal account.

**Request body:**

- `date` (string) **(required)** — Reporting period in the `YYYY-MM` format.
- `language` (object)

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. To get a report, pass the value to the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/finance/decompensation`

**Decompensation report**

Operation ID: `ReportAPI_GetDecompensationReport`

Use the method to get а decompensation report. It is the same as the report from the **Finance → Documents → Compensation and other accruals** section in your personal account.

**Request body:**

- `date` (string) **(required)** — Reporting period in the `YYYY-MM` format.
- `language` (object)

**Response 200:**

- `result` (object)
  - `code` (string) — Unique report identifier. To get a report, pass the value to the [/v1/report/info](#operation/ReportAPI_ReportInfo) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## Receipt

### `POST /v1/receipts/get`

**Get receipt in PDF format**

Operation ID: `GetReceipt`

> **Note:** 
 Method is available to sellers who signed a contract with "OZON Marketplace Kazakhstan" LLP.

**Request body:**

- `receipt_id` (string) **(required)** — Receipt identifier. Get the parameter value using the [/v1/receipts/seller/list](#operation/ReceiptsSellerList) method.

**Response 200:**

- `content` (string(byte)) — PDF file with receipt in binary format.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/receipts/seller/list`

**Get list of seller receipts**

Operation ID: `ReceiptsSellerList`

> **Note:** 
 Method is available to sellers who signed a contract with "OZON Marketplace Kazakhstan" LLP.

**Request body:**

- `page` (integer(int64)) — Number of pages to skip.
- `page_size` (integer(int64)) — Number of elements on the page.
- `posting_numbers` (array[string]) — Filter by shipment numbers.

**Response 200:**

- `has_next` (boolean) — Indication that not all entries are in the response:  - `true`: create another request with a different `page` value to get the remaining values; - `f
- `receipts` (array[object]) — Receipt details.
  - `created_at` (string(date-time)) — Receipt creation date.
  - `operation_type` (enum) — Values: `UNSPECIFIED, COMMODITY`
  - `order_id` (integer(int64)) — Order identifier.
  - `parent_receipt_id` (string) — Parent receipt identifier.
  - `posting_numbers` (array[string]) — Shipment numbers.
  - `receipt_id` (string) — Receipt identifier.
  - `receipt_number` (string) — Receipt number.
  - `type` (enum) — Values: `UNSPECIFIED, INCOMING, REFUND`
  - `updated_at` (string(date-time)) — Update date.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/receipts/upload`

**Upload receipt**

Operation ID: `UploadReceipt`

> **Note:** 
 Method is available to sellers who signed a contract with "OZON Marketplace Kazakhstan" LLP.

**Response 200:**

- `receipt_id` (string) — Receipt identifier.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## SellerRating

When working with Ozon, sellers should follow the requirements for service quality, delivery time, and communication with customers. The rating system reflects the seller's service quality. Some indicators are visible to customers, such as product rating and price index.

[Learn more about the rating system in the Help Center](https://docs.ozon.ru/global/en/launch/quality-assurance/)

### `POST /v1/rating/summary`

**Get information on current seller ratings**

Operation ID: `RatingAPI_RatingSummaryV1`

Seller rating on the following metrics: price index, delivery on time, cancellation rate, complaints, and other.
Matches the **Ratings → Seller ratings** section in your personal account.

**Request body:**

**Response 200:**

- `groups` (object) — Rating groups list.
- `localization_index` (object) — Localization index details. If you had no sales in the last 14 days, the parameter fields will be empty.
- `penalty_score_exceeded` (boolean) — An indication that the penalty points balance is exceeded.
- `premium` (boolean) — An indication that you have the [Premium](https://seller-edu.ozon.ru/seller-rating/about-rating/premium-program) subscription.
- `premium_plus` (boolean) — An indication that you have the [Premium Plus](https://seller-edu.ozon.ru/seller-rating/about-rating/subscription-premium-plus) subscription.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/rating/history`

**Get information on seller ratings for the period**

Operation ID: `RatingAPI_RatingHistoryV1`

Filtered information about ratings for a given period.
Matches the **Ratings → Seller ratings** section in your personal account.

**Request body:**

- `date_from` (string(date-time)) **(required)** — Period start.
- `date_to` (string(date-time)) **(required)** — Period end.
- `ratings` (object) **(required)** — Filter by rating.  Ratings for which you want to get a value for the period:  - `rating_on_time`: the percentage of orders completed on time in the la
- `with_premium_scores` (boolean) — Indication that the response should contain information about Premium program penxalty points.

**Response 200:**

- `premium_scores` (object) — Information on the Premium program penalty points.
- `ratings` (object) — Information on the seller ratings.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/rating/index/fbs/info`

**Get FBS and rFBS error index**

Operation ID: `RatingAPI_GetFBSRatingIndexInfoV1`

**Response 200:**

- `currency_code` (string) — Currency code of error processing cost.
- `defects` (array[object]) — Error index by day.
  - `date` (string) — Date in the `YYYY-MM-DD` format.
  - `index_by_date` (number(double)) — Error index value.
  - `processing_costs_sum_by_date` (number(double)) — Error processing costs.
- `index` (number(double)) — Error index value per period.
- `period_from` (string) — Billing period start date in the `YYYY-MM-DD` format.
- `period_to` (string) — Billing period end date in the `YYYY-MM-DD` format.
- `processing_costs_sum` (number(double)) — Error processing costs per period.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/rating/index/fbs/posting/list`

**List of shipments that affected FBS and rFBS error index**

Operation ID: `RatingAPI_ListFBSRatingIndexPostingsV1`

**Request body:**

- `cursor` (string) — Cursor for the next data sample.
- `filter` (object) **(required)**
  - `date_from` (string(date-time)) **(required)** — Period start date.
  - `date_to` (string(date-time)) **(required)** — Period end date.
  - `posting_numbers` (array[string]) — Shipment numbers.
- `limit` (integer(int64)) **(required)** — Number of values in the response.

**Response 200:**

- `cursor` (string) — Cursor for the next data sample.
- `errors` (array[object]) — Shipments that affected the index.
  - `charge_percent` (number(double)) — Processing cost as a share of shipment cost.
  - `charge_price` (number(double)) — Error processing cost.
  - `charge_price_currency_code` (string) — Currency code of error processing cost: - `RUB`: Russian ruble, - `BYN`: Belarusian ruble, - `KZT`: tenge, - `EUR`: euro, - `USD`: US dollar, - `CNY`:
  - `delivery_schema` (string) — Delivery scheme: - `FBS`,  - `rFBS`,  - `erFBS`.
  - `error_at` (string(date-time)) — Date when the error occurred.
  - `has_grace_status` (boolean) — `true` if shipment has preferential status.
  - `index` (number(double)) — Error index value.
  - `posting_error_type` (enum) — Values: `UNSPECIFIED, SELLER_CANCELLATION, SELLER_DELAY`
  - `posting_number` (string) — Shipment number.
  - `product_price` (number(double)) — Product cost in shipment.
  - `product_price_currency_code` (string) — Currency code of product cost: - `RUB`: Russian ruble, - `BYN`: Belarusian ruble, - `KZT`: tenge, - `EUR`: euro, - `USD`: US dollar, - `CNY`: yuan.
- `has_next` (boolean) — `true` if not all shipments are returned in the response.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

## Digital

### `POST /v1/posting/digital/codes/upload`

**Upload digital product codes for shipping**

Operation ID: `UploadPostingCodes`

Method is available only to sellers working with digital products. You can upload digital product codes within 24 hours of order confirmation.

Pass all digital product codes for each product in the order in a single request. If you don't pass all codes, the request returns an error.

**Request body:**

- `exemplars_by_sku` (array[object]) — Data on digital product codes by SKU.
  - `exemplar_keys` (array[string]) — List of digital product codes. The number of codes must match the `exemplar_qty` parameter.
  - `exemplar_qty` (integer(int32)) **(required)** — Number of digital product codes you pass to the customer.    The sum of `exemplar_qty` and `not_available_exemplar_qty` must equal the number of codes
  - `not_available_exemplar_qty` (integer(int32)) **(required)** — Number of digital product codes that you cannot pass to the customer.    The sum of `exemplar_qty` and `not_available_exemplar_qty` must equal the num
  - `sku` (integer(int64)) **(required)** — Product identifier in the Ozon system, SKU.
- `posting_number` (string) — Shipment number.

**Response 200:**

- `exemplars_by_sku` (array[object]) — Data on digital product codes by SKU.
  - `failed_exemplars` (array[object]) — List of digital product codes with errors.
  - `received_qty` (integer(int32)) — Number of accepted digital product codes.
  - `rejected_qty` (integer(int32)) — Number of codes not accepted or transferred.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/posting/digital/list`

**Get shipments list**

Operation ID: `ListPostingCodes`

> **Note:** 
Method is deprecated and will be disabled. Switch to the new version [/v2/posting/digital/list](#operation/PostingDigitalList).

Returns a list of shipments for which digital product codes need to be uploaded. Method is available only to sellers working with digital products.

To get a list of shipments in any status, use the [/v2/posting/fbo/list](#operation/PostingAPI_GetFboPostingList) method.

**Request body:**

- `dir` (enum) — Values: `ASC, DESC`
- `filter` (object)
  - `posting_number` (array[string]) — Posting number.
  - `since` (string(date-time)) — Period start in `YYYY-MM-DD` format.
  - `to` (string(date-time)) — Period end in `YYYY-MM-DD` format.
- `limit` (integer(int64)) — Number of values in the response. Maximum is 1000, minimum is 1.
- `offset` (integer(int64)) — Number of elements to be skipped in the response. For example, if `offset = 10`, the response starts with the 11th element found.
- `with` (object)
  - `analytics_data` (boolean) — Pass `true` to add analytics data to the response.
  - `financial_data` (boolean) — Pass `true` to add financial data to the response.
  - `legal_info` (boolean) — Pass `true` to add legal details to the response.

**Response 200:**

- `result` (array[object]) — Shipment list.
  - `additional_data` (array[object]) — Additional parameters.
  - `analytics_data` (object)
  - `cancel_reason_id` (integer(int64)) — Shipment cancellation reason identifier.
  - `created_at` (string(date-time)) — Date and time of shipment creation.
  - `financial_data` (object)
  - `in_process_at` (string(date-time)) — Date and time of shipment processing start.
  - `legal_info` (object)
  - `order_id` (integer(int64)) — Identifier of the order to which the shipment belongs.
  - `order_number` (string) — Number of the order to which the shipment belongs.
  - `posting_number` (string) — Shipment number.
  - `products` (array[object]) — Number of products in the shipment.
  - `status` (string) — Shipment status: `awaiting_packaging`.
  - `waiting_deadline_for_digital_code` (string(date-time)) — Deadline for providing digital product codes. Upload codes with the [/v1/posting/digital/codes/upload](#operation/UploadPostingCodes) method.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

### `POST /v1/product/digital/stocks/import`

**Update quantity of digital products**

Operation ID: `DigitalProductAPI_StocksImport`

Method is available only to sellers working with digital products. 

Allows you to change the product stock quantity.

**Request body:**

- `stocks` (array[object]) — Information about products in stock.
  - `offer_id` (string) **(required)** — Product identifier in the seller's system, article code.
  - `stock` (integer(int64)) **(required)** — Product stock quantity.

**Response 200:**

- `status` (array[object]) — Product details.
  - `errors` (array[object]) — Errors that occurred while processing the request.
  - `offer_id` (string) — Product identifier in the seller's system, article code.
  - `product_id` (integer(int64)) — Product identifier in the Ozon system, `product_id`.
  - `sku` (integer(int64)) — Product identifier in the Ozon system, SKU.
  - `updated` (boolean) — `true`, if the request was completed successfully and the stock is updated.

**Response default:**

- `code` (integer(int32)) — Error code.
- `details` (array[object]) — Error details.
  - `typeUrl` (string) — URL type.
  - `value` (string(byte)) — Error value.
- `message` (string) — Error description.

---

