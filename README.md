# Crawlbase (crawlbase)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
