# Kai — Local Coding AI, KV Cache & Runtime Envelope — 2026-09-26

## Purpose

Deep-dive Kai's video **“Best Local Coding AI for Your GPU (4GB to 512GB)”** and mine only the durable Harness Engineering implications.

Primary user source:

- Video: https://www.youtube.com/watch?v=C9M6iUFtVB4
- Creator: Kai
- Transcript supplied by the user on 2026-09-26

Primary / implementation evidence checked during review:

- Qwen3.8-27B official model card: https://huggingface.co/Qwen/Qwen3.8-27B
- Qwen3.6-35B-A3B official model card/config: https://huggingface.co/Qwen/Qwen3.6-35B-A3B
- Devstral Small 2 official config: https://huggingface.co/mistralai/Devstral-Small-2-24B-Instruct-2512/blob/main/config.json
- Ollama context-length documentation: https://github.com/ollama/ollama/blob/main/docs/context-length.mdx
- vLLM Qwen3.8-27B serving recipe: https://github.com/vllm-project/recipes/blob/main/models/Qwen/Qwen3.8-27B.yaml
- GSQ/RCO Qwen3.8 quant evidence: https://huggingface.co/npario/Qwen3.8-27B-GSQ-RCO-GGUF
- Example IQ2_XS artifact with explicit provenance: https://huggingface.co/sammyblues/Qwen3.8-27B-IQ2_XS-GGUF

Related HE research:

- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`
- `01 Research/Sources/Cloud Codes — Local Coding Models, VRAM Headroom & Runtime Fit — 2026-09-24.md`
- `01 Research/Sources/Qwen3.8 27B — Harness Comparison.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Context Engineering.md.md`

This is research evidence. It does **not** authorize changing ACR's default model, buying hardware, adding a hardware-aware router, or treating the video's GPU tiers as policy.

---

# Executive finding

Kai's durable contribution is not the claim that one model is “best” from 12 GB through 48 GB.

The important engineering point is:

> **Agent runtime fitness depends on the complete execution envelope, and context capacity is architecture- and runtime-dependent rather than a simple function of parameter count or model-file size.**

The deep pass found four especially useful refinements for HE:

1. **Growing-state topology matters.** Two similarly sized models can have radically different context-memory growth because only some layers may accumulate conventional KV state.
2. **Context has three identities:** advertised/model-supported context, runtime-allocated context, and effective task context after harness/tool/system overhead.
3. **Feature declaration is not feature evidence.** A model may contain an MTP/speculative head while the selected backend requires explicit configuration or may implement it poorly. Verify activation/effect directly.
4. **Quantized artifact identity matters below the model name.** Quant recipe, uploader/build, artifact digest, runtime and settings can materially change capacity and quality.

These refine HE's existing local-inference work. They do not justify a new subsystem.

---

# 1. The KV-cache thesis is technically sound

The video's strongest specific claim compares Qwen3.8-27B with Devstral Small 2 at long context.

The official Qwen3.8-27B model card reports:

- 27B parameters;
- 64 language-model layers;
- a repeating layout of `3 × Gated DeltaNet` then `1 × Gated Attention`;
- therefore 16 conventional full-attention layers out of 64;
- 4 KV heads in those full-attention layers;
- head dimension 256;
- native context 262,144 tokens.

The official Devstral Small 2 config reports:

- 40 text layers;
- 8 KV heads;
- head dimension 128;
- caching enabled across the text stack;
- no sliding window.

Using the ordinary FP16/BF16 KV approximation:

```text
KV bytes ≈ tokens
         × cache-growing layers
         × 2               # key + value
         × KV heads
         × head dimension
         × bytes/element
```

At 131,072 tokens:

```text
Qwen3.8-27B
131072 × 16 × 2 × 4 × 256 × 2
≈ 8.0 GiB

Devstral Small 2
131072 × 40 × 2 × 8 × 128 × 2
≈ 20.0 GiB
≈ 21.5 decimal GB
```

Runtime allocator overhead, cache precision and backend behavior can change actual observed allocations, but the order-of-magnitude difference in the video is supported by the published architectures.

### HE implication

> **Parameter count does not determine context-memory growth. Record the state-growth topology that materially constrains the workload.**

HE does not need a generic transformer calculator. The practical rule is simply to avoid assuming that similarly sized models have similarly sized long-context envelopes.

**Disposition: STRONGLY REINFORCE.**

---

# 2. Advertised context, allocated context and effective task context are different things

The video correctly calls out an especially important Ollama behavior.

Current Ollama documentation states that default context allocation depends on VRAM:

- `< 24 GiB`: 4K
- `24–48 GiB`: 32K
- `>= 48 GiB`: 256K

The same documentation says web search, agents and coding tools should use at least 64K context and shows `ollama ps` as the way to verify actual context allocation and CPU/GPU placement.

That creates three distinct numbers:

```text
model-supported context
        !=
runtime-allocated context
        !=
effective task context
```

The third is smaller again because the allocated window contains:

- system/harness instructions;
- project instructions;
- tool schemas;
- Skill/reference material;
- conversation history;
- retrieved files;
- tool output;
- task prompt and generated state.

The video's anecdotal claim that a few MCP connections can consume several thousand tokens is not preserved as a fact here; exact overhead should be measured from the actual harness.

### HE implication

> **Context claims must name which context they mean.**

For agent evaluation, record at least the configured/allocated context and avoid treating the model's published maximum as the operational context.

**Disposition: STRONGLY REINFORCE.**

---

# 3. Harness context tax is a physical runtime cost for local inference

HE already treats unnecessary context as semantic noise. Local inference makes that cost physical as well.

Every unnecessary always-loaded instruction, tool schema or retained transcript competes with repository evidence for:

- context occupancy;
- KV/cache state;
- prefill work;
- memory headroom;
- sometimes CPU/GPU offload behavior.

This is another reason to prefer progressive disclosure and lean tool surfaces, but it does **not** justify starving the model of required evidence.

### Candidate HE rule

> **Measure harness context tax where it is material; reduce only state that is not required for correctness.**

Do not optimize token count as a goal independent of task success.

**Disposition: REINFORCE context engineering.**

---

# 4. “The model supports MTP” is not enough

Qwen3.8-27B officially contains a trained Multi-Token Prediction head.

But the vLLM serving recipe makes the operational distinction explicit:

- speculative decoding is opt-in/configured behavior;
- the runtime needs specific arguments for the Qwen MTP head;
- support depends on runtime/version/backend;
- vLLM's verified hardware entries read **MTP acceptance from runtime metrics** rather than inferring that it works from throughput;
- the recipe explicitly warns that throughput alone cannot distinguish a useful drafter from one that loaded but was ignored.

The same recipe reports materially different measured MTP acceptance across precisions/builds and different available KV-token pools across variants.

Community measurements also show that MTP can be beneficial under one runtime path and net-negative under another. Those measurements are useful warnings, not universal performance claims.

### Candidate HE rule

> **Capability declaration is not activation evidence, and activation is not benefit evidence.**

For runtime optimizations such as MTP/speculative decoding, prefix caching or KV compression:

```text
model supports feature
        ↓
runtime supports feature
        ↓
feature configured/active
        ↓
feature produces intended effect
        ↓
workload improves
```

Each step can fail independently.

This generalizes beyond local inference to any harness/runtime capability.

**Disposition: NEW / STRONGLY MINE.**

---

# 5. Quantization provenance belongs in the execution identity

Kai is directionally right that “2-bit Qwen3.8” is not a sufficiently precise model identity.

Current public artifacts illustrate why.

The GSQ/RCO Qwen3.8 release publishes multiple mixed/non-uniform precision builds with different file sizes and benchmark behavior. At matched nominal low-bit ranges, different quantization methods produce materially different measured results.

The vLLM Qwen3.8 recipe gives another concrete example: separate NVFP4 builds are not interchangeable. Their weight footprint, available KV pool and observed MTP acceptance differ even though a casual inventory might label both simply “Qwen3.8 NVFP4.”

An independently published IQ2_XS artifact goes further and records:

- source model;
- exact quantization;
- SHA-256;
- retained layer count;
- llama.cpp revision;
- conversion host;
- fit-test context;
- measured peak allocation;
- benchmark configuration.

That is the right direction for reproducible local-model evidence.

### Candidate execution identity

When a local benchmark matters, the useful identity can include:

```text
base model
checkpoint/revision
quantization method/build
artifact producer/uploader
artifact digest
runtime + version
backend
context allocation
KV-cache type
GPU/CPU placement/offload
reasoning policy
speculative/MTP configuration
chat template
harness/tool surface
```

Not every experiment needs every field. Record the minimum set needed to make the result reproducible and to explain material variance.

### HE implication

> **A quantized model file is an execution artifact, not merely a compressed copy of a model name.**

**Disposition: STRONGLY REINFORCE execution provenance.**

---

# 6. The exact “2-bit A scored 16/20, B scored 7/20” claim is not strong enough to canonize

The transcript cites a same-model bug-fix comparison where two 2-bit uploads allegedly scored 16/20 and 7/20.

The deep pass found strong public evidence that Qwen3.8 low-bit artifacts can diverge materially, including:

- GSQ/RCO versus other matched-size quantizations;
- a published IQ2_XS artifact showing measurable perplexity/task degradation relative to BF16;
- different vLLM quant builds with different memory and MTP characteristics.

However, the exact 16/20 versus 7/20 bug-fix comparison was not pinned to sufficiently strong, reproducible primary evidence during this pass.

### HE treatment

Do not preserve the exact score pair as fact.

Preserve the stronger, independently supported principle:

> **Nominal bit depth does not uniquely identify quantization behavior.**

**Disposition: CLAIM PARKED; PRINCIPLE RETAINED.**

---

# 7. Qwen3.6-35B-A3B is a legitimate low-VRAM research candidate, not an HE recommendation

The official Qwen3.6-35B-A3B model card reports:

- 35B total parameters;
- 3B activated parameters;
- 40 layers;
- repeating hybrid linear/full-attention layout;
- native 262K context;
- MTP training;
- coding-agent benchmark results from the model publisher.

That architecture explains why an MoE model can offer much lower active compute per token than total parameter count suggests while much of its stored state remains outside fast memory.

The video's specific GTX 1070 / ~26 tok/s report remains a community hardware observation and should not become HE doctrine.

### HE implication

> **Sparse active compute and total storage/placement are separate dimensions.**

For ACR, Qwen3.6-35B-A3B may justify a bounded benchmark candidate if the exact runtime configuration fits the machine. It does not justify replacing a current baseline from model-card or anecdotal throughput evidence.

**Disposition: ACR FOLLOW-UP CANDIDATE; NO HE DEFAULT.**

---

# 8. Prefill/read speed and decode/write speed can constrain different workloads

The video's high-memory section usefully emphasizes that coding agents spend substantial time consuming repository context, diffs, logs and tool output before generating relatively short actions or reviews.

That means a single `tokens/sec` number is underspecified.

Useful phases can include:

```text
model load / warmup
prompt ingestion / prefill
first-token latency
decode / generation
verification/tool execution
end-to-end verified task
```

A quantization or runtime may improve decode while hurting prompt processing. The IQ2_XS artifact reviewed in this pass is a concrete example: its measured generation throughput improved relative to its BF16 reference on the test host while prompt throughput fell substantially.

### HE implication

> **Measure the phase that constrains the workload.**

For code review and repository work, prefill can matter as much as decode.

**Disposition: STRONGLY REINFORCE existing phase-timing guidance.**

---

# 9. Reasoning effort should be judged on verified task completion, not one response

Qwen3.8 exposes configurable reasoning effort and enables thinking by default.

Community tests support the video's warning that the highest reasoning level can be much slower and more token-expensive on some tasks. That does not establish `medium` as a universal optimum.

The durable HE objective is:

```text
correct verified task completion
--------------------------------
 total time / tokens / cost / retries
```

rather than optimizing:

```text
time per turn
or
decode tokens/sec
```

Lower reasoning can save time on an easy step and still lose overall if it increases failed attempts or weak verification.

### HE implication

> **Tune reasoning policy against completed-task outcomes on the target workload.**

**Disposition: REINFORCE workload evidence; REJECT universal reasoning defaults.**

---

# 10. Provider/hardware economics are observations with an expiration date

The video presents specific rent-versus-buy break-even thresholds and provider token prices.

Those numbers are useful for a purchase decision at a point in time, but they are poor durable HE knowledge because they change with:

- provider pricing;
- model aliases and revisions;
- hardware street prices;
- electricity costs;
- utilization;
- quantization/runtime improvements.

Even within the current research window, hosted DeepSeek family naming/pricing has moved from the video's referenced V4 Flash framing to newer family versions/routes.

### Candidate HE rule

> **Economic evidence must be timestamped and tied to the exact provider/model route; do not promote transient break-even numbers into architecture rules.**

The durable decision dimensions are privacy/compliance constraints, utilization, workload fit, latency and current total cost.

**Disposition: RETAIN METHOD; PARK NUMBERS.**

---

# 11. Claim audit

| Video claim | Deep-pass disposition | HE treatment |
|---|---|---|
| KV/cache can dominate long-context memory | Supported | Retain |
| Qwen3.8 grows conventional KV in 16/64 layers | Supported by official architecture | Retain |
| ~128K Qwen vs Devstral KV is roughly <9 GB vs >21 GB decimal | Supported by published geometry, before runtime overhead | Retain as illustrative calculation |
| Ollama under 24 GiB defaults to 4K | Supported by current Ollama docs | Retain with version/date sensitivity |
| 12–48 GB should all use one model | Too universal | Reject as policy |
| 16 GB standard Q4 has poor/no useful headroom | Directionally supported | Retain headroom principle, not tier prescription |
| Two same-bit quant uploads can differ greatly | Supported in principle | Retain provenance rule; do not canonize 16/20 vs 7/20 claim |
| MTP is free speed | Backend-dependent / overstated | Reject; require activation/effect evidence |
| Medium reasoning is universally optimal | Insufficient evidence / task-dependent | Reject |
| Prompt/read speed matters at large context | Supported conceptually and by benchmark phase differences | Retain |
| Buy vs rent flips at one fixed hardware tier | Ephemeral economics | Park numeric threshold |
| Lean harness preserves useful local context | Supported as an engineering direction | Retain only with correctness constraint |

---

# 12. Durable HE findings from this pass

## STRONGLY MINE

1. **Growing-state topology is part of capacity.**
2. **Separate model-supported, runtime-allocated and effective task context.**
3. **Capability declaration → runtime support → activation → effect → workload benefit are separate evidence steps.**
4. **Quant artifact provenance can be material to correctness and capacity.**
5. **Prefill and decode are distinct performance dimensions.**
6. **Harness context tax is both semantic and physical for local inference.**
7. **Measure verified completed-task economics, not single-turn speed.**

## REINFORCE

- exact execution configuration is the unit of local-model evidence;
- operating headroom is a reliability property;
- runtime/configuration failures can masquerade as model-quality failures;
- quantization must be evaluated on the target workload;
- model/harness/runtime variables should be controlled independently during bake-offs.

## PARK

- the video's exact GPU tier chart;
- fixed rent-versus-buy thresholds;
- specific community throughput numbers where the complete envelope is unknown;
- the exact 16/20 versus 7/20 low-bit bug-fix score pair;
- 128GB+ workstation architecture as an HE implementation concern.

## REJECT

- “largest/best model that fits” selection;
- model-supported context as evidence of allocated context;
- MTP support as evidence of acceleration;
- universal `medium` reasoning policy;
- universal quant-bit quality thresholds;
- changing ACR defaults from model cards or hardware charts.

---

# 13. ACR follow-up created by this evidence

This pass creates a bounded, concrete ACR question rather than a speculative model-shopping exercise:

> **Can ACR's local-model calibration distinguish reviewer quality from execution-envelope differences, and does the current Ollama allocation provide enough effective context for the review workload?**

A dedicated ACR session should inspect before modifying anything:

- current exact local model/tag/digest;
- quantization/build identity where available;
- Ollama/runtime version;
- actual allocated context (`ollama ps` / request configuration);
- CPU/GPU placement/offload;
- prompt-eval versus generation timing if exposed;
- current calibration artifact provenance;
- whether timeouts occur during prefill, generation or orchestration.

Only after that inspection should candidate benchmark arms be considered. Two plausible research candidates from this pass are:

- Qwen3.6-35B-A3B under a reproducibly identified local configuration;
- one specifically identified aggressive Qwen3.8-27B quant that actually fits with useful context/headroom.

The experiment should keep fixtures and evaluator fixed and change one execution variable at a time.

**Do not change the ACR baseline from this research alone.**

---

# Bottom line

Kai's video is useful because it exposes a hidden harness variable rather than because its GPU shopping ladder is authoritative.

For HE, the strongest refinement is:

> **The unit of evidence is not “model X.” It is model artifact + runtime + context allocation + active features + placement + harness under a defined workload.**

And one additional verification principle deserves to survive outside local inference:

> **Do not infer that a capability is working from its presence. Measure the runtime effect that would prove it.**
