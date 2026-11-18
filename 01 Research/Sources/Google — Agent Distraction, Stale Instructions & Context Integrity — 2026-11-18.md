# Google — Agent Distraction, Stale Instructions & Context Integrity — 2026-11-18

**Status:** Research only. **Disposition:** Reinforce existing HE investigation integrity; assess a small read-only pilot when justified; do not install the video's /sweep or change runtime. **Confidence:** high for Google's narrowly measured phenomenon; medium for broader documentation-conflict risk corroborated in other studies; low for the video's claimed implementation effectiveness.

## Primary and secondary evidence

- Sam Saccone and Madhuri Garikapati, **Agent Distraction**, Google Developers Blog, 2026-11-06: https://developers.googleblog.com/agent-distraction/
- Xiaonan Xu and Wenjing Wu, **Stale Instruction Files Override the Code in Coding Agents: A One-Sentence Fix**, 2026-10-22: https://arxiv.org/abs/2610.30571 ; replication dataset: https://github.com/WenJing95/stale-docs-data
- Tural Mehtiyev et al., **When Instruction Files Go Stale: Documentation Debt and Hidden Cost in Coding Agents**, 2026-11-06: https://arxiv.org/abs/2611.09802
- Thibaud Gloaguen et al., **Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?**, 2026: https://arxiv.org/abs/2602.11988
- Jai Lal Lulla et al., **On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents**, 2026: https://arxiv.org/abs/2601.20404
- Chroma, **Context Rot**, 2025-07-14: https://www.trychroma.com/research/context-rot
- Official Claude Code docs: https://code.claude.com/docs/en/hooks and https://code.claude.com/docs/en/permissions
- AI LABS secondary walkthrough, **Google Just Released Something To Fix AI Agent's Biggest Problem**: https://www.youtube.com/watch?v=JhZB3spnuIY ; user-supplied transcript and extensive screenshots of skill, verifier, rules, hook and reported results, 2026-11-18.

The demonstration's full executable skill and hook source are not available in these inputs. Screenshots are evidence of visible configuration, not proof of exact runtime security, reproducibility or measured value. No video code was run, installed or endorsed.

## Google's measured results — careful denominators

Google analyzed distributed agent traces from thousands of engineers and hundreds of millions of agent steps. Of **file-reading sessions**, 13% exposed the agent to linked TODO(bug/...) or FIXME(bug/...) comments. Agents took subsequent bug-targeting action in **6% of encounters**. Of those follow-up actions, **83% were unprompted**. These denominators differ; do not multiply them as one exact global session frequency.

Among unprompted detours, 32% were already fixed/verified, 9% obsolete or won't-fix, another 2% duplicate/restricted, and 57% still-open bugs outside user scope. For the still-open subset, 61% involved lookup only, while 39% edited code. Google used fixed-effects regression controlling for developer, codebase complexity, task type, model tier, number of files read, initial prompt length and encounter step. Unprompted detours were associated with **70% more steps, 77% more tokens and 105% more wait time** (p < 0.0001). This is observational association, NOT a guaranteed causal benefit or a savings forecast for an external sweep tool.

Google's actual intervention was narrower than the video: internally identify linked TODOs tied to fixed/obsolete bugs; inspect the bug record, file changes and history (sometimes years); propose removal ONLY when both comment and its reason were obsolete; submit every proposal for human review. Google reports thousands removed in six months and over 95% reviewer acceptance. Acceptance does not establish detection recall, false-negative rate or that the approach works for policy documents, memory and specs. The blog does not release a generic /sweep skill.

**Failure modes:** stale, resolved issue sends investigation into dead end; open but unrequested issue induces unauthorized scope creep; stale always-loaded guidance outranks current observed code; apparent dead code/comment is actually a safety, CI, fallback or compatibility constraint.

## Independent studies

**Xu/Wu:** 40 constructed Python projects, 16 kinds of convention, Claude Code and Codex CLI, 320 runs. Without a precedence rule, 90.1% of evaluated choices followed stale instruction-file guidance and only 4.2% followed observed code. A tested sentence specifically prioritized **existing code conventions** when documentation disagreed and required the agent to disclose conflicts. Code-following rose to 91.4%; test pass rate 89.4% to 100%. Claude frequently noticed the conflict while still following stale instructions; Codex rewrote existing code in 20/80 baseline runs. Generalization is limited to constructed convention-conflict tasks, not normative business requirements. The tested intervention does NOT say current code always outranks approved intent.

**Mehtiyev et al.:** 3 real stale instruction cases, 22 tasks, 732 agent trajectories. Stale test guidance produced about two extra tool calls in one model condition; task success could hide costs and agents rarely repaired the instruction itself. Not all cases demonstrated detectable overhead.

**Context-file economics are mixed:** Gloaguen et al. observed no general task-success improvement from AGENTS.md and over 20% higher inference cost in one benchmark; Lulla et al. observed lower median runtime and output tokens in a different 10-repository/124-PR experiment. Do not generalize either result. Persistent instructions should carry non-derivable, task-relevant information and be evaluated for net impact.

**Chroma:** 18-model controlled long-context tasks show non-uniform, often degrading reliability as context grows. Supports selective retrieval and intentional handoff, NOT a universal reset schedule or arbitrary token limit.

## AI LABS video's specific /sweep design — strengths

Screenshots show a project-local sweep skill, one separate sweep-verifier agent with CURRENT/OUTDATED/UNPROVEN classifications, a project PreToolUse archive guard, status labels on tickets, a CLAUDE.md scope and conflict rule, and a report using three verifier subagents. Its demo reports 62 checked claims, 5 out-of-date findings and 1 intent conflict. A concrete claimed mismatch is a route rename from products to catalog while CLAUDE.md still cites old locations.

Useful ideas:
- User task scope is an authorization boundary: unrelated TODOs are observations, not implicit new work.
- Require a particular source, file/line, issue status, git history, working-tree state and evidence before calling guidance stale.
- Separate a finding collector's suspicion from the verifier's input; allow UNPROVEN rather than forcing a binary conclusion.
- Distinguish intent and approved decisions from executable code state. An incomplete in-progress plan does not become stale because implementation is incomplete.
- Present proposed updates with exact old/new text and stop before any edits or archive moves.
- Use a fresh session for a genuinely new task when old failures or conflicting context risk contaminating execution.

## Design problems visible in the demo

1. The so-called read-only verifier declares **Read, Grep, Glob, Bash**. Unrestricted Bash is not inherently read-only. The natural-language label supplies no enforcement.
2. The video repeatedly uses **claude --dangerously-skip-permissions**. Do not normalize that for home or corporate machines carrying credentials. It also undermines the safety narrative.
3. Archive isolation is a **PreToolUse hook inspecting tool arguments**. One screenshot shows a block on a compound Bash command with an archive-path warning, then a reformulation succeeds. The full command is truncated, so whether the block was a false positive or a correct path rejection is unproven; neither outcome establishes robust isolation. A Bash interpreter, symlink, other tool, Git history, derived index or already-ingested context may defeat path-string matching. Unverified possibilities must not be asserted as successful exploit demonstrations.
4. Claude Code docs: PreToolUse **exit 2** blocks the current tool attempt and exposes stderr; **exit 1** is normally non-blocking; a timeout normally does not block absent an explicit fail-closed option. Hooks can be bypassed semantically if they check only lexical arguments. Treat policies requiring confidentiality with OS permissions, appropriate sandbox/ACL and product-managed policy, not an improvised repository hook.
5. Archive-in-repo does not remove history or keep text from future search/indexing; it may also impede forensic examination and provenance. Structural retrieval exclusions are better for relevance; real isolation is a different goal.
6. The screenshot treats 24 ticket statuses as incorrectly formatted because they are at line 10 rather than repeated in a header table; this looks potentially like format lint, NOT staleness. Creating a second status copy creates synchronization debt. Don't generalize to rewriting every ticket.
7. Three model verifiers can share data and reasoning biases. Fresh wording reduces anchoring but does not guarantee independent evidence, source correctness or authorization.
8. The demo's claimed five findings plus one conflict cannot establish false-positive/false-negative rates without a pre-labeled sample; scanning 62 claims and spawning agents costs money and maintenance. It does not prove the pipeline's ROI.
9. A blanket 'follow the app' precedence rule is unsafe for unimplemented requirements, incorrect code, regulatory/security policy and historical compatibility. Never infer a discarded intent solely from absence in code.
10. Code TODOs, CI/shell logic, fallback branches and compatibility paths require high proof of obsolescence; 'not recently executed' is not adequate.

## Practical authority model

| Question | Primary evidence owner | Safe action if contradictory |
| --- | --- | --- |
| What runs today? | Inspected current working tree/deployed code, actual runtime and tests | State observed behavior and version; surface mismatch |
| What SHOULD run? | Explicit current user-approved decision/spec/acceptance criteria | Seek owner decision; do not auto-rewrite intent |
| Why does this note or fallback exist? | Issue record, commit/change history, relevant consumers and constraints | UNPROVEN until verified; preserve by default |
| What may the agent do? | Current task and organizational permissions | Report out-of-scope observations; no speculative implementation |
| How should the agent find current state? | Project-owned source and derived index with provenance | Fix stale pointer at owning location after proof |

**Observation is not authorization. Intent is not implementation.** A current in-progress plan explains why code and intent may diverge, but does not override a current direct user instruction or policy.

## Home: minimum evidence-gathering pilot, not platform work

First identify at least one documented real recurring distraction from PMB, ACR or HE. Where justified, run an **on-demand, read-only, project-local audit** on a disposable checkout. Scope it to relevant task source pointers and linked TODOs rather than scanning the whole repository by default. Return CURRENT / STALE CANDIDATE / CONFLICT / UNPROVEN, quote the file/line, cite the source owner and history, explain why it matters to the task, propose the smallest change and stop without mutating.

Pilot with 5-10 labeled cases, including one genuine stale pointer, one still-relevant compatibility/fallback note, an intentional future plan, an in-flight uncommitted change, an outdated instruction and a policy-vs-code conflict. Compare no review versus review on representative tasks. Track false positives, unrequested edits, source attribution, number of tool calls, tokens/wall time, accepted proposals and recurring maintenance overhead. Do not claim Google's percentages as achievable savings.

Stop if speculative deletion candidates, irrelevant findings or tool overhead exceed observed value. Avoid always-on scans, automatic archiving, duplicated status tables, persistent new dashboards and archive hooks absent a demonstrated requirement.

## Work: gated and separate

Do NOT copy corporate data to personal Claude/ChatGPT, personal GitHub or local models. Start with approved enterprise providers and a sanctioned, narrowly scoped and ideally sanitized repository. Jira or the company tracking system owns issue decisions; GitLab and build/test systems own implementation evidence; policy owners determine protected data, access and release constraints. Start with a manual or report-only discrepancy review. Any team rollout needs explicit approval of source boundaries, operational ownership, human approval for proposed changes, retention, error handling, auditability and measured ROI. The Google results alone do not authorize such deployment.

## HE disposition and cross-references

Already covered in existing HE:
- Context Engineering — selective context and instruction minimization.
- Adaptive Autonomy, Diagnostic Gates & Fan-Out/Fan-In — investigate before mutation, bounded authority, independent reviewer caveats.
- Navigable Truth, Routine Manifests & Derived Screens — source ownership and disposable derived navigation.
- Session Rollover, Handoff & Verification — fresh handoff versus compaction and task-dependent context.

This source contributes **quantified production detour overhead, controlled stale-instruction precedence evidence, and visible failure modes in an attractive third-party sweep design**. It strengthens HE's investigation-integrity approach and does not justify parallel policy infrastructure.

**REINFORCE:** scope, source authority, historical intent, unproven classification, evidence-backed proposals, human final approval.

**ASSESS:** targeted read-only drift review only when repeatable distractions are observed.

**DEFER:** persistent scanning, archive/guard framework, scheduled sweeps, agent orchestration, universal memory pruning.

**REJECT AS DEFAULT:** code-always-wins, automatic delete/archive, semantic security via path substring hook, independent-model-as-proof, permission bypass and cross-home/work data transfer.

**No implementation authorized; this is source preservation and candidate assessment only.**
