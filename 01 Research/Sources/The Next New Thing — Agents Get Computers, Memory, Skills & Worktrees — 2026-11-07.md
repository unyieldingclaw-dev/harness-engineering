# The Next New Thing — Agents Get Computers, Memory, Skills & Worktrees — 2026-11-07

## Source and scope

- Video: **Hot: Agents get computers & fun annotation & free designs & more** — The Next New Thing AI.
- URL: https://www.youtube.com/watch?v=kus5P1211_g
- Channel: https://www.youtube.com/@TheNextNewThingAI
- Evidence supplied: user-provided video description/link index and timestamped transcript.
- Review date: 2026-11-07.
- Method: direct GitHub examination of all **15 named open-source repositories** (3 bonus projects, 10 ranked projects, 2 community projects); repository root/README and selected implementation, architecture, skill, test, and security files. Commits captured below.
- Excluded from repository count: the sponsor (Monid), Zapier, and non-GitHub links mentioned in the episode. Their commercial claims were not evaluated.
- Evidence limitations: static repository review, not an installed or executed benchmark. README statements are not independent performance measurements. License checks below are illustrative rather than a full dependency-license audit.

**Research only:** No third-party framework is approved for adoption, no PMB/ACR behavior changes are authorized, and no HE implementation decision is created by this note. Reuse concepts and independently implement/test them; do not copy source without license/attribution review.

## Executive verdict

The best opportunities are *mechanisms*, not a replacement platform:

1. **Claude-Mem:** indexed observation discovery, then selective timeline and full-detail retrieval. Treat automatic memory as historical evidence, never as authoritative project truth.
2. **Worktrunk:** reversible worktree lifecycle and evidence before branch cleanup, not merely shorter shell commands.
3. **Agent Desktop:** typed observations, ephemeral references, stale-target rejection, and re-observation after effects.
4. **Production Skeleton:** scope readiness gates to application stage, cite file:line evidence, separate UNKNOWN from FAIL and NOT APPLICABLE.
5. **TokenSaver:** fused task-local operations and transcript recovery; instrument against actual wall time, billed tokens, and task quality rather than estimated displaced calls.
6. **Superpowers / Anthropic Skills:** skill structure and fresh verification; avoid turning all task types into a mandatory multi-agent ceremony.

Important overlap: HE already has research on skills, retrieval economics, bounded side effects, session handoffs, fan-out/fan-in, review evidence, and host portability. Net-new material should extend those artifacts selectively, not establish another competing framework.

## Repository-by-repository source assessment

| Source (inspected ref) | What the repo actually implements | Mine / disposition | Main limitation or conflict |
|---|---|---|---|
| [vercel-labs/agent-desktop](https://github.com/vercel-labs/agent-desktop) (b43ac4f6e892) | Native accessibility-tree desktop CLI/C library for macOS, Windows, Linux, with snapshot-scoped refs | **ASSESS** observation/effect protocol | Accessibility coverage, sensitive UI data, permission boundary; not a model or a safety sandbox |
| [joeblau/subvox](https://github.com/joeblau/subvox) (591d02838521) | macOS AirPods head-motion experiment: trained short-vocabulary templates, DTW, leave-one-out and permutation tests | **PARK**; experimental-method example only | Not general silent dictation; sensor, vocabulary, user/session limits |
| [nilbuild/tinyhand](https://github.com/nilbuild/tinyhand) (c5fb36b7c4d7) | macOS pointer/annotation utility with tap and screen drawing | **NO HE CHANGE** | Visual presenter tool, not agent annotation protocol |
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) (2d8c08c182af) | Session observation capture + SQLite/FTS5 + Chroma search projection + worker and MCP retrieval | **PILOT CANDIDATE** for PMB recall comparison | Derived summaries, hooks, index refresh, privacy, lifecycle burden; not a PMB truth-store replacement |
| [Nutlope/hallmark](https://github.com/Nutlope/hallmark) (407fa689f4a2) | Theme-directed generation and browser/design checks with Floor/Reflex/Finish gates | **BORROW GATE CLASSIFICATION**, UI project only | Some purported hard floors are artistic preferences, not objective invariants |
| [pingdotgg/t3code](https://github.com/pingdotgg/t3code) (25bf84ab07f5) | Local/remote agent **control surface** over external coding providers, mobile/web/desktop | **MONITOR / OPTIONAL TOOL** | Remote access and permission mappings; does not prove a new HE harness is needed |
| [pbakaus/impeccable](https://github.com/pbakaus/impeccable) (687bc381bc90) | Incremental design commands, deterministic lint, project context, live element-targeted edits | **ASSESS** constrained design feedback workflow | Broad integration surface; mutation and validation scripts require trust |
| [obra/superpowers](https://github.com/obra/superpowers) (22f0b463e7a5) | Agent development methodology: design, plan, TDD, separate agents, spec/quality review, verification | **BORROW SELECTIVELY** | Mandatory workflow on small tasks increases overhead and authority creep |
| [anthropics/skills](https://github.com/anthropics/skills) (2c5958ecac7c) | Self-contained skill folders, SKILL.md metadata, templates and agent skill spec/reference | **REFERENCE ONLY** | Format portability is not behavioral equivalence; not every sample is needed |
| [max-sixty/worktrunk](https://github.com/max-sixty/worktrunk) (71d99c7e6b5a) | Git worktree CLI, branch/worktree lifecycle, hooks, removal safety analysis | **ASSESS / SMALL PILOT** where concurrent agents actually work | Extra dependency and lifecycle automation unearned for solo sequential tasks |
| [jakubkrehel/skills](https://github.com/jakubkrehel/skills) (f41f027797f7) | Discrete interface/accessibility/typography/layout/review skills | **UI-ONLY REFERENCE** | Standards checks need actual accessibility testing, not agent prose alone |
| [JetBrains/thinkrail](https://github.com/JetBrains/thinkrail) (519286befe0f) | Pi-based agent IDE, isolated worktrees, structured/spec graph, shared typed host/UI contracts | **MONITOR** | Spec graph duplicates PMB concepts unless a new concrete problem is shown |
| [ghostty-org/ghostty](https://github.com/ghostty-org/ghostty) (27ffca0dd671) | Terminal emulator plus libghostty, standards-driven terminal implementation | **NO HE CHANGE** | Mostly development-environment choice; no harness outcome evidence |
| [Brucee/tokensaver](https://github.com/Brucee/tokensaver) (8b8ab4d053b3) | MCP Search/Edit/Sql fusion, transcript recall, compaction recovery, estimated savings accounting, tests | **PILOT CANDIDATE** for tool economics; not blanket install | Claimed savings rely on modeled equivalents; fuzzy mutation and hook redirection add risk |
| [hallikkristjan-code/production-skeleton](https://github.com/hallikkristjan-code/production-skeleton) (041cfbaa3093) | Stage-aware 13-question production probe and explicit skeptic-review skill with evidence | **BORROW SMALL EVIDENCE PATTERN** | No identical checklist for CLI, personal-data app, and payments app; static proof is limited |

All refs are **GitHub default-branch heads sampled during the review**, not signed releases; use the exact SHA, not a moving README, when reproducing a claim.

## Deeper implementation findings

### 1. Claude-Mem: source versus projection and recall workflow

Primary files: [search architecture](https://github.com/thedotmack/claude-mem/blob/2d8c08c182af68b20bb7528c7bf989aa871a7d2f/docs/public/architecture/search-architecture.mdx), [database architecture](https://github.com/thedotmack/claude-mem/blob/2d8c08c182af68b20bb7528c7bf989aa871a7d2f/docs/public/architecture/database.mdx), [hook lifecycle](https://github.com/thedotmack/claude-mem/blob/2d8c08c182af68b20bb7528c7bf989aa871a7d2f/docs/public/architecture/hooks.mdx).

- Retrieval is explicitly layered: lightweight search index -> neighboring timeline -> selected full observations; MCP acts as a thin client to a worker HTTP API.
- SQLite/FTS5 stores observations; Chroma is a semantic projection. The architecture document explicitly says indexing coordination is **within one worker process**, not an atomic SQLite/Chroma transaction.
- Hook capture makes recall automatic but creates background processing, sensitive session retention and cross-host event-semantic questions.
- The README/docs' ~10x token-efficiency claim is **not established by this review**. No controlled PMB tasks were run.
- HE/PMB boundary: PMB's curated projectbrief/techContext/systemPatterns/activeContext/progress remain owned by PMB. Observations may reveal *where to look* or help reconstruct a past run, but must not silently overwrite current facts or decisions.
- Test only if actual multi-session recall failures or excessive manual searches are observed. First compare plain Git + PMB + targeted transcript search; only then consider index/capture infrastructure.

### 2. Worktrunk: safe cleanup is more valuable than compact syntax

Primary file: [worktree removal behavior](https://github.com/max-sixty/worktrunk/blob/71d99c7e6b5aff800237cce5029e546e43e5d675/docs/src/content/docs/remove.md).

- Removal safety checks include same commit, ancestor, empty branch-only diff, equal trees, simulated merge adds nothing, and matching patch-id after squash merges.
- Default branch removal depends on clean working trees and a bounded history walk; forced deletion is explicit.
- Useful abstraction: **workspace identity -> isolated change -> verified merge status -> safe cleanup**.
- Treat clone/worktree internals as *execution state*, not as PMB's durable memory, and do not infer branch deletion safety from a successful agent chat.
- The user-supplied prior review flagged possible stray .claude/worktrees artifacts. Fresh check of PMB's current main branch found that .claude/worktrees/ is already listed in .gitignore and no such tracked directory appeared under .claude. **No current defect verified.**
- Worth a trial only if multiple concurrent agents produce actual branch/worktree collision, stale workspace or cleanup problems; native git worktree commands remain the zero-dependency baseline.

### 3. Agent Desktop: a typed observe-act-observe loop

Primary files: [README](https://github.com/vercel-labs/agent-desktop/blob/b43ac4f6e892ef608f8851428613c4009f0e84c7/README.md), [concepts](https://github.com/vercel-labs/agent-desktop/blob/b43ac4f6e892ef608f8851428613c4009f0e84c7/CONCEPTS.md).

- The tool has **no model**. It exposes OS accessibility trees and returns JSON snapshots with refs tied to snapshot/session identity.
- Stale or ambiguous references are rejected, not guessed; actions are followed by a fresh read of the relevant window/surface. Partial observations are explicitly marked.
- This supports the stronger HE contract: observation identity, completeness, requested action, actual postcondition and provenance. A click acknowledgment alone is not business-effect proof.
- Capabilities can operate across sensitive desktop apps; snapshots, clipboard reads and screenshots may contain private information. Permission scope, explicit side-effect approval and log minimization must be enforced by the caller.
- Pilot in disposable UI test environment only. Do not enable unrestricted control of a real workstation as a side effect of HE research.

### 4. TokenSaver: measured usage and counterfactual claims are distinct

Primary files: [README](https://github.com/Brucee/tokensaver/blob/8b8ab4d053b3c52f160fdb32559a6fc15d05993e/README.md), [savings tests](https://github.com/Brucee/tokensaver/blob/8b8ab4d053b3c52f160fdb32559a6fc15d05993e/test/savings.test.mjs), [compaction tests](https://github.com/Brucee/tokensaver/blob/8b8ab4d053b3c52f160fdb32559a6fc15d05993e/test/compaction-digest.test.mjs).

- Search combines glob, content, symbols, importers and capped output; Edit batches mutations with exact/fuzzy matching; Sql combines schema introspection and bounded output.
- The savings test defines equivalent native calls using fixed weights and derives time/token savings from assumed call displacement. This is a **modeled counterfactual**, not an A/B experiment that measures what the unassisted model would actually do.
- Transcript scanning records usage and successful plugin calls, which is useful observational data. Post-compaction digest tests attempt to recover dropped conversation segments with IDs.
- Fuzzy batch edits and hook redirection broaden mutation/steering responsibilities; do not move either into PMB or ACR without an observed failure and deterministic rollback behavior.
- Proposed experiment: fixed commits and task prompts, same model/effort, 10+ task pairs with/without selective fused tools, actual token usage, wall time, success, error rate and manual source re-read count. Count tooling setup/maintenance.

### 5. Superpowers, Anthropic Skills: actionable practices but no blanket methodology adoption

Primary files: [Superpowers verification](https://github.com/obra/superpowers/blob/22f0b463e7a5e579904dd6211a520321035526f7/skills/verification-before-completion/SKILL.md), [systematic debugging](https://github.com/obra/superpowers/blob/22f0b463e7a5e579904dd6211a520321035526f7/skills/systematic-debugging/SKILL.md), [subagent development](https://github.com/obra/superpowers/blob/22f0b463e7a5e579904dd6211a520321035526f7/skills/subagent-driven-development/SKILL.md), [Anthropic skill template](https://github.com/anthropics/skills/blob/2c5958ecac7cee2b583967a2c4981d0f4a75c579/template/SKILL.md).

- Superpowers explicitly separates spec-compliance from code-quality review and requires fresh evidence before claims of completion; its debugging procedure prioritizes reproduction and tracing the owning component.
- Those are close to already-documented HE principles. The actual upstream skill also mandates per-task implementer agents, multiple reviews and strong workflow triggers. That may be disproportionately expensive for small changes.
- It contains an important bounded-authority nuance: irreversible/destructive, security-sensitive, out-of-worktree effects and unresolvable plans are human-stop conditions. Nevertheless, its general instruction to rule on ordinary ambiguities without pausing should **not** replace HE's user-owned intent decisions.
- Anthropic's self-contained skills and basic template are useful reference shapes. A shared SKILL.md is not proof that Claude/Codex/Cursor have equivalent tool rights, discovery semantics or compaction behavior.
- HE already contains stage selection, progressive disclosure, behavioral skill evaluation, review axes and evidence expiration. No new always-on policy justified.

### 6. Production Skeleton: actual evidence and stage scoping

Primary files: [README](https://github.com/hallikkristjan-code/production-skeleton/blob/041cfbaa3093bcaf79d4ba630cfaa214b3bce074/README.md), [skeptic review](https://github.com/hallikkristjan-code/production-skeleton/blob/041cfbaa3093bcaf79d4ba630cfaa214b3bce074/skills/skeptic-review/SKILL.md).

- The probe asks 13 production-readiness questions but stages requirements by application type. Audit marks PASS / WARNING / FAIL / UNKNOWN / N/A, with reasons and file:line evidence.
- Critically, its own skeptic skill warns that an automated probe PASS only proves a pattern exists; it does not prove that it actually runs or that a live service is configured as claimed.
- A positive check for backups is weaker than a *restore drill*; a privacy page is not a substitute for operational deletion handling; a CI YAML file is not evidence the workflow ran.
- ACR and HE should preserve separate *not-applicable*, *not-checked*, *evidence-missing* and *verified-failed* states rather than flattening all into failed or green.
- Use as a bounded, opt-in checklist for a suitable deployed app (e.g. Side Quest Atlas); do not apply to every utility or force a new HUMAN-GATES.md into every project.

### 7. Design tools: distinct use cases, not three simultaneous required skills

- **Hallmark** [skill](https://github.com/Nutlope/hallmark/blob/407fa689f4a276f2fd389208d271d0fef08f5058/skills/hallmark/SKILL.md) distinguishes blocking Floor, waivable Reflex and nonblocking Finish. But its Floor combines accessible focus/reduced motion with taste-specific exclusions (for example blanket bans on visual patterns). HE should not declare aesthetic preferences universally safety-critical.
- **Impeccable** [README](https://github.com/pbakaus/impeccable/blob/687bc381bc9091bf252c0a05303640561014a425/README.md) is especially useful for an *existing* interface: named target, critique/audit, direct annotations and local live variants. Confirm its optional validation script before permitting mutations.
- **Jakub Krehel's skills** [better-accessibility](https://github.com/jakubkrehel/skills/blob/f41f027797f77cb1594a5c718aeee3e6a7fe99ba/skills/better-accessibility/SKILL.md) tie review findings to specific WCAG 2.2 A/AA criteria and distinguish standards from stylistic recommendations.
- For a UI project, start with existing design tokens + one selected review workflow, then use real keyboard, responsive, and assistive-technology checks. No universal HE front-end methodology is warranted.

### 8. Control surfaces and other lower-priority projects

- **T3 Code:** [permission modes](https://github.com/pingdotgg/t3code/blob/25bf84ab07f53f141c20239c63e383823a9d5ea0/docs/user/permission-modes.md) document **Full access as initial default** and provider-specific enforcement differences. Remote access/pairing is another authority boundary. Valuable cautionary case, not a PMB memory feature.
- **ThinkRail:** [architecture.md](https://github.com/JetBrains/thinkrail/blob/519286befe0f15cc2dd270e251ecd8cdb9d0a01c/architecture.md) describes pi session host, typed client-host contract, isolated worktrees and an optional rich spec graph. Its UI + living specs might improve a user's workflow, but do not prove HE needs a spec graph.
- **Ghostty:** stable embeddable terminal library and standards-conformance posture; no evidence it improves AI agent acceptance outcomes relative to the current terminal.
- **Subvox:** statistical experiment with low-rate motion sensors, not general speech recognition. Methodology lesson: prove signal above chance before productizing.
- **TinyHand:** macOS visual annotation for human demos; not an AI inspection/evidence channel.
- **Sponsor/tool aggregator:** Monid and Zapier were presented commercially. No independent assessment of prices, privacy, tool safety or claimed savings was performed; do not treat sponsor claims as HE evidence.

## Explicit non-adoptions and research constraints

- Do not install claude-mem alongside PMB as a second authority, or merge generated observations into the five canonical files without human/source verification.
- Do not promote Superpowers' universal workflow, 15 new skills, or three separate design tools into mandatory HE stages.
- Do not adopt a remote agent control plane, general desktop-control permissions, always-on hooks, fuzzy mutation tools, Chroma, or a spec graph based on popularity.
- Do not create new HE decisions while HE-002 remains pending HE-001; research sources and synthesis are appropriate, implementation decisions require measured evidence.
- Do not interpret stars, before/after promotional screenshots, or self-reported savings as causal performance measurements.
- No code copied from external repositories; any future code reuse needs targeted license review and attribution.


## 2026-11-08 verified security addendum (subsequent source; does not change the Nov 7 pinned-code observations)

A second user-supplied video, [Beau Johnson — Five Trending Agent Repositories](https://www.youtube.com/watch?v=y2TmTP7qsvU), disclosed a claude-mem installer privacy incident that materially increases the requirements for any recall pilot. Full verified review: `01 Research/Sources/Beau Johnson — Five Trending Agent Repositories & Claude-Mem Consent Incident — 2026-11-08.md`.

**Confirmed by official [v15.8.3 notes](https://github.com/thedotmack/claude-mem/releases/tag/v15.8.3) and [merged fix #5220](https://github.com/thedotmack/claude-mem/pull/5220):** browser setup initiated by `npx claude-mem install` since v15.2.0 uploaded 30 days of agent prompt text and metadata to cmem.ai without on-screen consent. The Nov 2 v15.8.3 update added explicit local-only/upload consent; **it did not delete previously uploaded prompts**, and provider-side retention/deletion remained outside the fix. **Do not generalize** this to all plugin-only installations. The [Nov 7 v15.12.0 release](https://github.com/thedotmack/claude-mem/releases/tag/v15.12.0) also defaults to **retaining captured images locally**, a separate lifecycle/privacy consideration, not proof of default cloud upload.

**Updated disposition:** claude-mem remains a *conceptual retrieval-pattern* reference, but any future PMB recall pilot must first pass **specific installer/history-scope, transmitted-data, telemetry, retained-image, permission and deletion tests on disposable data**. Do not install it against real personal/company history merely to benchmark recall. No PMB/HE runtime change authorized.
