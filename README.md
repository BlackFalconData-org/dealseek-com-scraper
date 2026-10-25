# DealSeek Scraper [Just 💰$1.8]

Extract structured data from [console.apify.com](https://console.apify.com) — Collect DealSeek product deals with current prices and promotion codes. Compare discounts and product ratings across categories. Export structured products to JSON or spreadsheets and monitor new or changed deals with incremental runs.

**[DealSeek Scraper [Just 💰$1.8] on Apify →](https://apify.com/blackfalcondata/dealseek-com-scraper?fpr=1h3gvi)**

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

**Search with filters** — Search by keyword and location. Filter by 🌍 country, description format, 🔀 sort order, and more.

**Detail enrichment** — Fetch full descriptions, structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per ${NOUN} to control output size and cost.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from console.apify.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from console.apify.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "*",
  "country": "US",
  "maxResults": 100,
  "includeDetails": true,
  "descriptionMaxLength": 0,
  "compact": false,
  "descriptionFormat": "all",
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Search products by keyword or ASIN, enter * to browse current deals, or use a JSON array for multiple searches. |
| `location` | string | — | Optional label retained for existing workflows. DealSeek product deals have no geographic location filter. |
| `country` | enum | `"US"` | Which market to search. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste DealSeek homepage, hot deals (/hot), top deals (/top), promo codes (/promo-codes), brand (/brand/...) or product (/dp/ASIN) URLs. URLs replace keyword searches; results are merged and deduplicated. Unsupported URLs are skipped; the run reports an input error if none is supported. |
| `includeDetails` | boolean | `true` | Apply full description and contact/link formatting to complete product records already returned by DealSeek. Does not make extra product requests or add fields absent from the source. |
| `descriptionMaxLength` | integer | `0` | Truncate description to N chars. 0 = no truncation. |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `descriptionFormat` | enum | `"all"` | Pick a single description representation. `all` keeps every variant; `text` / `html` / `markdown` drop the others. |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, locations, startUrls, country) so different filter sets never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
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
| `category` | string | — | Filter by a DealSeek category name. |
| `sortBy` | enum | `"default"` | Use the default order or sort by discount. |
| `onlyPromoCodes` | boolean | `false` | Return products with a promotion code. |
| `onlyCoupons` | boolean | `false` | Return products with a coupon. |
| `minDiscountPercent` | integer | `0` | Return products meeting this discount percentage. |
| `emitUnchanged` | boolean | `false` | In incremental mode, also return products whose tracked data has not changed. |
| `emitExpired` | boolean | `false` | In incremental mode, return disappeared products only after a complete successful scan. |

---

## Output fields

Every product returns the same 168-field schema. Missing values are `null` — never omitted.

- `affiliate_promo_percentage`
- `all_sizes_available`
- `asin`
- `available_sizes`
- `avg_rating`
- `bitwise_type`
- `bought_past_month`
- `brand_name`
- `category`
- `codes`
- `color`
- `comments_count`
- `coupon_value`
- `created_at`
- `custom_text`
- `deal_hash`
- `discount_percentage`
- `discounted_price`
- `discounts`
- `expired_at`
- `extra_discount_amount`
- `fashion_audiences`
- `has_brand_deal`
- `has_coupon`
- `has_extra_discount`
- `has_lightning_deal`
- `has_price_drop`
- `has_promo_code`
- `image`
- `image_ids`
- `is_amazon_choice`
- `is_brand_name`
- `is_deal`
- `is_fba`
- `is_good_deal`
- `is_hot`
- `is_lightning_deal`
- `is_parent_asin`
- `last_updated`
- `last_verified_promo`
- `likes_count`
- `list_price`
- `list_price_savings_percentage`
- `lowest_price_3_months`
- `manual_promo_percentage`
- `material`
- `merchant_id`
- `merchant_name`
- `num_variations`
- `parent_asin`
- `price`
- `price_drop_percentage`
- `promo_code`
- `promo_verifier_verified`
- `promo_verifier_verified_at`
- `ratings_count`
- `requires_paid_shipping`
- `size`
- `sns_base_discount`
- `sns_base_price`
- `sns_coupon_value`
- `stacking_discount_percentage`
- `subcategory`
- `title`
- `today_top_deals_position`
- `variation_asins`
- `affiliatePromoPercentage`
- `allSizesAvailable`
- `availableSizes`
- `avgRating`
- `bitwiseType`
- `boughtPastMonth`
- `brandName`
- `commentsCount`
- `couponValue`
- `createdAt`
- `customText`
- `dealHash`
- `discountPercentage`
- `discountedPrice`
- `expiredAt`
- `extraDiscountAmount`
- `fashionAudiences`
- `hasBrandDeal`
- `hasCoupon`
- `hasExtraDiscount`
- `hasLightningDeal`
- `hasPriceDrop`
- `hasPromoCode`
- `imageIds`
- `isAmazonChoice`
- `isBrandName`
- `isDeal`
- `isFba`
- `isGoodDeal`
- `isHot`
- `isLightningDeal`
- `isParentAsin`
- `lastUpdated`
- `lastVerifiedPromo`
- `likesCount`
- `listPrice`
- `listPriceSavingsPercentage`
- `lowestPrice_3Months`
- `manualPromoPercentage`
- `merchantId`
- `merchantName`
- `numVariations`
- `parentAsin`
- `priceDropPercentage`
- `promoCode`
- `promoVerifierVerified`
- `promoVerifierVerifiedAt`
- `ratingsCount`
- `requiresPaidShipping`
- `snsBaseDiscount`
- `snsBasePrice`
- `snsCouponValue`
- `stackingDiscountPercentage`
- `todayTopDealsPosition`
- `variationAsins`
- `id`
- `dealId`
- `location`
- `entityName`
- `type`
- `source`
- `url`
- `dealUrl`
- `amazonUrl`
- `upc`
- `scrapedAt`
- `listingId`
- `listingKey`
- `subtitle`
- `description`
- `descriptionText`
- `descriptionHtml`
- `descriptionMarkdown`
- `priceText`
- `priceValue`
- `priceCurrency`
- `listingType`
- `publishedAt`
- `publishedAge`
- `validThrough`
- `portalUrl`
- `contentHash`
- `imageUrl`
- `galleryUrls`
- `sellerName`
- `sellerType`
- `sellerUrl`
- `sellerWebsite`
- `contactEmail`
- `contactPhone`
- `extractedEmails`
- `extractedPhones`
- `extractedUrls`
- `socialProfiles`
- `attributes`
- `condition`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `changeType`
- `ranking_signals`
- `rankingSignals`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "affiliate_promo_percentage": 0,
  "all_sizes_available": true,
  "asin": "B0C6WTWQRM",
  "available_sizes": [
    "6Pack",
    "12Pack"
  ],
  "avg_rating": 4.2,
  "bitwise_type": 137218,
  "bought_past_month": 800,
  "brand_name": "Brightown",
  "category": "Patio, Lawn & Garden",
  "codes": [
    "AED92X78"
  ],
  "color": "Warm White(6PC)",
  "comments_count": 0
}
```

*Truncated — full records contain 168 fields. See Output fields for the complete schema.*

**[Try DealSeek Scraper [Just 💰$1.8] now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/dealseek-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0045 |
| Result | $0.0018 |

See the [actor on Apify](https://apify.com/blackfalcondata/dealseek-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape console.apify.com?**
Use this actor on Apify to extract structured data from console.apify.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get console.apify.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape console.apify.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check console.apify.com's terms of service for your specific use case.

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
2. Open [DealSeek Scraper [Just 💰$1.8]](https://apify.com/blackfalcondata/dealseek-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
