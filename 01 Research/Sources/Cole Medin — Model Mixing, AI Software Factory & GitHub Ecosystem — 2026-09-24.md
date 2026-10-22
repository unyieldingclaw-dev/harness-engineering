# Cole Medin — Model Mixing, AI Software Factory & GitHub Ecosystem — 2026-09-24

## Purpose

Capture the useful evidence from Cole Medin's video **"AI Coding Rate Limits are RIDICULOUS Now - Here's How You Keep Scaling Anyway"** and inspect the current public repositories under `coleam00` for mechanisms relevant to Harness Engineering, PMB, and ACR.

Primary user source:

- video: `https://www.youtube.com/watch?v=NZq88JAJSag`
- transcript supplied by the user on 2026-09-24

Repositories inspected directly:

- `coleam00/ai-software-factory`
- `coleam00/Archon`
- `coleam00/adversarial-dev`
- `coleam00/agent-control-plane`
- `coleam00/harness-engineering-demo`
- `coleam00/claude-memory-compiler`
- `coleam00/context-engineering-intro`
- `coleam00/ai-native-starter-pack`
- `coleam00/Linear-Coding-Agent-Harness`

This is research evidence. It does **not** authorize installing Archon, adopting the factory, changing PMB, changing ACR, or introducing a model router.

---

# 1. Video claim: the expensive model is not needed for every stage

The video's main workflow is:

```text
issue / requested work
        ↓
strong model: plan
        ↓
cheaper/faster model: implement
        ↓
strong model: review
        ↓
cheaper/faster model: repair
        ↓
deterministic tests/build
        ↓
merge
```

Cole reports that his current preferred combination is GPT-6 Astra for planning/review and GLM 5.3 Flash for implementation/fixes. He says the mixed workflow used about one quarter of the cost/tokens of the all-frontier alternative on the example shown while producing similar or better visible results.

The durable idea is not those exact model names.

It is:

> **Route capability by task semantics and measured task fitness, not by a blanket assumption that the strongest model should perform every token-heavy stage.**

The video's evidence is anecdotal and limited to Cole's applications and workflow. It does not establish that implementation is always safe to delegate to a smaller model or that planning/review universally require the largest model.

**Disposition: REINFORCE behavioral evaluation; ASSESS task-stage model tiering.**

---

# 2. The implementation stage is a plausible cost/rate-limit leverage point

Cole's rationale is operational: implementation often consumes more tokens than planning/review, so replacing the implementation model can materially reduce subscription/API pressure.

That is plausible and useful, but HE should not encode:

```text
planning = large
implementation = small
review = large
```

as doctrine.

The correct abstraction is:

```text
stage / workload class
        +
required judgment
        +
verification strength
        +
measured model fitness
        ↓
model / effort selection
```

A well-specified mechanical implementation with strong tests may tolerate a smaller model. An unfamiliar concurrency bug, schema migration, security-sensitive change, or weakly specified integration may not.

**Disposition: ASSESS through real-task evals, not static routing rules.**

---

# 3. The video's workflow has an important property: the builder does not settle its own result

The same model class may fix findings, but the flow returns the candidate to a separate review stage and then deterministic checks.

This independently reinforces existing HE work:

- producer feedback and independent verification are different;
- deterministic checks should establish deterministic facts;
- model turn completion is not task completion;
- repair loops must be bounded;
- a stronger reviewer does not make the builder's self-report authoritative.

The video explicitly includes a maximum retry count around review/fix iteration.

**Disposition: STRONGLY REINFORCE.**

---

# 4. `coleam00/ai-software-factory`: the assurance mechanisms are more interesting than the autonomy claim

The README describes a repository that takes an issue through triage, planning, implementation, independent judgment, runtime/holdout verification, bounded repair, merge and optional deployment.

The strongest mechanisms found are below.

## 4.1 Human-owned mission and explicit non-goals

`MISSION.md` is protected and explicitly human-owned. It defines:

- what the product is;
- who it serves;
- in-scope capabilities;
- permanent out-of-scope requests;
- hard invariants;
- allowed evolutions;
- definition of done;
- open decisions;
- permanently human-owned qualities.

Its most interesting section is the permanent out-of-scope list. The repo explicitly treats this as protection against plausible but drifting requests.

### HE implication

Positive requirements alone do not define authority. For autonomous or semi-autonomous work, explicit **non-goals / forbidden scope expansion** can be as important as the requested outcome.

This does not mean every project needs a `MISSION.md`. The useful mechanism is an authoritative source that distinguishes:

```text
allowed evolution
vs
reasonable-looking scope drift
```

**Disposition: REINFORCE bounded authority; ASSESS in autonomous workflows only.**

## 4.2 The producer cannot modify its own judge

The factory protects governance, harness, lock and holdout paths. The builder may add assertions but cannot loosen the acceptance system without human action.

### HE implication

> **The worker that is being evaluated should not be able to silently weaken the mechanism that decides whether it passed.**

This is stronger than merely using an independent reviewer.

Potential applications include:

- test/verification configuration;
- protected policy files;
- holdout scenarios;
- acceptance thresholds;
- merge/release gates.

Do not apply protection mechanically where legitimate feature work must evolve tests. The important issue is **who is authorized to loosen a gate**, not whether tests are immutable.

**Disposition: STRONGLY REINFORCE authority separation.**

## 4.3 Independent holdout verification

The factory distinguishes ordinary end-to-end journeys from independent holdout scenarios and warns that a file named "holdout" is not actually hidden if the builder can read it.

### HE implication

This is a useful assurance pattern for behavior where overfitting to visible tests is a real concern:

```text
producer-visible acceptance checks
        +
independent verifier-only scenarios
```

Do not create hidden-test infrastructure by default. Use it where the model could trivially optimize to known examples or where unattended merge authority requires stronger evidence.

**Disposition: ASSESS for behavioral harness evals / autonomous delivery.**

## 4.4 Mutation calibration: prove that the verifier can fail

The factory includes deterministic mutations and records incidents where defects were or were not caught by particular verification rungs. Its setup guidance requires a baseline to pass, a relevant deliberate fault to fail, and wrong-identity evidence to remain inconclusive rather than being misreported.

This is a high-value mechanism.

### HE principle candidate

> **A verification system is not trusted merely because it reports green; calibrate it with known-negative cases that it must reject.**

This is directly relevant to:

- PMB behavioral evals;
- ACR calibration;
- runtime validation;
- release gates;
- evidence-verifier tests.

**Disposition: STRONGLY REINFORCE.**

## 4.5 Candidate identity matters

The factory's runtime material binds verification evidence to a delivered candidate/source revision and treats wrong-identity results separately.

### HE implication

> **Verification evidence should be attributable to the exact artifact it claims to verify.**

A passing runtime test against stale code is not evidence for the current candidate.

This is a concrete instance of HE's broader authority/provenance work.

**Disposition: STRONGLY REINFORCE.**

## 4.6 Bounded scheduling and stop controls

The consumer exposes halt/cancel behavior, schedules whole workflows rather than overlapping stage fragments, and states that timer ticks never overlap.

### HE implication

This supports the existing bounded-execution finding that scheduled autonomy needs lifecycle state, non-overlap, explicit cancellation/halts, and durable run identity rather than merely `cron + prompt`.

**Disposition: REINFORCE; no HE scheduler implementation implied.**

---

# 5. `coleam00/Archon`: deterministic skeleton, probabilistic nodes

Current Archon describes itself as a workflow engine for AI coding agents. Its useful architecture is the explicit composition of deterministic and probabilistic nodes:

```text
workflow-owned sequence
  ├─ AI plan
  ├─ AI implementation loop
  ├─ deterministic tests
  ├─ AI review
  ├─ human approval gate
  └─ PR action
```

Other useful properties include:

- isolated worktree per run;
- committed workflow definitions;
- fresh context as an explicit loop option;
- human-interactive gates;
- multiple UI/transport surfaces over one workflow definition.

### HE implication

The durable mechanism is:

> **Let deterministic workflow state own sequencing and gates; use the model only where judgment/generation is actually needed.**

This is strong corroboration for HE's artifact-gated and deterministic-vs-probabilistic separation.

### Caution

HE should not adopt Archon merely because it already implements these patterns. Archon is a full workflow engine, and adopting it would create meaningful runtime, configuration and exit-cost commitments.

**Disposition: MINE; do not adopt by default.**

---

# 6. `coleam00/adversarial-dev`: useful separation, questionable scoring semantics

The repo separates planner, generator and evaluator into distinct contexts and uses file-based artifacts between them. It also negotiates explicit completion contracts before building and bounds evaluator retry loops.

Useful mechanisms:

- separate generation from evaluation;
- define observable "done" before implementation;
- role-local context;
- durable file artifacts between agents;
- bounded retry.

Cautions:

- numeric 1-10 evaluator scoring with a fixed threshold is not inherently calibrated evidence;
- "adversarial pressure" can generate review noise or gratuitous changes;
- three agents are an implementation choice, not a principle;
- claims that this is categorically superior to single-agent work require task-specific measurement.

**Disposition: REINFORCE verifier separation; reject uncalibrated score-as-truth.**

---

# 7. `coleam00/agent-control-plane`: good run history and human resume gate; weaker completion authority

The control-plane repo stores run history in Neon, uses fresh rounds, supports pause/stop/resume, caps iterations, and parks a run in `awaiting_approval` before more rounds are authorized.

Useful mechanisms:

- external durable run state;
- explicit iteration cap;
- human resume authority;
- run/parent identity;
- token/status history;
- provider-independent runner boundary through Pi.

Caution:

The default orchestrated mode lets an LLM orchestrator decide whether the goal is done. That may be acceptable for some exploratory work, but HE should prefer explicit task/artifact completion semantics where consequential outcomes require stronger evidence.

The dashboard is also an active control plane, unlike HE's currently PARKed observer-only cockpit concept.

**Disposition: MINE lifecycle ideas; do not import control-plane architecture.**

---

# 8. `coleam00/harness-engineering-demo`: mostly corroboration of mechanisms HE already has

The demo shows:

- project rules + on-demand context;
- Plan → Implement → Validate flow;
- post-edit lint/typecheck hooks;
- stop-time validation gates;
- pre-tool security guards;
- separate review subagent;
- worktree isolation;
- bounded Ralph-style iterations.

This is useful implementation evidence, but little is new relative to HE/PMB/Superpowers research already captured.

One durable reminder is that hook-level controls can still apply during unattended runs, which is preferable to relying on prompt discipline alone.

**Disposition: REINFORCE; avoid duplicate documentation.**

---

# 9. `coleam00/claude-memory-compiler`: PMB-adjacent and worth a dedicated comparison later

This repo captures SessionEnd/PreCompact transcripts, uses a background model pass to extract durable knowledge into daily logs, compiles those logs into structured markdown concepts, and injects a compact index at future session start. Retrieval is index-guided rather than vector-based at small scale.

Potentially useful mechanisms for PMB research:

- automatic capture at session/compaction boundaries;
- durable raw-to-curated pipeline;
- background extraction rather than blocking the active session;
- concept/index separation;
- health checks for broken links, orphans, contradictions and staleness;
- explicit claim that full RAG may be unnecessary at small corpus size.

Risks / differences from PMB:

- model-generated extraction can promote incorrect or low-value memories;
- automatic injection can recreate startup-context growth if not tightly bounded;
- personal conversation mining has privacy/retention implications;
- the repo's scale thresholds are heuristics, not universal facts;
- PMB already has a deliberate durable/transient ownership model and should not be replaced by a transcript compiler without evidence.

### Recommendation

**PARK a dedicated PMB comparison until after the PMB pilot.** If pilot evidence shows capture/curation gaps, inspect this repo deeply against those observed failures.

---

# 10. Other repositories triaged

## `context-engineering-intro`

Earlier PRP/context-engineering template. Useful historically, but much of it duplicates current Superpowers + HE artifact-gated planning concepts. Its blanket "more examples = better" framing should not be imported without context-budget evidence.

**Disposition: LOW PRIORITY / historical corroboration.**

## `ai-native-starter-pack`

Useful patterns include deriving project rules from the real codebase, root-cause -> rule + regression-test feedback, and plan/implement/validate/review separation. Much overlaps current HE research.

**Disposition: MINE selectively if we later examine rule-generation / system-review loops.**

## `Linear-Coding-Agent-Harness`

Demonstrates one external system (Linear) as authoritative work state, with issue comments as handoff state and browser verification. The important principle is source ownership, not Linear itself.

**Disposition: REINFORCE one-authority-per-fact; no adoption need.**

Other repositories in the account are more application-specific or currently less aligned with HE's open research questions. Do not expand the research surface simply because they exist.

---

# 11. What the video does NOT prove

Do not preserve these as HE conclusions:

- that rate limits are universally worsening for every user/provider;
- that GLM 5.3 Flash is generally the best implementation model;
- that Astra is generally the best planning/review model;
- that a four-times cost result transfers across workloads;
- that frontier models are unnecessary for implementation;
- that a software factory should merge without human review;
- that more autonomous workflow stages imply better engineering.

These are workload- and time-sensitive claims that require local measurement.

---

# 12. Cross-project implications

## Harness Engineering

Strongly relevant:

- evaluated model tiering by workload class;
- deterministic workflow skeleton with probabilistic judgment nodes;
- worker cannot weaken its own acceptance authority;
- holdout verification where overfitting risk justifies it;
- verifier mutation calibration;
- candidate/evidence identity;
- bounded repair and scheduled-run lifecycle.

## PMB

No immediate implementation change. The memory compiler is a post-pilot comparison candidate. PMB should not become a workflow engine or token router.

## ACR

The strongest directly relevant mechanism is **verification calibration with known-negative cases**, which ACR already partially embodies. The factory's independent holdout concept may also inform external benchmark design, but it should not replace AACR-Bench or ACR's current evidence-basis model.

---

# Overall disposition

## STRONGLY REINFORCE

- separate producer from acceptance authority;
- deterministic gates for deterministic facts;
- calibrate verifiers using known failures;
- bind evidence to the exact candidate/revision;
- bound repair/continuation loops;
- preserve explicit scope/non-goals for autonomous work.

## ASSESS

- model tiering by workload class using real-task behavioral evals;
- independent holdout scenarios for unattended delivery;
- whether implementation stages can safely use cheaper models in specific project classes.

## PARK

- dedicated PMB comparison with `claude-memory-compiler` until pilot evidence exists;
- scheduler/control-plane implementation;
- hidden holdout infrastructure without an unattended-delivery need.

## REJECT

- hard-coded model-role mapping as HE doctrine;
- uncalibrated model confidence/score as acceptance authority;
- adopting a software-factory/control-plane architecture before a concrete operational problem requires it;
- copying every Cole Medin repository into the HE research backlog.
