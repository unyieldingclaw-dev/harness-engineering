# Coding Horizon — Local AI Total Cost, Ownership & Hybrid Routing — 2026-09-28

## Purpose

Deep-dive Coding Horizon's video **“The Truth About The Hidden Cost Of Local AI”** and mine the durable Harness Engineering implications without turning HE into a hardware-buying guide.

Primary user source:

- Video: https://www.youtube.com/watch?v=8DcNySDOOdw
- Creator: Coding Horizon
- Transcript supplied by the user on 2026-09-28

Primary technical references checked:

- NVIDIA RTX 5090 specifications — https://www.nvidia.com/en-us/geforce/graphics-cards/50-series/rtx-5090/
- AMD Ryzen AI Max+ 395 specifications — https://www.amd.com/en/products/processors/laptop/ryzen/ai-300-series/amd-ryzen-ai-max-plus-395.html
- AMD GPT-OSS 120B / Ryzen AI Max+ guidance — https://www.amd.com/en/blogs/2025/how-to-run-openai-gpt-oss-20b-120b-models-on-amd-ryzen-ai-radeon.html
- AMD Variable Graphics Memory / local model guidance — https://www.amd.com/en/blogs/2025/faqs-amd-variable-graphics-memory-vram-ai-model-sizes-quantization-mcp-more.html
- Apple Mac Studio configuration/pricing page — https://www.apple.com/us/shop/buy-mac/mac-studio/

Related HE research:

- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`

This note is research only. It does not authorize a hardware purchase or local-first architecture.

---

# Executive finding

The video's durable contribution is not any specific GPU, Mac, AMD system or payback period.

It is a better accounting boundary for local inference:

```text
local inference cost
=
hardware acquisition attributable to AI
+ incremental electricity
+ setup / tuning / maintenance time
+ storage / upgrades / repairs
+ model/runtime operational friction
- residual / shared value from hardware used for other work
```

and a separate value side:

```text
local value
=
privacy/control
+ offline availability
+ predictable marginal inference cost
+ customization
+ independence from provider limits
+ other non-AI value of the machine
```

The decision should be made against the **actual alternative being displaced**, not a generic “cloud” number.

### Candidate principle

> **Compare total incremental cost and value of the actual alternatives for the target workload, not sticker prices or per-token slogans.**

---

# 1. “No per-prompt bill” is not “free”

The video correctly distinguishes marginal API charges from ownership cost.

A local model can answer without a per-request provider fee while still requiring:

- capital hardware;
- electricity;
- system RAM / storage;
- setup and tuning;
- driver/runtime maintenance;
- hardware replacement or repair;
- operator time.

### HE implication

> **Cost claims need an explicit accounting boundary.**

“Free,” “local,” “$0/token,” and “already paid for” are different claims.

**Disposition: STRONGLY REINFORCE economic evidence semantics.**

---

# 2. The hardware numbers used as examples are directionally grounded

The video uses several concrete examples.

## RTX 5090

NVIDIA lists:

- 32 GB dedicated VRAM;
- 575 W Total Graphics Power for the reference card;
- substantial whole-system power/cooling requirements.

The video's illustrative electricity calculation is arithmetically sound:

```text
0.575 kW × 4 h/day × 30 days × $0.18/kWh
≈ $12.42/month
```

The video correctly labels that as a ceiling-shaped illustration for the card, not a measured whole-system inference bill.

## Ryzen AI Max+ 395

AMD lists up to 128 GB system memory and documents configurations exposing up to roughly 96 GB as graphics-addressable memory. AMD has demonstrated GPT-OSS 120B in an approximately 61 GB model representation with reported throughput up to roughly 30 tok/s under a specific software/driver/test configuration.

## Mac Studio

Apple lists the M4 Max Mac Studio base configuration at $1,999 with 36 GB unified memory and allows much larger unified-memory configurations at higher tiers, including up to 512 GB on M3 Ultra.

### HE treatment

These are **timestamped examples**, not durable purchase recommendations.

**Disposition: RETAIN method; PARK exact product economics.**

---

# 3. Model download size is not the memory purchase requirement

The video repeatedly makes the same distinction HE has already mined:

```text
weights on disk
!=
resident execution working set
```

Useful local execution also needs room for:

- KV/context state;
- runtime buffers;
- OS/apps;
- vision/projector state where applicable;
- concurrency;
- accelerator/CPU transfer behavior.

Likewise, CPU/system-RAM offload can expand what loads without making that memory equivalent to accelerator memory in latency/bandwidth.

### HE implication

This is **corroborating evidence** for the existing execution-envelope model.

**Disposition: REINFORCE.**

---

# 4. Acquisition cost and incremental cost are different cases

The video makes a particularly useful accounting distinction.

## Case A — hardware already owned for independent reasons

If the machine already exists for work, gaming, photography, development, etc., charging the entire historic purchase price to local AI can be misleading.

Relevant incremental costs may instead include:

- electricity;
- model storage;
- AI-specific upgrades;
- additional wear/maintenance if material;
- setup/operation time.

## Case B — hardware purchased primarily to obtain local AI

Then acquisition cost belongs in the comparison.

### Candidate principle

> **Economic evidence should distinguish sunk/shared infrastructure from new cost incurred because of the AI decision.**

This is not permission to call hardware “free” merely because it has multiple uses. Allocate cost according to the decision being evaluated.

**Disposition: NEW / STRONGLY MINE.**

---

# 5. The alternative being displaced must be comparable

The video's $20/month subscription arithmetic is useful as an illustration but is easy to misuse.

A consumer subscription, API usage, local open-weight model and hosted coding-agent product may differ in:

- model capability;
- tools;
- context;
- rate limits;
- privacy;
- reliability;
- maintenance burden;
- integrated features;
- licensing/commercial rights.

Therefore:

```text
$2,000 local computer
vs
$20 subscription
```

is not automatically an apples-to-apples inference-cost comparison.

### Candidate principle

> **Break-even arithmetic is meaningful only when the compared alternatives satisfy the same required workload and service boundary.**

**Disposition: STRONGLY REINFORCE.**

---

# 6. Setup and maintenance are real operational costs

The video calls out an often-ignored cost: operator time.

Local inference may require selecting and maintaining:

- model artifact / quantization;
- runtime;
- drivers;
- context settings;
- device placement;
- backend compatibility;
- tool/client integration.

For an enthusiast this may be acceptable or enjoyable. For a production workflow it is operational work.

### HE implication

> **Operational complexity belongs in the cost model when it consumes engineering time or creates reliability risk.**

HE should not attempt to assign a universal dollar value to that time. It should make the cost boundary visible when it changes the decision.

**Disposition: STRONGLY REINFORCE exit-cost / maintainability research.**

---

# 7. Privacy is a path property, not a “local model” label

The video correctly notes that a local model connected to cloud plugins/services can still transmit data externally.

### Candidate principle

> **Local inference is not a privacy guarantee; verify the full data path.**

Relevant boundaries can include:

- model runtime;
- UI/client telemetry;
- MCP/tool servers;
- web/search integrations;
- crash reporting;
- remote vector stores;
- external APIs.

Do not assume or inventory all of these universally. Inspect the path when privacy is part of the requirement.

**Disposition: NEW / STRONGLY MINE.**

---

# 8. Offline availability and control are real non-price benefits

A local model can remain available during:

- internet outage;
- travel/airplane use;
- provider outage;
- restricted/offline environment;
- provider quota exhaustion.

Those benefits may justify local capability even when strict dollar break-even is unfavorable.

### HE implication

Cost optimization should not collapse the decision into money alone.

A useful comparison may include:

```text
cost
capability
latency
privacy/control
availability
maintenance burden
provider dependence
```

**Disposition: REINFORCE multi-objective workload fitness.**

---

# 9. Hybrid routing is often more rational than “all local”

The video suggests using smaller local models for narrow/private work and stronger hosted models when the task actually needs more reasoning or capability.

That fits HE's existing model-tiered direction, with an important caveat:

> The routing boundary must be demonstrated by workload evidence, not a generic model-size ladder.

Possible examples:

```text
local narrow transform / extraction / repetitive draft
        ↓ if verified sufficient
keep local

hard semantic reasoning / broad agentic work
        ↓ if local candidate fails eval
hosted stronger configuration
```

### Candidate principle

> **Do not force architectural purity when a mixed execution portfolio better satisfies capability, privacy, cost and operational constraints.**

**Disposition: REINFORCE; no generic router implied.**

---

# 10. “The best value is the machine you already have” is a good heuristic, not a universal rule

The closing message is sensible for experimentation:

- test the hardware already owned;
- test a model sized to the real task;
- measure whether it helps;
- upgrade only when a measured constraint blocks the desired workload.

This aligns with HE's anti-speculation posture.

### Stronger HE formulation

> **Prefer evidence from the current execution envelope before purchasing capacity to solve a hypothetical bottleneck.**

That does not imply never buying hardware. It requires a demonstrated workload need.

**Disposition: STRONGLY REINFORCE.**

---

# 11. Economic evidence should attach to verified task completion

Tokens, electricity, hardware cost and subscription price are inputs.

The useful denominator is successful work.

A cheap local model that needs repeated retries or fails the task can be more expensive than a higher-priced hosted model that completes it reliably.

Conversely, a modest local model that handles a high-volume narrow task reliably can have excellent economics.

### Candidate principle

> **Compare cost per accepted/verified outcome when model quality materially changes retries or completion.**

This extends the existing HE rule to completed-task economics.

**Disposition: REINFORCE.**

---

# Consolidated HE mining

## STRONGLY MINE / REINFORCE

- cost claims need explicit accounting boundaries;
- distinguish acquisition cost from incremental cost on already-owned hardware;
- compare alternatives that satisfy the same workload/service requirement;
- include material setup and operational effort;
- local privacy requires verifying the complete data path;
- offline/control value can matter independently of break-even dollars;
- use existing hardware evidence before buying for a hypothetical workload;
- compare economics against verified task completion, not isolated token price.

## PARK

- fixed RTX/Mac/AMD purchasing recommendations;
- fixed electricity assumptions;
- fixed break-even periods;
- fixed model-to-hardware shopping tables.

## REJECT

- “no API bill” = free;
- “already own a computer” = zero local cost;
- “local model” = private by definition;
- a consumer subscription and a local model as automatically equivalent alternatives;
- buying hardware because a larger model exists rather than because a current workload requires it.

---

# Bottom line

The durable local-AI cost model is:

> **Evaluate the exact workload, the exact execution configuration, the real alternative being displaced, the incremental acquisition/operating/maintenance costs, and the non-price value such as privacy and offline control; then compare cost per verified useful outcome.**

HE should keep the method and let current hardware/pricing numbers expire.
