# The Stack — Local Coding Model Memory Tiers, Headroom & Artifact Fit — 2026-09-29

## Purpose

Evaluate The Stack's `Best Local AI For Every GPU Level (4GB To 512GB)` as Harness Engineering evidence rather than turning its September 2026 model picks into durable HE policy.

Primary sources reviewed:

- Video: https://www.youtube.com/watch?v=uIkbRvCihH8
- User-provided transcript and source list captured 2026-09-29
- Qwen3.8-27B official model card
- Ornith-1.5-9B official model card / repository
- OpenAI gpt-oss release and model card
- AtomicChat Qwen3.8-Flash-Next GGUF documentation
- Existing HE local-inference research and Qwen3.8 comparison notes

This source is mostly **corroborating evidence** for HE's existing local-runtime principles. It does not justify a new model ladder, hardware purchase guide or model default.

---

# Executive finding

The video's best rule is also its least flashy:

> do not fill the machine with the largest model file it can technically hold.

The useful selection problem is:

```text
usable memory / topology
- operating-system and runtime needs
- context/KV growth
- working buffers
- tool/harness overhead
        ↓
feasible model artifacts with headroom
        ↓
verify on the actual repository workload
```

A memory tier therefore produces **candidate configurations**, not a proven optimum.

---

# 1. Headroom is more important than nominal fit

The video repeatedly distinguishes model weights from the working state needed by an agent. It recommends leaving room for active context, repository material and runtime scratch space rather than selecting the largest file that can be loaded.

This directly reinforces HE's existing rule:

> **Nominal fit without operating margin is brittle.**

No new policy is needed.

**Disposition: STRONGLY REINFORCE.**

---

# 2. Source-model benchmarks do not transfer automatically to local quants

The video is unusually explicit about this caveat.

Examples:

- Qwen3.8-27B's published 61.7 SWE-bench Pro result is for Qwen's evaluated source-model configuration, not every local Q4 artifact;
- Ornith-1.5-9B's published Terminal-Bench improvement is evidence about the source model under its benchmark protocol, not direct evidence for a particular desktop GGUF;
- community quantizations and custom runtimes can materially change memory, speed and behavior.

Qwen's official model card confirms the 61.7 SWE-bench Pro result under a Claude Code harness with specific sampling/context conditions. Ornith's official card similarly reports its 46.2 Terminal-Bench 2.1 result under a named Terminus harness and separately reports a Claude Code score.

### HE principle

> **Benchmark evidence follows the execution configuration that produced it; a local quant is a new candidate until measured.**

**Disposition: STRONGLY REINFORCE exact execution identity.**

---

# 3. The uploader/build can matter as much as the nominal quant label

The video warns that two files both called `Q4` can have different sizes and packaging behavior.

HE already treats quantized artifacts as execution identities rather than only `model + bit depth`.

### HE implication

Record the minimum provenance needed to reproduce a meaningful comparison:

```text
base model / revision
quantizer or uploader when material
quant/build name
digest/tag if available
runtime/version
relevant placement/context settings
```

Do not turn this into a universal artifact registry unless repeated experiments require one.

**Disposition: REINFORCE.**

---

# 4. Runtime protocol support is part of agent fitness

The gpt-oss example is useful because a model can advertise native tools/structured output while a local runner or client fails to implement the expected protocol correctly.

OpenAI's model documentation confirms that gpt-oss is trained around the Harmony format and tool/structured-output behavior. A local agent that merely prints a nominal tool call instead of executing it has not reproduced the intended harness behavior.

### HE principle

> **Model capability + runtime support + client/harness integration together determine whether a capability is usable.**

This is the same evidence ladder HE already applies to thinking, MTP/speculation, context and other runtime features.

**Disposition: STRONGLY REINFORCE.**

---

# 5. More memory can buy fidelity or headroom instead of a larger model

The 32–48 GB tier keeps Qwen3.8-27B as the video's primary candidate and uses extra memory for a less aggressive quant or more context rather than automatically stepping to a larger architecture.

That is a useful counterweight to "bigger GPU -> bigger model."

### HE principle

> **Additional capacity should be allocated to the execution variable that improves the target workload; that may be model size, precision, context, residency, concurrency or simply headroom.**

**Disposition: REINFORCE hardware-capacity-as-enabler.**

---

# 6. Unified memory is not dedicated VRAM

The video correctly distinguishes Apple/unified-memory systems from dedicated-GPU configurations. The operating system and applications share the same pool, and usable accelerator memory can be below the headline capacity.

### HE implication

Hardware recommendations must refer to the **usable execution budget**, not only the machine's marketed memory size.

This is already covered by HE's hardware identity and execution-envelope guidance.

**Disposition: REINFORCE.**

---

# 7. Architecture-specific SSD residency is not generic model offload

The Qwen3.8-Flash-Next example is especially useful because its SSD behavior can be misunderstood.

AtomicChat documents a Mac configuration where a large n-gram lookup table stays on SSD while model weights remain resident. The access pattern is sparse and deterministic, making that table qualitatively different from streaming ordinary dense weights/expert matrices from disk every token.

The same documentation requires particular file sharding and runtime flags.

### HE principle

> **When a model appears to exceed memory capacity, identify which state is resident, conditional, pageable or streamed and why the access pattern is viable. Do not generalize an architecture-specific storage trick into a generic offload rule.**

**Disposition: STRONGLY REINFORCE storage/topology semantics.**

---

# 8. Published artifacts drift

The video calls out older guidance whose file-size assumptions no longer match the current artifact.

That is a small but important provenance lesson:

```text
model name
+ quant label
```

may still be insufficient when a community artifact changes over time.

### HE principle

> **Time-sensitive local deployment guidance should bind claims to a date/revision/artifact identity when drift would change feasibility.**

**Disposition: REINFORCE timestamped evidence.**

---

# 9. GPU tier charts are discovery aids, not routing policy

The video ends by telling users to treat the rows as starting points and verify actual memory placement plus real repository behavior.

That is the correct HE interpretation.

A durable tier chart would age quickly because:

- new models/quantizers appear;
- runtimes gain support;
- model artifacts change;
- benchmark protocols move;
- hardware topology differs;
- the target repository workload is not represented by VRAM alone.

### Candidate principle

> **Use hardware/model tier charts to generate candidate configurations; select only after exact-fit and workload evaluation.**

**Disposition: NEW WORDING / REINFORCE EXISTING RULES.**

---

# What HE should mine

## STRONGLY REINFORCE

- leave operating headroom;
- distinguish weights from active working state;
- source benchmark != quantized local result;
- exact artifact/runtime identity matters;
- runtime tool/protocol support must be verified;
- extra capacity may be better spent on headroom/precision/context than a larger model;
- unified memory and dedicated VRAM are different execution topologies;
- architecture-specific SSD tricks should not become generic offload guidance;
- timestamp artifact-dependent feasibility claims;
- local model charts are candidate discovery only.

## ASSESS

- whether ACR's local bake-off records enough artifact/runtime identity to distinguish these cases;
- whether one or two current 8–16 GB configurations deserve bounded experiments on the user's actual ACR fixtures.

## PARK

- every specific September 2026 model-per-VRAM recommendation;
- 128–512 GB workstation guidance that has no present workload;
- large-model SSD-streaming infrastructure;
- generalized HE hardware inventory.

## REJECT

- a permanent HE GPU-to-model table;
- largest-model-that-loads selection;
- carrying source-model benchmark numbers onto arbitrary quants;
- assuming a supported model name means tool calls work end to end;
- treating file size as total runtime memory;
- treating an architecture-specific SSD lookup-table design as ordinary offload.

---

# Bottom line

This video is useful mostly because its own caveats line up with HE's current direction.

The durable rule remains:

```text
hardware budget
        ↓
generate feasible exact configurations with headroom
        ↓
verify runtime/protocol/placement
        ↓
measure on the target repository workload
        ↓
keep the least expensive configuration that preserves required behavior
```

The memory tier is the beginning of candidate discovery, not the answer.