# AI Edge — Fable 5.2 Model Economics, Subagents, Effort and Skool Asset Accessibility — 2026-11-17

> **Access update — later on Nov 17, 2026:** The user provided full authenticated Skool lesson text and downloadable markdown/PDF resources for the model-routing guide, seven-repository pack, Graph Engineering/Clone Your Brain kit, and historical Fable 5 newsletter. The **earlier evidence gap and “request four assets” wording below describes the state before those downloads** and is retained as provenance, not the current access status. See [Downloaded Skool Guides, Model Routing, Seven Repos & Workflow Contracts](./AI%20Edge%20%E2%80%94%20Downloaded%20Skool%20Guides%2C%20Model%20Routing%2C%20Seven%20Repos%20%26%20Workflow%20Contracts%20%E2%80%94%202026-11-17.md) for the updated detailed review. The **Fable for Cheap executable ZIP/source remains uninspected**. No installer was run.


**Status:** HE source research, unreviewed. **No authorization** to execute third-party installers, change Claude settings/model defaults, or alter PMB, ACR, the independent Dashboard, HE runtime, CI, or work systems.

## Inputs and provenance

- User-supplied video: [AI Edge HQ / Miles Deutscher, *Anthropic Just Leaked How To Use Fable 5.2 For CHEAP! (insane hack)*](https://www.youtube.com/watch?v=sGH7TXA-BeA), time-stamped description and extensive screenshots (model/effort tables, official Claude docs, builtin subagents, effort inheritance, delegation, Claude API pricing, status update claim, "Fable for Cheap" setup, presenter 10-80-10 diagram). **Not** a full independently reviewed transcript or measured task log.
- [AI Edge Skool community](https://www.skool.com/milesdeutscher), [Classroom](https://www.skool.com/milesdeutscher/classroom), [Free AI Assets Library deep link](https://www.skool.com/milesdeutscher/classroom/69e1d07c?md=52272b1b89fd4daebdc6883a62f6a284), and [second lesson deep link](https://www.skool.com/milesdeutscher/classroom/69e1d07c?md=c839e60a8d1442018111e077c9ff7f2b). Public overview and indexed lesson names were visible; actual individual lesson bodies and downloadable packages could **not reliably be retrieved unauthenticated**. The whole library has *not* been audited.
- Independently corroborating primary material: [Anthropic/Claude developer guidance: Which Claude model should you build with?](https://claude.dev/blog/which-claude-model-should-you-build-with/), [Claude Code subagent docs](https://code.claude.com/docs/en/sub-agents), [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2), [What a task costs on Opus 5.5](https://claude.dev/blog/what-a-task-costs-on-opus-5-5/).

The video title's "leaked" and "insane hack" framing is marketing, not an evidence category. The useful mechanisms are first-party documented behavior and a third-party **uninspected** configuration package.

## Findings / disposition

| Video/topic | Evidence and scope | HE disposition |
| --- | --- | --- |
| Model-specific strengths and prices | First-party Anthropic comparison advises **Opus 5.5 medium** for broadly scoped daily engineering; **Sonnet 5.5** for well-scoped development; **Haiku 5.5** for narrow search/extraction/routing; **Fable 5.2** for especially difficult/long tasks. The model names, price table and provider availability are point-in-time; subscription quotas are not equivalent to API per-token invoices. | **REINFORCE** existing workload-based tiering; never hardcode video model names or prices into HE rules. |
| Cost of raising effort | More effort may improve difficult outcomes but can increase tool calls, token consumption and latency nonlinearly. Comparing *model × effort* and verified completion cost matters more than comparing raw rates. Changing a model/backend may change cache and capabilities. | **REINFORCE** real-task paired testing and outcome checks. |
| Built-in Explore is not always Haiku | Current first-party guidance indicates the built-in Explore subagent generally follows the main conversation's model (specific providers/aliases differ), not an unconditional Haiku default. A custom `Explore` subagent with `model: haiku` can be configured for bounded research, subject to version/precedence behavior. A generic `CLAUDE_CODE_SUBAGENT_MODEL` variable alone does not necessarily override built-in Explore or Plan. | **POSSIBLE PERSONAL EXPERIMENT**: verify effective model and failures on the user's installed Claude Code version before considering explicit overrides. No global default change justified yet. |
| Status updates cheaper | User's screenshot attributes an approximately **99% lower per-update cost** to short-note-based Haiku 5.5 status updates every 30 seconds. This is a specific vendor/post claim about individual background status updates, **not** 99% lower whole-session spend. | **RECORD ONLY** unless real usage shows background status cost material. |
| 10–80–10 lead/delegate/check rule | Presenter heuristic assigns strongest model to plan/review and cheaper models to bulk execution. It does not establish empirically optimal time/token split. Hard migrations and integration debugging still need judgment in execution; independent acceptance requires evidence, not model prestige. | **REJECT** as mandatory policy; **ASSESS** conditional delegation where clear contracts and strong verification exist. |
| Subagent proliferation / inherited effort | Anthropic guidance highlights subagent costs and warns that earlier prompts/skills can inadvertently encourage overspawning or needless approval. Screenshots show inheritance of main session effort by default. Actual override hierarchy differs for built-in vs custom agents and may evolve. | **REINFORCE** inspect effective model/effort and duplicate standing instructions before adding policy. |
| "Fable for Cheap" `install.sh` and custom agents | Video shows `Explore.md`, `fable-reviewer.md`, and `CLAUDE.md` rules being installed, but the actual downloadable source/script and diff were not supplied or reviewed. **No basis to claim** it is safe, version-compatible, idempotent, reversible, local-only, or effective. | **DO NOT INSTALL**; request source package, inspect file writes, agent definitions, permissions, per-project vs user-global changes, uninstall path, secret/network handling and tests. |
| "Leak" / always use Fable | A vendor-authored model-selection guide is not a leak. Complex-model delegation only helps if output checks detect cheap-worker mistakes and costs include coordination/rework. Fable capability on a benchmark does not establish superiority for a user's actual tasks. | **PARK** blanket Fable use and claims of guaranteed savings. |
| Token resets and promotional period | Screenshots show a **Nov 9–16, 2026** Fable inclusion promotion and time-limited reset credits. It has expired as of this research date (Nov 17). Do not infer present entitlements or future pricing from the promotion. | **ARCHIVE** as historical context only. |
| "Review entire repo and fix what is wrong" prompt | Screenshot of a broad autonomous refactor prompt is not a safe audit workflow. It can introduce speculative cleanup and alter fallback/CI/compatibility code. | **REJECT** autonomous blanket-fix behavior; preserve evidence, owner identification, narrow fix, independent verification. |
| Personal creator bench/examples | Screenshots show costs rising sharply with extra effort in one example; no controlled fixture, run repeats, logs, pricing basis, or independent acceptance were provided. | **DO NOT GENERALIZE** numerical ROI. |

## What the accessible Skool catalog suggests (titles only)

Promising indexed subjects to inspect next:

1. **Fable for Cheap Skill** — obtain *original source package*, not a copied command to run.
2. **Cut Claude Code Costs With Smarter Model Routing** — ask for exact config/snippets, defaults, prerequisites, version-specific behavior and measured user tasks.
3. **5 GitHub Repos That Fix Claude's Worst Habits** — collect exact upstream repo URLs, commits, license and evidence instead of trusting a curated title.
4. **Why Graph Engineering Will 10x Your Claude/Codex** — investigate only if it identifies an actual recurring cross-document retrieval/ownership failure; graph-by-default is unjustified.

No claim of having read restricted/JS-rendered lesson content. The user's authenticated-browser PDF export, Markdown, raw ZIP, screenshots or file attachments can close this evidence gap. Avoid sharing login cookies, tokens, private account exports, or corporate data. Do not ask the user to run `install.sh` or any arbitrary pasted executable.

## HE owner mapping; avoid duplicate doctrine

**Already owned by:** [Model-Tiered Workflows & Independent Factory Assurance — Sep 24](../Model-Tiered%20Workflows%20%26%20Independent%20Factory%20Assurance%20%E2%80%94%202026-09-24.md) (workload class, risk, verification, measured fitness); [Context Engineering](../Context%20Engineering.md.md) (scope of loaded instructions, skill discovery, model/runtime interactions); [Session Rollover, Handoff & Verification](../Session%20Rollover%2C%20Handoff%20%26%20Verification%20%E2%80%94%202026-09-16.md) (token spend ≠ active context); [Risk-Directed Review & Release Proof](../Risk-Directed%20Review%20%26%20Release%20Proof%20%E2%80%94%202026-09-24.md).

This source reinforces rather than replaces those decisions: **route by actual workload and verification**, not stage labels; do not treat a model reviewer as an independent acceptance mechanism; keep setup/skill modifications explicit and reversible; do not convert a creator heuristic into an organization-wide mandate.

The [AI Engineering Cockpit draft PR #28](https://github.com/unyieldingclaw-dev/harness-engineering/pull/28) remains a separate, parked scope. This note does **not** add a provider router, dashboard adapter, plugin, autonomous orchestration, user prompt harvesting or any cross-Home/Work data flow.

## Conditional small test only if the Skool source warrants it

- **Static review first:** extract ZIP to a disposable directory, list every file, inspect script side effects/network calls, agent frontmatter, versions, user-global vs project-local writes, permission rules and rollback. No `curl | bash`.
- **Baseline:** take one actual personal Claude Code task where built-in Explore or repeated low-stakes retrieval demonstrably consumes material quota; capture version, provider, main/subagent effective model/effort, total token usage, elapsed time, actual acceptance results and human corrections.
- **Treatment:** compare same task in fresh sessions with one explicit custom low-tier search agent versus built-in behavior; preserve strong model judgment and independently verifiable result check. Repeat enough to avoid a one-run conclusion, note cache, failure and retry effects.
- **Adoption gate:** savings must survive cost of setup, extra errors, retries and maintenance. If no measurable benefit, keep normal defaults. Never override security hooks or apply this to work devices without approval.

## Decision

**Research retained; implementation deferred.** The Skool course index is useful for discovery, but lesson source files are the missing evidence. Request four specific assets (above), not the entire library. No changes to existing HE principles or any runtime are justified by these screenshots alone.
