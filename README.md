# Taco Bell Scraper [Just 💰$0.72]

Extract structured data from [tacobell.com](https://tacobell.com) — tacobell.com menus: item name, category, price in USD, calories and image for every item. Pass a store number to get that restaurant's own prices, or leave it empty for the national menu. Structured rows, ready for a spreadsheet or price monitor.

**[Taco Bell Scraper [Just 💰$0.72] on Apify →](https://apify.com/blackfalcondata/taco-bell-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword and location. Filter by 📝 description format, and more.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from tacobell.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from tacobell.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "tacos",
  "location": "040890",
  "maxResults": 100,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Menu category to scrape — a slug ("tacos"), a path ("/food/tacos") or a full category URL. Leave the whole menu by passing a JSON array of several categories, e.g. ["tacos","burritos"]. Required if no startUrls provided. |
| `location` | string | — | Taco Bell store number (up to 6 digits, e.g. "040890") to get that restaurant's own prices. Leave empty for the national default menu prices. |
| `addresses` | array | — | Street addresses, ZIP codes or "City, ST" to find the nearest Taco Bell stores for. Each store's own menu and prices are returned. Ignored when Start URLs are set. |
| `storeNumbers` | array | — | Several Taco Bell store numbers (up to 6 digits each, e.g. "040890"). Combine with a category, or leave the category empty for the whole menu of each store. Ignored when Start URLs are set. |
| `maxStoresPerAddress` | integer | `1` | How many of the nearest stores to use for each address (1-50). |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw Taco Bell menu category URLs (e.g. https://www.tacobell.com/food/tacos?store=040890). Each URL becomes its own task; results are merged and deduped across all URLs. When provided, startUrls REPLACE the category/store fields (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeUnpriced` | boolean | `true` | Keep menu items the restaurant lists without a price (price is null). Turn off to get only priced items. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitExpired` | boolean | `false` | When incremental, also return a row with changeType EXPIRED for every item that disappeared from the menu since the previous run. Requires reading every requested menu completely; removals are only reported after a complete run. |
| `skipReposts` | boolean | `false` | When incremental, skip listings whose content matches an expired listing from a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED listings. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of listings included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results (including any contact details). Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every product returns the same 46-field schema. Missing values are `null` — never omitted.

- `type`
- `storeNumber`
- `storeName`
- `storeAddress`
- `storeCity`
- `storeState`
- `storePostalCode`
- `storeCountry`
- `storeLatitude`
- `storeLongitude`
- `storePhone`
- `searchedAddress`
- `storeTimeZone`
- `priceScope`
- `itemId`
- `itemName`
- `categoryName`
- `categories`
- `productType`
- `foodType`
- `categorySlug`
- `primaryCategory`
- `itemType`
- `price`
- `priceFormatted`
- `currency`
- `calories`
- `isModifiable`
- `isPurchasable`
- `accurateCalories`
- `isAvailableInStore`
- `isVegetarian`
- `maxOrderQuantity`
- `imageUrl`
- `productUrl`
- `portalUrl`
- `imageUrls`
- `sourceUrl`
- `listingId`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "type": "menu-item",
  "storeNumber": "040890",
  "storeName": null,
  "storeAddress": "801 West 7th Street",
  "storeCity": "Los Angeles",
  "storeState": "CA",
  "storePostalCode": "90017",
  "storeCountry": "US",
  "storeLatitude": null,
  "storeLongitude": null,
  "storePhone": null,
  "searchedAddress": "040890"
}
```

*Truncated — full records contain 46 fields. See Output fields for the complete schema.*

**[Try Taco Bell Scraper [Just 💰$0.72] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/taco-bell-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.00072 |

See the [actor on Apify](https://apify.com/blackfalcondata/taco-bell-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape tacobell.com?**
Use this actor on Apify to extract structured data from tacobell.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get tacobell.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape tacobell.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check tacobell.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Taco Bell Scraper [Just 💰$0.72]](https://apify.com/blackfalcondata/taco-bell-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 11*
