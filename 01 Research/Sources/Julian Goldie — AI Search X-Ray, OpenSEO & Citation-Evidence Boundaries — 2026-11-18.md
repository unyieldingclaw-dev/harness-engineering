# Julian Goldie — AI Search X-Ray, OpenSEO & Citation-Evidence Boundaries — 2026-11-18

**Disposition: RESEARCH ONLY — selectively borrow the evidence workflow; do not install, purchase or implement a new HE/PMB/ACR agent or SEO harness.**

**Date inspected:** November 18, 2026.  
**Source:** Julian Goldie SEO, ["NEW Claude Code SEO OS is INSANE (FREE)!"](https://www.youtube.com/watch?v=TklNxogu_OM) (about 19:35), associated screenshots (Canva, Zillow, Notion), and user-supplied transcript.  
**Primary code source:** [every-app/open-seo](https://github.com/every-app/open-seo) (`README.md`, `.agents/skills/competitor-analysis/SKILL.md`, `.agents/skills/ai-visibility-audit/SKILL.md`, `web/content/docs/mcp.mdx`, `web/content/docs/claude-code-plugin.md`, `docs/DATAFORSEO_API_KEY.md`) read directly from GitHub **main as observed on this date**, with selected blob SHAs below.  
**Provider docs:** Google [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), [AI features and your website](https://developers.google.com/search/docs/appearance/ai-features), and [Search documentation changelog](https://developers.google.com/search/updates); OpenSEO [AI visibility guide](https://openseo.so/docs/ai-visibility), [AI visibility audit](https://openseo.so/docs/skills/ai-visibility-audit), and [rank tracking](https://openseo.so/docs/rank-tracking).

## 1. Executive evidence assessment

The genuinely useful concept is a **bounded, source-tagged competitor analysis** that compares traditional search demand and ranking, featured snippet observations, AI-answer citation observations, and site crawl accessibility **without confusing those measurements**. Turn the source evidence into *one or two user-relevant recommendations*, each with citations and named unknowns. This is valuable as a research/reporting pattern, not as a novel HE runtime architecture.

The video labels its four layers **SEO**, **AEO**, **GEO** and **“AI SEO”**. Those labels are the creator's framing, not four independent canonical Google ranking systems.

- **SEO:** search results, keywords, page rankings and organic estimated traffic.
- **AEO:** featured snippets / answer boxes in a search results page, **not** the same event as citation in an AI Overview.
- **GEO:** source/brand citations within generated answers, which depend on query, engine, market, model and retrieval time.
- **AI SEO:** the creator's fourth checklist for robots/crawler reachability, `llms.txt`, answer placement, FAQs and machine readability. It mixes important standard accessibility requirements with speculative platform-specific tactics.

The claimed “three shapes AI copies” — **answer first, structure and proof** — are reasonable *human-readable writing principles*, but an observed correlation among selectively shown examples cannot establish that Google, ChatGPT or Perplexity systematically rewards those exact shapes. Google explicitly warns against “AEO/GEO hacks” or artificial text rewrites.

## 2. What the user-provided video actually shows (reported, not re-measured)

The video and screenshots show a single Claude Code prompt invoking `/ai-search-xray` with a domain, audience and desired output path. The output screenshots are *reports generated for the demonstration*, not public reproducibility artifacts: there are no preserved vendor response payloads, snapshot timestamps, request IDs, market/keyword coverage inventories or independently retrieved SERPs attached.

| Domain | Creator's asserted findings | Trust / limitations |
| --- | --- | --- |
| Canva | `22.7M` monthly US estimated Google visits, `716K` keywords, `88%` of traffic in the **top 100 queries** branded; `27` featured snippets and `177,922` search queries where Google's AI Overview reportedly cites Canva. Tool/template/reference pages feature in examples. | Counts **reported from video only**; search volume estimates are not real visitor logs; 88% subset is not necessarily 88% of whole-site traffic. “AI citation search count” is not number of individual user conversations, measured clicks, or proof of verbatim quotes. |
| Zillow | `~63M` monthly US organic estimated visits, `3.85M` keywords, `87%` of traffic in top ranked searches branded; `574,992` purported AI Overview citation queries. Example excerpts include a mortgage payment breakdown, dated rate summary, licensed company record and filter list. | Demonstration data, not independently verified. A crawler that sees `robots.txt` “allow” but receives WAF `403` is a relevant **access mismatch** to investigate, not proof that all Google or AI crawler fetches fail. |
| Notion | `llms.txt` and Markdown routes reportedly available; `~9K` cited search queries in chosen dataset, with much of sample branded; an inspected template page had an empty description. | This one page's empty field does not prove all Notion templates lack useful text, or that `llms.txt` creates citations. Query selection and sample coverage are not provided. |

The “almost nine in ten visits are branded” introduction has a **denominator ambiguity**: later Canva screenshot specifically says traffic from **top-100 keyword traffic**. Do not generalize to all site visitors or all search traffic without denominator data.

The demonstrated calls (“Calling openseo 7 times” and “Calling openseo 5 times”) do not provide tool names, payloads, cost, errors or whether data was cached. The full `ai-search-xray` `SKILL.md` and its custom logic were offered as a Boardroom member download and **were not provided or inspected**. Do not infer the paid skill is part of OpenSEO's audited public skills, secure by default, or truly a one-command/no-setup free workflow.

## 3. OpenSEO: verified repo architecture, capabilities and costs

**Project:** [every-app/open-seo](https://github.com/every-app/open-seo) is public open-source code with a hosted product, MCP and predefined skills. Its README describes keyword research, domain overview, rank tracking, backlinks, site audits and AI visibility. A remote OpenSEO MCP can be connected to Claude Code, Codex, Cursor and ChatGPT. A self-hosted option exists using Docker / Cloudflare / EveryApp; **running the UI or model locally does not make its keyword/citation data local or free**.

**Actual data dependencies and commercial boundary:**

- The OpenSEO README says SEO data requires a **pay-as-you-go DataForSEO key** or compatible reseller. `docs/DATAFORSEO_API_KEY.md` documents **$1 test credit and $50 minimum DataForSEO top-up**, with alternate reseller `treg`; these are documentation claims at inspection time, not quotes for future use.
- The README describes a hosted product and a **28% markup per DataForSEO request** for the hosted service. All actual pricing is liable to change; don't convert this to an unverified total for the demonstration.
- OpenSEO's **AI visibility** feature distinguishes DataForSEO-derived *questions* (mostly Google People Also Ask; **not logged real ChatGPT prompts or prompt frequencies**) from tracked API answers and their citations. It explicitly warns API-run answers may differ from consumer-product answers.
- A query's organic search volume is **not** AI prompt demand, a keyword count is **not** an audience count, and a citation-query count **is not** an AI impressions/clicks count.

**Audited public skill design is better than the marketing shorthand:**

- [`competitor-analysis/SKILL.md`](https://github.com/every-app/open-seo/blob/main/.agents/skills/competitor-analysis/SKILL.md), blob `1b2f791922cb7ff162257a8fbd9d5d51961f7761`: checks project context, reuses research within 30 days instead of rebuying, prefers first-party Search Console for your own domain, filters branded keywords, validates SERPs, labels inference, and explicitly warns not to infer page patterns from keywords alone. It **also mutates** project competitor context/research log and requires a reporting skill to save output. Thus it is not automatically a read-only analyst.
- [`ai-visibility-audit/SKILL.md`](https://github.com/every-app/open-seo/blob/main/.agents/skills/ai-visibility-audit/SKILL.md), blob `fcb12e340687d72b759e75e5f2431e5ce1c3a3a3`: uses an exact run ID and pagination, captures failed/no-answer prompts separately from negative responses, reads source pages, compares tracked engine answers, asks approval **before launching paid checks**, and bounds recommendations to one to three. It **also writes** saved key pages, research log, and potentially creates tracking runs/schedules after approval. It explicitly distinguishes `GPTBot` or `Google-Extended` training controls from search crawlers.
- [`web/content/docs/mcp.mdx`](https://github.com/every-app/open-seo/blob/main/web/content/docs/mcp.mdx), blob `f2a677d7676724e5fa51de8cc6133e42dfba58fe`: hosted MCP `https://app.openseo.so/mcp` is authenticated; user-scoped API keys can act on projects, save context and **spend credits**. A connected MCP does not imply read-only access. Inspect tool permissions and scopes before any connection.
- [`web/content/docs/claude-code-plugin.md`](https://github.com/every-app/open-seo/blob/main/web/content/docs/claude-code-plugin.md), blob `183d40109f85a5549a4b1fad06b45e1bc82cabff`: ships **twelve** OpenSEO skills, including SEO Audit, Competitor Analysis, AI Visibility Audit and AI Prompt Research. `ai-search-xray` is **not among the twelve listed bundled skills**. This does not prove the commercial skill's behavior, only that it is not part of the identified public bundle.
- [`docs/DATAFORSEO_API_KEY.md`](https://github.com/every-app/open-seo/blob/main/docs/DATAFORSEO_API_KEY.md), blob `af2b0d738a0b310423a104c8e594baffd35f1755`: documents account credentials/environment settings; **do not install or insert keys into global Claude config** as a research side effect.

**Not a full security/code review:** selected README, published skills and docs inspected; did not execute software, audit dependency graph, review every MCP implementation/API endpoint, run fee-bearing searches or validate the creator's proprietary skill.

## 4. Primary-source corrections — critical before anyone copies the blueprint

| Video recommendation or implication | Primary-source evidence / decision |
| --- | --- |
| “Add `llms.txt` so AI bots can find you.” | **Google Search does not use `llms.txt`**, including AI Overviews/AI Mode. Official [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) says it neither helps nor harms Google visibility. Maintain it only for a separately evidenced consumer or documentation-discovery use. |
| “Add `FAQPage` schema for AI snippets.” | Google [Search changelog](https://developers.google.com/search/updates) records FAQ rich result discontinued **May 7, 2026** and documentation removed later. A concise FAQ remains useful to people, but `FAQPage` markup does not establish AI citation preference or a current Google FAQ rich-result opportunity. |
| “Three short shapes copied word for word every time.” | Video shows selected examples, not a causal ranking study or a representative denominator. Google's official guide says no special **AI-first formatting, chunking or schema** is necessary for its AI search features; unique, accurate, people-first content and standard technical SEO are preferred. |
| “Let every AI bot in.” | `robots.txt` is **not a security control**. Crawlers differ in purpose (live search vs training vs on-demand fetches). Intentional legal/privacy/cost boundaries must be preserved; distinguish `Googlebot` Search controls from `Google-Extended` training permissions, and `OAI-SearchBot` from `GPTBot`. Audit exact access objectives first. |
| “`robots.txt` allows crawling, so AI can cite the page.” | A valid `robots.txt` is not enough: CDN/WAF, HTTP status, login, JS rendering, indexing and `nosnippet` may still limit access. Use first-party URL Inspection, crawl logs and targeted fetches. |
| “An estimated citation count represents users reached or traffic.” | Counts from sampled/provider keyword datasets are neither visits nor click attribution nor measured total conversation counts. Must include **dataset/source, locale, date, query set, engine/model, definition and unknown coverage**. |
| “Write a separate page for every calendar month / every buyer question.” | Google's AI optimization guide explicitly warns against mass-creating many query variation pages chiefly to manipulate AI/search ranking (scaled content abuse). Only publish genuinely useful and differentiated pages justified by real audience demand. |
| “A 3-minute one-prompt workflow means no technical work.” | OpenSEO data requires credentials, scopes and usually usage-based fees; custom skills need installation/inspection; reproducible reports need dated evidence. The demo elapsed time is not measured lifecycle effort or implementation assurance. |
| “Google rewards self-contained FAQ/how-to pages.” | This is a possible *correlation*. Google Search decides featured snippets programmatically and does not promise any particular document structure or schema will trigger a featured snippet. |

**Google Search's current guidance** (as of this inspection) is unusually direct: Google calls out `llms.txt`, tiny AI “chunks,” inauthentic mentions, and over-special schema as needless tactics. It offers first-party Search Console measurement and warns third parties do not have its internal ranking/AI systems. The creator's marketing advice is **not** equivalent to first-party mechanics.

## 5. What is actually transferable to Harness Engineering

Keep the analysis pattern, without importing marketing assumptions:

1. **Declare scope and objective.** Domain(s), market, intended customer, date, questions, acceptance criteria and allowed third-party services.
2. **Identify data source and denominator.** First-party console vs provider estimates vs fixed retrieved answers; record snapshot IDs and confidence; keep SEO keyword volume apart from AI prompt demand.
3. **Run bounded, auditable data collection.** Read relevant competitor pages/queries and limits; don't let tool retry/spend without approval; label inaccessible sources instead of filling gaps with assertions.
4. **Split observations, inference, proposed action.** A displayed citation and visible FAQ are observations; “Google chose it because of FAQ schema” is **unproven causal inference**.
5. **Output one or two actionable hypotheses.** Why they serve real user intent, what artifact to produce, what it costs/risks, and how a change would be validated. Limit output to actual scope.
6. **Verify actual outcomes over time.** A before/after comparison on controlled query, market, engine and timeframe can reveal movement; avoid promising attribution from a single rank snapshot.

This overlaps HE **Investigation Integrity, provenance, source-ownership and minimum durable correction** research. The useful transfer is a discipline around evidence/uncertainty *inside source reviews*, not a request for a new four-layer SEO agent, report platform, generic expert panel, or crawler.

### HE cross-project disposition

| Area | What may help | What to avoid |
| --- | --- | --- |
| **HE** | Confidence/evidence grades, denominators, validation thresholds, primary-vs-third-party distinction and deterministic report contracts | New “SEO harness” runtime, platform-specific heuristic policy, automated source rewriting |
| **PMB** | No new capability identified beyond provenance, source ownership and “capture vs confirm” | Replacing memory context with OpenSEO; importing browser crawlers into PMB release |
| **ACR** | Clarifies that source evidence ≠ model interpretation; do not confuse tooling scores with actual code-review outcome | Expensive data calls, SEO validation or additional agents |
| **Personal portfolio site** | **Possible separate future use:** a one-time normal SEO crawl/Google Search Console review and human-readable project/case-study pages, if the user chooses to optimize search discoverability | Buying AI Search X-Ray membership, duplicating pages solely for AI citation, deploying `llms.txt` as a ranking trick |
| **Home/Work dashboard** | None needed; independent dashboard scope remains parked | Adding OpenSEO MCP, dashboard routing, paid monitoring or corporate data flows |

## 6. Minimal future experiment (not authorized by this research)

If the **site owner's actual** discoverability problem is demonstrated: baseline one site in Google Search Console, inspect selected public pages and sitemap/robots access, and compare *a handful* of queries/page types against grounded competitors. Only if paid data truly improves the decision, estimate credit cost, get user approval, grant narrow project-scoped access and capture outputs for repeatability. The one useful experiment would compare organic impressions and actual visitors **before and after a user-facing content improvement**, treating changes in AI citations as noisy secondary observations.

Do not claim that a small personal portfolio's growth can be inferred from Canva/Zillow/Notion data. Their authority, branded demand and business model differ sharply.

## 7. Open evidence questions / explicit limits

- The custom `ai-search-xray` skill's full code and prompt were **not** supplied; only screenshots and creator narration were provided. Cannot verify tool counts, internal filtering, retry behavior, network calls, request spend or report generation.
- Cannot reproduce **22.7M / 63M / 27 / 177,922 / 574,992 / ~9K**, or prove data freshness, correctness or complete query coverage without vendor exports and repeatable queries.
- Report screenshots suggest claims about Canva's crawler directives, Notion's site content and Zillow's WAF, but their **current live HTTP responses** were not examined here. This note preserves **creator-reported observations**, not facts about each company's website.
- The user's personal website **was not audited**, and no Google Search Console account or external SEO account was accessed.
- No external tools or skills were connected, no payment, no global Claude config modifications, no user/HE/PMB/ACR code modifications, no PR merge.

**Final decision:** **PRESERVE / DO NOT IMPLEMENT.** Keep an eye on the source-quality, denominator and “claimed growth vs observed user outcome” distinctions. The credible improvement is better evidence practice, not an “AI SEO OS.”
