[Google News Scraper](https://apify.com/khadinakbar/google-news-scraper?fpr=data)

# 📰 Google News Extractor — No Login, Full Text & Bulk Search

Extract structured news article data from Google News by keyword, topic, or URL — no login, API key, cookies, or browser required. Built for developers, data teams, AI/LLM pipelines, and automated news monitoring workflows.

Runs on [Apify](https://apify.com/) — schedule it, call it via API, chain it with other actors, or integrate it directly into your MCP-compatible AI agent.

---

## What It Does

This actor fetches Google News RSS feeds, parses them into clean structured records, and optionally visits article pages to extract full body text. Each run produces a consistent JSON dataset you can export to CSV, Excel, JSON, or push to any downstream system.

**Output fields per article:** `title`, `source_name`, `source_url`, `google_news_url`, `published_at`, `description`, `image_url`, `search_query`, `topic`, `full_text`, `word_count`, `scraped_at`

---

## Key Features

**No login or API key required** — works out of the box with no authentication setup.

**Bulk keyword search** — pass multiple search queries in one run. Each query gets its own RSS feed and result set. Run 50 queries in a single call.

**Built-in topic sections** — monitor World, Technology, Business, Science, Health, Sports, Entertainment, or Nation headlines with a single click.

**50+ region/language editions** — scrape Google News in any edition: US English, German, French, Japanese, Arabic, Spanish, and 45+ more. Every language correctly sets `hl`, `gl`, and `ceid` parameters.

**Google search operators supported** — use quotes for exact phrases, minus to exclude terms, `OR` for alternatives, and `site:` to filter by publisher:

| Operator | Example | Effect |
| --- | --- | --- |
| `"exact phrase"` | `"interest rate hike"` | Match exact wording |
| `-keyword` | `apple -fruit` | Exclude term |
| `OR` | `AI OR "machine learning"` | Either term |
| `site:` | `site:reuters.com earnings` | Publisher filter |

**Time range filtering** — filter by past hour, 24 hours, 7 days, 30 days, or 1 year.

**Full article text extraction** — optionally visit each article URL and extract the full body text. Produces `full_text` and `word_count` fields. Designed for AI/RAG pipelines, sentiment analysis, and NLP workloads.

**Deduplication** — across multiple queries and topics, duplicate articles are removed automatically.

**Custom topic URLs** — paste any Google News section URL to extract niche topic feeds not covered by built-in sections.

**Structured output schema** — fully defined dataset schema with typed fields, descriptions, and examples. MCP-compatible: field names and descriptions are optimized for LLM tool-use routing.

---

## Input Options

| Field | Description |
| --- | --- |
| `searchQueries` | Array of keywords/phrases. Supports Google operators. |
| `topics` | Built-in sections: WORLD, TECHNOLOGY, BUSINESS, etc. |
| `topicUrls` | Custom Google News section URLs (HTML or RSS format). |
| `startUrls` | Raw RSS feed URLs for advanced use. |
| `maxResultsPerQuery` | 1–100 articles per query/topic (default: 100). |
| `regionLanguage` | Edition code like `US:en`, `DE:de`, `JP:ja` (50+ options). |
| `timeRange` | `1h`, `1d`, `7d`, `30d`, `1y`, or `any`. |
| `extractFullText` | Visit article pages and extract body text. |
| `decodeUrls` | Attempt to resolve real article URLs from Google redirects. |
| `deduplicateResults` | Remove duplicate articles across queries (default: true). |

---

## Output Schema

Each article record has these fields:

| Field | Type | Description |
| --- | --- | --- |
| `title` | string | Article headline, cleaned (source name suffix removed) |
| `source_name` | string | Publisher name (e.g. "Reuters", "BBC News") |
| `source_url` | string|null | Real article URL (when resolvable) |
| `google_news_url` | string | Google News redirect URL (always present) |
| `published_at` | string | ISO 8601 publication datetime |
| `description` | string|null | Article snippet/summary |
| `image_url` | string|null | Article thumbnail image URL |
| `search_query` | string|null | The query that found this article |
| `topic` | string|null | Built-in topic section (if applicable) |
| `full_text` | string|null | Full article body text (requires `extractFullText`) |
| `word_count` | integer|null | Word count of full text (requires `extractFullText`) |
| `scraped_at` | string | ISO 8601 extraction timestamp |

---

## Usage Examples

### Monitor a brand or topic daily

```
{
  "searchQueries": ["OpenAI", "Anthropic", "Google DeepMind"],
  "maxResultsPerQuery": 20,
  "timeRange": "1d",
  "deduplicateResults": true
}
```

### Collect top headlines across sections

```
{
  "topics": ["TECHNOLOGY", "BUSINESS", "SCIENCE", "HEALTH"],
  "maxResultsPerQuery": 10
}
```

### AI/RAG pipeline — full text extraction

```
{
  "searchQueries": ["large language models", "AI regulation EU"],
  "maxResultsPerQuery": 15,
  "extractFullText": true,
  "timeRange": "7d"
}
```

### Monitor non-English news

```
{
  "searchQueries": ["Künstliche Intelligenz", "Bundesliga"],
  "regionLanguage": "DE:de",
  "maxResultsPerQuery": 25
}
```

### Bulk competitive intelligence

```
{
  "searchQueries": [
    "Tesla earnings", "Ford EV", "GM electric",
    "Rivian news", "Lucid Motors", "NIO stock",
    "BYD sales", "Volkswagen EV", "BMW electric", "Mercedes EV"
  ],
  "maxResultsPerQuery": 10,
  "timeRange": "7d",
  "deduplicateResults": true
}
```

---

## Performance & Cost

| Mode | Articles/min | Cost estimate |
| --- | --- | --- |
| Metadata only | ~200 | $0.003 per article |
| With full text | ~30–60 | $0.003 + $0.005 per article |

**Example costs:**

- 100 articles (metadata): **$0.30**
- 100 articles (with full text): **$0.80**
- 1,000 articles (metadata only): **$3.00**
- Daily monitoring, 10 queries × 20 articles: **$0.60/day**

Google News RSS returns up to 100 articles per feed. A single run with 10 search queries can collect up to 1,000 articles.

---

## API & MCP Integration

### Call via Apify API

```
curl -X POST "https://api.apify.com/v2/acts/khadinakbar~google-news-scraper/runs?token=YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"searchQueries": ["artificial intelligence"], "maxResultsPerQuery": 10}'
```

### Use in an AI agent (MCP)

This actor is optimized for use via the [Apify MCP server](https://apify.com/apify/actors-mcp-server). When an LLM agent calls `call-actor` with `khadinakbar/google-news-scraper`, the structured input schema and output schema enable precise tool-use routing without hallucination.

**Typical LLM agent prompt:**

> "Get the latest 10 news articles about AI regulation from the past week using Google News"

**Agent call:**

```
{
  "searchQueries": ["AI regulation"],
  "maxResultsPerQuery": 10,
  "timeRange": "7d"
}
```

---

## Comparison vs. Competitors

| Feature | This Actor | data_xplorer | easyapi | scrapestorm |
| --- | --- | --- | --- | --- |
| Output schema (typed) | ✅ | ❌ | ❌ | ❌ |
| MCP-optimized fields | ✅ | ❌ | ❌ | ❌ |
| Bulk keywords (1 run) | ✅ | ✅ | ❌ | ✅ |
| Custom topic URLs | ✅ | ❌ | ❌ | ❌ |
| Full text extraction | ✅ | ❌ | ❌ | ❌ |
| Deduplication | ✅ | ❌ | ❌ | ❌ |
| 50+ regions | ✅ | ✅ | ✅ | ✅ |
| Dataset schema | ✅ | ❌ | ❌ | ❌ |
| Limited permissions | ✅ | ❌ | ❌ | ❌ |

---

## Notes

- **`source_url`** may be null for some articles. Google News encodes article URLs using a Base64 scheme that requires JavaScript to decode. The `decodeUrls` option attempts HTTP redirect following but cannot resolve all URLs. The `google_news_url` field is always populated and can be used to visit the article in a browser.
- **`full_text`** requires `source_url` to be resolvable. Articles where `source_url` is null will have `full_text: null` even when `extractFullText` is enabled.
- **Rate limits**: Google News RSS feeds are public and rate-limit tolerant. This actor runs within safe request limits.
- **No blocked content**: This actor only fetches publicly available RSS data and article pages. It does not bypass paywalls.

---

## Support

Found an issue or want a feature? Open a request via the Issues tab on the actor page.