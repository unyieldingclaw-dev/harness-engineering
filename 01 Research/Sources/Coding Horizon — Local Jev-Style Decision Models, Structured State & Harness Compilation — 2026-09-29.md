# Coding Horizon — Local Jev-Style Decision Models, Structured State & Harness Compilation — 2026-09-29

## Purpose

Evaluate `The New Age Of Local Super AI (Qwen + Jev)` against the current independent Jev-inspired projects rather than treating the demonstrations as proof that tiny local models replace general-purpose LLMs.

Primary sources reviewed:

- Video: https://www.youtube.com/watch?v=AsEgT9SpobM
- User-provided transcript captured 2026-09-29
- NanoJev: https://github.com/TianyuCodings/NanoJev
- SemIf: https://github.com/tseanard/SemIf
- JevHarness: https://github.com/TianyuCodings/JevHarness
- imajev: https://github.com/mohit67890/imajev
- Current public documentation/results linked by those projects

These projects are independent implementations inspired by Jev's decision interface. They are not downloadable copies of TypeSafe's hosted Jev model.

This is research only. It does **not** authorize Jev, NanoJev, SemIf, imajev or JevHarness adoption.

---

# Executive finding

The useful direction is not:

```text
tiny local model -> replace the general LLM
```

It is:

```text
strong model / human designs the task
        ↓
ordinary code extracts deterministic state and legal actions
        ↓
small decision model handles repeated fuzzy choices
        ↓
trusted code executes and verifies effects
```

The more the harness can turn a messy environment into a bounded decision contract, the less general inference is required at runtime.

---

# 1. NanoJev demonstrates specialization, not general intelligence

NanoJev uses Qwen3-0.6B with decision heads and returns probability distributions over supplied candidates without autoregressive answer-token decoding.

Its published game results are deliberately uneven:

- ViZDoom Basic: 128/128 successes;
- Predict Position: 27/128;
- Maze: 4/10;
- Snake: 8/8.

Hosted Jev performs better on the maze (7/10) while NanoJev dominates the basic shooting task.

That is exactly what a specialized mechanism should look like: a large gain inside one learned decision surface without a claim of broad capability transfer.

### HE principle

> **A narrow model's fitness is defined by the decision surface it was evaluated on; success on one bounded control loop does not generalize to adjacent tasks without evidence.**

**Disposition: STRONGLY REINFORCE capability step-down as workload-specific.**

---

# 2. The harness removes work before inference

The Doom and maze demonstrations do not feed raw pixels to a 0.6B model and ask it to act like a full game-playing agent.

The surrounding system supplies structured observations. The maze also uses ordinary exploration/path memory logic. The decision model receives a much smaller problem:

```text
state already extracted
+ question already chosen
+ legal actions already enumerated
        ↓
which action / score / probability?
```

### HE implication

> **Do deterministic perception, state derivation, bookkeeping and legal-action construction outside the model when those properties can be computed reliably. Spend model capacity only on the residual semantic judgment.**

This is stronger than simply choosing a smaller model; it changes the shape of the task.

**Disposition: NEW EVIDENCE / STRONGLY MINE.**

---

# 3. Direct decision readout can remove wasted generation — but may change the answer

SemIf compares two ways of using the same frozen Qwen3.5-4B model on the same state and 21 binary criteria:

- direct typed option-logit readout: median 1.023 s, zero output tokens;
- compact autoregressive JSON array: median 5.332 s, 111 output tokens.

That is a real systems saving when the application immediately turns the model output into control flow.

But the two paths agreed on only 18/21 choices. SemIf correctly labels the result as a systems comparison, not semantically identical inference delivered faster.

### HE principles

> **If generated text is immediately discarded and converted into a bounded decision, evaluate whether a decision-native mechanism can remove unnecessary generation.**

> **A faster inference path is a different mechanism until semantic equivalence is demonstrated; speed is not evidence that the decisions are interchangeable.**

**Disposition: NEW / STRONGLY MINE.**

---

# 4. Shared-state inference is a workload-shape optimization

SemIf also shows that when many questions share the exact same long state, that state can be prefetched once and reused across criteria.

Its 37-state × 21-criterion benchmark reports much higher throughput with prefix reuse and parallel suffix evaluation than with fresh scoring.

However, the experimental fast paths changed a small number of argmax decisions compared with fresh BF16 scoring.

### HE implication

> **Exploit repeated-state structure when it materially reduces repeated work, but treat the optimized execution path as a separately evaluated configuration when it can change outputs.**

This is analogous to cache/quantization/runtime changes elsewhere in HE.

**Disposition: NEW EVIDENCE / REINFORCE exact execution identity.**

---

# 5. Multimodal decision models expand the mechanism class, not the authority class

imajev applies the bounded decision pattern to text + images and includes an explicit `unknown` probability / abstain behavior.

That matters because some tasks need perceptual evidence but still have a narrow output contract:

```text
photo + record
        ↓
which declared field is contradicted?
```

rather than:

```text
photo + record
        ↓
generate an open-ended report
```

### HE implication

> **Input modality and output generality are independent dimensions. A multimodal task may still be a bounded typed decision rather than a generative-language task.**

Do not route to a large general vision model solely because the input contains an image.

**Disposition: NEW / STRONGLY MINE.**

---

# 6. Confidence and unknown need calibration, not faith

The video correctly warns that confidence is not a certificate of correctness.

The local projects reinforce that point:

- probabilities are conditional on the supplied options and prompt/interface;
- calibration can differ across workloads and modalities;
- imajev explicitly calibrates and exposes unknown/abstention;
- harder decision subsets show meaningful quality gaps between small local and stronger hosted systems.

### HE principle

> **Decision confidence is useful only after calibration on the relevant workload and consequence boundary; preserve an explicit abstain/escalate path.**

An automatically reversible classification and an irreversible purchase should not share the same execution threshold merely because the same model produced both.

**Disposition: STRONGLY REINFORCE.**

---

# 7. JevHarness exposes a stronger pattern: author once, execute many

The most HE-relevant repository in this batch may be adjacent to the video rather than central to it.

JevHarness lets a strong authoring LLM create a task-specific runtime made of:

- feature construction;
- explicit code/control flow;
- decision questions/criteria;
- memory/state;
- narrow Jev calls.

Once selected, the harness is frozen and executes without the authoring LLM making every runtime decision.

The critical boundary is good:

```text
trusted task adapter owns
  observations
  legal actions
  side effects
  scoring / reward

candidate harness owns
  feature construction
  decision questions
  control logic
```

The candidate harness cannot silently rewrite its reward or access hidden task state through the trusted adapter.

### Candidate HE principle

> **For repeated tasks, expensive open-ended reasoning can sometimes be compiled into an explicit, reviewable harness whose runtime uses cheaper bounded judgments.**

This is not "distill the LLM's chain of thought." The durable artifact is explicit code, features, state, criteria and control flow that can be reviewed and tested.

**Disposition: NEW / STRONGLY MINE.**

---

# 8. Harness optimization still needs an external evaluator

JevHarness can optionally revise candidates from execution traces and reward feedback, but its architecture keeps the environment/reward outside the candidate harness.

The repo also explicitly says its Pokémon Eval set was used for candidate selection, so the reported improvement is not an independent estimate on unseen games.

That is an important example of honest evidence scope.

### HE principles

> **An optimizer may change the harness, but should not own or silently redefine the reward/evaluator used to select it.**

> **A selection set is not an untouched final holdout merely because it is named Eval.**

**Disposition: STRONGLY REINFORCE independent acceptance and leakage-aware evaluation.**

---

# 9. The right architecture can be hybrid by inference class

The video uses a travel-assistant example that lands on a useful decomposition:

```text
bounded local decision model
  -> private sorting / routing / yes-no checks

generative model
  -> explanation / writing / ambiguous reconciliation

browser/control harness
  -> separately verified environment actions
```

That is a better mental model than attempting to force one model to perform every stage.

### HE implication

> **Route by inference class and required authority before model brand or parameter count.**

**Disposition: STRONGLY REINFORCE existing model-tiered workflow research.**

---

# What HE should mine

## STRONGLY MINE / REINFORCE

- structured state and legal-action construction before inference;
- decision-native inference when the application only needs a bounded choice/score;
- exact evaluation of optimized readout/cache paths;
- explicit abstain / escalation for uncertain decisions;
- modality separated from output generality;
- author-once / execute-many task-specific harness compilation;
- trusted environment/reward outside the candidate harness;
- selection evidence distinguished from independent final holdout evidence.

## ASSESS

- whether ACR contains repeated semantic choices that could be expressed as bounded decisions rather than generative review;
- whether any ACR or PMB workflow repeatedly regenerates decision logic that should instead exist as ordinary code/configuration;
- whether shared-state decision batches would materially reduce repeated inference in an existing workload.

## PARK

- JevHarness installation;
- custom NanoJev training;
- multimodal decision-model adoption;
- a generic decision-model routing layer.

## REJECT

- interpreting NanoJev's Doom result as broad model superiority;
- assuming direct-logit decisions are semantically identical to generated answers;
- letting a candidate harness define its own reward/acceptance boundary;
- treating an Eval set used for candidate selection as untouched final evidence;
- choosing an inference mechanism by parameter count alone.

---

# Bottom line

The strongest pattern in this research is **judgment compilation**:

```text
use broad intelligence during design
        ↓
make the task contract explicit
        ↓
compute deterministic state in code
        ↓
freeze legal actions and side-effect boundaries
        ↓
use a small decision mechanism for repeated fuzzy choices
        ↓
verify effects and escalate uncertainty
```

That is meaningfully different from "small models are now super AI." It is a harness architecture that makes a small model useful by narrowing what intelligence is required at runtime.