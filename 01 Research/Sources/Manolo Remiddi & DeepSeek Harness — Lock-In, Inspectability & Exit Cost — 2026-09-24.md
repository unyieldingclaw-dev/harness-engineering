# Manolo Remiddi & DeepSeek Harness — Lock-In, Inspectability & Exit Cost — 2026-09-24

## Source status

Secondary/scout source:

- Manolo Remiddi — “You're Not Locked In By The Model. You're Locked In By The Harness.”
- YouTube: https://www.youtube.com/watch?v=k8jYOcQUB_A
- User-provided transcript reviewed 2026-09-24.

Primary implementation evidence used to verify the technical claims worth retaining:

- DeepSeek Harness repository: https://github.com/deepseek-ai/deepseek-harness
- Architecture: https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md
- Safety: https://github.com/deepseek-ai/deepseek-harness/blob/master/SAFETY.md

This note preserves useful Harness Engineering mechanisms from the source. It does **not** adopt Manolo's product, sovereignty framing, business predictions, or DeepSeek Harness as an HE dependency.

---

## 1. The durable claim is harness dependence, not model irrelevance

Manolo separates the system into roughly:

```text
model weights
   ↓
inference/runtime
   ↓
provider/API
   ↓
harness
   ↓
interface
```

and attributes loops, tool calling, context, permissions, memory, hooks, and telemetry to the harness layer.

This independently reinforces the existing HE position that the evaluated behavioral unit is broader than the nominal model.

### Disposition

- **REINFORCE:** Model identity alone is not enough to explain agent behavior.
- **REINFORCE:** Context, tools, authority, memory, runtime, and verification must be considered part of the execution system.
- **REJECT:** “The model does not matter.” Model capability remains one material component of the system.

---

## 2. Exit cost is a useful missing portability dimension

The strongest new question from the video is not “is the harness open source?” It is:

> **If this harness/provider disappeared tomorrow, what would be expensive or difficult to move elsewhere?**

Exit cost can exist in:

- model/provider bindings;
- proprietary session formats;
- memory formats;
- instruction/skill conventions;
- tool protocols;
- plugin APIs;
- authentication and credentials;
- deployment/runtime assumptions;
- workflow semantics;
- hidden context or compaction behavior;
- project state trapped only inside conversation history.

Open source may reduce some exit costs, but license alone does not establish portability. A portable design should preserve important project truth and workflow state in formats that remain understandable outside the current harness.

### Candidate HE question

> **What durable state, behavior, or authority would be lost or require translation if this component were replaced?**

### Disposition

- **ASSESS:** Add exit cost to harness/runtime evaluation criteria.
- **REINFORCE:** Durable project truth should not depend on one conversation or provider.
- **REJECT:** Treating open source or model agnosticism alone as proof of low lock-in.

---

## 3. Inspectability is operationally useful without becoming observability infrastructure

Manolo highlights the ability to inspect:

- active system prompt/context;
- current context utilization;
- actions/tool calls;
- loops;
- permissions;
- token/cost information.

The durable HE mechanism is **effective-state inspectability**.

A useful future harness view may expose facts such as:

```text
active model + effort/runtime
loaded instructions
active skills/capabilities
available tools
current authority boundaries
context utilization / compaction state
run/task lifecycle state
verification state
```

This does not imply Datadog, Sentry, a central control plane, or a new Dashboard product.

### Disposition

- **REINFORCE:** Make materially important harness state inspectable when practical.
- **REINFORCE:** Observability consumes source-owned facts; it should not become a competing authority.
- **PARK:** UI/dashboard implementation until an observed operational need justifies it.

---

## 4. Compaction is not an authoritative memory system

The video correctly identifies summarization/compaction as potentially lossy and points toward external durable truth as a mitigation.

The useful HE conclusion is narrower than “every compaction makes the model dumber”:

> **Lossy conversation compaction must not become the authoritative store for durable project truth.**

Durable project state should remain reachable outside transient conversation so a successor session can reconstruct what matters without depending on repeated summaries of summaries.

### Disposition

- **REINFORCE:** Focused continuation beats transcript transplantation.
- **REINFORCE:** Current project truth belongs in durable, reachable artifacts.
- **REJECT:** Treating all compaction as failure or assuming larger context windows eliminate the need for durable state.

---

## 5. DeepSeek Harness is worth mining, not adopting

Direct repository inspection verifies several architectural claims in the video:

- the project is MIT licensed;
- it is explicitly a **developer preview** with compatibility-breaking changes expected;
- its architecture treats model adapters, tool registry, session log, agent loop, sandbox/policy, telemetry, and other capabilities as plugins or swappable seams;
- plugin registrations are designed as reversible effects;
- it distinguishes durable session events from live runtime events;
- the session log is the source from which model-visible history is derived;
- model-visible inputs are intended to be reconstructable from durable session events;
- tool execution exposes pre/post interception points;
- sandbox and permission controls are present but the project's own safety guidance says they are not sufficient isolation for untrusted workloads.

### HE mining targets

Worth deeper research if/when needed:

1. **Plugin lifecycle and reversibility** — capability registration/unload without hidden residual state.
2. **Capability seams** — provider/consumer ownership boundaries rather than one giant runtime object.
3. **Durable vs live events** — which facts must survive reload vs which are transient process state.
4. **Turn/step semantics** — explicit lifecycle boundaries around model requests and tool work.
5. **Tool interception** — deterministic pre/post execution gates.
6. **Session reconstruction** — model-visible state derived from durable events rather than opaque UI state.
7. **Sandbox composition** — how filesystem/subprocess/tool providers share or isolate an execution world.

### Current disposition

- **ASSESS:** Mine selected mechanisms as comparison evidence for HE.
- **PARK:** Adopting DeepSeek Harness as PMB/HE runtime.
- **REJECT:** Treating developer-preview software as production-ready or as the sole security boundary.

---

## 6. Claims intentionally not promoted

The video also makes business and ownership claims that are not needed for HE and are not sufficiently evidenced by the source itself, including specific OpenAI loss-per-dollar figures and implied future pricing consequences.

Do not retain those as architectural evidence.

Likewise, “sovereign user” is useful rhetoric for discussing control but is not an HE engineering requirement.

---

## Cross-source synthesis

This source independently reinforces several existing HE themes:

- the harness is the behavioral unit;
- durable state must outlive transient conversation;
- authority and sandbox boundaries matter as much as model intelligence;
- inspectability should expose effective runtime state;
- composability is useful when it lowers coupling and preserves ownership boundaries;
- portability should be judged by replacement/exit cost, not branding or nominal model support.

The genuinely new durable addition is **exit cost as an explicit evaluation dimension**.
