# AI Engineering Telemetry & Dashboard Repos — Deep Evidence Pass — 2026-09-18

## Purpose

Preserve the source-level evidence mined from the dashboard, session-runtime, context-management, worktree, and review repositories examined during the September 18 research pass.

This note is an evidence record, not an adoption decision. The associated Harness synthesis is:

- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`

The dashboard/cockpit concept is currently **PARKED** as an adjacent project candidate. It is not Harness Engineering architecture and it is not a PMB feature request.

## Evidence method

The review went below README-level claims and inspected source trees, runtime collectors, parsers, schemas, analytics code, statusline integrations, hooks, security boundaries, and machine-readable outputs where available.

Version-sensitive findings are tied to the exact commits below so later repository changes do not silently rewrite the evidence base.

---

## Repositories and pinned revisions

| Repository | Pinned revision | Primary evidence signal |
|---|---|---|
| `SuperLogicAI/Logic-Loop` | `bfad04e53342e054a5a43953a63de1a9cf44a539` | Multi-source session telemetry, transcript parsing, attention/decision state, Codex/Claude usage collectors |
| `mksglu/context-mode` | `6f0cc6841c687e754059f36714a11233fda1a02b` | Context-savings analytics, event store, continuity/retrieval/cache metrics |
| `max-sixty/worktrunk` | `d92b628d02749e83926638f9a73c6e7281e3d1ad` | Machine-readable worktree/statusline state; Claude context/rate-limit integration |
| `sonpham-org/claude-dashboard` | `1f6c8f14332ff7432805e531725fb21cb6c4cadf` | Hook-driven live session state, WebSocket UI, session panes/status |
| `CaptainASIC/reckoner` | `8a5d5b0d77f0461abf98e61709cf02d94c63fddb` | Provider credit/usage adapters and dashboard presentation |
| `trainingsites/campus-ai-os` | `509318d2ecf2ceb7c93c39fb998946ef584f4ae6` | Session-capture schemas, outcome rollups, doctor/report patterns |
| `alibaba/open-code-review` | `7a571b78d3493b249f6ad14d835c6a79a0a67d2e` | Per-session JSONL history, local viewer, explicit viewer/trust-boundary security model |
| `addyosmani/agent-skills` | `c004a74784a08295d52749b04cda634125b9a581` | Cross-host skill packaging and command surfaces; useful ecosystem context, little unique telemetry signal |
| `affaan-m/ECC` | `c752aac18616e26bf146f034a86947d8f6fc207e` | Cross-host skill mirrors and validated surfaces; useful ecosystem context, little unique telemetry signal |
| `Codpal-Limited/deckgauge` | `2228cf308a2d25da348beefe8e7fa50c367d3dcc` | Dashboard/scoring candidate; insufficient unique source-level telemetry signal in this pass to justify a stronger conclusion |

Associated video / scout source:

- “Top 10 Repos explained: ADHD, Ponytail, and more” — The Next New Thing
- https://www.youtube.com/watch?v=aX8Y183qDpY

The video is a discovery source only. Repository implementation is the evidence source for the findings below.

---

# 1. Logic Loop

Repository:

https://github.com/SuperLogicAI/Logic-Loop

Pinned commit:

`bfad04e53342e054a5a43953a63de1a9cf44a539`

## Source-level findings

The repository contains dedicated collectors/checks for concerns including:

- Claude statusline data;
- Codex usage metering;
- model traffic;
- transcript ingestion;
- attention state;
- decisions;
- worktrees;
- branch/diff/delta state;
- re-entry / “what changed” state;
- blockers and session state.

The interesting architecture is not the visual dashboard. It is the **adapter layer that turns several unstable external formats into a common local state model**.

Its transcript processing handles Claude and Codex formats and maintains session-specific parsing state. Importantly, the parser includes a **schema-drift tripwire** rather than assuming transcript envelopes remain stable forever. That is a durable pattern for any system that derives facts from vendor-owned transcript formats.

## Mine

- isolate each vendor/runtime parser behind an adapter;
- explicitly detect unknown/changed transcript envelopes;
- fail visibly rather than silently producing plausible-but-wrong telemetry;
- distinguish direct runtime facts from inferred attention/decision state;
- keep “since you left” / re-entry information as a derived view over authoritative facts rather than durable project truth.

## Do not import blindly

- transcript parsing is a brittle integration surface;
- inferred “attention” or “decision” state is weaker evidence than hook/runtime state;
- do not make a dashboard parser an authority over project state.

---

# 2. Context Mode

Repository:

https://github.com/mksglu/context-mode

Pinned commit:

`6f0cc6841c687e754059f36714a11233fda1a02b`

Key implementation area:

- `src/session/analytics.ts`
- `src/server.ts`
- session/event-store and retrieval-marker code

## Source-level findings

Context Mode has a materially richer analytics model than its headline “save context” pitch suggests.

Its analytics types include:

- raw bytes processed;
- bytes entering context;
- bytes kept out of context;
- context-savings percentage / ratio;
- per-tool savings;
- cache hits, misses, hit rate, and bytes saved;
- session uptime;
- session event counts;
- compaction / snapshot counts;
- continuity readiness;
- project-memory event counts;
- retrieval bytes;
- tool calls and concurrency;
- per-category session events;
- persistent index state.

It also distinguishes **runtime stats** from **persisted session/project events**, which is important. Not every useful metric belongs in one storage model.

Its event categories include files, working directory, rules, requests, intent/goal, constraints, skills, subagents, decisions, rejected approaches, external references, git operations, tasks, errors, compaction/resume events, cache, latency, plans, and blockers.

## Mine

- separate live/runtime measurements from durable event history;
- maintain a typed report object rather than letting UI code scrape prose;
- preserve provenance for computed savings metrics;
- instrument retrieval/omission systems so their claimed benefit can be measured;
- keep context-efficiency metrics explicitly distinct from task-quality metrics.

## Do not import blindly

- “bytes saved” is not proof of better outcomes;
- a large event taxonomy can become its own maintenance burden;
- PMB should not become an event database merely because Context Mode has one.

---

# 3. Worktrunk

Repository:

https://github.com/max-sixty/worktrunk

Pinned commit:

`d92b628d02749e83926638f9a73c6e7281e3d1ad`

Key implementation area:

- `src/commands/statusline.rs`

## Source-level findings

Worktrunk exposes the current worktree as JSON using the same structure as `wt list --format=json`.

Its Claude Code statusline integration parses host-supplied JSON for:

- current working directory;
- model display name;
- `context_window.used_percentage`;
- five-hour usage/rate-limit state;
- seven-day usage/rate-limit state;
- reset timestamps.

This is a strong pattern because the statusline consumes **host-provided structured facts** rather than asking the model to estimate its own state.

Worktrunk also implements a rate-limit pace model. That is interesting, but its priors are explicitly described as calibrated “by feel” pending real traces. Treat the prediction mechanism as experimental, not a portable truth.

A current Worktrunk commit message reports an internal observation that sessions were reading only **26% of the references their actions required**, and the project responded by moving retrieval cues closer to the actions that needed them. This is project-reported evidence, not an independently reproduced benchmark, but it is highly relevant to PMB’s own archive-pointer/retrieval measurements.

## Mine

- prefer structured host/runtime facts over model self-report;
- expose Git/worktree state in machine-readable form once, then let statuslines/UIs consume the same collector;
- put retrieval cues close to the action that needs the reference;
- measure retrieval behavior rather than assuming a pointer is enough.

## Do not import blindly

- do not adopt the rate-limit probability model without local traces and calibration;
- do not respond to poor retrieval by loading all references globally.

---

# 4. Claude Dashboard

Repository:

https://github.com/sonpham-org/claude-dashboard

Pinned commit:

`1f6c8f14332ff7432805e531725fb21cb6c4cadf`

## Source-level findings

The useful pattern is **hook-driven live state → local server → WebSocket/UI**.

The frontend is organized around session panes, status badges, split/tab state, session hooks, and live updates. This is a cleaner presentation pattern than deriving the entire UI from transcript replay.

The current repo also supports remote access and changed its server binding to `0.0.0.0` by default/configuration in the pinned revision. That is a reminder that a developer cockpit can become a security boundary the moment it stops being loopback-only.

## Mine

- hooks are a strong source for lifecycle events when the host provides them;
- live UI and durable history do not need to share one storage mechanism;
- session identity should survive UI layout changes;
- notifications should derive from explicit state changes where possible.

## Do not import blindly

- a future local cockpit should default to loopback/local-only access;
- do not expose transcripts, prompts, paths, or repository metadata on a network listener by default.

---

# 5. Reckoner

Repository:

https://github.com/CaptainASIC/reckoner

Pinned commit:

`8a5d5b0d77f0461abf98e61709cf02d94c63fddb`

## Source-level findings

The useful pattern is **provider-specific collectors behind a shared presentation model**. Provider quotas/credits/usage do not have one universal API or meaning, so provider logic belongs behind adapters rather than leaking into the dashboard core.

## Mine

- provider usage is an adapter concern;
- normalize only the dimensions that are truly comparable;
- preserve raw provider semantics alongside normalized display values;
- treat scraping/undocumented endpoints as fragile and replaceable.

## Do not import blindly

- do not make a dashboard dependent on one provider’s undocumented usage endpoint;
- do not pretend different provider “percent used” values describe identical quotas.

---

# 6. Campus AI OS

Repository:

https://github.com/trainingsites/campus-ai-os

Pinned commit:

`509318d2ecf2ceb7c93c39fb998946ef584f4ae6`

Relevant areas reviewed include session capture, context-record schemas, outcome reporting/rollups, and doctor/report formatting.

## Mine

- typed context/session records can decouple collection from presentation;
- rollups should be derived from underlying records rather than hand-maintained summaries;
- a “doctor” style report is useful for deterministic health facts;
- reports should distinguish missing, unknown, warning, and failed states rather than compressing everything into one score.

## Do not import blindly

- do not create schemas merely because another project has them;
- a dashboard should consume existing authoritative facts before asking every source system to adopt a new event model.

---

# 7. Open Code Review

Repository:

https://github.com/alibaba/open-code-review

Pinned commit:

`7a571b78d3493b249f6ad14d835c6a79a0a67d2e`

## Source-level findings

Open Code Review persists each review session to its own JSONL history and provides an optional local web viewer.

Its security assurance case explicitly models trust boundaries between:

- repository/diff input;
- the review CLI;
- the LLM provider;
- local output;
- the browser/viewer.

The viewer includes explicit host-header protections against DNS rebinding and other browser-facing controls.

## Mine

- append-only/session-scoped event files are a simple alternative to a database for bounded histories;
- viewer security is part of the architecture, not polish;
- local dashboards should have an explicit threat model once they expose code/session metadata;
- machine-readable findings/history are more reusable than prose-only reports.

## Do not import blindly

- JSONL is appropriate for some histories but not automatically the right common storage layer for a cockpit;
- review history belongs to the review system; a dashboard should consume it, not own it.

---

# 8. Agent Skills / ECC

Repositories:

- https://github.com/addyosmani/agent-skills — `c004a74784a08295d52749b04cda634125b9a581`
- https://github.com/affaan-m/ECC — `c752aac18616e26bf146f034a86947d8f6fc207e`

## Finding

These repos are useful evidence about distributing similar capabilities across Claude, Codex, Gemini, OpenCode, and related hosts, but this pass did not identify a unique dashboard/telemetry mechanism that should be promoted over the stronger signals above.

The relevant cockpit lesson is simply that **host adapters are a permanent design concern**. Do not assume Claude Code is the only runtime.

---

# 9. DeckGauge

Repository:

https://github.com/Codpal-Limited/deckgauge

Pinned commit:

`2228cf308a2d25da348beefe8e7fa50c367d3dcc`

## Finding

The project remains a dashboard/scoring research candidate, but the available source evidence in this pass did not establish a distinctive telemetry mechanism strong enough to carry into HE synthesis.

**Disposition:** retain the source pointer; do not invent a finding merely because the UI is interesting.

---

# Cross-repository evidence

The strongest recurring architecture is:

```text
source-owned facts
    ↓
adapter / collector
    ↓
normalized local snapshot or event
    ↓
derived views / attention rules
    ↓
UI
```

This is preferable to:

```text
transcript
    ↓
LLM summarizes everything
    ↓
dashboard
```

The latter is more expensive, less deterministic, and can quietly turn model inference into apparent system truth.

## Source authority hierarchy

Preferred order for a future cockpit:

1. host/runtime structured data;
2. source-system machine-readable commands/files/APIs;
3. hooks/events emitted by the source system;
4. stable repository/Git facts;
5. transcript parsing as a compatibility adapter;
6. model inference only for genuinely semantic summaries.

## Evidence durability

Because public repos change, all version-sensitive findings above are pinned to commits. If a repository later changes or disappears, the HE note retains the exact revision reviewed and the mechanism-level findings.

No third-party source code was copied into HE.

## Status

Research only.

No PMB, ACR, or Harness implementation changes authorized by this note.
