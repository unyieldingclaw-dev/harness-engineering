# CloudYeti — Harness Engineering, Guides/Sensors & Agent Legibility — 2026-10-21

## Source and evidence boundary

### Discovery / synthesis source

- User-supplied screen recording of a YouTube Short by **@CloudYeti**.
- Visible title: **What Is Harness Engineering? Explained in 3 Minutes**.
- Reviewed 2026-10-21 from the supplied recording and extracted frames.
- The short is treated as a **secondary synthesis source**, not the authority for the underlying claims.

Visible claims/examples in the short include:

- "Agent = Model + Harness"
- "Give the agent a map, tools, and memory"
- "AGENTS.md became a map, not the manual"
- "Guides before. Sensors after."
- a policy-gate example that blocks a destructive Terraform plan pending approval
- "Move memory out of chat and into the repo"
- "Make the result visible to the agent"
- browser / screenshot / logs / verification as post-change evidence

### Primary sources checked

1. OpenAI — **Harness engineering: leveraging Codex in an agent-first world**, Ryan Lopopolo, 2026-02-11  
   https://openai.com/index/harness-engineering/

2. Birgitta Böckeler / Martin Fowler — **Harness engineering for coding agent users**, 2026-04-02  
   https://martinfowler.com/articles/harness-engineering.html

The primary sources support much of the short's core framing, but not every visual/example should be attributed to OpenAI.

---

# 1. "Agent = Model + Harness" is useful shorthand, not a sufficient HE decomposition

Böckeler explicitly describes the broad usage of "harness" as everything in an agent except the model and then narrows that definition for coding-agent users.

The short uses:

```text
AGENT = MODEL + HARNESS
```

That is a useful teaching simplification.

HE already has a more precise responsibility model:

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

### Disposition

**REINFORCE**, not a new architecture.

Use "model + harness" as introductory vocabulary; use HE's responsibility decomposition when assigning ownership or deciding what can actually enforce a property.

---

# 2. "AGENTS.md became a map, not the manual" is directly supported by OpenAI

OpenAI reports that a large monolithic `AGENTS.md` failed because:

- context is scarce;
- too much guidance becomes non-guidance;
- the file rots;
- a single blob is difficult to verify mechanically.

Their replacement is a short `AGENTS.md` used primarily as a table of contents that points to structured repository-local sources of truth.

OpenAI explicitly describes this as progressive disclosure.

### HE relationship

This strongly reinforces existing HE work on:

- progressive disclosure;
- small routing surfaces;
- one authoritative owner per fact;
- generated/derived context versus source truth;
- avoiding always-loaded procedural sediment.

### Durable refinement

> **Always-loaded guidance should route to owned truth more often than it restates that truth.**

This does **not** imply a universal line-count threshold for `AGENTS.md`.

---

# 3. Guides and sensors provide useful control vocabulary

Böckeler defines two complementary control directions:

## Guides — feed-forward

Act before the model's action to increase the chance of a good first result.

Examples include:

- repository instructions;
- reference documentation;
- skills;
- coding conventions;
- scripts / code-mod tools;
- architecture guidance.

## Sensors — feedback

Observe after action and provide evidence for correction.

Examples include:

- tests;
- linters;
- type checkers;
- structural analysis;
- logs;
- browser inspection;
- review agents.

A guide without a sensor can encode a rule without establishing whether it worked.

A sensor without a guide can repeatedly detect avoidable failures after the fact.

### Transferable principle

> **Guides steer before action; sensors establish feedback after action. A reliable loop may require both, but only where the workload consumes the property.**

---

# 4. Computational vs inferential is an orthogonal axis

Böckeler separately distinguishes:

## Computational controls

- deterministic;
- fast;
- typically CPU/tool based;
- examples: tests, linters, type checking, structural checks.

## Inferential controls

- semantic/model-mediated;
- slower and more expensive;
- non-deterministic;
- examples: semantic review, AI code review, LLM-as-judge.

This is useful because "guide vs sensor" and "deterministic vs probabilistic" answer different questions.

A control can be:

```text
computational guide
inferential guide
computational sensor
inferential sensor
```

### HE relationship

This sharpens, but does not replace, HE's existing distinctions between:

- deterministic checks and model judgment;
- producer feedback and independent verification;
- advisory guidance and runtime enforcement.

### Transferable principle

> **Classify a harness control by both when it acts and how its result is established.**

---

# 5. Make the environment legible before asking the model to "try harder"

OpenAI reports that early progress was limited because the environment lacked tools, abstractions, and internal structure required for the agent to make progress.

Their response to failure was not primarily to increase model effort. They asked what capability was missing and how to make that capability legible and enforceable.

They later exposed:

- isolated runnable application instances;
- browser / DOM / screenshot access;
- logs;
- metrics;
- traces;

so Codex could reproduce bugs and verify changes directly.

This independently corroborates the recent Pocock interview evidence that better executable feedback can materially improve agent work.

### HE diagnostic

When agent output is poor, test these questions before assuming a stronger model or more reasoning effort is required:

1. Is the goal / acceptance intent clear?
2. Can the agent inspect the relevant state?
3. Does it have the authorized capability needed to act?
4. Can it observe the real effect?
5. Is there an acceptance-relevant sensor/check?
6. Only then: is the remaining gap plausibly model capability or reasoning effort?

This is a diagnostic ordering, not a mandatory workflow.

### Transferable principle

> **Before escalating model intelligence, improve what the agent can see, do, and verify when those are the actual missing properties.**

---

# 6. Result visibility is effect verification

The short's "make the result visible to the agent" example uses:

- browser loaded;
- screenshot captured;
- logs inspected;
- behavior verified.

OpenAI describes the same general mechanism: launch the changed application, drive it, inspect screenshots/DOM/logs/metrics, reproduce a failure, and validate the fix.

### HE relationship

**STRONGLY REINFORCES**:

- "Eyes as Evidence Acquisition";
- HE-001 Loop-Based Execution;
- execution success vs effect verification;
- observable postconditions;
- independent quality/evaluation boundaries.

### Boundary

A screenshot, passing test, or log line is not automatically sufficient evidence.

The evidence must correspond to the actual acceptance criterion.

---

# 7. "Move memory out of chat and into the repo" needs a narrower HE formulation

The short visually contrasts temporary chat state with durable repository artifacts such as:

- `progress.md`;
- `decision-log.yaml`;
- `gotchas/`.

OpenAI does make repository knowledge a system of record and checks complex execution plans, progress, decisions, and technical debt into the repository.

HE should preserve the more precise rule already established:

```text
knowledge
task state
execution artifacts
inference context
```

These are different resources with different owners and lifecycles.

### HE refinement

> **Move durable task state and earned project knowledge out of transient chat into the authoritative persistent owner.**

That owner may be Git, an issue/PR, PMB, a runtime state store, or another source-owned system depending on the fact.

### Reject

- treating every chat observation as memory;
- creating repository logs merely because persistence is available;
- duplicating authoritative facts into generic `memory.md` / `notes.md` files without a successor need.

---

# 8. Approval gate is control-plane evidence, not merely a "sensor"

The short shows a destructive Terraform plan being stopped pending approval.

The useful HE point is not the exact label attached to the mechanism.

The durable property is:

```text
proposed consequential action
       ↓
reviewable / detectable effect
       ↓
approval state
       ↓
runtime gate
       ↓
allowed or blocked
```

This directly reinforces **Bounded Execution Envelopes**:

> The model may know approval is required; the action boundary should enforce it when consequence warrants enforcement.

### Important distinction

A test/analysis may **sense** that an action is dangerous.

The runtime/policy gate **controls** whether the action is allowed.

Do not collapse observation and authority into the same concept.

---

# 9. Implication for specialist-agent admission

This source does not argue for more agents.

In fact, it provides a cheaper-alternative test before introducing another specialist or subagent.

Before creating a separate reasoning role, ask whether the missing property is actually:

- context -> expose/retrieve it;
- capability -> add the bounded tool;
- deterministic correctness signal -> add a sensor/check;
- reusable procedure -> add a skill;
- hard authority boundary -> add a runtime/control-plane rule;
- genuinely independent/specialized reasoning -> only then consider a separate agent role.

### Transferable principle

> **Do not create an agent to compensate for a missing tool, missing sensor, missing context, or enforceable policy boundary.**

This is compatible with HE's existing minimum-sufficient-mechanism and harness-ablation principles.

---

# 10. Source-level caution

The short is a compressed explainer. It should not be used to prove:

- that repository storage is always the correct memory owner;
- that app work and cloud work form a durable architecture boundary;
- that every harness needs every illustrated component;
- that all approval gates are sensors;
- that "Agent = Model + Harness" is precise enough for ownership analysis;
- that more harness machinery is necessarily better.

OpenAI also explicitly says the autonomy demonstrated in its internal product depends heavily on that repository's particular structure and tooling and should not be assumed to generalize automatically.

Böckeler similarly warns that building coherent guides, sensors, and self-correction loops is expensive and should direct human attention to the places where it is most valuable, not necessarily eliminate human input.

---

# 11. Disposition

## STRONGLY REINFORCE

- progressive disclosure;
- repository-local/source-owned durable knowledge where appropriate;
- agent legibility;
- "Eyes as Evidence Acquisition";
- loop-based execution;
- deterministic checks before probabilistic review;
- bounded approval enforcement;
- environment quality as part of harness capability.

## NEW DURABLE VOCABULARY

- **Guide** — feed-forward control before action.
- **Sensor** — feedback control after action.
- **Computational / inferential** — orthogonal execution type for either guide or sensor.

## SHARPEN

- always-loaded guidance should route to owned truth rather than duplicate it;
- poor results may indicate missing observability/capability/feedback before weak model intelligence;
- "move memory to repo" becomes "move durable state/knowledge to the authoritative persistent owner";
- sensing a condition and authorizing an action are distinct responsibilities.

## ASSESS

- whether important workflows have both useful guidance and acceptance-relevant feedback;
- whether current failures come from missing environment affordances before escalating model effort;
- whether a specialist role is substituting for a cheaper context/tool/sensor/policy mechanism.

## PARK

- generalized harness templates;
- recurring doc-gardening agents;
- continuous autonomous cleanup;
- repository-wide persistent memory conventions.

## REJECT

- app-work vs cloud-work as an HE architectural split;
- every chat detail becoming durable repository state;
- new agents as the first response to missing feedback/tooling;
- universal monolithic instruction manuals.

## Bottom line

The short is useful because it compresses several real harness-engineering lessons into an approachable model. The primary sources make the durable conclusion clearer:

> **Give the agent enough guidance to navigate intentionally, enough capability to act, enough sensors to observe real effects, and enough deterministic control to enforce consequential boundaries. Improve those properties before assuming the solution is simply a smarter model or another agent.**
