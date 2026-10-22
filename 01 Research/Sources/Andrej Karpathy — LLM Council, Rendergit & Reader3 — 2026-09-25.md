# Andrej Karpathy — LLM Council, Rendergit & Reader3 — 2026-09-25

## Purpose

Extend the earlier Karpathy GitHub pass beyond `autoresearch`, `nanochat`, and `llama2.c` by mining three additional Karpathy-owned repositories for Harness Engineering mechanisms:

- `karpathy/llm-council`
- `karpathy/rendergit`
- `karpathy/reader3`

This is source research only. It does not authorize adopting a council architecture, flattening whole repositories into model context, or treating disposable generated code as a general engineering policy.

Related HE notes:

- `01 Research/Behavioral Invariants, Declarative Goals & Bounded Autonomous Loops — 2026-09-25.md`
- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Context Engineering.md.md`

---

# Sources reviewed

## `karpathy/llm-council`

Pinned commit:

`92e1fccb1bdcf1bab7221aa9ed90f9dc72529131`

Reviewed:

- `README.md`
- `backend/council.py`
- `backend/storage.py`

Primary mechanism:

```text
user query
   ↓
parallel independent first responses
   ↓
anonymized peer evaluation/ranking
   ↓
aggregate ranking metadata
   ↓
chairman synthesis
```

The project is explicitly described by Karpathy as a small experimental/vibe-coded tool, not a maintained production framework.

## `karpathy/rendergit`

Pinned commit:

`14d7a58c0f4d815a3447f25e9ef1088c7e9ade84`

Reviewed:

- `README.md`
- repository structure
- CXML/LLM-view mechanism in `rendergit.py`

Primary mechanism:

```text
one repository source
      ↓
representation transform
      ├─ human-oriented browsable view
      └─ model-oriented CXML/text view
```

## `karpathy/reader3`

Pinned commit:

`64960f99d8d3c2eaaec56c6765b4c1aeae14c80b`

Reviewed:

- `README.md`

Primary mechanism:

```text
whole book
   ↓
chapter-sized semantic unit
   ↓
human chooses what to expose to the model
```

---

# 1. LLM Council is a concrete fan-out/fan-in implementation

`llm-council` gives direct implementation evidence for a three-stage fan-out/fan-in topology.

## Stage 1 — independent first opinions

`stage1_collect_responses()` sends the same user query to all configured council models in parallel before exposing any model to another model's answer.

HE significance:

- preserves initial path independence better than sequential critique chains;
- makes disagreement visible before convergence;
- parallelizes breadth rather than mutation;
- creates raw candidate artifacts that can be inspected independently of the synthesis.

**Disposition: STRONGLY REINFORCE independent fan-out where diversity has information value.**

## Stage 2 — anonymized peer evaluation

`stage2_collect_rankings()` relabels first-stage outputs as `Response A`, `Response B`, etc. before asking the council models to evaluate and rank them.

This is useful because model/provider identity is intentionally removed from the judging prompt.

HE interpretation:

> **Blind or identity-reduced evaluation can be a bias-control mechanism when evaluator expectations about the producer are irrelevant to the property being judged.**

It is not proof that the ranking is objectively correct. The evaluators are still models, share the same candidate set, and may share style/preferences or common failure modes.

**Disposition: REINFORCE anonymization as an optional bias-control technique; REJECT model ranking as ground truth.**

## Stage 3 — chairman synthesis

The designated chairman receives:

- all Stage 1 responses;
- all Stage 2 peer evaluations/rankings;
- the original user question.

It then synthesizes one final answer.

This is a direct implementation of:

> explore widely, adjudicate narrowly.

But the chairman is a synthesizer, not an independent acceptance authority. For factual, security, code-correctness, or other consequential claims, external evidence still outranks council consensus.

**Disposition: REINFORCE synthesis/acceptance separation.**

---

# 2. Preserve fan-out artifacts through fan-in

A particularly useful detail appears in `backend/storage.py`.

Assistant messages retain the stages separately:

```text
stage1 = individual model outputs
stage2 = peer evaluations/rankings
stage3 = final synthesis
```

This matters because the system does not preserve only the final chairman answer.

HE implication:

> **Fan-in should not destroy the evidence needed to audit how the synthesis was produced.**

Useful retained information may include:

- worker identity or role;
- worker scope;
- raw claim/candidate;
- evidence pointer;
- disagreement;
- evaluator observations;
- final synthesis.

This does not mean every full transcript must be retained forever. The principle is to preserve enough provenance for consequential fan-in decisions to remain inspectable.

**Disposition: STRONGLY REINFORCE provenance-preserving fan-in.**

---

# 3. Aggregate ranking is useful metadata, not truth

The implementation parses each evaluator's ordered list and computes mean ranking position per candidate.

That produces a convenient comparative signal.

Risks:

- correlated evaluators can amplify a shared blind spot;
- eloquent or familiar answers may rank above correct but terse ones;
- all evaluators are exposed to the same candidate set;
- averaging positions discards some disagreement structure;
- consensus is not executable evidence.

HE interpretation:

> **Consensus metrics may summarize evaluator opinion, but they should not silently become the acceptance mechanism for properties that can be tested or independently evidenced.**

**Disposition: ASSESS as metadata; REJECT majority/average rank as correctness proof.**

---

# 4. Fan-out independence can be intentionally staged

The council topology creates two different independence conditions:

```text
Stage 1: workers do not see each other's answers
Stage 2: evaluators see all candidates but not producer identity
```

That is a useful distinction.

HE should avoid treating “multiple agents” as automatically independent. Independence can be designed around the failure mode being reduced:

- hide peer conclusions during generation to reduce anchoring/path dependence;
- hide producer identity during evaluation to reduce identity/prestige bias;
- separate evaluators from mutators when self-approval is a concern;
- use external tests/evidence when correlated model judgment is insufficient.

**Disposition: REINFORCE deliberate independence design.**

---

# 5. `rendergit` separates canonical source from consumer-specific presentation

`rendergit` creates two views from the same repository:

- **Human View:** navigation, syntax highlighting, Markdown rendering, file sizes and directory structure;
- **LLM View:** flattened CXML/text intended for model consumption.

This suggests a broader HE pattern:

```text
canonical artifact
      ↓
consumer-specific renderer / adapter
      ├─ human representation
      └─ model representation
```

The key is that the representation changes while source authority does not.

### Candidate principle

> **Adapt representation to the consumer without creating a second source of truth.**

This is relevant to:

- dashboard human vs machine-readable views;
- status/doctor JSON plus human output;
- code/context adapters;
- review artifacts;
- evidence bundles.

**Disposition: STRONGLY REINFORCE shared collectors/canonical state with multiple renderers.**

---

# 6. Whole-repository flattening is situational, not a general context strategy

`rendergit` deliberately supports copying an entire filtered repository into model context.

That can be effective for a sufficiently small codebase and a task requiring broad structural inspection.

It conflicts with HE Progressive Disclosure when used indiscriminately on larger repositories because it can:

- consume context before relevance is known;
- dilute important local evidence;
- increase repeated input cost;
- make freshness harder if the flattened artifact is reused;
- duplicate information the model could retrieve selectively.

The project itself applies some practical filtering such as skipping binaries/oversized files, which is evidence that even a flattening tool benefits from admission control.

HE interpretation:

> **Choose flattening versus retrieval based on artifact size, task breadth, freshness requirements and measured model behavior—not ideology.**

**Disposition: ASSESS for small bounded artifacts; REJECT whole-repo injection as the default context architecture.**

---

# 7. `reader3` demonstrates human-controlled semantic chunking

`reader3` intentionally presents a book chapter by chapter so a human can choose a chapter to share with an LLM.

This is a very simple form of progressive disclosure:

```text
large source
   ↓
meaningful semantic unit
   ↓
selective exposure
```

The interesting part is not EPUB support. It is the use of a natural semantic boundary rather than arbitrary token slicing.

HE implication:

> **When a source already has meaningful structural units, retrieval/admission should prefer those units when practical rather than flattening or chunking blindly.**

Examples may include:

- document section;
- code file/function/module;
- test failure;
- issue/PR thread;
- run/experiment;
- memory topic;
- decision record.

**Disposition: REINFORCE semantic-unit progressive disclosure.**

---

# 8. `rendergit` and `reader3` expose an important tension

These two small projects use opposite context strategies:

```text
rendergit → flatten broadly for global inspection
reader3   → expose one semantic slice at a time
```

Neither is universally right.

This is useful HE evidence because it argues against one-size-fits-all context doctrine.

A harness should choose an admission strategy based on the task:

- **global architecture/code review:** broader representation may help;
- **localized implementation/debugging:** retrieve narrowly;
- **long source consumption:** semantic slices may be superior;
- **tiny repository:** full flattening may be cheaper than sophisticated retrieval.

### Candidate principle

> **Context admission strategy should follow task topology and source scale.**

**Disposition: STRONGLY REINFORCE evidence-driven context shape.**

---

# 9. “Code is ephemeral” is a useful pressure test, not an HE doctrine

Both `llm-council` and `reader3` explicitly describe themselves as vibe-coded, minimally supported experiments. Karpathy frames some generated utility code as cheap enough to recreate or modify with an LLM rather than maintain as a traditional library.

HE should take this seriously as an economic shift without overgeneralizing it.

When implementation cost drops, the break-even point changes between:

```text
maintain a generalized reusable framework
vs.
generate/modify a narrow local tool
```

But low creation cost does not remove:

- security review;
- provenance/licensing;
- dependency risk;
- regression risk;
- operational ownership;
- durable compatibility requirements;
- organizational support obligations.

### Candidate principle

> **Cheap code generation lowers the value of speculative generalization, but does not eliminate lifecycle ownership for durable systems.**

This reinforces the existing HE finding that cognitive complexity and maintenance burden are real harness costs.

**Disposition: ASSESS local-tool economics; REJECT “libraries are over” as a general architecture rule.**

---

# 10. Cross-project implications

## HE

Strong additions:

- blind/identity-reduced evaluation as a possible bias-control mechanism;
- preserve raw fan-out artifacts through synthesis;
- distinguish consensus metadata from acceptance evidence;
- adapt representation to consumer without duplicating authority;
- use semantic source structure for progressive disclosure where practical;
- choose flattening versus retrieval by source/task shape;
- recognize generated local tools as a changing reuse/maintenance tradeoff.

## ACR

Potential later audit questions:

- Are independent reviewer findings preserved before synthesis?
- Would reviewer identity masking reduce anchoring/prestige effects during opposition or adjudication?
- Does fan-in retain disagreement and evidence pointers, or flatten everything into one homogeneous list?
- Are consensus/duplicate counts being mistaken for correctness?

No immediate implementation change is justified before AACR-Bench/evidence-quality work establishes the evaluation baseline.

## PMB / work MB

No reason to add whole-repository flattening or council behavior.

The more relevant lesson is representation/admission:

- authoritative memory remains canonical;
- human and machine renderers may differ;
- retrieval should prefer meaningful semantic units;
- broad context injection should be justified by the task rather than convenience.

## Dashboard / Cockpit

Directly relevant pattern:

> one deterministic collector/state model → multiple renderers.

A future cockpit should not make the UI its source of truth. Machine-readable observations and human presentation should be separate views over canonical collected state.

---

# Research disposition

## STRONGLY REINFORCE

- independent first-pass fan-out when diversity matters;
- provenance-preserving fan-in;
- synthesis authority separate from acceptance authority;
- deliberate independence/bias-control boundaries;
- canonical state with consumer-specific renderers;
- semantic-unit progressive disclosure;
- task/source-scaled context admission.

## ASSESS

- anonymized/blind reviewer evaluation in ACR or opposition review;
- consensus/rank metadata as a secondary signal;
- whole-repo flattening for small repos or global inspection tasks;
- local disposable-tool economics versus durable framework maintenance.

## PARK

- a general-purpose HE council framework;
- persistent multi-model voting infrastructure;
- automatic repo flattening in PMB/ACR.

## REJECT

- council consensus as truth;
- chairman synthesis as independent proof;
- multiple models as automatic independence;
- whole-repo context injection as universal best practice;
- “code is ephemeral/libraries are over” as a general production rule.

---

# Bottom line

The deeper Karpathy GitHub pass adds three useful HE mechanisms:

1. **LLM Council:** fan out independently, reduce selected evaluator bias, preserve the branches, then synthesize—without confusing consensus with proof.
2. **rendergit:** keep one canonical source but render it differently for humans and models.
3. **reader3:** use meaningful semantic units for progressive disclosure instead of blindly loading the whole source.

Together they reinforce a broader direction:

> **Preserve source authority and evidence, shape context for the consumer and task, and only converge independent work after its provenance remains inspectable.**
