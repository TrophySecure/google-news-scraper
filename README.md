[Google News Scraper](https://apify.com/sourabhbgp/google-news-scraper?fpr=data)

# Google News Scraper — 100 Free | Search, Topics & Headlines

Scrape news articles from Google News. Search by keyword, browse by topic (technology, business, sports...), or get top headlines. Fast HTTP-only scraper — no browser needed. Supports all countries and languages.

## 💰 Pricing

- **100 free results** — lifetime, no credit card needed
- After that: **$2 per 1,000 results** (pay-per-event)

| Results | Cost |
| --- | --- |
| 100 | Free |
| 1,000 | $2 |
| 10,000 | $20 |
| 100,000 | $200 |

## 🔍 What can you scrape?

### Search

Search Google News by any keyword or phrase. Supports time operators (`when:1d`, `when:7d`) and site filters (`site:reuters.com`).

### Topics

Browse news by topic: technology, business, sports, entertainment, health, science, world, nation, or headlines.

### Headlines

Get the top headlines from Google News — the same stories shown on the Google News homepage.

## 📥 Input

| Field | Type | Default | Description |
| --- | --- | --- | --- |
| `urls` | string[] | `["headlines"]` | Search keywords, topic names, or `"headlines"` |
| `mode` | enum | auto-detect | `search`, `topic`, or `headlines` |
| `maxResults` | integer | 100 | Max results to return (0 = unlimited) |
| `language` | string | `en` | Language code (en, de, fr, ja...) |
| `country` | string | `US` | Country code (US, GB, IN, DE...) |

### Auto-Detection Rules

When `mode` is not set, the scraper auto-detects based on input:

- `"technology"`, `"business"`, `"sports"`, etc. → **topic** mode
- `"headlines"` or `"top"` → **headlines** mode
- Anything else (e.g., `"artificial intelligence"`) → **search** mode
- Set `mode` explicitly to override auto-detection

### Topic Names

| Topic | Code |
| --- | --- |
| headlines | h |
| world | w |
| nation | n |
| business | b |
| technology | t |
| entertainment | e |
| sports | s |
| health | m |
| science | snc |

### Example Inputs

**Search for a keyword:**

```
{
    "urls": ["artificial intelligence", "tesla stock"],
    "maxResults": 50
}
```

**Browse a topic:**

```
{
    "urls": ["technology"],
    "mode": "topic"
}
```

**Top headlines:**

```
{
    "urls": ["headlines"],
    "maxResults": 20
}
```

**Search with time filter:**

```
{
    "urls": ["OpenAI when:7d"],
    "mode": "search"
}
```

**Search specific site:**

```
{
    "urls": ["site:reuters.com climate"],
    "mode": "search"
}
```

## 📤 Output

Each result contains:

```
{
    "type": "search",
    "title": "AI Revolution Reshapes Global Economy",
    "source": "Reuters",
    "sourceUrl": "https://www.reuters.com",
    "googleUrl": "https://news.google.com/rss/articles/CBMi...",
    "publishedAt": "2026-04-06T12:00:00.000Z",
    "topic": null,
    "query": "artificial intelligence",
    "scrapedAt": "2026-04-06T14:30:00.000Z"
}
```

| Field | Description |
| --- | --- |
| `type` | `search`, `topic`, or `headlines` |
| `title` | Article headline (source name removed) |
| `source` | Publisher name (e.g., Reuters, BBC, CNN) |
| `sourceUrl` | Publisher website URL |
| `googleUrl` | Google-encoded article link (not the direct article URL) |
| `publishedAt` | Article publish date (ISO 8601) |
| `topic` | Topic name (only for topic mode, null otherwise) |
| `query` | Search query (only for search mode, null otherwise) |
| `scrapedAt` | When the data was scraped |

## 💡 Use Cases

- **News monitoring** — track keywords or topics across thousands of publishers
- **Media research** — analyze which outlets cover specific topics and how frequently
- **Trend tracking** — detect emerging stories by monitoring search results over time
- **Competitive intelligence** — monitor competitor mentions, product launches, and press coverage
- **Content curation** — aggregate news by topic for newsletters, dashboards, or feeds

## ⚡ Tips

- RSS feeds return up to **100 articles per query** — use multiple keywords to get more coverage
- Use `when:1d` or `when:7d` in search queries for time-filtered results (e.g., `"OpenAI when:1d"`)
- Use `site:domain.com` to filter by publisher (e.g., `"site:reuters.com climate"`)
- The `googleUrl` is a Google-encoded redirect link, not the direct article URL. The `sourceUrl` gives you the publisher's domain.
- Set `language` and `country` to get localized results (e.g., `language: "de"`, `country: "DE"` for Germany)
- Empty input `{}` defaults to top headlines — useful for health checks and quick tests