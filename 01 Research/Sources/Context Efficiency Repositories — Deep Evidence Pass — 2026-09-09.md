# Context-Efficiency Repositories — Deep Evidence Pass

**Date:** 2026-09-09  
**Corpus role:** comparative prior-art synthesis  
**Source bundle:** 11 repositories surfaced by *The Next New Thing* episode “10 Repos conserve your token usage”  
**Evidence boundary:** public repository README/docs/code-search evidence and current repository metadata were reviewed directly where accessible. Claims below are source-derived unless explicitly marked as inference or proposed experiment. No independent local reproduction was performed in this pass.

## Executive finding

The video groups these projects under “token savings,” but they attack materially different layers of the agent context problem. The most useful distinction is not compression percentage. It is **where information is kept, what representation enters model context, who decides what can be omitted, and whether omitted information remains recoverable**.

The corpus reveals at least six distinct mechanisms:

1. **Externalize:** keep raw tool output outside the model context and retrieve it on demand (`context-mode`, parts of `magic-compact`, parts of `token-optimizer-mcp`).
2. **Compress:** transform tool output or prompt text into a smaller representation before model ingestion (`headroom`, `leanctx`, RTK/Caveman in OmniRoute).
3. **Preserve structure while compacting:** retain conversation skeleton and recover omitted tool content (`magic-compact`).
4. **Compute outside the model:** have code perform bulk analysis and return only the result (`context-mode`).
5. **Project the repository:** maintain a deterministic or semi-deterministic structural graph so the agent queries a projection instead of rereading source (`graphify`, `codebase-memory-mcp`).
6. **Reduce work generated:** prevent unnecessary implementation before it exists (`ponytail`).

A seventh layer is orthogonal but relevant:

7. **Route execution:** select providers/models and fail over across them (`freellmapi`, `OmniRoute`).

### Strongest Harness Engineering conclusion

**Context reduction should be treated as an evidence-preservation problem, not merely a token-compression problem.** A mechanism is materially better when it can state:

- what was removed or transformed;
- what authoritative source remains available;
- how the model can recover the original when needed;
- what precision-sensitive content is protected;
- how freshness is established;
- how savings are measured against a counterfactual;
- and whether the mechanism adds enough startup/runtime context to erase its own benefit.

This sharpens the existing PMB/Rta-Smriti direction: **durable evidence and execution artifacts should not be conflated with model context**.

---

# Corpus relationship

This is a **new comparative synthesis entry**, not a claim that the individual projects are all new ideas.

| Existing entry | Relationship | Why |
|---|---|---|
| Rta-Smriti Brain | **MERGES WITH / SHARPENS** | Strong independent implementation evidence for durable evidence outside model context, bounded retrieval, provenance, freshness, and context compilation. |
| OpenMAIC | **MERGES WITH / SHARPENS** | Durable execution state, retrieval from durable state, context/runtime separation, and lifecycle state are directly relevant. |
| Hindsight | **ORTHOGONAL / SHARPENS** | Retrospective memory admission is different from runtime context optimization, but the same “do not silently promote derived information” discipline applies. |
| PMB Filing Contract | **SHARPENS** | Externalized context and repository projections strengthen the canonical-vs-derived-vs-snapshot model and the startup-context cost gate. |
| Matt Pocock | **MERGES WITH** | Context phases, `CONTEXT.md`, TDD seams, and “existing seam first” align with context minimization and reuse-before-invention. |
| Ras Mic / Ralphy | **MERGES WITH / ORTHOGONAL** | Execution profile, isolation, bounded iteration, and provider/model routing are adjacent execution-layer concerns. |
| David Ondrej | **MERGES WITH** | Progressive disclosure, deterministic work in code, decision review, and execution/discipline separation converge strongly. |
| Anthropic AI-Native SDLC | **SHARPENS** | Committed artifacts, subagent context/tool limits, continuous evals, and independent verification provide governance around context-saving mechanisms. |
| Archify | **MERGES WITH / SHARPENS** | Graph projections and architecture deltas raise the same question: deterministic projection vs inferred relationship, with provenance and freshness. |
| Adrian Cockcroft | **ORTHOGONAL / SHARPENS** | Execution identity/provider/runtime routing belongs in the execution layer, not PMB durable memory. |

**No existing entry is superseded by this bundle.**

---

# 1. Context Mode — highest-priority investigation

**Repository:** https://github.com/mksglu/context-mode  
**License:** ELv2  
**Primary mechanism:** externalized tool output + indexed session state + code-executed analysis.

## What the repository actually does

Context Mode describes itself as “the other half of the context problem.” Its central claim is that raw MCP/tool output should not automatically enter the model context. It reports examples such as 315 KB of raw data becoming 5.4 KB of context, and describes four related mechanisms:

- sandbox tools keep raw data outside the context window;
- session events are persisted in SQLite and indexed with FTS5/BM25;
- bulk analysis is executed as code and only the result is returned;
- final response style remains the model/user's concern rather than being enforced by the context layer.

The Claude Code integration is not merely an MCP server. The full plugin registers lifecycle hooks including `PreToolUse`, `PostToolUse`, `UserPromptSubmit`, `PreCompact`, `SessionStart`, and `Stop`, plus sandbox/meta tools. It also provides diagnostics and savings statistics.

## Architectural primitive

```text
agent
  -> tool request
  -> context-mode routing
      -> execute/index raw data outside model context
      -> return bounded result
  -> model

session events
  -> SQLite/FTS5
  -> BM25 retrieval
  -> targeted continuation after compaction
```

The strongest idea is **routing enforcement at the tool boundary**, not merely giving the model another optional tool. The repository explicitly notes that MCP-only installation gives tools but lacks automatic routing enforcement.

## Important design detail: “think in code”

The repository tells the model to stop using the LLM as a bulk data processor. For example, instead of reading dozens of files and asking the model to count lines/functions, it asks the model to write a small script that performs the computation and returns only the result.

This is a significant Harness principle:

> **Move deterministic computation out of the model whenever the computation can be performed by the execution environment.**

This converges with David Ondrej's “deterministic work in code, judgment in prompts” and Matt's preference for deterministic feedback loops.

## Freshness / authority implications

Context Mode's indexed state is useful, but an index is still a projection. The Harness must know:

- which repository revision the index represents;
- which files were included/excluded;
- when the index was refreshed;
- whether the requested fact requires current source rather than historical session state.

The repository's own session continuity feature indexes events rather than dumping them back into context. This is powerful, but it must not turn “previously observed” into “currently true.” That distinction is central to Rta-Smriti and the PMB Filing Contract.

## Disposition

**ADOPT DESIGN PRINCIPLE:** raw tool output does not automatically belong in model context.  
**HIGH-VALUE CANDIDATE:** bounded external evidence store with on-demand retrieval.  
**HIGH-VALUE CANDIDATE:** deterministic computation outside model context.  
**PRIORITY RESEARCH:** whether Context Mode's session/event model can reduce PMB startup and continuation cost without introducing stale-index errors.  
**DO NOT ADOPT YET:** its complete plugin/runtime architecture or license as a PMB dependency.

---

# 2. Headroom — compression + recoverability + measurement

**Repository:** https://github.com/headroomlabs-ai/headroom  
**License:** Apache-2.0  
**Primary mechanism:** local content-aware compression before model ingestion.

## Architecture

Headroom places a local layer between an agent/application and the provider:

```text
agent/app
  -> prompts/tool output/logs/RAG/files
  -> ContentRouter
      -> SmartCrusher (JSON)
      -> CodeCompressor (code)
      -> Kompress model (prose)
  -> CCR cache/retrieval
  -> LLM provider
```

It supports library, proxy, agent wrapping, MCP, and cross-agent memory surfaces.

## Particularly strong ideas

### Content-aware routing

The compressor is not a single generic “shorten this” operation. ContentRouter chooses a representation-specific mechanism. This is a useful counter to simplistic prompt compression.

### CCR: compressed representation + recoverability

Headroom's CCR keeps originals locally and exposes retrieval when the full content is needed. That makes the architecture closer to **bounded representation + authoritative backing store** than to lossy summarization.

### Cache-awareness

CacheAligner identifies volatile content that would damage provider KV-cache prefix reuse and does not rewrite those prompts. This is important because token minimization and cache economics are not identical objectives.

### Output-side optimization

Headroom also experiments with output-token reduction and effort routing. Its README explicitly distinguishes estimated output savings from measured control-arm results. That measurement discipline is more valuable to us than the particular optimization.

## Evidence quality

The current README includes reproducible benchmark commands and multiple scenarios, including code search, SRE debugging, codebase exploration, and GitHub triage. It reports both compression and task-quality evaluations and acknowledges confidence intervals rather than treating small observed deltas as proof.

## Disposition

**ADOPT DESIGN PRINCIPLE:** context optimization should be content-aware and reversible where feasible.  
**ADOPT EVALUATION PRINCIPLE:** report savings against a counterfactual and distinguish estimated from measured savings.  
**PRIORITY RESEARCH:** local reversible compression with retrieval.  
**ORTHOGONAL:** cache-prefix preservation and output-token optimization belong in Harness execution economics, not PMB core.

---

# 3. Magic Compact — preserve conversation structure instead of flattening it

**Repository:** https://github.com/aerovato/magic-compact  
**License:** BSD-3-Clause  
**Primary mechanism:** explicit user-triggered compaction that preserves conversation structure and caches omitted tool I/O.

## Core difference from ordinary compaction

Magic Compact argues that generic compaction collapses the conversation into a summary and loses the working shape of the session. Its alternative:

- preserve user messages verbatim;
- summarize old assistant turns individually;
- prune bulky completed tool I/O;
- preserve omission IDs;
- expose `read_omitted_content` to recover original tool input/output.

It is therefore a **structured externalization mechanism**, not merely a summarizer.

## Important implementation constraint

Claude Code does not expose enough plugin capability to rewrite the current transcript in place. Magic Compact therefore creates a compacted destination session and asks the user to resume it. OpenCode exposes more functionality and supports richer behavior.

This is evidence for a broader principle:

> **The harness should adapt to the actual execution substrate rather than pretending all agent hosts expose the same lifecycle controls.**

## Disposition

**ADOPT DESIGN PRINCIPLE:** compaction should preserve recoverable evidence and user intent rather than flattening everything into one opaque summary.  
**PRIORITY RESEARCH:** event-level/session-skeleton compaction vs PMB durable state.  
**DO NOT ADOPT:** plugin-specific session rewriting as PMB architecture.

---

# 4. Ponytail — reuse-before-invention / minimum implementation

**Repository:** https://github.com/DietrichGebert/ponytail  
**License:** MIT  
**Primary mechanism:** always-on guidance + skills/hooks that push the agent toward the smallest existing solution.

## Decision ladder

The core ruleset is explicit:

1. Does this need to exist? If no, skip it.
2. Does the standard library do it?
3. Does the native platform do it?
4. Does an installed dependency do it?
5. Can one line solve it?
6. Only then implement the minimum correct solution.

The repository explicitly protects trust-boundary validation, data-loss handling, security, and accessibility from the simplification rule.

## Why this matters to PMB/Harness

This is not fundamentally a token optimization tool. It reduces tokens by preventing unnecessary implementation.

That makes it a **decision-discipline mechanism**, not a context compressor.

It directly reinforces existing PMB goals around:

- overengineering;
- premature abstraction;
- existing seams;
- avoiding speculative architecture;
- diminishing returns.

The current repository also includes `ponytail-review`, `ponytail-audit`, `ponytail-debt`, and measured gain reporting. Its rule copies are checked for alignment across agent adapters, which is a useful operational pattern.

## Evidence caution

The README has evolved from earlier headline benchmarks to more conservative real-agent measurements. A later benchmark describes a 54% mean LOC reduction and 20% cost reduction across 12 feature tasks, while the older synthetic benchmark had much higher savings. This is exactly the kind of benchmark evolution the corpus should preserve rather than flatten into one “Ponytail saves X%” claim.

## Disposition

**ADOPT DESIGN PRINCIPLE:** reuse/native/stdlib/dependency checks before invention.  
**MERGES WITH:** existing PMB anti-overengineering and architecture-survey work.  
**CANDIDATE EXPERIMENT:** a narrow “reuse-before-new-mechanism” review pass for implementation tasks.  
**DO NOT ADOPT:** always-on global ruleset without measuring startup context cost.

---

# 5. Graphify — deterministic code projection with explicit edge provenance

**Repository:** https://github.com/Graphify-Labs/graphify  
**Licenses:** Apache-2.0 / MIT components  
**Primary mechanism:** code/docs/media → queryable graph projection.

## Strongest architectural idea

Graphify's current README explicitly distinguishes:

- `EXTRACTED`: relationship explicitly present in source;
- `INFERRED`: relationship resolved by graphify.

That is excellent evidence for the provenance model we have been building. A graph is not presented as undifferentiated truth.

The code graph is built locally with tree-sitter without an LLM. Non-code artifacts can receive a semantic pass through the assistant/backend.

## Output model

The tool produces a graph JSON plus report/visualization and supports:

- cross-file calls/imports/inheritance;
- communities;
- path queries;
- explanations;
- rationale/doc references;
- incremental updates;
- graph merge;
- architecture/call-flow exports;
- PR dashboards and impact analysis.

The repository says graph builds can be committed and shared, with an auto-rebuild hook and a merge driver for graph JSON.

## Important caveat

A graph is a projection. It can be stale, incomplete, or inferred. The existence of `EXTRACTED` vs `INFERRED` labels is therefore more important than the graph itself.

This directly connects to Archify and Rta-Smriti:

> **Projection must carry provenance and freshness; structural usefulness does not make it ground truth.**

## Disposition

**ADOPT DESIGN PRINCIPLE:** deterministic projections can reduce repeated source ingestion.  
**ADOPT PROVENANCE PATTERN:** extracted vs inferred edges should remain distinguishable.  
**PRIORITY RESEARCH:** codebase graph as a bounded context substrate for ACR.  
**MERGES WITH:** Archify architecture-delta research and Rta-Smriti graph/provenance findings.  
**DO NOT ADOPT:** graph as an authoritative replacement for source code.

---

# 6. codebase-memory-mcp — structural knowledge graph + semantic resolution

**Repository:** https://github.com/DeusData/codebase-memory-mcp  
**License:** MIT  
**Primary mechanism:** persistent structural graph with tree-sitter + embedded hybrid type resolution.

## Architecture

The project separates parsing from agent intelligence:

```text
repository
  -> tree-sitter across 158 languages
  -> structural graph
  -> Hybrid LSP refinement for supported languages
  -> persistent SQLite-backed graph
  -> MCP queries
  -> agent interprets results
```

The current implementation exposes structural search, call tracing, architecture summaries, impact analysis, dead-code detection, Cypher-like queries, ADR management, and other graph operations.

## Strong convergence

The project explicitly says it does **not** include an LLM. The MCP server is the structural analysis backend; the client agent is the intelligence layer.

That is a clean separation of:

- deterministic extraction;
- persistent projection;
- model reasoning.

The Hybrid LSP layer is especially relevant to our architecture-drift work because it attempts type-aware call resolution rather than treating text matches as semantic truth.

## Important caution

The repository also contains semantic/vector search and multiple inferred relationships. Those outputs need the same provenance/freshness treatment as Graphify and Archify.

Its persistent cache at `~/.cache/codebase-memory-mcp/` also raises a PMB-style lifecycle question: **when is the graph rebuilt, and how does the system prove that the graph corresponds to the current checkout?**

## Disposition

**HIGH-VALUE RESEARCH:** deterministic code intelligence as an ACR context substrate.  
**MERGES WITH:** Graphify and Archify.  
**CANDIDATE EXPERIMENT:** compare raw repository exploration vs graph-assisted exploration on fixed ACR tasks.  
**DO NOT ADOPT:** persistent graph as canonical repository truth.

---

# 7. Token Optimizer MCP — optimization as an enforced workflow, not optional advice

**Repository:** https://github.com/ooples/token-optimizer-mcp  
**License:** MIT  
**Primary mechanism:** MCP tools + lifecycle hooks + caching/compression + per-project knowledge graph + measurement.

## Current architecture

The current repository is substantially more sophisticated than the video description. It includes:

- MCP tools for smart reads/searches/compression;
- persistent SQLite-backed state;
- Brotli compression;
- large-read hooks;
- client-specific integrations;
- a project knowledge graph of findings/decisions/dead ends;
- savings reports;
- a randomized/control-arm measurement story;
- release provenance/checksum guidance;
- doctor tests that feed synthetic payloads through the real hook path.

The README explicitly says the MCP-only server is advisory, while the plugin/hook integration supplies enforcement.

## Important correction to the video

The video described the project as turning off Claude Code's trust prompt. The current repository evidence reviewed here does **not** support treating that as the current design description. Instead, current docs emphasize workspace trust, hook review, signed/provenance-aware release verification, and configurable refusal/advise/off modes.

That difference is important. The corpus should preserve the video's observation as a **historical/version-specific claim**, not as current project truth.

## Strongest idea

The project makes a distinction we should reuse:

> **Advisory MCP guidance is not enforcement.**

The hooks can refuse large reads, while the model-visible instructions can explain how to use the optimized tools. This matches PMB's layered enforcement model.

## Strong measurement pattern

The project says it measures itself and reports when the optimization loses. It also explicitly distinguishes measured data from estimates and has a doctor test that verifies the actual hook behavior rather than merely checking that configuration files exist.

This is extremely aligned with our corpus governance.

## Risks

- 74 MCP tools in the current release is itself a context/tool-discovery cost risk.
- A project knowledge graph that injects findings automatically can become another startup/retrieval burden.
- Large-read blocking can become an authority problem if the hook misclassifies a necessary read.
- Global/user-scoped hooks create cross-repository behavior that must be governed carefully.

## Disposition

**ADOPT DESIGN PRINCIPLE:** optimization mechanisms that matter should have deterministic enforcement where appropriate, not only instructions.  
**ADOPT EVALUATION PRINCIPLE:** “prove the hook works” rather than “configuration exists.”  
**PRIORITY RESEARCH:** measured large-read redirection and externalization.  
**DO NOT ADOPT YET:** its full MCP/tool surface or global knowledge graph.

---

# 8. LeanCTX — semantic prompt compression

**Repository:** https://github.com/jia-gao/leanctx  
**License:** MIT  
**Primary mechanism:** local LLMLingua-2 prompt compression through OpenAI/Anthropic/Gemini-compatible interfaces.

## What is genuinely different

LeanCTX compresses the **input prompt itself**, including prose, rather than externalizing tool results. Its interface is designed as a drop-in wrapper around common provider clients.

The current README reports a LongBench v2 short-subset comparison against naive head/tail truncation and reports 57% token removal for its Lingua configuration. It also emphasizes local execution and no default external transmission of prompt/user data.

## Risk

Semantic prompt compression is inherently more difficult to reason about than deterministic projection or reversible externalization. The user does not necessarily see exactly which words were removed before the model acts.

The right question is not “does the benchmark improve?” but:

> **Which classes of claims/instructions can be safely compressed, and which must remain byte- or semantically exact?**

This is particularly important for:

- security constraints;
- exact identifiers;
- commands;
- acceptance criteria;
- negative requirements;
- provenance labels;
- policy language.

## Disposition

**PRIORITY RESEARCH:** bounded semantic compression with protected instruction classes.  
**DO NOT ADOPT:** blind global prompt compression for PMB governance/context.  
**LOWER PRIORITY than externalization:** because reversibility and exactness are harder to guarantee.

---

# 9. pxpipe — multimodal context encoding

**Repository:** https://github.com/teamchong/pxpipe  
**License:** MIT  
**Primary mechanism:** render dense textual context as images and send through the model's vision channel.

## Interesting technical idea

The repository measures character density per image token and uses a local proxy to rewrite selected request content into PNGs. It includes a factsheet, manifests, model-specific render profiles, and a profitability gate.

The strongest aspect is not the image trick. It is the **explicit model/workload-specific evaluation**:

- model-specific rendering geometry;
- dense-context recall tests;
- arithmetic/state/never-stated tests;
- exact-token tests;
- cost counterfactuals;
- profitability gate;
- byte-exact content kept as text.

## Critical negative finding

The repository explicitly documents silent failures on dense exact identifiers. For example, its current README reports materially different recall for 12-character hex strings across models and says byte-exact values such as IDs/hashes/secrets must remain text.

That is exactly the kind of failure a token-savings headline can hide.

## Disposition

**RESEARCH ONLY:** multimodal context encoding is an interesting future compression class.  
**ADOPT EVALUATION PATTERN:** model-specific quality gates + profitability gate + protected exact-value classes.  
**DO NOT ADOPT:** as a general-purpose lossless context mechanism.

---

# 10. FreeLLMAPI — provider pooling and routing

**Repository:** https://github.com/tashfeenahmed/freellmapi  
**License:** MIT  
**Primary mechanism:** self-hosted OpenAI-compatible gateway across free provider tiers with routing/failover.

## Architecture relevance

The project is a local/self-hosted proxy. Current public material describes a single `/v1` endpoint over many provider/model endpoints, encrypted provider keys, model routing, rate-limit-aware fallback, and usage tracking.

The important Harness concept is **provider abstraction without forcing the agent to know provider-specific APIs**.

This reinforces the Execution Profile work from Adrian/Ralphy:

```text
agent task
  -> execution gateway
      -> provider/model/runtime identity
      -> capability/health/quota
      -> routing/fallback
  -> execution result
```

## Risks

Free-tier availability is volatile. Provider terms, limits, model IDs, and endpoint behavior change. A routing layer therefore needs freshness and operational health, not just a static model catalog.

The project itself is a good example of why provider information needs dates and provenance. Its catalog/recent updates explicitly track changed/retired free tiers.

## Disposition

**ORTHOGONAL TO PMB:** execution-layer infrastructure.  
**ADOPT DESIGN PRINCIPLE:** provider identity, routing, quota, and health belong in execution configuration, not model identity.  
**CANDIDATE FOR EXECUTION PROFILE RESEARCH:** alongside LiteLLM/OmniRoute.  
**DO NOT ADOPT AS PMB DEPENDENCY.**

---

# 11. OmniRoute — routing + compression + resilience

**Repository:** https://github.com/diegosouzapw/OmniRoute  
**License:** MIT  
**Primary mechanism:** multi-provider gateway with routing, quota-aware fallback, compression, session/context lifecycle, circuit breakers, and MCP/A2A surfaces.

## Architecture

The current architecture documentation identifies explicit services for:

- account selection/scoring;
- context lifecycle;
- IP filtering;
- session management;
- request deduplication;
- system prompt injection;
- thinking-budget management;
- wildcard model routing;
- rate-limit management;
- circuit breakers;
- context handoff;
- compression pipelines.

This makes OmniRoute relevant to the execution-layer model much more than simply “a cheaper API endpoint.”

## Important convergence

The combination of:

```text
provider selection
+ health/resilience
+ context lifecycle
+ compression
+ handoff
```

is essentially an **execution runtime**.

That is distinct from PMB and should stay distinct.

## Risk

The feature surface is large. A gateway that performs routing, compression, context handoff, system-prompt manipulation, MCP, A2A, session tracking, and UI functions becomes a significant source of policy and behavior. This is exactly where the corpus's “governance complexity is an evaluation signal” principle applies.

## Disposition

**HIGH-VALUE PRIOR ART:** execution/runtime layer.  
**MERGES WITH:** Adrian Cockcroft Execution Profile, Ralphy execution options, LiteLLM questions.  
**DO NOT ADOPT INTO PMB:** gateway feature surface or provider routing as memory architecture.  
**RESEARCH:** whether execution gateway telemetry should be captured as lightweight run provenance.

---

# Cross-repository synthesis

## A. “Context compression” is six different things

The video's title collapses important distinctions. The corpus now supports this taxonomy:

| Mechanism | Example | Original retained? | Retrieval path? | Main risk |
|---|---|---:|---:|---|
| Externalization | Context Mode | Yes | Indexed search/fetch | stale/missing retrieval |
| Structured compaction | Magic Compact | Yes | omission ID/tool | session reconstruction |
| Reversible compression | Headroom | Yes | retrieval | semantic distortion |
| Semantic compression | LeanCTX | Not necessarily | not inherently | silent instruction loss |
| Multimodal encoding | pxpipe | transformed | no general byte-exact recovery | visual misread/confabulation |
| Structural projection | Graphify/codebase-memory | source remains | graph query | stale/inferred projection |
| Work reduction | Ponytail | N/A | N/A | oversimplification |

This taxonomy should replace generic use of “token optimization” in future research notes.

---

# B. The most important design question is preservation, not compression

For any proposed context-saving mechanism, require an explicit answer to:

1. **What is authoritative?**
2. **What representation enters model context?**
3. **What information is omitted/transformed?**
4. **Can the original be recovered?**
5. **How is retrieval invoked?**
6. **How is freshness established?**
7. **What content is protected from transformation?**
8. **What is the counterfactual token/cost baseline?**
9. **What quality regression is measured?**
10. **What additional startup/tool/hook context does the mechanism itself introduce?**

This is a candidate reusable Harness evaluation contract.

---

# C. “Compute, don't read” is underappreciated

Context Mode's strongest idea may not be storage. It is the instruction to have the execution environment perform bulk deterministic analysis.

Instead of:

```text
50 files -> 700 KB -> model
```

prefer:

```text
50 files -> deterministic program -> 3.6 KB result -> model
```

This is a general Harness pattern:

> **Use the model for judgment; use deterministic tools for enumeration, counting, transformation, filtering, and other mechanically verifiable work.**

This converges with existing Harness Engineering work and should not become another standalone subsystem unless experiments show a gap.

---

# D. Externalized evidence + bounded context is the strongest emerging architecture

Across Context Mode, Headroom, Magic Compact, Rta-Smriti, and OpenMAIC, a common pattern is now well supported:

```text
                DURABLE / EXTERNAL STATE
                         │
                  provenance + freshness
                         │
                   selection / query
                         │
                 bounded representation
                         │
                     MODEL CONTEXT
                         │
                  judgment / decision
                         │
                 execution / verification
```

The critical missing research question is whether PMB can implement this pattern **without creating a second competing datastore or adding more startup context**.

---

# E. Graphs are projections, not truth

Graphify and codebase-memory-mcp provide strong prior art for deterministic codebase projection. Archify provides a narrower architecture-model comparator. Rta-Smriti provides broader evidence/provenance semantics.

The combined lesson is:

```text
source reality
   ↓
projection
   ↓
provenance + revision + freshness
   ↓
query
   ↓
agent judgment
```

Never:

```text
projection = truth
```

This should sharpen the existing ACR architecture-drift experiment rather than create a generic “knowledge graph” subsystem.

---

# F. Optimization mechanisms can consume the context they save

This is a particularly important corpus warning.

A context optimizer can add:

- MCP tool definitions;
- startup instructions;
- routing prompts;
- status lines;
- retrieval metadata;
- memory injections;
- indexes;
- hooks;
- background processes;
- diagnostics.

Therefore:

> **Net context cost must include the optimizer itself.**

This is directly relevant to PMB because the Filing Contract already found that startup context is a real operational constraint.

Candidate metric:

```text
net context benefit
= baseline context
  - optimized task context
  + optimizer startup cost
  + optimizer tool/schema cost
  + retrieval overhead
  + correction/recovery cost
```

A mechanism that saves 50K tokens from a tool output but injects 15K tokens of always-on instructions and 20 MCP tools may be a net loss.

---

# G. “Instruction vs enforcement” converges again

Several projects reveal the same pattern:

- Ponytail: instructions + hooks/adapters.
- Context Mode: MCP tools + lifecycle routing hooks.
- Token Optimizer: advisory MCP vs hook enforcement.
- Graphify: skill + deterministic extractor.
- OmniRoute: runtime gateway enforcement.

The corpus continues to support:

> **Use instructions for judgment and routing; use deterministic mechanisms for guarantees.**

This is consistent with PMB's existing layered enforcement ladder.

---

# Proposed Harness evaluation rubric for context mechanisms

Before admitting any context optimization into PMB/ACR, evaluate it on these axes:

### 1. Preservation

- lossless
- reversible
- bounded-loss
- irreversible

### 2. Authority

- original source retained
- derived projection
- model-generated summary
- model-generated hypothesis

### 3. Retrieval

- explicit deterministic retrieval
- semantic retrieval
- automatic injection
- no recovery

### 4. Freshness

- revision-bound
- event-bound
- timestamp-only
- unknown

### 5. Precision sensitivity

Can it preserve exact:

- IDs
- hashes
- commands
- policy text
- negative requirements
- acceptance criteria
- security constraints

### 6. Enforcement

- advisory only
- hook-enforced
- runtime-enforced
- deterministic CI-tested

### 7. Measurement

- anecdotal
- synthetic benchmark
- controlled benchmark
- production counterfactual
- confidence interval / uncertainty reported

### 8. Net cost

Measure:

- input tokens;
- output tokens;
- tool-definition tokens;
- startup tokens;
- retrieval tokens;
- latency;
- local CPU/memory;
- cache effects;
- correction/recovery cost.

### 9. Operational complexity

- one command;
- one hook;
- plugin;
- proxy;
- persistent database;
- background process;
- multiple authority surfaces.

### 10. Reversibility

- disable without data migration;
- restore original transcript;
- retrieve omitted evidence;
- reproduce baseline.

---

# Candidate experiments

## Experiment A — PMB startup vs task-context projection

Take a representative PMB session and compare:

1. current startup memory path;
2. bounded task-specific projection;
3. externalized history + targeted retrieval.

Measure:

- startup tokens;
- first useful action latency;
- task success;
- corrections;
- missed evidence;
- retrieval count;
- total lifecycle tokens.

**Gate:** no adoption unless net lifecycle cost improves.

## Experiment B — raw tool output externalization

On a fixed coding task, compare:

- raw tool output in context;
- compressed tool output;
- externalized output + retrieval;
- deterministic computation returning only a result.

Measure exact-answer fidelity, retrieval misses, correction rate, and token cost.

## Experiment C — graph-assisted ACR exploration

Compare raw repository exploration against:

- Graphify;
- codebase-memory-mcp;
- existing ACR/static-tool evidence.

Measure:

- time to locate affected symbols;
- tokens;
- false relationships;
- stale graph errors;
- architecture-drift detection recall/precision.

## Experiment D — reuse-before-invention review

Add a narrow review question:

> “Before adding this mechanism/dependency/abstraction, what existing capability could satisfy the requirement?”

Measure deleted code, rejected dependencies, review time, and false positives.

Do not make it an always-on global prompt until net value is demonstrated.

## Experiment E — context optimizer overhead

For every candidate optimizer, record its own:

- system/context additions;
- tool count;
- hooks;
- startup time;
- persistent storage;
- retrieval traffic.

Compute net savings rather than vendor headline savings.

---

# Final dispositions

| Candidate | Disposition | Confidence |
|---|---|---:|
| Context Mode | **HIGH-VALUE RESEARCH** | High |
| Headroom | **HIGH-VALUE RESEARCH** | High |
| Magic Compact | **HIGH-VALUE RESEARCH** | High |
| Ponytail | **ADOPT DESIGN PRINCIPLE / MERGE WITH EXISTING WORK** | High |
| Graphify | **HIGH-VALUE RESEARCH; MERGE WITH ARCHIFY** | High |
| codebase-memory-mcp | **HIGH-VALUE RESEARCH; MERGE WITH GRAPH/ACR WORK** | High |
| Token Optimizer MCP | **HIGH-VALUE RESEARCH; strong enforcement/measurement prior art** | High |
| LeanCTX | **PRIORITY RESEARCH, lower than reversible approaches** | Medium |
| pxpipe | **RESEARCH ONLY / useful negative evidence** | High |
| FreeLLMAPI | **EXECUTION-LAYER PRIOR ART** | High |
| OmniRoute | **EXECUTION-LAYER PRIOR ART** | High |

## Explicit non-adoptions

Do **not** adopt any of the following wholesale based on this corpus entry:

- a second PMB datastore;
- a generic always-on memory RAG layer;
- blind semantic prompt compression;
- image encoding as “lossless” context;
- a knowledge graph as canonical code truth;
- global optimizer hooks without measured net benefit;
- large MCP surfaces merely because they save downstream tokens;
- provider routing inside PMB;
- token savings as a sufficient optimization objective.

## Bottom line

The most important result of this research is not “which token-saving repo should we install?”

It is this:

> **The Harness should treat model context as a bounded working set over durable evidence, not as the durable evidence store itself.**

The best prior art does not merely shorten text. It **moves information to the right layer**:

- deterministic computation to the runtime;
- raw evidence to durable/local storage;
- structural relationships to deterministic projections;
- bounded summaries to model context;
- decisions to governed durable state;
- execution identity to the execution layer;
- verification to independent/deterministic mechanisms.

That is strongly convergent with Rta-Smriti, OpenMAIC, Hindsight, the PMB Filing Contract, and the existing Harness Engineering model. The next step should therefore be **measurement on our actual PMB context problem**, not installation of another context-management framework.
