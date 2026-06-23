[Google News Scraper](https://apify.com/bot_kevin/Google-News-Scraper?fpr=data)

## Scrape all Google News data

Our Google News Scraper allows you to extract news metadata such as title, link, source, publication date. The search is query-based but you can also extract general news by using an empty query.

The actor has a simple interface where you can specify your query as well as the language you want to use for news titles. Google News Scraper doesn't limit the number of results you can view which is a significant advantage when compared to manual browsing using Google News website. While using a regular Google News search, you'll only get approximately 100 results for your query.

## How it works

```
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({
    token: '<YOUR_API_TOKEN>',
});

const input = {
    "query": "food", // or null (all)
    "lang": "en",
    "country": "US",
    "maxItems": 100
};

(async () => {

    const run = await client.actor("bot_kevin/Google-News-Scraper").call(input);
    const { items } = await client.dataset(run.defaultDatasetId).listItems();
   console.log(await items);

})();
```

## Sample output

```
{
  title: 'Miss Manners: Should I have told him that his fancy food was awful? - The Mercury News',
  description: 'Miss Manners: Should I have told him that his fancy food was awful?',
  link: 'https://news.google.com/rss/articles/M6Ly93d3cubWVWhpcy1mYW5jeS1mb29kLXdhcy1hd2Z1bC9hbXAv?oc=5',
  companyName: 'The Mercuryd News',
  companyUrl: 'https://www.mercurynews.om',
  pubDateUnix: 1709918779
}
```