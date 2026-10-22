# Cloud Codes — HySparse2, DeepSeek V4.1 Flash & Agent Observation Economics — 2026-09-26

## Purpose

Deep-dive Cloud Codes' video **“Xiaomi and DeepSeek Just Solved the Biggest Problem in AI”** and mine only the durable Harness Engineering implications.

Primary user source:

- Video: https://www.youtube.com/watch?v=eXad7m0TjVM
- Creator: Cloud Codes
- Transcript supplied by the user on 2026-09-26

Primary technical sources checked:

- Xiaomi / LLM-Core, **HySparse2: Hybrid Sparse Attention with Two-Level KV Sharing** — https://arxiv.org/abs/2609.26368
- Xiaomi / LLM-Core, **HySparse: A Hybrid Sparse Attention Architecture with Oracle Token Selection and KV Cache Sharing** — https://arxiv.org/abs/2602.03560
- DeepSeek-AI, **DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression** — https://arxiv.org/abs/2609.19969
- Microsoft Research, **You Only Cache Once: Decoder-Decoder Architectures for Language Models (YOCO)** — https://arxiv.org/abs/2405.05254

Related HE research:

- `01 Research/Context Engineering.md.md`
- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Skill Routing, Gate-Bound Evidence & Orthogonal Review — 2026-09-26.md`

This note is research evidence. It does **not** authorize changing PMB/ACR, installing a model, building a context proxy, or adding a generalized output-filtering subsystem.

---

# Executive finding

The video's headline is broader than the evidence. Xiaomi and DeepSeek did not “solve the biggest problem in AI.”

They are attacking a narrower and highly relevant agent-runtime problem:

> **Long-horizon agents often generate short actions but receive long observations from tools and environments, making the workload increasingly input-heavy.**

This pattern increases several costs at once:

```text
short action / tool call
        ↓
large observation
        ↓
more prefill work
+ more retained context state
+ more KV/cache storage
+ more retrieval difficulty
+ more transfer/persistence pressure
```

HySparse2 and DeepSeek-V4.1-Flash address these costs inside model/runtime architecture. HE cannot retrofit those architectures into existing hosted models, but the research gives HE a stronger reason to control **observation amplification** at the harness boundary.

The strongest practical HE rule from this pass is:

> **Keep the evidence; discard the exhaust.**

Raw tool evidence may need to remain available, but active model context should receive only the portion needed for the current decision whenever that reduction can be done without destroying diagnostic evidence.

---

# 1. Observation amplification is a real agent workload characteristic

The video's opening example is intentionally dramatic: a tiny command such as `npm test` can produce tens of thousands of tokens of logs and stack traces.

The exact token count is anecdotal, but the workload shape is supported directly by HySparse2. Its introduction says agentic inference repeatedly produces short actions/tool calls that return much longer search results, execution traces, or documents. Those observations require prefill before decoding resumes and accumulate across turns.

DeepSeek-V4.1-Flash independently motivates its architecture with the same shift: long-horizon agent workloads are increasingly input-heavy, and prefill plus KV storage/transfer remain major deployment costs.

### HE implication

> **Action size and observation size are independent dimensions of agent cost.**

A tool that is cheap to invoke can still be expensive for the agent if its observation is large, noisy, repetitive, or difficult to retrieve from later.

This suggests a useful term for HE:

**observation amplification** — the ratio or qualitative gap between a small agent action and the volume of state returned to active context.

HE does not need a universal numeric threshold. The concept matters when observation size materially affects correctness, context pressure, latency, or cost.

**Disposition: NEW / STRONGLY MINE.**

---

# 2. Context growth and cache misses are separate costs

The video says the model must “re-read everything” each turn. That is directionally useful but too broad.

A working KV/prefix cache can avoid recomputing unchanged prior context. However:

- new observations still require prefill;
- caches can miss or be invalidated;
- longer retained context still increases storage/transfer pressure;
- the model still must retrieve the relevant evidence from a larger history.

DeepSeek explicitly treats KV-cache persistence and replay as deployment concerns, which would be unnecessary if long histories were simply recomputed from scratch every turn.

### HE implication

Keep these failure/cost modes separate:

```text
context grew
cache missed
new observation requires prefill
cache transfer/storage became expensive
retrieval quality degraded
```

They may produce similar latency symptoms but have different owners and remedies.

**Disposition: REINFORCE owning-component diagnosis.**

---

# 3. HySparse2 mechanics are supported by the paper

HySparse2 uses an 80B-total / 3B-active MoE research model with 49 Transformer layers.

Its attention design has two levels of sharing.

## Outer level — KV Bridging

The backbone is split into:

- a lower **self-decoder**;
- an upper **cross-decoder**.

Cross-decoder full-attention KV caches are generated from hidden states produced by corresponding full-attention layers in the self-decoder. Because all required cross-decoder cache state can be constructed from self-decoder outputs, prompt prefill can exit after the self-decoder instead of executing the full network.

The paper explicitly connects this structure to YOCO.

## Inner level — KV Reuse

Within each hybrid block, sparse layers reuse:

- the preceding full-attention layer's KV cache;
- the positions selected by that full-attention layer.

HySparse2 refines the earlier HySparse design by:

- replacing block-level selection with **token-level selection**;
- removing the separate SWA branch from sparse layers;
- forcing the most recent **128 tokens** into sparse selection;
- selecting **1,024 global tokens** in addition to that local window.

Only five of the 49 layers use full attention in the evaluated 80B-A3B configuration.

### HE treatment

The architecture is useful evidence for selective state reuse and progressive reduction, but HE should **not** translate internal token-level attention selection directly into an external retrieval algorithm. Model attention and repository/tool retrieval are different mechanisms.

**Disposition: MINE principle; do not cargo-cult implementation.**

---

# 4. Xiaomi's resource improvements are substantial — and narrowly scoped

For the 80B-A3B research configuration using FP8 KV-cache storage, the paper reports at 1M tokens:

```text
KV cache
Hybrid SWA   12.09 GB
HySparse      6.72 GB
HySparse2     2.69 GB

Prefill FLOPs reduction for HySparse2
vs HySparse      2.92×
vs Hybrid SWA    5.02×
```

These are not universal model numbers.

The prefill result is based on computed FLOPs, not a claim of exactly 5.02× lower wall-clock latency on arbitrary hardware/runtime stacks.

### HE implication

> **Resource-model evidence should preserve the unit being measured.**

FLOPs, wall-clock latency, bytes/token, total GB, HBM residency, and persistent storage are different measurements and should not be collapsed into “faster” or “smaller.”

**Disposition: REINFORCE measurement semantics.**

---

# 5. The validation horizon does not reach the full claimed operating horizon

This is the most important evidence-quality caveat in the source.

HySparse2 reports resource analysis through **1M tokens**.

Its post-training long-context/agent evaluation is reported through **256K tokens**. At 256K, RULER-v2 is:

```text
HySparse2     58.45
HySparse      32.61
Hybrid SWA    35.74
```

The result is encouraging and shows the sparse/reuse design did not merely buy efficiency by obviously destroying retrieval at the evaluated lengths.

But it does **not** establish equivalent retrieval quality at 1M.

### Candidate HE rule

> **The validation horizon must cover the operating horizon of the claim.**

More precisely:

```text
resource behavior verified at 1M
+
correctness/retrieval verified at 256K
≠
correctness verified at 1M
```

This is a reusable evidence-boundary rule for context windows, session duration, workload scale, concurrency, repeated iterations, or any other dimension where validation is performed at a smaller operating point than the headline claim.

**Disposition: NEW / STRONGLY MINE.**

---

# 6. DeepSeek independently targets the same input-heavy pressure

DeepSeek-V4.1-Flash is a different model and cannot be compared numerically head-to-head with the Xiaomi research model.

The DeepSeek report describes:

- 552B backbone parameters;
- context up to 1M tokens;
- a Causal Encoder-Decoder architecture;
- roughly 8B active parameters/token during prefill;
- roughly 16B active parameters/token during decode;
- cross-layer KV reuse in CSA2;
- FP4 KV caching;
- global KV footprint of about **890 bytes/token**, approximately one quarter of its V4-Flash baseline;
- **SWA Bounded Replay**, reducing persistent KV footprint to roughly one eighth of the previous baseline.

This is independent support for the same broad workload pressure: prefill, KV storage, bandwidth, and persistence become first-order concerns in long-horizon agents.

### Important non-comparison

Do not compare Xiaomi's `2.69 GB @ 1M` directly with DeepSeek's `890 bytes/token` as if one architecture is smaller.

The models, precision, cache definitions, architecture, and measurement basis differ.

**Disposition: CORROBORATE workload pressure; REJECT direct ranking.**

---

# 7. The “copied” narrative is not supported

The close publication dates are attention-grabbing, but the technical lineage is documented.

YOCO (2024) introduced a self-decoder / cross-decoder architecture in which global KV state is constructed once and reused, with early-exit prefill.

Xiaomi's original HySparse (February 2026) introduced cross-layer reuse of a full-attention layer's KV cache and selected positions by subsequent sparse layers.

HySparse2 combines a YOCO-style outer decoder structure with HySparse-style inner block reuse.

DeepSeek's report independently uses a causal encoder-decoder plus cross-layer sparse-attention KV reuse and references prior work in these areas.

### HE implication

The useful lesson is not attribution drama. It is convergence:

> **When independent systems facing the same workload pressure converge on similar mechanisms, treat that as stronger evidence that the pressure is real — not as proof that the mechanism should be copied into HE.**

**Disposition: REINFORCE evidence weighting.**

---

# 8. Persist state according to reuse horizon

DeepSeek's deployment design contributes a durable systems lesson beyond model architecture.

It distinguishes state that has long-lived reuse value from state that can be reconstructed cheaply enough not to justify equivalent persistence. SWA Bounded Replay reduces persistent KV storage by replaying a bounded amount of state rather than keeping the whole transient cache representation indefinitely.

HE should not mimic DeepSeek's cache format, but the ownership rule generalizes:

> **Persistence should be justified by expected reuse value; cheap short-lived derived state may be better recomputed than durably stored.**

This applies conceptually to:

- generated indexes;
- tool-result derivatives;
- intermediate summaries;
- temporary retrieval rankings;
- caches;
- handoff-derived views;
- verification intermediates.

It does **not** mean authoritative evidence should be discarded. The raw/owning source remains the truth when later recomputation depends on it.

**Disposition: NEW / STRONGLY MINE.**

---

# 9. Context efficiency has multiple independent axes

HySparse2 explicitly organizes KV-cache compression across several dimensions:

- **head** — reduce/share KV heads or latent representations;
- **sequence** — retain fewer token positions/cache entries;
- **layer** — share cache state across layers;
- **precision** — store cache values with lower numerical precision.

DeepSeek adds deployment concerns such as persistence and replay.

A useful generalized HE abstraction is therefore:

```text
what state exists
× how much is selected
× how often it is duplicated
× representation precision
× placement
× persistence lifetime
× recompute policy
```

This is not a proposed HE implementation matrix. It is a reminder that “context size” or “cache size” is a composite result, not a single design lever.

**Disposition: STRONGLY MINE as measurement taxonomy.**

---

# 10. Tool-output control is execution engineering, not cosmetic formatting

The most actionable HE consequence exists above the model architecture.

Large tool observations can impose:

- semantic noise;
- active-context occupancy;
- new prefill work;
- KV/cache growth;
- cache transfer/persistence cost;
- retrieval difficulty;
- slower recovery after cache misses.

Therefore noisy shell/test output is not merely ugly presentation.

### Candidate HE rule

> **Tool interfaces should preserve diagnostic evidence while minimizing irrelevant active-context payload.**

A useful pattern is:

```text
raw artifact retained by owning tool/system
        ↓
deterministic extraction/filtering where practical
        ↓
bounded relevant result enters active model context
        ↓
raw evidence remains addressable on demand
```

This is intentionally stronger than “summarize logs.”

An LLM-generated summary can omit the exact line needed to diagnose a failure. Prefer deterministic filtering/structuring when the output format permits it, and preserve access to raw evidence.

Examples of candidate mechanisms when a demonstrated problem exists:

- failure-only test output;
- deduplicated repeated stack frames/messages;
- bounded head/tail plus an artifact pointer;
- structured counts/status plus targeted failing sections;
- paging/search tools instead of injecting the entire artifact;
- explicit expansion when the model needs more evidence.

Do not implement a universal filter before reproducing the failure mode and identifying the tool/runtime that owns the output.

**Disposition: NEW / STRONGLY MINE; IMPLEMENT only from evidence.**

---

# 11. Architecture efficiency does not excuse harness inefficiency

A future model may make 1M-token attention dramatically cheaper.

That does not make irrelevant context free.

Even with cheaper prefill/cache storage, excess tool output can still:

- dilute relevant evidence;
- increase retrieval difficulty;
- create persistence/transfer work;
- make human inspection harder;
- increase provider/API token usage on systems without the same architecture.

### Candidate HE rule

> **Use model/runtime efficiency to widen the safe envelope, not to justify avoidable harness waste.**

HE should continue progressive disclosure and output discipline even as model architectures improve.

**Disposition: REINFORCE.**

---

# 12. Claim audit

| Claim | Evidence status | HE treatment |
|---|---|---|
| Agent loops often produce short actions and long observations | Supported by both HySparse2 and DeepSeek motivation | Retain |
| HySparse2 reduces 1M KV from 12.09 GB to 2.69 GB vs Hybrid SWA | Supported for Xiaomi's 80B-A3B FP8 research setup | Retain with scope |
| HySparse2 cuts prefill by 5.02× | Supported as FLOP reduction vs Hybrid SWA, not universal wall-clock speed | Retain with unit |
| Retrieval improves at 256K | Supported in paper | Retain |
| Retrieval is proven at 1M | Not established by cited evaluation | Reject inference |
| Five of 49 layers use full attention | Supported for evaluated HySparse2 model | Retain as architecture example |
| Sparse layers use 1,024 global + 128 recent tokens | Supported | Retain as architecture example |
| DeepSeek uses the same broad bridge/reuse ideas | Directionally supported, with different implementation | Retain without equivalence claim |
| Xiaomi and DeepSeek can be ranked from published cache numbers | Invalid comparison | Reject |
| One lab copied the other | Not supported; clear prior-art lineage exists | Reject |
| Future efficient 1M models remove need for context discipline | Unsupported | Reject |

---

# 13. Durable HE findings

## STRONGLY MINE

1. **Observation amplification is a first-class agent workload characteristic.**
2. **Validation horizon must cover operating horizon.**
3. **Persist derived state according to expected reuse horizon; recompute cheap short-lived state where appropriate.**
4. **Context/cache efficiency spans selection, sharing, precision, placement, persistence, and recomputation.**
5. **Tool-output control is runtime/context engineering, not cosmetic formatting.**
6. **Preserve raw evidence while admitting only needed evidence into active context when feasible.**

## REINFORCE

- progressive disclosure;
- context quality over raw context quantity;
- measurement-unit precision;
- owning-component diagnosis;
- exact workload/runtime provenance;
- deterministic extraction before probabilistic summarization when the format allows it;
- model/runtime improvements do not eliminate harness responsibility.

## ASSESS

- whether HE/PMB/ACR has reproducible cases where tool output materially pollutes context or causes retries;
- whether existing clients already truncate/filter shell and test output adequately;
- whether raw artifacts remain addressable after any filtering;
- whether validation/eval claims in current projects exceed the scale actually tested.

## PARK

- HySparse2/DeepSeek architecture implementation in HE;
- custom KV-cache manager;
- long-context proxy or generalized compression service;
- 1M-context local inference infrastructure;
- architecture-specific sparse retrieval algorithms.

## REJECT

- “bigger context means dump everything in”;
- treating a FLOP reduction as an equal wall-clock speedup;
- extrapolating correctness beyond the tested horizon;
- LLM summarization as the default evidence filter;
- discarding raw evidence to save context;
- building an output-control subsystem before a reproducible ownership-local failure exists.

---

# Bottom line

HySparse2 and DeepSeek-V4.1-Flash are valuable to HE because they make one workload fact impossible to ignore:

> **Long-running agents are often constrained more by the observations they ingest than by the actions they generate.**

Model labs are attacking that pressure with cache sharing, sparse selection, early-exit prefill, reduced precision, and bounded replay.

HE's controllable layer is different:

> **Preserve authoritative raw evidence outside active context, feed the model only the evidence needed for the current decision when that can be done safely, and verify claims at the same scale at which the system is expected to operate.**
