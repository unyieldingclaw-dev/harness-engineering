# The Stack — DeepSeek V4.1 Flash, HySparse2 & Million-Token Recall — 2026-09-28

## Purpose

Review The Stack's video **“Xiaomi & DeepSeek Solved A Massive Problem In AI”** as a corroborating source for the HySparse2 / DeepSeek-V4.1-Flash research already mined into HE.

Primary user source:

- Video: https://www.youtube.com/watch?v=85QP5JDZfQM
- Creator: The Stack
- Transcript supplied by the user on 2026-09-28

Primary technical sources checked:

- Xiaomi / LLM-Core, **HySparse2: Hybrid Sparse Attention with Two-Level KV Sharing** — https://arxiv.org/abs/2609.26368
- DeepSeek-AI, **DeepSeek-V4.1-Flash: Pushing the Limits of KV Cache Compression** — https://arxiv.org/abs/2609.19969
- DeepSeek-V4.1-Flash model repository — https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash
- DeepSeek API pricing — https://api-docs.deepseek.com/quick_start/pricing/
- Artificial Analysis long-context material referenced by the video, treated as secondary evidence where exact current rows were not independently pinned

Related HE evidence already present:

- `01 Research/Sources/Cloud Codes — HySparse2, DeepSeek V4.1 Flash & Agent Observation Economics — 2026-09-26.md`
- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`
- `01 Research/Context Engineering.md.md`

This note is deliberately short because the underlying architecture has already been deeply mined. Its purpose is provenance, corroboration and claim-boundary checking, not a duplicate synthesis.

---

# Executive assessment

The video is unusually careful about the main evidence boundary.

Its strongest statement is effectively:

> **The memory/cache savings are strongly supported; reliable retrieval at the far end of the advertised 1M-token window is not equivalently demonstrated.**

That matches the earlier HE deep dive.

The video therefore adds **corroboration**, not a new architecture thread.

---

# 1. DeepSeek's cache-compression claim checks out

DeepSeek-V4.1-Flash's official model card describes:

- a 40-layer causal encoder-decoder architecture;
- 20 encoder + 20 decoder layers;
- approximately 8B active parameters/token during prefill and 16B during decode;
- CSA2 attention modes that reuse KV and/or sparse Top-K indices across layers;
- FP4 main KV caching;
- a reported global KV footprint of approximately **890 bytes/token**, roughly one quarter of DeepSeek-V4-Flash.

The model supports a 1M-token context window.

### HE disposition

**REINFORCE** the existing rule that architecture-specific growing-state topology is a real execution-envelope variable.

---

# 2. DeepSeek pricing confirms that cache economics are operational, not merely theoretical

DeepSeek's current pricing page lists cache-hit, cache-miss and output prices separately and explicitly notes that prices may change.

At the time of this review, the page lists off-peak cache-hit pricing for the Flash route at **$0.003 per million input tokens**.

The exact number is ephemeral.

### HE implication

> **Architecture can create a real provider cost advantage, but pricing evidence must remain timestamped.**

Keep the causal mechanism; let the price expire.

**Disposition: REINFORCE existing economic-evidence rule.**

---

# 3. Xiaomi's 1M resource result and 256K quality result remain different evidence horizons

The HySparse2 paper reports approximately:

```text
1M-token resource analysis
Hybrid SWA KV      12.09 GB
HySparse2 KV        2.69 GB
prefill FLOPs       ~5.02x lower vs Hybrid SWA
```

Its strongest long-context retrieval/agent quality evidence is reported through 256K, including RULER-v2 58.45 for the post-trained HySparse2 comparison model.

That does not prove equivalent retrieval quality at 1M.

### Candidate principle already mined

> **The validation horizon must cover the operating horizon of the claim.**

**Disposition: CORROBORATE; no new principle needed.**

---

# 4. DeepSeek's 1M window is usable for agent benchmarks but that is not the same as a 1M retrieval proof

DeepSeek's model card says code-agent evaluations use a 1M context configuration.

That establishes that the execution path supports large-window agent runs.

It still does not answer a narrower question:

> Can the model reliably retrieve a specific old fact at every depth through 1M tokens?

Those are different properties.

### HE implication

Keep separate:

```text
accepts 1M input
runs agent workload with 1M budget
benefits from larger context on some tasks
retrieves arbitrary deep evidence reliably at 1M
```

Evidence for one should not silently prove all four.

**Disposition: REINFORCE evidence semantics.**

---

# 5. Independent long-context testing is useful but still horizon-bounded

The video cites Artificial Analysis testing that reaches roughly the 10K–100K range and reports a strong result for DeepSeek-V4.1-Flash.

This is useful independent evidence at that horizon.

Because the exact current Artificial Analysis result was not independently pinned during this HE pass, the numeric score remains **secondary-source evidence** in this note rather than a durable HE fact.

More importantly, even a verified 100K result would not become 1M evidence.

**Disposition: RETAIN boundary; PARK exact secondary number.**

---

# 6. “Keep important information recent” is a useful operational heuristic, not an HE invariant

The video recommends keeping important requirements/errors recent or repeating them near the end of a prompt because sparse architectures guarantee access to recent context while older information may compete for sparse selection.

That is plausible for the architectures discussed.

HE should not convert it into a universal instruction such as “always repaste important context.”

Why:

- model architectures differ;
- clients may cache, summarize, retrieve or pin context differently;
- repeated context can create contradictions and token waste;
- authoritative project state should remain in its owning artifact rather than depend on recency tricks.

The stronger HE formulation is:

> **Critical evidence and requirements must remain reliably addressable by the execution path; recency is only one possible mechanism.**

**Disposition: REFINE / DO NOT universalize prompt recency.**

---

# 7. Relationship to the previous Cloud Codes source

The prior Cloud Codes deep dive already mined:

- observation amplification;
- cache-miss versus context-growth distinction;
- KV Bridging / KV Reuse;
- validation-horizon mismatch;
- persist-by-reuse-horizon;
- multi-axis context-state compression;
- tool-output control as execution engineering.

The Stack video independently lands on the same central evidence boundary and therefore increases confidence in the synthesis without creating a new HE subsystem.

### Disposition

**CORROBORATING SOURCE.**

No separate canonical architecture change is justified from this video alone.

---

# Bottom line

The video is a good evidence-discipline follow-up:

> **Trust the demonstrated cache/resource savings at the lengths where they are derived or measured; do not promote an advertised 1M context capacity into verified 1M retrieval quality without a matching evaluation horizon.**

That is already captured in HE and should remain the canonical interpretation.
