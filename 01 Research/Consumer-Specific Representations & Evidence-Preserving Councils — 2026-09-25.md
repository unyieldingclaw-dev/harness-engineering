# Consumer-Specific Representations & Evidence-Preserving Councils — 2026-09-25

## Purpose

Synthesize HE implications from the deeper Andrej Karpathy GitHub pass covering `llm-council`, `rendergit`, and `reader3`.

Primary evidence:

- `01 Research/Sources/Andrej Karpathy — LLM Council, Rendergit & Reader3 — 2026-09-25.md`

Related HE research:

- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Behavioral Invariants, Declarative Goals & Bounded Autonomous Loops — 2026-09-25.md`
- `01 Research/Context Engineering.md.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`

This is research synthesis only. It does not authorize new orchestration infrastructure or project changes.

---

# Executive synthesis

Three small Karpathy projects expose a useful combined architecture:

```text
canonical source / task
        ↓
consumer-appropriate representation
        ↓
independent bounded workers when diversity helps
        ↓
preserve branch-level evidence
        ↓
structured synthesis
        ↓
separate acceptance evidence when consequence requires it
```

The central finding is:

> **Optimize representation and convergence without discarding source authority, independence, or provenance.**

---

# 1. Fan-out should preserve pre-convergence evidence

The most important `llm-council` lesson is not “use multiple models.”

It is that independent first-pass outputs exist as inspectable artifacts before synthesis.

That matters because convergence can erase useful differences:

- minority evidence;
- disagreement;
- alternate hypotheses;
- unique source paths;
- uncertainty;
- reviewer-specific observations.

### Candidate principle

> **Do not let fan-in erase the evidence that made fan-out valuable.**

A useful fan-in contract should preserve enough structure to reconstruct what each worker contributed and where disagreement remained.

This strengthens the existing HE principle:

> explore widely, adjudicate narrowly; preserve provenance through the fan-in boundary.

---

# 2. Independence should be engineered against a specific bias

`llm-council` uses two different techniques:

- workers answer independently before seeing peer answers;
- evaluators see the candidates but not producer identity.

These target different failure modes:

```text
hidden peer outputs   → reduces anchoring/path dependence
hidden producer label → reduces identity/prestige bias
```

This leads to a stronger HE framing:

> **Independence is not a binary property. Design which information is withheld or shared according to the bias or coupling being controlled.**

Possible dimensions include:

- peer conclusions;
- producer identity;
- shared retrieved evidence;
- mutation state;
- prior reviewer scores;
- user preference signals.

Do not add artificial isolation when shared context is actually required.

---

# 3. Consensus is metadata, not acceptance

Average rank, majority preference, or cross-model agreement can be useful signals.

They are not automatically evidence that a claim is true or a change is correct.

A council can agree because:

- models share training priors;
- they favor similar style;
- they all missed the same edge case;
- the candidate evidence is incomplete;
- the task is inherently subjective.

### Candidate principle

> **Use consensus to summarize judgment; use task-relevant evidence to establish acceptance.**

For subjective work, consensus may be part of the quality signal.
For code, security, runtime behavior, compliance, or other testable properties, executable/authoritative evidence should dominate.

---

# 4. Synthesis authority and acceptance authority remain separate

A strong chairman/synthesizer can:

- deduplicate;
- reconcile terminology;
- expose conflicts;
- combine complementary evidence;
- produce a readable final artifact.

But synthesis quality does not make the synthesizer the final authority for every domain.

### Candidate principle

> **A synthesizer may own convergence without owning truth.**

This aligns directly with HE's producer/evaluator/acceptance distinctions.

---

# 5. One source can support multiple valid representations

`rendergit` demonstrates a simple but important adapter pattern:

```text
repository source
    ├─ human-oriented view
    └─ model-oriented view
```

The source remains canonical. The renderer changes according to the consumer.

This generalizes cleanly:

```text
canonical observations
    ├─ statusline
    ├─ JSON API
    ├─ dashboard cards
    └─ LLM summary input
```

or:

```text
canonical project state
    ├─ human documentation
    └─ machine-readable contract
```

### Candidate principle

> **Separate source authority from presentation format.**

This reinforces the dashboard finding: reuse collectors/state and add renderers rather than reimplementing truth for each surface.

---

# 6. Representation can be optimized for the consumer without changing truth

Human users benefit from:

- navigation;
- hierarchy;
- syntax highlighting;
- visual grouping;
- concise summaries.

Models may benefit from:

- explicit delimiters;
- stable machine-readable structure;
- less decorative noise;
- clear file/source boundaries;
- normalized metadata.

This suggests a useful design rule:

> **Transform presentation aggressively; transform authority conservatively.**

A renderer/adapter may change shape, but should not silently reinterpret, summarize away, or overwrite source truth unless that transformation is explicit and its lossiness understood.

---

# 7. Context admission should follow task topology

`rendergit` and `reader3` are valuable partly because they take opposite approaches:

- `rendergit`: broad flattening for global code inspection;
- `reader3`: one semantic slice at a time for long-form reading.

The useful HE conclusion is not that one wins.

### Candidate principle

> **Choose context breadth according to source scale and task topology.**

Examples:

- global dependency/architecture question → broader source view may be justified;
- localized bug → retrieve relevant files/tests only;
- long document analysis → section/chapter-level semantic retrieval;
- tiny repository → full source may be cheaper than retrieval machinery;
- frequently changing repository → live retrieval may beat cached flattening.

This is Progressive Disclosure with an explicit exception for genuinely global tasks.

---

# 8. Prefer semantic units over arbitrary chunks when structure exists

`reader3` uses chapters because the source already contains a meaningful boundary.

The same principle can apply elsewhere:

- function/module instead of token window;
- test failure instead of terminal transcript block;
- PR/issue comment thread instead of arbitrary message slices;
- experiment/run instead of log byte range;
- decision record instead of generic memory chunk.

### Candidate principle

> **Use native semantic boundaries for retrieval/admission when they align with the reasoning task.**

Token windows remain a fallback when semantic structure is unavailable or too large.

---

# 9. Cheap generated tools change reuse economics

Karpathy's small recent utilities repeatedly accept a different maintenance posture than traditional shared libraries: build a narrow tool quickly, use it, modify/regenerate it when needed, and do not necessarily turn it into a maintained framework.

HE should treat this as a change in the economics of software creation, not as proof that durable software is obsolete.

A lower creation cost makes these questions more important:

```text
Is this capability local or shared?
Is the interface stable?
Does it carry security/operational risk?
Will many users/processes depend on it?
Is compatibility part of the contract?
Would regeneration be cheaper than maintaining generality?
```

### Candidate principle

> **The cheaper implementation becomes, the more carefully reuse and generalization must justify their lifecycle cost.**

This complements the existing finding that cheap implementation increases the importance of scope discipline.

---

# 10. ACR implications

No immediate implementation change.

Potential later experiments after the evidence-quality baseline stabilizes:

- preserve reviewer output/evidence before orchestration merges findings;
- measure findings lost or distorted during fan-in;
- test blind reviewer adjudication where producer/model identity is irrelevant;
- retain disagreement as structured metadata rather than forcing early consensus;
- distinguish reviewer consensus from verified defect evidence.

Possible measurements:

- unique finding contribution per reviewer;
- evidence retention through synthesis;
- disagreement rate;
- false-positive rate for consensus vs non-consensus findings;
- adjudication changes under blinded vs identified producer labels;
- duplicate-collapse errors.

Do not add a council layer merely because Karpathy has one.

---

# 11. PMB / work MB implications

No new memory artifact is justified.

The relevant design lessons are:

- keep authoritative memory canonical;
- let human and machine representations differ when useful;
- prefer semantic retrieval units;
- widen context only when the task genuinely needs broad structure;
- do not turn derived summaries or flattened exports into a competing source of truth.

This reinforces PMB's separation between durable project truth and derived continuation/context artifacts.

---

# 12. Dashboard / Cockpit implications

This research strongly reinforces the future cockpit boundary:

```text
source adapters
      ↓
canonical observations/state
      ↓
multiple renderers
      ├─ machine-readable API
      ├─ compact statusline
      ├─ human dashboard
      └─ optional LLM narrative
```

The LLM narrative should remain derived presentation, not raw-state authority.

### Candidate principle

> **Collect once, normalize once, render many ways.**

That reduces drift and duplicate logic.

---

# Research disposition

## STRONGLY REINFORCE

- preserve branch-level evidence through fan-in;
- engineer independence around specific biases/couplings;
- separate consensus from acceptance;
- separate synthesis authority from truth/acceptance authority;
- canonical source with multiple consumer-specific renderers;
- task-scaled context breadth;
- semantic-unit progressive disclosure;
- lower implementation cost increases scrutiny of generalization/lifecycle cost.

## ASSESS

- blinded adjudication in ACR/opposition review;
- structured disagreement retention;
- whole-repo/model-oriented renderers for small/global tasks;
- context breadth selection heuristics;
- disposable/local tool economics for one-owner workflows.

## PARK

- generic council framework in HE;
- model voting infrastructure;
- automatic context flattening service;
- universal human/LLM dual-renderer standard.

## REJECT

- majority vote as truth;
- multiple agents as automatic independence;
- chairman synthesis as acceptance evidence;
- whole-source flattening as default Progressive Disclosure;
- derived representation becoming a competing source of truth;
- “libraries are over” as general production guidance.

---

# Candidate HE principles — research status

> **Do not let fan-in erase the evidence that made fan-out valuable.**

> **Independence is not binary; design information sharing around the bias or coupling being controlled.**

> **Use consensus to summarize judgment; use task-relevant evidence to establish acceptance.**

> **A synthesizer may own convergence without owning truth.**

> **Separate source authority from presentation format.**

> **Choose context breadth according to source scale and task topology.**

> **Use native semantic boundaries for retrieval/admission when they align with the reasoning task.**

> **The cheaper implementation becomes, the more carefully reuse and generalization must justify lifecycle cost.**

These remain research candidates pending corroboration and/or local evaluation.

---

# Bottom line

The deeper Karpathy pass adds a useful counterweight to simplistic “more context” or “more agents” advice.

The better pattern is:

```text
right-sized context
+ consumer-appropriate representation
+ independent exploration where useful
+ preserved evidence
+ narrow synthesis
+ separate acceptance
```

That is a strong fit with HE's existing direction and does not require another framework to get the benefit.
