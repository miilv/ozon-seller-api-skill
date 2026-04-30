---
name: ozon-seller-api
description: Ozon Seller API reference and helper. Use when working with Ozon marketplace API, building integrations with Ozon, managing products/orders/warehouses/finances via the Ozon Seller API, or when the user mentions Ozon API.
---

# Ozon Seller API

You have access to the complete Ozon Seller API documentation (380 endpoints). Use it to help the user build integrations, write API calls, debug issues, and understand the Ozon marketplace API.

## Quick Reference

- **Base URL:** `https://api-seller.ozon.ru`
- **Protocol:** All endpoints use **POST** with JSON request/response bodies
- **Auth headers** (required on every request):
  - `Client-Id` — seller's client ID (number)
  - `Api-Key` — API key string
- **Content-Type:** `application/json`
- **Docs:** https://docs.ozon.ru/api/seller/en/

## Authentication

Two auth methods:
1. **API Key** — generated in seller's personal account at Settings > Seller API. Pass as `Client-Id` + `Api-Key` headers.
2. **OAuth Token** — for private apps. Create an app at https://seller.ozon.ru/app/settings/api-keys, get a token via OAuth flow. Pass as `Authorization: Bearer <token>` header (no `Client-Id` needed).

## Example Request

```bash
curl -X POST https://api-seller.ozon.ru/v2/product/list \
  -H "Client-Id: 123456" \
  -H "Api-Key: your-api-key" \
  -H "Content-Type: application/json" \
  -d '{"filter": {}, "limit": 10}'
```

## API Structure (Tag Groups)

### Description (Guides)
Introduction, Getting Started, Auth, OAuth, Environment, Process

### Basic Methods
- **APIkey** — get roles/permissions for current API key
- **SellerInfo** — seller account info
- **CategoryAPI** — product categories tree, attributes, attribute values
- **ProductAPI** — create/update/delete/archive products, get product info, images, ratings
- **BarcodeAPI** — add/generate barcodes
- **Prices&StocksAPI** — update prices, stocks, discounts, get stock info
- **Promos** — Ozon promotions: list, activate/deactivate products
- **PricingStrategyAPI** — pricing strategies: create, update, delete, add/remove items
- **BrandAPI** — brand certification
- **CertificationAPI** — quality certificates: create, bind, delete
- **WarehouseAPI** — warehouse list, delivery methods
- **FBSWarehouseSetup** — create/update/archive FBS warehouses
- **FBS** — FBS postings: list, get, ship, cancel, labels, acts
- **PolygonAPI** — delivery polygons for rFBS
- **FBO** — FBO postings: list, get; supply orders
- **FboSupplyRequest** — FBO supply drafts and orders
- **FBS&rFBSMarks** — product exemplar codes (Chestny ZNAK)
- **DeliveryFBS** — FBS carriages: create, approve, cancel, labels, acts
- **DeliveryrFBS** — rFBS delivery: cutoff, timeslot, tracking, status
- **Pass** — warehouse passes: create, update, delete, list
- **ReturnsAPI** — FBO returns and utilization
- **ReturnAPI** — return shipments by barcode
- **RFBSReturnsAPI** — rFBS return requests: list, approve, reject, compensate
- **CancellationAPI** — conditional cancellation requests
- **ChatAPI** — customer chats: list, send messages/files, history
- **SupplierAPI** — invoices: create, upload, get, delete
- **ReportAPI** — generate reports: products, postings, stocks, returns, discounted
- **AnalyticsAPI** — analytics data, stock on warehouses, delivery time
- **FinanceAPI** — finance transactions, cash flow, realization reports
- **Receipt** — receipts for Ozon Global
- **SellerRating** — seller rating summary and history
- **Digital** — digital products: upload codes, stock import

### Beta Methods
- **Quants** — quant info
- **ReviewAPI** — product reviews: list, comment, change status
- **Questions&Answers** — customer Q&A: list, answer, manage
- **SellerActions** — seller promotions: create discounts/vouchers/installments
- **FBP** — Fulfillment by Partner: drafts, orders, labels, acts

### Premium Methods
Extended chat, analytics, and finance methods (require Premium access)

### Ozon Delivery
Order management for Ozon Delivery: delivery check, map, checkout, orders, cancellations

### Push Notifications
Webhook setup for real-time notifications: new postings, cancellations, status changes, stock updates, chat messages. Includes the **Notification API** for managing webhook URLs (set, update, enable/disable, list, check, delete) and listing supported push types.

## Documentation Files

For the **complete endpoint index** with all 380 endpoints, see:
- [endpoints-index.md](endpoints-index.md) — quick lookup table of all endpoints

For **detailed endpoint documentation** (request/response schemas, descriptions), see:
- [reference/description.md](reference/description.md) — guides: intro, auth, environment, process
- [reference/basic_methods.md](reference/basic_methods.md) — all basic method endpoints (products, orders, warehouses, finance, etc.)
- [reference/beta_methods.md](reference/beta_methods.md) — beta endpoints (reviews, Q&A, seller actions, FBP)
- [reference/premium_methods.md](reference/premium_methods.md) — premium method endpoints
- [reference/ozon_delivery.md](reference/ozon_delivery.md) — Ozon Delivery endpoints
- [reference/errors.md](reference/errors.md) — error codes and handling
- [reference/push_notifications.md](reference/push_notifications.md) — push notification setup and Notification API

For the **full OpenAPI spec** (Swagger 3.0), see:
- [swagger.json](swagger.json) — complete machine-readable spec with all schemas

## How to Use This Skill

1. When the user asks about a specific endpoint, look it up in `endpoints-index.md` first
2. For request/response details, read the relevant `reference/*.md` file
3. For complex schema questions, consult `swagger.json` directly
4. Always include `Client-Id` and `Api-Key` headers in code examples
5. All Ozon Seller API endpoints use POST method
6. Generate working code examples in the user's preferred language
