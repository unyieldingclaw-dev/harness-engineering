# AI Edge — Downloaded Skool Guides, Model Routing, Seven Repos & Workflow Contracts — 2026-11-17

**Status:** Research / user-provided primary artifacts reviewed; interpretations below are **not** an HE standard, installation instruction or implementation approval. Companion to [AI Edge — Fable Model Economics, Subagent Delegation & Skool Access — 2026-11-17](./AI%20Edge%20%E2%80%94%20Fable%20Model%20Economics%2C%20Subagent%20Delegation%20%26%20Skool%20Access%20%E2%80%94%202026-11-17.md) and draft [PR #35](https://github.com/unyieldingclaw-dev/harness-engineering/pull/35).

> **Nov 18, 2026 update:** The user supplied the previously missing original `fable-for-cheap.zip`. It has been **statically reviewed without execution**; see [Fable for Cheap ZIP — Static Source Audit](./AI%20Edge%20%E2%80%94%20Fable%20for%20Cheap%20ZIP%20%E2%80%94%20Static%20Source%20Audit%20%E2%80%94%202026-11-18.md). Earlier lines describing the ZIP as unavailable or awaiting review are retained below as historical evidence, **not current status**. The source audit recommends **do not install this version unchanged**, principally due to incomplete restoration, vulnerable marker removal, and an audit permission/sequence contradiction.

## Scope, provenance and access update

The user supplied **actual authenticated Skool lesson content and downloads**, closing much of the accessibility gap recorded in the earlier note. Sources reviewed:

1. **Miles Deutscher, `Fable 5 is the best model ever released. Here's how I'm using it without going broke.`** One-page June 2026 newsletter, user-supplied PDF (`claude-mythos-fable-5-guide-zj547h(1).pdf`); one same-byte duplicate file was also available. Historical **Fable 5 / Opus 4.8** framing, **not** a current Fable 5.2 benchmark or paid usage record.
2. **`Cut Claude Code Costs With Smarter Model Routing`** — user-supplied Markdown (`1794675881912-cut-claude-code-costs-with-smarter-model-routing-guide-...md`) plus full user message; links to a Google Drive **Fable for Cheap ZIP**. The actual ZIP or `SKILL.md` **was not uploaded** or executed; claims about its effects and rollback remain unverified.
3. **`The 7 GitHub Repos That Make Claude 10x More Powerful`** — user-supplied Markdown, same content in two uploaded exports (`Pasted markdown(20261118-010727).md` and `1789595179961-7-repos-claude-setup-guide-...md`); includes a *bulk installation prompt*. Claims and star counts are creator descriptions, not evidence of correctness or operational fit.
4. **`Why Graph Engineering Will 10x Your Claude/Codex` / `Clone Your Brain — The Complete Starter Kit`** — seven-page user-supplied PDF (`clone-your-brain---starter-kit-1-z6qzui.pdf`) and matching exported text (`Pasted text (2)(20261118-010756).txt`). A six-stage workflow documentation and cold-execution recipe, **not** evidence of graph database, graph retrieval or autonomous AI reliability.
5. Earlier supplied **`Keep Fable's Best Thinking After the Model`** course-page export and appendix/handover text, preserved in conversation. Not the same as the missing Fable-for-Cheap executable skill ZIP.

First-party cross-checks at research time:
- [Anthropic: Which Claude model should you build with?](https://claude.dev/blog/which-claude-model-should-you-build-with/) (Nov 9, 2026)
- [Anthropic: Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2)
- [Claude Code subagents](https://code.claude.com/docs/en/sub-agents)
- Repositories and their README pages linked in the inventory below. A source's GitHub existence is not a security audit or a claim that the creator's installation instructions are up to date.

**No source ZIP code, user Claude transcripts, observed quota data, or actual completed A/B runs were supplied.** Therefore do not assert that the skill is safe, that it reduces *subscription usage* by a given percentage, or that the bulk installer succeeds.

## 1. Fable / Opus 10–80–10: useful heuristic, not an allocation law

The June newsletter promotes using a frontier model to **plan** and **review**, a less expensive model to **implement**. The later tutorial upgrades the model names to Fable 5.2 / Opus 5.5 / Sonnet 5.5 / Haiku 5.5 and adds selective helpers. The content is internally coherent as a *creator workflow*, but the 10/80/10 numbers are **not measured stage proportions**, and “the middle is just typing” is an overstatement. Integration, migrations, difficult refactors, debugging and unanticipated failures can require the strongest reasoning during execution. A high-end review can still miss defects.

**HE disposition — REINFORCE existing workload-based model selection.** Model and effort should follow an actual task contract, observed failure cost, verification affordances, latency and completion cost. A stage label is insufficient. Explicit model choice is more maintainable than a speculative dynamic model router until repeated savings are demonstrated.

Anthropic's published guidance starts most engineering at **Opus 5.5 medium**, steps down to Sonnet for well-scoped coding and Haiku for narrow extraction/search, and considers Fable for unusually difficult/long tasks. It advises measuring model × effort on *your own tasks* with outcome checks and cost per completed task, not merely unit token prices. Provider/version and alias differences matter.

## 2. “Fable for Cheap” guide: what is actually documented vs what remains unknown

**Observed from user-provided lesson:** The vendor guide says it offers `/fable-for-cheap setup|report|audit|remove`; creates a custom Haiku search agent and a deliberately invoked Fable reviewer; adds helper instructions; offers to reset high effort; reads local Claude Code session transcripts to estimate per-model spend; backs up files before edits; and may offer one-at-a-time rewrites of three existing skills after approval. It proposes user-global locations such as `~/.claude/skills/fable-for-cheap/SKILL.md`, `~/.claude/agents/Explore.md`, `~/.claude/agents/fable-reviewer.md` and `CLAUDE.md` changes. Those are *descriptions* of the uninspected package, not verified side-effect boundaries.

**First-party corroboration:** Claude Code built-in Explore no longer unconditionally runs on Haiku. Anthropic describes explicitly defining custom `Explore` with `model: haiku`, and notes differences in provider/model aliases and versions. Subagents may inherit session effort; high effort and automatic delegation may make work more expensive. The custom agent is a plausible experiment, not an assumed working replacement on every machine.

**Accuracy limits of the cost report:** The guide's “typical job” uses **100,000 input + 20,000 output tokens**: Fable $1.60, Opus $0.80, Sonnet $0.40 and Haiku $0.02 at its quoted API list rates. The arithmetic follows the example but is **not a measured typical task**. It omits time, cache differences, thinking/tool turns, retries and rework. The guide also omits Anthropic's **Haiku 5.5 higher pricing when prompts exceed 100K tokens**. A user's Claude subscription is quota-based, not an API dollar invoice; estimated API-equivalent spend does not establish quota equivalence. Exact version, plan, provider and tier need checking before relying on the report.

**Static-review checklist if ZIP is provided:** archive inventory; original checksums/source authorship; `install.sh`, `SKILL.md` and agent frontmatter; scripts' shell commands and network side effects; what `setup`, `audit`, `report` and `remove` read/write; transcript privacy; idempotency and conflicts with preexisting `Explore`; global versus project precedence; backup and restoration behavior; permissions; version detection; and tests. Treat “backs up everything” and “nothing changes without approval” as **unverified assertions** until code and diffs are inspected. Do not run the package, `curl | bash`, or invoke blanket audit/rewrite actions merely to inspect it.

**HE disposition — CONDITIONAL PERSONAL EXPERIMENT, NO INSTALL YET.** If evidence of significant low-judgment search spend exists, compare one explicit, disposable lower-tier agent in fresh sessions with default behavior. Do not impose it on work or HE globally.

## 3. Seven repositories: inventory and risk-filtered triage

The Skool guide contains seven entries and a ready-to-paste instruction to install **all seven**, globally where possible. That mass-installation prompt is **not endorsed**: several entries alter global instructions or supply scripts; all add attack surface, upgrade/compatibility burden and maintenance. Popularity and screenshots are not acceptance tests.

| Repo in pack | Source-stated function | HE relevance / disposition | Risk or constraint |
| --- | --- | --- | --- |
| [mvanhorn/last30days-skill](https://github.com/mvanhorn/last30days-skill) | Multi-source recent trend research | **BORROW** source diversity/recency/provenance principles for HE research; do not assume an install is needed | Engagement ranking is not evidence quality; external calls, optional credentials, tool and token budget; redundant with existing research routines unless deficiency shown |
| [AgriciDaniel/claude-ads](https://github.com/AgriciDaniel/claude-ads) | Advertising account audits | **REJECT FOR HE** as unrelated | “Read-only by default” is package claim, not proof of connector/credential safety |
| [microsoft/playwright-mcp](https://github.com/microsoft/playwright-mcp) | Browser interaction via accessibility/DOM | **ASSESS ONLY FOR A REAL BROWSER QA/RESEARCH TASK** | Upstream explicitly warns **Playwright MCP is not a security boundary**; authenticated browsing can cause side effects; need permissions, isolated profile and scoped destinations. Does not guarantee captions/transcripts are accessible |
| [tt-a1i/archify](https://github.com/tt-a1i/archify) | Render visual system diagrams | **OPTIONAL PRESENTATION TOOL** when a decision actually needs a diagram | Generated diagrams can present inaccurate architecture as fact; require traceable source validation; may duplicate Mermaid or existing docs |
| [msitarzewski/agency-agents](https://github.com/msitarzewski/agency-agents) | Catalog of role/persona prompts | **REJECT BULK IMPORT**, selectively inspect a specific role if a repeated real need is identified | 200+ prompts inflate routing/context/governance; roleplay is not independent expertise; credential/access limits persist |
| [garrytan/gstack](https://github.com/garrytan/gstack) | Structured planning, review, QA and browse skills | **MINE INDIVIDUAL PROCEDURES** only after overlap check with existing HE research | Guide explicitly proposes changing *global* `CLAUDE.md`, overriding browser choice and installing dozens of skills; violates scope restraint by default |
| [block/buzz](https://github.com/block/buzz) | Shared human/agent chat, identities and logs | **PARK** as adjacent multi-agent collaboration surface, not a HE dependency | Guide is point-in-time; upstream README now references **[block/buzz-app](https://github.com/block/buzz-app)** for Buzz 1.0; do not follow old alpha download, SmartScreen bypass, or hosted relay steps automatically |

The guide's quoted star counts, exact supported clients and install flags are **not frozen facts**. Consult maintained upstream README and release notes at the time of a *specific, authorized* evaluation. Avoid running installers for research.

Particularly relevant distinctions:
- **Playwright MCP**: capability to operate a browser is not authorization, safety, a durable audit trail, or guaranteed YouTube video transcript access. If used for source gathering, preserve source URLs, data scope and an honest “not retrieved” result.
- **gstack**: a role-focused workflow may improve a task without needing the entire product's global browsing instruction, model permissions or skill catalog.
- **Buzz**: an agent coordination surface is not a justification for separate Home/Work dashboard capture, expanded telemetry or automatic tool delegation.
- **last30days**: community sentiment is discovery input; claims require source-specific validation. Popularity weighting is vulnerable to correlated repetition and social amplification.

## 4. Graph Engineering / Clone Your Brain: name overstates mechanism

The seven-page PDF's substantive proposal is **six stages**:

1. **Audit** recurring work, capturing trigger, steps, duration and judgment.
2. **Triage** with `Clone Score = hours/month × repeatability (1–5)`.
3. **Specs**: one workflow per file, named executors, inputs, outputs, failure rules and **explicit human review gates**. Use `[CONFIRM: ...]` rather than invented facts.
4. **Folder**: a narrowly scoped agent-facing README, versioned workflow specs, optional referenced context, and human-readable maps.
5. **Maps**: a separate generated interactive HTML diagram per spec (a visualization layer, not the authoritative process).
6. **Run / cold test**: a fresh agent reads README + spec, identifies missing context, executes until a review gate; update the *owning file* when it fails, then rerun cold.

The PDF says **three clean cold runs** before declaring a workflow “cloned” (page 6). That is a proposed confidence criterion, not statistically sufficient proof for all workflows.

**What HE can reuse:** clear owner/executor for each step; named entry/exit conditions; explicit external-action gates; missing-data placeholders; evidence-producing completion checks; and a fresh-session reproducibility test. These align with HE's single ownership, investigation integrity, bounded authority and re-verification research. The direct new emphasis is **cold executability from a self-contained workflow spec**.

**What not to adopt blindly:**
- `Clone Score` ignores impact, reversibility, privacy, variability, failure cost, automation maintenance and owner appetite. A high score is an *investigation candidate*, not authorization to automate.
- “One workflow = one file” is a useful default but not a demand to mirror all routine work in a new hierarchy. Reuse an existing runbook/spec when its owner and purpose are already clear.
- The agent-facing README's language that briefing rules override defaults cannot override system/security policy, tool permission boundaries or direct human authorization.
- A **textual REVIEW GATE** is not an enforceable control if the running agent already has a credential or tool that can send, delete, merge or deploy. For consequential effects, require tool-level bounded permissions and independent confirmation/receipts.
- A fresh chat is not necessarily **zero context** when global agent settings, project configuration, tools or memorized credentials load. Cold-run tests must state what context and permissions were present.
- The elaborate interactive map is presentation output; adding it to every runbook creates a second artifact to maintain. Generate when the graph materially helps understanding, and regenerate from the spec rather than maintaining two independent truths.

**HE disposition — BORROW A TEST PATTERN, DO NOT IMPLEMENT A NEW GRAPH PLATFORM.** A targeted cold run for a recurring, currently problematic HE research handoff or project runbook could demonstrate a missing context owner. No evidence presently justifies a graph database, universal “brain folder,” process-cloning platform, or autonomous personal operations stack.

## 5. Proposals, evidence threshold and owner

| Candidate | Existing owner | What would make it worth doing | Now |
| --- | --- | --- | --- |
| Explicit cheap search subagent | Personal Claude Code config, **not HE runtime** | Repeated real-task comparison: total quota/API-equivalent cost, outcome correctness, time and rework, model/effort confirmation | **Defer ZIP inspection / baseline first** |
| Risk-scoped independent review | Existing HE risk-directed release proof / artifact-gated SDLC research | A repeatable defect class or meaningful failure reduction, independent acceptance evidence | **Reinforce; no new rule** |
| Cold-executable workflow contract | Owning runbook or project spec | Documented fresh-session question/mis-execution with root cause attributable to missing spec/context | **Borrow selectively** |
| Research source-sweep skill | Existing HE research provenance methodology | Demonstrated blind spot not solved by current source selection and verification | **Park** |
| Diagram generator / agent chat platform | Human presentation / separate dashboard product, not HE | Measurably clearer decisions or confirmed operational need | **Park** |

### Smallest next steps

1. Preserve the user-provided source files **as evidence**, not by pasting lengthy third-party copyrighted guides verbatim into HE.
2. **Only remaining high-value download:** the actual **Fable for Cheap ZIP/source** linked in the routing guide. If obtained, perform static side-effect review; no install.
3. If interested in efficiency, take a baseline of **one real personal Claude Code task** with a low-stakes search phase and an independently checkable result; record current version, provider, effective subagent model and effort, elapsed time, cache/retries and outcome. Do not generalize from “typical job” examples.
4. Stop there unless a verified regression, costly behavior, or repeated cold-run failure merits the smallest correction **in its owning component**.

**Decision:** research recorded and source gap narrowed. Keep PR #35 **draft**. No new code, settings, skills, model router, Dashboards, PMB, ACR, cloud deployment, corporate access, or automation is approved by this research.
