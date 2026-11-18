# Lydia Hallie — Rethinking Claude Code Practices, Evidence & HE Mapping — 2026-11-18

**Status:** Source research / review only; **not** an implementation approval, migration instruction, or claim of measured gains in our repositories. **Owner:** Harness Engineering research. **Scope:** Claude Code execution boundaries, developer instructions, verification, parallel sessions, and relevance to PMB/ACR/HE and the separate Home/Work dashboard.

## Sources and provenance

**Primary (direct talk):**
- Lydia Hallie, Anthropic Claude Code team, **“Rethinking Claude Code Best Practices”**, AI Engineer CODE 2026, November 11, 2026: [AI Engineer first-party event landing page, reading edition and complete timestamped transcript](https://ai.engineer/code/2026/talks/lydia-hallie); [full recording](https://www.youtube.com/watch?v=vlfCjWuv4es). The conference says its interrupted livestream was replaced by a complete recording (17m31s). This source takes precedence over secondary retellings.
- Anthropic's [Configure auto mode](https://code.claude.com/docs/en/auto-mode-config), [Hooks reference](https://code.claude.com/docs/en/hooks), [Auto mode default and study, August 7 2026](https://claude.com/blog/auto-mode-default-in-claude-code) (publisher redirect may lead to resources/articles URL).
- **Lydia-authored** first-party articles: [Choosing a Claude model and effort level in Claude Code — July 7 2026](https://claude.com/blog/claude-model-and-effort-level-in-claude-code) and [Maximizing the value of your Claude Code sessions — August 14 2026](https://claude.com/blog/maximizing-the-value-of-your-claude-code-sessions).

**Secondary/user-provided:**
- Nate Herk, [“Anthropic Just Released the New Rules of Claude Code”](https://www.youtube.com/watch?v=8Pg6G5h05hk), roughly 9m34s, user-provided 89-line time-aligned transcript and 37+ screenshots from November 18, 2026.
- Downloaded user-provided **8-page** `Anthropic Engineer Explains_ The New Way to Use Claude Code.pdf`, extracted and visually supplied, Nate Herk/AIS. User-provided **7-page** `Every Claude 2.0 Concept Explained_ Cheat Sheet.docx`. Separate [Video Database Google Sheet](https://docs.google.com/spreadsheets/d/1h5k7vAAXl5_f4VtguDkHwCCy-S-dd7RfG3-rUiMxfPw/edit?usp=sharing) opens publicly as a *video index*, with limited browser-rendered access to tabs/rows; **not** treated as engineering evidence.
- User-provided `ai-os-template.zip`, `nateherk-claude-code-mods.zip`; code-only static inspection in companion note. [Roast Room repo](https://github.com/nateherkai/roast-room); read README, `CLAUDE.md`, `package.json`, `.claude/skills/roast-room/SKILL.md`, roster. No user model settings or private project files inspected.

**Evidence tier:** Hallie's talk and her signed articles are attributed first-party practice and official product statements; not controlled outcome measurements for our workload. Anthropic product documentation governs semantics on the documented runtime version. Nate's guide is secondary and promotional; his ZIP code is evidence of *that code's behavior*, not evidence of product safety or effectiveness.

## Executive findings

The central point **is not** “turn on autonomy everywhere.” As people stop manually steering/approving/checking, each responsibility must be explicitly rehomed to a more reliable owner: **model reasoning**, **harness/runtime enforcement**, **environment isolation**, **executable checks**, and **human decisions**.

The best fit with HE is **control-ownership and evidence-based simplification**, not another agent control plane. This corroborates:
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md` — rules vs enforceable permissions/sandbox;
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md` — evaluate before promoting or removing learned rules;
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md` — evidence that checks *discriminate between plausible wrong outcomes*;
- `01 Research/Context Engineering.md.md` — task-specific context, minimum durable sources, no instruction sprawl.

## Source claim audit: the five jobs

| Job | Source-supported mechanism | Important boundary and HE takeaway | Decision |
| --- | --- | --- | --- |
| **1 Steering** | Better models need fewer universal “IMPORTANT / keep going / think harder” corrective phrases; Hallie suggests reviewing `CLAUDE.md` by asking which facts are not observable in code. `/doctor prompt-audit` was explicitly mentioned in the talk and her November 12 post. | Do **not** mechanically delete operational rules, source-ownership decisions, compatibility/fallback constraints, or authority boundaries on the assumption that code inspection reveals them. A stronger model can replace *behavioral scaffolding*; not facts the model cannot infer nor permission enforcement. Need local evidence before changing instructions. | **Reinforce** HE prompt health / no bulk purge |
| **2 Approving** | Claude Code auto mode inserts a permission classifier; routine actions can proceed, uncertain/forbidden actions get blocked or escalated. Auto mode defaults were introduced for qualifying new sessions on Pro/Max/Team in August 2026, not every environment or existing configured session. User-level `autoMode` settings can describe trusted environments and rule exceptions in prose. | A classifier is **not deterministic authorization**. Scope to actual workspace/remotes and provider/runtime version; avoid expanding `environment` or `allow` across corporate, cloud, home and personal repos by copying screenshots. Explicit user and organizational approvals still matter. | **Research only**, no user/work config change |
| **3 Saying no** | Hallie recommends deny rules where one mistake cannot be unwound: secrets, uncommitted work, irreversible external effects. `permissions.deny` checks *matching tool calls* before the auto classifier. Approved `npm test` can run untrusted dependency code that still accesses secrets unless the **execution environment** denies file/network access. Shell sandbox covers child processes but does not directly constrain every MCP, hook or non-shell tool. Cloud machines isolate local untracked secrets only if not sent/copied into them. | Evaluate composed authority, not a single allowed command. `Read(./.env)` is **not equivalent** to guaranteed OS-level isolation or a universal no-exfiltration guarantee. Corporate connectors, network, uploaded repo contents and remote commits remain separate egress channels. Git restore is not reliable for untracked data, secrets, published commits or external messages. | **High-value boundary confirmation** |
| **4 Checking** | Explicit definition of done, relevant test after a meaningful stage, show actual output, hooks (`PostToolUse`, `TaskCompleted`, `Stop`), CI, independent reviewer in fresh context. Hallie's auth migration shows multiple superficially plausible but wrong outcomes (legacy left behind, partial login coverage, existing users logged out). | A run claiming “done” is not evidence. Select **discriminating tests**, e.g. retained sessions after auth migration, not just generic test pass. Hook exit 2 can block a task/stop, but is event-specific; a failing command itself isn't always a hook failure. Avoid automatic full-suite Stop hooks where repeated suite costs, unrelated flaky failures, Windows shell semantics, or loop override would degrade reliability. | **Already fits HE**; no mandatory new hooks |
| **5 Starting sessions** | One coordinator starts multiple full cloud sessions and checks their results; multiple model roles (example coordinator Fable and worker Opus) are discussed. Product beta/plan/role availability is time-sensitive. | Distinguish coordination/orchestration UI from reproducible correct outcomes. Multiple sessions introduce merge conflicts, wasted calls and ambiguous ownership. A fresh reviewing agent can reduce shared-context bias but is not independent test evidence. No requirement to build a session manager, agent registry, general purpose dashboard or use expensive models by default. | **Research only** |

### Specific quantitative claim (and its limits)

- The conference talk says Anthropic went from about **2,500 code changes merged/day in May to >30,000/day in September**. The supplied Nate transcript appears to transcribe “200” at the start, while Nate's PDF says 2,500. Use the direct source's 2,500; do not claim a precisely measured user productivity gain or attribute all growth to a single feature.
- Anthropic's August 7, 2026 post describes a **1,053-person controlled dangerous-command insertion experiment**, with **143/1,053 = 13.6%** human detections versus **937/1,053 = 89%** auto-mode blocks on those examples. About 11% of dangerous insertions still passed the classifier **in that experiment**; these are not false-positive rates, guarantees, data-exfiltration prevention rates, or benchmarks for PMB/HE. The study design is not a general risk model.
- Human-in-the-loop review and classifier screening are both imperfect; hard enforcement, bounded environment, and real validation matter more than an “automatic approval” label.

### Corrections and caveats to Nate's instructions / screenshots

1. **“Cut everything the code can show” is too broad.** Relevant human-approved constraints can encode compatibility rationale, platform fallback, safety and externally owned facts even when their *symptoms* appear in code. Apply this only to demonstrably redundant model-compensation guidance.
2. **Nate's “exit 1 means marked completed; exit 2 fixes it” is simplified.** Per Anthropic's Hooks reference, on block-capable **Stop** and **TaskCompleted** hooks exit 2 blocks completion and sends stderr to the model. Other exit codes generally mean hook error with event/failure-handling nuances; `PostToolUse` cannot reverse an edit. `onFailure`, malformed outputs and Windows Git Bash vs PowerShell must be reviewed. A pass/fail test runner normally exits 1; a deliberate hook wrapper can map failure to 2, but this is not a universal script rule.
3. **Auto mode is not a trustable replacement for environment policy.** The documented control layers are `permissions.ask`, `permissions.deny`, classifier's `autoMode.environment`/`allow`/`soft_deny`/`hard_deny`, sandbox, OS/network/connector permissions and CI; each has different scope. On user settings `autoMode` list overrides require keeping `"$defaults"` to retain built-ins. Do not paste example `acme` repositories, cloud buckets, or “deploy to staging allowed” into real settings.
4. **Sample deny rules are narrow matches, not comprehensive data-loss prevention.** Read/Bash patterns can have coverage gaps and shell-equivalent variants. The real requirement is protecting secrets from the executing process with OS/sandbox/network constraints and excluding them from project/repo sync.
5. **Cloud environments are disposable, remote effects are not.** Commits, force pushes, messages and connector writes persist. Source-controlled deny rules may apply in a project but don't supersede corporate policy or imply the same tool availability in local, cloud, or Work sessions.
6. **Full test suite on every Stop** could be a serious recurring cost and cause retries on unrelated failures; choose a bounded deterministic gate keyed to the task and CI policy, not a blanket hook.
7. **Automatic merging and parallel sessions** are not general recommendations. User-owned approval must still gate material code changes, protected branches, published artifacts, security-related changes and unrelated cross-project work.
8. The PDF code samples must be **revalidated as actual JSON/shell** before use; extracted text contains layout/zero-width formatting anomalies. We have not executed them. `/doctor prompt-audit`, `/auto-mode-setup`, `/sandbox`, `/goal` and projects are **version- and platform-specific**, not portable primitives across Claude, Cursor, Gemini, and Codex.

## Additional high-value Lydia Hallie material

### A. Choosing model vs effort (Lydia, July 7, 2026)

Source: https://claude.com/blog/claude-model-and-effort-level-in-claude-code

A particularly useful diagnostic distinction: **insufficient capability** vs **insufficient effort/execution**. If the model read all relevant context, checked the right evidence, and still failed, *consider* a more capable model. If it skipped files, tests, or stopped prematurely, consider effort/workflow correction. Model selection fixes capacity, not missing evidence. Effort changes how many steps, reads, and verification actions happen, not only thinking tokens.

**HE implication:** Before routing a failed subagent to Fable or adding orchestration, diagnose the actual failure and compare outcomes per completed task. This complements the draft Fable economics/source audit in [HE PR #35](https://github.com/unyieldingclaw-dev/harness-engineering/pull/35). Don't infer that higher effort or a larger model is always faster/cheaper.

### B. Maximizing Claude Code sessions (Lydia, August 14, 2026)

Source: https://claude.com/blog/maximizing-the-value-of-your-claude-code-sessions

First-party recommendations: `/clear` between unrelated tasks, choose model/effort before a session to reduce prompt-cache churn, use explicit @file mentions, avoid noisy tool output in the primary context, examine `/context` for hidden instruction/tool costs, and compact deliberately around breakpoints. These are bounded, operator-friendly controls.

**HE implication:** investigate unnecessary context loads / duplicated instructions when there is an actual token/time burden. Neither prompt-cache behavior nor billing implies a passive “keepwarm” background ping is valuable. Compare against session duration, plan limits, and outcomes; do not introduce recurring token-consuming keep-alives absent measurements.

### C. Lydia GitHub and educational material

- Public profile: https://github.com/lydiahallie
- `lydiahallie/issue-tracker-mcp`: https://github.com/lydiahallie/issue-tracker-mcp — a deliberately **incomplete learning exercise**: small streamable-HTTP MCP issue board with one listed tool and four TODO tool registrations; solution branch. Good reference for **minimal tool contracts and inspector testing**, **not** a hardened authenticated production MCP service or an HE requirement.
- `lydiahallie/javascript-questions`: https://github.com/lydiahallie/javascript-questions — 2019-origin JavaScript quiz/explanation corpus; clearly marked by the author as covering historical syntax. Useful for developer education, not new harness architecture.
- `lydiahallie/javascript-react-patterns`: https://github.com/lydiahallie/javascript-react-patterns; `lydiahallie/javascript-guide`: https://github.com/lydiahallie/javascript-guide; the [JavaScript Visualized series](https://dev.to/lydiahallie/series/3341) and [theavocoder](https://theavocoder.com/index.html). Valuable developer-education resources about JS execution/model; little direct HE architectural novelty.
- `codebuddy-fs` and other demo repos appear in her GitHub account, but **were not individually audited**. Do not infer they implement Claude Code's internals or are production recommendations.

**Conclusion:** More relevant than cloning Lydia's repos is studying *her first-party operating guidance* and minimal runnable educational examples. The user's HE/PMB/ACR state already handles much of the proposed work.

## Concrete assessment for current projects

| Project | Actual fit / possible useful next action | Explicit non-action |
| --- | --- | --- |
| **HE** | Map Hallie's change in where responsibilities live against existing bounded execution / artifact-gated eval notes; retain observed-vs-hypothetical distinction. Optional **read-only** review of stale model-specific instructions; require evidence for deletions. | No second harness controller, global prompt purge, new loop framework or universal hooks suite. |
| **PMB** | Its durable canonical sources, provenance and compatibility routes **must not** be deleted because a modern model can inspect code. Prompt-audit findings are advisory. | Do not disrupt the pilot/release or install AI OS templates; no replacement memory DB. |
| **ACR** | Independent finding verification is aligned; remember November 16 Devstral large-diff agent timeouts were classified correctly. A code-review agent's approval is not stronger than a targeted reproducible test. | No new agents, model routing, forced 16K context, automatic retries or timeout changes from this video. |
| **Dashboard (PR #28)** | Possible later visual of real work items/verification/ownership when independently justified. | Do not turn it into a session/orchestration authority or link Home and Work secrets. |
| **Local Claude / Windows** | On a real recurring permission or accidental side effect, inspect exact effective permission mode, deny rules, sandbox availability, and shell. | Do not blindly paste screenshots into `settings.json`, use bypass mode, or run provided installer/scripts. |
| **Work** | Only organization-approved tools, boundaries, and data. Native Claude Code assumptions do not translate directly to corporate Cursor/Claude integrations. | No corporate file indexing, staging allowlist, auto-deploy, or MCP upgrades without explicit approval. |

## Minimal decision rule / stopping point

1. **First establish a real failure or waste:** what did the current model/rule/context/permission actually get wrong or duplicate?
2. **Locate its owner:** prompt/context fact, project config, permission system, sandbox, code, deterministic test/CI, external human governance.
3. **Propose the smallest evidenced correction** and keep the change reversible. Never treat risk of accidental exfiltration or irreversible deletion as a reversible code edit.
4. **Run a bounded reproducible check** on the original case plus a nearby negative case; verify behavior in a fresh session when changing instructions/routing.
5. **Stop when evidence supports no change.** A coherent large design or 30,000 changes/day headline is not a business case for implementation.

**Disposition:** research retained; first-party source wins; no activation, installation, merge, automation or configuration changes authorized.
