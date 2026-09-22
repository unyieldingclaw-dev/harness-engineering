# OpenMAIC — Agent Runtime Deep Evidence Pass — 2026-09-06

## Corpus relationship

**Relationship to existing corpus:** MERGES WITH and SHARPENS existing research on execution profiles, capability/authority separation, progressive disclosure, decision elicitation, evidence-first verification, durable state, context-cost control, execution provenance, and bounded recovery. It is also ORTHOGONAL prior art for a concrete durable-agent runtime/control plane.

**Source:** THU-MAIC/OpenMAIC, current `main` as inspected 2026-09-06.

- Repository: https://github.com/THU-MAIC/OpenMAIC
- Changelog: https://github.com/THU-MAIC/OpenMAIC/blob/main/CHANGELOG.md
- Agent session contracts: https://github.com/THU-MAIC/OpenMAIC/blob/main/packages/@openmaic/storage/src/agent-session/types.ts
- Agent session contract tests: https://github.com/THU-MAIC/OpenMAIC/blob/main/packages/@openmaic/storage/test/agent-session-contract.ts
- Runtime runner: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/runner.ts
- Runtime configuration: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/config.ts
- Tool-call integrity: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/tool-call-integrity.ts
- URL fetch/trust boundary: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/fetch-url.ts
- Owner-scoped response boundary: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/route-response.ts
- Curriculum planner skill: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/curriculum-planner/SKILL.md
- Deep research skill: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/deep-research/SKILL.md
- Fact-check skill: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/fact-check/SKILL.md

## Executive finding

OpenMAIC is useful prior art because its v1 agent workbench is not merely a prompt-and-tools wrapper. It implements a durable execution runtime with explicit lifecycle state, worker leases, crash recovery, event replay, cancellation, follow-up steering, owner scoping, tool-call repair, capability gating, bounded resource policy, and persisted artifact/material state.

The strongest Harness Engineering lesson is not "adopt OpenMAIC." It is that a reliable agent runtime can be decomposed into independently governed contracts:

1. **Lifecycle coordination** — queued/running/terminal state, worker claims, leases, attempts, cancellation, recovery.
2. **Execution evidence** — event stream plus append-only entry tree.
3. **Projection** — sparse per-owner views that can be repaired independently.
4. **Capability surface** — tools/skills registered only when corresponding capabilities exist.
5. **Authority boundaries** — owner authorization, lease fencing, URL trust, server-side provider configuration.
6. **Context materialization** — durable transcript is not necessarily identical to the model-facing context; invalid/interrupted history can be repaired at the read boundary.

This strongly reinforces the Harness distinction between **model capability, tool availability, execution authority, and durable execution state**.

## 1. Durable execution is a control-plane problem

OpenMAIC's runner comment is unusually explicit: every application process may run the loop, while PostgreSQL is authoritative for claims, lease generations, event ordering, cancellation, and recovery; a client connection is not part of execution lifetime.

That means execution ownership is deliberately moved away from the UI/request lifecycle and into a durable control plane.

The runtime configuration makes the control loop concrete:

- scan interval: 1s default;
- heartbeat: 2s;
- lease TTL: 10s;
- max concurrent sessions per application instance: 2;
- maximum consecutive unattended starts/resumptions: 5.

The important pattern is **bounded, lease-coordinated execution**, not simply "run the agent in the background."

Evidence:
- runner: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/runner.ts
- config: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/config.ts
- changelog v1.0.0: https://github.com/THU-MAIC/OpenMAIC/blob/main/CHANGELOG.md

### Harness implication

This sharpens the earlier execution-profile/runtime research: **execution identity should include lifecycle ownership and runtime admission policy, not only provider/model/runtime configuration.**

Candidate fields for a future Execution/Run Profile therefore include:

- worker/runtime identity;
- concurrency bound;
- retry/attempt policy;
- lease/heartbeat policy;
- cancellation semantics;
- context transformation policy;
- capability set;
- authority scope.

Do not adopt this field set yet; this is a research refinement.

## 2. Attempt generation is a real fencing primitive

The storage contract has an `attempt` generation that increments on successful claims. Settlement can require `expectedAttempt`, and durable writes reject stale worker/attempt combinations.

The tests explicitly exercise:

- successful claim;
- heartbeat;
- wrong-worker rejection;
- stale-attempt rejection;
- valid event append;
- valid finish;
- post-finish heartbeat rejection.

This is stronger than a generic mutex. It protects against an old worker continuing to write after ownership has moved elsewhere.

Evidence:
- types: https://github.com/THU-MAIC/OpenMAIC/blob/main/packages/@openmaic/storage/src/agent-session/types.ts
- contract tests: https://github.com/THU-MAIC/OpenMAIC/blob/main/packages/@openmaic/storage/test/agent-session-contract.ts

### Harness implication

This is relevant to ACR and multi-agent Harness execution whenever more than one process/session can act on the same durable task. **Publication authority and execution ownership need fencing, not merely conventions.**

It also strengthens the existing distinction between worktree isolation and publication authority: a worktree separates files; a lease/attempt fence separates *who is currently authorized to mutate durable execution state*.

**Disposition: HIGH-VALUE RESEARCH.**

## 3. Durable audit trail and model-facing context are different representations

The session storage contract deliberately separates four interfaces: lifecycle coordination, per-session event stream, append-only entry tree, and sparse per-owner projection. The code comments explicitly state that projection damage can be repaired independently and that a control-plane reader must not accidentally gain lease-bound write authority.

This is a particularly strong prior-art example for the PMB Durable State / Authority / Projection Model.

OpenMAIC also has an explicit model-facing context repair layer. `repairOrphanedToolCalls()` detects incomplete or non-contiguous tool results, synthesizes an interrupted result when necessary, and returns a provider-safe materialized view while leaving the entry tree itself immutable as the audit trail.

This yields a useful three-way distinction:

**durable truth → materialized execution view → model-facing context**

Those are not necessarily byte-identical.

Evidence:
- storage contract: https://github.com/THU-MAIC/OpenMAIC/blob/main/packages/@openmaic/storage/src/agent-session/types.ts
- tool-call integrity: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/tool-call-integrity.ts

### Harness implication

This may be more important than OpenMAIC's particular agent framework. PMB currently has a durable memory layer and startup context; ACR will have evidence and findings. We should not assume the durable record is always the same object that should be handed to a model.

**Candidate principle:**

> Preserve durable execution evidence; materialize a provider-valid model context from it at the execution boundary.

This is a candidate, not an adopted principle.

## 4. Tool-call integrity is a recovery concern, not just an LLM concern

OpenMAIC explicitly handles a race in which parallel tools can complete around an aborted assistant frame. A raw durable transcript may then be structurally invalid for a strict provider even though the missing result actually exists later in the transcript.

The repair algorithm:

- identifies assistant tool-call groups;
- locates their durable results;
- detects non-contiguous results;
- reorders known results into provider-required adjacency;
- drops incomplete aborted frames from the model view;
- synthesizes an explicit interrupted result only for genuinely missing results;
- leaves the immutable durable entry tree untouched.

The implementation also applies the repair again after context transforms because a transform that rebuilds context from raw durable state could otherwise reintroduce an already-repaired orphan.

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/tool-call-integrity.ts

### Harness implication

Recovery needs to be modeled as a **deterministic context-integrity stage**. This is analogous to ACR's deterministic evidence normalization: the model should not be expected to reason around malformed execution state when the Harness can establish a valid boundary representation.

**Disposition: HIGH-VALUE RESEARCH.**

## 5. Capability registration is conditional, not merely descriptive

The runner describes a minimal always-registered tool (`ask_user`) and capability-gated additions such as web search, materials, voice, skills, etc. Its prompt is assembled from the tools/capabilities actually registered.

The changelog describes this as provider-neutral tools combined with server-side provider capability resolution, uniform force-off behavior, startup routing validation, and provider force-off enforcement inside agent tools.

The architectural pattern is:

**server configuration → capability resolution → registered tool surface → prompt/tool contract**

rather than:

**model asks for feature → Harness assumes feature exists**.

Evidence:
- runner: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/runner.ts
- changelog: https://github.com/THU-MAIC/OpenMAIC/blob/main/CHANGELOG.md

### Harness implication

This materially sharpens our existing model/tool/authority separation. A capability should be resolved before it becomes part of the model's executable surface, and a disabled provider should not remain implicitly reachable through tool implementation details.

**Disposition: MERGES WITH capability/authority research.**

## 6. URL trust is an execution-authority boundary

`fetch_url` does not treat a syntactically valid URL as sufficient authority. The tool accepts URLs that were already exposed by a user message or `web_search`, and then applies strict URL normalization, DNS/IP safety checks, connection-time pinning, redirect revalidation, content-type limits, byte/page/character bounds, and cancellation.

The 1.0.1 changelog shows why this boundary matters: OpenMAIC fixed an environment-specific URL guard, added repository-scanning enforcement so gated call sites cannot silently reappear, and revalidated every provider redirect while stripping credentials across cross-origin hops.

This is concrete evidence for the principle:

> **Having a tool is not permission to reach arbitrary targets.**

Evidence:
- fetch implementation: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/fetch-url.ts
- changelog 1.0.1: https://github.com/THU-MAIC/OpenMAIC/blob/main/CHANGELOG.md
- session URL tests: https://github.com/THU-MAIC/OpenMAIC/blob/main/tests/agent-runtime/session-urls.test.ts

### Harness implication

This is a strong precedent for capability contracts that include **target authority**, not only function availability.

**Disposition: HIGH-VALUE.**

## 7. Owner scoping deliberately avoids existence oracles

Owner-scoped routes carry owner-resolution headers even on errors so anonymous clients retain their partition after a retry. Foreign and missing resources intentionally return the same 404 body, preventing an existence probe from distinguishing "does not exist" from "belongs to somebody else."

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/route-response.ts

### Harness implication

The broader lesson is that **authorization boundaries include information disclosure**, not only mutation permission. A Harness execution service can leak authority through read-side behavior even if all writes are protected.

This is more application-security-specific than PMB/ACR core, but useful as a concrete boundary example.

**Disposition: RELATED PRIOR ART.**

## 8. Long-running context is explicitly kept out of the chat when durable state can be reread

The curriculum planner says the per-lesson recap is the series memory and warns against replaying tool details, IDs, or already-summarized outlines. When an earlier detail is needed, it reads the persisted stage outline instead of keeping the whole detail alive in conversation context.

It also explicitly separates a clarification run from later execution: `ask_user` ends the run, the answer starts the next one, and the conversation remains intact.

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/curriculum-planner/SKILL.md

### Harness implication

This is independent supporting evidence for the PMB context-cost conclusion: **durable state and active context should be separate budgets.** Moving information from one startup file to another does not solve context cost; retrieval from durable state can.

It also supports the existing progressive-disclosure pattern: load the relevant state when needed rather than carrying all prior state continuously.

**Disposition: SHARPENS context-cost/progressive-disclosure research.**

## 9. Decision elicitation is bounded and explicitly gated

The curriculum planner uses two or three clarification rounds, at most three big things per round, and explicitly says not to ask what can safely be defaulted. It then requires a separate full-series confirmation before creating stages.

The important design pattern is not "ask more questions." It is:

**identify load-bearing unknowns → resolve them incrementally → present complete proposal → obtain explicit authorization → execute**

The skill also prohibits treating silence as consent and keeps the confirmation gate separate from clarification.

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/curriculum-planner/SKILL.md

### Harness implication

This independently reinforces the decision-elicitation candidate from David/Matt research while adding a useful distinction between **clarification authority** and **execution authorization**.

Potentially useful for PMB: decisions that change the durable plan should be explicitly committed before mutation, while safe defaults should not become needless user questions.

**Disposition: MERGES WITH decision-elicitation research.**

## 10. Evidence-first research is implemented as a bounded ledger

OpenMAIC's `deep-research` skill defines a concrete research contract:

- research before outlining/generation;
- split into 2–4 searchable facets;
- cap web searches at 8;
- fetch at most 6 URLs;
- prefer primary/authoritative sources;
- maintain a claim → source ledger;
- verify claims against fetched material rather than search snippets;
- cross-check genuine conflicts;
- soften/drop single-sourced load-bearing claims;
- stop once all outline-bearing facets and exact numbers/dates/names are sourced.

It also passes sourced facts explicitly into generation briefs rather than assuming the page generator remembers the research.

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/deep-research/SKILL.md

### Harness implication

This is strong supporting evidence for our corpus's **evidence-first, bounded discovery** discipline. The particularly useful concept is the explicit stopping rule: research ends when coverage is sufficient, rather than when the agent has exhausted curiosity.

**Disposition: MERGES WITH evidence-first research discipline.**

## 11. Fact checking is targeted rather than exhaustive

The `fact-check` skill explicitly avoids auditing every statement. It shortlists high-signal claims: exact numbers/dates/names, standards/formulas, absolute claims, causal/professional conclusions, contradictions, and suspiciously specific unsupported claims. It then verifies only that shortlist, with a normal first pass of roughly 6–8 searches.

It also distinguishes creating from reviewing. During creation, factual checking happens after pages exist; during review, findings are reported before edits, and existing approved inputs are protected from silent override.

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/skills/agent-runtime/fact-check/SKILL.md

### Harness implication

This provides useful prior art for **verification budget allocation** in ACR. A verifier can prioritize claims or changes with high consequence rather than spending equal compute on every line.

The approved-input protection is also relevant to governance: evidence that contradicts a previously approved input should surface a decision, not silently rewrite authority.

**Disposition: HIGH-VALUE RESEARCH.**

## 12. Resource limits are first-class runtime configuration

The runtime exposes explicit limits for concurrency, attempts, uploads, material count, total material bytes, and compaction configuration. The changelog additionally records bounded retries for scene generation and bounded PPTX parsing/skill ZIP inflation.

This matters because bounded execution appears at multiple layers:

- session admission;
- attempts/retries;
- network fetches;
- parsing;
- storage quotas;
- model context transformation.

Evidence:
- config: https://github.com/THU-MAIC/OpenMAIC/blob/main/lib/server/agent-runtime/config.ts
- changelog: https://github.com/THU-MAIC/OpenMAIC/blob/main/CHANGELOG.md

### Harness implication

This supports treating **resource bounds as part of the execution contract**, not deployment trivia. It strengthens the earlier Ralphy finding that concurrency/retry/attempt/cost limits belong in a task/run contract.

**Disposition: SHARPENS Execution Contract research.**

## 13. Security fixes are evidence about failure modes, not just features

The current 1.0.1 changelog is particularly valuable because it records concrete security failures and the structural changes made in response:

- write/read symmetry for classroom path containment;
- sanitization at the persistence boundary on both write and read;
- URL guard enforced in every environment plus repository-scanning regression protection;
- redirect-hop revalidation and credential stripping;
- production refusal of insecure development authentication;
- bounded archive inflation and PPTX parsing;
- server-pinned image/video models.

This is useful corpus evidence because these are not aspirational architecture statements; they are responses to discovered defects.

Evidence: https://github.com/THU-MAIC/OpenMAIC/blob/main/CHANGELOG.md

### Harness implication

For our corpus, **post-incident hardening is higher-value evidence than a feature description** when evaluating whether a proposed boundary is actually load-bearing.

That should inform future OpenMAIC research and our own Harness experiments: inspect security regressions and tests, not just README architecture diagrams.

## 14. Important limitations / don't over-generalize

1. OpenMAIC is an application-specific course-generation platform, not a general coding Harness.
2. Its PostgreSQL control-plane design is a concrete implementation choice, not evidence that PMB/ACR need PostgreSQL.
3. `@earendil-works/pi-agent-core` is a runtime dependency; the useful finding is the contract around it, not the library itself.
4. The skill taxonomy is application-specific. We should mine patterns, not copy its skills wholesale.
5. The durable runtime solves server-side multi-session execution; it does not by itself solve code workspace isolation, Git publication authority, deterministic code verification, or architecture-drift truth.
6. Its compaction support is explicitly configurable and described as a later-slice boundary; this is evidence that context transformation is a separate runtime concern, not proof that a particular compaction algorithm is correct.

## 15. Candidate Harness model extracted from OpenMAIC

OpenMAIC suggests a more complete runtime boundary:

```text
Request / Task
    ↓
Task identity + owner scope
    ↓
Admission / capability resolution / resource bounds
    ↓
Execution claim + lease + attempt fence
    ↓
Agent driver + registered tool surface
    ↓
Durable event / entry recording
    ↓
Deterministic context materialization / integrity repair
    ↓
Model execution
    ↓
Tool execution + authority checks
    ↓
Checkpoint / event append
    ↓
Terminal settlement OR crash recovery
```

The durable record and model context are deliberately separate boundaries.

For coding Harness work, additional layers remain necessary:

```text
workspace isolation
→ deterministic tests/static checks
→ independent review
→ publication authority
```

Therefore OpenMAIC should **not** replace the existing Harness model. It fills in the durable-execution/control-plane portion.

## 16. Research dispositions

| Finding | Relationship | Disposition |
|---|---|---|
| Durable session runtime | ORTHOGONAL TO execution-profile work | HIGH-VALUE RESEARCH |
| Lease + attempt fencing | SHARPENS execution/publication authority | HIGH-VALUE RESEARCH |
| Event log / entry tree / projection separation | SHARPENS durable-state model | HIGH-VALUE RESEARCH |
| Model-context integrity repair | ORTHOGONAL TO deterministic evidence normalization | PRIORITY RESEARCH |
| Capability-gated tool surface | MERGES WITH capability/authority separation | HIGH-VALUE |
| URL trust + SSRF boundary | SHARPENS capability target authority | HIGH-VALUE |
| Owner-scoped no-existence oracle | RELATED security prior art | SUPPORTING |
| Context retrieval instead of transcript replay | SHARPENS PMB context-cost research | HIGH-VALUE |
| Bounded decision elicitation | MERGES WITH existing decision research | SUPPORTING/HIGH-VALUE |
| Claim-source ledger | MERGES WITH evidence-first research | SUPPORTING |
| Targeted fact checking | SHARPENS verification budget | HIGH-VALUE |
| Resource bounds | SHARPENS Execution Contract | HIGH-VALUE |
| Security-regression-driven hardening | ORTHOGONAL evidence-quality lesson | HIGH-VALUE RESEARCH |

## Bottom line

OpenMAIC's strongest contribution to the corpus is a concrete implementation of a principle we have been approaching from several directions:

> **A reliable agent Harness is not just a prompt plus tools. It needs a durable execution/control plane that owns lifecycle, authority, recovery, evidence, and bounded resource policy. The model-facing context is a derived execution view, not necessarily the durable truth.**

The most promising follow-up is to compare OpenMAIC's control-plane contracts against our existing **Execution Profile / Task Contract / Isolation Contract / Evidence Contract / Publication Authority** candidates and identify which concepts are genuinely missing versus merely different names for things we already have.

Do not adopt architecture from this document directly. The next step should be a cross-corpus merge analysis and, if warranted, a small runtime experiment around durable execution ownership and context materialization.
