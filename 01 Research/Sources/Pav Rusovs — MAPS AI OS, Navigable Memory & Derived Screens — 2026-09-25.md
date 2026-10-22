# Pav Rusovs — MAPS AI OS, Navigable Memory & Derived Screens — 2026-09-25

## Purpose

Deep-mine Pav Rusovs' video **“Turn Claude Code Into Your AI Operating System (4 Layers)”** and its companion repository for mechanisms relevant to Harness Engineering.

The source presents a personal/business AI operating system called **MAPS**:

- Memory
- Agent
- Pulse
- Screen

HE should not copy that framework wholesale. The useful evidence is in the boundaries between those layers: source-owned truth, bounded navigation, least-privilege unattended execution, structured routine manifests, derived views, and explicit human acceptance.

## Source identity / preservation

- Video: https://www.youtube.com/watch?v=Zs3faMCDYNs
- Creator: Pav Rusovs / Automation Orbit
- User-provided transcript reviewed in full on 2026-09-25.
- Companion repository: https://github.com/pavrus117/ai-os-maps-guide
- Inspected revision: `066dc83a29bac80696872f3d4f8ce25f31cd439b`
- Repository state at review: one commit; root contains `README.md` and a PDF copy of the guide.
- No explicit LICENSE file was observed in the repository root during this pass. Treat the repository as research evidence; do not vendor/copy its prompts or implementation text into HE.

---

# Executive findings

The strongest HE value is not the “AI operating system” branding or the dashboard.

It is the architecture implied by the build order:

```text
authoritative files / project state
           ↓
rebuildable navigation map
           ↓
bounded agent + scheduled routines
           ↓
derived screen / dashboard
```

The source repeatedly enforces that the display should not become the owner of truth. The dashboard reads files produced elsewhere; if the dashboard disappears, the facts remain.

A second strong finding is that **navigation quality is a measurable harness property**. Rusovs tests a bounded signpost/map against ordinary workspace search and reports lower reading/token use and faster location of the target file on a small local experiment.

A third useful finding is the separation of unattended execution from authority. The always-on agent is deliberately constrained so it can draft/read/write within its allowed surface but cannot send/share/delete on its own.

---

# 1. Memory layer: navigation, not another fact store

The source uses one short top-level map/signpost file that contains pointers rather than duplicating business facts. Each area points to another signpost, which points to the real documents.

Two explicit design rules matter:

1. each fact has one authoritative home;
2. a deterministic check should detect stale/moved pointers.

### HE translation

This is a strong example of **Single Ownership + Progressive Disclosure**:

```text
small stable index
     ↓
area signpost
     ↓
authoritative artifact
```

The map is valuable only if it remains a **navigation layer**. It should not become a second knowledge base that copies the same facts.

The nightly/path checker is also important. A navigation map should not rely on model memory to stay correct; broken pointers are a deterministic integrity problem.

**Disposition: REINFORCE.**

---

# 2. “Two hops” is a useful measurement idea, not a universal law

Rusovs frames the memory quality question as whether Claude can find a fact within roughly two navigation steps from the root map.

That is useful because it converts “my files are organized” into an observable retrieval/navigation property.

However, HE should not promote **two hops** as a fixed architecture requirement. Different repositories/tasks may need:

- direct lookup;
- one-hop routing;
- several layers of project/domain hierarchy;
- semantic search;
- generated structural indexes.

### HE translation

The durable principle is:

> **Bound navigation depth and measure retrieval cost; do not optimize for an arbitrary universal hop count.**

Potential measurements:

- files opened before target found;
- retrieval/search calls;
- tokens read before authoritative source reached;
- elapsed time;
- wrong-source / stale-source rate;
- whether required source was actually retrieved.

This connects directly to HE's existing retrieval-observability work and Worktrunk's action-local cue findings.

**Disposition: ASSESS.**

---

# 3. The map experiment is directionally useful but weak evidence

The source reports a simple local A/B:

- same fact-finding question;
- fresh Claude Code sessions;
- one workspace with the map available versus normal workspace search.

Reported first-run result:

- without the map: about 62 seconds and 27.7k tokens;
- with the map: about 30 seconds and 16.8k tokens;
- approximately 40% lower reading/writing token use on that run.

A second repetition reportedly had a smaller reading gap while the mapped path remained about twice as fast.

### HE interpretation

This is useful **project-local evidence**, not proof that maps universally save 40%.

Limitations include:

- one question shape;
- tiny sample;
- one workspace;
- no holdout task class;
- possible caching/session/runtime variance;
- no evidence yet for maintenance/build cost of the map.

The important behavior is that the creator tried to measure the intervention rather than assuming the map was beneficial.

### HE experiment pattern

If PMB/work MB or another project considers a navigation map/index:

```text
representative task set
      ↓
baseline without new index
      ↓
with index/signposts
      ↓
measure location correctness + reading + time + quality
      ↓
include maintenance/staleness cost
```

This complements the recent Graphify finding: **a derived context layer must earn its lifecycle cost on the task classes that need it.**

**Disposition: REINFORCE measurement discipline; ASSESS the mechanism.**

---

# 4. Generated maps should be rebuildable materialized views

The dashboard “brain” is generated from the real map/notes and rendered as JSON/graph views. The displayed graph is therefore a **derived representation**, not the fact store itself.

### HE translation

A generated map/index should ideally have these properties:

- source artifacts remain authoritative;
- generation is repeatable;
- links retain source/path provenance;
- stale/broken references are visible;
- deleting the generated artifact loses no canonical information;
- deterministic extraction is preferred where structure already exists.

This is a better default than allowing an inferred graph to silently become project truth.

**Disposition: REINFORCE.**

---

# 5. Cross-machine agent: least privilege matters more than “always on”

The source runs the same official coding agent on a laptop and an always-on VPS. The important safety mechanisms are:

- private-network access rather than exposing the service publicly;
- a low-privilege execution account;
- a restricted agent settings profile;
- unattended agent cannot send/share/delete;
- consequential external actions stay human-owned.

### HE translation

The strongest pattern is:

> **Unattended availability should not imply unattended authority.**

A useful background agent can prepare drafts, perform bounded checks and update safe local artifacts without receiving permission to publish, send, delete or otherwise create irreversible external effects.

This aligns with Orca's effect-verification/authority work and the recent monotonic-policy findings.

**Disposition: REINFORCE.**

---

# 6. Cross-machine sync is under-specified and should not be copied literally

The guide proposes keeping the same folder on two machines and synchronizing via Git on a frequent schedule.

That can work in a single-writer/low-contention environment, but a naive bi-directional `pull → commit → push` loop can create:

- races;
- merge conflicts;
- surprise commits;
- stale pushes;
- overlapping writers;
- poor attribution;
- accidental synchronization of machine-specific state.

The guide also asks for absolute-path links in the map while promoting the same folder across two machines. Absolute paths are inherently brittle unless the directory layout is identical or a path-mapping layer exists.

### HE translation

For cross-machine state, define explicitly:

- canonical repository root / logical paths;
- writer ownership;
- branch/worktree ownership;
- conflict behavior;
- sync failure state;
- machine-local exclusions;
- whether a generated index stores logical IDs or machine paths.

Prefer repository-relative/logical identifiers for portable navigation artifacts.

**Disposition: REJECT naive two-way sync and absolute-path dependence as general HE patterns.**

---

# 7. Pulse: routine manifests are stronger than “scheduled prompts”

Rusovs describes a routine registry containing structured fields such as:

- name;
- schedule;
- execution machine;
- prompt/Skill;
- model / effort;
- allowed tools;
- turn limit;
- timeout;
- output destination.

Runs produce durable records, and execution caps prevent one failing routine from consuming the entire allowance.

### HE translation

This strongly reinforces a conclusion already visible in Orca:

> **Scheduled agent work should be a structured run contract, not merely cron + prose prompt.**

A useful routine contract should be able to answer:

```text
what runs?
when?
where?
with what authority?
with what budget?
what counts as completion?
where is the result/evidence?
what happens on failure?
```

The specific five-minute polling schedule is implementation detail, not a principle.

**Disposition: REINFORCE concept; PARK scheduler implementation in HE.**

---

# 8. Run logs are operational evidence, but telemetry fields need source ownership

The guide proposes a per-run record including time, routine, model, turns, cost and status.

That is useful, but HE should distinguish which fields are actually trustworthy:

- timestamps/run identity can be deterministic;
- model may be host-reported;
- turns can be counted by the runner;
- cost may be unavailable or only estimated under subscription products;
- “status” needs precise semantics: process exit, task execution result and human acceptance are different facts.

### HE translation

A routine log should preserve:

- source of each field;
- unknown/unavailable values;
- execution result separately from accepted task completion;
- provider usage/cost provenance.

**REINFORCE:** telemetry is evidence only when its semantics are explicit.

---

# 9. Screen: “show, don't store” is an excellent Dashboard boundary

The source's strongest Screen rule is that every dashboard panel is derived from files owned elsewhere. The dashboard itself should not become the only place where facts exist.

It also proposes panel-level timestamps and showing stale data as stale rather than silently hiding it.

### HE translation

This directly reinforces the parked Dashboard/Cockpit design:

> **Presentation consumes authority; presentation does not become authority.**

If the UI disappears, no project truth should be lost.

Every panel should identify:

- source;
- observed/generated time;
- freshness/staleness;
- availability/degraded state.

**Disposition: REINFORCE strongly.**

---

# 10. A dashboard with buttons is no longer purely read-only — separate command intent

The guide's advanced dashboard can trigger a routine by writing a small request file into a queue. The actual runner later consumes that request under the same routine policy/budget.

This is a good separation, but it changes the architecture:

```text
read model / presentation
        +
command-intent queue
        ↓
authorized runner
```

The screen still does not own project truth, but it now participates in the **command plane**.

### HE translation

If the parked Cockpit ever gains actions:

- keep read models separate from command intents;
- actions should emit bounded requests, not directly mutate source systems;
- the owning runtime re-validates current policy before execution;
- request status/effect evidence returns separately.

This is consistent with monotonic policy and tool-success-vs-effect-verification work.

**Disposition: ASSESS for a future post-MVP Cockpit; keep initial Cockpit read-only.**

---

# 11. Human acceptance and machine execution status should be separate

One MAPS rule says Claude never marks the task done; the human does.

The useful concept is not that machines can never mark anything complete. Scheduled systems need machine-readable run completion.

The better split is:

```text
run succeeded / failed       ← runtime-owned
artifact/checks produced     ← source/test-owned
accepted as complete         ← human/project acceptance authority
```

### HE translation

Do not overload `done` to mean all three.

This connects to Paperclip's structured lifecycle/ownership work and HE's producer-versus-acceptance boundary.

**Disposition: REINFORCE after refinement.**

---

# 12. Seven guide rules: HE disposition

The guide lists seven cross-layer rules. Their HE value is uneven.

### Human owns acceptance

Useful when interpreted as acceptance authority rather than denying machine run status.

**REINFORCE with refinement.**

### Append stores; never rewrite whole

Good for logs/event streams, not universal current-state storage. Mutable snapshots/configuration often should be replaced atomically.

**REJECT as a universal rule; REINFORCE append-only for event history where appropriate.**

### Deterministic code before model call

Strong when deterministic logic can answer the question/gate cheaply and correctly. Not a mandatory ordering if semantic judgment is intrinsically required.

**REINFORCE as default preference, not absolute law.**

### Show, don't store

Strong Dashboard/observability boundary.

**REINFORCE strongly.**

### Update navigation when a file moves

Strong ownership/integrity principle; deterministic checker is better than relying on memory.

**REINFORCE.**

### Nothing gets deleted; archive everything

Unsafe as a universal policy. Retention, privacy, secrets, legal requirements and storage hygiene can require deletion.

**REJECT as universal.**

### Plans/specs live in the repo, not chat

Strong durable-state principle when the repository is the correct owner.

**REINFORCE.**

---

# 13. Implications for current HE projects

## PMB / work MB

MAPS reinforces several existing directions rather than requiring a redesign:

- small startup/navigation surface;
- one owner for each durable fact;
- retrieval by pointer rather than copying everything into startup context;
- deterministic stale-pointer checks;
- durable project state outranks chat history.

Potential experiment, only if current pilot evidence shows a navigation problem:

> Compare representative required-retrieval tasks with current pointers versus a bounded generated navigation map, measuring source reached, search calls, reading volume and elapsed time.

Do **not** add a second fact store merely to mimic MAPS.

## Dashboard / Cockpit

MAPS strongly reinforces the existing parked architecture:

- build truth/data contracts first;
- UI last;
- derived/read-only views;
- explicit freshness;
- no dashboard-owned project facts;
- if actions are later added, use a separate command-intent path.

Its own statement that the dashboard is a minority of the value supports **not starting with UI**.

## Harness Miner

Routine/run records and navigation failures would be useful observation sources for Harness Miner, but Harness Miner should consume them rather than own them.

Potential mined questions:

- how often does the agent reach the right source without user path hints?
- where do stale pointers occur?
- which routines fail/retry repeatedly?
- which scheduled prompts should become deterministic code?

## Orca

MAPS and Orca are complementary references:

- Orca is stronger as a general multi-provider operator surface and session/runtime control plane;
- MAPS is stronger as an example of source-owned personal/work truth + background routines + derived dashboard.

Before creating a custom Cockpit, HE should evaluate what Orca already solves and identify which PMB/ACR/navigation-specific needs remain.

---

# Overall disposition

- **REINFORCE:** source-owned truth + rebuildable navigation indexes.
- **REINFORCE:** one authoritative home for durable facts.
- **REINFORCE:** deterministic integrity checks for navigation maps.
- **REINFORCE:** unattended availability with deliberately reduced authority.
- **REINFORCE:** structured routine manifests, run records and execution caps.
- **REINFORCE:** dashboard as derived projection; explicit freshness/staleness.
- **REINFORCE:** plans/specs should live in their durable owner rather than chat.
- **ASSESS:** bounded navigation-depth metrics and generated maps for projects with demonstrated retrieval friction.
- **ASSESS:** command-intent queue if a future Cockpit gains actions.
- **PARK:** always-on VPS, phone bot and custom personal AI OS as HE implementation.
- **PARK:** the six graph visualizations; useful product/UI ideas, not core HE architecture.
- **REJECT:** universal two-hop requirement.
- **REJECT:** universal append-only state.
- **REJECT:** universal never-delete/archive-everything rule.
- **REJECT:** naive bi-directional Git sync as a general multi-machine state strategy.
- **REJECT:** absolute machine paths as the portable identity model for shared maps.

## Candidate HE principles from MAPS

1. **Navigation is a derived capability; authoritative content remains in its owning source.**
2. **A navigation/index layer should be rebuildable and deterministically checkable.**
3. **Measure the cost of reaching authoritative context, not merely whether context exists.**
4. **Unattended availability should not imply unattended authority.**
5. **Scheduled agent work should have a structured execution contract and bounded budget.**
6. **Presentation should expose source and freshness without becoming the source of truth.**
7. **If a display gains actions, separate command intent from read-state projection.**
8. **Execution completion and acceptance completion are different states.**
