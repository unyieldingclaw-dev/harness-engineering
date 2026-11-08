# Austin Marchese — Seven Claude Features, Mods, Project Threads and Source-Auditable Dashboards — 2026-11-08

**Status:** Source-grounded product/harness research only. **No installation, security-policy modification, model routing change, new dashboard, automatic memory rule, PMB/ACR change, or implementation approval.**

**Secondary source:** Austin Marchese, [*7 NEW Claude Features That Will Save You HOURS*](https://www.youtube.com/watch?v=BgbJFggbTVE), full user-supplied transcript (~17:30) and 20+ screenshots across seven features, submitted November 8, 2026. Creator promotes BuildPartner and a sponsored Bird integration; these are not independent evaluations. The headline “ahead of 99%” and “hours saved” are not measured in the supplied presentation.

**First-party verification (current Nov 8, 2026; features are rolling betas and docs can change):**
- [Anthropic: Claude in Google Docs, Sheets and Slides (Oct 6)](https://claude.com/resources/articles/claude-now-works-in-google-docs-sheets-and-slides) and [Workspace add-on scope/limitations](https://support.claude.com/en/articles/16951679-use-claude-in-google-docs-sheets-and-slides).
- [Anthropic: Claude Code Mods (Oct 1)](https://claude.com/resources/articles/claude-code-mods) — mods are unsandboxed and inherit full Claude Code process access.
- [Anthropic: Claude Dashboards](https://support.claude.com/en/articles/17454700-get-started-with-claude-dashboards) and [Artifacts overview](https://support.claude.com/en/articles/17153992-what-are-artifacts-and-how-do-i-use-them).
- [Anthropic: new Claude Code Projects beta](https://code.claude.com/docs/en/claude-projects) and [settings/cloud-thread exceptions](https://code.claude.com/docs/en/settings).
- [Anthropic: Claude Haiku 5.5 model/price](https://platform.claude.com/docs/en/models/haiku-5-5/overview), [measured cost/quality tradeoffs](https://platform.claude.com/docs/en/about-claude/models/optimizing-for-cost-and-intelligence).
- [Anthropic: Claude Docs](https://support.claude.com/en/articles/16923645-get-started-with-claude-docs) and [artifacts sharing](https://support.claude.com/en/articles/9547008-publish-and-share-artifacts).
- [Anthropic: Claude Code Remote Control](https://code.claude.com/docs/en/remote-control).
- [OneWave-AI/claude-code-mods](https://github.com/OneWave-AI/claude-code-mods), especially `burn-meter/hooks/register.tsx` read via GitHub; code accesses `$.session.usage().cost?.usd` and `usage.rateLimits`, not raw payment bills or universally compatible quotas. Static source inspection; no install, runtime behavior test, mod execution, or audit.

## Executive classification

**Most mechanics are already in HE research.** Unique useful evidence: *the effective permission/configuration boundary of Claude Code project cloud threads* and *small UI-originated review proposals/provenance views that do not assume authority*. A secondary contribution is that third-party code inside the coding harness carries **full process authority**, even when it only appears to change the visual interface.

Do **not** implement all seven features as an HE module list; separate vendor product features from portable mechanisms.

| Video time | Feature | Confirmed external product behavior | HE/PMB/ACR disposition |
| --- | --- | --- | --- |
| 0:00–1:42 | Claude inside Google Docs, Sheets, Slides | Google Workspace add-on beta available on paid plans with Google/admin authorization. Reads/edits **current open file** and sees selection, structure, native formulas/slides. Separate connectors can create/edit files from Claude. Version history supports inspection. | **REFERENCE**, no new HE subsystem. Google file ownership/permissions remain authoritative. Check beta restrictions, approved enterprise access and effective read/write/undo before use. |
| 1:42–5:00 | Mods, `burn-meter`, blast radius, `reflect` rule proposals | Claude Code mods are TypeScript event handlers that can alter prompts, tool calls, approvals and UI; **not sandboxed**, run with Claude Code's machine authority. Screenshot examples are illustrative, not first-party assurance. | **STRONGLY QUALIFY**. A scoped suggestion panel can improve human review but must not self-approve a governing policy. Native/managed permission controls, deterministic preflight and one-off reports remain safer than installing a broad mod solely for a cosmetic effect. |
| 6:20–8:17 | Claude Dashboards | Beta dashboards write/read queries against connected sources, reveal each chart's query and last refresh. Video example builds repo data files, refresh script, and pushes generated snapshot into artifact — **not intrinsically live data**. | **REINFORCE** source/metric semantics and evidence inspectability. A query shown is not a *correct* query. No new HE dashboard: existing derived-screen guidance owns this. |
| 8:17–10:37 | Claude Code Projects beta | Coordinator breaks stream into parallel **cloud** threads, each with separate context and branch/PR. Cloud threads get project instructions/files and repo `CLAUDE.md`; **do not load repository `.claude/settings.json` permission rules, hooks, env or repo `.mcp.json`**. Local project-thread mode uses local machine rules but is awake-dependent and does not automatically load project memory files. | **MATERIAL HOST-SEMANTICS GAP**. Same repo ≠ same active controls. Important acceptance issue when comparing/reproducing local HE/ACR/PMB results. No new multiagent framework. |
| 10:37–12:34 | Haiku 5.5 | Oct 7 launch; starting API $0.10 input/$0.50 output per million for <=100k-token prompts; **$0.50/$2.50 above 100k**. New tokenizer produces ~30% more tokens than Haiku 4.5 on equivalent text. “75% less” derives from Anthropic's selected workload comparison, not universal savings. | **TEST ONLY IF USEFUL** for high-volume checkable review/triage/extraction. Measure cost per verified task, false negatives and escalation burden. No blanket substitution or model hardcoding. |
| 12:34–14:44 | Docs, Slides and design system | Claude Artifacts are real editable documents/decks, with exports and sharing. Shared new artifacts require a Claude account; designs need substantive human review and accessible export. | **PRODUCTIVITY REFERENCE** rather than a harness pattern. Keep canonical project docs and source content outside the vendor when required. |
| 14:44–16:38 | Remote Control and keep awake | `/remote-control` connects a **local running** Claude Code session from browser/phone. Execution/file access remain on host; host must be awake. Settings can let phone start sessions in permitted folders. | **CONDITIONAL PERSONAL CONVENIENCE**. Treat device trust, folder exposure, permissions, mobile-session actions and on-host secrecy as a real authority expansion, not a free uptime hack. Leave `bypassPermissions` off; prefer native time-limited keep-awake to persistent background access. |

## Evidence that changes practical recommendations

### 1. Mods: visual customizations can secretly be powerful code

[Anthropic's own mod announcement](https://claude.com/resources/articles/claude-code-mods) explicitly says mods can **rewrite prompts, block/rewrite/retry tool calls, approve or deny permissions, alter results and draw UI** and are *unsandboxed*. A nice-looking usage meter is not a security assessment. Managed `sec-default` provides specific extra guardrails on configured Team/Enterprise/managed machines, but does not isolate arbitrary mod process activity. Installing a mod or allowing its self-generated code has the same basic trust challenge as installing any privileged extension.

**Burn Meter** is a real [OneWave AI project](https://github.com/OneWave-AI/claude-code-mods). Its static source reads host-reported `$.session.usage()` `cost.usd` and rate limits, stores an accumulated session/lifetime presentation figure, and draws UI. If using subscription plan access rather than metered API billing, the USD figure is a host-reported **equivalent estimate**, not necessarily the **actual marginal bill**. The five-hour and weekly windows come from host-reported rate-limit fields, when supplied; do not infer quotas from token cost. Those rate-limit semantics can change across plans. It would be easy to build a prettier meter that gives misleading precision; first ask whether the **native usage screen** already supports the actual decision.

**Reflect screenshot:** user says “in this repository use pnpm, not npm”; mod proposes “Use pnpm instead of npm in this project,” with 95% confidence and distinct “this project” versus “all projects” destinations. This is a useful *review interaction*, **not** evidence of correctness or user authorization. Verify `packageManager`, `pnpm-lock.yaml`, scripts/CI; route scope to project, not global, if accepted. Prevent duplicate/contradictory rules. Human selects **approve / reword / reject** and own authoritative file; verify task in a fresh session. Prior HE `Austin Marchese — Claude Dreams & Governed Memory Improvement` plus PMB Dream already own the lifecycle. Do not introduce a second learning/feedback service.

**Blast-radius screenshot:** pre-command preview shows `rm -rf build && ls` affecting 84 files (~604 KB). Deletion inventory can help a human decide, but command/pipe/glob/symlink/environment/TOCTOU effects defeat simple static pattern checks. A UI preview is **not** a full shell safety boundary. Destructive approval must remain an explicit control outside inferred textual assurances. It is **especially dangerous** to assume ignored/build folders are always disposable; inspect actual tracked/untracked files and task context.

### 2. Dashboards: explanation is not proof, and “Updated now” can mean “snapshot regenerated”

Anthropic's dashboard UI lets people inspect source query, formula and refresh time. This is valuable legibility. The screenshot of **VA Command Center**, however, says a Node script generates repository-relative data, then **Claude separately uploads five files** to the dashboard artifact for refresh; the displayed values are not inherently live. The creator explicitly notes that the dashboard cannot update itself simply by executing the repo script in the artifact.

**HE evaluation question:** which source owns the fact, what time is the copy from, which join/status classification/units produced the count, can a reader re-run the calculation, and what decision would change if the count moved? The exact chart formula can be reproducible yet semantically wrong (e.g. counts “in production” via folder names vs true issue status).

**Smallest viable pattern:** source query or deterministic script → preserved query/code and data source → last successful refresh + stale/error state → derived read-only view. **Do not build a cockpit** until recurrent operator decisions require one. Prior HE `AI Engineering Observability & Dashboard Boundary` and `Navigable Truth, Routine Manifests & Derived Screens` already cover this.

### 3. Claude Code Projects: real host drift, not just coordination convenience

**Important first-party exception:** [new Project cloud threads](https://code.claude.com/docs/en/claude-projects) have their own isolated context and cloud runtime. They get project memory/instructions and `CLAUDE.md` from repo, **but not** repository `.claude/settings.json` permission rules, hooks or env, `.mcp.json` or local customizations. The docs specify that local machine threads *do* see local permissions/hooks/MCP tools but may **not** get project memory files loaded. This differs from a conventional single-repo cloud session outside Project threads, which can read some shared repository settings. Distinguish exact execution mode; “Claude Code in the cloud” is not one homogeneous configuration.

The product also:
- starts new projects with **Opus high effort for worker threads** and coordinator low effort; parallelization can burn plan limits rapidly;
- uses **auto mode** in threads when supported; approval prompts must be addressed inside that thread;
- can auto-open PRs and keep **auto-fix** enabled to respond to CI failures/comments;
- stores simple “run at most two threads” requests as **model instructions**, **not hard enforced concurrency caps**;
- operates on github.com repos with Claude GitHub App, not necessarily corporate GitLab/self-hosted installations;
- may change merge/branch state; independent review and PR controls still own acceptance.

**Specific HE relevance:** before comparing a local ACR agent with a cloud project thread, record actual host, project/run type, active controls, installed skills, connector identity, tools, model/effort, branch/worktree, scope, approval flow and verifier evidence. Source names and repository identity alone do not establish equivalent permissions. **Re-use** existing host-portability + bounded-effects research, do not create a universal environment manifest spec.

### 4. Haiku 5.5: cheaper input pricing is not the whole economics

Anthropic's official [model specs](https://platform.claude.com/docs/en/models/haiku-5-5/overview) document a >100k-token prompt tier with **5× input and output prices**, and the newer tokenizer. Its [cost/performance guide](https://platform.claude.com/docs/en/about-claude/models/optimizing-for-cost-and-intelligence) explicitly advises **cost per completed task**, not per token. The cited ~75% total cost decrease from 4.5 comes from a particular internal 79-task comparison with prompt caching and different native thinking behavior. There are scenarios where stronger models solve more often or need fewer retries, yielding better total cost.

**ACR candidate only:** narrow extraction/classification or deterministic-check adjudication on a fixed benchmark corpus, with false-positive/false-negative and recheck/escalation rate. **PMB candidate only:** bounded retrieval intent classification if a demonstrated error rate justifies an LLM rather than deterministic dispatch. No plan/API-subscription arbitrage or team-scale use case is proven by this video.

### 5. Remote Control: same host, expanded reachability

[Anthropic's Remote Control documentation](https://code.claude.com/docs/en/remote-control) distinguishes:
- local session remote control: **host stays on**, local files/tools remain accessible; mobile/browser can steer the live local process;
- cloud session: runs separately in Anthropic's cloud and may continue without local computer;
- cloud project thread: distinct host config as above.

The creator's “awake 3h” example can be a reasonable time-bounded convenience, but should not automatically override battery sleep, device policy or employer safeguards. The desktop screenshot exposes settings for (a) auto-connect sessions, (b) allow phone/claude.ai to start local sessions, (c) keep host awake, (d) selected folder list, and (e) **Allow bypass permissions mode**. Avoid enabling all just to read status remotely. Review which directories and identities are reachable and ensure mobile requires device/session authentication; not all Remote Control paths use the same account/provider.

### 6. Google Workspace, Docs, Slides: product integrations, not infrastructure

The [Google Workspace beta add-on](https://support.claude.com/en/articles/16951679-use-claude-in-google-docs-sheets-and-slides) is one Marketplace installation covering Docs/Sheets/Slides (the video sounds as if “install again” is mandatory; the official flow says one install). Add-on acts on the open Google file and its version history; other Google Drive/Docs connectors have different scopes. An add-on may be blocked by organization policy. Not a basis to bypass employer collaboration controls.

[Claude Docs](https://support.claude.com/en/articles/16923645-get-started-with-claude-docs) and Slides are legitimate native editable artifacts. Their new shared links require recipients to sign in with Claude; supported export formats depend on the artifact type. For source-controlled HE research and specifications, canonical Git/Markdown remains preferable. A brand/design system for a real project can still be productive without a platform-wide identity/design system catalog.

## Decision and follow-up boundary

**REINFORCE EXISTING HE:** context ownership, reversible reviewed promotion, source/metric provenance, derived read-only screens, execution identity, host capability mismatch, least-privilege effects, verified model-tier fitness.

**Potential distinct evaluation triggers:**
1. **Cloud project-thread pilot:** only if switching among multiple related cloud sessions is a *current* pain point. First use one disposable repository and deliberately test which project rules, hooks and permissions are missing in cloud vs local mode; record effective controls and resulting actions. Respect PR review, budgets and tests.
2. **Usage visibility:** only if native plan-limit display lacks a useful answer. Inspect a mod's complete source and privileges first; compare host-reported quota numbers against the native source; don't confuse plan consumption with API invoices.
3. **Scoped memory promotion:** only if repeated same correction is a proven problem despite PMB/Claude settings. Add a narrow proposal-and-approve action in owning workflow *if necessary*; no automatically authorized `CLAUDE.md` edits.
4. **Source-auditable dashboard:** only if a real decision currently uses untraceable or stale numbers; preserve recomputable code/query and refresh receipt before designing charts.
5. **Haiku 5.5 replacement:** only for an observed repeated small-model workload with checkable outputs; compare solved-task cost, regression risk, and operator verification time.

**DO NOT ADOPT:** universal mod framework, arbitrary third-party mods, forced multiagent Cloud Projects, personal copilot-as-manager ideology, a new memory governance layer, a dashboard to make work look busy, automatic unattended remote control, new subscription plan, or API model routing without a measured need.

**Repository edit:** source note added to existing November 8 research draft PR. No existing HE code, settings, policies, PMB, ACR, test or deployment modified.
