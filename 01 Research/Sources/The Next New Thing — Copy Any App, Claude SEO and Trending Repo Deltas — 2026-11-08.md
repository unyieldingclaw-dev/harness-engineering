# The Next New Thing — Copy Any App, Claude SEO and Trending Repo Deltas — 2026-11-08

**Status:** Source triage, not installation or an HE architecture proposal.  
**Video:** [Copy any app, Claude does SEO, and the top 10 repos](https://www.youtube.com/watch?v=-6SHbxdXMRU), The Next New Thing / Andrew Warner and Adam Brakhane, Oct 30, 2026. User supplied link/title; secondary [episode recap and timestamped transcript](https://moderncreator.app/2026-10-30-the-next-new-thing-copy-any-app-claude-does-seo-and-the-top-10-repos) cross-referenced with direct GitHub official README/code reading where indicated.
**Scope:** 3 bonus repos, 10 ranked GitHub repos; sponsor/affiliate pitches excluded as adoption evidence. Static repo reading; no runtime evaluation, deployment, billing, security pentest or SEO measurement was performed. Trending popularity and claims of outcomes do not establish quality or applicability.
**Related previous HE review:** Nov 7 [15-repo investigation](https://github.com/unyieldingclaw-dev/harness-engineering/pull/27), Oct 28 [Cloudflare security audit](https://github.com/unyieldingclaw-dev/harness-engineering/pull/25), Oct 22 [ECC review](https://github.com/unyieldingclaw-dev/harness-engineering/pull/20), Sept 25–29 Karpathy, skills, risk-directed reviewer and retrieval research.

## Executive disposition

**Most items overlap prior HE research and are not additional work.**

- **Potential narrow useful follow-ups:** source code for **EveryApp** if someone needs a managed small-app deployment gateway; **Claude SEO** for an authorized one-off audit of an actual site, not a global skill; **Anthropic knowledge-work plugins** as an example of role-scoped, file-based tool/skill packaging, not a company-wide integration plan.
- **Potential risk illustration:** `t3dotgg/tstack` offers highly autonomous `full-send` and consensus loops; human acceptance and irreversible merge authority must remain explicit.
- **Strongest general rule:** use the smallest current verified mechanism, read the installed skill, and do not promote marketing claims or subjective style preferences into universal safety/process requirements.

## Named projects: all 13

| Group | Repo and inspected ref | What it offers | HE / other disposition |
| --- | --- | --- | --- |
| Bonus | [every-app/every-app](https://github.com/every-app/every-app), `5f19f30b20dd138c46f15fdf492834f8145e8025` | Early-development Cloudflare gateway for running **supported/open-source, app-specific deployments** with common auth/storage/API handling. README says requires Workers Paid plan and a domain on Cloudflare. | **MONITOR** for a concrete web-app deployment need. A $5 base plan is **not** proof unlimited $5 all-in hosting, reliability or the right to copy commercial apps/assets. |
| Bonus | [t3dotgg/tstack](https://github.com/t3dotgg/tstack), `1085050f3baed1ac724caa4a8166145fb2ac88af` | Nine personal skill examples (council, PR prep, unattended `full-send`, `full-stop`, etc.). | **REFERENCE ONLY**. `full-stop` is more compatible with bounded human merge authority than `full-send`. Review prerequisite skills and permissions before even a disposable pilot. |
| Bonus | [joshuaswarren/omarchy-apple-dev](https://github.com/joshuaswarren/omarchy-apple-dev), `e1d7241b8b4e80dc604d3f42e65a5dc1a910c944` | Experimental path to iPhone builds/installs from Linux/Omarchy. README has dated receipts and explicit limits: several later tests **did not** complete full new-version login/build/device loop. | **NO HE ACTION**. For actual mobile app work use a tested toolchain/host; interesting portability research only. |
| #1 | [cloudflare/security-audit-skill](https://github.com/cloudflare/security-audit-skill), `c1c8a8c1471069fb0e188eeaff69b8e8db6564a8` | Coverage-led isolated security hunts, fresh refutation verifier, `confirmed/needs_validation/rejected` evidence and deterministic ledger/report validators. | **ALREADY COVERED** in HE PR #25. Conditional ACR review pilot only when exact capability gap shown; not a security certification. |
| #2 | [AgriciDaniel/claude-seo](https://github.com/AgriciDaniel/claude-seo), `6be93f45064ffbadced572b5d5c95f55fd263af0` | Opinionated SEO audit: crawlability, redirects, schema, Core Web Vitals, content, AI-search, local SEO. README advertises **26 sub-skills and 19 specialists**, many agent fan-outs. | **OPTIONAL, OUTSIDE HE** for a real site you own, preferably first-party Search Console, Lighthouse/PageSpeed and manual checks. Use only useful passes; avoid installing dozens of agents for a single site audit. Crawling cannot prove competitive SEO ranking/ROI. |
| #3 | [pbakaus/impeccable](https://github.com/pbakaus/impeccable), previously pinned `687bc381bc9091bf252c0a05303640561014a425` | Design-review/edit skills with site/element context. | **ALREADY COVERED** in HE Nov 7 research, optional UI-project quality reference. |
| #4 | [obra/superpowers](https://github.com/obra/superpowers), `22f0b463e7a5e579904dd6211a520321035526f7` | Mandatory brainstorm, planning, TDD and subagent review skills. | **ALREADY COVERED**, selective verification concepts only. |
| #5 | [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents), `9fa1196fffbcfd9de5146d2dd2869df84ab99838` | Financial simulated multi-agent analysis with risk debates and research memory. | **ALREADY COVERED** in Nov 8 [Beau Johnson follow-up](https://github.com/unyieldingclaw-dev/harness-engineering/pull/27); key lesson is historical data/score integrity, not agent count. |
| #6 | [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills), `2c606141936f1eeef17fa3043a72095b4765b9c2` | Inspired-by-Karpathy `CLAUDE.md` with think first, simplify, surgical edits and verified goals. Maintainer is **not** Andrej Karpathy. Repo install commands point to a different `forrestchang` namespace; verify current canonical/install source before using. | **ALREADY COVERED** in HE Karpathy/eval/skill research. Do not auto-download user instructions into every repo. |
| #7 | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill), `f6b76575f949707b4e3c37cd86cb44b0b98643e8` | Opinionated design-system and “taste” instructions, browser validation. | **UI-ONLY reference**; aesthetic preferences are not deterministic correctness conditions. |
| #8 | [jakubkrehel/skills](https://github.com/jakubkrehel/skills), `157da62016338e8693dcff93d47154a9e36f2157` | Targeted UI/typography/layout/WCAG 2.2 checks. | **ALREADY COVERED** Nov 7; actual keyboard/focus/contrast tests before claiming compliance. |
| #9 | [pingdotgg/t3code](https://github.com/pingdotgg/t3code), `3da48c1234045f89bac7db885bbee73464a60624` | Multi-agent/provider mobile/web/desktop **control surface**. | **ALREADY COVERED** Nov 7–8; initial new-thread **Full access** and provider-specific permission semantics are known concerns. |
| #10 | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins), `e050c2ec856373787143d7057663a6ebd1277df3` | Role-specific file-based plugin bundles containing a manifest, skills, commands and connectors; product-management and other domain packs. | **REFERENCE ONLY**. Useful example of per-function packaging; product abilities are coupled to authenticated connectors/host. Skills do not grant authorization. No enterprise rollout indicated. |

For projects not re-pinned in this note, older inspected refs are historical observations and must be refreshed before execution; **all repos are third-party code**.

## Practical findings that might matter

### EveryApp: “copy any app” is a misleading shorthand

The official README focuses on a common **Cloudflare-hosted gateway** and examples (OpenSEO, OpenCRM, OpenMail, OpenAnalytics). It says apps have no public endpoint and use delegated gateway access. The architecture could address deployment/identity duplication, but centralizes auth/data/API-key trust and Cloudflare-specific operational dependencies. Claims of “secure by default,” “infinite scaling” and “$5 for hundreds of apps” require deployment, capacity, billing, identity and access tests rather than trust in a README.

**Do not adopt a generic web-app platform just because HE/PMB has multiple repos.** A single existing website can remain on its current host; a mobile app has different deployment constraints.

### SEO is an application-task choice, not HE

A bounded audit of a real, authorized site may be worthwhile:
- first get the correct canonical URLs, sitemap, robots rules, metadata/schema, crawl status, accessible content and Core Web Vitals;
- verify claims against live rendered pages and Google Search Console;
- use paid keyword/competitor data only if a decision requires it;
- don't treat an AI recommendation, “GEO score” or SEO ranking promise as a measured search conversion.

The *all-SEO-agents* bundle is unnecessary for a small personal site unless it demonstrates lower total cost/higher issue quality on that site.

### Personal skill packs and authority boundaries

`tstack` includes a fully autonomous merge/PR guardian. This is precisely the kind of **optional capability whose authority must be bounded by the user**. Consensus among three models is not independent acceptance if they share the same wrong inputs; “keep looping until agreement” has unbounded token/latency risk. Prefer explicit stop/approval conditions over automatic merging.

### Tool-skill inflation and version drift

Coding-agent instructions can be made locally inspectable, narrow and maintainable; copies will drift with tool APIs and model changes. The four simple Karpathy-inspired principles are useful partly because they do **not** require a complicated agent hierarchy. But repository descriptions and “models comply 2× better” narratives are not an independent demonstration for HE.

### Anthropic role plugins are a pattern, not proof of portability

Official `anthropics/knowledge-work-plugins` includes `product-management` and `enterprise-search` packages with skills and integration declarations. It is an example of task/role packaging, but **tool and permission semantics are host-specific**. Do not infer an AI agent can read Jira, email, HR docs or customer data because a skill lists a connector.

## Overall decision

**Record research only.** No new HE feature, global rule, security framework, installed SEO skill, Cloudflare deployment platform, multi-agent merge workflow, ACR agent, or PMB memory system from the episode. If a real website SEO issue or mobile deployment problem emerges, assess the smallest appropriate tool in that owning project. Existing HE prior art already captures the common verification/skill lessons.
