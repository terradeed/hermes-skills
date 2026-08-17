---
name: terradeed-web-data-extraction
description: "Extract structured data from any URL using TerraDeed's x402-powered API. Clean markdown, structured JSON, and UK property intelligence with no API key required."
version: 1.0.0
author: TerraDeed Labs
license: MIT
platforms: [linux, macos, windows]
dependencies: []
metadata:
  hermes:
    tags: [web-scraping, data-extraction, x402, structured-data, property-data, company-intelligence, markdown, mcp]
    category: research
    related_skills: []
---

# TerraDeed Web Data Extraction

Extract structured data from any public URL using TerraDeed's x402-powered endpoints. No API key required — pay per call with USDC on Base mainnet.

## MCP Server (Recommended)

Install the official MCP server for native integration:

```bash
npm install -g terradeed-mcp-server@0.2.1
```

Or use the hosted endpoint:
```
https://terradeed.0mcp.dev/mcp
```

The MCP server exposes three tools:
- `scrape_url` — Clean markdown from any URL
- `extract_data` — Structured JSON with named fields
- `extract_property` — UK commercial property intelligence

## HTTP Endpoints

| Endpoint | Price | What It Returns |
|----------|-------|-----------------|
| `POST /scrape` | $0.01 USDC | Clean, LLM-ready markdown |
| `POST /extract` | $0.05 USDC | Structured JSON with confidence scores |
| `POST /extract/property` | $0.10 USDC | UK property data + government enrichment |

**Base URL:** `https://api.terradeed.co.uk`

---

## 1. Scrape URL to Markdown

Returns clean markdown from any public URL. Use `js_render: true` for SPAs.

### Request

```bash
curl -X POST https://api.terradeed.co.uk/scrape \
  -H "Content-Type: application/json" \
  -d '{"url": "https://example.com", "js_render": false}'
```

### Response

```json
{
  "content": "# Example Domain\n\nThis domain is for use in illustrative examples...",
  "url": "https://example.com",
  "status": "success",
  "word_count": 28,
  "title": "Example Domain",
  "js_rendered": false,
  "auth_method": "x402"
}
```

---

## 2. Extract Structured Data

Returns JSON with named fields you specify. Great for company research, job listings, product specs.

### Request

```bash
curl -X POST https://api.terradeed.co.uk/extract \
  -H "Content-Type: application/json" \
  -d '{
    "url": "https://example.com",
    "fields": ["company_name", "industry", "headquarters", "funding_stage"]
  }'
```

### Response

```json
{
  "url": "https://example.com",
  "status": "success",
  "data": {
    "company_name": "Example Corp",
    "industry": "Software",
    "headquarters": "London, UK",
    "funding_stage": "Series B"
  },
  "fields_requested": ["company_name", "industry", "headquarters", "funding_stage"],
  "fields_extracted": ["company_name", "industry", "headquarters", "funding_stage"],
  "js_rendered": false,
  "model": "claude-sonnet-4-6",
  "auth_method": "x402"
}
```

### Common Field Sets

**Company Research:**
`["company_name", "industry", "headquarters", "company_size", "funding_stage", "key_executives", "recent_news"]`

**Job Listings:**
`["job_title", "salary_range", "location", "employment_type", "required_skills", "experience_level", "application_url"]`

**Product Specs:**
`["product_name", "price", "currency", "availability", "description", "specifications", "brand", "sku"]`

---

## 3. Extract UK Property Intelligence

Returns structured property data from any UK commercial listing URL. Auto-enriched with flood risk, listed building status, and EPC data.

### Request

```bash
curl -X POST https://api.terradeed.co.uk/extract/property \
  -H "Content-Type: application/json" \
  -d '{"url": "https://www.savills.co.uk/commercial-property-for-sale/unit-5-bristol-road-bs1-4na"}'
```

### Response

```json
{
  "url": "https://www.savills.co.uk/...",
  "status": "success",
  "listing_type": "sale",
  "property": {
    "address": "Unit 5, Bristol Road, BS1 4NA",
    "asking_price": 850000,
    "currency": "GBP",
    "site_area_sqft": 3600,
    "use_class": "E",
    "tenure": "freehold",
    "epc_rating": "D",
    "frontage_road": "Bristol Road",
    "description_summary": "Prominent corner unit..."
  },
  "constraints": {
    "flood_zone": "Flood Zone 3",
    "listed_building": "Grade II"
  },
  "enrichment": {
    "flood_risk": { "flood_zone": "Flood Zone 3", "source": "environment_agency" },
    "heritage": { "listed_building": "Grade II", "source": "historic_england" },
    "epc": { "rating": "D", "source": "dluhc_epc_register" }
  },
  "confidence": {
    "address": 0.95,
    "asking_price": 0.90,
    "site_area": 0.85,
    "use_class": 0.80
  }
}
```

---

## Payment

TerraDeed uses x402 — an open payment standard on HTTP 402. No API key needed.

1. First request returns 402 with payment requirements
2. Sign an EIP-3009 TransferWithAuthorization
3. Retry with payment signature

**Simpler option:** Use the MCP server or contact TerraDeed for API key access.

**Wallet needed:** USDC on Base mainnet. Even $1.00 covers 100+ scrapes.

---

## Pricing

| Calls | Cost (USDC) |
|-------|-------------|
| 10 scrapes | $0.10 |
| 10 extracts | $0.50 |
| 10 properties | $1.00 |
| Mixed batch (5 each) | $0.80 |

---

## Links

- **Website:** https://terradeed.co.uk
- **API Docs:** https://api.terradeed.co.uk/docs
- **MCP Server:** `npm install -g terradeed-mcp-server@0.2.1`
- **Hosted MCP:** https://terradeed.0mcp.dev/mcp
- **GitHub:** https://github.com/terradeed
- **Support:** contact@terradeed.co.uk
