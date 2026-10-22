# Serop — Graphify A-B Test, Knowledge-Graph Economics & Tool Steering — 2026-09-25

## Purpose

Mine durable Harness Engineering lessons from Serop's video **“I Tested Graphify to Save Claude Code Tokens. Here’s What Happened.”**, then compare the observed experiment against the current Graphify implementation rather than treating one video run as a universal verdict.

Primary user source:

- Video: https://www.youtube.com/watch?v=uaVJfykIyzs
- Creator: Serop | AI Automation
- Transcript and screenshots supplied by the user on 2026-09-25

Current implementation evidence inspected:

- `Graphify-Labs/graphify`
- default branch at review time: `v8`
- pinned commit: `4000de15466588ec3ee32f9e10a587ca97d3b8a5`
- `README.md`
- `BENCHMARKS.md`
- `docs/how-it-works.md`
- install/hook implementation and generated skill references

Related HE research:

- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`
- `01 Research/Context Engineering.md.md`

This is research evidence only. It does **not** authorize installing Graphify into HE, PMB/MB, ACR or another repository.

---

# Executive finding

The most valuable HE lesson is not “knowledge graphs are bad” or “Graphify works.”

It is:

> **A derived context/index layer must earn its creation, maintenance, routing and context tax on the actual workload it is meant to improve.**

The video provides a useful negative/neutral result on a relatively small repository and ordinary feature task. The current Graphify repository provides separate evidence that graph-based structural retrieval may be more useful on large repositories and structural/multi-hop questions.

The combined evidence argues for **task-conditional retrieval infrastructure**, not universal always-on graph use.

---

# 1. The experiment is directionally strong because it tests the same real task under two harnesses

The video compares one engineering task using the same model under two setups:

```text
same task
   ├─ repo copy + Graphify graph/hook/rules → Claude agent
   └─ repo copy without Graphify            → Claude agent

measure:
- tokens
- time
- cost
- code-quality checks
```

The screenshots report approximately:

| Metric | With Graphify | Without Graphify | Difference |
|---|---:|---:|---:|
| Tokens | 67,222 | 67,204 | +0.03% |
| Time | 106.3 s | 102.8 s | +3% |
| Cost | $0.595 | $0.578 | +3% |
| Code-quality checks | 6/6 | 6/6 | same |

The reported graph-build cost was a lower-bound ~$1.42 / 355,773 input tokens in the tested setup.

This is substantially better evidence than judging the tool from its README because it tests an actual coding workflow and includes a control.

### Limitations

Do not overgeneralize the result:

- one repository (~200 files);
- one primary task class;
- small run count (screenshots show two Graphify runs and three control runs; transcript describes repeated runs generally);
- graph-enforcement behavior itself changed the harness;
- the repository/task supplied fairly direct search cues;
- Graphify has changed rapidly since the tested build/configuration.

**Disposition: STRONGLY REINFORCE workload-specific A/B evaluation; REJECT universal inference from one repo/task.**

---

# 2. The negative result is useful: derived context can add context instead of replacing it

The most important observed failure mode is that the graph often did **not** replace raw-source inspection.

The agent could use the graph to locate or connect code, but then still opened the files it needed to edit/understand. In that case the graph becomes an additional context/tool-call layer rather than a substitute for existing discovery.

The video reports:

- 4 of 11 graph queries were useless/unrelated;
- plain search often found the same code quickly;
- the graph helped with some dependency paths and dead-code observations;
- after navigation, the model still had to read source files;
- Graphify sometimes increased context pressure.

### HE implication

An index/retrieval accelerator should be evaluated against the **work it displaces**, not merely the information it can provide.

A capability that provides useful information but does not reduce or improve another expensive step may have negative net value.

Candidate question for any retrieval layer:

> **What expensive or unreliable behavior does this actually replace?**

**Disposition: STRONGLY REINFORCE.**

---

# 3. Build cost, maintenance cost and query cost belong in one ROI model

The video correctly includes the graph's up-front cost rather than measuring only steady-state task calls.

A general derived-index cost model is:

```text
Total cost =
  initial build
+ incremental maintenance
+ routing/tool-call overhead
+ returned-context overhead
+ operational complexity
```

Value may include:

```text
navigation time saved
+ context avoided
+ higher answer/review accuracy
+ cross-file defects prevented
+ reusable structural understanding across sessions
```

The graph pays for itself only if cumulative task value exceeds the lifecycle tax within the expected reuse horizon.

### Important version correction

The video's graph-build economics are **version/configuration specific**.

At current Graphify commit `4000de15466588ec3ee32f9e10a587ca97d3b8a5`, the project states that code graphs are built using deterministic tree-sitter AST extraction and a local embedder, with zero LLM credits for the code-graph build. The current implementation also caches unchanged files and only reprocesses uncached/changed inputs.

Therefore the video's 355,773-input-token build should not be treated as a current universal Graphify cost.

### HE implication

> **Economics are part of execution configuration and must be pinned to version, mode and workload.**

This mirrors HE's local-model finding that model name alone is not the execution envelope.

**Disposition: STRONGLY REINFORCE versioned empirical economics.**

---

# 4. Always-on tool steering can destroy the value of an otherwise useful capability

The video's most important harness result may be the hook behavior rather than the graph itself.

The tested setup forced/nudged the agent toward Graphify during search/file-read behavior. The creator concluded that the hook slowed runs that did not actually benefit from the graph and recommended turning it off for ordinary use.

Current Graphify has a more nuanced design:

- default behavior is a soft nudge toward graph queries;
- strict mode is optional;
- current strict mode blocks only the first raw source read in a session before reverting to the softer behavior.

This current design is consistent with the video's observed problem: forcing a specialized retrieval tool into every navigation step creates routing and context tax.

### HE implication

> **A useful capability does not imply an always-on mandate to use it.**

The harness should route structural retrieval when the task benefits from it rather than globally overriding ordinary search.

This directly reinforces:

- Progressive Disclosure;
- adaptive autonomy / loosen the execution path;
- minimum exposed capability surface;
- model-selected versus policy-enforced routing as an empirical question.

**Disposition: HIGH-VALUE REINFORCE.**

---

# 5. Structural graphs appear more valuable for relationship questions than ordinary file discovery

The independent video found Graphify useful for questions such as:

- what calls this function?;
- what is the dependency/path between components?;
- is this code apparently unused?;
- where is the surrounding structural impact of a change?

It found much less value when the task already named the feature/area and grep/file reads could navigate directly.

That suggests a useful task taxonomy:

## Likely stronger graph candidates

- multi-hop dependency tracing;
- unfamiliar large repository overview;
- change-impact analysis;
- call/import relationship questions;
- dead-code/caller discovery;
- cross-service or cross-package ownership paths.

## Likely weaker candidates

- known-file edits;
- direct keyword-search tasks;
- small/medium repos with obvious navigation;
- tasks where source files must be read anyway and graph output does not narrow the set materially.

### HE implication

Do not benchmark “Graphify” generically. Benchmark **task classes**.

**Disposition: ASSESS task-conditional routing.**

---

# 6. Current Graphify evidence suggests repo/task scale matters, but the evidence is not conclusive

Graphify's current `BENCHMARKS.md` reports a code-intelligence test on ERPNext (~1M LOC) where a fixed coding agent with one Graphify tool increased key-fact coverage from 70.8% to 82.0% over a graded set of six questions, at roughly 140K tokens/query.

This is useful but should be treated carefully:

- it is Graphify's own harness;
- n=6 is small;
- the task is code-intelligence QA, not ordinary implementation;
- it measures coverage, not general developer productivity.

Together with Serop's smaller-repo result, the evidence supports a hypothesis rather than a threshold:

> Graph/structural retrieval may add more value as repository scale, ambiguity and multi-hop structural reasoning increase.

Do not encode “500 files” or “1000 files” as a rule from this evidence.

**Disposition: ASSESS; no universal repo-size cutoff.**

---

# 7. Graph provenance is an HE-worthy mechanism

Current Graphify distinguishes edge provenance/confidence such as:

- `EXTRACTED` — explicit source/AST evidence;
- `INFERRED` — derived/resolved relationship with confidence;
- `AMBIGUOUS` — uncertain relationship requiring caution/manual review.

Edges retain source information and the graph validator enforces required structural fields.

This is a strong pattern regardless of whether HE ever uses Graphify.

### HE implication

Derived structural indexes should preserve:

```text
source pointer
relationship type
extraction/derivation class
confidence/ambiguity
index version/freshness
```

A graph should never become authoritative merely because it is easier to query than the source.

Candidate principle:

> **Derived structure accelerates navigation; source-owned artifacts remain truth.**

**Disposition: STRONGLY REINFORCE Evidence Before Architecture + provenance.**

---

# 8. Freshness is part of correctness

A code graph is a derived representation. If source changes without the graph changing, structural answers can become stale.

The video correctly identifies synchronization as a lifecycle cost. Current Graphify reduces this cost through deterministic extraction/caching and hook/update mechanisms, but the correctness requirement remains.

Any derived index needs:

- source revision identity;
- freshness/change detection;
- visible degraded/stale state;
- safe fallback to source inspection.

### Candidate principle

> **A stale derived index is not a cheaper truth source; it is an evidence-quality defect.**

**Disposition: STRONGLY REINFORCE.**

---

# 9. This connects directly to Harness Miner's future purpose

The emerging Harness Miner concept could eventually provide the evidence needed to decide whether structural tooling is worth deploying.

Instead of installing a graph because it sounds useful, historical run mining could ask:

- how often do sessions perform multi-hop dependency searches?;
- how many file-search/read calls precede the first useful edit?;
- which task types repeatedly struggle with call/dependency discovery?;
- how often does plain grep locate the target immediately?;
- do cross-file defects/reviews repeatedly miss impact relationships?

That evidence could justify a controlled Graphify-style experiment on the specific workload.

Do not build this integration yet. It is a strong example of why experience-derived harness evolution is useful.

**Disposition: ASSESS later through Harness Miner evidence.**

---

# 10. PMB/MB implication: do not cargo-cult a knowledge graph into memory

This source is about **code structure retrieval**, not proof that project memory should become a knowledge graph.

For MB/PMB:

- current durable memory ownership remains unchanged;
- retrieval should be evaluated by task success and actual required retrieval;
- a graph/index may be useful only if a demonstrated relationship-navigation problem exists;
- do not add a graph merely because graph navigation is visually or conceptually attractive.

**Disposition: no MB/PMB change.**

---

# 11. ACR implication: structural impact could be valuable, but only after a measured gap

Potential ACR-adjacent value:

- changed symbol → callers/dependents;
- cross-file impact candidates;
- apparently dead/unreferenced code;
- reviewer context narrowing.

However, ACR already has context-selection and evidence mechanisms. Adding a graph without showing current cross-file recall failures would be architecture-first behavior.

The correct path is:

```text
measured cross-file miss
   ↓
candidate structural retrieval
   ↓
controlled A/B on relevant fixtures/real PRs
   ↓
keep only if recall/evidence quality improves without unacceptable cost/noise
```

**Disposition: PARK for ACR until benchmark evidence shows the gap.**

---

# Research dispositions

## STRONGLY REINFORCE

- same-task A/B harness testing;
- derived context must displace/improve real work, not merely add information;
- include build + maintenance + routing + context cost in ROI;
- pin economics to exact implementation/version/configuration;
- keep derived indexes subordinate to source truth;
- retain provenance/confidence on derived relationships;
- make stale/degraded derived state visible;
- specialized tools should not become mandatory solely because they exist.

## ASSESS

- task-conditional structural retrieval;
- large/ambiguous repository workloads;
- change-impact/dependency analysis as a specialized use case;
- using Harness Miner data to identify whether graph navigation solves recurring real friction.

## PARK

- Graphify adoption in HE;
- Graphify adoption in MB/PMB;
- Graphify integration into ACR without a measured cross-file gap;
- repo-size thresholds such as 500/1000 files as policy.

## REJECT

- “knowledge graph saves tokens” as a universal claim;
- “the graph did not save tokens here, therefore graphs are useless”;
- always forcing a specialized retrieval tool on every search/read;
- treating the graph as source-of-truth;
- evaluating only steady-state query cost while ignoring build/maintenance overhead;
- judging architecture by visual graph appeal rather than workload outcomes.

---

# Candidate HE principles — research status only

> **A derived context/index layer must earn its creation, maintenance, routing and context tax on the workload it is meant to improve.**

> **A useful capability does not imply an always-on mandate to use it.**

> **Derived structure accelerates navigation; source-owned artifacts remain truth.**

> **A stale derived index is an evidence-quality defect, not a cheaper truth source.**

> **Benchmark retrieval mechanisms by task class and displaced work, not by feature existence.**

These remain research candidates pending cross-source comparison and local evidence.
