# Austin Marchese — Anthropic Growth Workflows, Sub-Threshold Work & Bespoke Tools — 2026-09-29

## Source

User-provided video and transcript:

- Austin Marchese — **How Anthropic Employees ACTUALLY Use Claude Code to Grow**
- YouTube: https://www.youtube.com/watch?v=BX5dLXe6CTI
- User-provided transcript/screenshots captured 2026-09-29

This note treats the video as a secondary synthesis of Anthropic growth-team practices. It preserves the mechanisms that matter to Harness Engineering without accepting the video's causal framing that the workflows themselves explain Anthropic's revenue growth.

---

# Executive finding

The useful material is not a new Claude Code architecture. It is a better way to discover AI opportunities.

The video clusters work into four patterns:

```text
1. EXISTING WORK
   automate a known recurring workflow

2. SUB-THRESHOLD WORK
   do useful work that humans previously skipped because manual cost exceeded expected value

3. BESPOKE LOCAL CAPABILITY
   build a narrow tool/skill for one exact stack, role or workflow

4. DECISION SUPPORT
   use AI to surface patterns, alternatives and next questions for a human decision-maker
```

The second category is the strongest new HE signal.

Most automation discovery starts from:

> What repetitive task am I already doing?

The source adds:

> What valuable work am I not doing at all because the old manual economics made it irrational?

That expands continuous-improvement research from **friction reduction** into **opportunity discovery**.

But cheaper implementation is not itself proof that the newly feasible task is worth doing. Scope, verification, data access, maintenance, runtime cost, authority and ownership still have to justify the capability.

---

# 1. Existing-work automation is useful but conceptually simple

The source shows recurring work being compressed into skills/tools:

- weekly reporting;
- ad-copy variation generation;
- campaign-data analysis;
- repeated data transformation;
- export/import workflows.

The transferable mechanism is:

```text
known workflow
   ↓
reconstruct actual steps + exceptions
   ↓
separate deterministic steps from judgment
   ↓
encapsulate repeated behavior
   ↓
retain human approval where consequence requires it
   ↓
measure time/quality/error change
```

### HE implication

> **Do not automate the vague description of a process; automate the observed process after its inputs, decisions, exceptions, outputs and authority boundaries are understood.**

A Skill is only one implementation mechanism. A script, existing tool, repository workflow, deterministic check or no automation may be smaller and better.

---

# 2. Sub-threshold work is the strongest new opportunity class

The source explicitly describes work that previously failed an ROI test because human time made it not worth attempting.

Examples include:

- exhaustive search-term review;
- many low-cost growth experiments;
- personalized customer summaries that would have taken too long manually;
- large-volume data review that no human would perform item-by-item.

The durable HE distinction is:

```text
OLD QUESTION
Can AI make an existing task cheaper?

NEW QUESTION
Does lower execution cost make a previously uneconomic task worth considering at all?
```

This can expose useful work such as:

- exhaustive consistency scans;
- large-scale stale-reference checks;
- cross-repository drift detection;
- repetitive evidence assembly;
- historical false-positive mining;
- candidate regression-fixture generation;
- broad anomaly scans;
- low-frequency quality checks humans routinely skip.

### Important guardrail

Lower implementation cost can also make low-value work look attractive.

A candidate remains unjustified if:

```text
expected avoided loss / gained value
        ≤
build + verification + maintenance + runtime + review + false-positive cost
```

### HE principle candidate

> **AI changes the feasibility frontier, not the value function.**

or operationally:

> **Revisit work that was rejected because of execution cost, but re-run the value test instead of assuming newly cheap work deserves to exist.**

---

# 3. "Audience of one" is a strong fit for bounded local capabilities

The source repeatedly argues that useful internal tools may be hyper-specific to one person's stack, edge cases and workflow.

That is consistent with HE's existing capability-before-platform direction.

A narrow tool can be justified even if no generalized product market exists, provided it has:

- a demonstrated recurring need;
- a clear owner;
- small enough implementation/maintenance cost;
- explicit inputs and outputs;
- bounded authority;
- measurable local benefit;
- easy replacement/removal.

### HE refinement

> **Local specificity is not technical debt by itself. Premature generalization can cost more than maintaining a narrow capability that solves one demonstrated workflow well.**

However:

> **"Build only for yourself" does not remove verification, security, provenance, maintenance or authority obligations.**

The source's claim that custom apps/workflows are now "close to zero" cost should therefore be treated as rhetorical. Coding cost may be much lower; lifecycle cost is not zero.

---

# 4. Decision support should expand judgment, not silently acquire authority

The source's fourth category shifts from "do this" to "do this and tell me what deserves attention / what we should consider next."

Examples include:

- scheduled review of many charts;
- surfacing concerning metrics;
- generating candidate next actions;
- source-grounded expert-perspective simulation.

The useful pattern is:

```text
large evidence surface
      ↓
model reduces / ranks / challenges
      ↓
human receives:
  facts
  anomalies
  hypotheses
  open questions
  candidate next actions
      ↓
human or owning system decides
```

### HE implication

> **Decision support should reduce the cost of seeing the decision surface without silently taking ownership of the decision.**

This aligns with the existing separation of evidence, inference and authority.

The "manager clone" / expert-perspective pattern needs an explicit epistemic boundary:

> **A source-grounded simulation is a modeled perspective, not the represented person's actual judgment.**

The value is in retrieving and applying cited frameworks consistently, not in pretending the model has recreated the individual.

---

# 5. Opportunity discovery should distinguish four different investment cases

A useful HE decision frame from this source is:

| Opportunity class | Core question | Typical evidence |
|---|---|---|
| Existing workflow | Can repeated known work be made cheaper/safer/faster? | current process time, errors, frequency |
| Sub-threshold work | Does reduced execution cost make previously skipped work worthwhile? | expected value, prior reason skipped, new marginal cost |
| Bespoke local capability | Is one narrow recurring local problem worth a dedicated tool? | recurrence, local fit, maintenance burden |
| Decision support | Can broader/faster evidence review improve a human decision? | missed signals, review burden, consequence of error |

These should not share one adoption rule.

For example, a repetitive deterministic export transform may require almost no model authority, while a daily "what matters?" briefing requires explicit evidence/inference labeling and stronger review of false positives/omissions.

---

# 6. Reusable Skills should emerge from understood workflows

The source suggests turning weekly workflows into one-command Skills.

The durable mechanism is useful, but HE should preserve a promotion boundary:

```text
observed recurring workflow
        ↓
steps + edge cases understood
        ↓
smallest implementation chosen
        ↓
behavior validated
        ↓
promote to reusable Skill only if the capability earns reuse
```

Do not start with "make a Skill" merely because Skills are available.

This reinforces existing HE guidance that repeated procedure may belong in a Skill, hard invariants in deterministic checks, and simple transformations in code.

---

# 7. Large-volume review is a natural AI fit when output is attention, not silent mutation

One of the source's strongest practical patterns is reviewing more evidence than a person can reasonably inspect manually, then surfacing a smaller attention set.

Examples:

- many dashboards/charts;
- search-term datasets;
- performance anomalies;
- large sets of candidate experiments.

A safer default shape is:

```text
source-owned raw data
       ↓
deterministic preprocessing where possible
       ↓
model triage / anomaly hypotheses
       ↓
small attention set + evidence pointers
       ↓
human or owning workflow confirms action
```

### HE implication

> **Use models to expand inspection coverage before expanding mutation authority.**

This is particularly compatible with read-only pilots.

---

# 8. Work-discovery itself needs evaluation

Opportunity mining can easily become idea generation theater.

Do not optimize for:

- number of automation ideas;
- number of Skills created;
- number of "AI use cases" identified;
- number of previously skipped tasks now attempted.

Measure instead:

- recurring human time actually removed;
- consequential errors avoided;
- useful evidence surfaced that would otherwise be missed;
- previously skipped work whose realized value exceeded lifecycle cost;
- false positives / unnecessary review introduced;
- maintenance cost;
- whether a bespoke tool remains useful after repeated runs;
- whether decision quality or time-to-decision materially changed.

### HE principle candidate

> **Opportunity discovery creates hypotheses; repeated operational value earns permanence.**

---

# 9. Relationship to existing HE research

## Experience-Derived Harness Evolution

That synthesis currently focuses on observed failures, friction and repeated work.

This source adds a complementary proactive question:

> **What valuable work is absent from the trace because humans previously decided it was too expensive to perform?**

History mining cannot discover all such opportunities because the missing work may leave no execution trace.

Opportunity discovery may therefore need:

- explicit interviews;
- abandoned backlog/idea review;
- repeated "we don't have time to" statements;
- high-volume evidence sources that are only sampled today;
- known manual thresholds or sampling shortcuts;
- decisions made with incomplete review because exhaustive review was uneconomic.

## Bounded Execution / authority

Sub-threshold work should often start read-only. Cheap execution is not justification for broader side effects.

## Context Engineering

Bespoke capabilities should retrieve only task-relevant frameworks/data rather than turning every personal reference into always-loaded context.

## Artifact-Gated / evaluation

Decision-support quality requires evaluating missed signals, false positives and downstream action quality—not merely whether the model generated plausible recommendations.

---

# 10. Implications for HE / PMB / ACR research

## HE

Potential opportunity-discovery questions:

- Which source-maintenance checks are skipped because they are tedious rather than unimportant?
- Which research principles are likely duplicated or drifting but never audited because manual review is expensive?
- Which external-link/source durability checks would be useful if cheap enough?
- Which research evidence can be structurally reduced before human/model interpretation?

## PMB

Potential below-threshold candidates should remain hypotheses until pilot evidence supports them:

- stale pointer/link scans;
- orphan durable-state detection;
- contradiction candidates;
- retrieval misses;
- unused frequently loaded context;
- recurring reconstruction of handoff state.

Do not convert PMB into a generalized knowledge-maintenance daemon merely because these checks are now possible.

## ACR

Potential high-value candidates include:

- mining historical false-positive families;
- recurring evidence-mismatch classes;
- candidate clean/dirty regression fixture generation;
- reviewer-overlap analysis;
- model/runtime bakeoff experiments that were historically too expensive to run often.

Again, candidate generation is not calibration evidence.

---

# Research disposition

## REINFORCE

- automate understood repetitive work with the smallest mechanism;
- seek work that became feasible because AI changed marginal execution cost;
- build narrow local capabilities before generalized platforms;
- use AI to expand inspection coverage while retaining decision/side-effect authority appropriately;
- source-grounded perspective simulation is advisory, not the represented person's authority;
- evaluate lifecycle cost, not coding cost alone.

## ASSESS

- explicit "sub-threshold work" discovery during HE/PMB/ACR improvement reviews;
- read-only high-volume scans that were historically skipped for cost reasons;
- one-off bespoke capabilities with repeated local value;
- whether scheduled attention briefs materially reduce missed issues or decision latency.

## PARK

- generalized automation marketplace;
- automatic conversion of every recurring workflow into a Skill;
- broad autonomous experimentation from the CASH marketing example;
- permanent expert/personality simulation infrastructure.

## REJECT

- assuming lower implementation cost proves the task is worth doing;
- treating a source-grounded simulated person as the actual person's judgment;
- measuring AI adoption by quantity of workflows/tools rather than realized operational value;
- broad mutation authority merely because read/analysis coverage became cheap.

---

# Bottom line

The strongest addition from this source is not "use Claude Code for growth."

It is a broader opportunity-discovery rule:

> **Look not only for repeated work to automate, but also for valuable work that was never performed because its old manual cost made it irrational. Re-evaluate that work under the new cost structure, then require the same evidence, ownership, authority and maintenance discipline as any other capability.**
