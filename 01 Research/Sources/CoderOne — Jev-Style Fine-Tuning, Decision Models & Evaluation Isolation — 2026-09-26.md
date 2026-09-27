# CoderOne — Jev-Style Fine-Tuning, Decision Models & Evaluation Isolation — 2026-09-26

## Scope

Deep-dive the CoderOne video **“I trained my own Jev for $5”** and its references for Harness Engineering implications.

Primary material:

- CoderOne video: https://www.youtube.com/watch?v=KmiVxA6Mtio
- CoderOne experiment repo: https://github.com/ipenywis/Jef
- Together AI `tev1`: https://github.com/togethercomputer/tev1
- Together AI article: https://www.together.ai/blog/how-to-train-your-own-jev
- TypeSafe AI Jev/System One launch material: https://typesafe.ai/blog/introducing-system-one-models-and-jev
- TypeSafe workflow evals: https://evals.typesafe.ai/
- TypeSafe System One adapter: https://github.com/typesafe-ai/system-one-adapter-python
- Featherless Simple Jev: https://github.com/featherless-ai/simple-jev
- Sun & Xu, **Type-Safe Is Not Error-Free**: https://arxiv.org/abs/2609.26758

Terminology:

- **Jev** = TypeSafe AI's hosted System One decision model/product.
- **Jef** = CoderOne's repository/artifact name for the Jev-inspired Qwen experiment.
- **Tev** = Together AI's open Jev-inspired Qwen recipe/model.

Do not conflate these implementations.

---

# Executive assessment

The durable HE finding is **not** “fine-tune Qwen instead of using Jev.”

CoderOne demonstrates that a small generative LLM can be cheaply specialized for a narrow decision interface, but the more important architecture lesson is workload matching:

```text
open-ended generation / changing instructions / explanation
        -> generative or fine-tuned LLM

changing questions/options + typed decision output
        -> decision-model-shaped mechanism

stable repeated label ontology
        -> conventional fixed classifier
```

Working principle:

> **Use the least-general inference mechanism that still matches the variability and evidence requirements of the task.**

This extends existing HE model-tier routing. The routing question is not only “which model/effort?” but sometimes “does this task need a generative model at all?”

---

# 1. What CoderOne actually built

`ipenywis/Jef` is explicit that it is an **independent Jev-inspired experiment**, not TypeSafe's Jev architecture, SDK or runtime.

The experiment:

- base: `Qwen/Qwen3.5-4B`, pinned to revision `851bf6e806efd8d0a36b00ddf55e13ccb7b8cd0a`;
- bf16 LoRA, rank 8 / alpha 16 / dropout 0;
- 10,616,832 trainable parameters (~0.23% of the 4B model);
- completion-only SFT;
- max sequence length 2,048;
- one epoch / 4,257 steps;
- one Modal L40S for training;
- merged bf16 weights served through vLLM;
- thinking disabled;
- output constrained to the supplied option letters.

This remains an autoregressive language model using Qwen's next-token head. It learns to emit one option letter. It does **not** reproduce Jev's native parallel typed-decision architecture or its calibrated Choice/Score/Noul contract.

**Disposition: RETAIN distinction.** A shared interface shape does not establish architectural equivalence.

---

# 2. Training cost is real but narrow

CoderOne records a completed training run of roughly 1h52m on a Modal L40S. The video reports about $4.17 training cost and rounds that to “$5.” The repo documents the measured runtime, while Modal pricing is time/resource dependent and can change.

Together's upstream article reports about $17 for its own hosted fine-tune using a different service path and hardware/runtime envelope.

### HE implication

> **Retain the method and measured run provenance; do not promote a dollar figure into architecture.**

Training price is timestamped provider evidence, not a durable property of “Jev-style fine-tuning.”

**Disposition: PARK exact cost as ephemeral.**

---

# 3. Dataset provenance is unusually strong for a tutorial repo

CoderOne copied Together's Tev recipe at pinned commit `1dde7782382c9f49d627153759b8d1deab426ce0`.

The reproduced upstream mix contains 37,840 training records and 4,568 development records across nine source families:

Public classification:

- MultiNLI;
- BoolQ;
- Banking77;
- AG News;
- SST-5.

Synthetic decision data:

- Policy;
- Policy v2;
- Routing v2;
- Research taxonomy v21.

The repo records dataset hashes and build provenance and warns that the recipe's MIT license does not automatically license all underlying third-party datasets for redistribution.

### HE implication

> **Training/eval artifact provenance must distinguish recipe/code licensing from source-data rights and identity.**

**Disposition: REINFORCE provenance discipline.**

---

# 4. CoderOne improves Together's evaluation boundary

Together's retained Tev evaluation explicitly says its development benchmarks were reused during development and are **not untouched final tests**.

CoderOne therefore creates a new 10% holdout from the upstream training pool:

- 34,056 training examples;
- 3,784 holdout examples;
- 16,632 training groups;
- 1,848 holdout groups;
- zero group overlap.

The split is deterministic and source-aware. Every `group_id` remains in one side only, preventing variants of the same logical item from leaking across train and holdout.

### HE principle

> **The split boundary must match the leakage boundary.**

Random row-level splitting is insufficient when multiple records derive from the same source case/document/scenario.

**Disposition: STRONGLY MINE for behavioral evals and ACR calibration.**

---

# 5. The final holdout is structurally isolated

The holdout remains local and is not uploaded to the Modal training volume.

This is stronger than a procedural instruction telling the trainer not to inspect final evaluation labels.

### HE principle

> **Evaluation isolation should be structural when practical, not merely procedural.**

Examples:

- training/optimization workers should not receive final holdout labels;
- control arms should not inherit treatment hooks/configuration;
- producers should not silently rewrite verifier acceptance semantics;
- optimizer loops should not consume final benchmark feedback that is meant to remain independent.

This complements existing Ponytail treatment-isolation and independent-assurance findings.

Important limit: the holdout is still drawn from the same nine source families, so the 88.85% result supports generalization to unseen groups from that mixture, not arbitrary real-world Jev equivalence.

**Disposition: STRONGLY MINE.**

---

# 6. Reported quality is meaningful but bounded

CoderOne reports 3,362 correct of 3,784 holdout cases = **88.85%** overall, with source-level accuracy ranging substantially (e.g. SST-5 materially weaker than policy/research synthetic sources).

This is useful evidence because the final holdout is group-isolated and kept away from the training environment.

It does **not** prove:

- parity with TypeSafe Jev;
- calibrated confidence;
- general decision-task accuracy;
- out-of-distribution robustness;
- Score or Noul quality;
- production throughput/capacity.

### HE principle

> **Evaluation claims should name the population and boundary actually tested.**

**Disposition: REINFORCE evidence-scope discipline.**

---

# 7. Zero invalid outputs are partly a harness guarantee

The evaluator calls vLLM with structured output constrained to the valid option letters. Therefore 0 invalid labels does not primarily demonstrate that the fine-tuned model learned perfect format discipline.

This is a concrete attribution lesson:

```text
semantic choice quality -> model + prompt/training/runtime
valid output alphabet   -> runtime/schema constraint
```

### HE principle

> **Attribute observed behavior to the layer that actually guarantees it.**

A deterministic/schema constraint can make format validity perfect while semantic accuracy remains imperfect.

**Disposition: STRONGLY MINE.**

---

# 8. Type safety is not semantic correctness

TypeSafe's Jev contract guarantees typed output over declared options. That is a valuable structural guarantee.

A September 2026 controlled study (`arXiv:2609.26758`) demonstrates why the guarantee must not be overextended: Jev and two Jev-like typed decision families maintained a 0% type-error rate while option-name/rubric interventions produced large semantic decision shifts. The paper's core distinction is that constraining support to legal outputs says nothing about whether probability mass is assigned to the semantically correct legal output.

### HE principle

> **Schema/type validity is a structural property; semantic correctness requires separate evidence.**

Corollary:

> **A perfect format-validity metric must not be counted as model-quality evidence when the harness makes invalid outputs impossible.**

**Disposition: STRONGLY MINE / add adversarial semantic-contract tests where typed decision models become consequential.**

---

# 9. Jev's real architecture target is different from CoderOne's experiment

TypeSafe describes Jev/System One as machine-native decision inference rather than string generation:

- shared unstructured/program state;
- typed `Choice`, `Score`, and `Noul` questions;
- typed probabilistic outputs;
- parallel answer production instead of autoregressive prose generation;
- training targeted at calibrated decisions (RLCD).

TypeSafe's workflow eval framing also emphasizes decomposing policy into narrow questions and keeping deterministic rules in code.

The useful HE abstraction is not the vendor-specific implementation:

> **If many narrow decisions share the same expensive state, avoid repeatedly paying for open-ended generation when a bounded decision primitive can satisfy the contract.**

This aligns with recent HE work on progressive disclosure, deterministic ownership, and avoiding repeated processing of expensive shared state.

**Disposition: MINE as workload architecture; do not adopt Jev automatically.**

---

# 10. Fine-tuned LLM vs decision model vs fixed classifier

CoderOne's comparison slide is useful after correcting several over-broad claims.

## Generative/fine-tuned LLM

Good fit when:

- instructions or output shape change frequently;
- explanation/prose is required;
- open-ended reasoning/generation matters;
- longer context or general capabilities are material.

Costs:

- larger inference envelope;
- autoregressive output path;
- probabilistic formatting unless constrained;
- raw token probabilities are not automatically calibrated confidence.

## Decision-model-shaped inference

Good fit when:

- the state/question/options can change;
- output is a bounded typed decision;
- low latency / high call volume matters;
- the application benefits from distributions/scores rather than prose.

Caution: probability outputs require calibration evidence; typed output does not prove semantic correctness.

## Fixed classifier

Good fit when:

- the label ontology is stable;
- the same classification problem repeats;
- labelled data exists;
- maximum simplicity/throughput matters.

The key constraint is the fixed output ontology, not a particular BERT-style architecture.

### HE principle

> **Use the least-general inference mechanism that still matches the variability, output contract and evidence requirements of the task.**

This is stronger than “use the smallest model.”

**Disposition: STRONGLY MINE into model/workload routing.**

---

# 11. Smoke-before-expensive-run is a useful operational pattern

CoderOne's workflow uses a small paid smoke training run before the full run. The smoke run tests:

- container/image resolution;
- data mount paths;
- model loading;
- LoRA/training setup;
- loss/eval execution;
- artifact export.

It found a real mount-path defect before the complete run.

### HE principle

> **For expensive/long-running execution, first run the smallest probe that exercises the same critical path.**

This is not specific to model training; it generalizes to migrations, deployments, benchmark sweeps and long autonomous jobs.

**Disposition: REINFORCE bounded execution / preflight evidence.**

---

# 12. Serving is a separate experiment from training

CoderOne's tutorial documents multiple serving-specific failures after successful training:

- outdated vLLM CLI flag;
- tokenizer regex compatibility warning;
- FlashInfer JIT path requiring unavailable `nvcc`;
- cold-start behavior;
- L40S/H100 runtime differences.

This is strong evidence that a trained artifact being valid does not establish a production-serving path.

### HE principle

> **Training/build success and runtime-serving fitness are separate gates.**

**Disposition: REINFORCE effect verification and execution-envelope identity.**

---

# 13. H100 vs L40S demonstrates workload-specific hardware fitness

CoderOne's small matched benchmark used 20 measured requests per endpoint, one warm-up request, concurrency 4 and the same deterministic sample.

The repo reports:

- L40S: p50 0.422s, p95 0.970s, 7.413 req/s;
- H100: p50 0.351s, p95 0.916s, 8.721 req/s.

The H100 was somewhat faster in the recorded matched benchmark, contrary to the video's informal impression that L40S was faster. The repo correctly calls the 20-request comparison a video-sized demonstration, not production capacity evidence.

### HE implication

> **Prefer instrumented matched comparisons over live-demo impressions.**

Hardware choice should include cost and workload utilization, not only fastest p50.

**Disposition: REINFORCE measured workload fitness; PARK hardware winner.**

---

# 14. Cold-start and warm latency are different products

The scale-to-zero deployment avoids idle GPU cost but produced cold starts measured from roughly 91 to 367 seconds during recorded runs. Warm request latency was near/sub-second.

### HE principle

> **Do not collapse cold-start latency, warm inference latency and throughput into one “speed” number.**

A cheap scale-to-zero experiment endpoint and an always-warm production endpoint have different operating contracts.

**Disposition: REINFORCE phase-specific runtime metrics.**

---

# 15. Artifact identity is exemplary

The training job records:

- base model + exact revision;
- dataset hashes;
- training recipe;
- package versions;
- GPU identity;
- adapter/merged artifact paths;
- generated manifest.

The repo then verifies saved model shards and serving load behavior.

This is a strong concrete example of HE's existing principle:

> **Evidence must identify the artifact/configuration it verifies.**

**Disposition: STRONGLY REINFORCE execution provenance.**

---

# 16. Related open implementations are evidence, not Jev replicas

`featherless-ai/simple-jev` exposes a Jev-inspired typed decision API over ordinary HF models by reading next-token logits and constructing typed responses server-side. It explicitly warns that its returned distributions/Noul values are not calibrated probabilities of correctness.

TypeSafe's `system-one-adapter-python` similarly provides a comparison adapter that maps ordinary LLM providers into the System One API shape and retains detailed attempt/usage provenance.

These projects reinforce an important separation:

```text
API/interface compatibility
!=
model architecture equivalence
!=
calibration equivalence
!=
behavioral equivalence
```

**Disposition: STRONGLY MINE portability boundary.**

---

# 17. HE candidate principles from this pass

> **Use the least-general inference mechanism that still matches the variability, output contract and evidence requirements of the task.**

> **The split boundary must match the leakage boundary.**

> **Evaluation isolation should be structural when practical, not merely procedural.**

> **Attribute observed behavior to the layer that actually guarantees it.**

> **Schema/type validity is a structural property; semantic correctness requires separate evidence.**

> **A perfect format-validity metric is not model-quality evidence when the harness makes invalid output impossible.**

> **For expensive execution, first run the smallest probe that exercises the same critical path.**

> **Training/build success and runtime-serving fitness are separate gates.**

> **Prefer instrumented matched comparisons over live-demo impressions.**

> **Do not collapse cold-start latency, warm latency and throughput into one speed number.**

> **Interface compatibility does not prove architecture, calibration or behavioral equivalence.**

---

# Research disposition

## STRONGLY MINE

- workload-to-inference-class routing;
- group-aware leakage boundaries;
- structural holdout isolation;
- schema-validity vs semantic-correctness separation;
- layer-owned attribution of guarantees;
- exact artifact/run provenance.

## REINFORCE

- smoke tests before expensive runs;
- runtime/serving as a separate gate;
- phase-specific latency metrics;
- matched benchmark conditions;
- provider economics as timestamped evidence.

## ASSESS

- whether any current HE/ACR workload contains enough repeated narrow typed decisions to justify testing a decision model rather than a general LLM;
- whether ACR benchmark fixtures need stronger group-level leakage checks;
- whether future typed decision routing needs adversarial option-name/rubric tests.

## PARK

- adopting Jev, Laya, Tev, Simple Jev or a custom classifier as HE infrastructure without a demonstrated workload;
- building a general inference router;
- fine-tuning a custom HE classifier merely because training is inexpensive.

## REJECT

- “$5 fine-tune” as proof of cheap production inference;
- treating 0 invalid labels as evidence of semantic model quality when structured decoding enforces validity;
- treating a held-out sample from the same source families as proof of broad Jev equivalence;
- treating output probabilities from an ordinary generative model as calibrated confidence without calibration evidence;
- treating API compatibility as architectural equivalence.

---

# Bottom line

The experiment is more valuable than its headline.

It shows that specialized decision behavior can be cheaply taught to a small generative model, but the durable HE result is broader:

> **Match the inference mechanism to the work. Preserve independent evaluation structurally. Attribute guarantees to their owning layer. And never confuse type-safe output with semantically correct decisions.**
