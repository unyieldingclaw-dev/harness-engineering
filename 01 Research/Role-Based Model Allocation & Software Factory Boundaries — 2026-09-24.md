# Role-Based Model Allocation & Software Factory Boundaries — 2026-09-24

## Purpose

Synthesize the durable Harness Engineering implications from Cole Medin's current model-mixing/software-factory work without turning a temporary model combination or a specific workflow engine into HE architecture.

Primary evidence:

- `01 Research/Sources/Cole Medin — Model Mixing, Archon & Software Factory — 2026-09-24.md`

Related HE research:

- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Risk-Directed Review & Release Proof — 2026-09-24.md`
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`

This note is research synthesis. It does not authorize a new model router, software-factory control plane, Archon installation, PMB change, or ACR redesign.

---

# Executive synthesis

The useful pattern is not `big model plans, small model codes` as a fixed rule.

A stronger model is:

```text
work stage / decision
        ↓
required judgment + risk + ambiguity
        ↓
accepted input artifact / available verification
        ↓
select the least expensive evaluated capability that can satisfy the role
        ↓
execute inside bounded authority
        ↓
produce evidence / artifact
        ↓
independent verification where consequence warrants it
```

This preserves cost/rate-limit leverage without pretending model tiers are interchangeable.

The key idea is **capability allocation by role and evidence**, not model routing as a product feature.

---

# 1. Role selection belongs above provider routing

Existing HE research already separates two layers.

## Inference gateway / provider layer

Owns mechanics such as:

- credentials/endpoints;
- provider protocol translation;
- provider health;
- quota/rate-limit state;
- retries/cooldowns;
- same-model/provider failover;
- raw capability eligibility;
- route diagnostics.

## Harness/workflow layer

Owns semantic questions such as:

- what capability this stage requires;
- whether substitution is allowed;
- which artifact must survive a transition;
- what verification is required;
- whether a cheaper model is acceptable for this role;
- when risk/ambiguity requires escalation.

### Candidate principle

> **The gateway should report what is available; the harness should decide what capability the task is allowed to use.**

This means a provider-neutral gateway can support role-based allocation without becoming the architecture's decision-maker.

**Disposition: STRONGLY REINFORCE existing gateway/harness boundary.**

---

# 2. Start with explicit role policy, not an AI router

A practical first form is boring and inspectable:

```text
planning / high ambiguity       → evaluated high-reasoning configuration
bounded implementation          → evaluated lower-cost configuration
independent semantic review     → evaluated high-reasoning configuration
mechanical checks               → deterministic tools
```

Then add explicit exceptions:

```text
security-sensitive implementation
cross-cutting shared-state change
unfamiliar architecture
runtime debugging with weak evidence
irreversible/high-blast-radius mutation
    → escalate implementation capability
```

The exact model names should remain configuration/evaluation data rather than HE doctrine.

### Candidate principle

> **Prefer explicit, evaluated role policy before adaptive model routing.**

Reasons:

- easier to reproduce;
- easier to attribute regressions;
- easier to audit;
- easier to roll back;
- no additional probabilistic decision-maker;
- less hidden coupling to current model catalogs.

**Disposition: REINFORCE.**

---

# 3. The accepted artifact determines how cheap implementation can safely become

A less-capable builder is plausible when the upstream artifact has removed most consequential ambiguity.

A plan/contract should make clear enough:

- required behavior;
- files/scope or discoverable boundaries;
- invariants / must-not-change behavior;
- success/postconditions;
- relevant constraints;
- verification route;
- unresolved questions that still require judgment.

If those are not settled, lowering builder capability can simply move planning errors into code.

### Candidate principle

> **The cheaper the execution model, the stronger the input contract and verification need to be.**

This is not a demand for more documentation. It is an evidence/ambiguity relationship.

**Disposition: REINFORCE artifact-gated lifecycle.**

---

# 4. Model changes are work-transfer boundaries

When planning, implementation, review, and fixing use different models/providers, the workflow must transfer the state that the next role actually owns.

Prefer:

```text
plan artifact
verification contract
current repository/diff
review findings
runtime/check evidence
```

over copying the prior model's conversation.

A role transition should preserve:

- objective;
- accepted constraints;
- authoritative artifact version;
- unresolved work;
- verification target;
- execution identity/provenance when relevant.

### Existing principle strengthened

> **Historical context informs; current project state governs.**

### Candidate refinement

> **Model specialization works best when roles exchange authoritative artifacts rather than inherited conversational assumptions.**

**Disposition: STRONGLY REINFORCE.**

---

# 5. Evaluation must vary one meaningful thing at a time

Cole's own benchmark playbook documents a prompt-parity confound in one premium baseline. That is useful evidence that model comparisons are easy to contaminate.

A serious HE role-allocation experiment should control:

```text
same real task / starting commit
same tool surface
same project context policy
same role prompt / artifact contract
same deterministic checks
same review/evaluator method
same retry policy
same runtime isolation
verified actual model identity
```

Then vary one dimension:

```text
plan model
implementation model
review model
or effort level
```

### Measurements

Use outcome-oriented evidence:

- task/postcondition success;
- regression failures;
- independent review findings;
- false positive / false negative where labels exist;
- retries/corrections;
- wall time;
- usage/cost when reliably exposed;
- human intervention;
- failure/timeout rate;
- source/model identity.

Do not optimize solely for output-token cost.

**Disposition: STRONGLY REINFORCE behavioral harness evals.**

---

# 6. Reliability matters more than one impressive run

A single visually convincing app build does not establish that a model combination is safe for a workload.

Repeated runs are useful because agent systems are stochastic and failure frequency matters operationally.

For important workload classes, measure repeated success instead of only `pass@1`:

```text
same task/configuration
run multiple fresh attempts
record outcome distribution
```

Use tasks that can reveal hidden-invariant failures, not only easy tasks where every strong model passes.

### Candidate principle

> **A routing policy should be justified by reliability on representative failure modes, not by one successful demo or leaderboard score.**

**Disposition: STRONGLY REINFORCE.**

---

# 7. Cheap builders require bounded correction loops

A strong-review / cheaper-fix loop can save expensive-model tokens, but it can also oscillate or burn resources indefinitely.

A safe loop needs:

- authoritative current attempt;
- allowed mutation scope;
- accepted review finding format;
- maximum iterations/time/cost;
- deterministic verification after changes;
- conflict/ambiguity escalation;
- preservation of unresolved findings;
- no automatic authority expansion merely because a reviewer requested a change.

This is not a new pattern; it is an instance of the existing bounded execution envelope.

### Existing principle strengthened

> **Review/fix automation should converge inside a bounded settlement loop or stop for adjudication.**

**Disposition: STRONGLY REINFORCE.**

---

# 8. A trustworthy factory needs a human-owned perimeter

The most important software-factory finding is not autonomy. It is that some parts of the judge must remain outside the worker's authority.

Examples from the inspected factory experiments include human-owned or protected:

- mission/scope constraints;
- hard invariants;
- quality floors;
- release/merge authority;
- acceptance boundaries;
- decision values that would redefine what passing means.

The worker can improve code and tests inside its scope, but must not be able to make itself pass by weakening the judge.

### Candidate principle

> **An autonomous producer must not control the acceptance boundary that authorizes its own settlement.**

This generalizes beyond software factories to reviewers, ACR fixers, CI generation, and future PMB automation.

**Disposition: STRONGLY REINFORCE authority-locality and independent verification.**

---

# 9. Independent evidence needs independence, not merely a separate prompt

A holdout or reviewer is only meaningfully independent if the producer cannot:

- read the hidden case when secrecy is part of the design;
- rewrite the acceptance criteria;
- disable the check;
- lower the required floor;
- replace the candidate identity being tested;
- silently substitute self-attested evidence.

A folder called `holdout`, a separate agent, or a new context window does not by itself establish independence.

### Candidate principle

> **Independence is an authority property, not a naming or process property.**

**Disposition: STRONGLY REINFORCE.**

---

# 10. Quality floors should be observed before they become governance

The inspected dark-factory experiment contains a useful ratchet concept: protected floors represent values actually observed, and the autonomous worker cannot lower them to compensate for regressions.

The strongest part is its refusal to invent a floor for an end-to-end check that had not actually been run in the required environment.

### HE translation

Do not create pass/fail thresholds because they feel reasonable.

A threshold becomes governance only after there is evidence that:

- the metric/check is meaningful;
- the value was actually observed in the intended environment;
- the consumer/decision is known;
- the worker cannot trivially tune the threshold around itself.

This is directly relevant to the PMB pilot rule against unsupported pass/fail thresholds.

### Candidate principle

> **Measured floors may become ratchets; invented floors are policy theater.**

**Disposition: REINFORCE.**

---

# 11. Deterministic scheduling is preferable when scheduling itself requires no judgment

The mature factory experiment keeps the scheduler deliberately dumb and lets durable issue/run state own lifecycle selection.

That is preferable to using an LLM to decide routine scheduling when the state machine already has enough information.

### Candidate principle

> **Do not spend model judgment on lifecycle decisions that deterministic state can decide.**

This is the scheduling analogue of HE's deterministic-review rule.

**Disposition: REINFORCE.**

---

# 12. Rate limits are a constraint, not architecture justification

Subscription exhaustion is a real operational pressure, but it should not force premature routing infrastructure.

Useful reactions are ordered roughly from least to most architectural:

```text
measure where expensive-model usage actually goes
    ↓
reduce unnecessary context/output
    ↓
tune effort on evaluated workloads
    ↓
move deterministic work out of LLM calls
    ↓
use explicit cheaper role configurations where evals support them
    ↓
use gateway/provider substitution where policy permits
    ↓
consider adaptive routing only if static policy becomes a demonstrated operational problem
```

### Candidate principle

> **Optimize the expensive boundary before building a smarter router.**

**Disposition: STRONGLY REINFORCE restraint.**

---

# 13. HE / PMB / ACR disposition

## HE

Preserve the role-allocation and authority-boundary mechanisms. Do not select a standard model combination or workflow engine.

Useful future eval:

- build a small role/model matrix only when there is a concrete workload and repeatable verification surface;
- record exact execution identity and cost/time;
- include hard/hidden-invariant tasks, not only routine implementations.

## PMB

No implementation change follows directly.

Potential post-pilot relevance:

- behavioral evals can test whether cheaper models/effort levels preserve PMB behavior;
- ablation can test whether instructions/skills earn their context cost;
- PMB should continue owning continuation/project truth, not provider routing;
- do not add a software-factory scheduler or model gateway to PMB.

## ACR

Model specialization is relevant only through ACR-native calibration/benchmark evidence.

Possible experiments:

- evaluator/reviewer model matrix on internal clean/dirty calibration plus AACR-Bench slice;
- lower-cost model for deterministic/bounded stages only if precision/recall/noise/timeouts remain acceptable;
- preserve evidence-basis semantics across model substitutions.

Do not infer ACR's best reviewer/fixer models from application-building demos.

---

# Candidate HE principles — research status

> **Allocate model capability by workload role, uncertainty, risk, and measured task performance.**

> **Prefer explicit, evaluated role policy before adaptive model routing.**

> **The cheaper the execution model, the stronger the input contract and verification need to be.**

> **Model specialization works best when roles exchange authoritative artifacts rather than inherited conversational assumptions.**

> **A routing policy should be justified by reliability on representative failure modes, not one successful demo or leaderboard score.**

> **An autonomous producer must not control the acceptance boundary that authorizes its own settlement.**

> **Independence is an authority property, not a naming or process property.**

> **Measured floors may become ratchets; invented floors are policy theater.**

> **Do not spend model judgment on lifecycle decisions that deterministic state can decide.**

> **Optimize the expensive boundary before building a smarter router.**

---

# Bottom line

Cole Medin's recent work provides useful implementation evidence for model specialization and lights-out workflows, but the durable HE lesson is more conservative than the video's model recommendation:

> **Use the least expensive capability that has been shown reliable for the role; make cross-model boundaries artifact-driven; preserve strong independent verification and bounded correction loops; keep provider mechanics below the harness; and ensure the autonomous worker cannot rewrite the rules that decide whether its own work passes.**

That gains most of the cost/rate-limit leverage without prematurely building an opaque routing or software-factory control plane.