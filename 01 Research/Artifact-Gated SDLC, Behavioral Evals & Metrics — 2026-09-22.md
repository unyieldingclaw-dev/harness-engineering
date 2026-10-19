# Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22

## Purpose

Synthesize the durable Harness Engineering implications from the first-party Claude Academy AI-Native SDLC material without copying Anthropic's file layout or duplicating Superpowers capabilities already present in the user's workflow.

Primary evidence:

- `01 Research/Sources/Claude Academy AI-Native SDLC — First-Party Deep Pass — 2026-09-22.md`
- `01 Research/Sources/Anthropic Opus 5.5 Prompting — Harness-Relevant Runtime Guidance — 2026-09-24.md`
- `01 Research/Sources/CoderOne — Jev-Style Fine-Tuning, Decision Models & Evaluation Isolation — 2026-09-26.md`
- `01 Research/Sources/Prompt Engineering — Bonsai 2 Compression, Agentic Horizon & Trace Failure — 2026-09-28.md`
- `01 Research/Sources/Stacked Podcast — Naive-N0.5-Flash, AI-Centered R&D & Capability Step-Down — 2026-09-28.md`
- `01 Research/Sources/Beyond Coding — Carlos Kelkboom, Planning Scaffolds & Iterative Agent Engineering — 2026-10-15.md`

Related HE research:

- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`

---

# Executive synthesis

The strongest pattern is not a specific `intent.md → spec.md → plan.md` folder structure.

It is a staged lifecycle in which:

1. each stage has an explicit artifact or observable state;
2. one source owns each fact;
3. acceptance of the current stage authorizes transition to the next;
4. the producer continuously checks its work;
5. independent verification challenges the finished result;
6. deterministic checks absorb deterministic review work;
7. consequential actions remain behind explicit gates;
8. real solved work becomes behavioral regression evidence;
9. evaluation isolation matches the real leakage boundary when independence matters;
10. behavioral evaluation covers the horizon at which failures can compound;
11. end-to-end acceptance outranks attractive local/microbenchmark wins;
12. metrics close the loop only when they change an engineering decision;
13. planning fixes high-cost structural freedoms before implementation when accidental invention would create expensive path dependence.

This maps cleanly onto Superpowers + PMB + ACR without requiring another workflow framework.

---

# 1. Stage transitions should be explicit

A useful generalized lifecycle is:

```text
intent / outcome
      ↓ accepted
specification / design
      ↓ accepted
implementation plan
      ↓ authorized
implementation
      ↓ verified
review
      ↓ accepted
release
      ↓ observed
maintenance evidence
      ↓
new intent when warranted
```

The harness does not need to own every artifact. It needs to know which source owns the artifact and what observable condition means the stage is complete.

### Candidate principle

> **Accepted artifacts should define stage transitions.**

This is stronger than "follow a workflow" because it makes transitions inspectable and reduces hidden conversational state.

**Disposition: REINFORCE.**

---

# 1A. Planning should freeze high-cost structural freedoms before implementation

The Carlos Kelkboom / SlopCodeBench pass adds a planning-time refinement that is missing from a generic "design → plan → implement" lifecycle.

A coding agent can satisfy the immediate request while silently inventing:

- responsibility boundaries;
- dependency direction;
- data/type shapes;
- public interfaces;
- persistence/integration seams;
- deployment assumptions;
- duplicate implementations of an existing concept.

When those choices are expensive to reverse, the plan should explicitly decide which of them are **not** delegated to implementation.

A useful working abstraction is a **Scaffold Contract**:

~~~text
Scaffold Contract
  ├─ ownership / responsibility boundaries
  ├─ dependency direction
  ├─ core domain/data shapes
  ├─ important interfaces
  ├─ integration seams
  ├─ runtime/deployment constraints
  ├─ representative examples
  ├─ executable invariants where cheap
  └─ known-plausible extension pressures
~~~

The Scaffold Contract is a concept, not a required new file. It should live inside the design/spec/implementation-plan artifact that already owns those decisions.

### Scaffold depth is risk-directed

Do not scaffold every task to the same depth.

A useful heuristic is:

| Work intent | Planning treatment |
|---|---|
| Spike / throwaway experiment | minimal boundary; make discard cheap |
| Normal feature | ownership, interfaces/types, dependency direction, representative verification |
| Durable subsystem | above + data contracts, integration/deployment seams, likely extension points |
| Cross-cutting / high-consequence | above + invariants, migration/rollback, security/observability boundaries, architecture checks where justified |

The governing question is:

> **How expensive would the wrong structural choice be to discover after the implementation agent has generated a large amount of code?**

### Extension-pressure test

Before high-volume implementation, test the scaffold against one to three **known-plausible** changes supported by current requirements, roadmap or demonstrated pressure.

The purpose is not speculative extensibility. It is to catch an obvious path-dependence trap while structural reversal is still cheap.

~~~text
current requirement
      ↓
accepted scaffold
      ↓
known-plausible extension
      ↓
does the extension fit the same ownership/interfaces?
      ├─ yes → proceed
      └─ no  → revise scaffold before implementation
~~~

Skip this test when no plausible extension pressure is known.

### Logical scaffold vs physical scaffold

For durable work, the logical scaffold may optionally be materialized in an isolated worktree before substantive implementation:

- folders/projects;
- interface/type declarations;
- selected signatures;
- empty adapters/modules;
- architecture/dependency checks;
- representative test skeletons.

Run cheap deterministic checks, then stop.

This creates a **Scaffold Gate**:

> **Does the materialized structure match the accepted scaffold closely enough that the implementation agent is filling an agreed shape rather than silently inventing a new one?**

A spike can skip this entirely. The gate exists to reduce expensive structural drift, not to create ceremony.

### Extension-horizon evidence

SlopCodeBench provides corroborating evidence that iterative agent work can degrade structurally across successive specification changes even when local checkpoints remain the unit of progress. The benchmark should not be treated as final authority: its public runner describes it as an evolving evaluation primitive and independent audit work has challenged parts of the measurement instrument.

The durable HE point is narrower:

> **A current-task pass does not establish extension fitness.**

For long-lived agent-built code, verification may therefore need to include whether accepted structural invariants survived the change:

- forbidden dependency checks;
- module ownership violations;
- duplicated domain implementations;
- architecture tests;
- complexity concentration;
- scaffold-to-implementation traceability where valuable.

### Candidate principles

> **Front-load the few structural decisions whose accidental invention would make future change expensive; let implementation agents generate freely inside those boundaries.**

> **Scaffold depth should scale with expected lifetime, change pressure, consequence and reversibility.**

> **Pressure-test structure against known-plausible change, not speculative universality.**

> **When a stable scaffold rule becomes mechanically checkable, prefer deterministic enforcement over repeatedly asking the model to remember it.**

**Disposition: REINFORCE / ASSESS in real planning workflows. Do not create a mandatory new artifact until a consumer requires one.**

---

# 2. Do not duplicate authorities across PMB, Superpowers, tickets, and Git

The current stack already has several legitimate owners:

- Superpowers: brainstorming/shared understanding, design/specification, implementation planning, TDD/implementation workflow.
- PMB: durable project memory, execution scope/authorization, handoff, governance, hooks.
- Git: branch/commit/worktree state.
- CI: build/test/check state.
- ACR/review tooling: review findings/evidence.

Creating duplicate `intent.md`, `spec.md`, or `plan.md` artifacts purely because another workflow uses those names would create competing truth.

### Candidate principle

> **One fact should have one owning source; linked systems may reference it but should not silently become competing authorities.**

**Disposition: STRONGLY REINFORCE.**

---

# 3. Feedback loop and verifier are different layers

The producer should be able to inspect and correct its own work while implementing.

Typical producer loop:

```text
change
  ↓
run check
  ↓
compare to expected result
  ↓
fix if needed
  ↺
```

Independent verification then checks the finished result from a fresh context or separate authority boundary.

### Candidate principle

> **Producer self-checking and independent verification solve different failure modes.**

This complements the existing HE distinction between execution success and effect verification.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Checks need acceptance semantics

A command by itself is not a complete verification contract.

Weak:

```text
run tests
```

Stronger:

```text
run tests → zero failing tests
run lint  → no disallowed findings
run build → successful exit/build marker
```

The harness should avoid letting the producer silently redefine what "green" means.

### Candidate principle

> **Verification should define the observable postcondition, not merely name the command to run.**

**Disposition: REINFORCE.**

---

# 5. Behavioral evals are distinct from structural CI

Current harness CI commonly answers mechanical questions:

- does a file exist?
- is syntax valid?
- does a hook run?
- are secrets absent?
- does a script pass shellcheck?
- does a template match expected structure?

Those checks are necessary but insufficient for behavior-shaping configuration.

Changes to instructions, skills, hooks, routing, models, quantization or runtimes can leave every structural test green while changing agent behavior materially.

### Candidate principle

> **Harness configuration is executable behavior and should have behavioral regression tests.**

A behavioral corpus should come from real solved tasks and incidents, not invented examples chosen only to satisfy a target count.

For each durable case, retain only what is needed to reproduce the behavior under test:

```text
starting state / fixture
prompt or task
expected or accepted outcome
observable checks
important failure mode
```

### PMB implication

The PMB pilot can become the beginning of this evidence base. Do not interrupt the pilot to build a framework first.

**Disposition: ASSESS after pilot.**

---

# 6. Deterministic checks should absorb deterministic review work

If CI, a linter, formatter, schema checker, or hook can decide a property reliably, an LLM reviewer should not spend tokens rediscovering it unless the semantic context changes the meaning.

Use probabilistic review capacity for questions that actually require interpretation:

- subtle correctness;
- security reasoning;
- intent/spec compliance;
- architecture drift;
- cross-file consequences;
- unsupported assumptions;
- residual risk.

### Candidate principle

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

This is directly relevant to ACR noise reduction.

**Disposition: STRONGLY REINFORCE.**

---

# 7. Metrics are decision inputs, not a product requirement

The Claude Academy maintenance loop uses system metrics to detect degradation and initiate diagnosis or new work. The durable mechanism is not the observability vendor.

A useful metric needs:

1. **owner/source** — where the fact comes from;
2. **baseline/expected meaning** — what normal looks like;
3. **freshness/provenance** — when/how it was measured;
4. **decision boundary** — what changes when the metric leaves the acceptable range;
5. **verification** — how the corrective action is confirmed.

Without those semantics, adding telemetry creates inventory rather than leverage.

### Candidate principle

> **Collect a metric only when a known engineering decision consumes it.**

**Disposition: REINFORCE.**

---

# 8. PMB metrics should remain lightweight and source-owned

PMB is primarily a local/repository harness and CLI, not a production SaaS runtime. Its useful metrics today are mostly available from existing deterministic sources:

- `mb status` / `mb doctor` health signals;
- startup context size;
- stale memory/handoff state;
- task-contract/scope warnings;
- hook/CI pass/fail;
- handoff/resume behavior;
- archive/reference retrieval when required;
- behavioral-eval pass/fail when that corpus exists;
- pilot outcomes such as human intervention and task completion quality.

Do not make PMB sessions append telemetry manually. If a metric matters, derive it from the owning system or add a deterministic machine-readable interface when an actual consumer exists.

### Tooling disposition

**Do not add Datadog or Sentry to PMB/HE now.**

Reasons:

- there is no production service whose runtime failure stream requires those platforms;
- most useful PMB/HE facts already exist in Git, CI, CLI output, hooks, or test artifacts;
- external telemetry would add cost, credentials, privacy/security surface, retention questions, and operational maintenance before there is a demonstrated need;
- it risks turning observability into a subsystem rather than answering an observed problem.

If a future PMB-hosted service or external Dashboard becomes operationally important, reassess from the concrete failure/diagnostic need.

**Disposition: REJECT tool-first adoption; PARK external observability platform.**

---

# 9. Product applications have a different threshold

A deployed user-facing application can justify an error/telemetry service much sooner than PMB or HE because production failures may otherwise be invisible.

For a small application, an error-focused tool can be sufficient before a full observability platform is warranted.

The decision should be driven by questions such as:

- Are production exceptions currently invisible?
- Do users report failures before the team can detect them?
- Is distributed tracing actually needed?
- Are latency/resource metrics necessary for a real SLO or debugging problem?
- Is there enough scale/operational complexity to justify maintaining the integration?

Do not infer that "AI-native SDLC uses metrics" means every project needs enterprise telemetry software.

---

# 10. Metrics worth observing in PMB's pilot before building anything

The pilot can collect a small set of evidence without introducing new infrastructure:

- task completed / not completed;
- human intervention points;
- handoff required and whether resume succeeded;
- stale or conflicting state encountered;
- archive/reference required and whether it was retrieved;
- scope warning/violation occurrences;
- number of repeated failed attempts before escalation;
- verification result and whether producer self-check disagreed with independent review;
- wall time if easy to capture;
- provider usage only when the provider exposes it directly and the value helps a decision.

Do not add unsupported pass/fail thresholds just to create a dashboard.

**Disposition: MEASURE FIRST, DESIGN LATER.**

---

# 11. Cross-project disposition

## HE

Document metrics semantics, evaluation isolation, horizon-aware behavioral evaluation, end-to-end acceptance and artifact-gated transitions as research principles. Do not build an observability platform or generic eval control plane.

## PMB

Use pilot evidence and existing CLI/CI sources. Assess behavioral evals after the pilot. Do not add Sentry/Datadog now.

## ACR

Continue benchmark-style measurement around finding quality, false positives/fabrication, timeouts, clustering, and accepted outcomes. Inspect fixture grouping/derivation before calling a corpus independent. If aggressive compression or lower-cost models are evaluated, make sure the fixture set exercises the review horizon and repeated behavior that matters, rather than only one-shot findings. External telemetry platform not justified by current evidence.

## User-facing applications

Evaluate production error visibility separately. If a real app has invisible production exceptions, an error-focused integration may be appropriate; do not generalize that requirement back into PMB/HE.

---

# 12. Effort/model configuration should be evaluated as harness behavior

Anthropic's Opus 5.5 guidance explicitly recommends starting at the model's default `medium` effort and then testing effort levels against the workload's own evals rather than carrying forward an old default.

This is a concrete example of behavioral harness evaluation.

A useful effort sweep should use more than a toy prompt. Prefer a small representative set such as:

```text
routine bounded implementation
difficult debugging/reasoning
review/architecture task
```

For each run, hold constant what can reasonably be held constant:

- starting repository state;
- task instructions;
- files/context supplied;
- tool/capability surface;
- fresh session/start state;
- verification method.

Observe:

- accepted/correct outcome;
- deterministic verification;
- independent review findings when relevant;
- elapsed time;
- provider usage/cost when available;
- retries/corrective turns;
- human intervention.

The goal is not to crown a universal “best” effort level. It is to find the **lowest effort that preserves the required behavior for the workload class**.

### Candidate principle

> **Model and effort configuration should be treated as evaluated harness configuration, not preference.**

**Disposition: REINFORCE.**

---

# 13. Behavioral guidance needs a model/version lifecycle

Anthropic's Opus 5.5 guide documents behaviors and mitigations that differ from Opus 5, including effort defaults, cache behavior, long-run turn termination, progress handling, and prompt patterns that may no longer be needed.

This gives behavioral evals another job: detect when persistent instructions have become unnecessary or harmful after a model/runtime change.

Before retaining a model-specific instruction, ask whether it still:

- prevents an observed current failure;
- encodes intentional project/team behavior;
- enforces a safety/deterministic requirement;
- materially improves an evaluated outcome.

Do not preserve a workaround merely because it was once useful.

### Candidate principle

> **Behavior-shaping guidance should be revalidated when the model/runtime changes materially.**

**Disposition: STRONGLY REINFORCE Model Capability Drift.**

---

# 13A. Harness mechanisms need measured contribution

The 2026-10-19 Apple MLE and HarnessTax evidence adds a stronger control condition for HE.

Apple's autonomous-MLE study reports that, under the tested equal-time / same-backbone conditions,
elaborate open-source MLE harnesses did not outperform a minimal coding-agent baseline. HarnessTax
likewise shows that the same model can have similar task success across different coding harnesses
while cost moves materially.

Neither result proves that minimal harnesses always win. They do establish that **added machinery
does not get credit merely for existing**.

A useful HE experimental pattern is a **Harness Ablation Protocol**.

```text
H0 = minimum credible harness for the task
H1 = H0 + one candidate mechanism
H2 = H1 + one additional mechanism
...
```

Hold the task corpus and model/runtime configuration stable where possible and measure:

- task success / correctness;
- end-to-end acceptance;
- token/provider cost;
- wall time;
- tool calls;
- retries / repeated reads;
- human intervention;
- constraint violations;
- recovery behavior;
- consequence-relevant failures.

A complementary **model ablation** holds the harness/configuration stable while changing the model.

### Experimental attribution rule

Automatic provider/model failover should be disabled in experiments that are attempting to measure
a specific model or harness contribution. If failover is enabled, failover itself is part of the
treatment and the result cannot be attributed to one model.

### Candidate principles

> **Compare new harness machinery against the smallest credible baseline that can perform the task.**

> **Measure cost per successful accepted outcome, not raw token reduction, model price, or harness sophistication.**

> **A harness feature should be able to name the property it purchases and show evidence that the workload consumes that property.**

---

# 14. The split boundary must match the leakage boundary

CoderOne's Jev-style fine-tuning experiment provides a concrete behavioral-eval example.

Instead of a simple random row split, the experiment keeps every logical `group_id` entirely in training or holdout. That matters when multiple records are variants or derivatives of the same underlying scenario.

The durable rule is broader than model training:

```text
row independence?       -> row split may be adequate
shared source/case?     -> group by source/case
same repo/incident?     -> group by the derivation boundary that could leak behavior
```

### Candidate principle

> **The split boundary must match the leakage boundary.**

A holdout is only as independent as the dependency structure it excludes.

**Disposition: STRONGLY REINFORCE behavioral-eval design.**

---

# 15. Evaluation isolation should be structural when exposure would invalidate it

CoderOne's final holdout remains outside the Modal training volume. The training environment cannot casually inspect the final evaluation labels.

This is stronger than a prose instruction saying “do not use the holdout.”

Related HE examples include Ponytail's contaminated control arm and independent acceptance authority.

### Candidate principle

> **Evaluation isolation should be structural when practical and when exposure would materially weaken the evidence.**

This does not imply hiding every test. Structural isolation is justified when the thing being optimized could overfit, game, rewrite or consume the evidence that is supposed to remain independent.

**Disposition: STRONGLY REINFORCE.**

---

# 16. Structural validity and semantic correctness need separate metrics

CoderOne's evaluator constrains generation to legal option letters, making invalid labels difficult/impossible by construction. Native typed decision systems can provide even stronger schema guarantees.

A structural guarantee is valuable, but it must be attributed correctly.

A controlled September 2026 study of Jev and Jev-like typed decision heads found 0% type errors even in conditions where semantic decision quality degraded sharply under option-name/rubric interventions.

### Candidate principles

> **Attribute a metric to the layer that actually guarantees it.**

> **Schema/type validity is a structural property; semantic correctness requires separate evidence.**

Therefore a benchmark for typed decisions should not treat “0 malformed outputs” as semantic quality evidence when the decoder/head enforces the legal output space.

Potential semantic tests include:

- label/rubric permutation where semantics should remain invariant;
- neutral-label controls;
- known-positive and known-negative decision cases;
- calibration/reliability checks when probability semantics matter.

Use only tests relevant to the consequential failure mode. Do not turn this into a universal adversarial-test framework.

**Disposition: STRONGLY REINFORCE.**

---

# 17. Behavioral fidelity needs a task horizon

Bonsai 2 provides a concrete example of why one-shot or short-task evaluation can miss an agent failure mode.

The compressed Qwen3.8-derived model is close to the parent on a broad short-form benchmark average and showed near-parity on several short practical tasks in an independent hands-on comparison.

The same source reported severe failure across six longer application-building loops, with traces dominated by repeated inspection/search actions and failure to progress into productive execution.

PrismML's own Terminal-Bench 2.1 and SWE-bench Verified results also show materially lower retention than the broad benchmark average.

The exact hands-on A/B includes runtime/provider differences, so it is not proof that compression alone caused the observed loop pathology. It is strong evidence that **short-task parity did not predict this deployment-shaped behavior**.

### Candidate principle

> **Evaluate behavior across the horizon at which failures can compound.**

The required horizon depends on deployment:

```text
single bounded decision       -> short eval may be enough
multi-step review             -> repeated evidence/judgment may matter
long autonomous build         -> planning + tool use + recovery + stopping matter
```

Do not add long-running evals just because they are more impressive. Add them when the intended workload can fail only after state accumulates.

### Diagnostic traces when needed

If an end-state failure needs diagnosis, useful signals may include:

- repeated identical tool calls;
- repeated retrieval of unchanged state;
- excessive steps before first material change;
- failure to transition from exploration to execution;
- retries without changed hypothesis/evidence;
- inability to recognize completed substeps;
- failure to stop or escalate.

These are diagnostic measurements, not universal telemetry requirements.

**Disposition: NEW / STRONGLY MINE.**

---

# 18. Aggregate benchmark averages can hide consequence-relevant cliffs

A broad average is useful for summarizing many tasks, but it can hide a large deficit in a workload that matters more to deployment.

Bonsai 2 illustrates the shape:

```text
broad benchmark retention        very high
long-agent benchmark retention   materially lower
```

Both can be numerically true.

### Candidate principle

> **Averages summarize; acceptance gates belong to the consequence-relevant workload metrics.**

That means a benchmark report should preserve important strata when averaging would hide the failure mode that drives the real decision.

Examples:

- clean vs defective code review;
- short vs long task horizon;
- deterministic correctness vs semantic judgment;
- easy vs adversarial cases;
- fresh context vs long accumulated session when relevant.

Do not create arbitrary strata. Preserve the ones tied to actual risks or deployment decisions.

**Disposition: NEW / STRONGLY MINE.**

---

# 19. End-to-end acceptance outranks microbenchmark wins

NaiveAI's NaiveRT case study is a strong industrial example of an optimizer producing many candidate performance changes under externally owned correctness and end-to-end criteria.

The company reports 151 optimization trials, with 63 adopted, 71 failed validation/rolled back, and 17 alternative/prototype paths.

More important than the count are the rejection examples:

- some changes improved a local measurement at one context or microbenchmark but regressed another important context or full-model path;
- some optimizations were numerically correct but did not improve end-to-end performance and were abandoned;
- a prefetch change could look less attractive under a warm-cache isolated benchmark while improving the integrated model step.

### Candidate principle

> **Optimize the property the system actually consumes; use local metrics to diagnose, not to redefine success.**

A safe optimization loop is:

```text
candidate
   ↓
correctness / invariant gate
   ↓
end-to-end target workload measurement
   ↓
keep / revise / revert
```

This applies beyond kernels:

- smaller context is not a win if retries rise;
- faster decoding is not a win if prefill dominates;
- cheaper reviewer is not a win if false positives rise beyond tolerance;
- smaller compressed model is not a win if long-horizon control collapses.

**Disposition: NEW EVIDENCE / STRONGLY REINFORCE.**

---

# 20. Experiment accounting should preserve rejected evidence when it changes future decisions

NaiveAI's adopted / failed / explored accounting is useful because failed experiments remain evidence rather than disappearing into an iteration loop.

HE should not require a ledger for every trivial attempt.

But when experimentation is expensive, repeated, automated or likely to revisit the same hypothesis, durable evidence can include:

```text
hypothesis / candidate identity
what changed
acceptance/evaluator identity
result
keep / revert / inconclusive
why
```

### Candidate principle

> **Retain rejected experiment evidence when forgetting it would cause expensive re-exploration or weaken provenance.**

This is the experiment analogue of preserving useful incident knowledge without turning every transient thought into memory.

**Disposition: NEW / ASSESS where automated experiment loops become material.**

---

# Candidate HE principles — research status

> **Accepted artifacts should define stage transitions.**

> **One fact should have one owning source; linked systems may reference it but should not silently become competing authorities.**

> **Producer self-checking and independent verification solve different failure modes.**

> **Verification should define the observable postcondition, not merely name the command to run.**

> **Harness configuration is executable behavior and should have behavioral regression tests.**

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

> **Collect a metric only when a known engineering decision consumes it.**

> **Model and effort configuration should be treated as evaluated harness configuration, not preference.**

> **Behavior-shaping guidance should be revalidated when the model/runtime changes materially.**

> **The split boundary must match the leakage boundary.**

> **Evaluation isolation should be structural when practical and when exposure would materially weaken the evidence.**

> **Attribute a metric to the layer that actually guarantees it.**

> **Schema/type validity is a structural property; semantic correctness requires separate evidence.**

> **Evaluate behavior across the horizon at which failures can compound.**

> **Averages summarize; acceptance gates belong to the consequence-relevant workload metrics.**

> **Optimize the property the system actually consumes; use local metrics to diagnose, not to redefine success.**

> **Retain rejected experiment evidence when forgetting it would cause expensive re-exploration or weaken provenance.**

---

# Bottom line

The Claude Academy, Opus 5.5, Jev-style evaluation, Bonsai compression and NaiveAI experiment material strengthen the case for artifact-gated work, horizon-aware behavioral evals, explicit verification semantics, evaluated model/effort/runtime configuration, leakage-aware holdouts and evidence-driven maintenance.

For the current HE/PMB environment, the right strategy remains deliberately small:

> **measure real workflow outcomes from systems that already own the facts; isolate evaluation evidence only where contamination would invalidate it; evaluate across the horizon where the intended workload can actually fail; distinguish structural validity from semantic correctness; optimize against the end-to-end property the system consumes; and add new infrastructure only when a concrete problem cannot be diagnosed or controlled reliably without it.**
