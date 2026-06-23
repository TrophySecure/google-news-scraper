[Google News Scraper](https://apify.com/thirdwatch/google-news-scraper?fpr=data)

# Google News Scraper

> Scrape Google News by keyword — headlines, sources, publication dates, descriptions, and article URLs across languages and countries.

## What you get

Structured Google News results for any keyword. Supports time ranges, language codes, and country codes so you can target local coverage. Ideal for media monitoring, trend analysis, and building news datasets.

## Output fields

| Field | Description |
| --- | --- |
| `title` | Article headline |
| `source` | News source name |
| `published_date` | Publication date |
| `description` | Article summary |
| `url` | Link to the full article |
| `language` | Article language |
| `country` | Country of the news source |

## Example output

```
{
    "title": "Tech stocks rally as AI spending surges",
    "source": "Reuters",
    "published_date": "2026-04-09",
    "description": "Major tech companies reported increased AI infrastructure spending...",
    "url": "https://www.reuters.com/technology/...",
    "language": "en",
    "country": "US"
}
```

## Input parameters

| Parameter | Required | Description |
| --- | --- | --- |
| `queries` | Yes | List of search keywords. Each query returns up to `maxResults` articles. Examples: `["artificial intelligence", "climate change"]`. |
| `maxResults` | No | Max articles per query. Default `25`, max `100`. |
| `language` | No | Language code — `en`, `es`, `fr`, `de`, `hi`, etc. Default `en`. |
| `country` | No | Country code — `US`, `GB`, `IN`, `DE`, etc. Default `US`. |
| `timeRange` | No | Time filter. Examples: `1h` (last hour), `1d` (last day), `7d` (last week), `30d` (last month). Leave empty for no time filter. |

## Use cases

- **PR teams**: Monitor brand and executive mentions in the news.
- **Analyst teams**: Track industry and competitor coverage over time.
- **Content teams**: Find trending stories worth covering.
- **Researchers**: Build longitudinal datasets on how a topic is covered.
- **Investors**: Follow news flow around tickers, sectors, or named companies.

 

## Use cases & recipes

Step-by-step guides on [thirdwatch.dev/blog](https://thirdwatch.dev/blog):

- [Build a News Aggregator with Google News (2026 Guide)](https://thirdwatch.dev/blog/build-news-aggregator-with-google-news)
- [Monitor Competitor Press Coverage with Google News (2026)](https://thirdwatch.dev/blog/monitor-competitor-press-coverage-with-google-news)
- [Scrape Google News for Brand Monitoring at Scale (2026)](https://thirdwatch.dev/blog/scrape-google-news-for-brand-monitoring)
- [Track Industry News with Google News at Scale (2026)](https://thirdwatch.dev/blog/track-industry-news-with-google-news-rss)

 -end

## Pricing

Pay-per-result pricing. Tiered discounts apply automatically based on usage volume.

| Tier | Price per result |
| --- | --- |
| FREE | $0.0015 |
| BRONZE | $0.00125 |
| SILVER | $0.001 |
| GOLD | $0.00085 |

## Limitations

- Returns article metadata, not full article body text.
- Paywalled sources still appear but the linked page may be gated.
- Rankings reflect Google News's algorithm and vary by country and language.
- `timeRange` is best-effort — very narrow windows in low-coverage topics may return fewer than `maxResults`.

## Compared to alternatives

- **vs. lukaskrivka/google-news-scraper**: This actor exposes language, country, and a short time-range syntax (`1h`/`1d`/`7d`/`30d`) in a single input shape, and is pay-per-result so test runs cost cents.

Pairs well with [Google Search Scraper](https://apify.com/thirdwatch/google-search-scraper) and [Twitter/X Profile Scraper](https://apify.com/thirdwatch/twitter-scraper) for broad news and social monitoring.

## FAQ

**Can I monitor a brand name continuously?**
Yes. Schedule this actor on Apify and write results to your dataset of choice. Combine multiple spellings or query variants.

**Which languages and countries work?**
Any standard ISO language and country code that Google News supports — `en`, `es`, `fr`, `de`, `hi`, and country codes like `US`, `GB`, `IN`, `DE`, `JP`.

**Do I get the full article text?**
No — just the metadata Google News exposes. Pair with a content extraction actor if you need article bodies.

**How recent can results be?**
Set `timeRange: "1h"` for the last hour. Sparse topics may return fewer items at tight ranges.

Last verified: 2026-04

More scrapers at [thirdwatch.dev](https://thirdwatch.dev).