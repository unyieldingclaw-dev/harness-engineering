# Anthropic Opus 5.5 Prompting — Harness-Relevant Runtime Guidance — 2026-09-24

## Source status

Primary source:

- Anthropic — “Prompting Claude Opus 5.5”
- https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5-5

Supporting/secondary source:

- Jay E / RoboNuggets — “Anthropic Just Revealed 12 New Rules for Prompting Opus 5.5”
- https://www.youtube.com/watch?v=vsGwx28z4jk
- User-provided transcript and screenshots reviewed 2026-09-24.

Claude Code `AGENTS.md` implementation reference:

- https://github.com/anthropics/claude-code/blob/main/mods/agents-md/README.md

The Anthropic documentation is authoritative where the video paraphrase differs from it.

---

## 1. Effort is a workload control, not a universal quality ranking

Anthropic recommends starting Claude Opus 5.5 at `medium`, its default, then testing effort levels against the user's own evaluations. Anthropic explicitly warns against simply carrying over effort settings from Claude Opus 5.

Important details:

- `medium` is the starting recommendation, not a universal optimum;
- higher effort can materially increase thinking, latency, and output-token cost;
- `xhigh` and `max` should be reserved for work where measured quality improves;
- lower effort is the primary mechanism for reducing thinking cost/latency;
- effort labels are model-specific and should not be assumed equivalent across model generations.

### HE implication

> **Choose the lowest effort level that preserves required behavior for the workload being evaluated.**

A useful evaluation should use real tasks, fresh starts, identical inputs/tool surfaces, and observable correctness rather than one toy prompt or subjective impression.

### Disposition

- **REINFORCE:** Behavioral evals should govern model/effort configuration.
- **REJECT:** `medium` as permanent HE-wide doctrine.

---

## 2. Effort switching has cache semantics that belong to the runtime

The secondary video says changing effort mid-conversation no longer wipes cache. The primary Anthropic guidance is more specific:

- changing the **top-level** `effort` between requests invalidates the prompt cache;
- a **per-message effort change** (beta) can preserve the cache.

### HE implication

Provider/runtime configuration is part of the evaluated harness. A model capability claim is not enough to infer cache or context economics.

### Disposition

- **REINFORCE:** Capture runtime/provider conditions when they materially affect cost or behavior.
- **ASSESS:** Measure actual client behavior before assuming Claude Code exposes or uses cache-preserving per-message effort changes.

---

## 3. A text-only end turn is not completion evidence

Anthropic documents a specific Opus 5.5 long-run behavior: progress updates may end a turn even when the overall task remains incomplete.

Recommended harness behavior:

- maintain an explicit checklist or equivalent task-state artifact;
- treat a text-only `end_turn` as a report, not proof of completion;
- if work remains and there is no blocker, the harness may issue a short continuation message naming the remaining work;
- stop after roughly two or three automatic continuations rather than retrying forever;
- optionally use an independent/smaller model to check conversation state against the completion condition.

### HE implication

> **Model turn termination and task completion are different states.**

This is strong corroboration for bounded execution and for separating model assertion from completion evidence.

### Disposition

- **STRONGLY REINFORCE:** Explicit completion state.
- **STRONGLY REINFORCE:** Bounded automatic continuation.
- **REJECT:** Infinite “keep going” loops.

---

## 4. Advisory time budgets and hard timeouts are different mechanisms

Anthropic recommends elapsed-time signals and optional time budgets for multi-agent harnesses. The model uses them to pace/parallelize work.

The documentation explicitly says the budget is **advisory** and does not stop the model at the limit. If a hard stop is required, the harness must enforce its own timeout.

Anthropic also notes that tighter time pressure can reduce search or verification depth, so quality must be re-measured.

### HE implication

```text
model-visible elapsed time / budget
        → pacing signal

runtime timeout
        → actual execution boundary
```

### Candidate principle

> **Advisory budgets may guide model behavior; hard execution limits belong outside the model.**

### Disposition

- **STRONGLY REINFORCE:** Bounded Execution Envelopes.
- **REINFORCE:** Resource controls belong to the component that can enforce them.

---

## 5. “Time matters” is task-specific guidance, not global doctrine

Anthropic reports that a simple time-sensitivity instruction can reduce completion time when latency matters.

However, it is specifically a speed/parallelization lever and can trade away search/verification depth.

### Disposition

- **ASSESS:** Use only where latency is an explicit task objective.
- **REJECT:** Adding it globally to PMB/HE instructions.

---

## 6. “Treat earlier answers as settled” has an explicit contraindication

Anthropic suggests a two-sentence system-prompt instruction for chat applications where Opus 5.5 repeatedly reconsiders earlier answers and adds latency.

Anthropic also explicitly says to leave this instruction out where later evidence should be able to reopen earlier conclusions, including long analyses and agentic tasks. It may reduce spontaneous correction of earlier mistakes.

### HE/PMB implication

The current engineering workflow depends on opposition review, fresh-context verification, root-cause analysis, and later evidence correcting earlier assumptions.

### Disposition

- **REJECT for global PMB/HE instructions.**
- **PARK:** Optional chat-only/model-specific optimization if an observed latency problem justifies it.

---

## 7. Reasoning extraction is not an engineering evidence strategy

Anthropic documents a `reasoning_extraction` refusal category for prompts that push the model to reproduce internal reasoning in visible response text.

### HE implication

HE should continue to request:

- evidence;
- assumptions;
- uncertainty;
- observable checks;
- decision rationale that can be stated normally;

rather than depending on hidden chain-of-thought reproduction.

### Disposition

- **REINFORCE:** Evidence over reasoning dumps.

---

## 8. Visual tooling is evidence acquisition, not prompt decoration

Anthropic says Opus 5.5 reads dense visual material better than prior versions but still benefits on the hardest inputs from high-resolution sources plus image-processing tools such as PIL/OpenCV or a narrower crop/zoom tool.

### HE implication

> **When inspection quality is the bottleneck, improve evidence acquisition before adding more prompt prose.**

### Disposition

- **REINFORCE:** Tool-assisted evidence acquisition.
- **REJECT:** Building a general image-analysis subsystem without an observed need.

---

## 9. Multi-app exploration is powerful but expands the trust boundary

Anthropic suggests broader context exploration before action in workflows where relevant information may be distributed across email, documents, spreadsheets, CRM records, and similar systems.

Anthropic also warns to keep untrusted content out of searched records because the instruction causes the model to act on what it finds.

### HE implication

Broad retrieval improves completeness only when the trust/authority boundary is explicit.

### Disposition

- **ASSESS:** Retrieval breadth by task and trust boundary.
- **REINFORCE:** Unknown context is a state to investigate, not permission to act blindly.

---

## 10. `AGENTS.md` support should not create duplicate authority

The RoboNuggets video describes Claude Code support for `AGENTS.md`. The current Anthropic repository implements this through an `agents-md` plugin with configurable modes. The default plugin mode behaves as a fallback: if the project has its own Claude instruction files, the plugin stays out; otherwise `AGENTS.md` can be loaded in the same instruction-file role.

Other modes can load both files, but that is an explicit configuration choice.

### PMB implication

PMB already has intentional `CLAUDE.md` behavior. Adding `AGENTS.md` merely to mirror another convention would introduce duplicate authority unless a deliberate cross-harness migration is being performed.

### Disposition

- **PARK:** Cross-harness instruction portability assessment.
- **REJECT FOR NOW:** Adding parallel `AGENTS.md` to PMB without a demonstrated portability requirement.

---

## 11. Model-specific guidance has a lifecycle

Several Opus 5.5 recommendations exist because its runtime behavior differs from Opus 5. Some earlier scaffolding is now unnecessary; some new behaviors require different handling.

This reinforces the existing HE concept of Model Capability Drift:

> persistent guidance should survive only while it still encodes intentional project behavior, protects a deterministic/safety requirement, or prevents a demonstrated current failure mode.

### Disposition

- **STRONGLY REINFORCE:** Re-evaluate model-specific workarounds against current behavior and evals.

---

## Cross-source durable findings

Promote/reinforce the following mechanisms, not the “12 tips” framing:

1. Effort selection should be empirically calibrated per workload.
2. Cache semantics belong to runtime/provider evaluation.
3. Turn end is not proof of task completion.
4. Completion should be represented by explicit task state/checks.
5. Automatic continuation must itself be bounded.
6. Advisory time signals and enforceable timeouts are separate layers.
7. Model-specific prompt workarounds should not become permanent harness doctrine without evidence.
8. Evidence-acquisition tooling can outperform additional prompting when inspection is the bottleneck.
