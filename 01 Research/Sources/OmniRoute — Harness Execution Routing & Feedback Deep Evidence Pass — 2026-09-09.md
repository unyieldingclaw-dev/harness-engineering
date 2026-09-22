# OmniRoute — Harness Execution Routing & Feedback Deep Evidence Pass

**Date:** 2026-09-09  
**Corpus role:** execution-layer prior art / comparative synthesis  
**Repository:** https://github.com/diegosouzapw/OmniRoute  
**Reviewed revision:** `949235736042b13cf64215632e6d44db7985af76` evidence surfaced from current repository documentation/code search  
**Relationship:** HIGH-VALUE RESEARCH; MERGES WITH execution-profile/routing/provenance work; ORTHOGONAL TO PMB durable-memory architecture.

## Executive finding

OmniRoute is presented publicly as a multi-provider AI gateway, but the strongest Harness Engineering lessons are not its provider catalog or free-tier aggregation. They are its separation of **fast request execution from slower feedback/intelligence**, explicit routing policy, hard-vs-soft provider decisions, operational-vs-semantic quality, sample-aware adaptive scoring, bounded telemetry, and routing explainability.

The central architectural pattern is:

```text
Agent / IDE
    |
    v
+---------------------------+
| DATA PLANE                 |
| routing / failover        |
| health / guardrails       |
| cache / streaming         |
+-------------+-------------+
              |
        bounded RoutingEvent
              |
              v
+---------------------------+
| CONTROL / INTELLIGENCE     |
| quality / evaluation       |
| telemetry / explanation    |
+-------------+-------------+
              |
              v
        adaptive preference
```

This is strong prior art for a Harness principle: **execution should not synchronously depend on observability, evaluation, or historical analysis unless that dependency is itself required for the decision.**

## Evidence boundary

This entry is based on current public repository README/documentation and implementation-oriented repository search. It records source-derived claims separately from architectural inference. No independent benchmark reproduction was performed.

## Corpus relationships

| Existing entry | Relationship | Why |
|---|---|---|
| Adrian Cockcroft | **MERGES WITH / SHARPENS** | Execution identity and runtime configuration belong in the execution layer. |
| Ras Mic / Ralphy | **MERGES WITH** | Model/provider overrides, fallback, capability negotiation, bounded execution and operational diagnostics. |
| David Ondrej | **MERGES WITH** | Execution vs discipline, capability/credential/cost/provenance/authority separation. |
| Rta-Smriti Brain | **ORTHOGONAL / SHARPENS** | Evidence provenance and confidence boundaries apply to routing feedback, but routing remains execution infrastructure. |
| OpenMAIC | **ORTHOGONAL / SHARPENS** | Durable state/context/runtime separation; provider configuration should not become PMB memory. |
| Context-efficiency bundle | **MERGES WITH** | Provider routing is one layer of context/execution economics; not a PMB memory mechanism. |
| Archify | **ORTHOGONAL** | Different execution concern; both benefit from explicit provenance and explainability. |

No existing entry is superseded.

# 1. Data plane vs control/intelligence plane

OmniRoute's adaptive-routing documentation explicitly separates the request hot path from the control plane. The hot path remains fast, asynchronous, memory-efficient and predictable. Routing outcomes are emitted as typed events and consumed by bounded in-memory sinks; persistence and external telemetry are kept off the synchronous path.

The `RoutingEvent` contains routing metadata such as request ID, provider, model, strategy, latency, TTFT, token counts, cost, retries, fallback use, outcome, status, finish reason and timestamp. The sink contract is designed to avoid synchronous I/O. The documented implementation dispatches events after request completion and keeps OTel export asynchronous. fileciteturn530file0L2-L2

### Harness lesson

This is stronger than generic “async logging.” It is an authority boundary:

```text
execution decision
      |
      +--> must remain available and bounded
      |
      +--> produces evidence
                |
                +--> telemetry
                +--> evaluation
                +--> historical analysis
```

Feedback consumers should not silently acquire authority over the execution hot path.

**Disposition:** ADOPT DESIGN PRINCIPLE — separate execution hot path from feedback/control plane.

# 2. Operational quality != semantic quality

OmniRoute explicitly separates operational quality from semantic quality. Operational quality comes from routing/runtime signals: HTTP failures, 429s, connection failures, malformed responses, interruptions, finish-at-length, zero-output successes and latency/TTFT behavior. A successful HTTP 200 is not treated as semantic quality.

Semantic quality is separately supplied by an evaluator and remains null until an evaluator provides it. The implementation keeps semantic quality out of the operational score. fileciteturn530file0L2-L2

### Harness lesson

This gives a useful general evidence ladder:

```text
reachable
   !=
operationally healthy
   !=
produced an answer
   !=
answer was useful
   !=
answer was correct
```

Do not collapse these into a single confidence value.

**Disposition:** ADOPT DESIGN PRINCIPLE — keep operational, semantic, and correctness evidence distinct.

# 3. Hard exclusion vs soft preference

The adaptive quality signal is explicitly a **soft preference**. Hard exclusion remains with deterministic resilience state such as circuit-breaker OPEN, exhausted quota, authentication failure, or model lockout. A temporary quality decline de-preferences a provider but does not itself hard-disable it.

This is a particularly strong pattern for governed Harness decisions:

```text
fuzzy/adaptive evidence
       -> preference

deterministic policy violation/failure
       -> exclusion
```

### Harness lesson

> **Use soft evidence to rank where appropriate; reserve binary authority for evidence and policy strong enough to justify it.**

This complements PMB's existing bounded-authority model.

**Disposition:** ADOPT DESIGN PRINCIPLE.

# 4. Sample-aware adaptive scoring

OmniRoute's quality tracker uses EWMA-style state and sample-aware confidence. A provider/model with no observations remains neutral rather than being either rewarded or punished. As observations accumulate, the measured operational signal has increasing influence; isolated failures do not destroy a previously healthy provider, and recovery is gradual.

The documented scoring behavior blends the observed operational score toward neutral according to sample count, with 50 samples as the stated convergence point. fileciteturn530file0L2-L2

### Harness lesson

This generalizes beyond routing:

> **Evidence strength should bound the influence of adaptive evidence on a decision.**

A single observation should not normally be able to overpower a large body of established evidence.

This is relevant to future PMB memory admission, reviewer reputation, architecture inference, provider selection, and other adaptive mechanisms.

**Disposition:** ADOPT EVALUATION PRINCIPLE — sample-aware influence; HIGH-VALUE RESEARCH — bounded adaptive evidence in Harness decisions.

# 5. Explainability as execution provenance

OmniRoute retains bounded routing events and exposes an explainability endpoint that returns recent decisions and provider/model quality state. Combo-level traces also exist.

This enables a useful operational question:

> **Why did the Harness choose this execution target?**

That is materially different from merely logging that a request went to provider X.

A useful execution provenance record should eventually support:

- task/request identity;
- execution profile;
- selected provider/model/runtime;
- routing strategy;
- relevant policy state;
- fallback/retry behavior;
- bounded outcome evidence;
- timestamp/revision/config identity;
- reason or scoring factors sufficient to reconstruct the decision.

**Disposition:** ADOPT DESIGN PRINCIPLE — execution choices should be explainable after the fact without retaining prompts or sensitive payloads unnecessarily.

# 6. Bounded telemetry and failure isolation

The OTel sink is optional and asynchronous. It is not registered unless configured. It enqueues bounded events and flushes in the background. Under overload, old telemetry can be dropped rather than backpressuring the data plane. The documentation explicitly states that telemetry unavailability does not stop routing. fileciteturn530file0L2-L2

This is an excellent operational pattern:

```text
telemetry unavailable
        |
        v
lose observability
NOT
lose execution
```

The same principle should apply to nonessential Harness diagnostics and evaluation infrastructure.

**Disposition:** ADOPT DESIGN PRINCIPLE — non-authoritative observability must fail independently and must not silently acquire execution authority.

# 7. Routing strategy should remain explicit

Repository code exposes multiple strategies and explicit fallback behavior. The adaptive routing documentation includes strategy identity and scoring factors. Code search shows a rules-based scorer using factors such as quota, health, cost and task fit, with separate configuration and fallback paths. fileciteturn529file12L249-L256

This supports keeping routing policy explicit rather than hiding it inside an opaque “smart router.”

For Harness Engineering, an execution profile should be able to state at least:

```text
model/provider identity
routing strategy
fallback policy
cost/latency constraints
capability constraints
credential boundary
authority boundary
```

The router may implement the policy; PMB should not own it.

**Disposition:** MERGES WITH — Execution Profile research.

# 8. Do not conflate model identity with execution identity

OmniRoute's architecture reinforces a distinction already established elsewhere in the corpus: the model name alone does not describe execution. Provider, endpoint, routing strategy, runtime configuration, fallback state and quality/health state can all affect what actually happened.

This strengthens the proposed Harness provenance model:

```text
Model identity
     +
Provider identity
     +
Execution/runtime configuration
     +
Routing policy
     +
Observed execution state
     =
Execution identity
```

The exact schema remains an experiment, not a final contract.

**Disposition:** ADOPT PRINCIPLE / MERGES WITH existing Execution Profile research.

# 9. What OmniRoute does NOT justify

The repository is large and ambitious. Its provider catalog, free-tier aggregation, dashboard, multimodal bridge, compression stack, and ecosystem are product concerns rather than reasons to expand PMB.

Do **not** infer from OmniRoute that PMB should own:

- provider catalogs;
- free-tier aggregation;
- provider-specific credentials;
- routing dashboards;
- model-serving infrastructure;
- universal context compression;
- provider routing policy as durable memory.

Those belong in execution infrastructure and should remain replaceable.

# 10. Proposed Harness experiment

A minimal experiment could test whether an explicit execution profile plus bounded routing provenance improves reproducibility and diagnosis without adding meaningful startup cost.

### Experiment

Run the same representative task under two execution configurations:

1. direct model selection;
2. explicit router with deterministic fallback and bounded decision trace.

Record:

- task success;
- selected model/provider/runtime;
- latency/cost;
- fallback/retry behavior;
- operational failures;
- semantic evaluation where available;
- ability of a fresh session to explain why the execution target was selected;
- provenance size;
- added startup/context cost.

Do not optimize provider count or free-token volume in this experiment.

# 11. Consolidated dispositions

| Finding | Disposition |
|---|---|
| Data plane / control plane separation | **ADOPT DESIGN PRINCIPLE** |
| Operational vs semantic quality | **ADOPT DESIGN PRINCIPLE** |
| Soft preference vs hard exclusion | **ADOPT DESIGN PRINCIPLE** |
| Sample-aware adaptive evidence | **ADOPT EVALUATION PRINCIPLE / HIGH-VALUE RESEARCH** |
| Explainable routing decisions | **ADOPT DESIGN PRINCIPLE** |
| Bounded asynchronous telemetry | **ADOPT DESIGN PRINCIPLE** |
| Explicit routing strategies | **MERGES WITH EXECUTION PROFILE** |
| Model vs execution identity | **MERGES WITH EXECUTION PROFILE** |
| Provider/free-tier ecosystem | **RELATED PRODUCT PRIOR ART — NO PMB ADOPTION** |
| OmniRoute as PMB dependency | **DO NOT ADOPT** |

## Bottom line

OmniRoute is useful to us **not because we need OmniRoute**.

It gives another concrete implementation of a Harness architecture in which:

> **execution is fast and bounded; evidence is emitted separately; adaptive intelligence consumes that evidence without silently becoming the execution authority; and the resulting decision remains explainable.**

That is a strong convergence with the execution-profile, provenance, bounded-authority, and evidence-discipline work already in the corpus.

The right question for PMB is therefore not “Should we add OmniRoute?” It is:

> **What execution facts must PMB/Harness preserve so that a later session can understand what model/runtime actually performed the work and why that execution path was chosen?**
