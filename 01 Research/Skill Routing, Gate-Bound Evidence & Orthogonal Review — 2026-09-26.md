# Skill Routing, Gate-Bound Evidence & Orthogonal Review — 2026-09-26

## Purpose

Synthesize the durable Harness Engineering findings from the Dubibubi Claude-Skills source pass without promoting any individual third-party Skill or framework into HE architecture.

Primary source note:

- `01 Research/Sources/Dubibubi — Top Claude Skills, Routing & Evaluation — 2026-09-26.md`

This synthesis should be read alongside existing HE work on:

- Progressive Disclosure;
- fan-out / fan-in and evidence preservation;
- behavioral Skill evaluation;
- adaptive autonomy and diagnosis-before-mutation;
- Continuous Improvement / Harness Miner;
- bounded autonomous loops;
- Single Ownership.

---

# Core synthesis

"Skill" is not a sufficiently precise architectural category.

A Skill can alter:

```text
presentation
implementation heuristics
specialist knowledge retrieval
workflow routing
mutation
review/evaluation
completion semantics
autonomous experimentation
```

Those interventions have different costs, authority boundaries and validation requirements.

Therefore HE should evaluate a Skill first by **what layer it changes**, not by its packaging format or popularity.

---

# 1. Classify the intervention before choosing the mechanism

A useful working taxonomy is:

```text
PRESENTATION POLICY
  compress / reshape output

IMPLEMENTATION HEURISTIC
  influence solution shape

DOMAIN REFERENCE
  provide specialist knowledge on demand

WORKFLOW ROUTER
  select procedures/playbooks/capabilities

DIAGNOSTIC / TRANSFORMER
  inspect or mutate an artifact

EVALUATOR / REVIEWER
  judge an artifact against an objective

COMPLETION / EVIDENCE
  decide what must be proven before done

EXPERIMENT HARNESS
  iterate candidate changes against an evaluator
```

This matters because the owning primitive can differ.

For example:

- a terse-response preference may belong in presentation policy;
- a repeatable security workflow may belong in a Skill with specialist references;
- a hard safety invariant may belong in deterministic code/hook/policy rather than a Skill;
- acceptance evidence may belong in a test/evaluator rather than the producer's prompt.

### Principle candidate

> **Classify the intervention by the behavior it owns before deciding that a Skill is the right container.**

---

# 2. Progressive Skill Disclosure

Several mature repositories converge on a similar shape:

```text
small core / router
       ↓
classify task or capability need
       ↓
load one specialist workflow/reference
       ↓
execute
```

Examples include technology-specific Vibe Security references, HyperFrames' core router plus on-demand creation workflows, and Impeccable's one entry capability with explicit leaf references.

This is preferable to injecting every procedure into every session.

### What a healthy router should expose

- clear trigger/intent boundary;
- leaf identity/version;
- fallback when no leaf fits;
- visible ambiguity when several leaves fit;
- bounded context cost;
- no silent authority expansion during leaf activation.

### Router evaluation questions

A router can fail even when every leaf is excellent.

Measure:

- required-skill discovery recall;
- false invocation rate;
- ambiguous routing frequency;
- context/tool overhead;
- duplicate/overlapping capability activation;
- outcome differences relative to no Skill or a simpler mechanism.

### Principle candidate

> **Progressive disclosure applies to procedures and capabilities as well as context: keep the routing surface small and load detailed behavior only at the point of need.**

---

# 3. Empirical forks should not masquerade as preference questions

Pstack provides a strong operational distinction:

```text
Can the answer be observed by running/measuring/prototyping?
    YES → acquire evidence.
    NO  → ask the authority that owns the preference/decision.
```

This aligns with Evidence Before Architecture and reduces a common failure mode where an agent asks the human to guess an engineering fact the agent could cheaply establish itself.

Examples of empirical forks:

- which implementation is faster;
- whether a UI layout wraps correctly;
- whether an eval separates two behaviors;
- whether a proposed parser handles the real fixture;
- whether a change reproduces/fixes a defect.

Examples of genuine authority decisions:

- product intent;
- acceptable business tradeoff;
- branding/taste where no objective constraint resolves it;
- permission for consequential external action;
- explicit project priority.

### Principle candidate

> **If a decision is empirically resolvable inside the authorized execution envelope, prefer obtaining evidence over asking the human to guess.**

Do not use this principle to bypass product/authority decisions by inventing a metric.

---

# 4. Preserve orthogonal review objectives through fan-in

Matt Pocock's Standards-vs-Spec review demonstrates an important fan-in discipline.

Two reviewers can inspect the same change while answering different questions:

```text
Standards reviewer
  "Does this conform to repo rules/design expectations?"

Spec reviewer
  "Does this implement the requested behavior and only that behavior?"
```

A change can pass one and fail the other.

Collapsing the two into a single score destroys useful information.

### HE implication

Fan-in should preserve:

- reviewer objective;
- source/provenance;
- evidence basis;
- contradictions;
- unresolved coverage gaps.

A synthesizer may summarize or adjudicate, but should not erase the axes that made independent review valuable.

### Principle candidate

> **Keep orthogonal acceptance axes distinguishable through fan-in; aggregation should not erase what kind of evidence failed.**

### ACR relevance

This deserves future ACR-specific inspection. ACR already uses multiple review domains. Verify that fan-in preserves domain/reviewer provenance and does not flatten security, correctness, architecture, policy and evidence-quality judgments into an opaque homogeneous finding pile.

No ACR change authorized here.

---

# 5. Gate-bound evidence and evidence expiration

Unlazy makes a particularly strong contribution: a successful check is not timeless proof.

Verification evidence depends on the thing actually measured, including the current definition of the check and material execution-envelope inputs.

Conceptually:

```text
acceptance gate G1
+ command/check C1
+ expectation E1
+ environment V1
      ↓
 evidence R1
```

If `C1`, `E1`, the relevant environment, or the gate semantics materially change, `R1` should not silently certify the new configuration.

### HE principle candidate

> **Verification evidence is valid only for the acceptance definition and execution envelope it actually measured. Materially change either and reacquire the evidence.**

This connects directly to:

- ACR calibration provenance;
- local-model execution-envelope benchmarking;
- behavioral Skill evals;
- CI/eval result caching;
- Harness Miner candidate-change experiments.

### Explicit abandonment

Another useful pattern is that an impossible gate is not silently deleted. It becomes an explicit abandonment/handoff state with a reason.

That preserves negative evidence and prevents "done" from being achieved by editing away an inconvenient requirement.

---

# 6. Minimum sufficient mechanism needs explicit ceilings

Ponytail's strongest lesson is not "write less code." It is:

```text
understand real flow
      ↓
reuse existing/native/stdlib capability first
      ↓
choose minimum sufficient custom mechanism
      ↓
retain safety boundaries
      ↓
if deliberately simplified, state its known ceiling and upgrade trigger
```

This is compatible with Loosen the Leash because it constrains acceptance and ownership without prescribing every implementation step.

### Why the ceiling matters

A deliberate simplification such as:

- global lock;
- linear scan;
- local-only store;
- single-worker queue;
- fixed routing table;

may be correct today.

The useful durable information is not a defensive essay. It is the **condition under which the simple design stops being sufficient**.

### Principle candidate

> **Prefer the minimum sufficient mechanism, but make known ceilings and evidence-based upgrade triggers explicit when the simplification is deliberate.**

Do not turn every simple line of code into a documented future roadmap. Record ceilings only when material.

---

# 7. Presentation compression cannot own safety semantics

Caveman is useful because its implementation contains escape hatches for cases where brevity can change meaning.

HE should distinguish:

```text
reasoning/evidence acquisition
          ≠
presentation rendering
```

A response can be concise without reducing the verification performed underneath it.

But irreversible actions, security warnings, ambiguous ordered instructions and other high-consequence communication should prioritize unambiguous semantics over token reduction.

### Principle candidate

> **Compress presentation only while semantic risk remains low; safety-critical meaning outranks output-token efficiency.**

---

# 8. Diagnosis-only and mutation should be separate modes

No AI Slop's detect/edit split is a small reusable pattern:

```text
DIAGNOSE
  identify concrete patterns/evidence
  no mutation

TRANSFORM
  make bounded changes
  report what changed
```

This reinforces HE's diagnosis-before-mutation work.

For systems that can modify code, rules, prompts or project state, a diagnosis mode should not quietly become an edit mode merely because a fix seems obvious.

### Principle candidate

> **When inspection and mutation have materially different authority or risk, expose them as separate modes and make transformations auditable.**

---

# 9. Delegation does not transfer acceptance authority

Pstack instructs the parent to review subagent work rather than pass through a child summary.

Matt's parallel review axes likewise use the parent as aggregator, not as proof source.

This reinforces an existing HE boundary:

```text
worker produces
reviewer evaluates
synthesizer converges
acceptance authority accepts
```

One actor can sometimes occupy more than one role for low-risk work, but the roles should remain conceptually distinct.

### Reinforcement

> **Delegating execution does not delegate responsibility for accepting the result.**

---

# 10. Skill behavior is versioned behavior

Taste, HyperFrames, Impeccable, Ponytail and other active repositories evolve their instruction bodies, routers, manifests and host adapters over time.

A behavioral experiment that says only:

```text
"with Ponytail"
```

is underspecified.

The reproducible identity should include at least:

```text
skill source
+ pinned revision/version
+ host/runtime
+ model/effort
+ task/eval
```

where these variables can materially affect behavior.

### Principle candidate

> **Treat a behavior-changing Skill like executable configuration: pin its identity in experiments and evidence.**

---

# 11. Source truth vs documentation caches

Matt Pocock's repository work reinforces Single Ownership in a useful way.

Facts already cheaply available from:

- package/config files;
- CLI `--help`;
- directory structure;
- current code;
- runtime-owned status;

should not automatically be copied into persistent instruction documents.

A restatement is a **cache**.

The cache should earn its place when:

- lookup is expensive or unreliable;
- the source is hard for the agent to discover;
- the durable value is rationale/convention/gotcha not represented in the source;
- measured retrieval behavior justifies it.

### Principle candidate

> **Do not persist cheap source-owned facts as instructions merely for convenience; treat restatements as caches that must earn freshness and context cost.**

This connects directly to MAPS navigation, Graphify economics and PMB retrieval design.

---

# 12. Evaluate Skills behaviorally, not socially

The video uses stars/install counts and personal experience as discovery signals. HE should not confuse those with causal evidence.

A useful Skill evaluation should preserve:

```text
representative task
same model/runtime where possible
baseline
candidate intervention
objective outcome checks
cost/time/tool/context measurements
regression/adversarial checks where appropriate
```

Ponytail's repository provides one concrete example of this style, with same-model control/treatment, extension tasks and safety probes. Its results remain project-local evidence and have external-validity limitations.

OpenSEO's previously mined behavioral Skill evaluator remains the stronger general reference because it adds fresh isolation, frozen candidate identity, holdouts and trace-based diagnosis after scoring.

### Reinforcement

> **A Skill earns durable context/routing cost through measured behavior, not popularity or plausible prose.**

---

# 13. Implications for Harness Miner

Harness Miner could eventually make Skill lifecycle decisions evidence-driven.

Potential observations:

- user corrections matching a Skill's claimed failure mode;
- tasks where a Skill should have triggered but did not;
- irrelevant Skill activations;
- additional tool/context cost caused by routing;
- repeated manual procedures suitable for a leaf Skill;
- obsolete rules/Skills that no longer affect outcomes;
- before/after behavior for candidate Skill revisions.

The feedback loop becomes:

```text
real work
  ↓
recurring failure/friction
  ↓
identify owning intervention class
  ↓
candidate Skill/rule/tool correction
  ↓
freeze candidate identity
  ↓
behavioral eval / ablation
  ↓
human decision
```

Harness Miner should not auto-write or auto-install Skills.

---

# 14. Implications for HE-002 modular capability architecture

This research strengthens the **case to assess modular capabilities**, but it also strengthens the risks already recorded under HE-002.

Evidence for modularity:

- specialist references can be loaded only at the point of need;
- routers can keep detailed workflows out of startup context;
- one capability can expose host-specific adapters;
- producer, critic and verifier roles can remain independently invokable.

Risks:

- trigger/routing ambiguity;
- huge meta-router surfaces;
- overlapping capability ownership;
- hidden dependencies between Skills;
- self-review presented as independent verification;
- dynamic installation expanding authority;
- procedural ceremony on trivial work.

Therefore this source pass does **not** authorize HE-002 implementation.

---

# Candidate principles status

## Strong candidates / reinforce existing HE

- **If a decision is empirically resolvable inside the authorized envelope, acquire evidence instead of asking the human to guess.**
- **Keep orthogonal acceptance axes distinguishable through fan-in.**
- **Verification evidence expires when the acceptance definition or material execution envelope changes.**
- **Prefer the minimum sufficient mechanism; record material ceilings and evidence-based upgrade triggers.**
- **Treat behavior-changing Skills as versioned executable configuration in experiments.**
- **Delegation does not transfer acceptance authority.**

## Useful but narrower

- Compress presentation only when semantic risk remains low.
- Separate diagnostic and mutation modes when authority/risk differs.
- Treat persisted restatements of cheap source-owned facts as caches.

## Needs evidence before promotion

- broad task-class routing as a default architecture;
- dynamic Skill installation;
- large agent "operating system" bundles;
- persistent terse-output policy;
- producer/critic pairs as a default quality strategy.

---

# Disposition

- **MINE:** progressive Skill disclosure, empirical forks, orthogonal review, gate-bound evidence, minimum-sufficient mechanism with ceilings, source-vs-cache distinction.
- **ASSESS:** routers and task-class playbooks against simpler baselines.
- **PARK:** domain-specific design/media Skills as HE architecture.
- **REJECT:** popularity/rankings as effectiveness evidence; loading every useful Skill always; treating same-model self-review as independent verification; universal heavy completion ledgers.

## Status

Research synthesis only.

No PMB, work-MB, ACR, Dashboard, Harness Miner or HE-002 implementation change authorized by this note.
