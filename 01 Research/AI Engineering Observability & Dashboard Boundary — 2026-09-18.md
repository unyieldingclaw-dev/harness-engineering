# AI Engineering Observability & Dashboard Boundary — 2026-09-18

## Purpose

Synthesize what Harness Engineering should retain from the September 18 deep mining pass on AI engineering dashboards, context telemetry, session state, worktree state, provider usage, and review history.

This note intentionally separates:

- observability principles that belong in HE research;
- PMB data-interface implications;
- a possible future Dashboard / AI Engineering Cockpit that does **not** belong inside HE or PMB.

Source evidence:

- `01 Research/Sources/AI Engineering Telemetry & Dashboard Repos — Deep Evidence Pass — 2026-09-18.md`

---

## Core finding

A useful engineering cockpit should primarily **aggregate source-owned facts** rather than ask an LLM to reconstruct system state from conversation.

Preferred architecture:

```text
Claude / Codex / other host runtime facts ─┐
Git / worktree / CI facts ─────────────────┤
PMB deterministic status ─────────────────┤
ACR machine-readable results ──────────────┤
provider usage / quota adapters ───────────┤
review/history systems ────────────────────┘
                     ↓
             adapters / collectors
                     ↓
         normalized snapshots + events
                     ↓
             derived attention state
                     ↓
             local read-only UI
```

Use an LLM only where the useful output is genuinely semantic, for example a concise “what changed since I left?” summary over already-grounded facts.

Do not use an LLM as the primary collector for branch state, context occupancy, quota state, CI status, stale handoffs, task-contract presence, or PMB health.

---

# Durable HE findings

## 1. Observability should consume authority, not become authority

A dashboard should not own project truth merely because it displays it.

Examples:

- Git owns branch/commit/worktree facts.
- Claude Code owns host-reported context-window and rate-limit facts.
- PMB owns PMB health/state facts.
- ACR owns review findings and evidence classifications.
- CI owns check/run status.

The cockpit may cache or normalize those facts, but should retain source identity and freshness.

**REINFORCE:** Single Ownership.

---

## 2. Structured source data outranks transcript inference

Source hierarchy for telemetry:

1. structured host/runtime data;
2. source-system machine-readable command/file/API;
3. source-emitted hook/event;
4. deterministic repository/system inspection;
5. transcript parsing when no better surface exists;
6. model inference for semantic interpretation only.

Transcript parsing can be useful, but vendor transcript formats are not stable contracts. If used, isolate them behind adapters and detect schema drift explicitly.

**REINFORCE:** Evidence Before Architecture.

---

## 3. Runtime snapshots and durable event history are different things

Live state such as:

- context percentage;
- current model;
- active branch;
- current worktree;
- quota remaining;
- dirty/clean tree;

is a snapshot concern.

Historical facts such as:

- handoff created/resumed;
- compaction occurred;
- review completed;
- blocked/unblocked;
- retrieval happened;
- CI failed/recovered;

may be event/history concerns.

Do not force both into one storage model merely for dashboard convenience.

**ASSESS:** Minimum event history required to answer useful re-entry and effectiveness questions.

---

## 4. Metrics need semantics, provenance, and quality boundaries

A number is not meaningful merely because it can be measured.

Examples:

- context bytes saved do not prove better task performance;
- archive retrieval rate is not inherently good or bad unless retrieval was required;
- model usage percentages from different providers may describe different quota systems;
- review count is not review quality;
- token count is not context health.

Every derived metric should identify:

- source;
- time/freshness;
- calculation;
- interpretation boundary;
- whether it is direct fact, derived fact, or inference.

**REINFORCE:** evidence classification and minimal provenance.

---

## 5. Retrieval observability is more useful than merely adding retrieval instructions

Worktrunk reports an internal observation that sessions were consuming only 26% of references required by the actions they performed, and responded by moving retrieval cues closer to the action. PMB has separately observed pointer-present / archive-not-fetched behavior in its own baseline work.

The converging lesson is not “load more references at startup.” It is:

> Measure whether the needed reference is actually retrieved at the point of use, then place the cue where the action occurs.

**ASSESS:** retrieval instrumentation and action-local routing cues.

---

## 6. Local dashboards are security boundaries

A cockpit may expose:

- repository paths;
- branch names;
- prompts or transcript excerpts;
- usage/account information;
- code-review findings;
- local service state.

The moment it binds beyond loopback or accepts browser/network traffic, it requires an explicit threat model.

**REINFORCE:** bounded authority and secure defaults.

---

# PMB implications

PMB repository examined at:

`729137ff270594a5eef3a7ef9ad46c8b1fa466de`

## What PMB already exposes

`mb status` is already a deterministic fast state check with five signals:

1. Initialized
2. Core Memory Present
3. Active Context Current
4. Standards Available
5. Tasks Present

It also emits an Attention section or `0 Issues`.

This is useful cockpit data, but the current command is terminal-oriented text rather than a documented machine-readable contract.

PMB also already has `mb doctor`, lifecycle hooks, handoff/compaction behavior, task contracts, and repository-level governance checks. The correct first question is how to expose those facts safely—not whether sessions should write a parallel dashboard log.

## Recommendation: do not make PMB sessions manually emit dashboard records

Do **not** add standing instructions such as:

> “At the end of every session, append dashboard telemetry.”

That would create:

- model-owned telemetry;
- extra context/process overhead;
- duplicate state;
- missed events when sessions stop unexpectedly;
- drift between PMB truth and dashboard records.

Instead prefer deterministic collection.

### First PMB interface candidate — only when a consumer exists

If a Dashboard project is authorized, the cleanest PMB change to evaluate is:

```text
mb status --json
```

or an equivalent stable machine-readable snapshot.

Candidate fields:

```text
schema_version
pmb_version
initialized
core_memory_present
active_context_status
active_context_last_reviewed
standards_status
active_task_count
handoff_present
handoff_age
attention[]
checked_at
```

This is **not an implementation request yet**. It is an interface candidate owned by PMB if/when an actual external consumer exists.

## What should come from the host instead of PMB

The following should not be recreated by PMB if the host already supplies them:

- model identity;
- context-window occupancy;
- five-hour / seven-day usage windows;
- session ID;
- working directory;
- compaction lifecycle hook events where directly available.

## What may require later instrumentation

Two PMB-relevant questions may eventually require explicit events if deterministic inference proves unreliable:

1. Was an archive/reference retrieval **required**, and was it actually performed?
2. Did a handoff/resume preserve the intended ephemeral state without causing routing regressions?

Do not add event logging for these until the PMB pilot establishes the measurement need and the least invasive collection point.

---

# PARK — Dashboard / AI Engineering Cockpit

## Concept

A separate local developer tool that provides one place to see the operational state of AI-assisted engineering work across projects and AI hosts.

Possible views:

### Sessions

- host: Claude Code / Codex / OpenCode / other;
- repo;
- branch;
- worktree;
- model;
- session age / last activity;
- current context occupancy;
- current attention state.

### Usage

- host/provider usage windows;
- reset times;
- current pace;
- API/token/cost data where the source exposes trustworthy values.

### Repository state

- dirty/clean;
- current SHA;
- ahead/behind;
- active worktrees;
- CI / PR state;
- unresolved review findings.

### PMB

- `mb status` snapshot;
- `mb doctor` summary when explicitly requested;
- active-context freshness;
- task-contract state;
- handoff presence/staleness;
- retrieval/continuity measurements if later instrumented.

### ACR

Potential future adapter only; examples:

- latest run;
- findings by severity;
- VERIFIED / INFERRED / SPECULATIVE distribution;
- false-positive / calibration metrics;
- model/fixture comparison results.

### Re-entry

Grounded “since you left” information built from Git, CI, review, session, and PMB facts.

---

## Boundary

The Dashboard should **not** become:

- a second Memory Bank;
- an orchestration engine by default;
- a transcript warehouse;
- an autonomous agent;
- an LLM-summary machine;
- the owner of Git/PMB/ACR truth;
- cloud infrastructure unless a later need justifies it.

Initial bias, if built:

- local-first;
- read-only;
- deterministic collectors;
- adapter architecture;
- no mandatory database until history requirements justify one;
- no AI calls for facts ordinary code can collect.

---

## When to start a separate Dashboard project

Do **not** start a new repository solely because the UI idea is appealing.

Start it when all of the following are true:

1. **At least three useful source adapters are real and available**, for example Claude Code runtime/statusline + Git/worktree + PMB status.
2. **A repeated operational question exists** that currently requires manually checking multiple tools, such as “what needs my attention?”, “where am I close to context/quota pressure?”, or “what changed while I was away?”
3. **The required fields can be written down as a small stable contract** without inventing model-generated state.
4. **The first version can remain read-only and local.**
5. **The PMB pilot has not revealed a more important foundational defect that should be fixed first.**

A one-off screenshot/dashboard desire is insufficient justification.

### Suggested MVP if those conditions become true

One local page with:

- current sessions/projects;
- context + usage pressure;
- repository/worktree state;
- PMB health/attention;
- one “since you left” grounded delta view.

No orchestration, no agent control, no auto-remediation in MVP.

---

# Decision for now

**PARK:** Separate Dashboard / AI Engineering Cockpit project.

**REINFORCE:** Source-owned structured telemetry, adapter isolation, schema-drift detection, minimal provenance, deterministic facts first.

**ASSESS:** PMB machine-readable status contract when an external consumer exists; retrieval/handoff measurement during PMB pilot.

**REJECT:** Adding manual “dashboard logging” obligations to PMB sessions now.

**REJECT:** Building dashboard functionality inside PMB or HE.

---

## Revisit triggers

Revisit the parked Dashboard concept when any of these occur:

- PMB pilot produces useful recurring telemetry that has no convenient view;
- ACR begins producing stable machine-readable run/calibration outputs worth aggregating;
- Claude/Codex usage/context monitoring repeatedly requires manual checking;
- multiple concurrent worktrees/sessions make re-entry and attention management painful;
- the user explicitly decides the cross-project operational view is worth maintaining as its own product.

## Status

Research synthesis complete.

No PMB implementation changes made.
No Dashboard repository created.
No HE architecture decision created.
