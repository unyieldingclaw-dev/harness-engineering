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
- `01 Research/Source Preservation & Evidence Durability — 2026-09-16.md`

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

- the current session remains coherent;
- a transition is inconvenient;
- the host/runtime performs it safely;
- durable project state is already current enough to recover if compaction loses detail.

Risks:

- semantic compression can omit details;
- compaction itself consumes context/tokens;
- repeated compaction can preserve stale assumptions or conversational debris.

## Fresh-session handoff

A fresh-session handoff intentionally resets conversational history while transferring only required state.

Useful when:

- the task is long-running;
- context pressure is high;
- the next unit of work is clear;
- authoritative project state already exists outside the conversation;
- the successor can re-read durable artifacts.

### HE principle

> **Prefer durable state + narrow handoff over reconstructing an entire project from conversation history.**

For PMB specifically, `memory-bank/` should remain authoritative for durable project truth. A handoff artifact should contain only ephemeral in-flight state that is not naturally represented there.

---

# 3. Handoff should be successor-goal-aware

The inspected Refresh skill asks for the goal of the next session before deciding what to carry forward.

This is stronger than a generic session summary because relevance is destination-dependent.

### HE principle

> **A handoff should be shaped by the successor's job, not by the desire to summarize the predecessor's history.**

The successor generally needs:

- the immediate goal;
- current in-flight state;
- relevant artifacts and paths;
- unresolved blockers;
- the next executable action;
- dead ends only when retry risk is material;
- enough provenance to know what was actually verified.

The successor generally does not need:

- a transcript recap;
- every decision already persisted in authoritative project state;
- stale alternatives;
- repeated background already loaded by the project harness.

---

# 4. Handoff pointers require reachability

A path or reference is useful only if the successor can resolve it.

The inspected Refresh skill explicitly distinguishes same-machine and cross-machine transfers and checks whether referenced files/data will survive the transition.

### HE principle

> **A handoff reference is valid only when the successor can resolve it.**

Examples of fragile references:

- `/tmp/...`;
- transient Cowork scratch space;
- absolute local paths transferred to another machine;
- uncommitted files when the successor works from a fresh checkout;
- authenticated links unavailable to the successor;
- tool-specific ephemeral IDs.

The handoff should either persist/copy the required artifact with explicit authority or clearly state the prerequisite for resolving it.

Do not silently copy project files merely to make a handoff self-contained.

---

# 5. Verification ritual vs evidence-directed verification

The source correctly identifies a real anti-pattern:

```text
"double-check everything"
"verify twice"
"be maximally thorough"
```

These can cause redundant self-review/tool calls, especially on models already trained to self-check.

That does **not** establish that explicit verification is generally wasteful.

Useful verification is tied to a property or risk:

- run the affected tests;
- reproduce the defect before changing it;
- verify acceptance criteria;
- inspect a security/enforcement boundary;
- challenge an architectural premise;
- confirm a deployment/runtime condition;
- use independent executable evidence where possible.

### HE principle

> **Remove ritualized repetition; preserve verification that acquires evidence about a required property.**

A model saying "I checked" is not evidence. A targeted test, invariant, runtime observation, reproducible failure, or focused independent review can be.

---

# 6. Opposition review is not redundant self-checking

PMB's Opposition/Opp Review mechanism should not be conflated with generic instructions such as "double-check your work."

The distinction is structural:

- **redundant self-checking** asks the same actor to revisit its own conclusion without a changed evaluation target;
- **opposition review** deliberately changes the review objective to challenge premises, scope, architecture, hidden assumptions, and failure modes.

Operationally, the user reports that Opp Review has repeatedly surfaced material issues in real PMB work. The repository also contains a documented incident where repeated self-review did not catch a faulty review-gate design premise and a separate Opus review did.

That is not controlled benchmark evidence, but it is direct operational evidence that the mechanism is doing non-trivial work in this environment.

### Current research disposition

- **REINFORCE:** preserve Opp Review for tasks where a wrong premise or semantic defect is expensive.
- **REINFORCE:** evaluate it by defects discovered and downstream corrections, not merely by tokens consumed.
- **ASSESS:** whether triggers can be narrowed further without losing observed defect-detection value.
- **REJECT:** removing adversarial/independent review solely because frontier models may over-verify when given generic self-check instructions.
- **REJECT:** invoking Opposition mechanically for trivial edits where there is no meaningful premise to challenge.

---

# 7. Read before asking; ask one high-value question at a time

Interview Me contains a useful interaction pattern:

1. inspect existing artifacts first;
2. identify unresolved decisions;
3. ask one question at a time;
4. cap the interview;
5. prioritize questions whose answers materially change scope, architecture, risk, or proof of done.

### HE implication

Clarification should not become ritual questioning.

Ask when the answer changes the work materially. Otherwise proceed with a stated assumption or use existing project evidence.

This aligns with HE's preference for bounded interaction and avoiding speculative process overhead.

---

# 8. Rules with reasons are useful; eliminating hard rules is not

Prompt Master encourages rewriting unexplained emphatic rules into normal-language rules with reasons.

Useful:

```text
Do not modify CI in this task because CI changes require separate approval and can affect every contributor.
```

Less useful:

```text
CRITICAL!!! NEVER TOUCH CI!!!
```

However, the existence of a reason does not eliminate the need for hard enforcement when consequences are expensive.

### HE implication

Use:

- plain-language guidance where model judgment is sufficient;
- reasons where they improve generalization;
- deterministic hooks/CI/permissions where failure must not depend on model judgment.

Prompt style is not a substitute for enforcement.

---

# 9. Self-modifying skills create governance drift

Prompt Master's self-improvement section instructs the skill to edit its own behavior after user corrections or positive feedback.

This is attractive because it appears to learn preferences automatically, but it creates a new authority path:

```text
one interaction
    -> inferred durable rule
    -> skill rewrites itself
    -> future tasks inherit the change
```

Risks include:

- overfitting to a one-off correction;
- accidental policy changes;
- silent behavioral drift;
- weak provenance;
- hard-to-reproduce regressions;
- conflicts with higher-authority project guidance.

### HE principle

> **A reusable capability should not silently promote one session's feedback into durable governing instructions.**

Candidate improvements should be proposed, reviewed, and versioned rather than self-installed by default.

This does not reject learning from failures. It places the durable correction in the component that owns the incorrect information and keeps the change inspectable.

---

# 10. Skill conformance and task quality are different evaluations

Prompt Master's included eval asks agents to test whether the skill obeys its own prompt-rewriting rules.

That can establish conformance, but not necessarily downstream task quality.

Useful evaluation dimensions should remain distinct:

1. **instruction conformance** — did the skill follow its specification?
2. **task success** — did the resulting prompt/work produce the required result?
3. **evidence quality** — was correctness demonstrated?
4. **efficiency** — tokens, time, retries, and human intervention;
5. **regression** — did a change harm previously working cases?

Multiple agents applying the same rubric are not automatically independent evidence.

---

# 11. PMB-specific research candidate

PMB already contains a policy-level Handoff trigger at reported context >=40% and Claude auto-compaction at a later configured percentage.

The observed gap is that the 40% condition currently depends on someone noticing/reporting context pressure.

A minimal candidate experiment is therefore:

> expose the runtime-reported context percentage in the status line and observe whether the existing 40% handoff candidate threshold predicts useful rollover points.

Do **not** initially:

- force a handoff automatically;
- add a cumulative 300K-token counter;
- create multiple warning tiers;
- change the 65% auto-compaction setting;
- redesign memory/handoff state;
- claim 40% is optimal before measuring it.

Measure whether the signal changes behavior and whether handoffs preserve task continuity better than waiting for compaction.

---

# 12. Research disposition

## REINFORCE

- Direct runtime context-pressure measurement over cumulative-token guessing.
- New task -> new session where practical.
- Durable project state + narrow ephemeral handoff.
- Successor-goal-aware transfer.
- Reachability checks for handoff references.
- Read-before-ask and one-question-at-a-time clarification.
- Evidence-directed verification.
- Opposition/adversarial review where wrong premises are expensive and observed defect-detection value remains high.
- Plain-language rules with reasons where model judgment is appropriate.
- Explicit scope/deliverable/report-back bounds.

## ASSESS

- PMB status-line exposure of `context_window.used_percentage`.
- Whether 40% is a useful empirical handoff candidate threshold.
- Which failed approaches merit preservation across session rollover.
- Whether current Opp Review triggers can be narrowed without reducing material issue detection.

## PARK

- Automatic handoff initiation.
- Multiple handoff fidelity modes in PMB.
- Generic Interview Me / Prompt Master capabilities inside HE.

## REJECT

- Universal 300K/500K token rollover rules.
- Treating cumulative usage as active-context pressure.
- Removing targeted verification because generic self-check prompting can be wasteful.
- Removing Opp Review solely from token-efficiency guidance aimed at redundant self-checking.
- Silent skill self-modification from one-off feedback.
- Writing/copying files during handoff without explicit authority.
- Treating model-to-model agreement or self-eval alone as proof of correctness.
