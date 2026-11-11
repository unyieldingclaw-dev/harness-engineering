# AI Engineering Cockpit — Home and Work Multi-Provider Scope — 2026-11-10

**Status:** User has explicitly requested a dashboard similar to Mansel Scheffel's Claude Command Centre **for both Home and Work**. This is a proposed product direction, not approval to install software or capture workplace data. Research / product-boundary note only.
**Placement:** Independent adjacent application, **not** PMB, ACR, or Harness Engineering runtime.
**Prior HE ownership decision:** [AI Engineering Observability & Dashboard Boundary — 2026-09-18](./AI%20Engineering%20Observability%20%26%20Dashboard%20Boundary%20%E2%80%94%202026-09-18.md) expressly parked the cockpit until a cross-project operational view was requested and useful adapters were available. The user-request trigger is now present; feasibility and work permissions are not yet validated.

## Research preservation and resumption

- [GBrain / StarMynd evidence assessment](./Sources/GBrain%20%26%20StarMynd%20%E2%80%94%20Memory%20Systems%20Evidence%20and%20Transfer%20Assessment%20%E2%80%94%202026-11-11.md) — facts, source limitations, transfer decisions and explicitly rejected architecture.
- [Post-PMB Resumption Checklist](./AI%20Engineering%20Cockpit%20%E2%80%94%20Post-PMB%20Resumption%20Checklist%20%E2%80%94%202026-11-11.md) — operational questions, provider capability gates, Home/Work separation, stop conditions and non-goals. **PMB completion is not permission to implement.**

## Decision and explicit boundaries

- **One versioned product/codebase**, provisioned as separate **Home** and **Work** instances. Shared code does not mean shared telemetry, identity, credentials, database, logs, browser storage, or endpoint.
- **Home** can run locally on the user's Windows development computer. **Work** is conditional on T-Mobile policy, IT/security approval, and permitted distribution and execution. If a custom local application is prohibited, reuse only the approved design/concepts in existing enterprise tooling.
- **No transfer** of work prompts, repository names/paths, branch metadata, corporate identifiers, credentials, logs or usage records to a personal instance or personal GitHub repo. A dashboard’s UI does not create authorization to extract company data.
- The dashboard **consumes source-owned facts**. Git, CI, PMB, ACR, vendor clients, and native billing/usage systems remain the authorities.
- Default **read-only**. No agent launcher, permissions broker, scheduler, autonomous remediation, persistent transcript archive, RAG/vector database, mandatory cloud service, or cross-provider prompt relay in the first release.
- Do not change HE/PMB/ACR runtime or company settings on the strength of this video.

## Motivation and scope

The user has a genuine desired outcome: one accessible view of development-tool activity and engineering posture across **Claude, Cursor, Gemini, Codex** at Home and at Work. The concept's historical readiness gates are not automatically passed by desire; before coding, verify at least two genuinely available provider surfaces, a recurring operational question that can be answered, and a data-minimal local deployment.

### Proposed MVP (not implementation approval)

1. **Overview:** source freshness, per-provider coverage, session activity where supported, failure/error counts, and links to native detail views.
2. **Usage and limits:** source-reported tokens, cache reads/writes, rate-limit windows and reset times where present; clearly distinguish subscription limits, API-billed consumption, model counts, and inferred estimates.
3. **Reliability:** API/tool request errors, rate limits, retries, latency and recurring failure signatures where telemetry exposes them; preserve event source and count-window.
4. **Security and context posture:** deterministic, user-initiated checks for credential-bearing config risk (including `.claude/settings.local.json`, `.mcp.json`, shell history / `.env` exposure risks), permission-mode hazards, oversized instruction/skill descriptions, and duplicate MCP registrations **only when supportable by local read-only evidence**.
5. **Project state:** Git/worktree state, optionally `mb status` / `mb doctor` outputs and ACR machine-readable reports, as source-identified separate adapters.

Defer task queues, remote agents, scheduled operation, notifications, automatic memory promotion and cross-provider routing until a measured need appears and authorization is separately established.

## Provider compatibility and evidence (as of 2026-11-10)

| Surface | Viable ingestion | Important limitation |
|---|---|---|
| Claude Code CLI / Desktop Code | Native opt-in OpenTelemetry metrics/events; host structured status/usage where exposed | OTel must be enabled/configured; Desktop Code is not the same monitoring configuration as Cowork. Metered tokens and billable amounts require precise semantics. |
| Claude Cowork | Organization-configured OpenTelemetry available on supported organization accounts | Admin-controlled and separate from Code. Content/privacy policy and software-version variation matter. Personal account access unverified. |
| Gemini CLI | First-party opt-in OpenTelemetry; local output file supported | Disabled by default; `logPrompts` defaults true when telemetry enabled unless explicitly set false. Non-CLI Gemini surfaces may differ. |
| OpenAI Codex CLI | First-party opt-in OpenTelemetry logs/events and configurable metric/trace exporters in supported clients | Export off by default, `log_user_prompt=false` default. Does not imply ChatGPT app telemetry access. |
| Cursor | Team dashboard / CSV where eligible; Enterprise Analytics API; Enterprise server-side OTLP export | Full OTel export is Enterprise-only and **pushes from Cursor servers to an HTTPS collector**, not to an arbitrary loopback port on a local laptop. Individual-plan local usage API cannot be assumed. |
| Git / PMB / ACR | Source-local deterministic commands or stable machine-readable reports | Never require PMB or ACR to log new events merely to feed a dashboard. |

Sources:
- Claude Code: https://code.claude.com/docs/en/monitoring-usage
- Claude Cowork: https://support.claude.com/en/articles/14477985-monitor-claude-cowork-activity-with-opentelemetry
- Codex: https://learn.chatgpt.com/docs/config-file/config-advanced
- Gemini CLI: https://geminicli.com/docs/cli/telemetry/
- Cursor: https://prod.cursor.com/docs/enterprise/opentelemetry-export
- Cursor analytics: https://cursor.com/docs/account/teams/analytics
- Cursor Analytics API: https://prod.cursor.com/docs/account/teams/analytics-api

**Critical correction to video-style displays:** a 13.8M token count attributed to a skill/session does not establish that the skill's instructions consumed 13.8M tokens. Distinguish *session associated with a skill* from *incremental instructions loaded by a skill*. Similarly, a large **deferred** MCP tool schema inventory does not equal active context already consumed. Never conflate a hypothetical retail token valuation, paid subscription limit, and actual invoice spend.

## Minimal source contract

For each observation, preserve at least:
- `environment` (Home/Work, local-only) and `provider` / `surface`;
- `source_kind`, source version/schema, retrieved-at and event-at timestamps;
- `metric`, `value`, `unit`, measurement interval, and aggregation grain;
- `confidence_class` = direct / derived / inferred and explanatory limitations;
- capability and freshness state = available / unavailable / permission-blocked / stale / error;
- optional bounded run/project identifiers **only when not sensitive**.

**Unavailable ≠ 0.** No inferred field should silently become a fact. Avoid general-purpose identity federation and a single universal provider model: field-level capabilities vary across vendors.

## Engineering shape

Start with an uncomplicated local, read-only web view backed by deterministic adapters and a small stable JSON contract. Prefer a local-only Python service / static frontend if building a dedicated UI; add SQLite **only if** the history/trend questions genuinely require it. Bind to `127.0.0.1`, no cloud endpoint, no external analytics, no ambient prompt-capture, and no exposed receiver by default. If an OTLP Collector becomes justified, make it a separate opt-in component, not a requirement for every installation.

The **first engineering spike** should prove two provider adapters and one actual repeated question; choose native source inspection or bounded synthetic fixtures if live telemetry cannot be obtained. Before implementing GUI affordances, independently reconcile one small sample with native provider counters, test absent/partial data, and test a clean uninstall/disable. Avoid scorecards with arbitrary magic thresholds (e.g., automatic red grades for skill file length) until justified by observed outcomes.

## Security and separation

- **Never enable logging of prompts, responses, source files, tool parameters, or authentication headers solely to improve a dashboard.** Some metadata/error messages contain sensitive data, so filter at source and verify redaction.
- Personal data and corporate data must not share destination, collector, local database, logs, file uploads, or cloud backup.
- Work installation, telemetry enablement, local auditing and software supply-chain ingestion **require employer authorization**. Do not install personal collector/agent onto corporate hosts, and do not point enterprise Cursor export to personal infrastructure.
- Read-only config audit must not itself print credentials; report file location and finding type with safe redaction. Include Claude approval allow-rule risk where `.claude/settings.local.json` records commands containing secrets.
- No source-code writes, settings auto-fixes, agent dispatch, or scheduled jobs in MVP.
- Retention should be short and configurable; default to metadata counts, no full transcripts, and no project names where they are sensitive.

## Proposed staged validation

**Stage 0 — Capability inventory:** confirm actual installed tools/versions on Home, supported collection surfaces, policies and approvals on Work. Inspect existing HE/PMB/ACR JSON/CLI contracts. Output capability matrix with `unknown` fields rather than guesses.

**Stage 1 — Home proof:** one local view with at least two real source adapters, source freshness, usage/reliability and redaction checks. Validate against native provider figures; no work data.

**Stage 2 — Other home providers:** add only where surfaced data is accessible and correct. Cursor may remain CSV/manual. Add project/git/PMB/ACR only if they answer a real question.

**Stage 3 — Work feasibility:** separately assess approval and available enterprise endpoints. Deploy same codebase only if sanctioned and isolated; otherwise choose the employer's existing observability stack.

**Stage 4 — Optional deeper views:** MCP/skills audits and alerts only for repeated confirmed problems. Approval/task/dispatch functionality is **not** part of the agreed observability scope.

## Acceptance criteria and stop conditions

Prove each displayed metric's owner, granularity, provenance and freshness; no invented model costs or quota resets; don't misreport missing telemetry as health. Verify no credential, prompt or work data escapes the instance. Uninstall/disabling the dashboard must not damage host tool settings. If provider integration needs unsupported reverse engineering, privileged export, an externally reachable service, or persistent high-maintenance hooks for marginal benefit, stop and use a native provider view instead.

## Current status

**Requested direction established; implementation not started.** No Home or Work agent/collector configured; no workspace data collected or exported. This note updates the historical parked research without rewriting the September 18 decision or silently treating a proposed MVP as approved implementation.
