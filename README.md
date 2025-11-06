# AI Overview Scraper

[![AI Overview scraper by cloro](https://github.com/cloro-dev/google-ai-overview-scraper/blob/main/aioverview-scraper-hero-image.png)](https://cloro.dev/aiooverview/?utm_source=github)

[![cloro](https://img.shields.io/badge/Powered%20by-Cloro-blue?style=for-the-badge)](https://cloro.dev/)

The [AI Overview Scraper](https://cloro.dev/aiooverview/) by cloro enables developers to programmatically interact with Google's AI Overview and automatically collect comprehensive search result analysis and AI-curated insights along with structured metadata. Instead of manual data collection, you can retrieve results as parsed JSON, raw HTML, or other formats for seamless integration into your workflows.

You can use cloro's AI Overview Scraper for search result monitoring, trend analysis, and comprehensive topic overviews. It handles dynamic AI-generated content, supports real-time extraction, and eliminates the need to manage authentication, sessions, or anti-bot systems.

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

\* Mandatory parameters

---

### Output samples

The AI Overview Scraper API returns a structured JSON object containing AI Overview's comprehensive summary and metadata.

**Structured JSON output snippet:**

```json
{
  "success": true,
  "result": {
    "text": "Artificial intelligence in healthcare has revolutionized patient care, diagnostics, and drug discovery in 2025. Key developments include AI-powered diagnostic tools with 95% accuracy, personalized treatment plans, and robotic surgery assistance...",
    "sources": [
      {
        "position": 1,
        "url": "https://example.com/healthcare-ai-report",
        "label": "Healthcare AI Institute",
        "description": "Comprehensive analysis of AI applications in healthcare and medical innovation..."
      },
      {
        "position": 2,
        "url": "https://example.com/medical-tech-trends",
        "label": "Medical Technology Journal",
        "description": "Latest developments in medical AI technology and their clinical applications..."
      }
    ],
    "html": "<div>Artificial intelligence in healthcare has revolutionized patient care...</div>",
    "markdown": "**Artificial intelligence in healthcare** has revolutionized patient care, diagnostics, and drug discovery..."
  }
}
```

## Comprehensive search analysis

Google AI Overview provides comprehensive search result analysis with AI-curated insights and synthesized information from multiple sources.

### AI Overview features

- **Topic synthesis**: Comprehensive overviews that synthesize information from multiple sources
- **Current developments**: Integration of recent research and developments in the queried field
- **Multi-source analysis**: AI-powered analysis that combines insights from various authoritative sources
- **Structured insights**: Well-organized information that breaks down complex topics into understandable components
- **Search result curation**: Intelligent selection and presentation of the most relevant search results

### Sources array structure

Each source in the `result.sources` array contains:

| Field         | Type    | Description                                   |
| ------------- | ------- | --------------------------------------------- |
| `position`    | integer | Position order of the source in the response  |
| `url`         | string  | Direct URL to the source content              |
| `label`       | string  | Source name or publication                    |
| `description` | string  | Brief description of what the source contains |

## Practical AI Overview scraper use cases

1. **Market research:** Generate comprehensive overviews of markets, industries, or topics for business intelligence.
2. **Executive summaries:** Create detailed summaries for leadership and decision makers.
3. **Research synthesis:** Combine information from multiple sources into coherent overviews.
4. **Competitive analysis:** Get AI-curated insights on competitive landscapes and industry dynamics.
5. **Educational content:** Produce comprehensive educational materials and topic explanations.
6. **Strategic planning:** Inform strategic decisions with comprehensive topic analysis and insights.

## Why choose cloro?

- **Simple integration:** Clean API design with comprehensive documentation and examples.
- **Reliable performance:** >99% uptime and low latencies (P50 < 30s, P90 < 60s)
- **No infrastructure hassle:** We handle rate limiting and browser management.
- **Comprehensive analysis:** Access to AI Overview's synthesized insights and multi-source analysis.
- **Developer support:** Responsive support team to help with integration and troubleshooting.

## FAQ

### Is scraping AI Overview allowed?

Any website is legal to be scraped as long as the information is publicly accessible.

### What makes cloro's AI Overview scraper unique?

cloro's AI Overview endpoint provides reliable access to Google's AI Overview with:

- **Comprehensive topic synthesis** from multiple authoritative sources
- **AI-curated insights** for enhanced understanding and analysis
- **Structured data extraction** for seamless integration into your workflows

### What's the recommended timeout for requests?

We don't recommend putting any timeout, given that our system retries automatically. We recommend setting up a retry mechanism in case of failure.

### Does the API support different countries?

Yes, you can specify country codes like `US`, `GB`, `DE`, `JP`, `CN`, `IN`, `BR` and more to get localized results relevant to specific regions.

### What kind of queries work best with AI Overview?

AI Overview excels at comprehensive topic overviews, market research queries, industry analysis, and any request that would benefit from synthesized information from multiple sources.

## Learn more

For detailed documentation, advanced features, and integration guides, visit:

- **API documentation:** [docs.cloro.dev](https://docs.cloro.dev)
- **AI Overview scraper page:** [cloro.dev/aioverview](https://cloro.dev/aioverview/)

## Contact us

If you have questions or need support, reach out to us on [our contact page](https://cloro.dev/contact).

---

_Built with ❤️ by the cloro team_
