# Crawlbase (crawlbase)

Crawlbase (formerly ProxyCrawl) is a web crawling and scraping platform that fetches any web page through a large rotating proxy network with optional headless-Chrome JavaScript rendering, returning raw HTML, Markdown, screenshots, or structured JSON. A single token-authenticated REST host (`api.crawlbase.com`) exposes the Crawling API, a Scraper API of ready-made site extractors, Cloud Storage for crawled pages, a Screenshots API, and a Leads API for domain email discovery.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/crawlbase/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/crawlbase/refs/heads/main/apis.yml)

## Access Model (read this first)

- **Token in the query string, not a header.** Every request authenticates with `?token=YOUR_TOKEN`. There is no OAuth and no `Authorization` header.
- **Two tokens per account.** A **Normal (TCP) token** is for static HTML/JSON and is faster and cheaper. A **JavaScript token** renders the page in headless Chrome and is required for SPAs, lazy-loaded content, screenshots, and any rendering parameter (`page_wait`, `ajax_wait`, `css_click_selector`, `scroll`, `screenshot`).
- **Free trial.** New accounts get **up to 10,000 free requests** with no credit card, usable for both regular and JavaScript-rendered crawls.
- **Charge on success only.** Only responses with `pc_status: 200` count against quota; failed requests (timeouts, blocks, target 5xx) are free.
- **Two products are legacy / closed to new sign-ups.** The **Screenshots API** (closed 2024-11-01) and the **Leads API** (closed 2024-10-01) still function for existing integrations. For new work, use the Crawling API's `screenshot=true` and the Scraper API's `email-extractor` respectively. The **Scraper API** is likewise documented as legacy, superseded by the Crawling API's `&scraper=` / `&autoparse=true` parameters.

## Grounding & honesty

Endpoints, HTTP methods, and the token query-param auth model are confirmed from Crawlbase's public documentation. Query parameters are modeled from the documented parameter reference. Response bodies are raw upstream content (HTML/Markdown/JSON/image) or scraper-specific JSON whose fields vary per scraper, so response schemas in the OpenAPI are illustrative rather than exhaustive. Pricing and rate-limit artifacts are marked `reconciled: false` — plan names and monthly prices are captured from the pricing page but per-request dollar rates come from an in-page calculator and were not reconciled.

Crawlbase is a REST platform. **It does not expose a documented public WebSocket API.** Its only asynchronous mode is the Crawling API `async=true` option, which returns a request id and POSTs the result to a client-supplied `callback` webhook. See `review.yml`.

## Tags

- Web Scraping
- Web Crawling
- Web Intelligence
- Data Extraction
- Proxy
- Scraper API
- Data Collection
- SERP
- Web Data

## Timestamps

- **Created:** 2026-07-12
- **Modified:** 2026-07-12

## APIs

### Crawlbase Crawling API

Fetch any URL through Crawlbase's rotating proxy network and return the page as raw HTML, Markdown, or structured JSON. Supply the target as a URL-encoded `url` query param plus a `token`. Use the Normal (TCP) token for static pages or the JavaScript token to render in headless Chrome, with optional geo-routing, device profiles, waits, clicks, scrolling, and async callbacks.

- **Human URL:** [https://crawlbase.com/docs/crawling-api/](https://crawlbase.com/docs/crawling-api/)
- **Base URL:** `https://api.crawlbase.com`

#### Tags

- Web Crawling
- Web Scraping
- Proxy
- JavaScript Rendering

#### Properties

- [Documentation](https://crawlbase.com/docs/crawling-api/)
- [API Reference](https://crawlbase.com/docs/crawling-api/parameters/)
- [OpenAPI](openapi/crawlbase-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/crawlbase.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/crawlbase.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Crawlbase Scraper API

Return ready-parsed JSON for supported sites — Amazon, eBay, Walmart, Google SERP / Shopping / News / Maps / Scholar, LinkedIn, Instagram, TikTok, YouTube, Booking.com, TripAdvisor, Yelp, and more — by naming a `scraper`. Documented as a legacy endpoint; new users are pointed at the Crawling API's `&scraper=` and `&autoparse=true`.

- **Human URL:** [https://crawlbase.com/docs/scraper-api/](https://crawlbase.com/docs/scraper-api/)
- **Base URL:** `https://api.crawlbase.com/scraper`

#### Tags

- Data Extraction
- Scraper API
- Structured Data
- SERP

#### Properties

- [Documentation](https://crawlbase.com/docs/scraper-api/)
- [OpenAPI](openapi/crawlbase-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/crawlbase.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/crawlbase.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Crawlbase Cloud Storage API

Retrieve, list, and delete pages that Crawlbase persisted when a crawl was sent with `&store=true`. Fetch a stored page by `rid` or `url`, pull up to 100 at once via bulk endpoints, page through all request IDs, and read the total stored count. Pages are retained 14 days by default.

- **Human URL:** [https://crawlbase.com/docs/storage-api/](https://crawlbase.com/docs/storage-api/)
- **Base URL:** `https://api.crawlbase.com/storage`

#### Tags

- Cloud Storage
- Web Data
- Data Collection
- Caching

#### Properties

- [Documentation](https://crawlbase.com/docs/storage-api/)
- [OpenAPI](openapi/crawlbase-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/crawlbase.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/crawlbase.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Crawlbase Screenshots API

Capture a rendered screenshot of any URL in headless Chrome, as PNG or JPEG, in viewport or full-page mode, with configurable width, height, and device profile. Requires the JavaScript token. Legacy endpoint (closed to new sign-ups 2024-11-01); the Crawling API's `screenshot=true` covers this for new projects.

- **Human URL:** [https://crawlbase.com/docs/screenshots-api/](https://crawlbase.com/docs/screenshots-api/)
- **Base URL:** `https://api.crawlbase.com/screenshots`

#### Tags

- Screenshots
- Web Intelligence
- JavaScript Rendering
- Monitoring

#### Properties

- [Documentation](https://crawlbase.com/docs/screenshots-api/)
- [OpenAPI](openapi/crawlbase-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/crawlbase.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/crawlbase.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

### Crawlbase Leads API

Given a bare `domain`, return publicly visible email addresses associated with it, along with the source URLs where each was found, for lead generation and contact research. Billed at one credit per ten emails. Legacy endpoint (closed to new sign-ups 2024-10-01); the email-extractor scraper is the suggested alternative.

- **Human URL:** [https://crawlbase.com/docs/leads-api/](https://crawlbase.com/docs/leads-api/)
- **Base URL:** `https://api.crawlbase.com/leads`

#### Tags

- Leads
- Data Extraction
- Email Discovery
- Data Collection

#### Properties

- [Documentation](https://crawlbase.com/docs/leads-api/)
- [OpenAPI](openapi/crawlbase-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/crawlbase.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/crawlbase.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)

## Common Properties

- [Domain Security](security/crawlbase-domain-security.yml)
- [Authentication](authentication/crawlbase-authentication.yml)
- [GitHub Organization](https://github.com/crawlbase)
- [LinkedIn](https://www.linkedin.com/company/crawlbase)
- [Website](https://crawlbase.com/)
- [Documentation](https://crawlbase.com/docs)
- [Plans](plans/crawlbase-plans-pricing.yml)
- [Rate Limits](rate-limits/crawlbase-rate-limits.yml)
- [Fin Ops](finops/crawlbase-finops.yml)
- [Blog](https://crawlbase.com/blog)

## Maintainers

**FN:** Kin Lane
**Email:** kin@apievangelist.com
