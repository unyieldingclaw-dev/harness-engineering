# Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24

## Purpose

Synthesize the durable Harness Engineering implications from local-coding-model hardware research without turning HE into a GPU sizing guide or local-inference platform.

Primary evidence:

- `01 Research/Sources/Cloud Codes — Local Coding Models, VRAM Headroom & Runtime Fit — 2026-09-24.md`

Related research:

- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`
- `01 Research/Context Engineering.md.md`

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
quantization
runtime/version
context configuration
hardware/backend
memory placement/offload
topology/headroom
```

This leads to two durable rules:

> **Compatibility qualifies a candidate; workload evidence selects it.**

> **A local model configuration is an execution artifact, not just a model name.**

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

- quantization;
- runtime version;
- backend/kernel support;
- context size;
- KV precision;
- CPU/GPU placement;
- offload percentage;
- cold/warm state;
- concurrency.

### Candidate principle

> **Behavioral evidence is attributable only to the execution configuration that produced it.**

A benchmark result without enough execution identity can be impossible to reproduce and easy to misdiagnose.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Model/runtime failures can masquerade as model-quality failures

ACR already contains a concrete example of this class: its Ollama provider previously inferred thinking support from a model-name prefix, causing unsupported requests to fail with HTTP 400 and making a configuration/capability bug look like a weak model. Current code probes Ollama's reported capability instead.

The same category exists for memory and context:

```text
bad finding quality?
OR
context truncation?
OR
CPU offload slowdown?
OR
runtime unsupported feature?
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
end-to-end
```

Do not collect all phases everywhere. Add phase-level telemetry only where total-duration evidence cannot distinguish competing explanations.

### Candidate principle

> **Instrument only deeply enough to discriminate the failure mode under investigation.**

**Disposition: ASSESS for ACR local-model bake-offs.**

---

# 6. Quantization belongs inside the model-evaluation identity

Quantization is not merely a deployment afterthought. It can materially alter capability, latency and memory headroom.

Therefore:

```text
Qwen-X @ quant A
```

and

```text
Qwen-X @ quant B
```

should be treated as different candidate execution configurations when quality is under evaluation.

HE should reject universal assumptions such as “4-bit is safe” or “below 3-bit is unusable.” Modern architecture-aware/non-uniform quantizers can produce exceptions, and only workload evidence answers whether those exceptions matter to the project.

**Disposition: REINFORCE.**

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
- quantization;
- Ollama/runtime version;
- configured context;
- CPU/GPU residency or offload;
- cold/warm status;
- prompt-eval/prefill time;
- generation time;
- token counts;
- timeout stage.

Do not add every field by default. First identify the fields needed to explain observed ACR differences.

Potential controlled experiment after inspection:

```text
same fixtures
same model family
same machine
vary one of:
  context size
  quantization
        ↓
measure
  clean false positives
  dirty recall
  evidence quality
  timeout rate
  prefill/generation/end-to-end time
```

This is more informative than changing model and quantization/context simultaneously.

**Disposition: ASSESS.**

---

# 10. Relationship to model-tiered workflows

`Model-Tiered Workflows & Independent Factory Assurance` currently says:

> Route by demonstrated workload fitness, not by model reputation or stage label.

Local inference refines that statement:

> **Route by demonstrated workload fitness of the exact execution configuration, not just the named model.**

For cloud models, exact execution identity may be partly opaque. For local models, more of it is observable and therefore should be controlled when comparisons matter.

This is an extension of the existing principle, not a new routing architecture.

---

# Consolidated disposition

## STRONGLY REINFORCE

- compatibility is not task fitness;
- leave operating headroom;
- execution provenance must include material runtime/configuration details;
- context has semantic and physical cost;
- diagnose runtime/configuration failure before blaming model capability;
- benchmark exact configurations, not marketing labels.

## ASSESS

- minimum local-inference provenance needed for ACR model evaluation;
- phase-level timing when ACR timeouts/variance cannot otherwise be explained;
- controlled context and quantization sweeps using existing ACR fixtures;
- whether residency/offload data materially predicts timeout or quality behavior.

## PARK

- local frontier inference infrastructure;
- custom hardware-aware model routing;
- multi-GPU/SSD-streaming optimization;
- generalized hardware telemetry in HE.

## REJECT

- biggest-model-that-fits selection;
- VRAM-only sizing as evidence of usefulness;
- universal quantization thresholds;
- full advertised context by default;
- active-parameter count as storage footprint;
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
+ exact model build
+ runtime
+ context
+ placement/headroom
        ↓
measured correctness + latency + failure behavior
```

That keeps hardware optimization subordinate to task success instead of letting “it loads” become architecture evidence.
