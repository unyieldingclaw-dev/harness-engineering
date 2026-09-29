# Austin Marchese — Karpathy LLM Wiki, Autoresearch & Context Engineering — 2026-09-29

## Why this source was reviewed

User-provided video and screenshots:

- Austin Marchese — **How to 10x Your Claude Code Projects (Karpathy's Method)**
- YouTube: https://www.youtube.com/watch?v=yfeHoOkn2TI
- User-provided transcript captured 2026-09-29

Primary references checked:

- Andrej Karpathy — **LLM Wiki** gist: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- Andrej Karpathy — **autoresearch**: https://github.com/karpathy/autoresearch

Related HE material:

- `01 Research/Austin Marchese — Loop Engineering & Skill Mining — 2026-09-01.md`
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`
- `01 Research/Context Engineering.md.md`
- `01 Research/Navigable Truth, Routine Manifests & Derived Screens — 2026-09-25.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- prior Autoresearch / experimental-loop research

This note is research only. It does **not** authorize a PMB redesign, an Obsidian migration, automatic knowledge-base mutation, automatic skill rewriting, or unattended self-improvement.

---

# Executive finding

The video packages three Karpathy ideas together:

1. a persistent LLM-maintained knowledge base;
2. objective iterative experimentation through Autoresearch;
3. context engineering / progressive disclosure.

Most of those ideas reinforce HE research already present.

The most useful new evidence comes from the gap between **Karpathy's first-party mechanisms** and **Austin's implementation extensions**.

Karpathy's LLM Wiki separates:

```text
immutable raw evidence
        ↓
LLM-maintained derived synthesis
        ↓
schema / maintenance instructions
```

Karpathy's Autoresearch separates:

```text
mutable candidate implementation
        ↓
fixed resource budget
        ↓
fixed evaluator
        ↓
keep / revert
```

Austin extends these ideas with:

- `/ingest-source`;
- `/improve-system`;
- session-start reminder hooks;
- post-write ingestion nudges;
- expert-framework routing;
- loops and schedules;
- conversational history as a proxy for subjective quality.

The resulting HE lesson is not "copy Austin's AI OS." It is:

> **Automate maintenance and evidence handling more readily than promotion of interpretation into durable truth or governing instructions.**

A related distinction is important:

> **A reminder hook and a mutation hook have different authority.**

And Austin's subjective adaptation loop should not inherit the evidentiary strength of Karpathy's fixed-evaluator Autoresearch loop:

> **Feedback can generate candidate learnings; it does not automatically validate them.**

---

# 1. Karpathy's LLM Wiki has three explicit ownership layers

The first-party LLM Wiki gist defines three layers.

## Raw sources

Raw source documents are curated, immutable and treated as the source of truth. The LLM may read them but does not rewrite them.

## Wiki

The wiki is explicitly LLM-generated derived material: summaries, entities, concept pages, comparisons and synthesis. The LLM maintains cross-references and revises derived pages as new evidence arrives.

## Schema

A client-specific instruction file such as `CLAUDE.md` or `AGENTS.md` tells the agent how the wiki is structured and how ingest/query/maintenance workflows operate.

This is a useful ownership separation:

```text
source evidence       → authoritative input
compiled synthesis    → derived, maintainable view
schema                 → operating contract for the maintainer
```

### HE implication

This strongly corroborates existing HE distinctions between authoritative state, derived indexes/summaries and operating instructions.

> **Derived synthesis can compound without acquiring source authority.**

> **The schema may govern maintenance without becoming the evidence being maintained.**

This does not imply that PMB should adopt Karpathy's exact directory layout or wiki representation.

**Disposition: STRONGLY REINFORCE existing ownership model.**

---

# 2. Karpathy's ingestion workflow is more supervised than Austin's automation framing

Karpathy's gist says a source ingest can update many wiki pages, indexes and logs. It also says he personally prefers to ingest sources one at a time, read the summaries, inspect updates and guide emphasis.

Austin's demo moves farther toward automation. His proposed setup can use a hook/skill combination to process a new source, extract concepts, update wiki pages, add cross-links and update indexes.

Those are not equivalent authority models.

A useful spectrum is:

```text
A. notify that source exists
B. deterministically normalize / index source
C. propose derived updates
D. write derived synthesis automatically
E. mutate governing instructions automatically
```

Risk and authority increase down the list.

### HE implication

> **Automating source bookkeeping does not automatically justify automating semantic promotion.**

Examples of relatively low-risk automation:

- copy/normalize an authorized source;
- compute hashes/metadata;
- update a deterministic catalog;
- flag orphaned references;
- remind the operator that ingestion is pending.

Higher-risk operations include:

- deciding which existing conclusions are superseded;
- promoting a one-off interpretation to durable project truth;
- rewriting project rules/skills because a conversation appeared to go better;
- changing acceptance criteria based on the same model's retrospective.

**Disposition: REINFORCE authority separation.**

---

# 3. Reminder hooks and mutation hooks are materially different

Austin's screenshots show a practical pattern where `SessionStart` hooks emit reminders such as:

- run a YouTube sync command on selected days;
- run `/improve-system` after enough time has passed.

The user then decides whether to invoke the durable-learning operation.

That is different from a hook that directly rewrites knowledge, rules or skills.

```text
reminder hook
    event → notification → human/worker chooses action

mutation hook
    event → persistent state change
```

### HE implication

> **A hook that surfaces attention does not possess the same authority as a hook that mutates durable state.**

This should be considered when evaluating hook risk and required evidence.

A reminder can be broadly useful with little governance burden. A persistent mutation should have an owner, evidence, scope and rollback path appropriate to the state it changes.

This is consistent with HE's existing bounded-authority work.

**Disposition: NEW REFINEMENT / REINFORCE.**

---

# 4. `/improve-system` is useful as evidence capture, dangerous as automatic promotion

Austin describes using the back-and-forth of a successful editing session as a proxy for what improved the output, then running `/improve-system` to capture lessons into the system.

This is useful because real corrections contain evidence about friction and missing context.

But conversation history is an ambiguous evaluator.

A user edit may represent:

- a reusable preference;
- a project-specific constraint;
- a one-time exception;
- missing source context;
- a model execution mistake;
- a changed requirement;
- an incorrect correction;
- a genuinely reusable process lesson.

Therefore:

```text
conversation correction
       ↓
candidate learning
       ↓
ownership / recurrence / consequence analysis
       ↓
proposed smallest durable change
       ↓
behavioral evaluation or later corroboration
       ↓
promotion if earned
```

### HE implication

This reinforces `Experience-Derived Harness Evolution`:

> **History proposes harness changes; experiments earn them.**

Austin's mechanism is useful for **capture and surfacing**. It should not be treated as proof that a durable skill/rule should change.

**Disposition: STRONGLY REINFORCE existing controlled-improvement boundary.**

---

# 5. Subjective feedback loops are not equivalent to Autoresearch

Karpathy's Autoresearch works because the acceptance envelope is unusually controlled.

The first-party repo fixes key parts of the experiment:

- a narrow mutable implementation surface;
- a baseline;
- a fixed evaluator/metric;
- a fixed time budget;
- repeated candidate changes;
- keep improvements, revert losses.

Austin explicitly notes that many ordinary tasks do not have this kind of objective metric. He proposes adapting the mindset to subjective outputs by mining conversational correction or using delayed business metrics such as conversion.

Those can still be useful loops, but they are epistemically different.

```text
objective loop
  candidate → fixed evaluator → measured result

subjective adaptation loop
  candidate → human/model feedback → inferred lesson
```

The second loop has additional uncertainty:

- evaluator drift;
- preference noise;
- delayed/confounded outcomes;
- model self-evaluation bias;
- ambiguous causal attribution.

### HE implication

> **Do not inherit the evidentiary strength of an objective experiment loop when the evaluator has become subjective, model-generated or confounded.**

For subjective work, useful evidence may include:

- blinded comparisons;
- repeated human preference;
- narrow semantic rubrics;
- production/business outcome metrics;
- independent evaluator calibration;
- delayed observation over multiple comparable cases.

The loop may still improve the system; its claims should simply remain bounded by the quality of the evaluator.

**Disposition: NEW REFINEMENT / REINFORCE behavioral-eval discipline.**

---

# 6. The "under 50 lines" CLAUDE.md rule is not a durable rule

Austin recommends a short project `CLAUDE.md` and demonstrates a prompt asking for fewer than 50 lines. He immediately acknowledges that 50 is arbitrary.

The useful mechanism is not the numeric limit.

### HE implication

> **Always-loaded context should earn its place; specialized context should move behind progressive disclosure where practical.**

Persistent project instructions are appropriate for information that is:

- broadly applicable;
- stable enough to justify automatic loading;
- difficult or costly to rediscover;
- behaviorally important across task classes.

Task-specific frameworks, examples, source corpora and specialized procedures should generally be discoverable/retrievable rather than always loaded.

**Disposition: REINFORCE Context Engineering; REJECT fixed line-count rule.**

---

# 7. Expert-framework routing is progressive disclosure, not expert simulation proof

Austin's `expert-advice` skill classifies a question and loads one or two relevant frameworks instead of injecting the entire knowledge collection.

Mechanically:

```text
question
   ↓
classify need
   ↓
retrieve narrow relevant reference set
   ↓
produce answer
```

That is a useful progressive-disclosure pattern.

The specific mapping of business topics to named personalities is not an HE architecture principle and should not be treated as reproducing those people's actual judgment.

### HE implication

> **Route to relevant source-grounded frameworks; do not load the whole library or confuse a sourced perspective with the person.**

This reinforces prior HE research on Skills, context routing and perspective panels.

**Disposition: REINFORCE.**

---

# 8. The wiki is a navigation layer as well as a synthesis layer

Karpathy's gist describes `index.md` as a content-oriented catalog used to locate relevant pages, then drill into them. At moderate scale he reports this can be enough without embedding infrastructure.

This is useful because it separates the question:

> "Can we build a semantic/vector index?"

from:

> "What is the smallest navigation mechanism that reliably finds the needed source or derived page?"

### HE implication

This corroborates `Navigable Truth, Routine Manifests & Derived Screens`:

> **Derived navigation structures should earn their lifecycle cost and should not acquire truth ownership.**

Start with simple catalog/search structures if they meet the workload. Escalate to heavier retrieval infrastructure only when measured retrieval failures justify it.

**Disposition: REINFORCE.**

---

# 9. Cross-links and graph views are inspectability aids, not correctness evidence

Austin highlights Obsidian graph view as a way to inspect whether a knowledge base is interlinked.

That can help humans spot:

- isolated pages;
- dense hubs;
- unexpected structure;
- likely missing links.

But a graph that looks richly connected does not establish that:

- citations are correct;
- summaries are faithful;
- contradictions are resolved correctly;
- the right evidence is retrieved for the task.

### HE implication

> **Navigability/visual structure can reveal maintenance problems; it is not a substitute for evidence fidelity.**

Do not optimize the graph as an end in itself.

**Disposition: PARK visualization as optional operator aid.**

---

# 10. Hooks, loops and schedules are triggers; they do not define the quality boundary

Austin presents loops, schedules and hooks as ways to make improvement work recur without constant manual prompting.

These mechanisms answer:

- **when** should something run?

They do not answer:

- what is allowed to change;
- what counts as evidence;
- what evaluator owns acceptance;
- what should happen on uncertainty;
- whether a durable mutation is justified.

### HE implication

> **Trigger automation and acceptance authority are separate design dimensions.**

This directly aligns with Bounded Execution and routine-manifest research.

**Disposition: REINFORCE.**

---

# 11. What should be mined vs parked

## REINFORCE

- immutable/source-owned evidence separate from LLM-maintained synthesis;
- schema/instructions separate from the evidence being maintained;
- progressive disclosure over always-loaded specialized context;
- simple derived navigation before heavier retrieval infrastructure;
- conversational history as evidence for candidate improvements;
- owner-mapped promotion rather than indiscriminate rule growth;
- reminder hooks as low-authority attention mechanisms;
- bounded measurable loops with fixed evaluators where possible;
- evaluation claims limited to the quality of the evaluator.

## ASSESS

- source-ingestion workflow as a capability boundary;
- periodic reminder/nudge for evidence-based system review;
- which derived knowledge structures are actually useful for PMB/HE retrieval;
- whether subjective quality workflows can be given stable rubrics or delayed objective measures;
- whether an `/improve-system`-style review uniquely reduces recurring friction beyond incident-driven learning.

## PARK

- automatic semantic ingest of every new source;
- Obsidian graph view as an HE requirement;
- loops/schedules solely because they exist;
- expert-personality routing as architecture;
- an HE-wide wiki subsystem;
- automatic self-rewriting skills/rules.

## REJECT

- fixed 50-line root-instruction limit;
- treating conversational edits as validated durable rules;
- calling subjective self-improvement loops equivalent to fixed-evaluator Autoresearch;
- allowing ingestion hooks to silently rewrite authoritative project truth;
- treating a richly linked wiki as proof of factual correctness.

---

# 12. Canonical-owner mapping

The findings should remain owned by existing HE syntheses rather than spawning another architecture layer:

| Finding | Canonical owner |
|---|---|
| always-loaded vs progressive context | `Context Engineering.md.md` |
| raw evidence vs derived navigation/synthesis | `Navigable Truth, Routine Manifests & Derived Screens — 2026-09-25.md` |
| conversation corrections as candidate learning | `Experience-Derived Harness Evolution — 2026-09-25.md` |
| objective evaluator / keep-revert experiment loop | existing Autoresearch / behavioral-eval research |
| reminder hook vs mutation authority | `Bounded Execution Envelopes — 2026-09-22.md` + experience-evolution research |
| ingest/source preservation | `Source Preservation & Evidence Durability — 2026-09-16.md` |

No new HE subsystem follows automatically.

---

# Bottom line

Austin's video is useful because it places several good ideas in one practical workflow, but the strongest HE result comes from **not collapsing them**.

```text
raw evidence
    ≠
derived synthesis
    ≠
maintenance instructions
    ≠
feedback telemetry
    ≠
validated improvement
    ≠
automation trigger
```

Karpathy's first-party work is strongest where these boundaries are explicit: immutable sources, derived wiki, schema, fixed experiment evaluator and bounded mutable surface.

Austin's extensions are most useful when they preserve those boundaries: progressive retrieval, reminders, ingestion assistance and candidate-learning capture.

They become weaker when conversational correction is treated as automatic proof that the durable system should rewrite itself.

The HE stance remains:

> **Use automation to reduce bookkeeping; use evidence to earn durable behavioral change.**
