# Beau Johnson — Five Trending Agent Repositories & Claude-Mem Consent Incident — 2026-11-08

**Status:** Source review and verified security addendum; research only, not an adoption/installation decision.  
**Video:** [NEW 5 GitHub Repos Blowing Up (LIFE CHANGING)](https://www.youtube.com/watch?v=y2TmTP7qsvU&t=74s), Beau Johnson / Build N Public.  
**Evidence supplied:** User-provided ~14-minute transcript, video description, and GitHub-trending screenshot, reviewed 2026-11-08.  
**Method:** Direct inspection of official GitHub README/docs, release notes, selected skill and changelog text; source-head SHAs captured below. **Did not** install dependencies, execute agents, run benchmarks, reproduce privacy traffic, independently validate GitHub weekly stars, or inspect the hosted service's actual data retention.

## Executive finding

This video covers **four repositories HE already mined** in the 2026-11-07 [15-repository review](./The%20Next%20New%20Thing%20%E2%80%94%20Agents%20Get%20Computers%2C%20Memory%2C%20Skills%20%26%20Worktrees%20%E2%80%94%202026-11-07.md) (T3 Code, claude-mem, Superpowers) or in the [ECC-specific research PR #20](https://github.com/unyieldingclaw-dev/harness-engineering/pull/20) (ECC). **TradingAgents is the new repository.**

The significant new evidence is **not the popularity ranking**. It is a **confirmed history-exfiltration-without-consent product defect in claude-mem's browser installer**, plus an illustrative set of **point-in-time data/evaluation integrity fixes** from TradingAgents.

**Disposition:** Correct earlier privacy caveats; constrain all future third-party memory trials to explicit, testable data-handling terms. Retain TradingAgents as a narrow evidence-integrity example; do not install any of the five into PMB/ACR/HE.

## Source and claim map

| Video item | Official GitHub repo / inspected head (2026-11-08) | Video claim and independently verified boundary | HE decision |
| --- | --- | --- | --- |
| #1 T3 Code (10:35) | [pingdotgg/t3code](https://github.com/pingdotgg/t3code) `3da48c1234045f89bac7db885bbee73464a60624` | Official README/FAQ describe a **control surface over existing coding-agent harnesses**, not a harness itself; phone/remote access operates agents on the connected host. [Permission documentation](https://github.com/pingdotgg/t3code/blob/main/docs/user/permission-modes.md) confirms the **initial new-thread default is Full access** and providers enforce approvals differently. [FAQ](https://github.com/pingdotgg/t3code/blob/main/docs/user/faq.md) discloses usage telemetry and reported faster Claude five-hour rate consumption through the Agent SDK (provider attribution is a maintainer claim, not independently reproduced). | **MONITOR / optional UI** only after an actual need for multi-provider/remote control. Test least-privilege defaults and observed authority, not the UI label alone. |
| #2 claude-mem (07:54) | [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) `9efb0c358af5d531de29b2142c8963886e063530` | Official v15.8.3 notes and merged PR #5220 confirm prior browser setup **uploaded ~30 days of local prompt text and metadata without an on-screen consent gate** (see detailed section). Current README labels main bundle v15.12.1; GitHub latest published release API showed v15.12.0 at review. README states anonymized usage telemetry is **on by default**; v15.8.3's **analytics checkbox during setup** is separate and defaults off following the fix. | **DO NOT INSTALL AS DEFAULT; PRIVACY REVIEW REQUIRED BEFORE ANY PILOT**. Historical traces are not PMB truth. |
| #3 ECC (05:26) | [affaan-m/ECC](https://github.com/affaan-m/ECC) `d2ef2680594f9a17526a6e1a2e18b3d242c5d17b` | README advertises **71 agents, 314 skills, 98 legacy command shims**, and hooks. Video's `~30K` description-token figure is the narrator's estimate, **not a measured actual session cost**. Full installation and background self-learning are unjustified for HE. Prior HE PR #20 already examined installer/source ownership, host capability declarations vs measured activation, and isolation fail-closed boundaries. | **NO NEW IMPLEMENTATION**; existing ECC assessment suffices. Selectively study isolated tactics only when a real need is evident. |
| #4 Superpowers (03:11) | [obra/superpowers](https://github.com/obra/superpowers) `22f0b463e7a5e579904dd6211a520321035526f7` | Verified `skills/using-superpowers/SKILL.md`: a “1% chance” requires skill invocation and makes pre-response skill checks mandatory. Useful tests/verification patterns coexist with required brainstorm/plan/review ceremony and subagent cost. Video's `~1,400` always-on token estimate was **not measured** here. | **BORROW narrowly**; do not install universal workflow or delegate the human product-manager role. |
| #5 TradingAgents (00:53) | [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents) `9fa1196fffbcfd9de5146d2dd2869df84ab99838` | Official README implements analyst reports → bull/bear debate → trader proposal → risk review → portfolio manager decision; simulation/research, not production trading proof. Official CHANGELOG confirms 0.7.0 (2026-10-16) supports running on a ChatGPT Plus/Pro login, and 0.7.1 (2026-10-27) corrected time alignment and historical data defects. [Research paper](https://arxiv.org/abs/2412.20138) dates to 2024; results cannot establish live returns. | **REFERENCE** only for independent-evidence and temporal-fidelity design; **DO NOT ADOPT** as finance agent or new HE orchestration stack. |

The video’s weekly star counts, total counts, install counts, claims of “life changing,” and model savings are snapshots or estimates from its author; **not independently reproduced** by this review and not used as evidence of effectiveness or security.

## Critical verified update: claude-mem's installer uploaded prompts without consent

**Confirmed first-party sources:**

1. [v15.2.0 release, 2026-10-25](https://github.com/thedotmack/claude-mem/releases/tag/v15.2.0): installer opened browser flow; “Reveal” step uploaded filtered prompts to hosted CMEM for intent extraction. Changelog disclosed uploading, but the setup interface did not ask for approval.
2. [Issue #5176, opened 2026-10-29](https://github.com/thedotmack/claude-mem/issues/5176): reports no on-screen consent for transmitting prompt text.
3. [Merged PR #5220](https://github.com/thedotmack/claude-mem/pull/5220): explicitly states browser setup read 30 days of local histories and transmitted **full prompt text, session IDs, project names, folder paths and timestamps** without requesting permission, despite privacy-policy wording that claimed otherwise. Its single prechecked “improve product” checkbox was **not an upload-consent control**.
4. [v15.8.3 release, 2026-11-02](https://github.com/thedotmack/claude-mem/releases/tag/v15.8.3): scan now runs locally until explicit consent; local-only means no prompt upload from this installer path; per-tool selection; analytics consent separate and initially off; duplicate-upload guard. The release **does not delete earlier uploaded prompts** and leaves **server-side retention/deletion outside scope**.
5. [v15.12.0 release, 2026-11-07](https://github.com/thedotmack/claude-mem/releases/tag/v15.12.0): screenshots and images that the agent reads or creates are **kept by default** and linked to observations; retention of existing images persists after the feature is switched off. Release states images are **not sent to an AI model by this image-capture feature**. This is a **local retention issue to examine separately**; do *not* infer default automatic cloud upload of all images.

**Narrow affected path:** the incident is specifically attributed to fresh **browser setup launched by `npx claude-mem install` since v15.2.0**, before v15.8.3. The upstream evidence does not establish that every installation, every plugin-only route, or all historical sessions were uploaded.

**Important distinction:** CLI/main README’s general “anonymous usage analytics are on by default” is *not* the same setting as the fixed browser consent panel’s optional “improve-product” checkbox. Review both instead of falsely claiming telemetry was universally turned off.

### HE/PMB security lessons grounded in the incident

- **Collection boundary:** before enabling observer hooks, determine which sessions, agent platforms, folders, prompts, tool outputs, screenshots, credentials and user/private/employer content the integration can inspect. “Local memory” is not proof of local-only *setup, inference, sync, export or telemetry*.
- **Transmission boundary:** inspect network calls and explicit opt-ins at **installer time**, not merely during ordinary agent use. An opt-in setting for analytics does not imply a separate product feature cannot transmit full prompts.
- **Retention / erasure boundary:** a privacy fix does not remediate already-transmitted data. Identify who operates remote storage, downstream processors, deletion process and residual copies; mark unknown if vendor evidence is incomplete.
- **Cross-platform boundary:** installer scanned other agents' history, not just Claude Code. Limit analysis to approved data sources, especially when corporate/private and personal profiles coexist on one machine.
- **Supply-chain boundary:** treat downloaded hooks/installers as privileged code; review provenance/version, observed effects, network destination and rollback. Do not reflexively install another scanning framework to secure the first.
- **Promotion boundary:** memory observations are historical evidence for human-reviewed updates, not authoritative facts/rules. This incident raises adoption risk even if the retrieval architecture is technically sound.

**Candidate pre-pilot check (not a new always-on gate):** use a disposable test user/project, no real secrets, with outbound traffic recorded; test installer setup without consent, with “keep local,” with selective consent; verify the exact files and histories touched, traffic destinations, telemetry preferences, retention/deletion behavior, image capture and uninstall/rollback. Require user authorization before any actual hosted import. If these properties cannot be verified, **do not trial on meaningful data**.

If an operator ran affected `npx` browser setup between **Oct 25 and Nov 2**, recommend checking with the provider about retained/deleted data and assessing exposed content; **do not assume this user ran the installer**.

## TradingAgents: the stronger HE lesson is data-time integrity, not agent debate

The [TradingAgents paper](https://arxiv.org/abs/2412.20138) presents multi-role analyst, bull/bear, risk and decision-agent debates and experimental returns; it is a research result, not validated production trading performance.

Official [TradingAgents CHANGELOG](https://github.com/TauricResearch/TradingAgents/blob/main/CHANGELOG.md) contains **specific fixed defects**:

- v0.7.0, 2026-10-16: “first close after decision” backtest scoring. A previous same-day-close scoring method could evaluate a decision against prices already unavailable when the decision was made. Old log entries and new entries could then mix incompatible measurement conventions when resuming a run.
- v0.7.1, 2026-10-27: non-zero-padded dates such as `2024-7-5` could compare incorrectly against disclosure dates and allow filings from **after** the putative analysis date into historical context.
- v0.7.1: scoring a still-open session and failing to settle around long market holidays; memory rotation on backfilled dates could distort what the agent treated as most recent; financial statements could show numbers under the wrong reporting period.

**Transferable HE pattern:** a multi-agent “bull/bear” or independent-review structure does not validate the evidence inputs or temporal alignment. Review must ask **what information was available at the modeled decision time**, which sources were used, whether revisions/late publications leak into the replay, and whether a metric really measures the claimed outcome. Deterministic date/time source filtering and executable fixtures outrank rhetorical debate.

**Potential applicability:** any future HE/ACR agent-evaluation experiment using historical artifacts, issue/CI history or before/after performance reports. This is a **research caution**; no current reproducible HE/ACR defect has been identified by this repository.

**Against a generic “make agents argue” rule:** many agents reading the same wrong data can agree on a wrong answer. Opposition review is useful when there are distinct evidence scopes and verifiable claims, not because a debate occurred.

## Why the other three do not change our roadmap

- **T3 Code:** useful if concurrent provider workflows and mobile access cause real friction. Today it adds a control/permission/remote-access surface. Its default Full access and provider-specific enforcement deserve explicit checks before any real project pilot. No proof of better code quality, lower spend, or broad security parity was obtained.
- **ECC:** the existing [HE ECC analysis (PR #20)](https://github.com/unyieldingclaw-dev/harness-engineering/pull/20) already isolates reusable delivery/health/eval mechanisms. Its scope and self-learning automation are not justified by “314 skills.” Description-token estimates must be checked in a real host/session, not inferred by summing repository text.
- **Superpowers:** useful source of narrow systematic debugging, TDD and proof-of-completion patterns. Its mandatory “1% chance” invocation policy and universal ceremony compete with HE’s requirement to solve the observed problem with a bounded workflow. Additional agents consume time and budget and do not replace independent evidence.

## Recorded disposition / no-implementation boundary

1. **REINFORCE**: consent and effective data-boundary verification must precede any third-party session/history memory pilot.
2. **REINFORCE**: test execution-time evidence, source dates and evaluation windows before trusting historical agent performance claims.
3. **RETAIN**: source docs, pinned repo commits and incident release references as evidence; do not generalize to every version or installer path.
4. **DEFER**: T3 control surface, ECC install, Superpowers blanket workflow, and TradingAgents model stack.
5. **NO ACTION**: no third-party software installed, no chat archives exported, no PMB/ACR code changed, no new universal policy, no security framework introduced by this video.

Related HE sources: `01 Research/Observed Effects, Scoped Recall & Lifecycle Evidence — 2026-11-07.md`; `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`; `01 Research/Risk-Directed Review & Release Proof — 2026-09-24.md`; `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`.
