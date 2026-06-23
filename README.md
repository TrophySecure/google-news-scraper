[Google News Scraper](https://apify.com/scrapeio/google-news-scraper?fpr=data)

# Google News Scraper – Extract Headlines, Sources & Links by Keyword | Apify Actor

**Google News Scraper** collects headlines, publication dates, sources, snippets, and direct article URLs from [Google News](https://news.google.com) for any keyword — export up to 2,000 deduplicated results per run to CSV or JSON, Excel‑ready out of the box.

## 🧠 Overview

This **Google News Scraper** is an Apify Actor built for journalists, PR agencies, SEO analysts, investors, and data teams who need **real‑time news monitoring at scale** without the cost or complexity of Google's paid News API. Point it at a keyword, set how many unique articles you need (1–2,000), and the Actor pulls from the public Google News RSS layer, merges multiple time windows (`1h`, `7d`, `30d`, `1y`, `all`), follows pagination automatically, and deduplicates by GUID and link. Perfect for **brand monitoring**, **competitive intelligence**, **PR measurement**, **stock market sentiment tracking**, and **research datasets** — no API keys, no quotas, no rate‑limited SDKs.

## ✨ Features

- **Extract** headlines, publish dates, source names, snippets, and direct article URLs in one structured Dataset.
- **Search** any keyword or phrase just like typing into Google News.
- **Scrape** up to **2,000 unique articles per run** with automatic deduplication across feed passes.
- **Merge** multiple RSS time windows (`1h`, `24h`, `7d`, `30d`, `1y`, `all`) to maximize depth on a single keyword.
- **Unwrap** Google's redirect URLs to return the **best‑effort publisher URL** when available.
- **Export** Excel‑friendly **CSV** (UTF‑8 BOM, quoted fields, CRLF) plus JSON in one run.
- **Monitor** run diagnostics via `OUTPUT.meta.stoppedReason` (`completed`, `partial_no_more_feed`, `no_items_or_http_error`).
- **Automate** with the Apify scheduler for daily, hourly, or custom‑cron news pipelines.
- **Integrate** with n8n, Make, Zapier, Google Sheets, and BigQuery via the Apify REST API and webhooks.
- **Skip** Google Cloud setup — no News API key, no billing, no OAuth.

## 🎯 Use Cases

- **Brand Monitoring:** Track every mention of your company, product, or executives across thousands of publishers in real time.
- **Competitive Intelligence:** Watch competitor product launches, hiring announcements, and funding events by scraping their name or ticker daily.
- **PR & Media Measurement:** Measure share of voice, earned‑media coverage, and campaign pickup across trade and mainstream press.
- **Investment & Stock Research:** Feed headline datasets into sentiment models for tickers, commodities, or macro events like `Fed rate decision`.
- **SEO & Content Planning:** Spot trending topics, breaking news, and publisher angles for timely SEO content and editorial calendars.
- **AI Training Datasets:** Build headline or news‑summary corpora for LLM fine‑tuning, classification, or retrieval‑augmented generation.

## ⚙️ Input Parameters

| Name | Type | Required | Description | Example |
| --- | --- | --- | --- | --- |
| `keyword` | string | Yes | Search term or phrase (aliases: `q`, `query`, `searchQuery`). | `"artificial intelligence"` |
| `numberOfResults` | integer | Yes | Unique articles to collect, 1–2000 (aliases: `maxResults`, `results`). | `200` |

## 📤 Output Example (JSON)

```
{
  "position": 1,
  "keyword": "artificial intelligence",
  "title": "OpenAI unveils next‑gen reasoning model for enterprise customers",
  "link": "https://news.google.com/rss/articles/CBM...",
  "articleUrl": "https://www.reuters.com/technology/openai-unveils-next-gen-reasoning-model-2026-04-21/",
  "pubDate": "Tue, 21 Apr 2026 14:22:00 GMT",
  "sourceName": "Reuters",
  "description": "The new model targets regulated industries with improved factual accuracy and audit logs.",
  "guid": "CBMiU2h0dHBzOi8vd3d3LnJldXRlcnMuY29t..."
}
```

## 📋 Output Data Schema

Every article row includes these fields:

| Field | Type | Description |
| --- | --- | --- |
| `position` | integer | 1‑based rank in the current run. |
| `keyword` | string | The search keyword used. |
| `title` | string | Article headline (HTML stripped). |
| `link` | string | URL as returned by the RSS feed (often a Google News redirect). |
| `articleUrl` | string | Best‑effort **publisher URL** unwrapped from Google's redirect. |
| `pubDate` | string | Publication timestamp from the feed. |
| `sourceName` | string | Publishing outlet (e.g. `Reuters`, `Bloomberg`). |
| `description` | string | Snippet / summary text (HTML stripped). |
| `guid` | string | Stable feed ID — used for deduplication. |

## ▶️ How to Use

1. **Run on Apify Console:** Open the [Google News Scraper Actor page](https://apify.com/scrapeio/google-news-scraper), click **Try for free**, enter your keyword and `numberOfResults`, and press **Start**. Download results from the **Dataset** tab or `RESULTS_CSV` in Storage.
2. **Via API:** Call the Actor with the [Apify API](https://docs.apify.com/api/v2) using `ApifyClient` to fetch the dataset on run completion.
3. **Via CLI:** Run with the [Apify CLI](https://docs.apify.com/cli):

```
$apify call scrapeio/google-news-scraper --input='{"keyword":"climate policy","numberOfResults":500}'
```

## 🔗 API Example (JavaScript)

```
const { ApifyClient } = require('apify-client');

const client = new ApifyClient({ token: 'YOUR_APIFY_TOKEN' });

const run = await client.actor('scrapeio/google-news-scraper').call({
  keyword: 'semiconductor supply chain',
  numberOfResults: 100,
});

const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(`Fetched ${items.length} unique headlines`);
items.slice(0, 5).forEach(a => console.log(`${a.pubDate} — ${a.sourceName}: ${a.title}`));
```

## 📈 Why Use This Google News Scraper?

- **Speed & Automation:** Pull hundreds of fresh headlines per keyword in seconds — no manual searching, no copy‑paste, no scraper maintenance.
- **Scale & Reliability:** Smart multi‑window RSS merging plus `rel="next"` pagination delivers depth, while GUID/link deduplication keeps rows unique.
- **Excel‑Ready Exports:** `RESULTS_CSV` uses UTF‑8 BOM, quoted fields, and CRLF line endings — opens cleanly in Excel, Google Sheets, or Power BI.
- **Publisher URL Resolution:** The Actor unwraps Google's redirect links when possible, giving you the **direct publisher URL** for click‑tracking, sentiment pipelines, and citation.
- **Scheduler & Webhooks:** Combine with the Apify scheduler to run every hour, and webhook the new Dataset into Slack, HubSpot, or your data warehouse.
- **No API Quotas:** Skip Google Cloud's paid News API, enterprise contracts, and throttles — this Actor uses the public RSS layer.

## ❓ FAQ

**Q: Is it legal to scrape Google News?**
Google News RSS feeds are public. Storing headlines and links is widely considered acceptable for monitoring and research — but full‑text republishing is governed by publisher copyright and Google's [terms](https://policies.google.com/terms). Always respect source outlets' robots and ToS.

**Q: Does this call the paid Google News API?**
No. This Actor fetches the public `news.google.com/rss/search` feed. There is no Google Cloud project, billing account, or News API key involved.

**Q: Why do I get fewer articles than `numberOfResults`?**
Google's index may not have that many unique stories for your keyword. Check `OUTPUT.meta.stoppedReason`: `partial_no_more_feed` means the feed was exhausted; `completed` means the target was met.

**Q: Can I change country or language?**
The current release locks locale to `hl=en-US` / `gl=US` for reproducible RSS URLs. Custom locales are on the roadmap — contact the maintainer for priority.

**Q: What output formats are supported?**
Dataset exports to **CSV, JSON, Excel, XML, and HTML**. An Excel‑friendly `RESULTS_CSV` and a full `RESULTS_JSON` are also written to the key‑value store.

**Q: Does it handle rate limits?**
Yes — requests are spaced and retried on transient HTTP errors. You can also add Apify Proxy if needed for high‑frequency scheduled runs.

**Q: Is `articleUrl` always the publisher URL?**
When Google wraps the publisher URL in a redirect, the Actor attempts to unwrap it. When unwrapping fails, `articleUrl` falls back to the RSS `link` (a Google News URL).

**Q: Can I run this on a schedule?**
Yes. Use the **Apify scheduler** to trigger the Actor every hour, day, or custom cron, and webhook new items into Slack, email, or a database.

## 📣 Start Monitoring Google News Today

**[Run the Google News Scraper on Apify now →](https://apify.com/scrapeio/google-news-scraper)** and turn any keyword into a clean, deduplicated stream of headlines, sources, and links.

---

## 🔗 Related Scrapers by ScrapeIO

Combine with these Apify Actors to build full media‑intelligence pipelines:

- **[Google Maps Scraper](https://apify.com/scrapeio/google-maps-scraper-advance)** — local businesses, reviews, and contacts.
- **[Amazon Multi‑Marketplace Scraper](https://apify.com/scrapeio/amazon-multi-marketplace-scraper-most-advanced)** — ASINs, prices, and rankings across 23 regions.
- **[Facebook Ad Library Scraper](https://apify.com/scrapeio/meta-facebook-ad-scrapper-using-ad-library-url-premium)** — Meta ads by keyword, Page ID, or URL.
- **[Instagram Ads Scraper](https://apify.com/scrapeio/instagram-scraper-premium)** — Instagram‑only ads with creative and copy.
- **[WhatsApp Ads Scraper](https://apify.com/scrapeio/whatsapp-scraper-premium)** — Click‑to‑WhatsApp ad intelligence.
- **[Facebook Ad Library Brand Finder](https://apify.com/scrapeio/facebook-ad-library-suggestions)** — resolve brand names to verified Page IDs.
- **[YouTube Video Downloader](https://apify.com/scrapeio/youtube-downloader)** — download videos or audio to cloud storage.

---

**[FastAd](https://www.fastad.in)** — automation & growth tooling.