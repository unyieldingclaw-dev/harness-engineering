# Observed Effects, Scoped Recall & Lifecycle Evidence — 2026-11-07

## Purpose and provenance

Synthesize HE-relevant engineering patterns from the 2026-11-07 review of The Next New Thing's *Hot: Agents get computers & fun annotation & free designs & more* and 15 named GitHub repositories.

Primary evidence ledger: [The Next New Thing — Agents Get Computers, Memory, Skills & Worktrees — 2026-11-07](Sources/The%20Next%20New%20Thing%20%E2%80%94%20Agents%20Get%20Computers%2C%20Memory%2C%20Skills%20%26%20Worktrees%20%E2%80%94%202026-11-07.md).

Related existing HE research, **not superseded**:

- [Skill Routing, Gate-Bound Evidence & Orthogonal Review — 2026-09-26](Skill%20Routing%2C%20Gate-Bound%20Evidence%20%26%20Orthogonal%20Review%20%E2%80%94%202026-09-26.md)
- [Derived Index Economics, Tool Steering & Structural Retrieval — 2026-09-25](Derived%20Index%20Economics%2C%20Tool%20Steering%20%26%20Structural%20Retrieval%20%E2%80%94%202026-09-25.md)
- [Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation — 2026-09-25](Monotonic%20Policy%2C%20Cross-Host%20Instructions%20%26%20Behavioral%20Skill%20Evaluation%20%E2%80%94%202026-09-25.md)
- [Harness Portability, Exit Cost & Inspectability — 2026-09-24](Harness%20Portability%2C%20Exit%20Cost%20%26%20Inspectability%20%E2%80%94%202026-09-24.md)
- [Session Rollover, Handoff & Verification — 2026-09-16](Session%20Rollover%2C%20Handoff%20%26%20Verification%20%E2%80%94%202026-09-16.md)
- [Budgeted Side Effects, Approval Gates & Receipts — 2026-09-25](Budgeted%20Side%20Effects%2C%20Approval%20Gates%20%26%20Receipts%20%E2%80%94%202026-09-25.md)

**Status: RESEARCH / ASSESS ONLY.** No decision to change HE implementation, PMB, ACR, instructions, agent permissions or workflows. HE-002 remains pending HE-001. No tests on third-party runtime behavior were executed.

## Executive conclusion

Newly observed *implementation details* matter more than newly invented principles:

1. Desktop automation can represent a bounded observation and its targets by **snapshot identity**, explicitly reject stale/ambiguous references, and re-observe the effect.
2. Multi-agent worktrees need **lifecycle closure**, with observable branch safety, source-control status and cleanup conditions, not just task spawning.
3. Generated session memory needs **scoped discovery and selective expansion**, with a visible boundary between historical observation and authoritative project facts.
4. Optimization claims need an **honest counterfactual**, not a model that assumes every fused call displaced several native calls.
5. Production readiness should be **stage-specific**, evidence-bearing, and distinguish unverified from failed or inapplicable.
6. A skill's content and packaging do not determine its **authority, routing, marginal value or cross-host equivalence**.

These mostly reinforce existing HE findings. The novelty is source-specific, executable patterns we can test when an actual failure makes them worthwhile.

## A. UI action correctness is an observed state transition

**Evidence:** vercel-labs/agent-desktop, README and CONCEPTS.md at b43ac4f6e892.

The implementation separates:

- observation: application/window identity, accessibility snapshot, completeness and target ref;
- requested action: target ref + operation and arguments;
- dispatch outcome: command accepted, rejected, stale, ambiguous or timed out;
- effect evidence: new observed state and task-specific postcondition;
- privacy/authority: which desktop surfaces and data the caller is permitted to touch.

An action reporting success is not proof that the intended user-visible outcome occurred. A screenshot is also not by itself evidence of *causality*. The caller must inspect the relevant state and compare against the promised postcondition.

**HE disposition: REINFORCE / possible future acceptance contract**, not a desktop-control implementation plan. This extends current bounded-side-effect and receipt work. No reason to grant blanket control to an agent on the primary machine.

### Smallest justified test

Disposable application and permission-scoped test profile; automate a reversible setting toggle. Capture pre-state, action result, post-state, ambiguity/stale errors, and mismatch reporting. Test negative cases (reordered controls, missing permission, incomplete snapshot). If existing automation already meets the acceptance threshold, stop.

## B. Workspaces are resources with completion semantics

**Evidence:** max-sixty/worktrunk at 71d99c7e6b5a, particularly docs/src/content/docs/remove.md; obra/superpowers using-git-worktrees skill at 22f0b463e7a5.

Parallel agent branches are execution containers with identity and lifecycle, distinct from canonical memory, requirements or completed work.

Useful minimum record:

| Field | Why it matters |
|---|---|
| Task / owner / branch / worktree path | Establishes which actor may change which checkout |
| Base revision and current HEAD | Reveals the actual change boundary |
| Dirty status and external effects | Prevents accidental loss |
| Test/acceptance proof tied to diff | Shows what was actually verified |
| Merge/PR state | Separates agent completion from integration |
| Cleanup disposition | Retain, remove safely, or hold for human review |

Worktrunk's multiple branch-equivalence tests demonstrate how nontrivial post-squash cleanup is. Its convenience syntax is secondary.

**HE disposition: ASSESS for concurrent-work failure classes.** A naive global worktree manager is speculative architecture for a single active agent. Prefer native git; only add worktree automation after measured repeated issues.

**PMB observation:** Current .gitignore already ignores .claude/worktrees/; current tracked .claude listing has no worktrees directory. An older architectural review mentioned this risk, but **a current remediation defect was not reproduced**.

## C. Session observations are evidence; project state is authoritative

**Evidence:** thedotmack/claude-mem at 2d8c08c182af, search architecture and SQLite/Chroma worker documentation.

The retrieval pattern is:

1. small result set with stable observation IDs;
2. optional neighboring timeline context;
3. fetch full detail for selected IDs;
4. verify against current source before updating project memory.

This is a good retrieval *interface* even if no vector store is adopted. Its implementation also reveals lifecycle cost: event hooks, a worker, SQLite, Chroma projection, reindexing and possible cross-store skew.

**Boundary:** PMB owns durable project memory; Git/code/config/tests are sources of current behavior; historical transcripts/observations are secondary context. A derived search index must not auto-promote speculative or old summaries to project truth.

**Disposition: PILOT ONLY IF NECESSARY.** Start by measuring missed recall, incorrect reuse of stale context and time spent finding evidence over a real set of sessions. Compare plain file and transcript search against a three-level retrieval UI before adding a permanent capture/index service.

## D. Displaced work is not the same as claimed savings

**Evidence:** Brucee/tokensaver at 8b8ab4d053b3, test/savings.test.mjs and README.

TokenSaver combines operations and estimates native calls avoided; its test encodes specific equivalent-call and per-call timing assumptions. Those can be honest **model outputs**, but do not establish the true no-plugin counterfactual.

A valid experiment must hold fixed:

- tasks and source revisions;
- model, thinking effort, tools, permissions and starting context;
- task success and regression requirements;
- actual input/output/cache billed token usage where available;
- end-to-end wall time;
- number of redundant reads or actions, plus retry/failure rate;
- plugin startup, maintenance and debugging costs.

Run paired trials with/without the selective helper. Report task-class results, not just a global percentage. Do not let hook-enforced tool usage defeat simpler existing commands.

**Disposition: REINFORCE current Derived Index Economics principle.** The smallest code-independent gain may simply be to ask tools for capped, focused results and batch safe *read-only* inquiries. Fuzzy multi-file mutation deserves separate review.

## E. Readiness gates are conditional on the system being released

**Evidence:** hallikkristjan-code/production-skeleton at 041cfbaa3093, README, skeptic-review skill and broken-app fixture.

A stronger acceptance record is:

| State | Meaning |
|---|---|
| VERIFIED PASS | A relevant check was run and supports this specific claim |
| VERIFIED FAIL | A relevant check ran and the stated expectation failed |
| NOT VERIFIED | Evidence is unavailable or the test was not run |
| NOT APPLICABLE | Outside this application's declared scope, with reason |
| WAIVED / DEFERRED | An owner explicitly accepted residual risk and scope |

Distinguish code-shape detection from actual effects (e.g., configuration for backup exists versus restore verified). Apply gates by release stage and consequence, rather than forcing all apps to have equivalent privacy, finance and resilience requirements.

**HE disposition: REINFORCE gate-bound evidence.** This may inform ACR reporting semantics, but ACR should not begin asserting live production readiness based only on diff review. For Side Quest Atlas, consider a one-time deploy-readiness audit at the relevant milestone.

## F. Skill and design-tool discipline

**Evidence:** obra/superpowers; anthropics/skills; Nutlope/hallmark; pbakaus/impeccable; jakubkrehel/skills.

- **Superpowers:** fresh verify-before-completion and spec-versus-quality review are useful reference implementations; universal multi-stage workflows are not a default recommendation for HE.
- **Anthropic Skills:** canonical skill packaging and simple metadata, with host adaptation explicitly verified.
- **Hallmark:** hard floor / waivable default / finish polish is a useful distinction, but stylistic preferences must not masquerade as universal safety invariants.
- **Impeccable:** narrow-target critique/edit and live feedback can reduce ambiguous visual review, but tool permissions and optional validation scripts matter.
- **Jakub Krehel:** WCAG 2.2 criterion-driven accessibility findings are stronger than generic design taste; meaningful keyboard/assistive-technology tests remain essential.

**Disposition:** UI tools can be evaluated per UI project (Side Quest Atlas or personal site). No evidence supports three required overlapping design skills, nor another full HE workflow policy. Keep one canonical style source per product.

## Proposed bounded evaluation queue (not implementation commitments)

| Priority | Candidate | Trigger / observed problem needed | Minimum comparison | Owner of result |
|---|---|---|---|---|
| P1 when triggered | PMB recall retrieval | Repeatable wrong/missed context or costly cross-session research | Plain PMB/Git/search versus selective observation recall on fixed questions | PMB evaluation; HE research |
| P1 when triggered | Worktree lifecycle | Concurrent agents actually collide, orphan state or lose changes | Native Git versus Worktrunk; cleanup failures and effort | HE experiment; not PMB state |
| P1 when triggered | Readiness evidence states | Existing review confuses N/A, UNKNOWN and failure | Small corpus of reports with expected statuses | HE research / ACR assessment |
| P2 | UI observe-act-verify | Authorized UI automation reports success without effect | Reversible UI toggle with stale refs and blocked action cases | HE experiment, control tool owns execution |
| P2 | Fused search helpers | Repeated redundant searches/calls in real workloads | Paired native tools vs selective fusion; actual tokens/time/quality | HE economics / tool layer |
| Project-specific | UI critique/accessibility | Specific UI defects, not a desire for more tooling | One selected skill + real WCAG checks vs baseline | Side Quest Atlas / website |

**Stop rule:** If a candidate does not improve the relevant outcome under controlled conditions, do not add its infrastructure. A clean architecture diagram or popularity spike is not a pass.

## Explicit boundaries and non-decisions

- No new control plane, persistent memory daemon, vector DB, spec graph, global worktree manager or desktop-control agent is authorized.
- No automatic PMB memory updates from observation logs; no ACR reviewer-count increase because another repo has multiple reviewers.
- No promotion of self-reported “saved calls,” “10x tokens,” theme screenshots or GitHub stars to HE success claims.
- Keep current HE-002 decision status **PENDING HE-001**.
- Prefer local, reversible, deterministic changes tied to an observed operational failure over speculative generalized governance.

## Recommended documentation action

Retain this synthesis and its pinned source ledger under existing HE research paths. It is research evidence, not an approved architectural RFC or production roadmap. Revisit only when a pilot is triggered and evidence can be recorded with actual inputs, version, result and limitation.
