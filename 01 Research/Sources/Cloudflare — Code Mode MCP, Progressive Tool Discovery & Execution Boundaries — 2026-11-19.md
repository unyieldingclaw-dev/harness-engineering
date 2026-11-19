# Cloudflare — Code Mode MCP, Progressive Tool Discovery & Execution Boundaries — 2026-11-19

**Status:** Source research only; no installation or engineering change authorized.  
**Source supplied:** 102-second vertical screen recording, `ScreenRecording_11-19-2026 08-37-36_1.mp4`, showing a short explainer with the provocative claim that MCP is dying, followed by Cloudflare Code Mode's search/execute design and a closing claim that MCP is not actually dead. The screen recording is a secondary, highly compressed explanation, not a technical specification or a benchmark.  
**Cross-reference:** HE draft PR #39 (Agent Distraction & Context Integrity) and draft PR #28 (Home/Work Multi-Provider Dashboard research). This note deliberately remains separate from both; it records a distinct tool-interface mechanism, not an expansion of those implementations.

## Executive determination

**Mine the pattern, do not install the platform.** Code Mode addresses a real, distinct source of context cost: exposing a large tool/API catalog and intermediate results to the reasoning model. It can postpone operation discovery and execute multi-step calls in a restricted runtime, passing only a selected summary to the model. It is **still MCP**, not a replacement for MCP, and the security decision remains with host-side permissions and individual downstream operations.

For a modest number of existing tools, the appropriate baseline is direct, explicit MCP tools or **provider-native tool search**. A new generic code-execution proxy would have a higher operational/security burden and no established user-specific return. The strongest transferable principle is *bounded capability disclosure plus bounded execution authority*.

**Evidence categories:** (A) Cloudflare first-party blog/docs/source; (B) independent external security research; (C) short-video depiction; (D) HE inference. Marked distinctions below prevent marketing claims from becoming implementation requirements.

## 1. Video claims, with correction and confidence

| Video depiction / claim | Source-supported interpretation | Assessment |
| --- | --- | --- |
| “MCP is dying / what replaces it” | Cloudflare implements **an MCP server** with a different model-facing tool surface; it does not abandon MCP transport/protocol. Video itself ends with “MCP isn't dead.” | Rhetorical hook; misleading if literal. |
| A model with many direct tools burns context on definitions and intermediate results | Both Cloudflare and Anthropic explicitly discuss tool-schema and intermediate-result context overhead. The size depends on client and tool-loading strategy. | Supported mechanism, not measured local pain. |
| Model writes code, uses one sandbox, can fetch multiple records and return a focused result | Cloudflare Code Mode does exactly this; host-defined connector/request function remains the route to actual APIs. | Supported. |
| Cloudflare exposes 2,500+ endpoints through search() and execute() | The **core Code Mode pair** is search/execute. The live repository's tool list additionally names docs and whoami. A benchmark table elsewhere calls the interface 3 tools. | Core architectural claim correct; literal tool-count varies by version/document. |
| “Millions of lines of real code” vs few tool-call training examples | The video provides no controlled evidence that code-generated calls outperform direct calls for the user's tasks; see the related first-party Anthropic work instead. | Plausible general rationale, **not** a validated local performance advantage. |
| Large schema reduction means cheap/faster/more reliable workflows | Cloudflare's figures measure tool-definition token footprint, not complete task tokens, latency, error recovery, dollar cost, or security outcomes. | Unproven extrapolation. |

**Recording limits:** The supplied screen capture is about 102 seconds, primarily animated slides and subtitles. It demonstrates a narrative, not a reproducible environment, code audit, or evaluation result. No source ZIP, configuration, or installed artifact was supplied or executed.

## 2. Separate the three implementation patterns

There are **not just two choices** (“native MCP bad; Code Mode good”).

| Pattern | Model-visible capabilities | Execution/control point | Appropriate fit |
| --- | --- | --- | --- |
| **Direct small MCP/tool set** | Individual named tools with explicit schemas | Native tool calls, existing client permissions; host executes each tool | Small stable catalogs; transparent approvals/auditing; current HE/ACR scope |
| **Provider-native tool search/deferred schemas** | Search plus selected tool schemas only when needed | Normal provider/native tool calls after discovery; conventional permission boundary | Medium/large catalogs when the provider already supports search; likely first benchmark against Code Mode |
| **Server-side Code Mode search/execute** | Compact generic tools; model writes JS to search server-held OpenAPI and call APIs | Generated code in an isolated Worker, host-supplied authenticated request callback | Thousands of endpoints or large intermediate data; requires method/account-specific authorization |
| **Durable Code Mode runtime with MCP/OpenAPI connectors** | One “codemode” tool, discover/describe APIs, approved connector methods | Separate executor, connectors and durable runtime supporting pause/replay/history | Workflows needing verified durable approvals/history; significantly heavier system |

Cloudflare's **generic** single-code-tool adapter is another variant: it embeds all upstream TypeScript definitions into a single tool and explicitly does *not* scale automatically to massive catalogs. Its published “search and execute” OpenAPI pattern is the relevant large-catalog case.

Cloudflare labels the generic SDK **experimental** and documents an MCP SDK v1 / legacy handler compatibility constraint as of the research date. Do not make version assumptions from the video.

**Independent alternative:** Anthropic announced Tool Search Tool and Programmatic Tool Calling in November 2025. Tool Search loads selected tool definitions on demand; programmatic calling handles chained operations and intermediate result filtering. Evaluate native features *before* building an external broker. Model/provider capability and corporate approvals differ; do not assume portability across Claude, Cursor, Codex and Gemini.

## 3. Quantitative claim audit: what the 99.9% does and does not mean

Cloudflare's repository README reports the following comparison for its API:

| Presentation of the Cloudflare API | Definition/context tokens reported |
| --- | ---: |
| Raw OpenAPI document in prompt | ~2,000,000 |
| Direct native MCP, full schemas, 2,594 operations | 1,170,523 |
| Direct native MCP, minimized required fields | 244,047 |
| Code Mode tool description | ~1,100 |

The February 20, 2026 blog rounds the Code Mode figure to about 1,000 tokens and describes **99.9% less tool-definition context than full native schemas**. The benchmark uses tiktoken. These are **source-reported numbers**, not independently reproduced in HE.

The relevant equation for a user task is NOT “catalog tokens saved = task tokens saved”:

**Total task footprint = advertised tool definitions + discovery prompt/result + model-written code + returned data + model retries + error/approval exchanges + any extra planning.**

Additional costs include Worker initialization, API/network round trips, catalog sync and schema validation, independent execution/audit, developer maintenance, and debugging code that may invoke several operations within one outward tool call.

### Why a smaller catalog changes the answer

A hand-picked five- or ten-tool interface may have little schema overhead and already offer clear permissions and low latency. Cloudflare's asymptotic advantage matters when the *unselected* catalog is truly enormous, not merely because the project uses MCP. A provider-native search feature can remove much of the upfront schema overhead without adding a second sandbox and execution layer.

**No local benchmark exists.** Do not quote the 99.9% figure as Home savings, improved ACR quality, decreased Claude subscription limit consumption, or a Work productivity increase.

## 4. Actual Cloudflare repo and SDK behavior (November 19 snapshot)

Primary repo: https://github.com/cloudflare/mcp

- README documents `search`, `execute`, `docs`, and `whoami`; it also documents a `?codemode=false` mode that exposes direct native operation tools. The difference between “2 core tools,” “3 in a comparison row,” and “4 named tools in the current README” is a documentation/version caveat, **not** evidence that the underlying technique is fabricated.
- `search` accepts model-written JavaScript to inspect a server-held, pre-resolved OpenAPI specification and return relevant operations/schema fragments. `execute` accepts model-written JavaScript using an authenticated host callback. The full spec and intermediate API objects need not be returned to the reasoning context.
- The README states that search/execute results are capped at **approximately 6,000 tokens** by default and can be uncapped with `truncate: false` for workflows that do not echo the result into the model. Capping changes observable data completeness; uncapping can restore the very context/bandwidth problem this approach addresses.
- Authentication is via OAuth grant/selected scopes or an API token. Missing scope handling includes 403 insufficient-scope challenges; some endpoints can otherwise return an empty list when scopes are missing. Empty data must not automatically be interpreted as zero resources.
- The repository states that the OpenAPI spec is pinned to a reviewed Forge release. Its AGENTS.md says updates are proposed by PR rather than automatically changing live runtime schema on a six-hour schedule; the separate skills catalog syncs every six hours. Conflating these would distort provenance and compatibility analysis.
- The generic code runtime blocks direct outbound fetch/connect by default. However, host-provided request/connector methods **are** egress by design. No credentials need enter the sandbox for a proper integration.
- The generic OpenAPI MCP server pattern itself does not provide durable per-operation approval. Cloudflare instructs implementers to enforce access and approvals in the host request callback/upstream handler before effects.
- A distinct durable Code Mode runtime supports execution records, connector approvals, replay/rollback and stored snippets. It uses Durable Objects/SQLite and introduces runtime state; it is not a free feature of merely exposing search/execute. Never recommend it without a concrete persistence/replay need.

This is a **documentation/source-level inspection**, not a completed security audit, installed trial, or provider integration test.

## 5. Security and operational risk register

| Boundary / risk | Why it matters | Smallest credible control |
| --- | --- | --- |
| **Permission laundering through a generic executor** | A client may approve “execute JS” once while the program performs several API mutations; an outer tool grant is not necessarily an inner-operation grant. | Enforce method/resource/account-level permissions and human confirmation **at the host operation boundary**. |
| **Model-authored code under adversarial content** | Retrieved docs or API fields can contain prompt-injection instructions that steer code generation or subsequent calls. | Treat source data as untrusted; explicit allowed resources/operations, sandbox and logged calls; no natural-language-only promise of read-only behavior. |
| **Unbounded or unintended fanout** | A short JS loop can perform many API calls or mutate many resources in one execution. | Request count/rate/time budgets; idempotency for mutations; stop conditions; per-call trace visibility. |
| **Stale discovery and schema mismatch** | Indexed/synced specs may drift relative to live API, permissions and returned types. | Pin and identify spec version; typed validation; explicit errors; recheck API state before consequential changes. |
| **Truncated or summarized intermediate results** | ~6K output caps and model-chosen filtering can omit exceptions/failures or hide a bad denominator. | Preserve structured errors/counts, truncation flags, pagination and audit traces; never represent partial results as complete. |
| **Opaque compound work** | “One model tool call” can hide a sequence of operations; failures/retries are harder to attribute. | Log individual endpoint/method/account, status and correlation ID, with secrets redacted; expose exact operation list before approval. |
| **Isolation and sandbox vulnerability** | A sandbox is an additional attack surface, not a proof of impossibility of escape. | Follow patched runtime releases; restrict sensitive capabilities even when sandboxed; avoid self-hosting untrusted code without operational necessity. |
| **Provider-specific behavior** | Claude/ChatGPT/Codex/Cursor/Gemini have different tool-search support, approvals, quota, and enterprise authorization. | Validate each integration independently; no universal policy inferred from marketing demos. |

### Independent security evidence worth preserving

Check Point Research, **“When Agentic Glue Melts: Exploiting Cloudflare Code Mode and Workers”** (Aug 7, 2026), reports **five workerd vulnerabilities**, including two rated Critical, and demonstrates cross-tenant heap disclosure and Code Mode sandbox escape mechanisms in its research environment. **Their report explicitly states that Cloudflare patched the managed production service**, and identifies **workerd v1.20260619.1** as the minimum corrected version for the discussed self-hosted case. This is historical vulnerability evidence, **not** a claim that current managed Code Mode remains vulnerable or that HE is exposed.

The durable lesson: sandbox assumptions themselves require evaluation and patch governance. A generic executor expands the TCB (trusted computing base), even if it shrinks the prompt.

Source: https://research.checkpoint.com/2026/when-agentic-glue-melts/

## 6. Relationship to HE Agent Distraction & Context Integrity (#39)

**Adjacent, not equivalent mechanisms.**

- Code Mode reduces **schema/context loading** and can suppress intermediate tool-output noise. That may make more context budget available for relevant instructions.
- It does **not** determine whether encountered instructions are fresh, authoritative, on-task, or approved. An out-of-scope TODO remains out of scope even when discovered through two tools rather than hundreds.
- It does **not** prevent in-session stale state. A single composed execution can be convenient, but endpoint data still changes between observation and mutation. Revalidate preconditions at the point of action.
- It can **amplify** scope errors: a mistaken model-written loop can affect numerous resources. Host authorization must be narrower than the generic tool identity.
- Useful common principle: **progressive disclosure plus explicit source/authority boundaries**. A discovery response conveys information about *what is possible*, not permission to *do it*.

## 7. Bounded transfer assessment

| Context | Observed need | Candidate lesson | Disposition |
| --- | --- | --- | --- |
| **HE research** | Real source and first-party implementation exist; no HE native-tool bloat benchmark. | Preserve as a tool-interface pattern and counterexample to “MCP is dead.” | **Document now; no runtime.** |
| **PMB** | Existing source-ownership and governance-load concerns; no evidence a huge tool catalog is a bottleneck. | Avoid stuffing all information/capabilities into every session; retain existing source-owned minimalism. | **No change.** |
| **ACR** | Local Ollama reviewer pipeline, not a large MCP operation catalog; 32K and CPU/offload issues are different constraints. | Code Mode does not address the observed ACR timeout/offload diagnosis. | **Not relevant to current defects.** |
| **Home multi-provider cockpit (PR #28)** | Integration boundaries remain research-only; provider telemetry APIs not demonstrated at 2,500-operation scale. | Narrow read-only connectors and normalized provider-specific observability. | **Prefer explicit integrations; reconsider only on catalog evidence.** |
| **Work Release Engineering** | GitLab/Jira/CI may present numerous operations but actual schema load, rights, audit and approval patterns are not measured. | Native tool search and restricted read-only APIs could be useful; generic execution creates additional enterprise trust boundary. | **Conceptual assessment only; no access/config change.** |

**Do not connect Home tokens to Work tools or expose company repositories to public-hosted runtimes without explicit corporate approval.** A “trusted sandbox” is not equivalent to an approved data-processing boundary.

## 8. Small, falsifiable evaluation — only if a real need emerges

Do not implement this test merely because the technology exists. Start by inspecting **existing** per-session tool catalog and error/latency traces. If they reveal significant schema overhead or repeated needless tool-result transfer, evaluate the smallest representative task set:

1. **Baseline:** explicit native tools with fixed read-only scopes.
2. **Comparator:** native deferred tool search / selective tool loading where supported.
3. **Candidate:** code execution with progressive search, bounded operations, identical read-only scopes.
4. **Task fixtures:** one lookup, two dependent lookups, pagination/aggregation, not-found response, insufficient permission, stale-state conflict, oversized output and invalid schema.
5. **Measure:** full prompt/input and output tokens; total tool round trips; first-result and end-to-end time; correct operation chosen; answer/result completeness; error classification; replay transparency; unauthorized-call attempts; human approval burden and implementation maintenance.
6. **Control:** same model, task definitions, data snapshot, authorization and result requirements; repeat runs rather than single demos. Include no-op or denied-call control to test security.
7. **Decision:** only adopt if measurable *end-to-end* gain exceeds new runtime, sandbox, audit, approval and provider compatibility costs **without accuracy or permission regression**.

**Stop condition:** if there are fewer than about ten frequently used small native tools, a finite/compact schema set, or no demonstrated context/tool-selection pressure, do not add Code Mode. This is an engineering heuristic, not a proven threshold.

## 9. Follow-up research questions (not backlog commitments)

1. Which of Home's actual agent setups load how many tool-definition tokens today? Is a provider-native tool-search feature already handling discovery?
2. Where would one compound call obscure operations that currently require individual approval, review or rollback?
3. Can the specific provider support bound result-size and error completeness guarantees without extra infrastructure?
4. Under what *measured* Work catalog size and enterprise approval model would a code-execution proxy be acceptable, compared with narrow approved integrations?

These questions authorize further source research, **not** code/connector work.

## 10. Primary sources and provenance

1. Cloudflare / Matt Carey, **Code Mode: give agents an entire API in 1,000 tokens** (Feb 20, 2026). https://blog.cloudflare.com/code-mode-mcp/ — architectural claim, comparison, example.
2. **cloudflare/mcp README** (inspected Nov 19, 2026). https://github.com/cloudflare/mcp/blob/main/README.md — current tool list, ~6K output cap, scope handling, direct tools option, reported 2,594-operation token table.
3. **cloudflare/mcp AGENTS.md** (inspected Nov 19, 2026). https://github.com/cloudflare/mcp/blob/main/AGENTS.md — spec pin, token handling, generated tools and staging/compatibility details. Treat as repository-maintainer description, not a full audit.
4. Cloudflare, **Build a Code Mode MCP server** (docs updated Oct 27, 2026). https://developers.cloudflare.com/agents/model-context-protocol/guides/build-codemode-mcp-server/ — simple code versus search/execute, approval and sandbox limitations, experimental status.
5. Cloudflare Agents SDK, **How Code Mode works**. https://github.com/cloudflare/cloudflare-docs/blob/production/src/content/docs/agents/tools/codemode/how-it-works.mdx — durable architecture and generated execution lifecycle.
6. Cloudflare Agents SDK, **Use MCP tools with Code Mode**. https://github.com/cloudflare/cloudflare-docs/blob/production/src/content/docs/agents/tools/codemode/mcp.mdx — MCP connector and optional per-operation approvals.
7. Anthropic, **Code execution with MCP: Building more efficient agents** (Nov 4, 2025). https://www.anthropic.com/research/code-execution-with-mcp — independent related mechanism.
8. Anthropic, **Introducing advanced tool use** (Nov 24, 2025). https://www.anthropic.com/research/advanced-tool-use — native tool search, programmatic tool calling, caveats for small catalogs.
9. **Check Point Research**, *When Agentic Glue Melts* (Aug 7, 2026). https://research.checkpoint.com/2026/when-agentic-glue-melts/ — coordinated disclosure and production-patched sandbox vulnerabilities.
10. User-supplied ~102-second screen recording; frames examined at regular intervals and compared against first-party docs. No direct provenance or factual claim from the video beyond its visible narrative is assumed.

## Decision record

**ADOPT (research principle):** progressive discovery, selective result return, explicit host-side operation authority, and measured end-to-end context economics.

**DEFER (implementation):** a generic Code Mode gateway, Cloudflare-hosted connector, durable runtime, shared Home/Work proxy, and provider-wide routing abstraction. No demonstrated local bottleneck or corporate authorization currently supports those.

**REJECT (claim):** “MCP is dead,” “99.9% reduction in total agent spend,” “the sandbox makes a generic execute tool safe,” or “one outward tool call implies one authorized downstream action.”

**Change boundary:** This document is the sole requested mutation in this research branch. No production code, configuration, credentials, existing workflow, PMB/ACR repo, company systems or HE runtime was touched.
