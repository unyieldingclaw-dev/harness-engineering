# Bounded Execution Envelopes — 2026-09-22

## Purpose

Capture a cross-source Harness Engineering concept that became explicit during the 2026-09-22 mining pass on Eli the Computer Guy's orchestration discussion and was materially corroborated by Anthropic's first-party Opus 5.5 harness guidance on 2026-09-24.

Primary/source evidence:

- `01 Research/Sources/Eli the Computer Guy — Orchestration, Bounded Runs & Productized AI — 2026-09-22.md`
- `01 Research/Sources/Anthropic Opus 5.5 Prompting — Harness-Relevant Runtime Guidance — 2026-09-24.md`

Related HE research:

- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`

This note is research. It does **not** authorize a new HE control plane, PMB redesign, generalized model router, or dashboard implementation.

---

# Core concept

Agent autonomy should be broad enough to do useful work but bounded by deterministic limits owned outside the worker.

A useful abstraction is:

```text
Run / Task
  ├─ explicit authority
  ├─ scoped context
  ├─ allowed tools
  ├─ execution capability
  ├─ resource budget
  │   ├─ provider/token spend
  │   ├─ wall time
  │   ├─ iterations/retries
  │   └─ consequential actions
  ├─ stop conditions
  ├─ escalation conditions
  ├─ continuation-state requirements
  └─ effect-verification requirements
```

The worker may decide **how** to solve the task inside the envelope.

The worker should not be the sole authority deciding whether it may expand the envelope.

---

# Why this matters

Prompt instructions such as "be careful," "do not loop," or "keep costs low" are advisory. They are not execution controls.

For long-running or side-effecting work, the stronger pattern is:

```text
model freedom inside scope
        +
deterministic limits outside model
        +
observable stop reason
        +
small continuation state
        +
explicit reauthorization when needed
```

This connects several existing HE findings:

- **Authority must be explicit and local to the fact being asserted.**
- **Unknown is a state, not permission to act.**
- **Execution success and effect verification are separate.**
- **Historical context informs; current project state governs.**
- Scheduled autonomy needs lifecycle state, not merely a timer and prompt.
- Focused continuation is preferable to transcript transplantation.
- Observability should consume source-owned facts rather than becoming authority.

The new contribution is that **resource consumption and action scope belong inside the same bounded-run model as authority and verification**.

---

# What should be externally enforceable when the runtime supports it

Not every task needs every limit. Choose only limits tied to a real failure mode or consequential risk.

Potential controls include:

- maximum wall time;
- maximum provider spend or token usage;
- maximum retry/iteration count;
- tool allow/deny boundaries;
- filesystem or repository scope;
- external-action count or class;
- explicit approval gates for send/submit/delete/deploy/merge/purchase/permission changes;
- stop-on-uncertain-authority behavior;
- postcondition verification for consequential effects.

A harness should prefer native runtime/provider enforcement over prose rules where available.

---

# Stop is a lifecycle event, not a failure to preserve state

When a run reaches a boundary, the preferred behavior is not "keep going until done" and not "dump the full transcript."

Preserve the minimum successor state required for a safe continuation decision:

```text
objective
completed work
verified current state
unresolved work
reason stopped
resources consumed (when available)
required authority/budget to continue
next verification target
```

Then stop.

Continuation should be explicit when crossing the original envelope.

This is compatible with PMB's narrow handoff direction and HE's current-state-authority principle candidate.

---

# Relationship to orchestration

Bounded execution does **not** require a multi-agent orchestrator.

The concept applies equally to:

- one Claude/Codex session;
- one local-model review;
- an ACR batch;
- a scheduled automation;
- a delegated worker;
- a future multi-agent run.

Therefore HE should not implement an orchestration layer merely to gain bounded runs.

Prefer limits at the smallest component that actually owns the resource or action being bounded.

Examples:

- provider/token budget → provider/runtime adapter where possible;
- subprocess timeout → execution host;
- retry ceiling → caller that owns retries;
- repository scope → execution/worktree boundary;
- send/delete authority → action/tool boundary;
- continuation state → PMB/handoff only where PMB actually owns that state.

---

# PMB boundary

PMB should not automatically become the enforcer for every bounded-run dimension.

PMB's likely role, if evidence supports a change, is narrower:

- preserve stop/continuation state;
- expose run-related status that PMB genuinely owns;
- make authority/continuation boundaries clear in durable/transient project context;
- potentially record why a session stopped when that fact is needed for safe continuation.

PMB should **not** duplicate host/provider facts such as token counters, process timers, or execution liveness if the host owns them more reliably.

Before changing PMB, inspect current implementation and pilot evidence for a demonstrated failure that a bounded-run mechanism would solve.

**Current disposition: ASSESS, not IMPLEMENT.**

---

# 2026-09-24 first-party corroboration: completion and budgets

Anthropic's Opus 5.5 prompting guidance provides concrete runtime evidence for two parts of this model.

## Turn end is not task completion

Anthropic documents long unattended runs where a progress update may end a model turn while work remains. Its recommended harness behavior is to keep explicit task/checklist state and treat text-only turn termination as a report rather than proof of completion.

If no blocker exists and work remains, an automatic continuation may be appropriate — but Anthropic recommends stopping after roughly two or three automatic continuations rather than looping indefinitely.

This sharpens the execution-envelope model:

```text
model turn ended
      ≠
task complete

explicit open work + no blocker
      → bounded continuation

repeated continuation without closure
      → stop / review
```

### Durable implication

> **Completion state belongs to the task/harness contract, not to the model's decision to end a turn.**

## Advisory time signal is not a hard timeout

Anthropic also recommends model-visible elapsed-time/budget signals for some multi-agent workloads, but explicitly states that the budget is advisory and does not stop the model at the limit. A real hard stop requires an external timeout.

```text
model-visible budget
      → pacing guidance

runtime timeout
      → enforceable boundary
```

### Durable implication

> **Advisory budgets may guide model behavior; hard execution limits belong outside the model.**

This is now first-party corroboration for the existing HE distinction between prompt guidance and deterministic execution controls.

---

# Candidate HE principle — research status

> **Autonomy must operate inside an externally enforced execution envelope.**

A fuller working form is:

> **Give the worker freedom inside scope; keep authority expansion, resource ceilings, stop conditions, and consequential effect verification outside the worker.**

Supporting refinements:

> **Completion state belongs to the task/harness contract, not to model turn termination.**

> **Advisory budgets may guide model behavior; hard execution limits belong outside the model.**

The concept now has stronger cross-source support, including first-party Anthropic runtime guidance, but implementation should still follow observed workload needs and available enforcement points rather than becoming a universal HE control plane.

---

# Dispositions

## REINFORCE

- Bounded authority.
- Explicit stop conditions.
- Focused continuation state.
- Source-owned observability.
- Effect verification for consequential actions.
- Native/deterministic enforcement over prompt-only guidance.
- Explicit task completion state distinct from model turn termination.
- Bounded automatic continuation rather than open-ended retry.
- External timeout for a genuinely hard wall-time limit.

## ASSESS

- Which runtime/provider limits are actually available in Claude, Codex, local models, and ACR.
- Whether PMB has an observed continuation/runaway-work failure that bounded-run metadata would solve.
- Whether ACR already enforces adequate timeout/retry ceilings or needs stronger run budgets.
- Whether task/checklist state already provides enough completion evidence in the current PMB + Superpowers workflow.

## PARK

- Universal smart model routing.
- HE-owned token accounting service.
- New orchestration/control-plane product.
- Dashboard implementation before existing start criteria are met.

## REJECT

- Model self-policing as the only budget/control mechanism.
- Adding governance fields without an owning runtime or demonstrated consumer.
- Treating a timeout as proof of safe termination.
- Treating `end_turn` as proof that the assigned task is complete.
- Infinite automatic continuation.
- Full transcript dumping as bounded-run continuation state.

---

# Bottom line

Bounded runs are a useful HE concept because they preserve autonomy without granting open-ended authority or resource consumption.

The right implementation pattern is decentralized ownership:

> **put each limit at the component that can actually enforce it, keep completion state outside model self-reporting, preserve only the continuation state that must survive the stop, and avoid building a new control plane until a real workload requires one.**
