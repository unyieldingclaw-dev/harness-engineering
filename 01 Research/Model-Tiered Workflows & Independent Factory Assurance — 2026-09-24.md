# Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24

## Purpose

Synthesize the durable Harness Engineering implications from model-mixing, decision-model and AI-centered R&D research without adopting a specific software-factory stack, model choice, decision service or control plane.

Primary sources:

- `01 Research/Sources/Cole Medin — Model Mixing, AI Software Factory & GitHub Ecosystem — 2026-09-24.md`
- `01 Research/Sources/CoderOne — Jev-Style Fine-Tuning, Decision Models & Evaluation Isolation — 2026-09-26.md`
- `01 Research/Sources/Prompt Engineering — Bonsai 2 Compression, Agentic Horizon & Trace Failure — 2026-09-28.md`
- `01 Research/Sources/Stacked Podcast — Naive-N0.5-Flash, AI-Centered R&D & Capability Step-Down — 2026-09-28.md`

Related HE research:

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Risk-Directed Review & Release Proof — 2026-09-24.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`
- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`

This is research. It does **not** authorize a model router, software factory, autonomous merge pipeline, custom classifier, Jev/Laya adoption, recursive self-improvement loop or PMB redesign.

---

# Executive synthesis

The useful pattern is not "large model plans, small model codes."

The stronger pattern is:

```text
workload class
    +
required judgment
    +
output variability / contract
    +
verification strength
    +
consequence / reversibility
    +
measured mechanism fitness
        ↓
inference mechanism + model + effort choice
```

That selection can vary by stage, but stage names are only proxies for the actual capability requirement.

The Jev/Jef/Tev research adds an important refinement: the correct substitution may not be a cheaper general model. For some workloads the better fit is a narrower decision model or conventional classifier that does not perform open-ended generation at all.

The Bonsai/NaiveAI follow-on adds another refinement: capability reduction is often **non-linear and failure-mode-specific**. “Move down one model tier” is not a predictable small percentage decrement. A cheaper/compressed configuration can preserve one-shot quality while losing long-horizon control.

Separately, unattended or semi-autonomous delivery becomes credible only when the producer cannot define, weaken, inspect when harmful, or self-certify the acceptance boundary.

The combined HE direction is therefore:

> **Use the least-general verified mechanism that satisfies the workload, while keeping acceptance authority, negative tests, scope boundaries, and consequential gates outside the producer.**

---

# 1. Model tiering should be evidence-based, not role-hardcoded

A fixed rule such as:

```text
planner = frontier
builder = small
reviewer = frontier
```

is attractive because it is simple, but it will be wrong for some work.

Examples:

- a routine CRUD implementation with strong tests may be safe for a smaller model;
- a tricky data migration may require frontier-level implementation judgment;
- a highly constrained plan for a tiny known codebase may not require the strongest model;
- a deterministic review pass should not consume a model at all.

### Candidate principle

> **Route by demonstrated workload fitness, not by model reputation or stage label.**

A model/effort profile should be evaluated against representative real tasks and revalidated after material model/runtime changes.

**Disposition: STRONGLY REINFORCE.**

---

# 2. Planning can compress judgment into a cheaper execution stage — when the plan is actually good

Cole's experiments support a useful hypothesis: a strong planning artifact can reduce the amount of open-ended judgment required during implementation.

That does **not** mean implementation becomes mechanical by definition.

The effect depends on the plan containing enough verified context to constrain the builder:

```text
objective
relevant current state
explicit constraints
files/components likely involved
acceptance semantics
known risks
verification path
```

If the plan omits a critical design decision, integration hazard or safety invariant, the implementation model must recover that judgment itself.

### HE implication

> **A planning artifact is valuable partly because it transfers judgment forward; its quality determines how safely later stages can use cheaper capability.**

This gives artifact quality an operational consequence beyond documentation.

**Disposition: ASSESS through task evals.**

---

# 3. Cost/rate-limit optimization belongs after correctness boundaries

The temptation is to optimize the most token-heavy stage first.

The safer ordering is:

```text
1. define required outcome and risk
2. define verification strong enough to detect bad execution
3. measure candidate model/effort/mechanism combinations
4. choose the lowest-cost configuration that preserves required behavior
5. revalidate after model/runtime changes
```

Do not start with price and retrofit acceptance afterward.

### Candidate principle

> **Capability substitution is safe only to the degree that the surrounding verification can detect the substituted mechanism's failure modes.**

This connects routing directly to harness quality.

**Disposition: REINFORCE.**

---

# 4. Independent assurance needs authority separation, not merely another model call

A separate reviewer helps, but a producer can still undermine the process if it can modify the test, threshold, policy or holdout that decides acceptance.

The stronger pattern is:

```text
producer
   ↓ candidate
independent verifier / deterministic checks
   ↓ evidence
acceptance authority
```

with the producer unable to silently weaken the verifier's rules.

### Candidate principle

> **The component under evaluation should not be able to loosen the acceptance mechanism that evaluates it.**

This does not require immutable tests. It requires explicit authority for weakening or changing acceptance semantics.

**Disposition: STRONGLY REINFORCE.**

---

# 5. Green verification should itself be tested

A verifier that only ever sees correct implementations is uncalibrated.

The factory's mutation pattern suggests a general HE rule:

```text
known-good candidate  → should pass
known-bad candidate   → should fail for the intended reason
wrong candidate       → should not be mistaken for valid evidence
```

This is stronger than merely running the test suite.

### Candidate principle

> **Calibrate the judge, not just the worker.**

Examples:

- deliberately broken runtime behavior must fail the runtime verifier;
- an evidence checker must reject a known unsupported claim;
- a review benchmark should include clean cases and seeded defects;
- a deployment health check must prove it is reading the deployed revision rather than a stale service.

**Disposition: STRONGLY REINFORCE.**

---

# 6. Evidence needs candidate identity

Verification results are only meaningful when we know what artifact produced them.

Useful identity may include:

- commit SHA;
- worktree/branch;
- build identifier;
- deployment revision;
- model/runtime/version for behavioral evals;
- exact fixture version;
- training/quantized artifact identity when relevant.

### Candidate principle

> **Evidence must identify the artifact/configuration it verifies.**

A fresh test result against the wrong code is stale evidence wearing a new timestamp.

**Disposition: REINFORCE.**

---

# 7. Holdout verification is a specialized tool, not a universal requirement

Independent scenarios hidden from the producer can detect overfitting to visible acceptance tests.

Use holdouts when:

- the producer has broad autonomy;
- merge/release may happen without a human reading the diff;
- visible tests are easy to game;
- behavioral evaluation is important enough to justify independent scenarios.

Avoid holdout machinery when ordinary deterministic checks plus independent review already provide adequate evidence.

### Candidate principle

> **Use verifier-only evidence only when producer visibility materially weakens the test.**

**Disposition: ASSESS.**

---

# 8. Non-goals are an authority boundary for autonomous work

A task can be locally reasonable and globally wrong for the product.

For more autonomous execution, a durable scope contract benefits from both:

```text
what may change / what outcome is wanted
        +
what must not become true
```

Permanent non-goals and hard invariants provide the latter.

### Candidate principle

> **Autonomous scope needs explicit negative boundaries when plausible adjacent work would otherwise look valid.**

Do not confuse:

- permanent non-goal;
- deferred backlog item;
- unresolved design choice.

Those have different authority semantics.

**Disposition: REINFORCE bounded authority; ASSESS where autonomy expands.**

---

# 9. Workflow engines are optional implementations of a useful architecture

Archon demonstrates a clean separation:

```text
workflow state / dependencies / gates
        +
AI nodes for judgment/generation
        +
deterministic nodes for deterministic facts
        +
worktree isolation
        +
human gates when required
```

HE should preserve the architecture without concluding that a workflow engine is required.

For many repos, Superpowers + PMB + CI + explicit handoff may already provide enough structure.

### Candidate principle

> **Prefer deterministic lifecycle ownership, but use the lightest mechanism that reliably owns it.**

**Disposition: REINFORCE restraint.**

---

# 10. Implication for PMB

No immediate implementation.

Potential later questions after the pilot:

- Does PMB preserve enough authoritative state for model-tier handoff without duplicating plans/specs?
- Could real pilot sessions seed workload-class behavioral evals?
- Are stop reasons and continuation state strong enough for bounded cross-model handoff?
- Does PMB need stronger negative-scope representation, or do current task contracts/project docs already own it?
- Is there an observed capture/curation failure that justifies comparing `claude-memory-compiler`?

Do **not** add:

- model routing fields;
- token budgets PMB cannot enforce;
- hidden holdout state;
- automated transcript extraction;
- Archon integration;
- decision-model infrastructure;

without observed need.

---

# 11. Implication for ACR

ACR is a natural testbed for several mechanisms because it already has calibration and multiple models.

Possible future experiments:

1. classify ACR workloads by semantic difficulty rather than route all reviews identically;
2. compare a strong reviewer + cheaper evidence/formatting stages where deterministic boundaries exist;
3. expand known-negative verifier calibration;
4. ensure every benchmark result records exact model/runtime/configuration identity;
5. preserve clean cases to detect noise/fabrication regressions;
6. inspect benchmark fixture families for group/derivation leakage before claiming a clean holdout;
7. assess whether any repeated narrow typed decision inside ACR would benefit from a decision-model-shaped mechanism before adding another general LLM call;
8. if aggressive compression is tested, include enough long/repeated review behavior to detect state/control degradation rather than relying only on single findings.

Do not add a generic model router or decision model merely because cost pressure exists.

---

# 12. Route by inference class before routing by model tier

The Jev/Jef/Tev research makes a previously implicit distinction explicit.

Some workloads do not need open-ended text generation at all.

A useful routing ladder is:

```text
Does the task require open-ended generation,
changing instructions, or explanation?
        |
       yes
        ↓
generative / fine-tuned LLM

       no
        |
Do questions/options vary while output remains
a bounded typed decision?
        |
       yes
        ↓
decision-model-shaped mechanism

       no
        |
Is the output ontology stable and repeatedly labelled?
        |
       yes
        ↓
fixed classifier / deterministic mechanism where possible
```

This is not a claim that Jev, Laya or any particular classifier is automatically better. It is a mechanism-selection question.

### Candidate principle

> **Use the least-general inference mechanism that still matches the variability, output contract and evidence requirements of the task.**

This is stronger than “use the smallest model” because a smaller generative model may still be unnecessarily general for a fixed decision problem.

**Disposition: NEW / STRONGLY MINE.**

---

# 13. Evaluation isolation should follow the leakage boundary

CoderOne's experiment improves on the upstream development evaluation in two useful ways:

1. it keeps complete `group_id` families on only one side of the train/holdout split;
2. the untouched final holdout stays local and is not uploaded to the training environment.

The first addresses derivation leakage. The second addresses evaluation exposure.

This generalizes Ponytail's treatment-contamination lesson:

```text
control must not inherit treatment
trainer must not see final holdout
producer must not silently weaken verifier
optimizer must not consume final benchmark feedback
```

### Candidate principles

> **The split boundary must match the leakage boundary.**

> **Evaluation isolation should be structural when practical, not merely procedural.**

Structural isolation is not required for every test. Use it when exposure would materially weaken the evidence.

**Disposition: NEW / STRONGLY MINE.**

---

# 14. Structural output guarantees and semantic quality are separate evidence

CoderOne's evaluator constrains vLLM generation to valid option letters. Therefore a 0% invalid-output rate is partly a runtime/schema guarantee, not evidence that the model learned perfect output behavior.

The distinction is even more important for native typed decision models. A September 2026 controlled study of Jev and Jev-like models demonstrated that a 0% type-error rate can coexist with large semantic decision failures under option-name/rubric interventions.

This yields two HE rules:

> **Attribute observed behavior to the layer that actually guarantees it.**

> **Schema/type validity is a structural property; semantic correctness requires separate evidence.**

A type-safe system can eliminate an entire class of malformed outputs while still choosing the wrong legal answer.

For consequential typed decisions, evals should test semantic invariants and adversarial label/rubric conditions where relevant, not merely schema validity.

**Disposition: NEW / STRONGLY MINE.**

---

# 15. Capability step-down is an empirical search, not a linear ladder

The Stacked Podcast describes a useful cost-control method:

```text
prove the task with a strong model
        ↓
try a cheaper candidate
        ↓
measure whether required quality survives
        ↓
continue downward while it does
```

The durable mechanism is good. The implied smoothness is not.

There is no general reason to expect a sequence such as frontier -> mid-tier -> small/open model to lose a predictable 1–3% of useful capability at each step.

Bonsai 2 is a concrete counterexample: broad short-form benchmark retention can remain near parity while long-horizon agentic capability drops much more sharply.

### Candidate principle

> **Capability step-down is a measured search over exact configurations; do not assume degradation is smooth, monotonic or evenly distributed across failure modes.**

A safe search therefore needs workload-specific gates rather than one scalar “accuracy” number.

**Disposition: NEW / STRONGLY MINE.**

---

# 16. Failure tolerance constrains how far substitution can go

The Stacked discussion correctly emphasizes defining acceptable failure before optimizing cost.

A useful order is:

```text
consequence / reversibility
        ↓
important failure modes
        ↓
verification / human oversight strength
        ↓
acceptable error / uncertainty boundary
        ↓
model/mechanism cost search
```

This prevents a cheaper configuration from being selected on average accuracy while failing catastrophically on a rare property the deployment actually cares about.

### Candidate principle

> **Cost substitution is bounded by the failures the surrounding system can tolerate and reliably detect.**

**Disposition: STRONGLY REINFORCE.**

---

# 17. AI-centered R&D can scale execution without transferring acceptance authority

NaiveAI provides a useful industrial example of AI performing much of an experiment loop while human researchers retain direction, constraints, evaluation criteria and critical decisions.

For the NaiveRT runtime, the company reports 151 documented optimization trials:

```text
63 adopted
71 failed validation or rolled back
17 alternatives / prototypes
```

AI models performed profiling, implementation, tests, numerical validation and result analysis. Candidate changes still had to satisfy correctness gates and end-to-end performance criteria.

### HE interpretation

The transferable pattern is not “recursive self-improvement.”

It is:

```text
human / external owner defines objective + constraints + evaluator
        ↓
AI executes a broad experiment search
        ↓
empirical evidence
        ↓
keep / revise / revert
```

### Candidate principle

> **Automation depth can increase without transferring ownership of the objective or acceptance boundary to the optimizer.**

This is consistent with Autoresearch and HE's existing bounded-autonomy direction.

**Disposition: NEW EVIDENCE / STRONGLY REINFORCE.**

---

# 18. End-to-end objectives outrank local optimization wins

NaiveAI documents cases where isolated kernel/microbenchmark improvements did not improve the full model path, and changes were rejected even when numerically correct.

This gives routing/optimization work a useful reminder:

> **A local stage win only matters if the downstream workflow consumes that property.**

Examples in HE include:

- faster model turn but more retries;
- cheaper reviewer but more fabricated findings;
- faster kernel but slower full-model path;
- fewer prompt tokens but worse task completion;
- smaller model file but worse long-agent control.

### Candidate principle

> **Optimize the end-to-end verified property; use local metrics to diagnose, not to redefine success.**

**Disposition: NEW / STRONGLY MINE.**

---

# 19. Adoption and price statistics retain their sampling frame

Vercel reports that open-weight models carried 56% of **Vercel AI Gateway** token volume in August 2026.

That is meaningful evidence about that gateway's production population. It is not a global census of all AI inference.

Likewise, a provider's current token price says little without workload mix, cache behavior, provider route and date.

### Candidate principle

> **Do not generalize a platform-specific adoption or cost statistic beyond the population and execution conditions that produced it.**

**Disposition: REINFORCE evidence scope.**

---

# Candidate HE principles — research status

> **Route by demonstrated workload fitness, not by model reputation or stage label.**

> **Use the least-general inference mechanism that still matches the variability, output contract and evidence requirements of the task.**

> **A planning artifact transfers judgment forward; its quality determines how safely later stages can use cheaper capability.**

> **Capability substitution is safe only to the degree that surrounding verification can detect the substituted mechanism's failure modes.**

> **Capability step-down is a measured search over exact configurations; do not assume degradation is smooth, monotonic or evenly distributed across failure modes.**

> **Cost substitution is bounded by the failures the surrounding system can tolerate and reliably detect.**

> **The component under evaluation should not be able to loosen the acceptance mechanism that evaluates it.**

> **Calibrate the judge, not just the worker.**

> **Evidence must identify the artifact/configuration it verifies.**

> **Use verifier-only evidence only when producer visibility materially weakens the test.**

> **The split boundary must match the leakage boundary.**

> **Evaluation isolation should be structural when practical, not merely procedural.**

> **Attribute observed behavior to the layer that actually guarantees it.**

> **Schema/type validity is a structural property; semantic correctness requires separate evidence.**

> **Automation depth can increase without transferring ownership of the objective or acceptance boundary to the optimizer.**

> **Optimize the end-to-end verified property; use local metrics to diagnose, not to redefine success.**

> **Do not generalize a platform-specific adoption or cost statistic beyond the population and execution conditions that produced it.**

> **Autonomous scope needs explicit negative boundaries when plausible adjacent work would otherwise look valid.**

> **Prefer deterministic lifecycle ownership, but use the lightest mechanism that reliably owns it.**

---

# Bottom line

The useful response to rate limits and inference cost is not a bigger routing architecture.

It is a measured capability-allocation loop:

```text
classify the work
        ↓
choose the least-general viable mechanism
        ↓
set acceptance semantics + failure tolerance
        ↓
measure exact model/effort/runtime options if a model is needed
        ↓
step down only while required behavior remains inside the boundary
        ↓
independently verify
        ↓
revalidate when the model/runtime/decision contract changes
```

For unattended delivery or automated optimization, stronger assurance is needed only where the autonomy actually requires it: protected acceptance authority, known-negative verifier calibration, candidate identity, bounded repair, explicit negative scope, structural evaluation isolation where contamination would invalidate evidence, and end-to-end acceptance metrics that the optimizer cannot silently redefine.

That keeps model economics subordinate to correctness instead of turning cost optimization into a new control plane.