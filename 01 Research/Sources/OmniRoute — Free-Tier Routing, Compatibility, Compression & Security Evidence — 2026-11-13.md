# OmniRoute — Free-Tier Routing, Compatibility, Compression & Security Evidence — 2026-11-13

**Disposition:** **MINE SELECTIVELY / DO NOT ADOPT THE GATEWAY.** This is a public-source HE research record, not a deployment recommendation or an implementation authorization. No credentials, provider logins, proxy, local models, CLI settings, interception tools, or project code were touched.

**Origin:** User's November 13, 2026 screen recording `ScreenRecording_11-13-2026 20-55-06_1.mp4`, which advertises OmniRoute and an aggregate ~1.6 billion free tokens/month. The video is discovery material, not proof of quotas, fidelity or safe operations.

**Pinned upstream snapshot:** [cheaper-inference/OmniRoute at `49a288e557f25e70a2a12b4c32bf0696e28b3a15`](https://github.com/cheaper-inference/OmniRoute/tree/49a288e557f25e70a2a12b4c32bf0696e28b3a15), branch `release/v3.8.54`, inspected **2026-11-13**. Release's `package.json` says `3.8.54` and MIT. The public source was read but **not installed, executed, benchmarked or independently security-audited**. Upstream documentation sections have different `lastUpdated` dates, so code, tests and operator documentation must be distinguished. Most links below are pinned to this commit to avoid moving-target claims.

## Executive judgment

The system is a **traffic-routing gateway**, not a passive engineering dashboard. It advertises **393 registered provider definitions**, an OpenAI-compatible surface, account and model fallback, cost/quota accounting, native/emulated tool compatibility, OAuth/API-key/web-cookie authentication, prompt transformations, local persistence, MCP/A2A, and other services. A large catalog is not evidence that every connection works, implements tools, is permitted by contract, or has usable free quota.

The useful transferable ideas are: **(1) connection-level billing identity; (2) explicit unknown/zero/stale quota states; (3) fail-closed spend boundaries; (4) route-compatibility gates; (5) bounded tool-output compression with recoverable evidence; (6) provenance for quota claims.** Most of the platform is not useful to HE, PMB, ACR or the separate telemetry Dashboard.

## Source/evidence ledger

| Topic | Source at pinned commit | What it supports, not what it proves |
| --- | --- | --- |
| Scope and implementation claims | [README](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/README.md); [architecture](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/architecture/ARCHITECTURE.md); [package version](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/package.json) | Catalog/gateway claims, routable provider registry, persistent settings and large API surface; not field-tested reliability |
| Free-tier estimation | [free-tier methodology](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/reference/FREE_TIERS.md); [free-tier guide](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/getting-started/FREE-TIERS-GUIDE.md); [catalog](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/open-sse/config/freeModelCatalog.ts) | ~1.62B **estimated** recurring pool tokens after deduplication, separate credits/gates, provider eligibility caveats |
| Billing vs model | [subscription ladder](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/routing/SUBSCRIPTION_LADDER.md); [strict free policy](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/routing/STRICT_ZERO_COST.md) | Billing depends on actual provider connection, overage semantics and checked allowance—not a model's nominal price |
| Provider functions | [generated provider reference](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/reference/PROVIDER_REFERENCE.md) | `native` vs `emulated` vs `none` tool calling; `none` may silently drop tools |
| ToS guard and documentation mismatch | [settings default](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/src/lib/db/settings.ts); [candidate filter](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/open-sse/services/autoCombo/strictZeroCostFilter.ts); [regression test](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/tests/unit/issue-15059-tos-avoid-auto-default.test.ts); [older free-tier prose](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/reference/FREE_TIERS.md) | Current code-backed new-install default is `excludeTosAvoid: true`; older docs still incorrectly describe advisory-only/false default |
| Compression | [RTK guide](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/compression/RTK_COMPRESSION.md); [compression guide](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/compression/COMPRESSION_GUIDE.md) | Named lossy modes, output classes, failure exceptions, optional raw-output retention and tests; not a guarantee of downstream answer correctness |
| Higher-risk capture | [TPROXY transparent decryption](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/security/MITM-TPROXY-DECRYPT.md); [provider reference](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/reference/PROVIDER_REFERENCE.md) | Optional privileged HTTPS interception/CA operations and web-cookie integrations have much higher authority than local routing |

## Findings and decisions

### 1. The 1.62B headline is not a usable budget

The upstream catalog claims **~1.62 billion documented recurring tokens per month** over shared provider pools after deduplication, and **~2.22B in the first month** including eligible signup credits. It separately lists no-cap/rate-limited products, regional verification and one-time boosts. It explicitly says the number is **not granted by OmniRoute** and that an install without eligible credentials may have zero routes. Its headline depends heavily on one estimated Mistral 1B/month allowance and other pooled providers. Source tables include dated corrections (e.g., removed free tiers and revised Gemini/Ollama Cloud classifications). CI consistency of *catalog arithmetic* is not independent validation of provider eligibility or continued access.

**Mine:** attach acquisition date, provider-source link, credential/plan eligibility, account-level pool ID, billing unit, quota reset, observed usage and `known/unknown/stale` state. Aggregate only actual eligible pools; **unavailable ≠ zero**. **Do not mine:** speculative “free inference capacity” numbers into HE or Dashboard product goals.

### 2. Billing is a **connection** fact, not just a model fact

`SUBSCRIPTION_LADDER.md` distinguishes `subscription | metered | keyless | unknown` connection billing and `hard-stop | meters-to-paid | unknown` overage. An OAuth subscription connection to a model can be plan-included while an API-key connection to the same model charges per token. Nor does `authType` alone prove free/paid status. Unknown connections are *not* treated as free.

`auto/subscription` is deliberately **fail closed**: curated subscription, hard-stop overage, live quota headroom; stale or unverified entitlements are excluded; the empty pool returns an error rather than silently billing a different route. `auto/thrifty` is a separate explicit strategy permitted to escalate through keyless/free/cheap/premium rungs. This is a critical distinction for future **user-authorized** cost experiments, but not a mandate to put a gateway in front of Claude Code or Codex.

**Mine:** source-owned billing and quota semantics, explicit `no billable fallback` acceptance criteria, and per-connection rather than per-model restrictions. If no fallback is explicitly authorized, **stop rather than substitute**.

### 3. Fallback must preserve **capability and source identity**

The [provider reference](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/reference/PROVIDER_REFERENCE.md) explicitly documents tool-calling states:
- `native`: provider-native function calling.
- `emulated`: prompt-embedded tool markers parsed by a regex-based adapter.
- `none`: requested `tools` can be **silently discarded** (reference cites upstream issue #7286).

Thus “one compatible endpoint” is not equivalent task semantics. A fallback can change tool-call fidelity, structured-output behavior, context size, model reasoning, privacy, or costs without a visible error. For a coding task, losing tool calls or changing tool result shape is an execution failure even if the endpoint returns HTTP 200.

**Mine:** pinned provider/model/connection identity in any measured run; explicit feature-contract checks; record original and actual provider after fallback, reason, completeness, permissions and cost/usage. **Reject:** silent substitution of models during ACR or release-critical engineering and any “provider equivalence” claim based only on API shape.

### 4. ToS filtering is real in code, but documentation has drift

- [`src/lib/db/settings.ts`](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/src/lib/db/settings.ts) sets **`excludeTosAvoid: true` for new installs** at the inspected commit.
- [`issue-15059-tos-avoid-auto-default.test.ts`](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/tests/unit/issue-15059-tos-avoid-auto-default.test.ts) asserts that default and candidate exclusion.
- [`virtualFactory.ts`](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/open-sse/services/autoCombo/virtualFactory.ts) applies the guard to candidate pools.
- Older [`docs/reference/FREE_TIERS.md`](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/reference/FREE_TIERS.md) §ToS attention and [`STRICT_ZERO_COST.md`](https://github.com/cheaper-inference/OmniRoute/blob/49a288e557f25e70a2a12b4c32bf0696e28b3a15/docs/routing/STRICT_ZERO_COST.md) still contain statements that the flag is advisory or defaults false. Those parts are **stale relative to inspected implementation and test**. An explicit opt-out remains available; the classification itself is curated and does not replace checking actual provider terms.
- `STRICT_ZERO_COST` is a **different opt-in** (`freeAccessPolicy: "off"` by default); do not conflate it with the default-on ToS guard.

**Mine:** source-vs-runtime-settings validation and “policy as a testable predicate”; always resolve documentation conflicts by looking at current executable behavior and effective settings. **Do not infer:** ToS compliance from a vendor-maintained label.

### 5. Compression is a **transformation** of evidence, not a free optimization

The upstream advertises RTK command-aware filtering, Caveman prose shortening, message aging, truncation, stacked pipelines and other modes; figures such as **15–95%** are upstream claims for eligible inputs, not an independently reproduced improvement in coding-task success. RTK has command detectors, inline test/validation gates, `unless` exceptions for error cases, and configurable raw-output retention (`never` among supported modes). Some compression is explicitly lossy; removing test warnings, shell errors, fallback diagnostics, uncommon stack frames or compatibility paths can produce a shorter but **incorrectly reassuring** agent context.

**Mine:** if a measured problem later justifies compaction, compare raw vs transformed output on fixture cases including **failed assertions, intermittent errors, CI conditionals, platform-specific code, hidden fallback branches and nonzero exits**. Require exact preservation of critical failure facts, explicit transformation attribution and recoverable raw evidence (subject to secret redaction/retention rules). **Do not implement:** generic automatic prompt compression in PMB, ACR, or the Dashboard.

### 6. Gateway security surface is substantially larger than a dashboard

The repository documents provider-key/OAuth/web-cookie connections, local persistence, request logs, compression and numerous administration endpoints. Some provider wrappers explicitly request browser session cookies or token files, which may expose high-value credentials to the gateway. Its optional **Linux root-only transparent HTTPS decryption** mode (TPROXY) installs an interception CA and uses `CAP_NET_ADMIN`/kernel routing; this is far beyond normal local model inference. This mode is described as opt-in, **not** observed enabled in the user's environment. Even without it, the gateway forwards prompts to whichever **cloud** provider is selected—“local gateway” does not mean “local inference” or private content retention.

**Mine:** threat-model distinction among (a) read-only telemetry viewer, (b) credentialed prompt router, (c) high-privilege network interceptor. Provider routing needs independent permission, secret-storage, egress, logs, prompt/privacy, installation and revocation reviews. **Reject:** putting OmniRoute between work-approved models and corporate sources without organizational authorization, installing root CAs for testing, or importing desktop/web session cookies to chase free tiers.

## Project-level ownership / no-go

| Owner | Evidence worth retaining | Decision |
| --- | --- | --- |
| **Harness Engineering** | Connection-billing vs model-price, observed/unknown quota, deterministic fail-closed spend, capability-aware substitution, raw evidence fidelity, policy/documentation drift | **Research only**; cross-reference existing HE source-authority, context-efficient tooling and model-routing notes before inventing any new standard |
| **Home AI Lab** | Optional *one-provider, one-client* pinned proxy comparison against direct Ollama for a genuinely recurring workflow | **Defer** until direct native/local path is insufficient; no ambient reroute of Claude/Codex/Cursor |
| **ACR** | Deterministic source/model identity and error-path preservation reinforce existing evaluation controls | **Do not integrate** gateway, automatic fallback or lossy compression into calibration |
| **PMB** | No new memory/retrieval requirement; keep historical source provenance as research | **Do not modify** release, hooks, CI, memory files, or schema |
| **Multi-Provider Dashboard** | May cite provider metadata/availability *as externally supplied data* if authorized; distinguish plan quota, observed usage and measured cost | **Do not make a proxy mandatory**; dashboard remains separate, read-only, Home/Work isolated. Native provider data remains primary |
| **Work environment** | Corporate tool/telemetry authorization and data policies control everything | **Blocked by default absent explicit approval**; never route work prompts through personal infrastructure |

## Minimum future evaluation — not executed or authorized

**Trigger:** repeated real workflow where current provider/native controls fail to answer billing/limit questions or must be manually switched, *not* free-token headline or catalog size.

1. Recheck the selected **specific** provider's current ToS, plan and billing path. Exclude web-cookie/scraped session sources and ToS-avoid candidates; use a test key with a small hard spending cap if any remote provider is needed.
2. Use an isolated local test deployment; inspect version/hash, outbound network behavior, management authentication, listener exposure, log retention, and all proxy/interception options. Do not install a CA, change system-wide environment variables or redirect normal IDE traffic.
3. Test **direct Ollama → fixed model** against **OmniRoute → that identical fixed Ollama model** on a small pinned set. Compression OFF, fallback OFF, no history/memory/agent extensions. Measure output/stream/tool compatibility, tokens, latency, errors and resource overhead; inspect source attribution.
4. Deliberately simulate missing provider, no tool support, stale usage, exhausted quota and prohibited billable fallback. Require observable refusal rather than silently selecting an unapproved route.
5. Only if the deterministic baseline succeeds, independently test **one** opt-in fallback or safe-output filter on held-out coding tasks. Record capability preservation, changed failures and raw evidence.
6. Decision: **keep direct tools / allow small local proxy spike / stop**. No dashboard integration or permanent secret/credential consolidation by default.

**Pass criterion:** observed improvement in the specific repeated task, no unauthorized billable fallback, no tool-call semantics loss, unchanged safety/security boundaries, understandable failure modes and maintenance cheaper than the benefit. If no measured benefit, **archive and stop**.

## Research disposition

**Supported:** repository contains concrete code paths and tests for cost/eligibility separation, fallback and output filtering; source-level discrepancy in docs vs settings is real. **Not supported:** 1.62B tokens available to this user, provider-wide reliability, 393 interoperable tool-capable routes, guaranteed prompt fidelity, zero-cost engineering, improved developer outcomes, or a safe default installation in Home or Work.

**No implementation order.** Preserve a few tested ideas in HE's research backlog; do not install/merge/provision anything merely because the aggregate platform is coherent.
