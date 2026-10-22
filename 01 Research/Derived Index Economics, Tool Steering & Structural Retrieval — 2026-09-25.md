# Derived Index Economics, Tool Steering & Structural Retrieval — 2026-09-25

## Purpose

Synthesize the Harness Engineering implications of using derived structural indexes/knowledge graphs to help coding agents navigate repositories, with emphasis on lifecycle economics, task-conditional routing, provenance and avoiding mandatory tool use that adds more context than it removes.

Primary evidence:

- `01 Research/Sources/Serop — Graphify A-B Test, Knowledge-Graph Economics & Tool Steering — 2026-09-25.md`
- `Graphify-Labs/graphify` at commit `4000de15466588ec3ee32f9e10a587ca97d3b8a5`

Related HE research:

- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Context Engineering.md.md`

This is research synthesis only. It does not authorize adding a knowledge graph, retrieval index, hooks or Graphify to any repository.

---

# Executive synthesis

Derived context structures are often sold as if indexing is automatically an optimization.

HE should use a stricter test:

```text
source corpus
    ↓
derived structure/index
    ↓
extra build + freshness + routing + query/context cost
    ↓
which existing work is displaced or improved?
    ↓
measured task outcome
```

The central principle candidate is:

> **A derived context layer earns its place only when its lifecycle cost is outweighed by measured improvement on the task classes that actually need it.**

This applies beyond knowledge graphs to:

- semantic indexes;
- embeddings/vector stores;
- generated architecture maps;
- repository summaries;
- call graphs;
- dependency catalogs;
- automatically generated context packs.

---

# 1. Context infrastructure has lifecycle economics

A derived context layer is not free simply because later queries are cheap.

Its full cost envelope includes:

```text
C_total =
    C_build
  + C_incremental_refresh
  + C_routing
  + C_query
  + C_returned_context
  + C_operational_complexity
  + C_staleness/failure risk
```

Its value envelope can include:

```text
V_total =
    fewer search/read calls
  + lower model context usage
  + lower wall time
  + better structural recall
  + fewer missed dependencies
  + reusable knowledge across sessions
  + improved verification/review coverage
```

A local A/B test should ask whether `V_total > C_total` over the expected reuse period.

### Candidate principle

> **Amortization belongs in context architecture.**

A large up-front index can still be valuable if reused enough; a zero-cost build can still be harmful if every task pays routing/context overhead.

---

# 2. Measure displaced work, not just retrieved information

A retrieval tool can answer useful questions and still fail as an optimization.

If the agent:

1. queries the index;
2. receives a structural answer;
3. then performs the same grep/read sequence it would have performed anyway;

then the index may only have added steps/context.

The correct evaluation question is:

> **What work disappeared, became cheaper, became more accurate, or became newly possible because the derived layer existed?**

Potential displaced work:

- repeated repo-wide grep;
- opening many candidate files;
- reconstructing dependency paths manually;
- redundant architecture exploration;
- repeated cross-file caller discovery.

If no expensive behavior is displaced and quality does not improve, the layer has not earned its cost.

### Candidate principle

> **Optimization claims should identify the work they replace.**

---

# 3. Specialized retrieval should be routed by task type

The useful distinction is not:

```text
Graph vs no graph
```

but:

```text
task characteristics
       ↓
retrieval mechanism
```

Possible routing dimensions:

- repository scale;
- task ambiguity;
- known vs unknown target location;
- number of relationship hops;
- cross-package/service impact;
- whether ordinary lexical search already yields a high-quality target set;
- consequence of missing a dependency.

### Example

```text
Known symbol/file + focused edit
    → ordinary search/read likely sufficient

Unknown architecture + multi-hop dependency question
    → structural graph/index candidate

Change-impact/security-critical dependency review
    → structural retrieval candidate + source verification
```

### Candidate principle

> **Route specialized context tools where their information advantage matches the task, not on every task.**

---

# 4. Tool mandates are themselves harness interventions

A hook/rule that forces a model to use a tool is not a neutral implementation detail.

It changes:

- tool ordering;
- model autonomy;
- context contents;
- latency;
- error surface;
- whether the model can use a simpler path.

Therefore:

```text
capability value
!=
mandatory capability value
```

A tool may be highly useful when invoked selectively and harmful when globally enforced.

This extends the adaptive-autonomy finding:

> **Loosen the execution path, not the acceptance boundary.**

The harness should reserve mandatory routing for cases where evidence shows the behavior is load-bearing or necessary for safety/correctness.

### Candidate principle

> **A useful capability does not imply an always-on mandate to use it.**

---

# 5. Derived structure must retain source authority and provenance

A graph/index is a navigation/analysis artifact.

It should retain enough information to distinguish:

```text
EXTRACTED / directly observed relationship
INFERRED / derived relationship
AMBIGUOUS / unresolved relationship
```

and point back to source locations where possible.

The source remains authoritative for current behavior.

### Candidate principle

> **Derived structure accelerates navigation; source-owned artifacts remain truth.**

This aligns with HE's broader evidence-classification work and should apply to generated summaries and semantic indexes as well as graphs.

---

# 6. Freshness is part of correctness, not maintenance trivia

A derived index can be internally valid yet externally stale.

Useful freshness controls include:

- source revision/commit identity;
- changed-file detection;
- cache invalidation;
- index-version identity;
- visible stale/degraded state;
- safe fallback to direct source inspection.

Do not allow:

```text
index did not update
       ↓
missing relationship
       ↓
agent interprets absence as truth
```

### Candidate principle

> **A stale derived index is an evidence-quality defect.**

---

# 7. Benchmarks need task-class and version identity

A retrieval/index benchmark should record:

- exact tool version/commit;
- extraction mode;
- hook/routing policy;
- repository size/shape;
- model/effort;
- task class;
- number of runs;
- build/refresh cost;
- task-time/query cost;
- context/token usage;
- quality/coverage outcome;
- whether raw source inspection was still required.

This prevents false conclusions such as:

- “Graphify costs hundreds of thousands of tokens to build” when a newer code path uses deterministic extraction;
- “graphs save tokens” based on a large-repo structural QA workload;
- “graphs are useless” based on a small repo with a directly named feature location.

### Candidate principle

> **Benchmark retrieval systems as execution configurations, not product names.**

---

# 8. Negative results are architecture evidence

A neutral/negative A/B result is useful.

It can show that:

- an architecture solves a problem the local workload does not have;
- the routing policy is wrong even if the capability is useful;
- startup/maintenance cost overwhelms steady-state benefit;
- native search has improved enough that old scaffolding no longer pays;
- the task is too easy to expose the proposed advantage.

This is exactly the kind of evidence HE should preserve rather than selecting only successful tool demonstrations.

### Candidate principle

> **A harness experiment is valuable when it eliminates unnecessary architecture.**

---

# 9. Connection to Experience-Derived Harness Evolution

A future Harness Miner-style system could identify whether the environment exhibits the workload characteristics that justify structural indexing.

For example:

```text
historical sessions
      ↓
measure:
- repo-search depth
- file-open count before useful edit
- repeated caller/dependency questions
- user corrections caused by missed cross-file impact
- structural-review misses
      ↓
select candidate task class
      ↓
run controlled with/without structural index
```

This is a concrete example of:

> **History proposes harness changes; experiments earn them.**

Do not build automatic Graphify recommendation/routing yet. The conceptual connection is sufficient.

---

# 10. Cross-project implications

## Harness Engineering

Strong additions:

- lifecycle economics for derived context;
- displaced-work measurement;
- task-conditional retrieval routing;
- mandatory-tool routing as a measurable harness intervention;
- provenance/freshness requirements for derived indexes;
- versioned retrieval benchmarks;
- negative results as architecture-pruning evidence.

## Work MB / PMB

No graph implementation implied.

If memory/retrieval later exhibits a demonstrated relationship-query problem, evaluate the smallest retrieval/index mechanism against the real failure. Do not introduce graph architecture merely because it works for code relationships.

## ACR

Structural dependency retrieval may later help cross-file review/impact analysis, but only after current benchmarks demonstrate missed relationships that existing context selection does not solve.

## Harness Miner

A future miner can provide workload evidence used to select retrieval experiments, but should not automatically install/index/reroute tools.

---

# Research disposition

## STRONGLY REINFORCE

- evaluate context infrastructure with full lifecycle economics;
- measure displaced work;
- task-conditional tool routing;
- treat forced tool use as an intervention;
- retain provenance and source authority;
- freshness/degradation is evidence quality;
- version/configuration identity belongs in benchmark results;
- negative results can justify removing/avoiding architecture.

## ASSESS

- specialized structural retrieval for multi-hop/change-impact tasks;
- local workload classification before choosing retrieval architecture;
- Harness Miner metrics that reveal repeated structural-navigation friction.

## PARK

- knowledge graph as a standard HE component;
- repo-size thresholds;
- automatic graph routing;
- ACR graph integration without measured need;
- memory graph conversion for PMB/MB.

## REJECT

- derived index by default;
- mandatory specialized retrieval on ordinary tasks without evidence;
- product-level token-savings claims without version/task data;
- graph/index as authoritative project truth;
- benchmark results that omit build/maintenance cost;
- keeping infrastructure because it is visually or conceptually impressive.

---

# Candidate HE principles — research status only

> **A derived context layer earns its place only when its lifecycle cost is outweighed by measured improvement on the task classes that actually need it.**

> **Optimization claims should identify the work they replace.**

> **Route specialized context tools where their information advantage matches the task, not on every task.**

> **A useful capability does not imply an always-on mandate to use it.**

> **Derived structure accelerates navigation; source-owned artifacts remain truth.**

> **A stale derived index is an evidence-quality defect.**

> **Benchmark retrieval systems as execution configurations, not product names.**

> **A harness experiment is valuable when it eliminates unnecessary architecture.**

These remain research candidates until supported by additional sources and/or local workload evidence.
