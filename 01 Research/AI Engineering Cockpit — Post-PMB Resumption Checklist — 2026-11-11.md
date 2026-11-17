# AI Engineering Cockpit — Post-PMB Resumption Checklist — 2026-11-11

**State:** **PARKED — research / decision checklist only.** The user requested one future **Multi-Provider AI Engineering Dashboard** for Claude, Cursor, Gemini and Codex, in **separate Home and Work deployments** from one maintainable codebase, once PMB close-out is complete. User demand is established; feasibility and implementation authorization are **not**.

**Canonical context:**
- [2026-09-18 HE Observability & Dashboard Boundary](./AI%20Engineering%20Observability%20%26%20Dashboard%20Boundary%20%E2%80%94%202026-09-18.md)
- [2026-09-18 source-level telemetry repo mining](./Sources/AI%20Engineering%20Telemetry%20%26%20Dashboard%20Repos%20%E2%80%94%20Deep%20Evidence%20Pass%20%E2%80%94%202026-09-18.md)
- [2026-11-10 Home/Work Cockpit Scope](./AI%20Engineering%20Cockpit%20%E2%80%94%20Home%20and%20Work%20Multi-Provider%20Scope%20%E2%80%94%202026-11-10.md)
- [Mansel source research in HE draft PR #28](https://github.com/unyieldingclaw-dev/harness-engineering/pull/28)
- [GBrain / StarMynd evidence assessment](./Sources/GBrain%20%26%20StarMynd%20%E2%80%94%20Memory%20Systems%20Evidence%20and%20Transfer%20Assessment%20%E2%80%94%202026-11-11.md)

## Hold conditions — not automatic triggers

- [x] Capture the separate Home/Work, four-provider user request and explicit exclusions.
- [x] Reconcile existing HE observability research and preserve GBrain/StarMynd/Mansel evidence.
- [x] Make a resumption checklist findable from the proposed Dashboard scope.
- [ ] PMB release/close-out is demonstrably stable by PMB's own acceptance criteria. **Do not** redirect PMB development, standards, CI, memory files or release gates for Dashboard purposes.
- [ ] User explicitly chooses to resume Dashboard Stage 0. Completing PMB **does not** itself authorize coding or instrumentation.

**Separate PMB preservation note, not a Dashboard dependency:** closed PRs [#83](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/83) and [#95](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/95) retain full research on branches; [#140](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/140) merged a backlog summary. Decide on durable off-startup archival of both before deleting branches. [#98](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/98) has unmerged deferred changelog/test fixes: compare each against future main rather than cherry-picking wholesale. None of these authorize a Dashboard feature.

## Stage 0 — feasibility and go/no-go, no implementation

- [ ] Re-read HE main, open research PRs and provider documentation *at that time*. Reconcile any changed decisions; don't silently assume draft notes merged.
- [ ] Name **one or two repeated operating questions** native tools fail to answer well (e.g., “Which provider is limiting this session?” or “Is displayed usage missing/stale?”). Record a real example and what decision improves.
- [ ] Inventory **installed/eligible Home surfaces**, versions, plan restrictions and available safe local data. Never infer that all four providers offer equivalent telemetry.
- [ ] Revalidate source capability matrix with **available / unavailable / unknown / permission blocked**, provenance, granularity, timestamps and update cadence for each field:
  - Claude Code: native usage/limit surfaces and opt-in OTEL where supported.
  - Codex CLI: opt-in logs/OTEL where supported; separate from ChatGPT app data.
  - Gemini CLI: opt-in telemetry; check prompt-logging defaults and disable content collection before any testing.
  - Cursor: individual/team CSV or API only where permitted; **Enterprise OTLP is server-to-reachable-HTTPS-collector**, not a local desktop feed.
- [ ] Evaluate **build vs extend an existing tool vs use native dashboards** against actual questions, privileges, maintenance burden and incremental value. No dashboard is a valid result.
- [ ] Confirm Home source-local permission boundaries and safe retention. Create **no** cross-Home/Work data flow, personal proxy for work data, host-global config edits, or mandatory always-on collector.
- [ ] For **Work**, separately determine corporate permission for software execution, telemetry configuration, storage, transport and publication. If approval/access is missing, mark Work **blocked**, not “coming soon”; do not test on corporate machines or export identifiers to Home.
- [ ] Record a one-page decision: observed problem, native alternative, two usable source adapters (or none), minimal operator questions, security boundary, stop criteria, test plan, owner, and explicit **go/no-go**.

**Stop at Stage 0** if fewer than two real useful data sources are legally/technically accessible; if a native UI answers the problem; if supporting the result demands account scraping, unsupported APIs, broad secrets access or an exposed receiver; or if the value is not worth ongoing integration maintenance.

## Stage 1 — only after a separate explicit go decision

- [ ] Define a tiny **source-owned JSON observation** contract: environment, provider/surface, metric and units, event/retrieval times, source/schema version, confidence (direct/derived/inferred), availability state, observation window and bounded safe identifiers. **Unavailable is not zero.**
- [ ] Implement **two** provider adapters with read-only, deterministic inputs and native evidence cross-checks. Favor on-demand local reads; no inference from transcripts and no model-driven ingestion.
- [ ] Show only an **attention-first** Home view: data/source availability, freshness, meaningful failures/limits, current usable evidence and a route to native details. Add a field only if it changes a decision.
- [ ] Test fixtures for unavailable/permission denied, stale vs zero, partial windows, failed parser, provider schema drift, timestamp conflict, duplicate event, secret-bearing error message and “inferred cost” mislabeled as actual spend.
- [ ] Confirm bind to loopback only, no external analytics or prompt capture, safe redaction, no accidental file/log persistence; disabling/uninstalling has no effect on Claude/Cursor/Gemini/Codex settings.
- [ ] Run during several real Home workflows; record whether it answered the chosen questions faster or more reliably than native tools. If not, retire the spike.

## Stage 2 — scope expansion needs new evidence

- [ ] Add a third/fourth provider **only** if a supported native surface exists. A manual Cursor CSV import may be better than permanent integration; label manual freshness explicitly.
- [ ] Optionally link Git/worktree state, `mb status`/`mb doctor`, or ACR result artifacts as **read-only external adapters**. No forced event schema, file edits, hooks or telemetry tasks in PMB/ACR.
- [ ] Add history or SQLite **only** for a measured trend question that cannot be answered from bounded source reads; justify storage, retention, cleanup and portability.
- [ ] Re-evaluate Work as an **independent security/approval decision**. Same maintainable codebase does not imply shared configuration, credentials, collectors, stores, identifiers, logs or release permissions.

## Explicitly optional / rejected for MVP

| Item | Disposition |
| --- | --- |
| Session/source-health, provider availability, usage/limit data where native | **Candidate** — verify semantics and access first. |
| Security/context posture checks (including `.claude/settings.local.json` plaintext allow rules) | **Optional** user-initiated, read-only, credential-safe checks; avoid unsupported claims about content or enforcement. |
| Historical trends, notifications, skill usage attribution | **Defer** until real observed questions and accessible events; never equate skill-associated session tokens with skill-instruction tokens. |
| Built-in PMB/ACR features, retrieval memory, persistent transcript store, universal provider abstraction | **Reject as MVP**; violates ownership/minimization or creates unjustified architecture. |
| Autonomous orchestration, automatic remediation, model routing, task dispatch, permissions brokerage, agent factory | **Out of scope**, separate product decision if ever requested. |
| Cross-company telemetry collection, work data to personal infrastructure | **Prohibited without corporate authorization**; not an extension of Home. |

## End-of-resumption decision record (fill in when reopened)

- Date, Home/Work scope and source inventory:
- Repeated operational problem and native alternative:
- Capability matrix / provider references and evidence freshness:
- Work permission status (allowed, prohibited or unknown):
- Two-adapter proof and acceptance/failure results:
- Lifecycle maintenance and security costs:
- Decision: **no build / small Home proof / sanctioned Work evaluation**
- Explicit user authorization and next owner:

**No resumed work, provider configuration, install, data collection or PMB changes were authorized by preserving this checklist.**
