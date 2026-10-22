# Turing Post — Harness Minimalism, Long-Horizon State & Control Planes — 2026-10-19

## Source set

Primary synthesis video supplied by Nolan:

- Turing Post TV — **AI Models Are Eating Their Harnesses. Now What?**
  - https://www.youtube.com/watch?v=pEHfZUrahGc
  - reviewed with user-supplied frames from the Turing Post / AI Engineer NYC 2026 material

Primary references verified during the deep pass:

- Apple Machine Learning Research — **How Much of a Harness Does a Strong Agent Need for Autonomous ML Engineering?**
  - https://machinelearning.apple.com/research/harness-autonomous-ml-engineering
- HarnessTax — **How Much Does the Harness Matter for Coding Agents?**
  - https://harnesstax.github.io/
- Apple Machine Learning Research — **SCLATE: A Substrate for Continual-Learning Agent Training and Evaluation**
  - https://machinelearning.apple.com/research/sclate-agent-training-evaluation
- AI Engineer NYC 2026 — Arca — **Inside Arca’s long-horizon agents that run the business**
  - https://ai.engineer/nyc/2026/talks/arca
- AI Engineer NYC 2026 — Arize — **What does an AI engineer do?**
  - https://ai.engineer/nyc/2026/talks/arize
- AI Engineer NYC 2026 — Anthropic — **Accelerating frontier AI in Financial Services**
  - https://ai.engineer/nyc/2026/talks/anthropic
- AI Engineer NYC 2026 — Google DeepMind — **Coding an Autonomous Wall Street Coworker: Custom Skills, Connectors, and Event Hooks**
  - https://ai.engineer/nyc/2026/talks/deepmind
- AI Engineer NYC 2026 — Modal — **Great Agents Start with Great Infrastructure**
  - https://ai.engineer/nyc/2026/talks/modal

## Why this source set matters to HE

This batch does not justify a new orchestration framework. It does something more useful: it separates **temporary model compensation** from **durable operational control**, and gives HE evidence for deciding which harness mechanisms should exist, which should be tested, and which should eventually be deleted.

The central synthesis is:

> **Models are absorbing some behavioral scaffolding; they are not eliminating the need for an operational control plane.**

A stronger model may make a long prompt, forced planning ritual, special retrieval subagent, or redundant reviewer unnecessary. It does not remove the need for containment, identity, authority boundaries, durable task state, recovery, credential isolation, observability, or evaluation when those properties are required by the workload.

---

# 1. Minimum agent loop is a control, not a production target

The video demonstrates a tiny coding-agent loop:

1. define a tool;
2. send conversation + tool schema to the model;
3. execute the requested tool call;
4. return the tool result;
5. repeat until the model stops requesting tools.

That primitive is valuable to HE as a **minimum viable harness control condition**.

It is not a production recommendation. An unconstrained model-to-shell loop lacks meaningful containment, scoped authority, rollback, credential isolation, approval gates, resource budgets, and effect verification.

### Transferable principle

> **Agency is cheap; reliable agency is not. Add harness structure only when it purchases a demonstrated property.**

Examples:

- sandbox → containment;
- approval hook → bounded authority;
- checkpoint → recoverability;
- persistent task state → resumability;
- trace/eval → observability and regression evidence;
- skill routing → selective context;
- Scaffold Contract → structural intent.

A harness mechanism whose purchased property cannot be named and measured has a higher burden of proof.

---

# 2. Apple MLE result: elaborate scaffolding can become redundant

Apple's October 2026 MLE study compares strong coding-agent backbones under equal time budgets and reports that the tested state-of-the-art open-source MLE harnesses provided no advantage over a single-session minimal-harness coding-agent baseline in that benchmark family.

This is important evidence against assuming that:

- more agents;
- dedicated retrieval agents;
- elaborate orchestration;
- more hand-written process machinery

necessarily improve autonomous engineering performance.

### Boundary

The result is domain- and benchmark-specific. It does **not** establish that runtime safety, durable state, permission boundaries, traceability, approval, recovery, or identity are unnecessary.

### HE interpretation

> **Do not use "harness sophistication" as a proxy for capability. Compare against the smallest credible baseline.**

---

# 3. HarnessTax: harness choice can move cost more than correctness

HarnessTax evaluates 21 model/harness pairs across seven models and three coding harnesses: Claude Code, Codex CLI, and Pi, on SWE-bench Lite and Terminal-Bench 2.0.

The reported high-level result is that the same model often achieves similar success rates across harnesses while cost varies materially. The authors also show that a simple harness can remain competitive and that a model may not perform best in the harness sold by the same vendor.

The exact cost ratios are workload-specific and should not be generalized as universal constants.

### HE implication

Model evaluation is incomplete if it tests only the model.

The evaluated unit is at least:

```text
model
+ harness
+ runtime/tool configuration
+ effort/reasoning configuration
+ task corpus
```

### Transferable principle

> **Measure cost per successful outcome for a model-harness pair, not model price or harness reputation in isolation.**

---

# 4. Long-horizon is not long-running

Arca's example spans days because the work waits on people, approvals, settlement, and external events. The agent is not continuously burning inference throughout that horizon.

The useful pattern is:

```text
perform runnable work
        ↓
reach external dependency
        ↓
persist task state
        ↓
stop
        ↓
event / time / human action
        ↓
resume in a fresh run
```

Potential wake-up signals include:

- human response;
- approval;
- artifact arrival;
- settlement;
- CI completion;
- issue/ticket update;
- scheduled time;
- external system event.

### HE implication

A long-lived business or engineering objective should not imply one immortal context or one continuously running process.

### Transferable principle

> **Persist the job, not the inference.**

---

# 5. Separate four persistence resources

This source set sharpens a distinction that HE already partially owns.

## Knowledge

Reusable project/domain truth across work.

## Task state

Where a specific job currently stands: completed work, unresolved work, blockers, next action, waiting condition.

## Execution artifacts

Prepared environments, snapshots, caches, derived files, worktrees, build artifacts, or other resumable execution material.

## Inference context

Only what the current model call needs to decide the next action.

These resources have different lifecycles and authorities.

SCLATE further supports separating models, harnesses, memory, session events, scheduling, and memory consolidation rather than treating all persistence as one chat history.

### Transferable principle

> **Do not use a larger context window as a substitute for durable state design.**

---

# 6. Run retention contract

Arize's Signal architecture makes the keep/destroy boundary explicit.

A run assembles evidence, repository access, tools, temporary credentials, and context inside an isolated sandbox. The run is traced. Useful outputs/state survive; the sandbox, credentials, and temporary access are torn down.

This suggests an HE **Run Retention Contract**.

### Keep deliberately

- accepted output;
- verified findings;
- durable task-state checkpoint;
- selected traces/evidence;
- approved memory;
- promoted reusable skill/rule;
- receipts required for audit or recovery.

### Destroy deliberately

- ephemeral sandbox;
- short-lived credentials;
- scratch checkout/worktree when no longer required;
- disposable intermediate context;
- redundant raw tool output after required evidence is extracted;
- abandoned temporary artifacts.

### Transferable principle

> **Persistence must be earned by a successor need, not inherited because the agent happened to see or create something.**

---

# 7. Control plane and execution plane are not prompt text

The conference material repeatedly separates model reasoning from operational control.

A useful HE decomposition is:

```text
MODEL
  ↓
TASK / CONTEXT SCAFFOLDING
  ↓
CONTROL PLANE
  ↓
EXECUTION PLANE
  ↓
ASSURANCE PLANE
```

## Task / context scaffolding

Examples:

- task prompt;
- plans;
- skills;
- retrieval;
- routing;
- compacted state;
- examples.

This layer is most exposed to model-capability drift.

## Control plane

Examples:

- triggers;
- admission/quotas;
- identity;
- permissions;
- budgets;
- approval state;
- lifecycle/state transitions.

## Execution plane

Examples:

- workspace;
- sandbox;
- shell/tools;
- compute;
- filesystem;
- network;
- connectors.

## Assurance plane

Examples:

- traces;
- deterministic checks;
- evaluators;
- approval receipts;
- effect verification;
- regression gates.

The term "harness" may still refer to the whole system, but HE should distinguish these responsibilities when assigning ownership or evaluating removal.

---

# 8. Capability, not secret

Google DeepMind's Managed Agents example keeps real credentials outside the model's sandbox. The agent sees a placeholder/dummy value; an outbound proxy substitutes the actual secret only for the configured destination.

The exact implementation is product-specific. The durable design rule is not.

### Transferable principle

> **Give the model the capability required for the task without exposing the underlying reusable secret when the runtime can enforce that separation.**

This complements least privilege and destination-bound authority.

---

# 9. Approval requests are behavioral; approval enforcement is deterministic

Anthropic's financial-services example separates:

- an agent proposing a booking amendment;
- a human approving it;
- a deterministic control preventing application before approval.

Google DeepMind similarly distinguishes model-facing instructions/skills from protected hooks around consequential tool calls.

### Transferable principle

> **The model may know that approval is required; the action boundary should enforce it when consequence warrants enforcement.**

Do not rely on "remember to ask" as the only control for irreversible or high-consequence actions.

---

# 10. Coordinator/worker fan-out is justified by information value

The Turing Post material shows a coordinator dispatching independent workers for database migration, code changes, and slowdown reproduction.

This is valid when work is meaningfully decomposable, but the source set does not support "more workers = better."

Each worker creates another context, execution path, cost surface, and fan-in problem.

### HE implication

Fan-out should be justified by:

- independence;
- specialization;
- uncertainty reduction;
- parallel critical-path value;
- independent corroboration.

The coordinator should consume **results/evidence**, not full worker transcripts by default.

### Transferable principle

> **Fan out for independent information value, not agent count.**

---

# 11. Epistemic state must survive compaction and handoff

The video uses a failure mode where uncertainty in evidence can be lost when a summary turns "probably caused" into "caused."

HE already distinguishes source truth, generated summaries, and evidence quality. This pass sharpens the requirement:

Compaction/handoff should preserve whether a statement is:

- verified;
- inferred;
- speculative/hypothesis;
- unresolved;
- failed attempt;
- decision;
- constraint.

### Transferable principle

> **Compaction must preserve epistemic state, not merely semantic gist.**

A shorter summary that upgrades uncertainty into fact is a correctness regression.

---

# 12. Every harness compensation needs an admission and retirement test

The sources converge on a lifecycle problem: models improve, but compensating instructions and machinery tend to remain.

HE already says harness evolution includes deletion. This pass makes the mechanism explicit.

For a behavioral scaffold or workaround:

```text
Observed failure:
  model/runtime fails X

Compensation:
  add mechanism Y

Admission evidence:
  reproduction case Z improves with Y

Retirement test:
  after material model/runtime change,
  rerun Z without Y

If Z still passes:
  remove or reduce Y
```

Durable safety/control mechanisms may not be candidates for removal merely because the model improved; their purchased property may be external enforcement rather than compensating for weak reasoning.

### Transferable principle

> **Temporary model compensation should have an executable reason to exist and an executable way to prove it can disappear.**

---

# 13. Harness ablation protocol

The minimum agent loop plus HarnessTax/Apple evidence creates a useful experimental baseline.

For a stable task corpus:

```text
H0 = minimum credible harness
H1 = H0 + one mechanism
H2 = H1 + one mechanism
...
```

Measure at least:

- task success/correctness;
- token/provider cost;
- wall time;
- tool calls;
- retries/repeated reads;
- human intervention;
- constraint violations;
- recovery behavior;
- consequence-relevant failures.

Two complementary experiments matter:

## Harness ablation

Same model, same task corpus, different harness/configuration.

## Model ablation

Same harness/configuration, same task corpus, different model.

This helps separate model capability from harness contribution.

### Important control

For experimental work, automatic provider/model failover invalidates attribution unless failover itself is the treatment being evaluated.

---

# 14. Source-level caution

The evidence does **not** justify:

- deleting HE's current controls wholesale;
- claiming simple harnesses always outperform complex ones;
- assuming all multi-agent systems are waste;
- keeping agents continuously alive for long-horizon work;
- preserving every trace forever;
- treating a giant context window as durable memory/state;
- assuming stronger models remove the need for deterministic authorization;
- installing new infrastructure before a measured workload requires it.

---

# 15. Disposition

## STRONGLY REINFORCE

- smallest justified harness;
- evidence before architecture;
- bounded execution;
- human approval for consequential effects;
- deterministic enforcement where available;
- evidence-preserving fan-in;
- model/runtime revalidation;
- harness evolution includes deletion;
- cost measured per successful outcome.

## NEW DURABLE REFINEMENTS

- minimum credible harness as an experimental control;
- four-way separation: knowledge / task state / execution artifacts / inference context;
- Run Retention Contract;
- control-plane / execution-plane / assurance-plane responsibility split;
- epistemic-preserving compaction;
- admission + retirement tests for behavioral scaffolding;
- persist the job, not the inference.

## ASSESS

- a small HE Harness Ablation Protocol;
- model/harness cross-matrix using local models where practical;
- explicit run-retention contract in long-running workflows;
- event-driven task resumption where real external waits exist;
- structured epistemic labels in handoff/compaction artifacts where evidence shows drift.

## PARK

- generalized long-horizon scheduler;
- organization-wide agent control plane;
- automatic worker farms;
- permanent trace warehouse;
- generalized sandbox snapshot infrastructure.

## REJECT

- complexity as evidence of capability;
- multi-agent orchestration by default;
- unlimited context as a substitute for state;
- prompt-only approval for consequential actions when deterministic enforcement exists;
- preserving all execution exhaust because it might be useful someday.

## Bottom line

The strongest interpretation of this source set is not "harnesses are disappearing."

It is:

> **Behavioral scaffolding should be treated as provisional and measurable; operational controls should be judged by the property they enforce. HE should build the smallest measured harness that preserves the properties the workload actually requires, and should continuously test whether temporary scaffolding can be removed.**
