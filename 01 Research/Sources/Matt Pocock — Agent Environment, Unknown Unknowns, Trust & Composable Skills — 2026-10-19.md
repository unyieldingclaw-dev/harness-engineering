# Matt Pocock — Agent Environment, Unknown Unknowns, Trust & Composable Skills — 2026-10-19

## Source set

Primary interview supplied by Nolan:

- Nate Herk with Matt Pocock — **Matt Pocock: The Most Valuable Skill to Learn in the Age of AI**
  - https://www.youtube.com/watch?v=Z8lf5oWKfj0
  - user supplied a full transcript in the research session; interview claims are practitioner reports, not controlled performance evidence

Primary implementation source inspected directly:

- `mattpocock/skills`
  - https://github.com/mattpocock/skills
  - inspected at `5beb35b040e3de97e54fa9e386d7e42d15c295c2`
  - latest inspected commit message: `chief-of-staff: keep memory in a dated, commit-cited wiki (#1304)`

Relevant inspected files:

- `README.md`
- `skills/productivity/grilling/SKILL.md`
- `docs/productivity/grill-me.md`
- `skills/engineering/grill-with-docs/SKILL.md`
- `docs/engineering/wayfinder.md`
- `skills/in-progress/chief-of-staff/SKILL.md`
- `skills/productivity/teach/SKILL.md`
- `skills/productivity/writing-for-agents/SKILL.md`
- `skills/engineering/improve-codebase-architecture/SKILL.md`

## Overlap and independence boundary

The same Nate Herk / Matt Pocock interview is already represented on HE `main` in `01 Research/Sources/Matt Pocock — Agent Environment, Intent & Verification Interview — 2026-10-19.md`. **Do not double-count** the two notes as independent support. This PR's incremental source material is its inspection of the pinned `mattpocock/skills` repository and the explicit Specialist Bench comparison; merge/reconcile overlapping interview claims with the canonical existing note before accepting a second long source.

A skill's checked-in text is evidence of intended behavior, **not** evidence that it executes reliably, improves outcomes, or is enabled by default. The named skills and in-progress Chief of Staff are examples for assessment, not authorized implementations.

## Why this source matters to HE

The interview is useful because it pushes against two easy failure modes in AI engineering:

1. treating more detailed upfront process/specification as automatically better; and
2. treating stronger models or more agents as substitutes for a good execution environment.

Pocock's recurring argument is that the agent still faces a communication gap with the human and that high-quality work depends heavily on the environment the agent operates in: code structure, tests, linting, browser access, logs, terminology, and other feedback surfaces.

The repository provides concrete mechanisms for that philosophy and also exposes its limits.

The strongest HE synthesis is:

> **Communicate intent and values clearly, then make the environment enforce and reveal as much correctness as possible. Use procedural scaffolding only where it resolves a demonstrated uncertainty or failure.**

---

# 1. "Tell agents what you care about" is not the same as "tell agents every step"

In the interview, Pocock repeatedly frames the central problem as a communication gap. The model cannot infer all project-specific values, priorities, terminology, tradeoffs, or hidden constraints.

His proposed response is not a giant permanent procedure. It is to make the things that matter legible.

Examples include:

- explicit project values;
- domain language;
- tests and checks;
- architecture conventions;
- deployment/runtime access;
- source-backed teaching/research;
- interactive clarification when design is under-specified.

### HE interpretation

This reinforces a distinction HE should preserve:

```text
WHAT WE CARE ABOUT
    durable intent / invariants / constraints

HOW THIS TASK WORKS
    triggered skill / procedure / workflow

WHAT IS TRUE RIGHT NOW
    repository / runtime / observed evidence
```

Persistent guidance should favor the first category.

Do not turn every successful procedure into always-loaded policy.

---

# 2. Environment quality can reduce model effort requirements

Pocock says he commonly uses Opus 5.5 at medium effort and is considering low effort because he has "raised the floor" around the agent. He attributes much of the improvement to tests and a better-operating codebase rather than simply to higher reasoning effort.

He gives a concrete example from a video-editor project: once the agent could actually run the application, click through it, access needed dependencies, and take screenshots, output quality improved substantially.

This is anecdotal evidence, not a universal benchmark.

### HE interpretation

> **Before escalating model effort, ask whether the agent is missing an executable feedback surface.**

Candidate diagnostic order:

1. Is the task/intent clear?
2. Can the agent observe the relevant system?
3. Can it run deterministic feedback loops?
4. Is the code/environment navigable?
5. Only then: does the task require a stronger model or higher effort?

This aligns with HE's existing minimum-sufficient-mechanism and environment/runtime work.

---

# 3. Spec-driven development failure: useful warning, not a universal rejection of specs

Pocock describes a months-long attempt to use persistent detailed specifications as the main driver of a complex personal codebase. His reported result was degrading code and degrading agent output. He eventually refocused on code quality and ease of modification.

The evidence here is one experienced practitioner's repeated experience, not a controlled study.

HE should therefore **not** conclude:

- specs are bad;
- planning is bad;
- persistent decision artifacts are bad.

The narrower useful interpretation is:

> **A specification should not substitute for maintaining the executable environment the agent must actually change.**

A stale or over-complete spec can become another derived representation that competes with code/runtime truth.

### HE consequence

Treat specs as communication/decision artifacts whose authority is explicit.

Do not assume a detailed prose representation is more authoritative than executable project state merely because it is easier for an agent to read.

---

# 4. Unknown unknowns justify interrogation only when decisions are actually unresolved

Pocock's `grill-me` / `grilling` pattern is designed to expose unstated design decisions.

The current implementation:

- models the topic as a decision tree;
- asks only questions whose prerequisites are settled;
- separates facts the agent can discover from decisions the human must make;
- does not execute until shared understanding is confirmed;
- recommends a fresh conversation for the standalone `grill-me` entry point.

This is materially better than a generic "ask me 50 questions" prompt.

### HE value

The transferable mechanism is a **decision frontier**:

```text
known facts
   ↓
currently answerable decisions
   ↓
user decision / evidence acquisition
   ↓
newly exposed decisions
```

### Boundary

The official docs also describe failure modes:

- too much scope produces very long sessions;
- question volume creates decision exhaustion;
- some questions cannot be resolved by discussion and require a prototype;
- passive agreement produces confident-looking plans the human did not really decide.

### HE interpretation

> **Interrogate unresolved decision boundaries, not the whole task by default.**

This could strengthen HE's pre-implementation uncertainty handling without becoming universal ceremony.

---

# 5. Domain language is context compression plus alignment

Pocock recommends a project glossary / ubiquitous language so that:

- the human;
- code;
- database;
- UI;
- documentation;
- and agent

use the same terms for the same concepts.

His repo explicitly argues this can make the codebase easier for the agent to navigate and reduce verbose reasoning.

### HE interpretation

This is not primarily a documentation feature.

It is a **semantic compression and retrieval mechanism**.

A stable domain term can replace repeated explanatory prose if:

- the term is actually used by the project;
- its definition has one authoritative owner;
- code and artifacts use it consistently;
- it remains discoverable on demand.

### Risk

Creating a glossary for terms nobody uses merely adds another cache that can drift.

Admission should be driven by recurring ambiguity, not by template completeness.

---

# 6. Trust ladder = evidence-gated autonomy

The interview recommends beginning with observable, low-consequence tasks and increasing trust only as the user learns how the system behaves.

Pocock's framing includes:

- watch the agent closely at first;
- learn what the harness actually does;
- start with simple measurable tasks;
- add more agents, skills, and connected data only after confidence grows.

This maps cleanly to HE's adaptive-autonomy work.

### Transferable principle

> **Expand authority based on observed reliability at the next consequence level, not on model reputation or a one-time successful demo.**

A useful HE trust ladder is not "manual -> autonomous" as one jump.

It may look like:

```text
read / analyze
  ↓
draft
  ↓
local reversible mutation
  ↓
branch / isolated environment
  ↓
propose external effect
  ↓
approved external effect
  ↓
bounded autonomous effect
```

The exact steps are workload-specific.

---

# 7. Pocock's current skill architecture strongly supports composability

The inspected README explicitly contrasts the skill collection with frameworks that "own the process."

The current design emphasizes:

- small skills;
- composability;
- user-invoked orchestration skills;
- model-invoked leaf skills;
- issue-tracker integration;
- explicit boundaries between planning, research, prototyping, and implementation.

This reinforces HE's skepticism toward large monolithic agent frameworks.

### Important detail

The repo itself documents where its heavier workflow, `wayfinder`, fails:

- it is only justified for work too large for one session;
- comprehensive maps can fall into a waterfall trap;
- parallel decision sessions can duplicate/conflict because they do not share context;
- an agent can sometimes cross the "plan, don't implement" boundary;
- the heavier flow is slower and denser than a single-session grilling flow.

This self-documented failure evidence is especially useful to HE because it shows that composability does not remove orchestration risk.

---

# 8. The in-progress Chief of Staff is useful prior art and a warning

At the inspected commit, `skills/in-progress/chief-of-staff/SKILL.md` is explicitly under `in-progress`.

It proposes:

- one long-running goal;
- a chief-of-staff agent as DRI;
- all execution delegated to subagents;
- sparse communication through context pointers;
- recurring schedules where supported;
- a durable interlinked wiki;
- both tactical work and strategic environment improvement.

This is very close to the "agent office / software factory" direction HE is currently assessing.

## Useful patterns

- protect the coordinator's context;
- use pointers rather than copying worker transcripts;
- date code-related knowledge and cite commits;
- separate sources from derived memory;
- improve the environment for future tasks;
- give agents direct feedback surfaces such as logs/tests/browser access.

## Risks

### Long-running single-session coordinator

This conflicts with HE's recent "persist the job, not the inference" direction if interpreted literally.

### Wiki as another knowledge authority

A wiki can help historical memory, but can also become another source of stale derived truth if project state already has canonical owners.

### "Use every user message as an excuse" to improve the environment

That instruction creates a risk of unsolicited architecture/governance churn.

Environment improvement should be triggered by demonstrated friction or a bounded audit, not every interaction.

### "All work should be done in subagents"

That is a strong universal policy without evidence that every task benefits from delegation.

### HE disposition

Treat Chief of Staff as **experimental prior art**, not an architecture to copy.

---

# 9. Direct implication for the HE Specialist Bench

This source **strengthens the need to test the Specialist Bench and raises its burden of proof at the same time.**

It supports:

- specialist isolation where context pollution is real;
- subagents for research whose reading should stay out of the main context;
- sparse pointer-based fan-in;
- explicit human ownership of decisions;
- fresh sessions for decision/review work;
- composable skills instead of a monolithic office.

It challenges:

- permanent specialist roles;
- universal fan-out;
- an autonomous coordinator;
- long-running single-context "chief of staff";
- large process maps;
- spec-heavy workflows detached from executable code;
- strong model effort as the first response to poor outcomes.

### Specialist Bench refinement

The experiment should add one explicit question:

> **Could the observed benefit be achieved more cheaply by improving the environment, adding one deterministic feedback surface, or invoking a temporary skill in the main session instead of creating a specialist?**

This becomes a required alternative explanation when evaluating every specialist.

---

# 10. Candidate Trial #1

This source set is a good first candidate for the Specialist Bench because it contains:

- an interview with opinionated practitioner claims;
- an actively changing public implementation repo;
- explicit failure stories;
- source-vs-inference traps;
- a live multi-agent experiment (`chief-of-staff`);
- overlap with existing HE research;
- potential evidence both for and against the proposed Specialist Bench.

## Trial question

> **What, if anything, from Pocock's current agent-engineering approach should HE adopt as durable architecture, reusable skill patterns, or experimental hypotheses—and what should HE explicitly reject or keep provisional?**

## Why this is adversarial

A weak Evidence Miner can easily over-promote:

- "spec-driven development is bad";
- "medium effort is optimal";
- "four agents are equivalent to eight developers";
- "chief of staff is the future";
- "glossaries always save tokens";
- "the harness barely matters."

Those would exceed the evidence.

A strong Opposition Reviewer should force them back to bounded claims.

## Status

**Candidate Trial #1 source frozen. Experiment not yet executed.**

Reason: the Specialist Bench protocol requires fresh/isolated role passes. The current research session has already inspected and synthesized the source set, so using the same context as an "independent" treatment would contaminate the comparison.

---

# 11. Relationship to existing HE work

## STRONGLY REINFORCES

- Evidence Before Architecture
- smallest justified harness
- progressive disclosure
- modular capabilities
- source-backed research
- deterministic feedback/evaluation
- adaptive autonomy
- fresh-session review
- environment/runtime fitness
- context pointers rather than transcript fan-in
- single ownership of durable truth

## SHARPENS

- **Intent vs procedure**: durable guidance should emphasize what matters; detailed procedures should be triggered.
- **Environment before effort escalation**: fix observability/testability/runtime access before assuming the model needs more reasoning.
- **Decision frontier**: only interrogate decisions whose prerequisites are known.
- **Semantic compression**: shared project vocabulary can reduce repeated prose.
- **Specialist admission**: compare a specialist against the cheaper alternative of one better environment/skill/feedback loop.

## ASSESS

- a bounded "grill" capability for unresolved high-impact decisions;
- domain-language artifacts only where recurring ambiguity is demonstrated;
- pointer-based specialist fan-in;
- lower model effort after deterministic environment quality is established;
- source/research subagents as context isolation.

## PARK

- HE chief-of-staff agent;
- persistent agent wiki;
- recurring autonomous environment-improvement schedules;
- generalized decision-map framework.

## REJECT AS DEFAULT

- all work through subagents;
- long-lived coordinator context as the durability mechanism;
- permanent comprehensive specs as project truth;
- universal maximum model effort;
- multi-agent organization before simple alternatives are tested;
- environment changes on every user interaction.

---

# 12. Bottom line

Pocock's strongest contribution to HE is not a particular skill.

It is the operating philosophy:

> **Make intent explicit, make the environment legible and self-checking, expose unresolved decisions instead of guessing them, and use the smallest composable process that closes the actual gap.**

His own in-progress Chief of Staff work is useful because it also shows exactly where that philosophy can drift into orchestration machinery.

That tension makes this source valuable for HE.
