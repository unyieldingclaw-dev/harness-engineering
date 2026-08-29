# Claude Token Limits & Master Prompt — Context Efficiency Lessons

**Source:** Jack Roberts — *Paste This Into Claude, Never Hit a Token Limit Again*

**YouTube:** https://www.youtube.com/watch?v=xXS83RALMtc

**Reviewed:** 2026-08-29

**Assessment status:** Research input only. No architectural change implied.

---

## Source Summary

The video presents a "master prompt" intended to audit and optimize an agent's setup and reduce unnecessary context/token consumption. Its workflow is organized into three phases:

1. **Audit** — inspect tools, MCP servers, recent sessions, repository scope, scripts, skills, scheduling, and related configuration.
2. **Fix** — apply selected context/cost reductions, shorten or consolidate skills, route work to cheaper models where appropriate, and improve session hygiene.
3. **Report** — summarize the before/after state and reported savings.

The video's broader argument is that users should be more deliberate about agentic coding because model usage limits and costs can be consumed quickly. It recommends using deterministic automation where an agent is not actually required and preserving model usage for work that benefits from reasoning.

The video also claims a large percentage of "waste" can be found in some setups and frames the prompt as a way to avoid hitting token limits. Those quantitative claims are not treated as general evidence here.

---

## Useful Observations

### 1. Context is a budget

Persistent instructions, session history, MCP/tool descriptions, skills, screenshots, and other context all compete for model context and attention. More available context is not automatically better.

**Harness Engineering implication:** evaluate context by relevance and operational value rather than maximizing the amount available to an agent.

### 2. Avoid repeated rediscovery

Stable information should not need to be rediscovered from scratch in every session when a durable, inspectable artifact can provide the required context more efficiently.

This reinforces existing Harness Engineering work around:

- durable artifacts;
- progressive disclosure;
- context ownership;
- handoff;
- reusable skills;
- separating project state from transient session state.

### 3. Prefer deterministic automation when reasoning is unnecessary

The video gives bookkeeping/file-processing automation as an example: use code or scheduled automation for deterministic work, and use an agent to create, modify, or reason about the automation rather than repeatedly consuming model tokens to perform the deterministic operation.

**Harness Engineering implication:** distinguish **agent-required work** from **automation-required work**. Do not introduce an agent merely because an existing deterministic process could be wrapped in one.

### 4. Session hygiene can reduce unnecessary context

The video recommends practices such as clearing unrelated session context, batching related questions, editing rather than repeatedly sending corrective messages, and keeping agent responses appropriately concise.

These are workflow practices rather than architecture. They are candidates for measurement, not blanket rules.

### 5. Model routing is a potential cost-control mechanism

The video recommends using less expensive/capable models for work that does not require the strongest model.

**Harness Engineering implication:** model selection can be treated as an operational optimization, but routing complexity must be justified by measurable benefit. A multi-model architecture should not be introduced merely because it sounds efficient.

### 6. Reusable procedures can be more valuable than giant prompts

The useful part of the "master prompt" is not its size. It attempts to turn recurring operational knowledge into a repeatable procedure.

This is consistent with the distinction between:

- persistent project context;
- reusable skills/capabilities;
- task-specific context;
- orchestration.

A giant startup prompt should not become the default replacement for those boundaries.

---

## Claims Requiring Skepticism

### "Never hit a token limit again"

Not supported literally. Better context hygiene can reduce consumption, but it cannot remove provider quotas, context-window limits, rate limits, or subscription usage limits.

### Large percentage "waste" claims

The video presents large savings/waste figures from its own demonstrations. These should be treated as setup-specific measurements unless independently reproduced under controlled conditions.

### Universal master prompt

A prompt that audits everything before beginning work can itself consume substantial context. The audit is useful when it discovers a real problem; running a comprehensive audit before every task can become the waste it is intended to prevent.

---

## Relationship to Existing Harness Research

This source reinforces several existing themes:

- **Progressive Disclosure** — do not load potentially useful information merely because it exists.
- **Context as a Managed Resource** — relevant context is more valuable than maximum context.
- **Deterministic Enforcement / Automation** — use code for deterministic work where an agent adds no necessary judgment.
- **Capability vs Orchestration** — reusable procedures should not automatically become a mandatory workflow.
- **Model Capability Drift** — practices that compensate for weaker models may become unnecessary as models improve.
- **Evidence Before Architecture** — measure actual context/cost problems before introducing routing, compaction, retrieval, or additional infrastructure.

### Candidate principle

> **Context is a budget, not a capability.**
>
> Give an agent the information it needs when it needs it; do not treat maximum available context as a design goal.

### Candidate assessment question

> Can the Harness reduce unnecessary context consumption without reducing task performance or making the system harder to understand and maintain?

---

## What We Should *Not* Copy

- Do not install or mandate the video's "master prompt" as a universal startup prompt.
- Do not treat reported percentage savings as established evidence.
- Do not add model-routing infrastructure without an observed cost/performance problem.
- Do not replace durable artifacts, skills, or progressive disclosure with one giant prompt.
- Do not add a new agent where deterministic automation is sufficient.
- Do not optimize token consumption at the expense of necessary evidence or context.

---

## Experimental Value

This source is a good candidate for controlled Harness Engineering experiments around **context efficiency**.

A useful experiment would compare equivalent tasks under:

1. baseline project/session context;
2. deliberately minimized relevant context;
3. additional persistent context;
4. task-specific progressive disclosure.

Measure at minimum:

- task success;
- verification failures;
- model input tokens;
- model output tokens;
- wall-clock time;
- number of corrective turns;
- human intervention required.

The goal should be to determine whether reduced context actually improves operational efficiency, not merely whether the token count decreases.

---

## Overall Assessment

**Value: Moderate as workflow research; low as an architectural authority.**

The strongest contribution is the reminder that context and model usage are finite operational resources and that deterministic automation should absorb work that does not require model reasoning.

The headline claim and large savings claims should be treated as marketing until independently reproduced.

The useful idea for Harness Engineering is not **"use this master prompt."** It is:

> **Make context intentional, progressive, and measurable.**

---

## Related Research

- Anthropic context-engineering research — progressive disclosure and reducing unnecessary persistent guidance.
- Agent Skills ecosystem research — reusable capabilities and progressive-disclosure boundaries.
- Memory / dreaming research — consolidating durable knowledge outside active sessions rather than continually carrying all historical context.
