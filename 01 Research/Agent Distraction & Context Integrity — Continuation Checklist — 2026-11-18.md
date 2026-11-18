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
> Start by reading HE draft PR #39 and its two committed documents:
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

Recheck PR #39 and actual repository state before recommending further work. This document is a handoff and evaluation checklist, NOT proof that any incident exists or that the user approved a pilot. When no concrete failure can be identified, stop at 'research documented, no further action justified.'
