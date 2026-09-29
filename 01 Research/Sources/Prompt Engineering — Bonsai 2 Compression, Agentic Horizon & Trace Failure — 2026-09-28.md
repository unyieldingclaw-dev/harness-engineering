# Prompt Engineering — Bonsai 2 Compression, Agentic Horizon & Trace Failure — 2026-09-28

## Purpose

Deep-dive Prompt Engineering's video **“Bonsai 2: Qwen 27B on 6GB VRAM”**, PrismML's Bonsai 2 release material, model/runtime repositories, and the benchmark claims relevant to Harness Engineering.

Primary user source:

- Video: https://www.youtube.com/watch?v=jxOOiNUB9DQ
- Creator: Prompt Engineering
- Transcript supplied by the user on 2026-09-28

Primary technical sources checked:

- PrismML, **Introducing Bonsai 2 27B** — https://prismml.com/news/bonsai-2-27b
- PrismML model/demo repository — https://github.com/PrismML-Eng/Bonsai-demo
- PrismML Bonsai 2 whitepaper — https://github.com/PrismML-Eng/Bonsai-demo/blob/main/bonsai-2-27b-whitepaper.pdf
- PrismML GGUF model card — https://huggingface.co/prism-ml/Ternary-Bonsai-2-27B-gguf
- PrismML MLX model card — https://huggingface.co/prism-ml/Ternary-Bonsai-2-27B-mlx-2bit
- `AGENTS.md`, `MODEL-FORMATS.md`, backend/runtime guidance and community benchmark records in the PrismML demo repository

Related HE research:

- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Sources/Kai — Local Coding AI, KV Cache & Runtime Envelope — 2026-09-26.md`

This note is research evidence. It does **not** authorize an ACR model change, PrismML runtime installation, hardware purchase, or new routing subsystem.

---

# Executive finding

Bonsai 2 is a technically impressive compression result, but it demonstrates why **compression fidelity must be evaluated at the workload horizon that matters**.

PrismML's headline is real under its stated benchmark suite:

```text
Ternary Bonsai 2 27B   83.9
Qwen3.8-27B FP16       85.4
retention              98.2%
```

The model packs Qwen3.8-27B-derived language weights into approximately 5.9 GB in the densest GGUF band.

However, PrismML's own long-horizon agent benchmarks are much weaker relative to full precision:

```text
Terminal-Bench 2.1
Bonsai 2      52.8
FP16 Qwen     69.7
retention    ~75.8%

SWE-bench Verified
Bonsai 2      60.8
FP16 Qwen     80.6
retention    ~75.4%
```

Prompt Engineering's independent hands-on experiment shows an even sharper failure on six long application-building tasks while short tasks remained near parity.

The durable HE point is not “ternary models are bad.” It is:

> **A compressed model can preserve short-horizon benchmark capability while losing the state tracking, recovery, or control behavior required for long agent loops.**

---

# 1. What Bonsai 2 actually is

PrismML describes Ternary Bonsai 2 27B as derived from Qwen3.8-27B and trained into a ternary representation rather than merely post-training rounding every weight with a conventional generic quantizer.

Its language weights use ternary values with group-wise scale information. PrismML's released GGUF bands are:

```text
PTQ1_0   ~1.75 bits/weight   ~5.9 GB
PQ2_0    ~2.13 bits/weight   ~7.2 GB
```

The model card describes the underlying ternary representation as approximately 1.72 effective bits/weight before packing overhead.

The 5.9 GB claim therefore refers to a specific model artifact/packing, not the complete memory required to run a useful agent session.

### HE implication

This strongly reinforces:

> **Artifact size is not execution working-set size.**

Weights, KV/context state, runtime buffers, vision components, tool/harness context and operating headroom remain separate resource consumers.

**Disposition: REINFORCE existing local-runtime envelope guidance.**

---

# 2. Bonsai 2 has a runtime dependency that is part of correctness

Bonsai 2's released ternary GGUFs are **not generic stock llama.cpp artifacts**.

PrismML's repository says the rotated weight basis requires an activation transform present in its custom llama.cpp build.

The runtime boundary is consequential:

- `PTQ1_0` / `PQ2_0` safely fail in stock llama.cpp because the types are unknown;
- the development `Q2_0` band can be more dangerous because upstream recognizes the type and Qwen architecture but lacks the required transform, so it can load and produce garbage rather than fail cleanly;
- PrismML deliberately keeps that development file separate and labels it fork-required.

### Candidate principle

> **When an artifact depends on a custom runtime interpretation, artifact identity and runtime identity together form the executable model.**

A model file that loads is not evidence that the model is executing correctly.

This is a concrete example of HE's existing rule:

> capability declaration / load success -> runtime activation evidence -> workload evidence.

**Disposition: STRONGLY REINFORCE.**

---

# 3. The 98.2% claim is real — and narrower than it sounds

PrismML's release reports an aggregate 83.9 versus 85.4 for full-precision Qwen3.8-27B across a multi-domain suite covering knowledge/reasoning, math, coding, instruction following, vision and agentic/tool-use tasks.

The aggregate is useful for broad capability retention.

It is not a guarantee of equal retention on every workload class.

The long-horizon agent results in the whitepaper are materially weaker, with approximately three-quarters retention on Terminal-Bench 2.1 and SWE-bench Verified.

Those results matter because long agent loops repeatedly compound small errors in:

- tool selection;
- state tracking;
- progress recognition;
- recovery;
- planning continuity;
- stopping behavior.

### Candidate principle

> **Capability retention is conditional on task horizon and interaction topology, not just benchmark average.**

**Disposition: NEW / STRONGLY MINE.**

---

# 4. Prompt Engineering's test design is useful but not model-only isolation

The creator ran:

- Bonsai 2 locally through PrismML's llama.cpp fork;
- full Qwen3.8-27B through OpenRouter;
- both inside the same Pi coding harness;
- same prompts, tools and instructions;
- thinking disabled for both.

That is a reasonable practical comparison, but it is not a pure model-weight A/B because the execution paths differ materially:

```text
Bonsai 2
local hardware
PrismML fork/runtime
compressed artifact

vs

Qwen3.8 FP16/provider build
hosted inference path
provider runtime
```

The creator also intentionally turns thinking off, whereas PrismML's headline benchmark numbers are reported in thinking mode at high/max effort.

Therefore the experiment answers:

> “How did these two available execution configurations behave in this harness with thinking off?”

It does **not** isolate compression as the only causal variable.

The creator says an earlier Bonsai run on different hardware failed in a similar pattern, which strengthens the hypothesis but still does not provide a fully controlled model-only comparison.

### HE implication

> **Preserve the exact comparison question. Do not silently upgrade a practical system comparison into a causal model-quality claim.**

**Disposition: REINFORCE execution-envelope provenance.**

---

# 5. Short tasks looked close

The video reports near-parity on its short-task group.

Examples include:

- SVG drawing;
- analog-clock rendering;
- a planted-bug debugging task;
- a spreadsheet/data-cleaning task;
- a broken refund-report task;
- a roughly 100K-token repository fact-retrieval task;
- a short single-file UI task.

Both configurations showed successes and failures in broadly similar places.

This supports the narrower interpretation of PrismML's compression claim:

> **For bounded work that completes in one or a few turns, the compressed artifact may preserve much of the parent's usable capability.**

It does not establish long-loop equivalence.

**Disposition: RETAIN as workload-scoped evidence.**

---

# 6. Long agent builds exposed a qualitatively different failure mode

The second test set contains six longer builds requiring repeated planning, file work, execution, visual inspection and repair.

The creator reports:

```text
full Qwen3.8: 35 / 60 rubric points
Bonsai 2:     no working page across the six builds
```

The more useful evidence is in the traces.

On one task, Bonsai made 153 tool calls; approximately 150 were read/list/search-style exploration. On another observed loop, the same tool was called 114 times.

The full model also failed and wasted time, but its worst repeated identical action across the six tasks was reported as six rather than 114.

### HE interpretation

The compressed model was not merely producing syntactically worse code.

The observed pathology was closer to:

```text
observe
↓
fail to recognize sufficient progress / state change
↓
observe again
↓
repeat
↓
never transition into productive execution
```

This is exactly why end-state scoring alone is insufficient for diagnosis.

### Candidate principle

> **For long-horizon agents, evaluate progress control and state-transition behavior in addition to answer quality.**

Possible diagnostics, only when a failure requires them:

- repeated identical tool calls;
- repeated retrieval of unchanged state;
- steps without artifact progress;
- failure to transition from exploration to execution;
- retry loops without changed hypothesis or evidence;
- tool-call count before first material modification;
- stop/escalation behavior.

Do not turn these into universal telemetry. Capture them where they discriminate the observed failure mode.

**Disposition: NEW / STRONGLY MINE.**

---

# 7. Benchmark averages can hide the workload that matters most

A single aggregate such as 98.2% can be mathematically correct while masking a much larger deficit in a consequential sub-domain.

This is not a criticism unique to PrismML. Any aggregate can hide a tail or workload-specific cliff.

### Candidate principle

> **Do not let an aggregate benchmark score substitute for the metric attached to the deployment workload.**

For an agent deployment, long-horizon agent benchmarks and real workflow traces can be more decision-relevant than a broad average dominated by one-shot tasks.

**Disposition: STRONGLY REINFORCE behavioral-eval design.**

---

# 8. Reasoning configuration is part of the benchmark identity

PrismML's published headline suite uses thinking/reasoning enabled at high/max effort.

Prompt Engineering disables thinking on both candidates to inspect a non-thinking baseline.

Neither is inherently the “correct” configuration.

They answer different questions.

### HE implication

A statement such as:

```text
Bonsai 2 retains X% of Qwen
```

is incomplete without the reasoning configuration when that setting materially changes behavior.

This reinforces:

> **Model + artifact + runtime + reasoning policy is the evaluated configuration.**

**Disposition: REINFORCE.**

---

# 9. Context support is not free just because weights are tiny

PrismML advertises a 262K model context window, but its own runtime scripts choose RAM-tiered context defaults rather than automatically allocating the model maximum.

Its agent guide states FP16 KV state is approximately 64 KiB/token for this architecture, or roughly 6.3 GiB at 100K tokens, before the rest of the execution working set.

PrismML also provides optional lower-precision KV modes for memory-constrained long contexts.

### HE implication

This reinforces the existing distinction:

```text
small weight artifact
!=
small long-context working set
```

and:

```text
model context capability
!=
runtime-allocated context
!=
effective task context
```

**Disposition: REINFORCE existing local-runtime synthesis.**

---

# 10. “Runs on 6 GB” should not become a hardware tier rule

The densest language-weight file is approximately 5.9 GB.

That does not establish that a 6 GB GPU provides a healthy agent execution envelope once runtime allocations, context state and other components are included.

PrismML's own project supports multiple forms of CPU/GPU/unified-memory execution and publishes community hardware benchmarks rather than claiming a universal 6 GB agent configuration.

### Candidate principle

> **A compressed weight file can expand hardware eligibility without proving workload viability on the minimum memory that holds the file.**

**Disposition: REINFORCE working-set boundary.**

---

# 11. Implication for ACR

Bonsai 2 is interesting because it could make a 27B-derived model artifact executable on hardware that cannot comfortably host a conventional 4-bit 27B configuration.

That is enough to make it a **candidate**, not a default.

If ACR ever evaluates it, the benchmark must record at least:

- exact Bonsai 2 packing;
- exact PrismML runtime/build;
- context allocation;
- KV mode;
- reasoning setting;
- CPU/GPU placement;
- same ACR fixtures/evaluator;
- review-quality metrics;
- timeout behavior;
- repeated-tool/progress pathologies if the reviewer uses tools or an agent loop.

The key hypothesis would be:

> **Does the compression preserve ACR's bounded review judgment even if it degrades long-horizon autonomous building?**

That is an empirical question and belongs in ACR, not HE.

**Disposition: ASSESS in ACR only if current local-envelope work shows a useful test slot.**

---

# Consolidated HE mining

## STRONGLY MINE / REINFORCE

- compression fidelity is workload- and horizon-dependent;
- aggregate benchmark retention can hide a deployment-critical capability cliff;
- trace behavior can reveal control/state failures that final score cannot diagnose;
- runtime identity can be part of model correctness, not merely performance;
- reasoning policy belongs in benchmark provenance;
- small weights do not imply small long-context working set;
- minimum file fit does not prove agent viability.

## ASSESS

- whether ACR's bounded review workload tolerates aggressive compression better than long autonomous coding;
- minimal loop/progress diagnostics needed if ACR gains agentic tool loops;
- exact runtime/artifact provenance needed for a Bonsai comparison.

## PARK

- installing PrismML's runtime for HE itself;
- treating Bonsai 2 as a replacement for Qwen3.8 in general;
- building a generic agent-loop pathology detector without an observed need.

## REJECT

- “98.2% benchmark retention” -> “98.2% agent equivalence”;
- “5.9 GB weights” -> “healthy 6 GB agent runtime”;
- a file loading successfully as proof of correct runtime interpretation;
- short-task parity as sufficient evidence for long autonomous work.

---

# Bottom line

Bonsai 2 demonstrates two truths at once:

1. modern compression can preserve an extraordinary amount of short-horizon capability in a tiny artifact;
2. the remaining loss can concentrate in exactly the multi-step control behavior that agents depend on.

For HE the durable rule is:

> **Evaluate compression at the task horizon and interaction pattern you intend to deploy, and use traces to diagnose whether failures come from knowledge, coding, state tracking, progress control, runtime configuration, or another owning component.**
