# Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24

## Purpose

Synthesize the durable Harness Engineering implications from local-coding-model hardware research without turning HE into a GPU sizing guide or local-inference platform.

Primary evidence:

- `01 Research/Sources/Cloud Codes — Local Coding Models, VRAM Headroom & Runtime Fit — 2026-09-24.md`
- `01 Research/Sources/Kai — Local Coding AI, KV Cache & Runtime Envelope — 2026-09-26.md`

Related research:

- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`
- `01 Research/Context Engineering.md.md`
- `01 Research/Sources/Qwen3.8 27B — Harness Comparison.md`

This is research only. It does not authorize model changes, hardware purchases, ACR implementation changes, or a new routing subsystem.

---

# Executive synthesis

HE's model-selection work needs one additional dimension for local inference:

```text
workload class
+ required judgment
+ verification strength
+ consequence / reversibility
+ model capability
+ execution envelope
        ↓
measured workload fitness
```

For local models, the **execution envelope** includes:

```text
model/checkpoint
quantization/build
artifact provenance when material
runtime/version
context configuration
active runtime features
hardware/backend
memory placement/offload
topology/headroom
```

This leads to three durable rules:

> **Compatibility qualifies a candidate; workload evidence selects it.**

> **A local model configuration is an execution artifact, not just a model name.**

> **A declared capability is not evidence that the runtime activated it or that it improved the workload.**

---

# 1. Capacity is an envelope, not a scalar

A statement such as `8 GB GPU` or `model = 5.8 GB` is insufficient to determine usable capacity.

Local inference competes for memory across model state, context/KV state, compute buffers and backend/runtime allocations. The exact categories vary by architecture, but the general engineering consequence is stable:

> **Nominal fit without operating margin is brittle.**

This is analogous to other HE-managed constraints:

- context-window occupancy;
- rate-limit headroom;
- queue capacity;
- tool-output budgets;
- worker concurrency.

Systems designed to the theoretical maximum often fail first under variable workload.

**Disposition: REINFORCE.**

---

# 2. Context engineering and runtime engineering meet at local inference

Context is not only what the model reasons over. In local runtimes it can also change memory use, prompt-processing time and offload behavior.

This creates a useful bridge between HE's context principles and runtime concerns:

```text
retrieve only what is needed
        ↓
less semantic noise
+ lower active context pressure
+ potentially lower local memory/prefill cost
```

This does not justify starving a model of evidence to save memory. The optimization objective remains task correctness.

### Candidate principle

> **Context reduction is beneficial only when it removes non-required state without weakening the evidence needed for the task.**

**Disposition: REINFORCE existing context-quality guidance.**

---

# 3. Execution provenance must extend below the model name

Cloud/provider research already showed that a nominal model name can hide different execution paths. Local inference has an analogous problem.

Two runs against “the same model” may differ materially because of:

- checkpoint/revision;
- quantization method and build;
- quantizer/uploader or artifact digest where third-party builds matter;
- runtime version;
- backend/kernel support;
- context allocation;
- KV precision;
- CPU/GPU placement;
- offload percentage;
- reasoning policy;
- speculative/MTP configuration;
- cold/warm state;
- concurrency;
- chat template and harness/tool surface.

### Candidate principle

> **Behavioral evidence is attributable only to the execution configuration that produced it.**

A benchmark result without enough execution identity can be impossible to reproduce and easy to misdiagnose.

Do not collect every possible field universally. Record the minimum identity needed to reproduce the result and explain material variance.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Model/runtime failures can masquerade as model-quality failures

ACR already contains a concrete example of this class: its Ollama provider previously inferred thinking support from a model-name prefix, causing unsupported requests to fail with HTTP 400 and making a configuration/capability bug look like a weak model. Current code probes Ollama's reported capability instead.

The same category exists for memory, context and runtime features:

```text
bad finding quality?
OR
context truncation?
OR
CPU offload slowdown?
OR
runtime unsupported feature?
OR
feature present but inactive?
OR
request timeout during prefill?
OR
actual model weakness?
```

### Candidate principle

> **Before attributing a failure to model intelligence, rule out execution-envelope failure.**

This is particularly important in comparative model evaluation.

**Disposition: STRONGLY REINFORCE.**

---

# 5. Latency should be decomposed by phase when it matters

A single end-to-end duration can conceal the actual bottleneck.

For input-heavy coding/review workloads, useful phases may include:

```text
load / warmup
prompt ingestion / prefill
first-token latency
generation
verification/tool time
end-to-end verified task
```

Long-context research strengthens this point: a runtime or quantization can improve decode while hurting prompt processing, so “tokens/sec” without a phase label can be misleading.

Do not collect all phases everywhere. Add phase-level telemetry only where total-duration evidence cannot distinguish competing explanations.

### Candidate principle

> **Instrument only deeply enough to discriminate the failure mode under investigation.**

**Disposition: ASSESS for ACR local-model bake-offs.**

---

# 6. Quantization belongs inside the model-evaluation identity

Quantization is not merely a deployment afterthought. It can materially alter capability, latency and memory headroom.

Therefore:

```text
Qwen-X @ quant/build A
```

and

```text
Qwen-X @ quant/build B
```

should be treated as different candidate execution configurations when quality is under evaluation.

Recent Qwen3.8 serving evidence goes further: two builds with similar high-level precision labels can have different weight footprints, available KV pools and speculative/MTP behavior. Nominal bit depth is therefore not a sufficient artifact identity.

HE should reject universal assumptions such as “4-bit is safe” or “below 3-bit is unusable.” Modern architecture-aware/non-uniform quantizers can produce exceptions, and only workload evidence answers whether those exceptions matter to the project.

### Candidate principle

> **A quantized model file is an execution artifact; provenance can be part of correctness evidence.**

**Disposition: STRONGLY REINFORCE.**

---

# 7. Sparse compute and storage footprint must not be conflated

MoE and conditional-memory architectures add several independent dimensions:

- total parameters stored;
- parameters activated per token;
- resident fast-memory state;
- offloaded state;
- conditional/disk-backed state;
- context-state footprint.

### Candidate principle

> **A compute-sparsity number is not a memory-capacity number.**

The same general lesson applies to any compressed headline metric: identify which resource it actually measures before using it for architecture decisions.

**Disposition: REINFORCE measurement semantics.**

---

# 8. Hardware topology is part of the execution path

Memory amount alone cannot describe throughput when the runtime moves state across:

- PCIe;
- multiple GPUs;
- unified memory;
- networked accelerators;
- SSD-backed conditional memory.

### HE implication

When topology affects a controlled evaluation, record enough information to explain it. Do not build a general hardware inventory system unless repeated evaluations need one.

**Disposition: ASSESS minimal provenance; REJECT infrastructure for its own sake.**

---

# 9. ACR is the natural place to test this, but only as measurement

ACR's current calibration harness already supports model overrides and has positive, negative and clean fixtures. Its timing system records per-agent attempt time, timeout ceilings, retries and total run duration.

The next useful question is not “Which bigger local model should ACR use?”

It is:

> **Can ACR distinguish model-quality variance from execution-envelope variance in its local benchmark results?**

A bounded research pass should inspect whether existing calibration artifacts already expose or can cheaply obtain:

- exact model tag/digest;
- quantization/build identity;
- Ollama/runtime version;
- actual configured/allocated context;
- KV mode/precision if relevant;
- CPU/GPU residency or offload;
- reasoning and speculative/MTP settings if used;
- cold/warm status;
- prompt-eval/prefill time;
- generation time;
- token counts;
- timeout stage.

Do not add every field by default. First identify the fields needed to explain observed ACR differences.

Potential controlled experiment after inspection:

```text
same fixtures
same evaluator
same machine
vary one material execution variable
        ↓
measure
  clean false positives
  dirty recall
  unsupported/fabricated findings
  evidence quality
  timeout rate
  prefill/generation/end-to-end time
```

Possible candidate arms may include a specifically identified Qwen3.6-35B-A3B configuration or an aggressive Qwen3.8-27B quant, but only after proving that the configuration fits with useful context/headroom. Their existence is not evidence that either should become the ACR default.

**Disposition: ASSESS in a dedicated ACR session.**

---

# 10. Relationship to model-tiered workflows

`Model-Tiered Workflows & Independent Factory Assurance` currently says:

> Route by demonstrated workload fitness, not by model reputation or stage label.

Local inference refines that statement:

> **Route by demonstrated workload fitness of the exact execution configuration, not just the named model.**

For cloud models, exact execution identity may be partly opaque. For local models, more of it is observable and therefore should be controlled when comparisons matter.

This is an extension of the existing principle, not a new routing architecture.

---

# 11. Context has three distinct identities

The Kai deep pass exposes a useful distinction that should be explicit in HE:

```text
model-supported / advertised context
        ↓
runtime-allocated context
        ↓
effective task context after harness overhead
```

These are not interchangeable.

Current Ollama documentation, for example, assigns context defaults according to VRAM and recommends a larger allocation for agents/coding workloads. A model may advertise hundreds of thousands of tokens while the runtime allocates only a small fraction unless configured otherwise.

Effective task context is smaller again because system instructions, tool schemas, Skills, history, retrieved files and tool output occupy the same window.

### Candidate principle

> **When context matters to an evaluation, record the allocated context and distinguish it from both the model maximum and the evidence actually available to the task.**

This also means harness/context overhead can be a physical local-inference cost, not just a semantic-noise concern.

**Disposition: NEW / STRONGLY MINE.**

---

# 12. Growing-state topology belongs in capacity reasoning

Two models with similar parameter counts can have very different long-context memory growth.

Qwen3.8-27B provides a concrete example: its 64-layer language stack uses conventional full attention only once every four layers, so only 16 layers accumulate ordinary full-attention KV state. Devstral Small 2 uses conventional cached attention across all 40 text layers.

Using their published KV-head geometry, the raw BF16/FP16 KV estimate at 131,072 tokens is roughly:

```text
Qwen3.8-27B      ≈ 8.0 GiB
Devstral Small 2 ≈ 20.0 GiB
```

before runtime overhead and implementation-specific choices.

The exact numbers are illustrative; the durable point is architectural.

### Candidate principle

> **Capacity depends on which state grows with sequence length, not only how many parameters the model has.**

HE does not need an architecture catalog. Record this detail only when it materially explains a local workload constraint.

**Disposition: NEW / STRONGLY MINE.**

---

# 13. Runtime feature support needs an evidence ladder

A model can contain a feature without the selected runtime using it correctly or beneficially.

Qwen3.8 contains an MTP head, but the vLLM recipe requires explicit speculative-decoding configuration and records acceptance from runtime metrics. The recipe explicitly warns against inferring a working drafter from throughput alone.

That yields a reusable evidence ladder:

```text
model declares capability
        ↓
runtime supports capability
        ↓
capability is configured and active
        ↓
runtime effect is measurable
        ↓
target workload improves
```

### Candidate principle

> **Capability declaration is not activation evidence, and activation is not benefit evidence.**

This applies beyond MTP to prefix caching, KV compression, tool support, reasoning controls and other harness/runtime features.

**Disposition: NEW / STRONGLY MINE.**

---

# 14. Optimize completed-task economics, not isolated speed

A lower reasoning level, faster decode path or smaller quant may reduce the cost of one turn while increasing retries, weak findings or verification failures.

Likewise, a runtime that generates quickly can still be poor for repository work if long prompt ingestion dominates latency.

The durable optimization target is therefore the verified task outcome:

```text
correctness / evidence quality / completion
against
total time + tokens + retries + failures + cost
```

not any one of:

- generation tokens/sec;
- time to first response;
- tokens per turn;
- nominal reasoning level.

### Candidate principle

> **Tune model/runtime settings against verified completed-task outcomes on the target workload.**

**Disposition: REINFORCE.**

---

# 15. Economic thresholds are timestamped evidence, not architecture

Provider pricing, hosted-model aliases, GPU street prices and runtime efficiency can change quickly.

A rent-versus-own calculation can support a current purchase decision, but a fixed break-even threshold should not become durable HE policy.

### Candidate principle

> **When economics influence an architecture or routing decision, bind the evidence to the date, provider/model route, usage assumption and relevant privacy/compliance constraints.**

Retain the method; let the numbers expire.

**Disposition: NEW / RETAIN METHOD, PARK NUMBERS.**

---

# Consolidated disposition

## STRONGLY REINFORCE / MINE

- compatibility is not task fitness;
- leave operating headroom;
- execution provenance must include material runtime/configuration/artifact details;
- context has semantic and physical cost;
- distinguish model-supported, runtime-allocated and effective task context;
- architecture-specific growing state can dominate long-context capacity;
- diagnose runtime/configuration failure before blaming model capability;
- benchmark exact configurations, not marketing labels;
- capability declaration must be followed by activation/effect evidence;
- prefill and decode are distinct performance dimensions;
- completed-task outcomes outrank isolated throughput.

## ASSESS

- minimum local-inference provenance needed for ACR model evaluation;
- actual allocated Ollama context for current ACR local runs;
- phase-level timing when ACR timeouts/variance cannot otherwise be explained;
- controlled context and quantization sweeps using existing ACR fixtures;
- whether residency/offload data materially predicts timeout or quality behavior;
- whether one specifically identified Qwen3.6-35B-A3B or Qwen3.8 quant deserves a bounded ACR benchmark arm.

## PARK

- local frontier inference infrastructure;
- custom hardware-aware model routing;
- multi-GPU/SSD-streaming optimization;
- generalized hardware telemetry in HE;
- fixed GPU-tier recommendations;
- fixed rent-versus-buy break-even numbers;
- unpinned community throughput and low-bit score claims.

## REJECT

- biggest-model-that-fits selection;
- VRAM-only sizing as evidence of usefulness;
- universal quantization thresholds;
- full advertised context by default;
- model-supported context as evidence of runtime allocation;
- active-parameter count as storage footprint;
- MTP/speculative support as proof of acceleration;
- universal reasoning-effort defaults;
- changing ACR defaults from a hardware/model chart alone.

---

# Bottom line

HE should treat a local inference configuration the same way it treats any other consequential execution environment: as a versioned, measurable envelope whose behavior must be demonstrated on the workload.

The useful equation is not:

```text
VRAM -> model
```

It is:

```text
workload
+ required evidence
+ exact model artifact
+ runtime
+ allocated/effective context
+ active runtime features
+ placement/headroom
        ↓
measured correctness + latency + failure behavior
```

That keeps hardware optimization subordinate to task success instead of letting “it loads” or “the feature exists” become architecture evidence.
