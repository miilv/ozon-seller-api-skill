# Ozon Seller API — Claude Code Skill

A [Claude Code skill](https://docs.anthropic.com/en/docs/claude-code/skills) that gives Claude full access to the **Ozon Seller API** documentation (369 endpoints).

When active, Claude can look up any Ozon API endpoint, generate working code examples, and help you build integrations with the Ozon marketplace.

## Installation

### As a plugin (recommended)

Add this repo as a [Claude Code plugin](https://docs.anthropic.com/en/docs/claude-code/plugins):

```bash
claude plugins add https://github.com/miilg/ozon-seller-api-skill
```

### Manual install

Clone into your personal skills directory:

```bash
git clone https://github.com/miilg/ozon-seller-api-skill.git ~/.claude/skills/ozon-seller-api
```

Or into a project:

```bash
git clone https://github.com/miilg/ozon-seller-api-skill.git .claude/skills/ozon-seller-api
```

## Usage

Claude activates the skill automatically when you mention Ozon API, or you can invoke it directly:

```
/ozon-seller-api
```

### Examples

```
How do I create a product via Ozon API?
Show me how to update stock levels for FBS warehouses
What endpoints are available for order management?
Generate a Python script to fetch my product list from Ozon
```

## What's Included

| File | Description |
|------|-------------|
| `SKILL.md` | Main skill — overview, auth, API structure |
| `endpoints-index.md` | Quick lookup table of all 369 endpoints |
| `swagger.json` | Full OpenAPI 3.0 spec (English) |
| `reference/description.md` | Guides: intro, auth, OAuth, environment |
| `reference/basic_methods.md` | Basic endpoints with request/response schemas |
| `reference/beta_methods.md` | Beta endpoints (reviews, Q&A, FBP) |
| `reference/premium_methods.md` | Premium method endpoints |
| `reference/ozon_logistics.md` | Ozon Logistics endpoints |
| `reference/errors.md` | Error codes and handling |
| `reference/push_notifications.md` | Webhook/push notification setup |

## API Coverage

369 endpoints across 66 tags including:

- **Products** — create, update, delete, archive, images, ratings
- **Prices & Stocks** — update prices, stock levels, discounts
- **Orders (FBO/FBS/rFBS/FBP)** — list, ship, cancel, labels, acts
- **Warehouses** — list, create, configure FBS/rFBS warehouses
- **Finance** — transactions, cash flow, realization reports
- **Analytics** — sales data, stock analytics, delivery time
- **Promotions** — Ozon promos and seller actions
- **Returns & Cancellations** — manage returns and cancellation requests
- **Chat** — customer communication
- **Reports** — generate various reports
- **Push Notifications** — webhook setup for real-time events

## Source

Documentation sourced from the [official Ozon Seller API docs](https://docs.ozon.ru/api/seller/en/).

## License

The API documentation content belongs to Ozon. This skill packages it for use with Claude Code.
