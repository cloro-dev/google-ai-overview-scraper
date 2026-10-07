# Google AI Overview Scraper

[![Google AI Overview Scraper by cloro](https://github.com/cloro-dev/google-ai-overview-scraper/blob/main/aioverview-scraper-hero-image.png)](https://cloro.dev/ai-overview/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google AI Overview scraper](https://cloro.dev/ai-overview/?utm_source=github) by cloro returns the AI Overview block as structured JSON: the answer text and markdown, every cited source with position, and the organic results alongside it for comparison.

## How do you scrape Google AI Overview?

1. Get an API key at [cloro.dev](https://cloro.dev/?utm_source=github&utm_medium=readme).
2. POST a query to `https://api.cloro.dev/v1/monitor/google`.
3. Read the parsed fields from the JSON response.

AI Overview is returned by the Google Search endpoint with `include.aioverview`, in the same response as the organic results, so you can compare what ranks against what gets cited in one request. The two diverge sharply: citations from top-10 organic pages fell from 76% to 38% over the period Ahrefs measured across 4 million URLs.

### Request sample (Python)

```python
import requests

payload = {
    'query': 'what is a serp api',
    'country': 'US',
    'include': {'aioverview': {'markdown': True}},
}

response = requests.post(
    'https://api.cloro.dev/v1/monitor/google',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload,
)

print(response.json())
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/google \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query": "what is a serp api", "country": "US", "include": {"aioverview": {"markdown": true}}}'
```

Node.js and async/webhook examples are in the [endpoint documentation](https://cloro.dev/docs/api-reference/endpoint/monitor-google).

### Request parameters

| Parameter | Description | Default |
| --- | --- | --- |
| `query`\* | The search query | – |
| `country`\* | Country code for localized results (`US`, `GB`, `DE`). Required unless you send `gl` | – |
| `location` | [Google canonical location name](https://developers.google.com/google-ads/api/reference/data/geotargets) for geo-targeting. Mutually exclusive with `uule` | – |
| `uule` | Pre-encoded Google UULE string. Mutually exclusive with `location` | – |
| `device` | `desktop`, `mobile`, `ios` or `android` | `desktop` |
| `pages` | Number of result pages to return | `1` |
| `include.aioverview` | Send `{"markdown": true}` to get the AI Overview block, with Markdown | – |
| `include.paaAioverview` | Include AI Overviews inside People Also Ask | `false` |
| `include.html` | Return URLs to the full HTML, one per page (expire after 24h) | `false` |

\* Required

## What data does the Google AI Overview scraper return?

```json
{
  "success": true,
  "result": {
    "aioverview": {
      "text": "A SERP API returns search engine results as structured data...",
      "markdown": "A **SERP API** returns search engine results...",
      "sources": [
        { "position": 1, "url": "https://example.com/serp-api", "label": "What is a SERP API", "description": "A SERP API returns search results as JSON..." }
      ]
    },
    "organicResults": [
      { "position": 1, "title": "SERP API guide", "link": "https://example.com/guide", "snippet": "How SERP APIs work..." }
    ]
  }
}
```

1. **`aioverview.text`** and **`aioverview.markdown`** — the Overview answer.
2. **`aioverview.sources`** — every cited URL with position, label and description. This is the field that matters for GEO work.
3. **`organicResults`** — returned in the same response, so you can measure the gap between ranking and citation on the same query.

Full field-level schemas are in the [AI Overview reference](https://cloro.dev/docs/api-reference/endpoint/google/ai-overview).

## Use cases

- **Citation tracking** — whether your domain is cited in the Overview, and at what position.
- **Rank-versus-citation analysis** — the same response carries both, so the gap is measurable directly.
- **Trigger-rate research** — how often an Overview fires for a query class, which varies enormously by intent.
- **Competitive monitoring** — which domains Google's Overview trusts on your category's questions.

## FAQ

### How is AI Overview different from AI Mode?

Different content systems on the same SERP. Measured across 1.3 million AI Mode citations, the two cite the same URLs only 13.7% of the time. Use the [AI Mode scraper](https://cloro.dev/ai-mode/) for that surface.

### Does an AI Overview appear on every query?

No, and the rate varies sharply by intent rather than averaging out. Commercial and question-shaped queries trigger it far more often than navigational ones, and `GET /v1/countries?model=aioverview` lists the countries where Google offers it. When Google shows none, including in a country it doesn't serve, the response is a 200 with no `aioverview` key, and the [AI Overview add-on](https://cloro.dev/docs/guides/providers#google-search-multi-page-pricing) is still charged.

### Can I get AI Overviews inside People Also Ask?

Yes, via `include.paaAioverview`.

### Why do my Overview counts differ between runs?

Overviews are not deterministic, and a parser miss looks the same as an absent Overview: in both cases the `aioverview` key is missing. Sample by hand periodically to separate the two before trusting a trend line.

## Learn more

- **Endpoint reference:** [cloro.dev/docs](https://cloro.dev/docs/api-reference/endpoint/google/ai-overview)
- **Product page:** [cloro.dev/ai-overview](https://cloro.dev/ai-overview/)

## Other cloro scrapers

[AI Mode](https://cloro.dev/ai-mode/) · [ChatGPT](https://cloro.dev/chatgpt/) · [Copilot](https://cloro.dev/copilot/) · [Gemini](https://cloro.dev/gemini/) · [Google Search](https://cloro.dev/google-search/) · [Google News](https://cloro.dev/google-news/) · [Grok](https://cloro.dev/grok/) · [Perplexity](https://cloro.dev/perplexity/)

## Contact us

Questions or support: [ask the docs AI assistant](https://cloro.dev/docs/?assistant).
