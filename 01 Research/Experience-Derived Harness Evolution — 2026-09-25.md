# Experience-Derived Harness Evolution — 2026-09-25

## Purpose

Synthesize the Harness Engineering implications of mining real coding-agent history to improve rules, skills, hooks, tools, workflows and evaluation assets without turning raw transcripts into a new memory authority or automated self-modification system.

Primary evidence:

- `01 Research/Sources/Cole Medin — Conversation Mining & Harness Evolution — 2026-09-25.md`
- `coleam00/skills` at commit `dfaa9105741fc5ba9b16b6a72551cad4bad70415`

Related HE research:

- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Source Preservation & Evidence Durability — 2026-09-16.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`

This is research synthesis only. It does not authorize transcript export, permanent transcript retention, PMB redesign, automatic harness editing, or new analytics infrastructure.

---

# Executive synthesis

Harness Engineering already says systems should improve continuously from evidence rather than intuition.

Agent-history mining adds a concrete feedback source:

```text
real work
   ↓
execution traces
   ↓
structured observations
   ↓
incident/pattern analysis
   ↓
candidate harness change
   ↓
controlled evaluation
   ↓
human-approved correction
   ↓
future work
```

The central rule is:

> **History can tell us where to look; only evaluated evidence should decide what becomes permanent.**

This creates a practical closed loop between observability, diagnosis, ownership, behavioral evals and harness maintenance.

---

# 1. Treat agent history as telemetry/evidence, not memory

Conversation logs can explain:

- what the user asked;
- which tools were called;
- what failed;
- what was retried;
- which correction the user supplied;
- where work diverged;
- which artifacts were produced.

They should not automatically become:

- project truth;
- long-term memory;
- startup context;
- instructions;
- acceptance evidence.

A transcript contains historical claims and intermediate state that may be wrong or obsolete.

### Candidate principle

> **Operational traces are evidence for harness evolution, not durable project truth.**

This preserves the current PMB boundary: durable project state belongs in its owning memory/artifact, while history remains evidence about how that state was reached.

---

# 2. Use two loops: reactive and proactive

## Reactive improvement loop

Use after a specific run fails, surprises the user or requires correction.

```text
observed symptom
   ↓
reconstruct the run
   ↓
identify cause + owning component
   ↓
propose smallest durable correction
   ↓
replay original task / failure path
```

This should be evidence-directed and may conclude that no permanent change is justified.

## Proactive improvement loop

Use periodically across a bounded history window.

```text
recent runs
   ↓
aggregate recurring friction / corrections / repeated work
   ↓
rank by impact + recurrence + ownership
   ↓
identify candidate automation/rule/skill/test removal or addition
```

The proactive loop should not promote something simply because it occurred frequently.

### Candidate principle

> **Separate single-run failure learning from cross-run pattern mining.**

This prevents a one-off mistake from becoming permanent scar tissue and prevents broad trend analysis from losing the evidence detail needed for root cause.

---

# 3. Reduction should precede semantic analysis

Raw transcripts are high-volume, noisy and source-specific.

For structural questions, deterministic tools should reduce them first.

Examples:

- count commands/tool calls;
- identify retries/timeouts;
- group sessions by project/model;
- locate repeated user corrections;
- detect repeated file/path errors;
- extract timestamps/session IDs;
- identify explicit error/result fields.

Then send only:

```text
counts
representative examples
source pointers
uncertainty / parse gaps
```

to a semantic model.

### Candidate principle

> **Use deterministic reduction for structural facts; spend model context on interpretation.**

This is Progressive Disclosure applied to telemetry.

---

# 4. Preserve source identity through the analysis pipeline

A normalized event/finding should retain enough provenance to answer:

- which host/runtime produced it;
- which session/run;
- which project/repo;
- when it occurred;
- what raw record/artifact supports it;
- which parser/schema version interpreted it;
- whether it is direct fact, derived fact or model inference.

Suggested conceptual shape:

```text
source
session_id
project_identity
observed_at
kind
value / evidence pointer
authority_class
parser/schema_version
confidence / derivation when applicable
```

Do not make this a mandatory schema implementation yet. The important mechanism is provenance retention.

### Candidate principle

> **Derived harness-learning evidence should remain traceable to the run that produced it.**

---

# 5. Transcript adapters need degradation semantics

Transcript formats are implementation details, not durable standards.

A parser should therefore report:

- records parsed;
- records skipped;
- unknown envelope/types;
- degraded status;
- parser/schema version.

If a vendor changes transcript format, an analytics system must not silently report fewer failures and interpret that as improvement.

### Candidate principle

> **Unknown transcript shapes are evidence gaps, not zero events.**

This extends HE's existing first-class `unknown/unverifiable` discipline.

---

# 6. Recommendations must map to the owning primitive

The analysis should not default to “add a line to CLAUDE.md.”

A candidate correction should first ask what owns the failure:

```text
code behavior              → code / test
hard invariant             → deterministic check / hook
repeatable procedure       → Skill
external action/data       → MCP / API / CLI / tool
project-wide behavior      → rules only if broadly and persistently needed
runtime/model config       → configuration
retrieval failure          → retrieval cue/index/routing owner
durable project decision   → owning memory/doc
one-off mistake            → maybe nothing persistent
```

### Candidate principle

> **Experience should improve the owning component, not merely expand startup instructions.**

This is Single Ownership applied to feedback loops.

---

# 7. A recommendation is not a validated improvement

The same agent that failed may also generate a plausible but wrong explanation for the failure.

Even a correct diagnosis does not prove a proposed harness change is beneficial.

Therefore the loop must separate:

```text
discovery / hypothesis
         from
behavioral evaluation / proof
```

Useful proof mechanisms may include:

- replay of the original failure;
- clean/dirty regression fixtures;
- control vs changed configuration;
- ablation;
- independent review;
- deterministic acceptance checks.

### Candidate principle

> **History proposes harness changes; experiments earn them.**

This is the most important guardrail against self-reinforcing prompt scar tissue.

---

# 8. Harness evolution includes deletion

A system that only mines failures will tend to accumulate rules.

History should also identify:

- behavior current models perform correctly without instruction;
- rules never exercised;
- workflows superseded by newer host capabilities;
- repeated steps now handled deterministically;
- skills whose routing/capability is obsolete;
- duplicate or conflicting instructions.

Ablation is the complementary operation to opportunity discovery.

### Candidate principle

> **Continuous improvement includes removing scaffolding that no longer changes evaluated behavior.**

This ties transcript mining directly to Model Capability Drift and context-budget maintenance.

---

# 9. Retention is part of the harness design

A history-mining loop creates pressure to keep raw transcripts indefinitely.

That is not automatically justified.

Raw traces can contain everything the agent was allowed to read or print, so retention creates security/privacy cost.

Use the smallest evidence retention that supports the decision.

Potential layers:

```text
short-lived raw transcripts
        ↓
normalized/redacted events
        ↓
selected durable regression cases / incident records
```

Not every organization or project will use all three.

### Candidate principle

> **Behavioral telemetry inherits the sensitivity of everything the agent was allowed to see.**

> **Preserve the evidence needed for learning, not an unlimited copy of all agent history.**

This extends Source Preservation without turning HE into an archive.

---

# 10. Local-first should be the default for transcript analysis

For a single-user/local engineering harness, cloud analytics infrastructure should require a demonstrated need.

Reasons:

- raw transcripts are sensitive;
- local volumes are often manageable;
- simple deterministic extraction can remove most token cost;
- a local database/file format avoids an additional trust/retention boundary;
- the value is the analysis loop, not the storage vendor.

Cloud/lakehouse infrastructure may become justified for:

- large team/shared analytics;
- very high event volume;
- governance/audit requirements;
- cross-project or organization-wide measurement;
- existing approved enterprise infrastructure.

Do not adopt Databricks merely because it was used in the source video.

**Disposition: local-first default; infrastructure on evidence.**

---

# 11. Relationship to the Dashboard / AI Engineering Cockpit

This research does **not** invalidate the existing Dashboard boundary that says the cockpit should not become a transcript warehouse.

The two concepts have different jobs.

## Harness-evolution pipeline

Historical evidence used to answer:

- what repeatedly goes wrong?
- what are we correcting manually?
- which rule/skill/tool should change?
- which configuration no longer earns its cost?

## Dashboard / cockpit

Operational/re-entry view used to answer:

- what is active now?
- what needs attention?
- what changed since I left?
- what are current context/quota/repo/PMB states?

A future cockpit may display summarized harness-evolution results, but should not own the raw corpus.

### Boundary

> **Analytics may produce a view; the view should not become the evidence store merely because it displays the result.**

---

# 12. Relationship to PMB

PMB should remain durable project/context memory, not historical analytics infrastructure.

Potential value from history mining:

- detect repeated retrieval misses;
- detect stale handoff patterns;
- detect repeated corrections to durable context;
- find memory that is frequently loaded but never useful;
- identify handoff/debugging state that is repeatedly reconstructed.

But the output should be **candidate PMB improvements**, not automatic memory mutation.

No PMB implementation change is justified yet.

The PMB pilot remains the better place to establish which behaviors actually need measurement.

---

# 13. Relationship to ACR

ACR already creates a rich domain-specific history of model findings, deterministic filters, evidence verification and calibration runs.

A history-mining loop could later identify:

- recurring false-positive mechanisms;
- finding families repeatedly suppressed or corrected;
- reviewer overlap/duplication;
- missing clean fixtures;
- timeout/runtime patterns;
- evidence mismatch classes;
- real incidents that should become committed regression cases.

The durable output should be a **candidate fixture/experiment**, not an automatically changed reviewer prompt or threshold.

This fits the existing ACR direction: real failures should become calibration evidence when reproducible.

---

# 14. Minimal closed-loop architecture

If HE later implements or pilots this pattern, the smallest useful architecture is:

```text
Agent hosts / session logs
          ↓
read-only source adapters
          ↓
local deterministic reducer
          ↓
small normalized evidence set
          ↓
reactive OR proactive semantic analysis
          ↓
owner-mapped candidate correction
          ↓
behavioral eval / replay / ablation
          ↓
human approval
          ↓
change in owning component
          ↓
future-run observation
```

Do not start with:

- cloud lakehouse;
- dashboard;
- universal transcript schema;
- autonomous self-editing;
- permanent raw retention.

Start with the smallest question that real evidence can answer.

---

# 15. What should be measured if this is piloted

Useful outcome measures include:

- recurring failure eliminated / not eliminated;
- new regression introduced;
- human corrections per comparable task;
- retries/failed attempts;
- context/startup cost added or removed;
- unique useful automation/rule candidate discovered;
- recommendation rejected as unsupported;
- parser coverage/degradation;
- wall time/token cost of analysis;
- whether a durable regression case was created.

Do not optimize for:

- number of recommendations;
- number of rules added;
- amount of history retained;
- number of tables/events captured.

Those are activity metrics, not improvement.

---

# Research disposition

## STRONGLY REINFORCE

- real work should feed continuous harness improvement;
- traces are evidence, not memory/truth;
- reactive failure learning and proactive pattern mining are different loops;
- deterministic reduction before semantic analysis;
- provenance through normalization/analysis;
- unknown parser state must remain visible;
- recommendations must map to owning components;
- behavioral eval/ablation before durable changes;
- deletion of obsolete scaffolding is improvement;
- local/private handling by default.

## ASSESS

- a minimal local session/event reducer for a selected HE/PMB pilot window;
- conversion of real recurring failures into behavioral regression cases;
- minimal evidence retention needed after raw transcripts expire;
- ACR history-to-calibration-case workflow;
- whether a periodic proactive scan provides unique value beyond incident-driven review.

## PARK

- Databricks adoption;
- permanent transcript warehouse;
- organization-wide history analytics;
- universal cross-agent event schema;
- Dashboard ownership of historical trace storage;
- autonomous harness self-editing.

## REJECT

- ingesting raw transcript corpora directly into model context;
- treating the transcript as current project authority;
- promoting high-frequency patterns without consequence/ownership analysis;
- using the same model's recommendation as proof its fix is correct;
- turning every user correction into a permanent rule;
- measuring harness evolution by artifact count.

---

# Candidate HE principles — research status only

> **Operational traces are evidence for harness evolution, not durable project truth.**

> **Separate single-run failure learning from cross-run pattern mining.**

> **Use deterministic reduction for structural facts; spend model context on interpretation.**

> **History proposes harness changes; experiments earn them.**

> **Experience should improve the owning component, not merely expand startup instructions.**

> **Continuous improvement includes removing scaffolding that no longer changes evaluated behavior.**

> **Behavioral telemetry inherits the sensitivity of everything the agent was allowed to see.**

These should remain research principles until corroborated by additional independent sources and/or local behavioral evidence.

---

# Bottom line

This source provides a concrete mechanism for HE's Continuous Improvement principle:

> **mine what the harness actually did, reduce it to auditable evidence, propose the smallest owner-correct change, prove that change against real behavior, and only then make it durable.**

That is much stronger than either “let the model learn from its chats” or “store every transcript forever.”
