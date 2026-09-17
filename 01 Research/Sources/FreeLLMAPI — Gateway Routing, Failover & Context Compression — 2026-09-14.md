# FreeLLMAPI — Gateway Routing, Failover & Context Compression — 2026-09-14

## Purpose

Research intake from:

- Video: **Free LLM - for building, agents, images, etc.** — The Next New Thing
- YouTube: https://www.youtube.com/watch?v=ANJTdT0Ggrw
- Repository: https://github.com/tashfeenahmed/freellmapi

This is a source-derived research artifact for Harness Engineering. It records what the project actually implements, where the video framing is accurate or overstated, and which mechanisms are worth further HE analysis.

It is **not** a recommendation to adopt FreeLLMAPI, copy its code, depend on its free-tier economics, or add a provider router to Harness Engineering.

---

## Executive finding

The video's free-token story is the least important part for HE.

The stronger engineering value is in the implementation beneath it:

- same-model-first provider failover;
- explicit model/provider/session affinity;
- bounded fallback with route diagnostics;
- adaptive routing with reliability/speed/capability/headroom inputs;
- deterministic context compression with fidelity gates;
- explicit context handoff on model transitions;
- protocol translation and client compatibility boundaries;
- quota/cooldown enforcement below the agent layer;
- observable recovery/fallback behavior;
- clear examples of provider-specific failure semantics.

The repository is useful primarily as evidence about **gateway/harness responsibility boundaries**, not as a pattern for putting more routing intelligence into the harness.

---

# 1. Reality check against the video

## Video framing

The video presents FreeLLMAPI as a way to combine free quotas from many AI providers behind one API, automatically fail over between providers, keep the same model where possible, and use the result from coding agents such as Claude Code and OpenCode.

The current repository supports the core architectural claims, but the implementation has evolved beyond the demo.

Current README claims:

- 34 providers;
- 474 model families;
- 635 provider/model endpoints;
- OpenAI-compatible, Anthropic Messages, Gemini, and Ollama-compatible surfaces;
- smart routing and failover;
- unified models and named profiles;
- sticky sessions and context handoff;
- prompt compression;
- encrypted provider keys;
- self-updating signed catalog metadata.

Source:
https://github.com/tashfeenahmed/freellmapi/blob/main/README.md

### Important correction

The video states 45 providers. The current project documentation says 34. Treat provider/model counts as volatile catalog metadata rather than durable architecture facts.

### Free-token headline requires caution

The repository currently markets roughly 7.4B tokens/month of aggregate free capacity, but those numbers are not equivalent to a guaranteed account-level monthly budget.

Issue #905 explicitly distinguishes:

- documented quota pools;
- rate-limit-only theoretical ceilings;
- values that should not be mixed into one apparent budget.

https://github.com/tashfeenahmed/freellmapi/issues/905

Issue #1065 exposed a real accounting failure where shared provider pools were counted once per model. One user's dashboard showed 2.37B tokens where the pool-deduplicated value was 294M. The issue was fixed, but it is useful evidence that aggregate capacity arithmetic can become misleading when quotas are shared across models.

https://github.com/tashfeenahmed/freellmapi/issues/1065

### HE implication

Do not treat nominal token capacity, model count, or provider count as a trustworthy operational capability without knowing the quota scope, concurrency, reset window, provider restrictions, and shared-pool semantics.

---

# 2. Same-model-first failover

FreeLLMAPI distinguishes between:

1. the same logical model available through multiple providers; and
2. changing to a different logical model when the preferred model cannot serve the request.

The README describes **unified models** where the same model served by multiple providers is collapsed into one logical entry with strict in-group failover before falling through a larger fallback profile.

Relevant implementation/documentation:

- `server/src/services/model-groups.ts`
- `server/src/services/router.ts`
- `docs/en/architecture/00-high-level-index.md`

### Useful pattern

Recovery should prefer the least behavior-changing alternative first.

Conceptually:

```text
same route/key retry where appropriate
    -> same model / alternate provider
    -> compatible alternate model
    -> explicit escalation/substitution
```

This minimizes semantic drift compared with treating every healthy model as an interchangeable endpoint.

### Constraint

The same advertised model name does not prove identical execution. Providers may differ in serving stack, quantization, model revision, safety layer, tool translation, reasoning settings, or multimodal support.

This reinforces existing HE execution-provenance research rather than establishing provider equivalence.

---

# 3. Adaptive routing is real, but narrower than the marketing language

FreeLLMAPI implements multiple routing strategies. The scoring code normalizes signals and combines them as weighted dimensions.

Current documented/implemented inputs include:

- reliability;
- speed;
- intelligence/capability metadata;
- quota headroom;
- rate-limit pressure;
- provider/model health;
- context-window fit;
- tool/vision capability constraints.

Source:
`server/src/services/scoring.ts`

The code explicitly states that reliability uses a Beta posterior / Thompson Sampling, while speed and intelligence are deterministic inputs.

The default balanced weights currently favor reliability:

```text
reliability 0.50
speed       0.25
intelligence 0.25
```

Other presets include smartest, fastest, and reliable.

### Important distinction

The system does **not** continuously evaluate semantic answer quality and discover which model is "smartest" from completed work.

What it learns directly is largely operational reliability. Intelligence is based on configured/catalog capability metadata. Therefore the durable lesson is not "let a bandit discover the best model." It is:

> adaptive routing can combine observed operational evidence with explicit capability metadata, but those evidence classes should remain distinguishable.

### Useful implementation boundary

The code deliberately keeps model ranking separate from key selection. That is a good separation of concerns: selecting a model is not the same decision as choosing among credentials/endpoints that can serve it.

---

# 4. Guardrails around adaptive behavior are more interesting than Thompson Sampling itself

The scoring implementation contains several examples of bounded adaptation:

- quota headroom reduces a model's score as a free pool approaches exhaustion;
- rate-limit pressure demotes routes that are actively failing;
- operator-selected routing presets remain explicit;
- some dynamic adjustments are opt-in rather than silently time-dependent;
- custom operator weights are protected from automatic task-type rewriting;
- invalid/stale settings fall back safely rather than failing routing.

This is more relevant to HE than the algorithm name.

### HE research implication

Adaptive behavior should have:

- explicit authority boundaries;
- bounded influence;
- operator-visible policy;
- safe fallback behavior;
- enough provenance to explain why a different path was selected.

Do not infer that a learning router should own architecture-level workflow decisions.

---

# 5. Provider error semantics are the hard part of failover

The repository's issue history demonstrates that HTTP status alone is not a sufficient failure taxonomy.

## NVIDIA NIM example

Issue #522 documents NVIDIA returning HTTP 400 for a temporarily `DEGRADED` hosted function. The request itself was valid, but the gateway initially treated the response as a client error rather than retryable provider unavailability.

https://github.com/tashfeenahmed/freellmapi/issues/522

## Groq tool-call example

Issue #168 documents Groq returning HTTP 400 for a tool-generation failure. For an automatic route with other tool-capable models available, the correct operational behavior was to continue the failover chain rather than terminate the user request as invalid.

https://github.com/tashfeenahmed/freellmapi/issues/168

### Durable lesson

Provider abstraction is not merely request-format normalization.

A production-quality gateway must understand:

- which failures are caller errors;
- which are provider/runtime failures;
- which failures are capability mismatches;
- which are transient and retryable;
- which should trigger a cooldown;
- which should move to another key;
- which should move to another provider;
- which should move to another model;
- when retrying risks duplicate side effects.

This is strong evidence against reimplementing provider routing casually inside a harness.

---

# 6. Context handoff treats model change as a state-transfer event

FreeLLMAPI optionally inserts a compact handoff message when a conversation moves to a different model.

Source:
`docs/en/clients/01-agent-clients.md#context-handoff`

The handoff tells the new model that it is continuing an existing task and should use the provided conversation rather than restart or re-ask setup questions.

Current implementation characteristics include:

- session affinity;
- handoff only when the selected model changes;
- in-memory session storage;
- bounded TTL;
- no disk persistence for the handoff state;
- explicit admission that provider-internal hidden state cannot be recovered.

### HE implication

A model transition should be analyzed as a **handoff/state-transfer event**, not just a routing event.

Potentially material state includes:

- current goal;
- accepted decisions;
- relevant artifacts;
- unresolved work;
- tool results;
- definition of done;
- verification evidence;
- previous execution identity and reason for transfer.

This reinforces existing HE work-transfer and continuity research. It does not imply adopting FreeLLMAPI's exact session-key or TTL implementation.

---

# 7. Deterministic prompt compression is a high-value pattern

FreeLLMAPI's compression pipeline is opt-in and operates before routing/token budgeting.

Source:
`docs/en/compression/01-compression-pipeline.md`

Modes currently include:

- `off`;
- `lossless`;
- `standard`;
- `aggressive`.

The pipeline can perform:

- repeated-block deduplication;
- whitespace normalization;
- reversible homogeneous-JSON table encoding;
- command-aware tool-output filtering;
- stale file-read supersession;
- older-turn condensation;
- lexical relevance filtering;
- optional hard token targets.

### Fidelity gates

Each engine is fail-open and its output is discarded if the transform grows the request, throws, or fails fidelity requirements.

Protected information includes:

- numeric literals;
- diff hunks;
- explicit constraints;
- security instructions;
- error lines;
- JSON keys;
- code fences;
- URLs;
- file paths;
- stack traces;
- structured tool metadata.

Tool-call and tool-result envelopes must remain valid.

### Trust boundary

Repository-local compression filters are disabled by default unless the operator explicitly trusts them. This is important because project-controlled transformation rules are executable influence from an otherwise untrusted repository.

### HE implication

The strongest reusable lesson is:

> prefer deterministic, inspectable context reduction for structured/noisy data before introducing model-based semantic compaction.

Context reduction should be measured against fidelity, retries, and downstream error—not just token savings.

---

# 8. Routing diagnostics and recovery provenance

`server/src/services/router.ts` maintains explicit diagnostics for why candidate routes were unavailable, including examples such as:

- no usable key;
- cooldown/rate limits;
- context too small;
- vision/tool capability mismatch;
- provider incompatibility;
- route already failed earlier in the request.

The code intentionally converts an opaque "all models exhausted" condition into an actionable explanation without exposing credentials.

### HE implication

Recovery without provenance makes later evaluation weak.

When a material execution changes route/model/provider, the system should be able to preserve enough evidence to explain:

```text
requested capability/model
selected execution
reason selected
failure/recovery events
reason for each transition
final execution
material cost/context consequences
```

This should remain lightweight and task-relevant rather than becoming a general observability platform.

---

# 9. Gateway vs harness responsibility boundary

FreeLLMAPI provides useful concrete evidence for a boundary already under HE investigation.

## Responsibilities demonstrated as gateway concerns

- provider credentials;
- API/wire translation;
- provider adapters;
- key selection;
- rate limits and quota pools;
- cooldowns;
- provider health;
- same-model provider failover;
- context-window/capability eligibility;
- retry/error classification;
- raw route telemetry.

## Responsibilities that remain harness concerns

- what capability the task requires;
- whether model substitution is allowed;
- whether a task is suitable for delegation;
- what state must survive a handoff;
- what evidence establishes completion;
- when failure warrants escalation rather than retry;
- which workflow stages/capabilities should run;
- which context is authoritative;
- human approval at load-bearing decisions.

### Research conclusion

HE should define semantic and governance expectations **above** the gateway rather than absorb provider integration mechanics itself.

---

# 10. Signed remote catalog: useful mechanism, different governance problem

FreeLLMAPI distributes a signed model catalog so model/quota/provider metadata can update independently of the executable code.

This is technically useful, but signatures only establish publisher authenticity/integrity. They do not establish that a changed routing input has been reviewed and approved for a governed enterprise harness.

### Candidate HE pattern

Where external capability catalogs are used, consider:

```text
fetch/signature verify
    -> inspect/diff
    -> approve/pin
    -> promote
    -> rollback if needed
```

Do not infer that trusted publisher identity should automatically grant authority to change harness behavior.

---

# 11. Terms, production use, and free-tier limits

The project itself documents significant restrictions and uncertainty across free providers. Some tiers are explicitly evaluation/prototyping only; others have ambiguous proxy/resale clauses or usage limitations.

Source:
`docs/en/architecture/00-high-level-index.md#terms-of-service-review`

### HE implication

"Technically routable" is not equivalent to "permitted for the intended workload."

Provider terms, privacy requirements, data handling, residency, and production permissions belong in capability eligibility before a harness or gateway selects an endpoint.

This is particularly relevant to enterprise environments.

---

# Cross-check against existing HE research

This source reinforces existing HE themes rather than creating a new architecture direction.

## Strong reinforcement

### Progressive / selective context

The compression architecture reinforces existing context-engineering research: context reduction should be source-aware, deterministic where possible, and independently inspectable.

### Execution provenance

Provider/model transitions reinforce the existing finding that model name alone may not fully identify execution conditions.

### Bounded adaptation

Adaptive routing is useful only when authority and fallback behavior remain explicit and reversible.

### Work transfer / handoff

Model switching strengthens the case for explicit state-transfer semantics rather than assuming conversation continuity survives execution changes automatically.

### Gateway/harness separation

The implementation complexity of provider routing strongly supports keeping provider integration mechanics below the Harness layer.

---

# What to mine

## REINFORCE — high value

1. **Semantic-preserving recovery order** — same-model alternatives before cross-model substitution where practical.
2. **Model transition as handoff** — execution change requires explicit continuity/state-transfer treatment.
3. **Deterministic context reduction first** — structured filtering/deduplication before semantic compaction.
4. **Fidelity gates around context reduction** — reject transformations that lose required evidence/constraints.
5. **Observable recovery chains** — preserve concise execution/fallback provenance.
6. **Facts vs heuristics** — distinguish provider-reported state from local inference when making runtime decisions.
7. **Gateway/harness responsibility boundary** — provider mechanics below; capability/verification/governance above.
8. **Quota scope matters** — shared pools and reset semantics matter more than headline token totals.

## ASSESS

1. Whether HE should define a generic semantic recovery-order principle independent of any provider router.
2. What minimum handoff metadata should be preserved when execution moves between models/providers/sessions.
3. Which deterministic tool-output filters have measurable value for PMB/ACR/work Memory Bank.
4. What minimum route/provenance metadata is required for controlled model comparisons.
5. Whether externally supplied capability catalogs should be pinned/approved in governed environments.

## PARK

- Thompson Sampling as a Harness requirement.
- General-purpose smart model routing inside HE.
- Automatic time-of-day/task-type weight adjustment as an HE pattern.
- Multi-model Fusion as default evaluation architecture.
- A dedicated provider catalog service.

## REJECT

- Treating headline free-token totals as guaranteed usable capacity.
- Treating every HTTP 400/429/5xx uniformly across providers.
- Treating same nominal model name as proven identical execution.
- Treating model substitution as transparent when task state or behavior can change.
- Reimplementing provider/quota/protocol routing inside Harness Engineering without a concrete requirement.
- Copying FreeLLMAPI code into HE merely because the patterns are useful.

---

# Disposition

**MINE SELECTIVELY.**

FreeLLMAPI is a strong reference implementation for runtime gateway behavior and a useful stress test for the boundary between inference infrastructure and a higher-level AI harness.

Its highest-value contribution to HE is not free inference. It is evidence that routing, failover, quota handling, context transfer, compression, and provider compatibility are distinct engineering responsibilities with different ownership and failure modes.

No architecture decision follows automatically from this research.
