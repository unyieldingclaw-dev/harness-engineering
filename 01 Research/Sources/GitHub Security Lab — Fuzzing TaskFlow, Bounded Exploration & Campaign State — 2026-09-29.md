# GitHub Security Lab — Fuzzing TaskFlow, Bounded Exploration & Campaign State — 2026-09-29

## Why this source was reviewed

Follow-up deep dive from the TestGuild / GitHub Security Lab fuzzing item.

The first pass established the obvious high-level pattern:

- model decides what to try;
- deterministic tools execute AFL/clang/coverage work;
- coverage provides an external progress signal;
- geometric budgets bound exploration;
- a plateau rule stops the loop;
- arbitrary model-selected build/shell authority requires an isolation boundary.

This pass inspects the actual repositories and implementation details to determine what Harness Engineering should retain beyond those headline observations.

Primary repositories inspected:

- https://github.com/GitHubSecurityLab/seclab-taskflows-fuzzing
- https://github.com/GitHubSecurityLab/seclab-taskflow-agent
- https://github.com/GitHubSecurityLab/seclab-taskflows

Related first-party article:

- GitHub Security Lab — AI-powered fuzzing with the GitHub Security Lab Taskflow Agent

Repository snapshots inspected on 2026-09-29:

- `GitHubSecurityLab/seclab-taskflows-fuzzing` main tree `669a16a4fff8bd24886337e29f6348f6dc1f1415`
- `GitHubSecurityLab/seclab-taskflow-agent` current main code around `cdcb6fcd65c31f0fe934bed3248034b1c408eedf`
- `GitHubSecurityLab/seclab-taskflows` main tree `3c236acffdc82afb2d6e79266f600fa9bc8f32ee`

This note is research only. It does **not** authorize HE to adopt TaskFlow, AFL++, an MCP orchestration framework, autonomous fuzzing, a new control plane, or a general-purpose campaign database.

---

# Executive finding

The strongest reusable pattern is not fuzzing itself.

It is a concrete implementation of **bounded autonomous experimentation with externalized state and evidence**:

```text
externally defined campaign/stage
        ↓
model chooses next hypothesis / intervention
        ↓
deterministic tool performs execution
        ↓
objective measurement records result
        ↓
durable state persists outside model context
        ↓
external policy decides continue / spend more / stop
        ↓
separate triage attributes the observed failure
```

The implementation contains several refinements that are directly useful to HE:

1. mandatory lifecycle/bookkeeping operations happen before optional model analysis;
2. durable campaign state is not conversational state;
3. candidate competition uses a fixed budget and deterministic evaluator;
4. stage criticality is explicit — some failures stop the run while later evidence/reporting stages are best-effort;
5. crash discovery and crash meaning are separate stages;
6. failure ownership is classified explicitly, including `harness_bug` and `needs_investigation`;
7. reproducers and evidence packages survive the model turn that created them;
8. known failures are replayed against later builds rather than merely marked resolved;
9. task-level checkpoints make orchestration resumable without replaying the entire conversation;
10. current executed configuration can contradict stale comments/documentation, reinforcing that executed state outranks descriptive prose.

The project is also a useful warning: autonomy does not become safe merely because the workflow is carefully structured. The fuzzing taskflow deliberately exposes arbitrary host shell/build capability and therefore explicitly requires a disposable environment.

---

# 1. The orchestration split is unusually clean

The fuzzing repository documents three operational layers:

```text
shell driver
    → stage ordering / outer loop

TaskFlow YAML
    → model-owned decisions per stage

MCP servers + SQLite
    → execution primitives + durable campaign state
```

The repository states the intended ownership explicitly:

> LLM agents own decisions, MCP tools own execution.

The model decides:

- what target is worth fuzzing;
- how to author or modify a harness;
- which coverage gap is worth pursuing;
- whether a seed, harness change or dictionary enrichment is the best next move;
- how to classify and explain a crash.

The deterministic layer owns operations such as:

- compilation;
- AFL execution;
- coverage replay;
- LCOV parsing;
- corpus minimization;
- crash minimization/replay;
- persistence/upsert;
- subprocess timeout;
- artifact packaging.

### HE implication

This is a strong instance of:

> **Use model judgment for the uncertain choice; use ordinary code for execution mechanics and measurable facts.**

It does not imply that every HE workflow needs MCP or a workflow engine.

**Disposition: STRONGLY REINFORCE.**

---

# 2. Durable campaign state is deliberately outside model context

The fuzzing pipeline persists targets, harnesses, runs, coverage, gaps, crashes, verdicts, call graphs, suggestions and iteration notes in SQLite.

Stages communicate through that persistent store rather than through an in-memory conversational handoff.

The codebase also prefers explicit tool arguments and says MCP tools should not depend on global mutable state.

The parent TaskFlow Agent separately checkpoints task-level progress and a `ResultStore` snapshot to disk after each completed task. Resume skips already-completed tasks and reuses the prior model configuration unless explicitly overridden.

The checkpoint stores useful execution facts including:

- completed/skipped task index;
- task success;
- models used;
- duration;
- token/cache usage;
- named outputs;
- taskflow globals;
- model-config override;
- error/final status.

### HE implication

There are two different persistence scopes here:

```text
workflow checkpoint
    → enough state to resume orchestration

campaign/domain state
    → durable facts produced by the work itself
```

Neither needs the entire conversation transcript.

This is strong evidence for the existing HE direction:

> **Persist the smallest source-owned state required for continuation; do not use conversation history as the operational database.**

It also reinforces a useful ownership distinction: PMB should preserve project/continuation facts it actually owns, while domain runtimes should own their own machine state.

**Disposition: STRONGLY REINFORCE Session Rollover / source-owned state.**

---

# 3. Mandatory bookkeeping happens before optional reasoning

`fuzz_iteration.yaml` makes six operations mandatory before the model is allowed to spend remaining steps on coverage analysis:

1. open a fuzz-run record;
2. execute AFL under the assigned budget;
3. close the fuzz-run record even when AFL reports failure;
4. execute coverage replay;
5. persist coverage;
6. fold the new queue into the persistent corpus.

Only after those lifecycle facts are recorded does the prompt permit optional reasoning about seeds, harness changes or dictionary enrichment.

The iteration also records a concise durable note at the end.

### HE implication

This is a useful implementation pattern when a long-running probabilistic worker owns judgment but not lifecycle truth:

> **Record lifecycle/evidence facts deterministically before spending the remaining budget on interpretation or optimization.**

This is not a requirement to persist every model turn. It is appropriate when losing the bookkeeping would make later evidence ambiguous or make resume unsafe.

Potential HE analogues include:

- record candidate identity before review;
- record execution result before asking the model to explain it;
- persist a stop reason before handing off;
- preserve verifier output before attempting repair.

**Disposition: NEW IMPLEMENTATION EVIDENCE / REINFORCE.**

---

# 4. Resource escalation is externally scheduled, not model-invented

The outer fuzzing driver owns the budget schedule:

```text
30s → 60s → 120s → 240s → 480s → 960s
```

The model receives the budget for each iteration; it does not decide how much wall-clock authority to grant itself.

A deterministic helper reads persisted coverage and stops early when eligible harnesses have two consecutive gains below the configured absolute line-coverage threshold.

This creates a useful separation:

```text
model chooses how to use budget
        ≠
model owns budget
```

### HE implication

> **When an objective progress metric exists, resource escalation and stop policy can remain external while the worker retains freedom inside each budget.**

Do not generalize coverage plateau into a universal HE stopping metric. Many knowledge-work tasks do not have an objective marginal-progress signal.

**Disposition: STRONGLY REINFORCE Bounded Execution.**

---

# 5. Candidate generation and candidate selection are separate operations

When multiple fuzz harnesses are requested, the pipeline runs a qualifier stage before the main campaign.

Each candidate receives the same configured short wall-clock budget, produces measured line coverage, and a later deterministic-selection task chooses the highest line percentage per target. Ties prefer the lower harness ID.

Losers are marked `superseded`, and the main loop ignores them.

Importantly, the qualifier prompt explicitly says **do not improve the candidate during qualification**. That keeps the comparison closer to a controlled bakeoff.

### HE implication

This is a compact example of a good candidate/evaluator boundary:

```text
produce alternatives
        ↓
freeze them for comparison
        ↓
same bounded evaluation
        ↓
predeclared metric
        ↓
deterministic tie break
        ↓
selected candidate continues
```

The line-coverage metric is domain-specific and incomplete as a quality measure, but the **evaluation shape** is reusable.

This reinforces HE's existing bakeoff and evaluator-independence work:

> **When alternatives are being compared, freeze the candidates during the comparison and keep selection semantics outside the candidates.**

**Disposition: STRONGLY REINFORCE Artifact-Gated / Autoresearch / Ponytail control-isolation work.**

---

# 6. Discovery, reproduction, classification and remediation are separate stages

The crash workflow deliberately separates several jobs that are often collapsed into one agent response.

## Discovery

AFL produces a crash candidate.

## Reduction + reproduction

The triage prompt first minimizes the input, then replays it under ASan/UBSan to obtain a cleaner trace and stack hash.

If the minimized case no longer reproduces, it is recorded as non-reproducible instead of silently disappearing.

## Bug-class triage

The triage stage records a concise bug class, minimal triggering shape and confidence. It is explicitly told **not to write a patch yet**.

## Ownership / security verdict

A later stage reads the harness, library source and public call chain, then chooses among verdicts including:

- `vulnerability`;
- `library_hardening`;
- `harness_bug`;
- `non_reproducible`;
- `oom`;
- `timeout`;
- `assertion_failure`;
- `duplicate`;
- `needs_investigation`.

Only after that does it draft a minimal fix sketch.

## Remediation authority

The generated patch is explicitly marked **review required**. It is evidence/advice, not an autonomous code change.

### HE implication

This is a concrete version of the failure-learning pattern HE has been developing:

```text
observe failure
    ↓
make it reproducible / minimize it
    ↓
classify failure mechanism
    ↓
identify owning component / boundary
    ↓
propose smallest correction
    ↓
verify separately
```

The important part is that `harness_bug` is a first-class outcome. The discovery tool is allowed to be wrong.

### Candidate principle

> **A failure found by the harness is not automatically a failure of the system under test; attribution is its own evidence problem.**

**Disposition: STRONGLY MINE as implementation evidence for owning-component diagnosis.**

---

# 7. Reproducer artifacts are a stronger handoff than prose diagnosis

The vulnerability-report stage packages a reproducer bundle containing the minimized input, harness source, build command/context and sanitizer evidence.

The report also includes:

- stack trace;
- root cause with file/line evidence;
- public-API reachability;
- exploitability reasoning;
- suggested patch;
- regression-test sketch;
- confidence;
- unresolved notes.

### HE implication

> **When another actor must verify or repair a failure, preserve the smallest executable/replayable evidence package that demonstrates it.**

This is stronger than passing only a prose summary between agents or sessions.

Potential ACR analogue: where practical, a review finding is stronger when it can point to a reproducible failing test/command or minimal evidence fixture rather than only a natural-language objection.

**Disposition: STRONGLY REINFORCE independent executable evidence.**

---

# 8. “Fixed” status demonstrates both a good pattern and an attribution hazard

The pipeline replays previously known minimized crash inputs against the current harness/binary. If the crash no longer reproduces, it marks the verdict `fixed`.

This is much stronger than closing a finding because code changed.

However, the taskflow's own prompt acknowledges two possible explanations:

> the upstream code has been fixed **or the harness behaviour changed**.

The implementation still writes a note that says “likely fixed upstream.”

That means replay establishes:

```text
old reproducer no longer produces the old observed effect
```

It does **not by itself** establish:

```text
specific owning defect was corrected for the intended reason
```

### HE implication

This is a valuable evidence-boundary example:

> **Effect disappearance proves closure only when the execution/evaluator envelope needed to attribute the effect is still equivalent, or the changed envelope is explicitly accounted for.**

This connects directly to Unlazy gate-bound evidence, Ponytail treatment isolation and HE's execution-provenance work.

**Disposition: NEW CAUTION / STRONGLY MINE.**

---

# 9. Partial failure policy is explicit, but deserves scrutiny

The outer driver treats the central campaign stages as required, while later stages such as crash triage, fixed-crash confirmation, call-graph analysis and per-crash reporting are invoked with shell `|| true` in the driver.

Inside the YAML, fan-out work such as one harness iteration or one crash report frequently uses `must_complete: false`, while setup/listing tasks use `must_complete: true`.

The parent TaskFlow Agent also retries failed tasks up to a fixed limit before checkpointing failure/resume state.

### HE implication

This demonstrates an important orchestration requirement:

> **Failure tolerance is stage-specific; “continue on failure” must be tied to what evidence/output becomes incomplete.**

Continuing is appropriate when one branch can fail without invalidating every other branch. It is dangerous when a later report appears complete despite skipped evidence.

HE should therefore preserve both:

- the task result/status;
- the incompleteness semantics caused by tolerated failure.

The TaskFlow run manifest helps here by recording per-task `ok / failed / skipped`, model identity, duration and usage.

**Disposition: STRONGLY REINFORCE explicit stage criticality and manifestable partial success.**

---

# 10. Checkpoint/resume is task-granular and source-owned

The TaskFlow Agent saves a checkpoint after each task. A resumed run skips completed tasks and restores captured result state.

This is a substantially stronger continuation mechanism than “summarize what happened and start again.”

It works because the runner owns a deterministic lifecycle and can identify the next task.

### HE boundary

Do not generalize this into “PMB should checkpoint every task.”

PMB and an orchestrator have different ownership:

- orchestration runtime may own exact task index/result state;
- PMB may own only the durable project/handoff information needed across hosts/sessions;
- Git/CI/domain stores may own other authoritative state.

### Candidate principle

> **Resume state should be persisted by the component that can deterministically identify completed work and the next legal transition.**

**Disposition: STRONGLY REINFORCE Session Rollover / Single Ownership.**

---

# 11. The run manifest is a good observability boundary

TaskFlow emits a machine-readable manifest with:

- overall status;
- error;
- task status;
- model labels;
- timing;
- token/cache usage;
- named outputs.

It intentionally excludes endpoints and tokens.

The manifest is an **audit artifact**, not an authority source. The implementation even treats failure to write the manifest as best-effort so telemetry failure does not change the actual task result.

### HE implication

This reinforces two existing rules:

> **Observability should consume execution truth, not become execution truth.**

> **Telemetry publication failure should not silently redefine the underlying acceptance result.**

**Disposition: REINFORCE AI Engineering Observability.**

---

# 12. Idempotency supports recovery, but only where semantics permit it

The fuzzing repository explicitly pursues idempotency where cheap: repeated pipeline runs upsert targets/harnesses/runs rather than blindly duplicating state, and persistent corpus/call-graph suggestions survive later campaigns.

This is useful for autonomous work because retries and resumes are normal, not exceptional.

### Candidate principle

> **Operations likely to be retried or resumed should be idempotent where the domain semantics permit it.**

This should not become a universal requirement for consequential actions that inherently cannot be replayed safely.

**Disposition: REINFORCE bounded retry/recovery design.**

---

# 13. The security boundary is honest: logging is not containment

The fuzzing workflow can execute arbitrary model-selected build/shell commands directly on the host.

The current toolbox configuration intentionally does **not** require interactive confirmation because the workflow is designed to run unattended. Instead, Security Lab tells users to:

- use a disposable Codespace/VM;
- avoid elevated privileges;
- restrict network access to what is necessary;
- retain shell logs for after-the-fact inspection.

This is the correct conceptual distinction:

```text
audit log
    ≠
permission gate
    ≠
execution containment
```

### HE implication

> **When unattended execution intentionally removes an interactive approval gate, containment must move to another enforceable boundary rather than being replaced by logging.**

**Disposition: STRONGLY REINFORCE.**

---

# 14. Executed configuration outranks stale comments

This deep pass found a concrete documentation/configuration contradiction.

`run_fuzzing.sh` and the Python `local_shell.py` docstrings still contain text saying shell commands require/surface confirmation.

The current `toolboxes/local_shell.yaml` says the opposite: `shell_exec` is intentionally **not** under `confirm` because autonomous execution has no interactive stdin. The taskflow README and security warning agree with the YAML.

The executable toolbox configuration is therefore the relevant current behavior; some descriptive comments are stale.

### HE implication

This is a small but useful example of Current State Authority:

> **When documentation and executable configuration disagree, verify executed behavior before treating either prose description as truth.**

The durable correction, if this were HE-owned code, would belong in the stale comments/docs rather than in the working configuration merely to make them agree.

**Disposition: STRONGLY REINFORCE owning-component correction and current-state authority.**

---

# 15. What not to copy

The project is a strong case study, not a blueprint for HE.

Do **not** infer that HE should add:

- a YAML workflow engine;
- a general MCP execution layer;
- SQLite campaign state for ordinary coding sessions;
- a dashboard;
- geometric budgets for every task;
- line coverage as a universal progress metric;
- autonomous host shell access;
- multi-model fan-out by default;
- crash/finding taxonomies unrelated to the target domain.

The transferable mechanisms are smaller than the implementation.

---

# Durable HE extraction

## STRONGLY MINE / REINFORCE

- model judgment and deterministic execution can have separate owners;
- mandatory lifecycle/evidence bookkeeping can precede optional probabilistic reasoning;
- long-running campaign state should live in a durable source-owned store rather than conversational context;
- resource escalation should remain externally owned when the worker is autonomous;
- objective plateau signals can bound suitable exploratory loops;
- candidate generation and candidate evaluation should be separated during controlled bakeoffs;
- freeze candidates while measuring them when mutation would contaminate comparison;
- deterministic tie-breaking prevents a model from inventing a winner after a tie;
- failure discovery, reproduction, classification, attribution and remediation are distinct evidence stages;
- `harness_bug` / evaluator failure must remain valid outcomes;
- minimal executable reproducers are stronger continuation evidence than prose alone;
- disappearance of an effect needs equivalent execution/evaluator conditions before it is attributed to the intended fix;
- stage criticality should define whether partial failure may continue and what becomes incomplete;
- task-level checkpoint/resume belongs to the runtime that owns task lifecycle;
- retryable operations should be idempotent where domain semantics allow;
- audit telemetry should not own run truth;
- logs are not containment;
- stale documentation should not outrank executable current state.

## ASSESS

- whether ACR should persist more exact per-run/candidate lifecycle state before attempting richer autonomous calibration loops;
- whether ACR's seeded-defect fixtures can distinguish defect-not-seen from defect-seen-but-missed;
- whether ACR findings would benefit from a minimal executable/replayable reproducer artifact for selected high-value findings;
- whether any future HE autonomous experiment loop has a trustworthy objective progress metric that supports adaptive spend/plateau stopping;
- whether tolerated branch failures are always visible in generated summaries/manifests.

## PARK

- Security Lab TaskFlow adoption;
- autonomous fuzzing in HE;
- campaign SQLite as generic HE infrastructure;
- generic experiment dashboard;
- geometric budget schedules outside workloads with measured progress semantics;
- general multi-model workflow orchestration.

## REJECT

- conversation history as campaign state;
- allowing an optimizing candidate to redefine the metric that selects it;
- interpreting a non-reproducing old failure as proof of the intended root-cause fix without checking the execution envelope;
- treating logging as a security boundary;
- treating stale prose/commentary as stronger authority than executable configuration;
- autonomous arbitrary host-shell execution without an appropriate isolation boundary.

---

# Canonical HE ownership

The detailed implementation evidence remains in this source note.

Existing canonical owners already contain the high-level principles and should remain the durable synthesis rather than spawning a new orchestration doctrine:

- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
  - externally owned budgets/stops;
  - arbitrary-command isolation;
  - tool/action authority;
  - bounded autonomous exploration.

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
  - verifier calibration;
  - independent acceptance semantics;
  - candidate/evaluator separation;
  - artifact identity and effect verification.

- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
  - narrow continuation state;
  - source-owned resume information;
  - evidence-directed verification.

- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
  - source-owned facts;
  - audit/telemetry as consumers rather than authority;
  - snapshot/history distinctions.

No new HE control plane or TaskFlow architecture is justified by this research.

---

# Bottom line

The most valuable Security Lab lesson is not “use agents for fuzzing.”

It is:

> **Let the model choose among uncertain engineering moves inside a bounded experiment, while deterministic components own execution, evidence capture, resource ceilings, durable state, stopping, and consequential authority. Then reproduce and attribute failures before proposing repair.**

The repo is valuable precisely because it also exposes the limits of that pattern: when the model still has arbitrary shell authority, orchestration discipline does not replace containment; when an old crash disappears, replay does not automatically prove the intended fix; and when comments disagree with executable configuration, current executed state must win.