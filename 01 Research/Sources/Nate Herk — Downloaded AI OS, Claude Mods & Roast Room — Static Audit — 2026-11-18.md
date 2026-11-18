# Nate Herk — Downloaded AI OS, Claude Mods & Roast Room — Static Audit — 2026-11-18

**Status:** Evidence-preservation static audit. The two user-uploaded ZIPs were **opened and inspected without installation or execution** on November 18, 2026. No Claude Code settings, plugins, global `CLAUDE.md`, code, user environment, credentials, PMB, ACR, HE runtime, workplace or GitHub PRs were changed by this inspection (other than committing these research notes on a separate draft HE branch).

**Companion:** [Lydia Hallie — Rethinking Claude Code Practices, Evidence & HE Mapping — 2026-11-18](./Lydia%20Hallie%20%E2%80%94%20Rethinking%20Claude%20Code%20Practices%2C%20Evidence%20%26%20HE%20Mapping%20%E2%80%94%202026-11-18.md). **Study first-party original talk, not just the productized download.**

## 1. Evidence, checksums, and inspection scope

| User-provided file | SHA-256 of uploaded ZIP | Archive inspection |
| --- | --- | --- |
| `ai-os-template.zip` | `553050edebb682147884fc500a6ad730c948cc780381d48de7622f061dba18a5` | 32,133,191 compressed bytes; 150 ZIP entries (97 files + 53 dirs); 35,947,848 uncompressed bytes; ZIP CRC OK; no traversal paths or symlink entries seen |
| `nateherk-claude-code-mods.zip` | `862737625a50d83bae1c49330877e7a93a62ad48c30628dbb9be6350fd2eb4be` | 90,320 compressed bytes; 31 regular entries; 223,204 uncompressed bytes; CRC OK; no traversal paths or symlink entries seen |

The ZIPs were **not run**, installed, extracted into a live Claude config, packaged, or trusted as instructions. Inspected plaintext manifests, README/installation instructions, relevant skills and significant hooks; searched readable JavaScript for network, filesystem, secret handling and process actions. The large 3D application minified `dist/app.js` (~1.66 MB) and bundled media (~32 MB) were **not fully reverse-engineered or security-tested**, and third-party dependency supply chains were not audited. Static review finds no demonstrated exfiltration in the selected source, **not** a certificate that the whole package, its dependencies or its runtime permissions are safe. Installation provenance/signatures and actual Claude mod-runtime behavior were not verified.

### Linked public repository (separate source)

- [nateherkai/roast-room](https://github.com/nateherkai/roast-room) — public repo found and inspected via GitHub connector: README, `CLAUDE.md`, `package.json`, `.claude/skills/roast-room/SKILL.md`, and `roster.json`. This repo was **not** installed or executed; it is a separate model of *simulated advisory debate*, not a technical review system.
- The user's [Nate Herk Video Database spreadsheet](https://docs.google.com/spreadsheets/d/1h5k7vAAXl5_f4VtguDkHwCCy-S-dd7RfG3-rUiMxfPw/edit?usp=sharing) publicly identifies itself as an index with date, title, watch, summary and resource columns; limited public HTML is visible, but the workbook's complete tabs, current rows and dynamic links were not audited. Do not treat it as proof for claims in the videos.

## 2. AI OS starter archive — what it actually ships

Archive root: `AIS-OS/`.

Core items:
- `CLAUDE.md` and `AGENTS.md`: duplicated shared operating manual, explicitly instructing users to keep both in sync; placeholders for user context and personal connections, 3Ms method and repeated preference/routing guidance.
- `context/`, `connections.md`, `decisions/log.md`, `references/`, `archives/`, `aios-intake.md`.
- **Six skills** in each of `.agents/skills/` and `.claude/skills/`: `onboard`, `audit`, `grill-me`, `link`, `level-up`, `3d-brain`. The **duplicated trees** are portability packaging, not evidence of a single canonical owner across providers; maintaining two paths requires controlled sync.
- A local 3D knowledge-globe renderer, build/scaffold scripts, prebuilt JavaScript bundle, assets and media. The bundled 3D animation/video contributes substantially to package size; the existence of a 3D layout is **not** graph analytics or evidence of better recall.

**Concrete behavioral evidence and risks:**

| Source file | Observed behavior | HE/PMB evaluation |
| --- | --- | --- |
| `AIS-OS/.agents/skills/audit/SKILL.md` | Describes a read-only audit **toward inspected systems**, but **automatically writes** dated reports to the project's `audits/` folder and prior-report comparisons. | Useful distinction between verified defect, unverified gap and improvement opportunity. But “audit” isn't strictly side-effect-free; do **not** invoke blindly against PMB/HE while a read-only restriction is active. |
| `AIS-OS/.agents/skills/onboard/SKILL.md` | Reads manuals/context, interviews, writes/rewrites context, instructions and backlinks, backs up existing files into `archives/` on reruns. | Not a harmless template/view. It changes memory authority and instructions; **do not replace PMB** or run against the active PMB release. |
| `AIS-OS/.agents/skills/grill-me/SKILL.md` | Checkpoints interview answers to dated Markdown capture files, later promotes confirmed facts to canonical pages. | **Potential borrow:** a small “capture vs confirmed source” distinction, already reflected in PMB. Avoid duplicating permanent personal memory stores. |
| `AIS-OS/.agents/skills/level-up/SKILL.md` and `CLAUDE.md` | Encourages one automation/repeated workflow improvement per cycle, asks whether AI can do a task. | A legitimate prompt for discovery, **not evidence** a task needs automation. Can cause governance creep or “weekly automation” busywork if applied mechanically. |
| `AIS-OS/.agents/skills/3d-brain/assets/template/serve.mjs` | Starts a Node HTTP server bound to **127.0.0.1**; host/origin checks; GET API for reading indexed local Markdown and a `/api/reveal` that spawns an external opener for selected files. `build.mjs` reads approved source folders and materializes a graph. | Local-first and apparently intentionally scoped, but it still **exposes selected personal/project content via an HTTP service to local clients**, invokes processes to reveal files, and creates a separate derived index. Not a security-verified or PMB-authoritative service. |
| `AIS-OS/scripts/sync-codex-skills.sh` | Exists to coordinate the duplicated provider skill folders. | Another moving part with maintenance and drift implications; if provider interoperability is a real requirement, prefer canonical source + explicit adapters rather than silent duplication. |

**Decision:** Study, do not install as a new personal memory authority. Its best transferable ideas are *evidence-graded audits, capture/enrich boundary, explicit source routing and clear examples*. Those concepts substantially **overlap existing PMB/HE research**; the 3D graph/OS is not a justified project requirement.

## 3. Claude Code mods ZIP — exact implementation classes

Archive root: `nateherk-claude-code-mods/`; `.claude-plugin/marketplace.json` advertises **four installable mods**: Cache Keeper, Recording Mode, Goal Meter, and Collision Guard.

**Important:** `INSTALL-FOR-CLAUDE.md` itself tells an AI to install all four when handed the folder, update the Claude CLI if older, copy the marketplace to `~/.claude/nateherk-mods`, install via `claude plugin install ...`, and append a note to the user's **global `CLAUDE.md`**. That is *an instruction supplied by an untrusted package*, not user authorization or a harmless README. It also includes remote shell installer one-liners for missing Claude CLI. **We did not follow any of these instructions.** A configuration with `CLAUDE_CONFIG_DIR` would still write the marketplace to the physical home `~/.claude` according to its installer instructions, increasing setup ambiguity.

### 3.1 Cache Keeper

Source: `cache-keeper/hooks/register.mjs`, `pricing.mjs`, `README.md`.

- Adds cache status, a multi-session board, large cold-send warning, `/handoff`, and an **opt-in** `/keepwarm` action.
- Source code actually invokes `$.model.fork({ prompt: 'cache-keeper keep-alive ping. Reply with only: ok' })` during keep-warm operation; keeps pings/counts and estimated cost. Thus **it can create real model calls**, not merely show cached values, when activated. Subscriber plan quotas, provider pricing/caching, and actual human benefit must not be equated to the script's API-dollar estimates.
- Keeps session heartbeats and handoff Markdown files under `~/.claude/mods-data/cache-keeper/`; can offer clear-and-continue actions. Any artifact in handoffs has potential sensitive context (not reviewed).

**Verdict:** **Do not install** absent a measured local cache/working-time problem. The ability to issue background model calls and save session handoffs is not justified by a hypothetical token optimization. Compare against Lydia's first-party advice (`/clear`, `/context`, explicit `/compact`) first.

### 3.2 Recording Mode

Source: `recording-mode/hooks/register.mjs` and shared `privacy.mjs`.

- Designed to redact secrets/personal details on **display**, restrict certain tool calls during recording, save a local recording state and configuration.
- Code reads `.env` and `.env.local` from the current working directory and up to **three parent directory levels**, parses secret **values** via `secretValuesFromEnv`, and holds those values **in mod-process memory** as replacement candidates. Code comments say values are not written to disk.
- Intercepts `prompt.submit`, `tool.call` and UI rendering while recording; the **redaction rule set is regex-based and cannot guarantee all sensitive data is hidden**, nor does it remove secrets from underlying transcripts/history.
- Reading secret material for display masking is a security-relevant *authority and exposure* tradeoff. This feature should not be granted secret access just to create a recording unless the use case and isolation are explicit.

**Verdict:** **Do not install globally** on this evidence. It does not substitute for secret files being absent or inaccessible and can introduce an additional sensitive-file reader.

### 3.3 Goal Meter

Source: `goal-meter/hooks/plan.mjs`, `register.mjs`.

- Adds task-plan UI and local task progress; registers a custom tool and reacts to `/goal`; reads session transcripts and persists local status, and can consult model/turn completion to score progress.
- Task progress and model self-report are **not proof** that code is safe, requirements met, or tests passed. Potentially redundant with existing task/PR/CI artifacts.
- On Windows the source includes a PowerShell transcript-tail fallback, increasing platform-specific behavior to test before trusting it.

**Verdict:** UI inspiration only. No new progress database, goals framework or automated validation gate justified for HE.

### 3.4 Collision Guard

Source: `collision-guard/hooks/register.mjs`, `coach.mjs`.

- Logs per-chat changed-file evidence under `~/.claude/mods-data/collision-guard/`; checks other sessions' edits over a time window and can offer **proceed / move to worktree / cancel**. Its own comment refers to recent files and a session ledger.
- This is an advisory concurrency detector; a 30-minute recent-edit window is **not an OS lock, worktree isolation, Git merge guarantee, or authoritative cross-process ownership system**. It can miss changes from external editors, sessions, other machines, or unobserved paths.

**Verdict:** Borrow only the *conceptual* preference for isolated branches/worktrees if actual collisions arise. Deterministic Git status and separate worktrees are more direct than a global UI modification.

## 4. Roast Room — what we could borrow

Repository: https://github.com/nateherkai/roast-room

**Observed:** `.claude/skills/roast-room/SKILL.md` uses a Chair, user-confirmed seats, default seven expert-inspired simulators and optional extras, multiple rounds of deliberation, a weighted verdict and a local visualization. `CLAUDE.md` explicitly states the advisor outputs are **simulations**, not statements by the real public figures, absent a verbatim sourced quote. Public source material is used to construct role “brains” and prompt context.

**Potentially useful:** (1) independent *perspective coverage* for a planning decision; (2) explicit dissent/downside; (3) requiring user confirmation of who participates; (4) recording evidence/provenance rather than presenting simulated quotes as fact. These overlap existing HE “advisory board”/investigation-integrity research.

**Limits:** Multiple role prompts on the same underlying model are **correlated model judgment**, not independent expert review or executable verification. Weighted and numerical verdicts are easy to overinterpret; confidence should come from **real sources, explicit assumptions and measured outcomes**. Not a reason to clone a celebrity simulator into HE, PMB, or ACR. No need to run the local web server to borrow these principles.

## 5. Cross-artifact risk register

| ID | Severity if installed unreviewed | Evidence | Suggested smallest correction/handling |
| --- | --- | --- | --- |
| S1 | **High** | Mods installer seeks to modify global Claude settings and install a local plugin marketplace, even though user's task is to **research** | Never run from untrusted README; no install or global edits without a separately explicit request and sandboxed evaluation |
| S2 | **High** | Recording mod intentionally reads `.env` values from working/parent directories | Keep sensitive files out of plugin scope; avoid installation absent a supported and auditable requirement |
| S3 | **Medium** | Cache Keeper `$.model.fork` pings and saved handoffs | Treat as opt-in potentially billable calls, measure actual quality/savings, exclude sensitive handoffs |
| S4 | **Medium** | AI OS `/audit` writes reports; `/onboard` rewrites personal context; `/3d-brain` serves indexed content | Distinguish auditing from mutation and visualization from canonical knowledge; don't deploy to active PMB |
| S5 | **Medium** | Parallel-mod advisory ledger provides time-window warnings, not actual exclusive write ownership | Prefer explicit worktrees/branches; test overlapping edits on real fixture before considering automation |
| S6 | **Medium** | Session-cache and token-cost estimates use bundled pricing assumptions and model behavior estimates | Keep rates, subscription quotas, cache TTL and task outcomes separate; no unverified “savings” claims |
| S7 | **Low / context-specific** | Duplicate `.agents/skills` and `.claude/skills` plus `AGENTS.md`/`CLAUDE.md` within AI OS starter | Avoid second writable authority and manual duplication; document source precedence if actually adopted |
| S8 | **Unknown** | Minified 3D bundle, dependencies, model mod API compatibility and supply-chain signatures not tested | Do not claim safety; request independent verification before any future installation consideration |

No confirmed external data exfiltration found in the selected scripts; **no whole-package security certification**. Script behavior in a live Claude Code installation, browser and across OS versions was **not tested**.

## 6. Recommendation / relevance to existing HE work

| Concept from downloads | Existing HE/PMB ownership | Action |
| --- | --- | --- |
| Evidence-based audits | HE experience-derived harness evolution; PMB doctor/status | **Retain as corroboration**, don't add a competing audit engine |
| Capture then confirm | PMB provenance and canonical owner | **Already aligned** |
| Tool permission vs OS containment | HE bounded execution envelopes | **High relevance**, keep concrete risks and first-party refs |
| Source-backed independent dissent | HE investigation integrity and ACR evidence verification | **Retain principle, not a persona system** |
| 3D knowledge globe | Separate visualization novelty | **Reject as PMB/HE requirement** |
| Scheduled “level up” and keepwarm ping | No demonstrated user workload need | **Do not build/install** |
| Per-session worktree collision warnings | Potential actual multi-session overlap | **Optional future evaluation only if collisions occur** |
| Progress meter / external command center | Parked Home/Work dashboard research [draft PR #28](https://github.com/unyieldingclaw-dev/harness-engineering/pull/28) | **Do not integrate**; maintain separate read-only/local-first scope |
| Broad AI OS from external template | PMB already owns durable project memory | **Do not replace or cross-wire** |

**Disposition:** Preserve source findings as research; no implementation, third-party installers, new hooks, credentials access, external communications, dashboard scope growth, or PR merge authorized. All findings remain scoped to inspected archive versions and documented sources.
