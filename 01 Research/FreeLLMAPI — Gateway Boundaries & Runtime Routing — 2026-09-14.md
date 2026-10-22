# FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14

## Purpose

Synthesize the durable Harness Engineering lessons from the FreeLLMAPI source review without importing its implementation or treating its free-tier economics as architecture requirements.

Primary evidence:

- `01 Research/Sources/FreeLLMAPI — Gateway Routing, Failover & Context Compression — 2026-09-14.md`

Related HE research:

- `01 Research/Context Engineering.md.md`
- `01 Research/Sources/Simon Willison — Harness Engineering Findings — 2026-09-01 through 2026-09-14.md`
- `01 Research/Sources/GitHub AI Repos — Top 10.md`
- `01 Research/Sources/Agent Harness & Workflow Repos — 2026-08-30.md`

This document records research implications only. It does not authorize implementation or modify HE decisions.

---

# Executive synthesis

FreeLLMAPI is useful to Harness Engineering primarily because it demonstrates the operational complexity that appears once model/provider routing becomes real infrastructure.

The central lesson is not that HE should become a smarter router. It is the opposite:

> Provider integration, quotas, health, retry semantics, protocol translation, and same-model failover are substantial gateway responsibilities. Harness Engineering should define the semantic expectations above that layer rather than duplicate it.

FreeLLMAPI also supplies strong implementation evidence for several existing HE themes:

- execution identity is more than a nominal model name;
- model switching is a handoff/state-transfer event;
- deterministic context reduction can be safer than LLM summarization;
- fallback/recovery should minimize semantic change;
- runtime adaptation should remain bounded, explainable, and reversible;
- execution provenance matters when a system silently changes route or provider.

---

# 1. Semantic-preserving recovery should precede substitution

## Observation

FreeLLMAPI groups equivalent logical models across providers and attempts in-group failover before moving farther down a heterogeneous fallback chain.

## HE interpretation

A recovery action should be evaluated partly by how much it changes the semantics of execution.

A useful recovery ordering is:

```text
retry same execution path when safe
    -> alternate credential/endpoint for same provider/model
    -> alternate provider serving same logical model
    -> declared-compatible alternate model
    -> explicit escalation/substitution
```

The exact steps will vary by environment. The durable principle is to prefer the least behavior-changing recovery that can plausibly restore service.

## Why this matters

Changing providers may alter execution details. Changing models can alter far more:

- reasoning behavior;
- tool-use reliability;
- context limits;
- structured-output behavior;
- safety middleware;
- latency/cost profile;
- instruction following.

A harness should not describe a cross-model substitution as if nothing material changed.

## Disposition

- **REINFORCE:** recovery should minimize semantic change where practical.
- **ASSESS:** whether this principle belongs in HE-001 as a generic runtime/recovery consideration.
- **REJECT:** automatic equivalence between nominally similar endpoints without evidence.

---

# 2. Model transition is a state-transfer boundary

## Observation

FreeLLMAPI explicitly recognizes that switching models during an ongoing conversation can break continuity. Its optional handoff mechanism tells the successor model that it is inheriting an active task and provides recent visible context.

## HE interpretation

Execution transfer should not be modeled as a simple pointer change.

When execution moves between models, providers, sessions, or agents, the system should consider which state must survive:

- user goal;
- accepted decisions;
- constraints;
- relevant artifacts;
- unresolved work;
- tool outputs;
- definition of done;
- verification evidence;
- previous execution identity;
- reason for transfer.

The state does not necessarily need to be one serialized handoff object. The important point is that continuity requirements are explicit.

## Constraint

No handoff can reconstruct provider-internal hidden state or context that never crossed the observable boundary.

## Disposition

- **REINFORCE:** explicit work transfer/handoff semantics.
- **ASSESS:** minimum state required for a bounded cross-model/session handoff.
- **REJECT:** assuming conversation continuity survives model/provider changes automatically.

---

# 3. Deterministic context reduction should be preferred where structure is known

## Observation

FreeLLMAPI implements request-side compression that can deduplicate repeated blocks, reduce repetitive command output, supersede stale file reads, compact structured data, and apply source-specific filters.

Critically, the built-in path is deterministic and fail-open. Transformations are rejected when fidelity checks fail.

## HE interpretation

This reinforces a broader context-engineering rule:

> If the information structure is known, reduce noise with deterministic transforms before asking another model to summarize or prune it.

Examples include:

- ANSI stripping;
- repeated-line collapse;
- test/build output filtering;
- stale duplicate file-read elimination;
- bounded head/tail retention;
- JSON structural compaction;
- exact command-specific filters.

## Fidelity is more important than compression ratio

A useful compressor must preserve the information required for correct reasoning and verification.

Potential protected classes include:

- explicit constraints;
- errors;
- diffs;
- numeric values;
- file paths;
- stack traces;
- tool-call/result envelopes;
- security instructions.

## Disposition

- **REINFORCE:** source-aware, deterministic context reduction.
- **ASSESS:** specific noisy-output classes observed in PMB/ACR/work workflows.
- **REJECT:** generic summarization solely because context is large.

---

# 4. Runtime adaptation needs bounded authority

## Observation

FreeLLMAPI combines observed reliability with deterministic speed/capability metadata and quota/rate-limit headroom. It also contains explicit controls to prevent dynamic adjustments from silently overriding some operator-selected policies.

## HE interpretation

Adaptive routing is useful only when its authority is bounded.

A general HE pattern is:

```text
measure
    -> adapt within declared policy
    -> preserve evidence of the adaptation
    -> retain operator override / rollback
```

The algorithm is secondary to the authority model.

This aligns with existing HE principles around bounded capability, explicit ownership, and human review over architecture changes.

## Disposition

- **REINFORCE:** bounded and explainable adaptation.
- **PARK:** Thompson Sampling or any specific bandit algorithm as an HE requirement.
- **REJECT:** opaque model-selected routing policy as architecture governance.

---

# 5. Provider error handling belongs below the Harness layer

## Observation

FreeLLMAPI's issue history shows that provider responses cannot be reliably classified by HTTP status alone.

Examples include:

- a provider returning HTTP 400 for temporary model degradation;
- a provider returning HTTP 400 for tool-call generation failure that should trigger another tool-capable route.

## HE interpretation

Correct failover requires provider-specific operational knowledge.

The gateway layer must often know whether an error means:

- invalid user request;
- unsupported parameter;
- unavailable model;
- provider degradation;
- rate limit;
- quota exhaustion;
- tool-generation failure;
- schema mismatch;
- authentication failure;
- transient transport failure.

This is strong evidence that provider retry/failover logic should not be casually duplicated by a general-purpose harness.

## Disposition

- **REINFORCE:** provider abstraction is an inference-gateway concern.
- **REJECT:** HE owning provider-specific HTTP/error taxonomy without a demonstrated requirement.

---

# 6. Execution provenance must survive fallback

## Observation

FreeLLMAPI records why candidate routes were unavailable and can summarize recovery/fallback state instead of returning only an opaque failure.

## HE interpretation

When runtime behavior changes materially, outcome-only telemetry is insufficient.

For controlled evaluation or debugging, useful minimal provenance may include:

```text
requested capability/model
selected model/provider/runtime
reason selected
fallback/escalation events
failure category
final execution identity
```

Capture should remain discriminating and lightweight.

This extends the existing HE finding that nominal model name may be insufficient execution identity when backend/provider/runtime conditions differ.

## Disposition

- **REINFORCE:** execution provenance across fallback and model transitions.
- **ASSESS:** minimum fields needed for HE-controlled evaluations.
- **REJECT:** building a general observability platform without evidence of need.

---

# 7. Quota and capacity are scoped facts, not headline numbers

## Observation

FreeLLMAPI's own issue history shows how easy it is to overstate free capacity when shared quotas are counted per model instead of per provider pool.

## HE interpretation

Capacity claims need a scope.

Relevant scope dimensions may include:

- account;
- provider;
- model;
- shared quota pool;
- credential/key;
- minute/day/month window;
- concurrency;
- token vs request limits;
- documented cap vs theoretical rate ceiling.

## Disposition

- **REINFORCE:** operational measurements require explicit scope.
- **REJECT:** token-count headlines as evidence of usable capacity.

---

# 8. Gateway and Harness responsibilities should remain distinct

## Inference gateway responsibilities

The FreeLLMAPI case supports assigning these concerns below the Harness layer:

- credentials;
- provider adapters;
- wire/API translation;
- provider health;
- quota/rate-limit accounting;
- retry/cooldown;
- same-model provider failover;
- low-level capability eligibility;
- raw route diagnostics.

## Harness responsibilities

HE remains concerned with higher-level semantics and governance:

- capability required for the task;
- context ownership and retrieval;
- workflow/capability composition;
- whether model substitution is permitted;
- state-transfer requirements;
- escalation policy;
- verification requirements;
- artifact ownership;
- human approval boundaries.

## Important overlap

The layers may exchange metadata, but ownership should stay clear.

For example:

- gateway reports `model X unavailable because tool capability missing`;
- Harness decides whether a different capability/model is acceptable.

The gateway should not silently redefine the task requirement, and the Harness should not need to understand every provider's retry semantics.

## Disposition

- **REINFORCE:** clear gateway/harness responsibility boundary.
- **ASSESS:** interfaces required between those layers.
- **REJECT:** collapsing inference infrastructure and harness governance into one routing subsystem by default.

---

# 9. External capability catalogs should not receive implicit governance authority

## Observation

FreeLLMAPI uses a signed remotely updated catalog for models, quotas, compatibility metadata, and provider quirks.

## HE interpretation

Cryptographic authenticity answers:

> Did this metadata come from the trusted publisher unchanged?

It does not answer:

> Should this change alter execution policy in this governed environment?

For enterprise or controlled evaluation use, a stronger pattern may be:

```text
retrieve
verify signature
review diff
pin/approve
promote
rollback if necessary
```

The degree of control should match the risk of the metadata.

## Disposition

- **ASSESS:** signed/versioned capability catalogs as external evidence sources.
- **PARK:** automatic catalog-driven routing changes as an HE default.

---

# 10. Relationship to existing HE research

## Context Engineering

FreeLLMAPI reinforces existing research that:

- deterministic tools should perform deterministic work;
- tool-output shape materially affects context quality;
- context cost must be measured by source category;
- context reduction must preserve correctness, not just shrink tokens.

## Simon Willison findings

It reinforces the existing execution-provenance finding that provider/backend identity can materially affect model behavior.

## Model Routing & Escalation thread

It strengthens the existing research question preserved in `GitHub AI Repos — Top 10.md`:

> What model-routing responsibilities, if any, should a harness own versus an inference gateway such as LiteLLM?

Current evidence now leans strongly toward:

- provider mechanics below the Harness;
- semantic capability/escalation policy above the gateway.

This is a research conclusion, not a new architecture decision.

## Agent Harness & Workflow research

FreeLLMAPI's compression pipeline reinforces the OmniRoute-derived finding that tool output and prose should not be treated as one generic compression problem.

---

# Consolidated research findings

## REINFORCE

1. Recovery should prefer the least behavior-changing viable path.
2. Model/provider/session changes are potential state-transfer boundaries.
3. Deterministic context reduction is preferred where structure is known.
4. Compression requires fidelity checks and recovery/fail-open behavior.
5. Runtime adaptation should be bounded and explainable.
6. Execution provenance should include material fallback/substitution events.
7. Provider-specific retry/error semantics belong in the gateway layer.
8. Capacity measurements require explicit quota scope.
9. Harness and inference gateway responsibilities should remain distinct.

## ASSESS

1. Minimum cross-model/session handoff state.
2. Minimum execution provenance for controlled HE evaluations.
3. Deterministic tool-output filters justified by observed PMB/ACR/work noise.
4. Interface between Harness capability requirements and gateway eligibility/failure metadata.
5. Pin/review policy for externally updated capability catalogs in governed environments.

## PARK

- Thompson Sampling as a standard HE mechanism.
- General-purpose model-ranking infrastructure.
- Provider catalog ownership in HE.
- Fusion/multi-model synthesis as default architecture.
- Time-of-day adaptive routing.

## REJECT

- Treating free-token totals as reliable capacity guarantees.
- Treating nominally identical model names as proven identical execution.
- Treating model substitution as semantically transparent.
- Copying FreeLLMAPI implementation code into HE.
- Rebuilding provider routing and error taxonomy in the Harness without a concrete requirement.

---

# Research disposition

**MINE SELECTIVELY / REINFORCE EXISTING THREADS.**

FreeLLMAPI provides strong evidence for runtime/gateway concerns and for the boundary between inference infrastructure and Harness Engineering.

It does **not** currently justify:

- a new HE routing subsystem;
- a new architecture decision;
- provider-specific code inside HE;
- copying FreeLLMAPI source;
- changing PMB or ACR implementation.

Future implementation should occur only when an observed problem and an HE decision justify it.
