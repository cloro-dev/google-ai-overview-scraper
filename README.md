# Google AI Overview Scraper

[![Google AI Overview scraper by cloro](https://github.com/cloro-dev/google-ai-overview-scraper/blob/main/aioverview-scraper-hero-image.png)](https://cloro.dev/ai-overview/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [Google AI Overview Scraper](https://cloro.dev/ai-overview/) by cloro lets developers programmatically interact with Google's AI Overview and collect search result analysis and AI-curated insights with structured metadata. You can retrieve results as parsed JSON, raw HTML, or other formats for integration into your workflows.

You can use cloro's AI Overview Scraper for search result monitoring, trend analysis, and topic overviews. It handles dynamic AI-generated content, supports real-time extraction, and removes the need to manage authentication, sessions, or anti-bot systems.

## How it works

The AI Overview scraper handles the rendering, parsing, and delivery of results in your requested format. You provide your search query, API credentials, and optional parameters as shown below.

### Request sample (Python)

```python
import json
import requests

# API parameters
payload = {
    'prompt': 'Overview of artificial intelligence in healthcare 2025',
    'country': 'US',
    'include': {
        'markdown': True
    }
}

# Get a response
response = requests.post(
    'https://api.cloro.dev/v1/monitor/aioverview',
    headers={'Authorization': 'Bearer YOUR_API_KEY'},
    json=payload
)

# Print response to stdout
print(response.json())

# Save response to a JSON file
with open('response.json', 'w') as file:
    json.dump(response.json(), file, indent=2)
```

### Request sample (cURL)

```bash
curl -X POST https://api.cloro.dev/v1/monitor/aioverview \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Overview of artificial intelligence in healthcare 2025",
    "country": "US",
    "include": {
      "markdown": true
    }
  }'
```

### Request sample (Node.js)

```javascript
const axios = require("axios");

const payload = {
  prompt: "Overview of artificial intelligence in healthcare 2025",
  country: "US",
  include: {
    markdown: true,
  },
};

axios
  .post("https://api.cloro.dev/v1/monitor/aioverview", payload, {
    headers: {
      Authorization: "Bearer YOUR_API_KEY",
      "Content-Type": "application/json",
    },
  })
  .then((response) => {
    console.log(response.data);
  })
  .catch((error) => {
    console.error("Error:", error);
  });
```

### Request parameters

| Parameter          | Description                                                                 | Default value |
| ------------------ | --------------------------------------------------------------------------- | ------------- |
| `prompt`\*         | The search query or topic for overview generation (1-10,000 characters)     | –             |
| `country`          | Optional country/region code for localized results (e.g., `US`, `GB`, `DE`) | `US`          |
| `include.markdown` | Include response in Markdown format when set to true                        | `false`       |
| `include.html`     | Include URL to full HTML response when set to true (URL expires after 24h)  | `false`       |

\* Mandatory parameters

---

### Output samples

The AI Overview Scraper API returns a structured JSON object containing AI Overview's comprehensive summary and metadata.

**Structured JSON output snippet:**

```json
{
  "success": true,
  "result": {
    "text": "Artificial intelligence in healthcare has changed patient care, diagnostics, and drug discovery in 2025. Key developments include AI diagnostic tools with 95% accuracy, personalized treatment plans, and robotic surgery assistance...",
    "sources": [
      {
        "position": 1,
        "url": "https://example.com/healthcare-ai-report",
        "label": "Healthcare AI Institute",
        "description": "Analysis of AI applications in healthcare and medical innovation..."
      },
      {
        "position": 2,
        "url": "https://example.com/medical-tech-trends",
        "label": "Medical Technology Journal",
        "description": "Latest developments in medical AI technology and their clinical applications..."
      }
    ],
    "videos": [
      {
        "url": "https://www.youtube.com/watch?v=example123",
        "title": "AI in Healthcare: Latest Innovations",
        "thumbnail": "https://example.com/thumb.jpg",
        "source": "HealthTech Channel",
        "platform": "YouTube",
        "date": "2 days ago",
        "duration": "12:34"
      }
    ],
    "html": ["https://storage.cloro.dev/results/c45a5081-808d-4ed3-9c86-e4baf16c8ab8/page-1.html"], // each URL is one rendered SERP page; expires after 24 hours
    "markdown": "**Artificial intelligence in healthcare** has changed patient care, diagnostics, and drug discovery...[Healthcare AI Institute](https://example.com/healthcare-ai-report)[Medical Technology Journal](https://example.com/medical-tech-trends)"
  }
}
```

## Comprehensive search analysis

Google AI Overview provides search result analysis with AI-curated insights and information synthesized from multiple sources.

### AI Overview features

- **Topic synthesis**: Overviews that synthesize information from multiple sources
- **Current developments**: Recent research and developments in the queried field
- **Multi-source analysis**: AI analysis that combines insights from various authoritative sources
- **Structured insights**: Information organized into understandable components
- **Search result curation**: Selection and presentation of relevant search results
- **Video content extraction**: Automatic extraction of relevant videos with metadata including thumbnails, duration, and source information

### Sources array structure

Each source in the `result.sources` array contains:

| Field         | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| `position`    | integer | Position order of the source in the response  |
| `url`         | string  | Direct URL to the source content              |
| `label`       | string  | Source name or publication                    |
| `description` | string  | Brief description of what the source contains |

### Videos array structure

When videos are present in the AI Overview, the `result.videos` array contains extracted video information:

| Field       | Type   | Description                           |
| ----------- | ------ | ------------------------------------- |
| `url`       | string | Direct video URL (e.g., YouTube link) |
| `title`     | string | Video title                           |
| `thumbnail` | string | Thumbnail image URL                   |
| `source`    | string | Channel or source name                |
| `platform`  | string | Video platform (e.g., YouTube)        |
| `date`      | string | Upload date (e.g., "2 days ago")      |
| `duration`  | string | Video duration (e.g., "12:34")        |

> Only `url` is guaranteed. Every other field is best-effort: Google does not attach every piece of metadata to every video card, and `thumbnail` and `duration` in particular are only available when Google renders the rich carousel preview (roughly 60% and 15% of videos in practice). Check for field presence before reading.

### Ads array structure

When Google injects sponsored ads inside the AI Overview, the `result.ads` array contains both text ads and shopping/product ads:

| Field         | Type    | Description                                      |
| ------------- | ------- | ------------------------------------------------ |
| `position`    | integer | Position of the ad (1-indexed)                   |
| `title`       | string  | Ad title                                         |
| `url`         | string  | Ad destination URL                               |
| `domain`      | string  | Domain name of the advertiser                    |
| `description` | string  | Ad description text                              |
| `price`       | object  | Product price for shopping ads (`{ value, currency }`) |
| `oldPrice`    | object  | Original price before discount (`{ value, currency, raw }`) |
| `store`       | string  | Retailer name for shopping ads                   |

## Practical AI Overview scraper use cases

1. **Market research:** Generate overviews of markets, industries, or topics for business intelligence.
2. **Executive summaries:** Create summaries for leadership and decision makers.
3. **Research synthesis:** Combine information from multiple sources into coherent overviews.
4. **Competitive analysis:** Get AI-curated insights on competitive landscapes and industry dynamics.
5. **Educational content:** Produce educational materials and topic explanations.
6. **Strategic planning:** Inform strategic decisions with topic analysis and insights.

## Why choose cloro?

- **Simple integration:** Clean API design with documentation and examples.
- **Reliable performance:** >99% uptime and low latencies (P50 < 30s, P90 < 60s)
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Synthesized analysis:** Access to AI Overview's synthesized insights and multi-source analysis.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping AI Overview allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's AI Overview scraper unique?

cloro's AI Overview endpoint provides access to Google's AI Overview with:

- **Topic synthesis** from multiple authoritative sources
- **AI-curated insights** for understanding and analysis
- **Structured data extraction** for direct integration into your workflows

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP`, `CN`, `IN`, `BR` and more to get localized results relevant to specific regions.

### What kind of queries work best with AI Overview?

AI Overview handles topic overviews, market research queries, industry analysis, and requests that benefit from synthesized information from multiple sources.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [cloro.dev/docs](https://cloro.dev/docs/)
- **AI Overview scraper page:** [cloro.dev/ai-overview](https://cloro.dev/ai-overview/)

## Other available scrapers

- **[AI Mode](https://cloro.dev/ai-mode/)** - Extracts structured data from Google AI Mode for general knowledge queries, workflow optimization, and technical guidance.
- **[AI Overview](https://cloro.dev/ai-overview/)** - Extracts structured data from Google AI Overview for comprehensive search result analysis and AI-curated insights.
- **[ChatGPT](https://cloro.dev/chatgpt/)** - Extracts structured data from ChatGPT with advanced features including shopping cards, raw response data, and query fan-out.
- **[Copilot](https://cloro.dev/copilot/)** - Extracts structured data from Microsoft Copilot for development tools, Microsoft ecosystem research, and enterprise-focused queries.
- **[Gemini](https://cloro.dev/gemini/)** - Extracts structured data from Google Gemini for complex reasoning, content generation, and source confidence scoring.
- **[Google Search](https://cloro.dev/google-search/)** - Extracts structured data from Google Search results, including organic results, People Also Ask questions, related searches, and optional AI Overview data.
- **[Google News](https://cloro.dev/google-news/)** - Extracts structured news articles from Google News with titles, snippets, sources, dates, and thumbnail images for news monitoring and media tracking.
- **[Grok](https://cloro.dev/grok/)** - Extracts structured data from Grok for current events, news tracking, and real-time information gathering.
- **[Perplexity](https://cloro.dev/perplexity/)** - Extracts comprehensive structured data from Perplexity AI with real-time web sources, automatically detecting and extracting rich data objects.

## Contact us

If you have questions or need support, reach out to us at [support@cloro.dev](mailto:support@cloro.dev).

---

Built with ❤️ by the cloro team
