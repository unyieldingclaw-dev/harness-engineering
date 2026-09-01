# Web Ingestion & Open-Source Scraping — 2026-09-01

**Status:** Research / candidate capability; no implementation or dependency adoption implied

## Why this matters

Austin Marchese's `/web-scraping` → `/ingest-source` pattern exposes a potentially useful capability boundary for Harness Engineering:

> acquire external web content → normalize it into durable project knowledge → preserve provenance

The useful architectural idea is not a particular vendor or skill name. It is a reusable ingestion capability that can handle ordinary HTML, JavaScript-rendered pages, crawling, extraction, and source provenance without making every downstream workflow solve web acquisition independently.

## Firecrawl

Firecrawl itself is open source and can be self-hosted. The core project is primarily licensed under AGPL-3.0, while some SDKs/UI components are MIT. Its cloud product adds capabilities beyond the open-source offering. The current product surface includes Search, Scrape, Parse, Crawl, Map, and Interact, with Markdown and structured JSON as primary outputs.

**Assessment:** Firecrawl is the closest direct match to the functionality Austin demonstrated, but the AGPL license and the operational complexity of self-hosting should be treated as explicit adoption considerations rather than incidental details.

## Open-source alternatives

### Crawl4AI

Crawl4AI is an LLM-oriented Python web crawler/scraper designed to produce AI-ready Markdown and extracted structured data. It supports browser-based JavaScript rendering through Playwright and has session management, hooks, extraction strategies, and other crawler controls.

The project currently identifies itself as Apache 2.0 with an attribution requirement.

**Assessment:** Strongest candidate for a PMB/Harness proof of concept if the desired implementation language is Python and the primary need is `URL → clean Markdown/structured content → durable artifact`.

### Crawlee

Crawlee is a general-purpose open-source crawling and browser automation library from Apify. It supports raw HTTP/HTML crawling plus Playwright/Puppeteer browser crawling, queues, retries, routing, persistence, and large-scale crawl management. Both JavaScript/TypeScript and Python implementations are available under Apache 2.0.

**Assessment:** Better foundation when the requirement is a robust crawler framework rather than a turnkey LLM-content extraction service. It is more infrastructure than the initial PMB use case appears to require.

### Stagehand

Stagehand is an open-source MIT-licensed browser automation SDK built around Playwright. Its `act`, `observe`, and `extract` primitives are useful for pages where deterministic selectors are insufficient and browser interaction itself is required.

**Assessment:** Useful complementary capability for interactive/agentic browsing, but not a direct replacement for a crawler/ingestion layer. Introducing an LLM into basic acquisition should be avoided unless deterministic browsing cannot solve the task.

## Capability comparison

| Capability | Firecrawl OSS | Crawl4AI | Crawlee | Stagehand |
|---|---|---|---|---|
| Clean Markdown for LLMs | Strong | Strong | Requires composition | Extraction-oriented |
| JS-heavy pages | Strong | Strong | Strong | Strong |
| Whole-site crawl | Strong | Strong | Strong | Not primary purpose |
| Structured extraction | Strong | Strong | Requires composition | Strong |
| Browser interaction | Strong | Available | Strong | Strong |
| Search/discovery | Strong in Firecrawl product | Limited / compose externally | Not primary purpose | Not primary purpose |
| Self-hostable | Yes | Yes | Yes | Yes |
| License | AGPL-3.0 core | Apache 2.0 + attribution | Apache 2.0 | MIT |
| Primary fit | Firecrawl-like ingestion service | LLM-friendly ingestion utility | Crawler infrastructure | Browser automation |

## Harness recommendation

Do **not** make Firecrawl, Crawl4AI, Crawlee, or Stagehand a Harness dependency yet.

Instead, define a provider-neutral **Web Acquisition / Ingestion capability** with an explicit contract:

```text
Input:
  URL(s) + acquisition policy

Acquire:
  HTTP fetch
  browser-rendered fetch when required
  optional crawl

Normalize:
  clean text / Markdown
  optional structured extraction

Record:
  canonical URL
  retrieval timestamp
  source title
  content hash
  acquisition method
  relevant HTTP/browser metadata

Output:
  durable source artifact
  provenance metadata
```

A provider can then implement that contract without downstream skills knowing whether the content came from Firecrawl, Crawl4AI, Crawlee, Playwright, or a simpler HTTP fetch.

### Important design constraint

Do not start with a general-purpose autonomous web agent.

The default path should be deterministic and cheap:

```text
HTTP fetch → extraction → artifact
             ↓ failure / JS required
        browser fetch
             ↓ failure / interaction required
        interactive browser
```

Likewise, do not make semantic search part of the acquisition primitive unless evidence shows it belongs there. Search/discovery and page acquisition are related but distinct capabilities.

## Proposed research/POC

The next useful experiment is small:

1. Pick a representative set of ordinary, documentation, and JS-heavy pages.
2. Acquire each with a simple HTTP/HTML path.
3. Fall back to browser rendering when necessary.
4. Compare Crawl4AI and Firecrawl OSS only if the baseline proves insufficient.
5. Measure extraction quality, provenance fidelity, runtime, operational complexity, and failure modes.
6. Determine whether the result is valuable enough to become a reusable Harness capability.

Success should be defined by the quality of the durable artifact and its provenance, not by how sophisticated the scraper is.

## Sources

- Firecrawl GitHub: https://github.com/firecrawl/firecrawl
- Firecrawl product/crawl documentation: https://www.firecrawl.dev/crawl
- Crawl4AI GitHub: https://github.com/unclecode/crawl4ai
- Crawlee GitHub: https://github.com/apify/crawlee
- Crawlee Python: https://github.com/apify/crawlee-python
- Stagehand GitHub: https://github.com/browserbase/stagehand

## Disposition

- **ADOPT:** Treat web acquisition/ingestion as a potential reusable capability boundary.
- **ASSESS:** Crawl4AI and Firecrawl OSS through a small representative POC.
- **ASSESS:** Crawlee if crawl orchestration becomes a demonstrated requirement.
- **PARK:** Stagehand unless interactive browser behavior is actually required.
- **REJECT FOR NOW:** Vendor-specific skill contracts and autonomous web-agent architecture as the default ingestion path.
