# Cloud Codes — Local Coding Models, VRAM Headroom & Runtime Fit — 2026-09-24

## Purpose

Mine durable Harness Engineering lessons from Cloud Codes' video **“Best Local Coding AI for Your GPU (4GB to 512GB)”** without turning its hardware ladder or model picks into HE doctrine.

Primary user source:

- Video: https://www.youtube.com/watch?v=qkRIW2ieOK8
- Creator: Cloud Codes
- Transcript supplied by the user on 2026-09-24

Independent sources checked during review:

- llama.cpp memory/KV-cache implementation and maintainer explanations
- `ornith-ai/Ornith-1.5-9B-GGUF`
- `ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF`
- `Qwen/Qwen3-Coder-Next`
- `TokenRhythm/NeoHorse-1-4B`
- DwarfStar / ds4 documentation for DeepSeek V4.1 Flash storage/streaming

Related HE research:

- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`

This is research evidence. It does **not** authorize changing ACR's default model, buying hardware, adding a local-model router, or redesigning PMB.

---

# Executive finding

The video's strongest contribution is not its model-by-VRAM chart.

It is this distinction:

> **A model fitting in memory is a compatibility fact. A model helping on the real task is an empirical outcome.**

For local inference, a nominal model name is not enough to describe an execution target. Useful local-model identity includes at least:

```text
model/checkpoint
+ quantization
+ runtime/version
+ context configuration
+ hardware/backend placement
+ offload topology
+ workload
```

The physical execution budget is also broader than weight-file size. In common local runtimes, memory is consumed by model weights, KV/context state, compute/intermediate buffers, outputs and other runtime allocations. Context therefore has both a semantic cost and a physical memory/throughput cost.

This adds an important missing dimension to HE's existing model-tiering work:

> **Route by measured workload fitness under the actual execution envelope, not by model reputation, parameter count, advertised context window, or “it fits.”**

---

# 1. The “weights-only trap” is real

The video frames the practical memory budget as:

```text
weights + KV cache + runtime scratch space
```

That wording is simplified, but the mechanism is sound.

llama.cpp exposes separate allocations for:

- model-weight buffers;
- KV-cache buffers;
- output buffers;
- compute/intermediate buffers;
- backend-specific memory.

The exact allocation model depends on architecture/runtime/backend, so HE should not turn the video's three buckets into a universal formula. The durable point is:

> **Weight size is only one contributor to runtime viability.**

A model whose weights consume nearly all fast memory can still fail, offload, or become unusably slow once context and runtime buffers are allocated.

**Disposition: STRONGLY REINFORCE.**

---

# 2. Context is both semantic state and physical resource

HE already treats context as scarce cognitive input. Local inference adds another dimension: context state consumes memory and affects runtime performance.

For transformer-style KV caching, more retained tokens generally require more cache state. Runtime behavior can also change with:

- context length;
- KV-cache precision/type;
- batch/ubatch settings;
- flash-attention support;
- architecture-specific recurrent or hybrid state;
- number of concurrent sequences.

### HE implication

> **Context budgets should be understood as execution budgets as well as information budgets.**

A larger advertised context window does not mean the full window is operationally sensible on a given machine.

This supports progressive disclosure from another direction: retrieving only relevant project state can improve both reasoning quality and local execution headroom.

**Disposition: REINFORCE.**

---

# 3. Headroom is a first-class reliability property

The video repeatedly recommends leaving memory free rather than loading the largest model that barely fits.

That principle is stronger than the specific VRAM tiers.

Operating margin absorbs:

- KV growth;
- runtime/compute buffers;
- multimodal projectors or auxiliary heads;
- transient allocations;
- batching/concurrency;
- backend variation;
- driver/runtime overhead.

### Candidate HE principle

> **A capacity plan with no operating margin is not a robust capacity plan.**

This applies beyond GPUs: context windows, rate limits, queues, tool-output budgets and worker pools all become brittle when designed to nominal maximum capacity.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Runtime support is part of model fitness

The video correctly emphasizes that a model file can fit and still be unusable if the runtime does not implement its architecture or required kernels.

This is especially relevant for fast-moving local models with:

- hybrid/recurrent attention;
- new MoE layouts;
- specialized projectors;
- multi-token prediction/speculative heads;
- architecture-specific quantization kernels;
- SSD/host-memory streaming behavior.

### HE implication

A local execution target should not be identified only as:

```text
model = X
```

It should be closer to:

```text
model revision
quantization/build
runtime + version
backend
context settings
hardware placement
```

A benchmark that changes any of these has changed the execution configuration.

This extends HE's existing execution-provenance finding from provider/model routing into local inference.

**Disposition: REINFORCE exact execution identity.**

---

# 5. Quantization is a workload-specific tradeoff, not a universal ladder

The video gives useful intuition that quantization can trade memory for model quality, but some of its language is too universal.

It broadly describes 4–5 bit quantization as a safe zone and very low-bit builds as falling off sharply. That can be a useful rough heuristic for many conventional quantizers, but it is not an HE rule.

A strong counterexample is the current `ISTA-DASLab/Qwen3.8-27B-GSQ-RCO-GGUF` release. Its authors report a non-uniform GSQ/RCO IQ3_S build at approximately 11.8 GB that matches their BF16 baseline on AIME25 and LiveCodeBench v6 and is close on GPQA-Diamond. Lower variants around 8.4–10.1 GB show differing tradeoffs.

Important caveat: these are the quantization authors' evaluations, not independent proof of equal capability on ACR or other real repositories.

### HE implication

> **Quantization quality must be evaluated per model, quantizer, runtime and workload.**

Do not encode fixed “bits = quality” thresholds without local task evidence.

**Disposition: REINFORCE evaluation; REJECT universal bit-count rules.**

---

# 6. Verified Ornith 1.5 9B sizes explain why an 8GB-class GPU is tight

The official `ornith-ai/Ornith-1.5-9B-GGUF` repository currently lists roughly:

- Q4_K_M: 5.78 GB
- Q5_K_M: 6.64 GB
- Q6_K: 7.56 GB
- Q8_0: 9.79 GB
- BF16: 18.4 GB

It also documents llama.cpp/local-app support.

That supports the video's claim that a 9B Q4-class model can nominally fit on an 8GB GPU. It simultaneously demonstrates the video's more important point: Q4 leaves only a relatively small gross margin before KV/cache and runtime allocations.

### ACR implication

For an 8GB-class local ACR environment, “Ornith 9B fits” is not enough to choose a context size or declare it the best reviewer.

Context length, timeout behavior, full/partial GPU residency and actual fixture accuracy must be measured together.

**Disposition: ASSESS through existing ACR calibration, not through the chart.**

---

# 7. The 27B-at-16GB story is interesting, but not a free upgrade

The video's 16GB example relies on unusually aggressive/non-uniform quantization rather than an ordinary 27B Q4 load.

The current GSQ-RCO Qwen3.8-27B release lists variants from roughly 8.4 GB through 11.8 GB plus an optional multimodal projector. The 11.8 GB IQ3_S build is the release's recommended “task-lossless” point based on the authors' benchmark suite.

This is technically interesting because it shows that smarter precision allocation can move the practical fit boundary.

But for an 8GB-class GPU, even an 11.8 GB weight file cannot reside fully in VRAM. It therefore implies CPU/system-memory placement or other offload, which changes latency and throughput materially.

### HE implication

> **A clever quant can move the storage boundary without removing the data-movement boundary.**

Benchmark the complete placement/runtime configuration, not just the compressed file size.

**Disposition: MINE; do not infer suitability for ACR.**

---

# 8. MoE active parameters are not the same as memory footprint

The video correctly warns against interpreting “active parameters per token” as the amount of model state that must be stored.

The official Qwen3-Coder-Next model card describes:

- 80B total parameters;
- 3B activated parameters;
- 256K native context;
- coding-agent/tool/recovery training.

The small active set helps compute efficiency, but the model remains an 80B-total model whose weights must be placed across available storage/memory according to runtime support.

### HE implication

Keep these dimensions separate:

```text
total stored parameters
active compute per token
resident fast-memory footprint
offloaded state
context-state footprint
actual throughput
```

**Disposition: REINFORCE measurement precision.**

---

# 9. PCIe/offload and memory topology can dominate after “fit” is solved

The video highlights an important second-order problem: moving some model state to system RAM can make a configuration fit, while data movement across PCIe becomes the actual bottleneck.

This is broadly correct, but exact impact is architecture/runtime/workload dependent.

The same caution applies to:

- multiple GPUs with different interconnects;
- unified memory versus dedicated accelerator memory;
- SSD-backed model components;
- remote/distributed inference.

### HE implication

> **Capacity and throughput are topology-dependent, not scalar-memory-dependent.**

A useful local benchmark should record enough placement information to explain why two nominally similar memory configurations behave differently.

**Disposition: REINFORCE.**

---

# 10. Separate prompt/prefill time from generation time

The video notes that offloaded or long-context configurations may be especially painful while reading a long prompt before producing output.

This suggests a better measurement model than one total wall-clock duration:

```text
load/warmup
prompt ingestion / prefill
first-token latency
generation throughput
end-to-end completion
```

For coding/review workloads, this distinction matters because many tasks are input-heavy: large diffs, standards, retrieved project context and tool transcripts may dominate prefill even when the generated review is short.

### HE implication

> **Measure the latency phase that constrains the workload, not only total response time.**

This is directly relevant to local ACR benchmarking.

**Disposition: ASSESS in ACR.**

---

# 11. DwarfStar demonstrates storage hierarchy, not magic memory elimination

The video's DeepSeek V4.1 Flash example is directionally useful but should be described carefully.

Current DwarfStar documentation describes DeepSeek V4.1 Flash's Engram conditional memory as disk-backed; ds4 keeps the large Engram table on SSD and fetches only needed rows. ds4 also documents persistent SSD KV/prefix behavior and model/runtime-specific streaming strategies.

That is an important architectural idea:

```text
hot state -> fast memory
less-frequently accessed state -> slower storage
```

But it is not a generic technique that makes arbitrary model weights free. It depends on model architecture and runtime support.

### HE implication

> **Use storage hierarchy according to access pattern, but do not generalize architecture-specific streaming into a universal inference rule.**

**Disposition: PARK for current HE/ACR needs; retain as frontier evidence.**

---

# 12. “Fits is a spec; helps is a test” maps directly to HE

This is the video's strongest line and should survive the model names around it.

A hardware/model chart can establish candidate feasibility. It cannot establish task fitness.

For local coding/review, a useful evaluation should ask:

- Did the model produce the correct result?
- Did it preserve required evidence/provenance?
- What false positives/false negatives did it introduce?
- Did it complete within the workload's time budget?
- What context was actually supplied?
- Did it remain fully resident or offload?
- Where did it fail?
- Was the run cold or warm?

### Candidate HE principle

> **Compatibility qualifies a candidate; workload evidence selects it.**

This directly strengthens `Model-Tiered Workflows & Independent Factory Assurance`.

**Disposition: STRONGLY REINFORCE.**

---

# 13. ACR-specific evidence from the current repository

ACR already has several pieces required for this style of evaluation:

- `calibration/calibrate.ts` can override `CALIBRATION_MODEL` and run committed positive/negative fixtures;
- `src/core/timingReport.ts` records total duration, per-agent attempt timing, timeout ceilings and retries;
- `src/core/llm/ollamaProvider.ts` explicitly probes runtime capabilities instead of guessing from model names;
- calibration contains clean/dirty/falsifying cases and existing timeout-related history.

This is strong groundwork.

The missing question is whether a model bake-off records enough **execution-envelope provenance** to distinguish model quality from runtime/hardware configuration.

Candidate metadata to inspect before adding anything:

```text
exact model/tag or digest
quantization
Ollama/runtime version
configured context size / KV mode
hardware/backend
GPU-resident vs CPU/offloaded placement
cold vs warm run
input/prompt token count
prefill / prompt-eval duration
output token count
generation duration
end-to-end duration
timeout stage
```

Not every field must become permanent ACR telemetry. First determine which ones are available cheaply and which actually explain observed variance.

### Recommendation

**ACR SHOULD LOOK AT THIS.**

The target is benchmark provenance and phase timing — **not** a model switch.

---

# 14. What the video does not establish

Do not preserve these as HE conclusions:

- that one exact model is best for every GPU tier;
- that 8GB universally implies Ornith 1.5 9B;
- that 16GB universally makes Qwen3.8-27B the best coding choice;
- that 4–5 bit quantization is always lossless enough;
- that sub-3-bit quantization is categorically unusable;
- that 48GB is a universal boundary for “real agents”;
- that advertised native context should be consumed in full;
- that parameter count predicts coding quality;
- that offloading is acceptable merely because it prevents OOM;
- that a larger memory number means a faster system.

These are workload-, runtime- and architecture-dependent.

---

# Overall disposition

## STRONGLY REINFORCE

- “fits” and “helps” are different questions;
- runtime memory includes more than model weights;
- context has physical execution cost;
- operating headroom is a reliability property;
- execution identity includes runtime/quant/context/placement, not just model name;
- workload evidence should select models;
- exact runtime capability is preferable to model-name inference.

## ASSESS

- ACR benchmark metadata for quantization/runtime/context/hardware placement;
- prefill vs generation timing in local ACR runs;
- controlled context-size sweeps on representative ACR fixtures;
- same-model quantization comparisons before assuming a larger model is an upgrade;
- whether offload/residency explains ACR timeout variance.

## PARK

- 48GB+ workstation/local frontier models for current ACR needs;
- multi-GPU interconnect optimization;
- SSD-streamed frontier inference as an HE component;
- a custom local-inference scheduler/router.

## REJECT

- VRAM-only model recommendations as sufficient evidence;
- “largest model that fits” as selection policy;
- universal quantization bit thresholds;
- advertised context window as an operational target;
- active-parameter count as memory-footprint proxy;
- changing ACR's model because of a hardware chart.
