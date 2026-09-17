# Session Rollover, Handoff & Verification — 2026-09-16

## Purpose

Synthesize the durable Harness Engineering lessons from Ben AI's token-optimization material and direct inspection of the supplied `Refresh`, `Interview Me`, and `Prompt Master` skills.

Primary evidence:

- `01 Research/Sources/Ben AI — Token Optimization, Refresh & Prompt Master — 2026-09-16.md`

Related HE research:

- `01 Research/Context Engineering.md.md`
- `01 Research/Sources/Claude Token Limits & Master Prompt — Context Efficiency Lessons.md`
- `01 Research/Sources/Agent Skills & Context Efficiency — Reputable Guidance.md`
- `01 Research/Sources/FreeLLMAPI — Gateway Routing, Failover & Context Compression — 2026-09-14.md`

This is research synthesis only. It does not authorize PMB or HE implementation changes.

---

# Executive synthesis

The material reinforces a useful distinction that Harness Engineering should make explicit:

> **Context rollover, compaction, durable memory, and verification are separate mechanisms with different jobs.**

A long session should not be managed by a universal absolute token threshold. When the host exposes actual context-window occupancy, that runtime signal is a better measure of pressure. Crossing a threshold should initially make the pressure visible; it should not automatically terminate work.

For systems such as PMB that already persist project truth outside the conversation, a planned fresh-session handoff can be cleaner than repeatedly compacting conversational history. The handoff should carry only ephemeral state that the successor cannot recover from authoritative artifacts.

Verification should likewise be separated into **ritual repetition** and **evidence acquisition**. Model-specific guidance against redundant self-check prompting does not invalidate tests, acceptance criteria, independent executable checks, or risk-directed review.

---

# 1. Context pressure is observable runtime state

Absolute token counts are a poor universal rollover rule because:

- models may have different context-window sizes;
- cached and cumulative token usage are not identical to active context occupancy;
- subagent usage can consume substantial tokens without filling the primary context;
- plan/rate-limit consumption is a separate constraint again.

When available, use the host's own context-window measurement.

Claude Code currently exposes `context_window.used_percentage`, `remaining_percentage`, token components, cache state, and rate-limit state to the local status-line command.

### HE principle

> **Observe context pressure directly when the runtime exposes it; do not infer it from cumulative spend.**

### Governance boundary

A context-pressure signal may justify:

- a visible warning;
- a recommendation to finish the current unit of work;
- a planned handoff at a natural boundary.

It should not automatically grant authority to:

- stop the session;
- modify project state;
- generate persistent artifacts;
- discard conversational state;
- change models.

Those remain separate actions.

---

# 2. Planned handoff and compaction solve different problems

## Compaction

Compaction preserves continuity inside the same logical session by reducing older history.

Useful when:

- required state exists primarily in the conversation;
- work cannot cleanly stop yet;
- context exhaustion is approaching;
- a fresh session would require expensive reconstruction.

Risks:

- lossy synthesis;
- accidental retention of stale/noisy state;
- continued dependence on a long conversational lineage.

## Fresh-session handoff

A handoff ends one execution context and starts another with selected state.

Useful when:

- durable project state already exists externally;
- a natural task boundary exists;
- the current context contains substantial stale/redundant history;
- the successor can re-open authoritative artifacts.

Risks:

- missing ephemeral state;
- broken file/source references;
- duplicating authoritative project truth into the handoff;
- stale handoff artifacts being mistaken for current truth.

### HE principle

> **Use compaction to preserve necessary conversational continuity; use handoff to transfer only the state a fresh execution cannot reconstruct from authoritative artifacts.**

---

# 3. Handoff quality depends on destination and reachability

The inspected Refresh skill makes an important distinction between a handoff that remains in the same environment and one that travels to another person/machine/context.

This yields a durable HE principle:

> **A handoff pointer is valid only if the successor can resolve it.**

For repository-backed work, preferred references are generally:

1. repo-relative paths;
2. shared remote identifiers/URLs;
3. explicit embedded state only when no durable/shared home exists;
4. machine-local absolute paths only for genuinely local continuation.

A handoff should never assume that an ephemeral sandbox path survives.

### PMB implication to assess

PMB's handoff protocol should verify the reachability of any ephemeral-state pointer it emits, but should not silently copy/write files to create that reachability. Any new persistent write remains an explicit governed action.

---

# 4. Successor-goal-aware transfer is stronger than transcript summarization

Refresh asks what the next session is intended to accomplish before deciding what to carry forward.

This is useful because relevance is destination-dependent.

A summary asks:

> What happened?

A handoff should ask:

> What does the successor need to accomplish the next goal without redoing or contradicting prior work?

### HE principle

> **Generate transfer context against the successor's job, not against the source session's chronology.**

This is progressive disclosure applied across session boundaries.

---

# 5. Durable state should not be re-summarized into handoff state

Generic Refresh must reconstruct project state from the conversation because it cannot assume a durable project memory system exists.

PMB can.

Therefore PMB should preserve its stronger separation:

- durable decisions/constraints/progress -> Memory Bank;
- exact interrupted work / uncommitted state / immediate resume command -> handoff;
- startup instructions -> loaded normally by the environment;
- large references -> reopened on demand.

### HE principle

> **Do not create a second summary of information that already has an authoritative durable owner.**

Duplication creates drift and increases context cost.

---

# 6. Clarification should target high-impact unknowns

The inspected Interview Me skill and Anthropic's field guide converge on a practical pattern:

- inspect existing artifacts first;
- ask one question at a time;
- prioritize answers that would materially change architecture/scope/output;
- stop after a bounded number of questions;
- skip interviewing when the task is already clear.

### HE refinement

The source's rule that everything the user does not explicitly claim becomes the model's decision is too broad.

A governed harness should instead distinguish:

- routine, reversible implementation choices -> agent may decide inside approved scope;
- scope, architecture, security boundaries, irreversible operations, external side effects, acceptance criteria -> retain explicit escalation/approval when material.

### HE principle

> **Ask only for unknowns that materially change the work; do not convert unspecified decisions into unlimited model authority.**

---

# 7. Verification ritual versus evidence-directed verification

This is the most important correction to the source material.

Anthropic's current Opus 5 guidance does say that generic instructions to double-check/re-verify can produce redundant work because Opus 5 self-corrects strongly.

But Anthropic's general current-model guidance still recommends verification against explicit test criteria and identifies Opus 5 as a model-specific exception for migrated self-check instructions.

The inspected Prompt Master skill itself preserves evidence checks on long runs, requiring progress claims to point to actual tool results.

The durable distinction is therefore:

## Verification ritual

- repeat the same reasoning with no new evidence;
- generic "double-check everything" language;
- unconditional verifier subagent with no explicit property to check;
- repeated review passes that share the same assumptions and evidence.

## Evidence-directed verification

- executable tests;
- reproduced failures;
- acceptance criteria;
- invariants;
- runtime observations;
- targeted security/enforcement inspection;
- independent evidence sources;
- review aimed at a load-bearing premise;
- faithful audit of progress claims against tool results.

### HE principle

> **Remove redundant verification instructions; preserve verification that acquires or evaluates relevant evidence.**

This is compatible with existing HE findings:

- verification must produce relevant evidence;
- independent executable evidence is stronger than another model opinion;
- verification should be risk-directed.

---

# 8. Model-specific prompting guidance must stay model-specific

Prompt Master takes Opus 5 guidance about over-verification and phrases it as a broader Claude 5 rule.

That is exactly the kind of drift HE should guard against.

### HE principle

> **Do not promote model-specific behavior into a cross-model harness rule without evaluation evidence.**

Persistent harness instructions should distinguish:

- model/runtime fact;
- model-family observation;
- cross-model invariant;
- local workflow preference.

When the source only establishes one of those scopes, retain that scope.

---

# 9. Self-improving instruction files require governance

Prompt Master instructs itself to modify its own rule/reference files after user corrections, positive feedback, and upstream guidance changes.

That is not an acceptable default for a governed harness.

### Risks

- single-example overfitting;
- accumulated instruction bloat;
- hidden behavior changes;
- untrusted-context poisoning;
- unclear ownership and provenance;
- a success in one task becoming a bad global rule.

### HE principle

> **Learning may propose a durable correction; it should not silently become the durable correction.**

Durable rule changes should identify:

- the observed/reproducible failure or supported improvement;
- the owning component;
- the smallest durable correction;
- verification that the correction fixes the original failure without regressions.

This is consistent with existing HE failure-analysis guidance.

---

# 10. Skill conformance is not outcome quality

Prompt Master's supplied eval checks whether its own procedure executed correctly and whether expected prompt text appeared/disappeared.

That is useful as a smoke test.

It does not prove that the rewritten prompt performs better on real tasks.

### HE evaluation split

1. **Conformance** — did the capability execute its documented procedure?
2. **Task outcome** — did the produced result meet the external success criteria?
3. **Operational cost** — what did the procedure cost in context, latency, retries, and human intervention?
4. **Regression** — did the improvement damage other supported workflows?

A capability can pass conformance while making task outcomes worse.

---

# Research disposition

## REINFORCE

- Context is a managed resource and should be observable.
- Host-reported context occupancy is preferable to arbitrary absolute-token rollover rules.
- Fresh-session handoff and compaction are distinct mechanisms.
- Handoff pointers need persistence and reachability semantics.
- Successor-goal-aware context selection is a useful progressive-disclosure pattern.
- Durable state and transient transfer state should have different owners.
- Clarification should target high-impact unknowns only.
- Verification should be evidence-directed and risk-directed.
- Model-specific guidance should not silently become a universal harness rule.
- Durable instruction changes require explicit governance.
- Skill conformance and outcome evaluation must be separated.

## ASSESS

- A minimal context-pressure status indicator for PMB using Claude Code's status-line data.
- Whether PMB's existing 40% handoff candidate threshold is useful in controlled sessions.
- Successor-goal and artifact-reachability checks in the PMB handoff process.
- Fresh-session handoff versus targeted compaction on equivalent long tasks.
- PMB instruction wording that removes generic duplicate-checking language while preserving tests, evidence, review, and acceptance criteria.

## PARK

- Automatic context-threshold rollover.
- Multiple handoff fidelity modes.
- Portable bundling/cross-machine transfer machinery until there is an observed need.
- New interview/prompt-master skills when existing planning mechanisms already cover the operational need.

## REJECT

- Universal 300K/500K context-rot thresholds.
- "Never verify" as a general rule.
- Automatic project mutation by a handoff operation.
- Automatic self-modification of persistent skill instructions from single-session feedback.
- Self-referential multi-agent evaluation as sufficient proof of outcome quality.

---

# Candidate PMB hypothesis, not implementation authorization

A low-risk experiment is now evident:

> **Expose Claude Code's actual context percentage in the status line and visibly mark the existing handoff-candidate threshold, without automatically triggering handoff or changing compaction behavior.**

This would make an existing PMB policy observable without adding autonomous authority.

The pilot could then determine whether the threshold correlates with useful handoff timing before PMB changes session behavior around it.
