# Cole Medin — Conversation Mining & Harness Evolution — 2026-09-25

## Purpose

Mine durable Harness Engineering lessons from Cole Medin's video **“The Biggest AI Coding Agent Upgrade Is Already on Your Machine?!”** and the current `coleam00/skills` implementation behind the video's opportunity-scan workflow.

Primary user source:

- Video: https://www.youtube.com/watch?v=td52e2tQFIU
- Creator: Cole Medin
- Transcript supplied by the user on 2026-09-25

Implementation evidence inspected:

- `coleam00/skills` at commit `dfaa9105741fc5ba9b16b6a72551cad4bad70415`
- `.claude/skills/opportunity-scan/SKILL.md`
- `.claude/skills/system-evolution-review/SKILL.md`
- `.claude/skills/ablate-ai-layer/SKILL.md`

First-party behavior/terms checked:

- Claude Code application-data documentation for local transcript/history storage and cleanup
- Databricks Free Edition documentation/terms summary

Related HE research:

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Source Preservation & Evidence Durability — 2026-09-16.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Sources/Cole Medin — Model Mixing, AI Software Factory & GitHub Ecosystem — 2026-09-24.md`

This is research evidence. It does **not** authorize uploading private transcripts to Databricks, retaining all transcripts indefinitely, changing PMB, or automatically editing rules/skills/hooks from mined recommendations.

---

# Executive finding

This is one of the more directly HE-relevant sources reviewed so far.

The durable idea is not Databricks and not “let Claude read all its old chats.” It is a closed-loop harness-maintenance pattern:

```text
real agent runs
    ↓
raw execution evidence
    ↓
deterministic reduction / normalization
    ↓
pattern or incident analysis
    ↓
candidate harness correction
    ↓
behavioral eval / ablation / replay
    ↓
human-approved durable change
    ↓
observe future runs
```

The key distinction is:

> **Operational traces can become evidence for improving the harness, but they are not automatically memory, truth, or permission to change the harness.**

This source strongly reinforces HE's Continuous Improvement principle while adding a concrete evidence source: the actual history of how the harness behaved in real work.

---

# 1. The local conversation history is real, useful execution evidence

Current Claude Code documentation states that `~/.claude/projects/<project>/<session>.jsonl` contains the full persisted conversation transcript, including messages, tool calls and tool results. These session transcripts are automatically cleaned when older than `cleanupPeriodDays`, which defaults to 30 days. `~/.claude/history.jsonl` stores typed prompts and is not covered by the same automatic cleanup.

This makes the video directionally correct: a significant body of behavioral evidence may already exist locally without adding a new logging layer.

However, two corrections matter.

## 1.1 Do not assume transcript = complete cognition

The video says the files contain every “thought” the agent had. HE should not preserve that as a literal claim.

A transcript contains whatever the host chose to persist. It should be treated as **execution/log evidence**, not as a complete representation of the model's hidden internal reasoning.

## 1.2 A transcript is not authoritative project truth

Transcripts can contain:

- stale assumptions;
- failed attempts;
- model guesses;
- user corrections;
- abandoned plans;
- tool outputs that were later superseded.

Therefore:

> **History can explain what happened; current source-owned state still governs what is true now.**

This is consistent with HE/PMB's existing handoff and current-state authority work.

**Disposition: STRONGLY REINFORCE as evidence source, not truth source.**

---

# 2. The strongest design is two separate improvement loops

The current `opportunity-scan` skill is more disciplined than the video's simple “top ten improvements” demo.

It explicitly separates two targets.

## Reactive loop — one failed or frustrating run

Input:

- the run's artifacts;
- optionally the session log;
- the user's observed symptom.

Question:

> **What in the AI layer would have prevented this specific failure?**

The skill instructs the agent to reconstruct the run, verify the user's symptom against evidence, identify the smallest durable change and admit when a failure does **not** justify a permanent harness change.

This maps very closely to the user's existing failure-analysis prompt and HE's diagnosis-before-mutation work.

## Proactive loop — a window of real usage

Input:

- a bounded window of logs/history;
- an optional theme/steer.

Question:

> **What do I keep doing/correcting manually that should become a durable harness primitive?**

The skill recommends aggregating recurring commands, repeated instructions/corrections, tool usage and friction rather than ingesting all raw transcripts into context.

### HE implication

Do not collapse incident analysis and trend mining into one generic “learn from history” pass.

They answer different questions:

```text
reactive: why did this run fail and what smallest durable change could prevent recurrence?

proactive: what recurring friction is common/expensive enough to justify encoding?
```

**Disposition: STRONGLY REINFORCE.**

Candidate principle:

> **Separate single-run failure learning from cross-run pattern mining.**

---

# 3. “Aggregate, don't ingest” is a high-value HE mechanism

The `opportunity-scan` implementation explicitly warns that log windows can be huge and tells the agent to prefer prompt/command history, reduce with deterministic tools (`jq`, `grep`, `sort | uniq -c`), and put only frequencies and representative samples into model context.

This is much stronger than asking a frontier model to read hundreds of thousands of raw transcript tokens.

The useful architecture is:

```text
raw logs
   ↓
parser / deterministic reducer
   ↓
counts + representative samples + pointers
   ↓
semantic analysis
```

not:

```text
all transcripts
   ↓
context window
   ↓
hope
```

### HE implication

> **Use deterministic reduction for structural facts; spend model context on interpretation.**

This extends Progressive Disclosure and aligns with the existing Dashboard/observability finding that structured source data outranks transcript inference.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Raw transcript storage and structured observations are different layers

The video proposes putting raw JSONL plus normalized tables into Databricks. The architectural separation is useful even if Databricks itself is not.

A scalable pipeline can distinguish:

```text
raw source transcript
        ↓
source-specific parser
        ↓
normalized observations/events
        ↓
rollups / queries
        ↓
semantic analysis
```

Potential normalized entities include:

- session/run identity;
- project/repo;
- model/host/runtime when observable;
- user prompt/command metadata;
- tool call/result metadata;
- retries/failures;
- corrections/escalations;
- context/handoff/compaction events where available;
- artifact/commit identity;
- verification results;
- usage/timing when source-reported.

Do not infer semantic labels such as “failure”, “bad plan” or “wrong decision” into the base event layer unless the derivation is explicit.

### Existing HE connection

The Dashboard research already recommends normalized source-owned snapshots/events and transcript adapters only where no stronger interface exists. This video strengthens the historical-analysis use case but does **not** justify turning the Dashboard into a transcript warehouse.

**Disposition: REINFORCE adapter + normalized-event architecture.**

---

# 5. Transcript schema drift is a real reliability risk

The video itself notes inconsistent nested schemas across transcript records. This directly connects to the earlier Logic Loop finding that transcript parsers need schema-drift tripwires.

A durable transcript-mining system should record at least:

- source host;
- parser/schema version;
- unknown/unparsed record count;
- ingest time;
- source session identifier;
- degradation status when the parser encounters new envelopes.

Unknown records should not silently disappear and produce false trends.

### Candidate principle

> **A degraded parser must be visible; unknown transcript shapes are evidence gaps, not zero events.**

**Disposition: STRONGLY REINFORCE.**

---

# 6. Discovery is not evaluation

The current `opportunity-scan` skill states this explicitly:

> it is a discovery tool for **what to change**, not a quality eval for **whether the built change is good**.

This distinction is essential.

A transcript-derived recommendation such as:

```text
“add a rule never to guess file paths”
```

is a **hypothesis**.

It is not evidence that the rule will improve future behavior without collateral cost.

A stronger HE loop is:

```text
observed recurring failure
        ↓
proposed owner + correction
        ↓
controlled behavioral replay / eval
        ↓
compare quality, regressions, context cost
        ↓
approve or reject
```

This connects directly to `ablate-ai-layer`, whose current implementation runs control/stripped arms in throwaway worktrees, grades rules blind to treatment, and keeps “untested” distinct from “no difference.”

### Candidate principle

> **History proposes harness changes; experiments earn them.**

**Disposition: STRONGLY REINFORCE.**

---

# 7. The owning-component rule is more important than the recommendation count

The video's simple example asks the model to suggest changes to rules, hooks and skills. The current opportunity-scan skill is stronger because it first learns the active agent's extension points and maps findings to the best-fit primitive.

HE should go one step further and retain the existing ownership discipline:

```text
deterministic code/tool defect → code/test/hook
repeatable procedure          → Skill
project-wide behavior         → rules / CLAUDE.md when truly always-needed
external capability           → MCP/API/CLI/tool owner
runtime/configuration         → runtime/config
project decision              → owning durable project artifact
one-off execution error       → possibly no durable change
```

Do not let “conversation mining” become an automated CLAUDE.md growth machine.

The current skill itself contains the useful warning that not every failure is a system gap and that high-frequency behavior is not automatically worth encoding.

**Disposition: STRONGLY REINFORCE Single Ownership.**

---

# 8. The dynamic repository-tree hook is useful only if an observed failure justifies its cost

The video reports a real change from its mining pass: a session-start hook that injects the current repository layout to reduce guessed paths.

The mechanism is better than a static directory tree copied into global rules because the data is generated from current repository state.

But HE should not generalize this into “inject the repo tree every session.”

Questions to answer first:

- Are path-guess errors frequent enough to matter?
- Can the host/tool search discover paths cheaply on demand?
- How large is the tree/context cost?
- Would an action-local file discovery step solve the same problem with less startup context?
- Does the repository structure change enough that startup injection materially helps?

### HE implication

> **Prefer dynamic source-derived context over stale handwritten mirrors, but still prove that always-loaded dynamic context earns its budget.**

The correct next step after adding such a hook is behavioral evaluation/ablation, not assuming “dynamic” means “free.”

**Disposition: ASSESS, not adopt.**

---

# 9. Databricks is an implementation choice, not the HE pattern

The video uses Databricks because it offers storage, Spark/SQL, Genie and an MCP/CLI surface.

HE should not infer that a cloud lakehouse is required for agent-history analysis.

At personal/project scale, the same architecture could be implemented with a local database or columnar files plus a deterministic parser/query layer. A larger managed platform becomes reasonable only when volume, collaboration, governance or cross-team analytics justify it.

### Important current Free Edition boundary

Databricks' current Free Edition documentation states that it is for learning/training/non-commercial use and that Databricks reserves the right to train on data uploaded there. The video description itself also warns that Free Edition terms differ from paid terms.

Therefore:

- do not upload proprietary/work transcripts to Free Edition;
- do not assume a scrub pass is perfect protection;
- treat transcript export as a data-governance decision, not a convenience step.

### HE disposition

**PARK Databricks adoption. REINFORCE local-first/private-by-default analysis.**

---

# 10. Transcript privacy risk is higher than ordinary source-code analytics

Claude Code's current application-data documentation states that session transcripts are plaintext on disk and that content read by tools, command output and pasted text can appear in those transcripts. OS file permissions are the primary at-rest protection.

That means a transcript corpus may contain:

- source code;
- internal paths;
- command output;
- issue/customer/business text;
- pasted data;
- accidentally printed credentials;
- external tool responses.

Therefore, a transcript-mining pipeline needs an explicit retention and export policy.

Possible controls:

- local processing by default;
- bounded retention window;
- project/repo allowlist;
- deterministic secret scanning/redaction before export;
- exclusion of known-sensitive paths/sessions;
- provenance that records whether content is raw, redacted or derived;
- no raw transcript replication when derived observations answer the question.

### Candidate principle

> **Behavioral telemetry inherits the sensitivity of everything the agent was allowed to see.**

**Disposition: STRONGLY REINFORCE security/governance boundary.**

---

# 11. Preserve evidence intentionally; do not retain every transcript forever by default

This video creates a useful tension with HE's Source Preservation note.

The answer is not “keep every transcript forever.”

HE's existing preservation rule still applies:

> preserve the evidence trail, not necessarily the entire artifact.

For harness-evolution cases, the durable evidence may be:

- session ID/date/project;
- short representative excerpt or derived event;
- exact observed failure/correction;
- relevant artifact/commit pointer;
- proposed harness change;
- eval/replay result;
- decision taken.

Raw transcripts can remain ephemeral when those derived records preserve enough evidence.

Retain raw history longer only when there is a concrete reproducibility, audit or research reason and the security/retention cost is accepted.

**Disposition: REINFORCE selective evidence durability.**

---

# 12. A harness-evolution dataset should include successes, not only failures

The video focuses heavily on friction/failures because they produce obvious improvement ideas.

A failure-only corpus can bias the system toward scar-tissue rules.

Useful questions also include:

- Which tasks repeatedly succeed without special rules?
- Which old rules are never exercised?
- Which workflows became unnecessary after model upgrades?
- Which intervention actually reduced retries?
- Which skill is consistently routed correctly?
- Which tools produce reliable outcomes with little supervision?

This is where `ablate-ai-layer` becomes especially relevant: improvement includes **removing** obsolete instructions, not only adding new ones.

### Candidate principle

> **Harness evolution should mine both recurring failure and recurring unnecessary scaffolding.**

**Disposition: STRONGLY REINFORCE Model Capability Drift + context reduction.**

---

# 13. Cross-project implications

## Harness Engineering

High-value research additions:

- real-run traces as a harness-maintenance evidence source;
- reactive incident learning vs proactive usage mining;
- deterministic reduction before semantic analysis;
- transcript adapters with schema-drift visibility;
- transcript-derived recommendations as hypotheses requiring eval;
- retention/privacy as part of harness observability;
- removal/ablation as a first-class improvement path.

## PMB

Do **not** turn PMB into a transcript database.

PMB may eventually benefit from evidence about:

- repeated retrieval failures;
- stale/misleading handoffs;
- repeated user correction of durable state;
- memory references that are never retrieved;
- recurring session-start friction.

But the telemetry/history pipeline should remain separate from PMB's durable project-memory authority.

No PMB change is justified from this source alone.

## Dashboard / AI Engineering Cockpit

The existing boundary remains valid: the Dashboard should not become a transcript warehouse.

A future cockpit could consume **derived harness-evolution findings**, such as:

- recurring failure class;
- recommendation awaiting validation;
- rule/skill under ablation;
- verified improvement/regression;

It should not own raw transcript history simply because history exists.

## ACR

ACR may eventually benefit from mining historical review runs for:

- repeated false-positive families;
- reviewer-specific duplication;
- evidence-verifier mismatches;
- timeout/configuration patterns;
- findings repeatedly corrected by humans;
- cases that should become committed calibration fixtures.

This should produce candidate benchmark/regression cases, not model-generated policy changes without calibration.

---

# 14. What the video does not prove

Do not preserve these as HE conclusions:

- that every agent stores equivalent local transcripts in equivalent formats;
- that a transcript contains every internal model thought;
- that 30 days is the correct retention period;
- that Databricks is the best or necessary storage/query layer;
- that free cloud upload is appropriate for private/proprietary transcripts;
- that frequent patterns automatically deserve permanent rules;
- that a model can safely modify its own harness from its own postmortem;
- that adding more rules/hooks/skills means the harness improved;
- that the video's specific repository-tree hook is universally useful.

---

# Overall disposition

## STRONGLY REINFORCE

- Continuous Improvement from real operational evidence.
- Single-run reactive learning distinct from cross-run proactive mining.
- Deterministic reduction before model interpretation.
- Source/adapter provenance and schema-drift visibility.
- Transcript-derived recommendations are hypotheses, not truth.
- Corrections belong in the owning component.
- Behavioral eval/ablation before durable harness change.
- Private/local handling of sensitive operational traces.
- Harness improvement includes removing obsolete scaffolding.

## ASSESS

- a small local normalized session/event schema for HE experiments;
- whether selected real sessions should seed behavioral evals;
- minimal retention needed to reproduce harness failures;
- dynamic repo-layout injection only if path-guess failures are measured;
- ACR conversion of recurring historical failure modes into committed calibration cases.

## PARK

- Databricks as HE's transcript analytics platform;
- a permanent transcript warehouse;
- cross-provider universal transcript ingestion;
- automatic harness self-modification from mined recommendations;
- Dashboard ownership of raw transcript history.

## REJECT

- reading entire transcript corpora into model context;
- treating transcript history as current project truth;
- treating frequency as proof of importance;
- treating every failure as a missing rule;
- uploading unreviewed work/proprietary transcripts to Databricks Free Edition;
- measuring improvement by number of new AI-layer artifacts.

---

# Candidate HE principles — research status only

> **Operational traces are evidence for harness evolution, not durable project truth.**

> **Separate single-run failure learning from cross-run pattern mining.**

> **Use deterministic reduction for structural facts; spend model context on interpretation.**

> **History proposes harness changes; experiments earn them.**

> **A degraded parser must be visible; unknown transcript shapes are evidence gaps, not zero events.**

> **Behavioral telemetry inherits the sensitivity of everything the agent was allowed to see.**

> **Harness evolution should mine both recurring failure and recurring unnecessary scaffolding.**

These should remain research candidates until compared against additional sources and/or local evidence.
