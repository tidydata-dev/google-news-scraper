# Google News Scraper — Python, Node.js & cURL examples

Search **Google News** for any keyword, company or `site:` and get **title, publisher, publish date, related coverage and the real article URL** (not Google's redirect), in 15 countries/languages with time filters. **$1 per 1,000 articles.**

Hosted on Apify: **[Google News Scraper](https://apify.com/tidydata/google-news-scraper)**. These examples show how to call it from your code; the Actor runs in the cloud, no API key for the source site, no proxies to manage.

## Python

```bash
pip install apify-client
export APIFY_TOKEN=...   # free account at apify.com → Settings → API & Integrations
```

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("tidydata/google-news-scraper").call(run_input={
    "queries": [
        "\"electric cars\"",
        "site:reuters.com tesla"
    ],
    "editions": [
        "US:en",
        "GB:en"
    ],
    "timeRange": "7d"
})
for item in client.dataset(run["defaultDatasetId"]).iterate_items():
    print({k: item.get(k) for k in ['publishedAt', 'publisher', 'title', 'url']})
```

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('tidydata/google-news-scraper').call({"queries": ["apify"], "timeRange": "7d"});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items.length, items[0]);
```

## cURL (one call, results as JSON)

```bash
curl -s -X POST "https://api.apify.com/v2/acts/tidydata~google-news-scraper/run-sync-get-dataset-items?token=$APIFY_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"queries": ["apify"], "timeRange": "7d"}'
```

## More TidyData Actors

[YouTube Transcript](https://apify.com/tidydata/youtube-transcript-scraper) · [YouTube Comments](https://apify.com/tidydata/youtube-comments-scraper) · [YouTube Scraper](https://apify.com/tidydata/youtube-channel-scraper) · [Website Contacts](https://apify.com/tidydata/website-contact-extractor) · [Google News](https://apify.com/tidydata/google-news-scraper) · [App Reviews](https://apify.com/tidydata/app-reviews-scraper) · [Merge & Dedupe](https://apify.com/tidydata/merge-dedupe-datasets) · [all of them](https://github.com/tidydata-dev)

Examples are MIT-licensed.
