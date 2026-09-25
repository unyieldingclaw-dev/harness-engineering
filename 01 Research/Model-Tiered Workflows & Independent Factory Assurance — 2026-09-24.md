# Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24

## Purpose

Synthesize the durable Harness Engineering implications from Cole Medin's model-mixing video and current repositories without adopting his specific software-factory stack, model choices, or control plane.

Primary source:

- `01 Research/Sources/Cole Medin — Model Mixing, AI Software Factory & GitHub Ecosystem — 2026-09-24.md`

Related HE research:

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Risk-Directed Review & Release Proof — 2026-09-24.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`

This is research. It does **not** authorize a model router, software factory, autonomous merge pipeline, or PMB redesign.

---

# Executive synthesis

The useful pattern is not "large model plans, small model codes."

The stronger pattern is:

```text
workload class
    +
required judgment
    +
verification strength
    +
consequence / reversibility
    +
measured model fitness
        ↓
model + effort choice
```

That selection can vary by stage, but stage names are only proxies for the actual capability requirement.

Separately, unattended or semi-autonomous delivery becomes credible only when the producer cannot define, weaken, or self-certify the acceptance boundary.

The combined HE direction is therefore:

> **Use the cheapest verified capability that satisfies the workload, while keeping acceptance authority, negative tests, scope boundaries, and consequential gates outside the producer.**

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
3. measure candidate model/effort combinations
4. choose the lowest-cost configuration that preserves required behavior
5. revalidate after model/runtime changes
```

Do not start with price and retrofit acceptance afterward.

### Candidate principle

> **Capability substitution is safe only to the degree that the surrounding verification can detect the substituted model's failure modes.**

This connects model routing directly to harness quality.

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
- exact fixture version.

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

without observed need.

---

# 11. Implication for ACR

ACR is a natural testbed for several mechanisms because it already has calibration and multiple models.

Possible future experiments:

1. classify ACR workloads by semantic difficulty rather than route all reviews identically;
2. compare a strong reviewer + cheaper evidence/formatting stages where deterministic boundaries exist;
3. expand known-negative verifier calibration;
4. ensure every benchmark result records exact model/runtime/configuration identity;
5. preserve clean cases to detect noise/fabrication regressions.

Do not add a generic model router merely because cost pressure exists.

---

# Candidate HE principles — research status

> **Route by demonstrated workload fitness, not by model reputation or stage label.**

> **A planning artifact transfers judgment forward; its quality determines how safely later stages can use cheaper capability.**

> **Capability substitution is safe only to the degree that surrounding verification can detect the substituted model's failure modes.**

> **The component under evaluation should not be able to loosen the acceptance mechanism that evaluates it.**

> **Calibrate the judge, not just the worker.**

> **Evidence must identify the artifact/configuration it verifies.**

> **Use verifier-only evidence only when producer visibility materially weakens the test.**

> **Autonomous scope needs explicit negative boundaries when plausible adjacent work would otherwise look valid.**

> **Prefer deterministic lifecycle ownership, but use the lightest mechanism that reliably owns it.**

---

# Bottom line

The useful response to rate limits is not a bigger routing architecture.

It is a measured capability-allocation loop:

```text
classify the work
        ↓
set acceptance semantics
        ↓
measure model/effort options
        ↓
use the least expensive option that preserves required behavior
        ↓
independently verify
        ↓
revalidate when the model/runtime changes
```

For unattended delivery, add stronger assurance only where the autonomy actually requires it: protected acceptance authority, known-negative verifier calibration, candidate identity, bounded repair, and explicit negative scope.

That keeps model economics subordinate to correctness instead of turning cost optimization into a new control plane.