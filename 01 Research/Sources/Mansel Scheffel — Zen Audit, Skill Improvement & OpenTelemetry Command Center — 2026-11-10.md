# Mansel Scheffel — Zen Audit, Skill Improvement & OpenTelemetry Command Center — 2026-11-10

**Status:** Preliminary research; screenshot/video-description intake only; awaiting full dashboard build prompt and separate Security/Context video review. **Not a proposed build.**  
**HE decision:** Reuse existing evidence/ownership/observability principles; do not construct a second PMB health system, cross-provider observability platform, or automatic skill-rewriting engine.

## Source boundary

User provided screen captures, descriptions and chapters from three videos:
1. [This One Skill Cleans Up Your Entire Claude Setup](https://www.youtube.com/watch?v=Rf8VeB0keCc) — Zen inventory, broken/stale/risky/clutter findings, item-level Keep/Do it/Change/Ask, approved changes, before/after and undo. Chapters include 00:55, 03:43, 05:23, 06:38.
2. [Your Claude Skills Are Quietly Getting Worse (So I Built This Plugin)](https://www.youtube.com/watch?v=z4dlYIqGjtc) — setup/harvest/refine/decide/apply, event evidence, recurring error clusters, proposed owner-directed corrections, human decision gates, recheck. Chapters 01:07, 02:50, 05:21, 07:00, 08:38, 11:04.
3. [Claude Code + OpenTelemetry = Claude Command Center](https://www.youtube.com/watch?v=dITtLiC9FzM) — Claude Code OTEL, local dashboard views, tasks and schedules, model and skill usage, context/security panels. Chapters 01:43, 04:35, 10:17, 14:59.

**What was not independently observed:** actual running author's code, complete build prompt, reproducible before/after benchmark, security implementation, full video transcripts, host-level permissions and data flow. Screenshot numbers (e.g., ~42K startup tokens, 16 broken findings, ~30K after, caching percentages, 32.2M tokens attributed to one skill) describe the demonstrator's environment, **not measured PMB/HE results**. Do not elevate them to general benchmarks.

## Evidence-backed concepts vs adoption decisions

| Demonstrated concept | Judgment | Narrow HE/PMB interpretation |
| --- | --- | --- |
| Read-only setup census with categorized evidence and source paths | **Useful; mostly existing** | Use `mb doctor`, project/version checks and actual host configurations. Distinguish broken today, risk/exposure, stale assertion, unused/unobserved and aesthetic clutter. A count is not a priority. |
| Keep / Do it / Change / Ask + archived rollback receipts | **Useful** | Human decisions over proposed, owner-specific changes; verify *actual* rollback method instead of promising one undo for arbitrary system changes. |
| Drift and failure clustering (Reddit failures silently reported as no results) | **Strong mechanism** | Treat degraded source as unknown/degraded rather than valid zero; retain source and repeated event details. Do not add a constantly running watcher until a failure class and data source justify it. |
| Harvest -> evidence -> route -> decision -> correction -> recheck | **Existing HE direction, good demonstration** | Evidence is not policy. Owner-directed change only after reproduction/clear evidence. A correction is successful only if fresh-session original-path verification passes without routing regressions. |
| Three M's: megaphone/money/meaning always human-approved | **Good illustration, not a universal taxonomy** | HE already limits authority; use existing change/review ownership and relevant security/business constraints rather than add three global gates indiscriminately. |
| OTEL telemetry and cross-host cockpit | **Technically possible in parts; defer build** | Preserve vendor-native event semantics, source identity and privacy. Expose only comparable metrics; mark unavailable and partial signals rather than invent parity. |
| Automated routing/scheduling/task queues | **No established requirement** | Not PMB's job. First demonstrate a repeated manual task with measurable benefit and an appropriate bounded executor. |
| Token reduction and skill cost ranking | **Diagnostic only** | Screen shows heuristic skill attribution; sessions may be assigned to multiple skills. This is not causal attribution or exact cost saved. Cache hit rate and low initial token count are not outcomes of user tasks. |

## Source-specific cautions that matter

1. **False certainty from diagnostics.** "No calls in 30 days" is evidence of low *observed* use, not proof of dead code or an unneeded fallback. Include event-source coverage, clock/window, unobserved paths, permissions and whether the component may be invoked outside the captured sessions before retirement. Hooks, CI, emergency paths and work-specific integration especially deserve caution.
2. **Security is more than reducing warnings.** Screenshots show a Firecrawl URL containing an API key and broad `Bash(*)` permissions; the narrator's environment is not evidence these settings are appropriate in PMB. Treat credential-in-URL and permissive grants as risk candidates whose effective exposure/authority must be checked, not merely cleaned for appearance. Do not record credential values in findings.
3. **Approval is not verification.** "Approved" permits an owned, scoped modification, not carte blanche to edit siblings, upgrade skills or assume tests pass. Recheck the owning artifact, original failure and collateral behavior.
4. **A green dashboard may lie.** Stale telemetry, incomplete tool instrumentation, dropped events, provider rate limits or a parser that silently drops unknown schemas can report apparently healthy activity. Every result should carry a capture time, source and coverage/degradation indicator.
5. **Privacy boundary.** OTEL events can include command inputs/results, paths, prompt content and connected-service identifiers depending on host/settings. Local collector, opt-in, minimal fields, redaction, retention and network/permission boundaries require evaluation **before** capture. In particular, a local UI is not proof that the capture path never leaves the host.
6. **Automatic self-improvement.** The demo keeps high-impact changes for humans, but one approval must not silently authorize a chain of cross-file edits or recurring collection. The lifecycle belongs to evidence, not an always-growing "AIOS" overlay.

## First-party cross-host facts (checked 2026-11-10)

| Host / surface | Confirmed first-party telemetry capability | Boundary |
| --- | --- | --- |
| Claude Code | [Monitoring](https://code.claude.com/docs/en/monitoring-usage) documents OTEL events/metrics, including tool, skill, hook, token and cost signals. `OTEL_LOG_USER_PROMPTS` disabled by default; some detailed content flags independent. | Verify local version, invocation mode, collection config and exporter; do not treat screenshot of someone else's daemon as this host's setup. |
| Gemini **CLI** | [Gemini CLI telemetry](https://geminicli.com/docs/cli/telemetry/) supports local file or OTLP; telemetry `enabled` defaults **false**. When telemetry is enabled, `logPrompts` defaults **true** unless set false; event/trace payloads need review. | Not equivalent to Google Gemini web app, Gemini Code Assist or all Gemini surfaces. |
| Codex CLI | [Codex configuration](https://developers.openai.com/codex/config-advanced) documents OTEL log export (disabled by default), per-event fields, `log_user_prompt=false` and optional metrics/traces. | OTel log export is separate from built-in anonymous analytics; verify exact installed version/surface and content settings. |
| Cursor | [Cursor Enterprise OTel export](https://prod.cursor.com/docs/enterprise/opentelemetry-export) is **Enterprise-plan server-side**, exporting logs/metrics, no traces or historical backfill. Message/tool payloads are separately opt-in. | Not evidence a consumer/personal Cursor install can provide the same data. Cost is best-effort, not invoices; metrics and per-session logs have different join semantics. |

**No uniform source contract has been demonstrated** across all four. Prefer one source-specific probe at a time, first. Normalize only a field once two providers can actually supply comparable meaning. Explicit `unavailable`, `not_enabled`, `degraded`, `not_supported` are valid results.

## Existing HE/PMB ownership: avoid building duplicate mechanisms

- HE already has `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`. It explicitly parks an independent cockpit, privileges source-owned snapshots/events, defines provenance/degradation requirements and suggests `mb status --json` **only if a consumer is approved**. This work is corroboration, not a new observability architecture.
- HE already has `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`, with transcript-vs-telemetry evidence reduction, repeated-failure analysis, ownership routing, unknown schema handling, approval and fresh verification. Do not duplicate this as new memory governance.
- HE already documented [Mansel context ownership](https://github.com/unyieldingclaw-dev/harness-engineering/pull/28) and [minimal director patterns](https://github.com/unyieldingclaw-dev/harness-engineering/pull/29) on separate drafts; no new director/agent layers follow from this screenshot intake.
- PMB main branch currently has `mb doctor` and `mb status`, hooks, CI, a 5-file memory system, and `docs/LOGGING-GUIDE.md`, `docs/TELEMETRY-GUIDE.md`, `docs/WORK-MB-INVESTIGATION-BRIEF.md`. A new slash skill that reruns these without a specific detection gap is duplication. Portable Work MB briefs explicitly say **inspect the work environment before importing mechanisms**; Work MB itself was not accessed or validated here.
- Concrete **documentation drift candidates** found while reading PMB main (not tested runtime defects): `README.md` version badge says **1.2.0** while `VERSION` says **1.3.0**, confirmed by changelog's archived 1.3.0 release; `docs/TELEMETRY-GUIDE.md` says Gemini CLI telemetry is enabled by default, while the first-party Gemini CLI **OTEL enabled** setting is false by default. Different telemetry/analytics categories may explain part of this, so reconcile terms before editing. They demonstrate why an audit needs source version and exact claim comparison; they do **not** establish an urgent PMB feature defect.

## Recommended bounded experiment (proposal only; no test executed)

**Objective:** Determine whether a single read-only, deterministic PMB audit helps the ongoing Find -> Fix -> Find loop more than the present process.

**Baseline:** Record the current PMB version, branch, `mb doctor` exit/status, `mb status`, active failing tests/CI and a *bounded* list of 5-10 already-reported problems. Avoid a new comprehensive inventory as the initial product.

**For each candidate:** capture (a) observed impact vs hypothetical risk, (b) reproduction/proof, (c) source and owner, (d) smallest proposed change, (e) guardrail/permission scope, (f) original-task replay criterion and (g) unexpected regression outcome. "Not reproduced" and "no change" must be first-class outcomes.

**Decision:** Pick **one** repeatable, high-impact problem that the current `mb doctor`/CI fails to communicate clearly; make *no* implementation until reproduction and expected benefit are established. Compare a before/after read-only audit with the existing process; only then decide whether to add a check to an existing command. If the baseline process already covers the problem, do nothing.

**Exit:** Fixed path reproducible before and absent after; fresh-session original task passes; existing routing and CI checks still pass; no new permission broadening, telemetry data egress, duplicate rule or continuous background service. Stop after the pilot; do not grow into a control center automatically.

## Pending evidence / Phase 2 questions

1. Obtain the **full original dashboard build prompt**, including dependency, storage, listener/bind, OTel exporter, runtime task scheduling, secrets and security claims; compare it with the screenshots rather than adopting it.
2. Study author's separate **Security** video and independently verify checks for real permission bypass, command exposure, leaked settings, data egress, default bind, authorization, retention and undo.
3. Study author's separate **Context** video; test whether warnings correspond to demonstrable task failures or startup load, rather than arbitrary token thresholds.
4. Determine actual host/version/plan access before promising metrics from Cursor, Gemini, Codex or Claude; local personal sandbox first, company/work permission and data owners separately.
5. If the user's supplied `/zen` skill source is available, do a **read-only code/provenance review** before any installation. A recorded demo is not a security audit of downloaded code.
6. No claimed evidence yet that always-on watchers, scheduled jobs, PR-driven skill repair, universal adapters, UI, or high-dimensional scoring improve PMB performance.

## Decision summary

**RETAIN:** Read-only evidence census; broken/risky/stale/unused categories; source-failure-as-degraded; explicit approvals and receipts; owner-directed fix; independent fresh-run verification; platform-specific telemetry caveats.

**ASSESS LATER:** one small diagnostic delta; cross-host field parity; optional source-specific telemetry pilot; actual prompt/security/context evidence.

**PARK / REJECT NOW:** importing `/zen` without review, building a command-center UI inside PMB, user-wide autonomous skill rewrites, 2-week universal scans, mandatory "three M" policy gates, transcript mass harvesting, work/personal event-stream merging, and fixed token-health scores.

**Operational action taken in this research pass:** documentation only, in a draft HE research branch. No PMB, Work MB, ACR or runtime changes; no scripts executed, permissions altered, external agents installed, or logs exported.
