[Google News Scraper](https://apify.com/andok/google-news-scraper?fpr=data)

# Google News Scraper for Monitoring & Alerts

Monitor Google News for brand mentions, competitor activity, and topic coverage across any keyword. Whether you need daily PR clipping, real-time crisis monitoring, or trend research, this actor delivers structured article data from Google News RSS feeds without heavy DOM scraping. Schedule it hourly on Apify to build a continuous news intelligence pipeline.

## Features

- **Keyword monitoring** — search Google News by any query and get matching articles
- **Bulk queries** — process multiple search terms in a single run
- **Publisher extraction** — automatically parses the source publisher from each headline
- **Language targeting** — filter results by language and region (e.g. `en-US`, `de-DE`, `uk-UA`)
- **Structured output** — every article includes title, link, publisher, and publication date
- **No browser needed** — queries official Google News RSS endpoints for fast, reliable results
- **Pay-per-result pricing** — only pay for articles found, with charge-limit safety built in

## Input

| Field | Type | Required | Default | Description |
| --- | --- | --- | --- | --- |
| `queries` | `array` | Yes | — | List of search keywords to monitor (e.g. `artificial intelligence`, `Tesla earnings`) |
| `maxItems` | `integer` | No | `50` | Maximum number of articles to return per query (1-100) |
| `language` | `string` | No | `en-US` | Language and region code for results (e.g. `en-US`, `de-DE`, `fr-FR`) |

### Input Example

```
{
  "queries": ["artificial intelligence", "OpenAI funding"],
  "maxItems": 25,
  "language": "en-US"
}
```

## Output

Each dataset item represents one news article. Key fields:

- `query` (`string`) — the search term that matched this article
- `title` (`string`) — article headline
- `link` (`string`) — URL to the full article
- `publisher` (`string`) — name of the publishing outlet
- `pubDate` (`string`) — publication date in ISO format

### Output Example

```
{
  "query": "artificial intelligence",
  "title": "Google DeepMind unveils new AI model for protein folding",
  "link": "https://www.reuters.com/technology/google-deepmind-ai-protein-2025-01-15/",
  "publisher": "Reuters",
  "pubDate": "Wed, 15 Jan 2025 14:30:00 GMT"
}
```

## Pricing

| Event | Cost |
| --- | --- |
| Article Found | Pay-per-event (see actor pricing page) |

## Use Cases

- **PR & brand monitoring** — track mentions of your company or product across news outlets daily
- **Competitive intelligence** — monitor competitor launches, funding rounds, and press coverage
- **Market research** — gather news sentiment data on industries, trends, or emerging technologies
- **Crisis detection** — schedule hourly runs to catch negative coverage early
- **Content curation** — feed article data into newsletters, dashboards, or Slack channels
- **AI/LLM pipelines** — supply fresh news context to RAG applications and AI agents

## Related Actors

| Actor | What it adds |
| --- | --- |
| [RSS & Atom Feed Parser](https://apify.com/andok/rss-parser) | Parse any RSS/Atom feed beyond Google News |
| [Hacker News Scraper](https://apify.com/andok/hackernews-scraper) | Track tech community discussions and trending topics |
| [Wayback Machine Scraper](https://apify.com/andok/wayback-machine-scraper) | Retrieve historical versions of articles and pages |

## Notes

- Google News RSS feeds are public but may rate-limit aggressive usage. Space scheduled runs at least 15 minutes apart for best reliability.
- Results depend on Google News indexing; very recent articles (under ~5 minutes old) may not appear immediately.