# Agent Distraction & Context Integrity — Continuation Checklist — 2026-11-18

**Research status:** OPEN / DEFER IMPLEMENTATION.  
**Parent evidence:** [Draft PR #39](https://github.com/unyieldingclaw-dev/harness-engineering/pull/39) and [Google source audit](./Sources/Google%20%E2%80%94%20Agent%20Distraction%2C%20Stale%20Instructions%20%26%20Context%20Integrity%20%E2%80%94%202026-11-18.md).  
**Current approval boundary:** documentation and investigation only. No automatic code changes, cleanup, archival, installation, or policy deployment.

## Purpose

Continue focused investigation into how stale and irrelevant context induces coding-agent side quests, incorrect instruction precedence and unnecessary tool calls. The goal is to determine whether a **small, read-only, source-owned discrepancy check** would solve an **observed** problem in Home or Work environments. Do not presuppose a new service, skill, scheduler, knowledge graph, dashboard, or archive.

## Already mined and documented

- Google primary article: bug-linked TODO encounters, unprompted actions and adjusted associated step/token/wait overhead; narrower internal human-reviewed reference cleanup.
- Xu/Wu stale-instruction experiment, Mehtiyev real-repo evidence, Chroma context-rot evidence, mixed AGENTS.md cost results and official Claude Code hook behavior.
- AI LABS video screenshots: /sweep prompt, project-specific verifier, proposed CURRENT / OUTDATED / UNPROVEN categories, three parallel validators, 62-claim example report, CLAUDE.md precedence rules, and archive guard.
- Operational caveats: no automatic code-always-wins precedence, apparent read-only agent includes Bash, screenshot uses --dangerously-skip-permissions, path-based archive hook is not a security control, archive artifacts still persist in Git, duplicate statuses create drift, model reviewers can share biases.
- HE concepts already applicable: Investigation Integrity, Evidence Before Architecture, Single Ownership, Context Engineering, Navigable Truth, bounded autonomy and verified handoffs.

**Source of detail, citations and exact findings:** the parent Google source-audit note. This checklist intentionally does not duplicate that evidence.

## Questions for focused follow-up

1. **Observed incidents:** What recent PMB/ACR/HE sessions actually exhibit a side quest, a wrong file/owner, reliance on stale instructions or repeated investigation from a stale reference? Identify concrete traces or diffs. Do not infer incidence from Google's population-level figures.
2. **Context authority:** For each incident, which source owned the information (approved requirement, current implementation, issue history, decision record, derived index, agent configuration, CI or runtime)? Was the issue a wrong source, bad source content, bad retrieval, missing task boundary or execution error?
3. **Ownership and timing:** How should in-progress plans, compatibility paths, current worktrees, CI-only references, reverted commits and historical decisions be treated? Explicitly classify **UNPROVEN** if present evidence does not settle intended behavior.
4. **Minimal prevention:** Could a task-local instruction such as 'leave unrequested issues alone; note discrepancies' prevent the example without a separate skill or new data model? Would correcting the existing owning document suffice?
5. **If necessary, read-only review:** What narrow recurring check could emit evidence-backed *proposed* corrections without mutation? Why would an independent verifier change confidence rather than duplicate uncertainty?
6. **Economics and false positives:** What labeled pilot cases establish added detection value and extra runtime cost? Define success as lower incorrect/unrequested behavior and acceptable review overhead; measure tokens and tool calls but do not optimize tokens alone.
7. **Work boundaries:** What investigation could run within sanctioned GitLab/Jira, enterprise models and approved data rules? No cross-home/work transfer, personal AI endpoint, global agent hook or unattended mutation.

## Gate-based research checklist

- [x] Preserve primary Google findings and distinct study provenance in HE.
- [x] Preserve video screenshot mechanism and caveats without installing it.
- [x] Cross-reference existing HE principles rather than creating duplicate policy.
- [x] Separate observed implementation, approved intent, historical rationale and task authorization.
- [ ] Collect 2-3 **actual** recent home incidents demonstrating stale context, wrong guidance or side quests; include source evidence and the cost of each failure.
- [ ] Classify each incident's **owning component** and smallest source-level durable fix; do not recommend workflow infrastructure if a document change solves it.
- [ ] Assess whether a simple task-local scope rule already addresses the problem.
- [ ] If repeatability warrants it, design a **non-mutating** test plan with 5-10 labeled cases including genuine obsolete pointers and deliberate nonobsolete negatives (CI/shell/fallback, future intent, working tree).
- [ ] Compare baseline vs candidate on false positives/negatives, unsupported edits, correct source attribution, tool calls, token usage, wall time and operational cost.
- [ ] Decide **reject, retain manual, or request separate implementation approval** based on findings. No automatic promotion to a project.
- [ ] Independently assess Work only after enterprise permissions, data-handling constraints and actual use cases are known.

## Explicit non-goals / guardrails

- No autonomous spec, code TODO, memory or decision deletion.
- No archives that an agent is expected to be technically incapable of reading solely through path-matching hooks.
- No global CLAUDE.md rewrite, --dangerously-skip-permissions, installed video skill or always-on scans.
- No unconditional code-over-policy precedence. Code proves observed behavior, not intended policy.
- No PMB release changes, ACR runtime changes or dashboard coupling.
- No synthetic claims of achieved 70% steps / 77% token / 105% wait improvements.

## Suggested new chat

**Title:** Agent Distraction & Context Integrity — HE Research

**Suggested opening prompt:**

> Bishop, continue the Agent Distraction & Context Integrity investigation as a distinct HE research thread.
>
> Start by reading HE draft PR #39, including the Google source audit, mechanism-level deep dive, and continuation checklist:
> https://github.com/unyieldingclaw-dev/harness-engineering/pull/39
>
> Use Google's November 6, 2026 Agent Distraction article, the independent stale-instruction studies, and the AI LABS /sweep walkthrough already analyzed as the evidence baseline. Do not duplicate that source mining.
>
> First examine whether our real PMB, ACR or HE work has **documented** incidents of stale guidance, unrequested side quests or context conflicts. Distinguish implemented behavior from approved intent, preserve CI/fallback/compatibility code unless proven obsolete, and classify uncertain findings as UNPROVEN.
>
> Identify the smallest correction owned by the actual source before proposing a new skill, architecture, memory subsystem, scheduled sweep or archive. Compare a simple task-level scope rule against any heavier review process. If pilot-worthy, design a read-only and reversible evaluation, **not implementation**.
>
> Keep Home and Work isolated, and make no changes to PMB, ACR, HE runtime, personal Claude configuration or work systems without separate authorization. Preserve further evidence and decisions in HE PR #39 or another explicitly justified research artifact. Be candid about operational cost, authority boundaries, uncertainty and diminishing returns.

## Resumption note

Recheck PR #39 and actual repository state before recommending further work. This document is a handoff and evaluation checklist, NOT proof that any incident exists or that the user approved a pilot. When no concrete failure can be identified, stop **operational adoption or a production sweep**; source-driven conceptual research and controlled, non-mutating fixture designs remain appropriate when they answer a distinct question.

---

## Repository incident evidence pass — 2026-11-18 (Home)

**Method and limit.** Inspected merged PMB/ACR PR narratives, relevant changed-file patches, and the current ACR `main` memory/docs. This is **retrospective, repository-documented evidence**, not a replay of original agent transcripts or a quantified incidence study. A PR description is a provenance-bearing report, not independent proof of every asserted test result. The cases below distinguish observed effects, source conflict, process cost, and plausible-but-unproven distraction. No runtime, configuration, Work, or repository other than this HE research branch was modified.

### Evidence-backed cases

| Case and source | What the repository records | Information/control owner; minimum disposition | Classification |
| --- | --- | --- | --- |
| **ACR [#77](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/77)** | `standards/MEMORY-BANK.md` repeated `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=50` in three places while the effective project's `.claude/settings.json` read `65`. Advisory-create upgrade semantics meant the stale adopter copy would not be overwritten. The PR removed the repeated value and pointed to the settings owner. | Project configuration owns the value; document only where to read it. Preserve PMB's distribution/override boundaries. | **Observed stale instruction value; corrected.** No evidenced wrong agent behavior attributable to the value. |
| **ACR [#79](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/79)** (after #78) | A memory-bank PR recorded a supposedly current `main` hash that its own merge immediately invalidated; two consecutive corrections shipped a stale tip. The PR removed that moving value and specified `git log` for current state while retaining immutable historical commit and tag identifiers. | Git owns the moving ref. Do not mirror it in durable session guidance. | **Observed self-invalidating memory; corrected.** |
| **ACR [#70](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/70)** | A previously correct release-tag cleanup command was rerun after state changed, deleting the *good* `v1.15.0` tag and returning the GitHub Release to draft. The release was retagged and republished. A guard from [#69](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/69) protected version-before-tag, **not** this deletion case. | Live Git/release state owns the precondition; revalidate destructive commands immediately before execution. A stale command is not made safe by archiving documents. | **Observed state-stale operational action with recovery.** No evidence of autonomous TODO distraction. |
| **ACR [#46](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/46)** | Review agents treated deleted `-` diff lines as surviving defects (8/8 in one fixture; a real #44 artifact also reproduced it). An explicit prompt correction still misfired 7/7. A post-image evidence filter dropped the one verified removed-code finding in a replay of 15 real findings; a wiring-seam regression test caught an initially inert implementation. | Diff/post-image semantics and `synthesize()` own the check; deterministic evidence filtering beat another instruction or reviewer. Keep deterministic-source exceptions and fail-open behavior. | **Observed context-interpretation/review defect; corrected in a merged PR.** Not documentation staleness. |
| **PMB [#175](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/175)** | **14 of 43 merged PRs in two weeks** existed only to record another PR because review-marker freshness and records-in-diff requirements interacted. The merged PR moved review records to PR-body blocks and periodic explicit batches. | Review lifecycle and record ownership, not a missing scan. | **Measured governance/workflow overhead; remediation merged.** Not evidence that an agent independently chose a side quest. |
| **PMB [#167](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/167)** | The agent-spawn advisory warned during an ordinary review: limit 6 versus normal review's 6–8 agents, idle-only reset and a checkout-wide shared counter; observed counts reached 27 and then 55. The merged change made a per-session rolling 60-minute advisory with a provisional 25-spawn threshold. | Hook implementation and performance-budget policy; only review the size threshold against measured usage later. PR states `systemMessage` is user-visible, **not model-visible**. | **Observed false operational signal; corrected.** Do not assert the WARN distracted an agent. |
| **ACR [#100](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/100) / [#101](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/101)** | README and memory bank continued to describe GitHub self-hosted review/calibration as live after the runner was retired **2026-11-09** and workflows disabled. The merged docs fixes correct the major claims; at this pass ACR `main` `memory-bank/techContext.md` lines 88–89 still advise retrying a timed-out CI `ai-review` job. | ACR's own docs and current GitHub workflow state. Residual line should be made explicitly *historical / conditional* in a future **authorized ACR doc edit**, not changed here. | **Confirmed residual stale guidance; no demonstrated user/agent harm.** |
| **PMB [#179](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/179)** | `mb upgrade` sourced files from the mutable `MB_HOME` working tree; a feature branch/uncommitted edits could be copied into another repo. A merged precheck now requires clean, current `main`; tests deliberately have a visible `MB_UPGRADE_UNCHECKED=1` escape. | Installer source-provenance invariant and explicit test harness. Preserve the test-only path, not treat it as dead bypass code. | **Verified unsafe pre-fix source path and fix; no specific accidental downstream install substantiated by this pass.** |

### Negative controls and important corrections

- **False stale claim withdrawn — ACR [#75](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/75).** Comparing PMB's source-repository `VERSION` and an adopter's installed-copy `.pmb-version` as if they were the same datum was a mistaken inference, retracted after checking the distinct consumers. Never "repair" such differences by synchronizing unrelated fields.
- **Historical compatibility/branch evidence — ACR [#71](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/71).** Squash-merged branch commits need not be ancestors of `main`. `git branch --merged` and `git branch -d` refusing a branch do **not** establish unmerged work or dead history. Check PR head identity and both directions before any branch deletion; none authorized here.
- **No lossy context pruning — ACR [#77](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/77) and [#82](https://github.com/unyieldingclaw-dev/ai-code-review-agent/pull/82).** Cap-driven condensation silently removed substantive operational lessons on a first pass; review restored them. A gitignored `handoff.md` held uncommitted operational context. Intentional archives and pointers preserve forensic reasons; an agent may not infer obsolescence from absence in startup context.
- **HE [#21](https://github.com/unyieldingclaw-dev/harness-engineering/pull/21)** records nonportable historical citations and unmerged research drafts. That is documentation provenance work, **not** a verified example of an HE agent independently undertaking an unrelated task. An unmerged draft is not authoritative `main`, nor automatically stale/dead.

### Diagnosis, comparison, and decision

**Finding:** The repository history does contain real stale-memory values, self-invalidating status facts, misleading guidance, diff-context errors, scope/control friction and release-state mistakes. It does **not** establish the frequency of **unrequested TODO-induced detours** in our own agent sessions or show that a repository-wide sweep would prevent these disparate classes. Causal claims and costs should stay attached to each case, rather than converted into a pooled "agent distraction rate."

**Smallest known effective action, in order:**

1. **Correct the owning source or retrieval pointer** when a contradiction has an identified owner; do not copy mutable values into additional instructions. If a runtime failure is demonstrated, test the real consumer/wiring seam. Do not auto-rewrite approved intent to match code.
2. **Maintain task-local authorization:** unrelated TODOs, bugs and speculative improvements may be noted in the result, **not** edited or started without the user's task authorization. This is an inexpensive instruction, not an additional project skill.
3. **Use one on-demand, read-only discrepancy investigation only when a specific current task encounters a contradiction** that cannot be settled by inspecting the owning source and history. Label unknown intent, in-progress work, fallback/CI code, or unmeasured effects **UNPROVEN**.

**Decision as of 2026-11-18: RETAIN MANUAL; DO NOT RUN OR BUILD A SWEEP.** Most directly relevant cases already received source-level corrections. Retrospective PR counts are not an agent-session denominator. There is no measured remaining repeat pattern or prospective ROI supporting new skill, archive subsystem, background process, dashboard, multiple verifier agents, or always-on audit.

**Reopen condition (decision heuristic, not a measured threshold):** at least two separately documented *current* task-scope/context failures on comparable Home tasks after the existing corrections, or one high-impact, clearly attributable unauthorized mutation. Capture original requested scope, encountered context, behavior/diff, owning source and history, operator rework, and baseline run cost. If met, use the already-defined **5–10-case, labeled, disposable-checkout read-only pilot** to compare no check, a task-local scope rule and a narrowly triggered discrepancy check. Count true/false positives, unsupported "obsolete" calls, unrequested edits, owner attribution, user review effort, tools, tokens, wall time and maintenance; require an actual win before requesting implementation authorization. Otherwise stop.

**Work boundary unchanged:** no corporate source/telemetry was inspected here. Home evidence and code must not be transferred to Work; any Work assessment needs independently sanctioned data, tools and authorization.


---

## Scope correction — mechanism-level research, not a sweep-implementation decision

The retrospective incident matrix is useful for identifying demonstrated Home failures, but the **no-sweep** decision must not be generalized into **stop researching context integrity**. The dedicated deep dive in this PR addresses *five distinct failure mechanisms*: unrequested TODO detours; stale instruction precedence; documentation debt with successful-but-wasteful recovery; in-session stale tool observations (Concord); and trajectory/specification drift (Wink). These require different evidence and controls. Neither the lack of a Home detour rate nor the existence of current source-owned fixes disposes of the broader research question.

**Read first:** `01 Research/Agent Distraction & Context Integrity — Mechanism-Level Deep Dive — 2026-11-18.md`.

**Decision unchanged:** no production sweep, autonomous archival, global instruction rewrite or work integration authorized. **Research decision revised:** retain the distinct mechanisms, threat/authority model, ranked lightweight controls, and eight-fixture experiment design for future validation. The 2-incident reopening threshold applies to *operational adoption*, not to conducting meaningful research.
