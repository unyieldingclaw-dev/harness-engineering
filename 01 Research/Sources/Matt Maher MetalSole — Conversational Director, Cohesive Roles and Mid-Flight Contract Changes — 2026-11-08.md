# Matt Maher (MetalSole) — Conversational Director, Cohesive Roles and Mid-Flight Contract Changes — 2026-11-08

**Classification:** Source-specific HE research. **No adoption, implementation, config, code, role, security, model routing, CI, PMB, or ACR changes authorized.**  
**Video:** [*How the Pros Actually Work With AI Now*](https://www.youtube.com/watch?v=KbuSHtTeyEA), MetalSole / Matt Maher, **October 23, 2026**, ~27:03.  
**Evidence:** user-supplied video description, the speaker's complete “director” prompt, timestamps, and 35+ video screenshots from several messages through November 8; cross-checked chapter wording and transcript on [Modern Creator's Oct 23 read-along](https://moderncreator.app/2026-10-23-matt-maher-how-the-pros-actually-work-with-ai-now). **Video demonstration, not independently inspected code or reproducible benchmark.** Screenshot UI may be illustrative and contains “Bypass permissions” in the client. No poster-maker repository link, code audit, network trace, cost report, complete baseline run or automated verifier results supplied.

**Host capability verification (docs read Nov 8):**
- Official [Claude Code custom subagents](https://code.claude.com/docs/en/sub-agents): independent context/tool scope, background execution; nested *subagents* default up to 3 levels below main; current v2.1.217+ default concurrency cap 20, may be reconfigured; model and effort configurable, subagents share plan quota. These are **host-specific**, not universal agent primitives.
- Official [Claude Code agent teams](https://code.claude.com/docs/en/agent-teams): experimental team lead + teammates, direct teammate communication, **no nested teams**, increased tokens/coordination overhead, no session resumption for in-process teammates, task statuses can lag, and shared files can conflict. Distinguish teams from nested subagents.
- Official [Claude Code scheduled tasks](https://code.claude.com/docs/en/scheduled-tasks): `/loop` polling is local/session-scoped and not a durable autonomous scheduler; event delivery and cloud/desktop schedules have different lifecycle and authority.
- Official [Claude Code settings hierarchy](https://code.claude.com/docs/en/settings): instruction files are not permission guards; effective policy includes local/project/managed settings and varies by host.

## Decision

**Strong alignment with desired HE interaction:** one persistent *human-facing* coordinator that can explain, assign, and reconcile independent work, while the human owns objectives and acceptance. The architectural insight is **source-owned cohesive capabilities + observable execution receipts**, not a requirement for a permanent AI director, many model roles, or recursive multi-agent hierarchy.

**Most material new source-specific HE experiment candidate:** change requests arriving during parallel execution must have *task-version and invalidation semantics*. A still-running task is not necessarily honoring the latest requirements, and a “done” flag is not proof against the current request. Existing HE lifecycle/ownership and bounded-execution research owns the implementation semantics; do not build a new control plane from a video.

**Second useful insight (spoken by creator at ~9:47):** the video's original Words / Numbers / Colors split is potentially **the wrong decomposition**. Those choices share the same poster concept; role boundaries should follow **coherent decision/data authority**, not job titles, organizational analogies, or maximum possible parallelism. A single subject-definition unit could jointly choose them. This undercuts naive “more specialized workers is always better” interpretations.

**Important evidence gap:** creator suggests adding a **historical image evaluator near 24:41** after noticing the 1886 Statue of Liberty artwork appears anachronistic. That evaluator is **proposed**, not demonstrated as working. Final attractive images do not establish historical fidelity, correctness, accessibility, runtime robustness, rework cost, or economical scaling.

## What the actual video demonstrates

| Time | Direct evidence | Mechanism and limitation |
| --- | --- | --- |
| 00:00–02:08 | Draws a network with director; Engineering/Design/Research/Quality/Operations/Writing nested roles and possible linked systems. | **Vision illustration**, not implemented proof of scalable company-size agent factories. |
| 02:08–06:45 | Starts in empty `poster_maker` folder; natural-language instructions define director as contact, delegated worker creator, model/effort selector and status reporter. | Proves simple user interaction / generated instruction files in one demo, not deterministic orchestration or portable host behavior. Creator insists director never works; **HE should allow cheap direct tasks** to avoid delegation tax. |
| 07:12–11:56 | Defines Artist, Words, Numbers, Colors plus later Concept. Creator says “role” is really a **controllable context/domain**, and admits unrelated independently selected words/numbers/colors can conflict. | **Strong ownership insight.** Keep interdependent semantic decisions together; parallelize only independent or safely reconcilable work. |
| 12:02–15:12 | Generated `role-*.md`, `AGENTS.md`, `STYLE.md`; subject `01-concept.md` followed by parallel research. Director reports concise worker status and source discrepancies (official Eiffel Tower stairs count 1,665 vs another source 1,792; “no source means no pick”). | Good example of role-local instructions, source confidence and inspectable artifacts. But source conflicts and final provenance need independent checks; statuses in markdown are self-reports unless reconciled with process/file state. |
| 15:12–17:08 | Adds art-direction guidance, local gallery/Engineer role while Paris pipeline runs; directory/file responsibilities expand. | Evolving requirements **can be expressed to director**, but no evidence these exact changes were safe to apply without invalidating existing attempts. |
| 17:08–19:00 | Asks for **three varied posters per subject and stacked gallery thumbnails** while Art Direction and Engineer remain running, annotated “still running/not restarted.” | **Key case**: task contract changes while work is in flight. Requires explicit acknowledgment, supersession, artifact version and evaluation against latest contract. Process liveness ≠ adoption of new requirement. |
| 19:00–21:43 | Queues five more subjects; launches many parallel models. Adjusts role instructions so workers do not recursively start as Directors; image shows ~12 finished and numerous active tasks. | New instructions may not retroactively govern launched workers; model/rate/slot usage and merge contention not measured. Prevent role inheritance loops and uncontrolled fan-out with native enforceable limits when used. |
| 21:43–23:21 | Repeated failure becomes new rule/role; argues conversational memory alone won't durably preserve role expectations. | **Partly aligned with PMB.** Evidence of a repeat or one-off correction is a *candidate*, never sufficient authorization to rewrite durable `AGENTS.md` or global memory. Check owning component, reproduce, review scope, accept, fresh-run verify. |
| 23:21–24:14 | “Audio department” has its own director which delegates to nested specialists. | Conceptually **possible using nested subagents**, but native **agent teams explicitly forbid nested teammates**; shared hard limits, lifecycle and cross-provider capability differ. Only split once parent context/coordination is a measured problem. |
| 24:14–26:20 | Gallery of posters, notices likely 1886-historical mistake and says he'd *add an evaluator*. Mentions possible Slide/Email/Research roles. | Attractive partial deliverable, **not an end-to-end verified pipeline**. Factual images/claims and outward email require provenance and explicit authorization. |
| 26:20–end | Recommends asking an agent to install the director from the video prompt; claims same concept applies across clients. | General semantics portable, **behavior and permissions aren't**. A new external repo/provided `AGENTS.md` must be reviewed before treated as authority. |

## Distinguish coordinator responsibilities from host/runtime facts

A coherent small-run design (not a required new HE subsystem):

```
Human: define objective, constraints, budget, acceptance, approve consequences
   ↓
Coordinator: analyze / propose / route / reconcile / report
   ├─ direct deterministic action when sufficient
   ├─ one bounded researcher/worker when value is clear
   └─ optional independent verifier for outcome/consequence
                 ↓
Project sources + execution receipts + verified artifacts
                 ↓
Human accepts, modifies or rejects
```

Coordinator can decide **which work to propose and where to route it**, not autonomously grant higher privileges, invent ownership, or treat status prose as evidence. The host/runtime or process manager establishes whether a job is actually running; the repository establishes which version/files exist; the verifier establishes outcome evidence. The user establishes changed intent and final acceptance.

**Essential contract questions for a real delegated task**:
- owner, task id/attempt, inputs/accepted requirements revision, allowed output paths, allowed tools, model/effort policy, budget/deadline;
- dependencies, current statuses *grounded by host*, exact output contract and how to verify;
- permitted actions and what must be referred back to user;
- invalidation/rollback condition; settled attempt, evidence and final owner.

The full field set is a diagnostic checklist, **not a new mandatory manifest**; embed into existing issue/task/checklist/handoff formats and adopt only necessary fields when an experiment proves they matter.

## Mid-flight changes and “still running” counterexample

What should happen when creator changes from one poster per city to three styles + gallery grouping:
1. Treat user change as **new requirement revision** for affected outputs, not a silent chat message.
2. Determine which active tasks consume affected requirements: Artist, Gallery, Art Direction, possibly Concept; unrelated research may continue if its contract is unchanged.
3. Send revision/acknowledgment to tasks that can adapt; if a task cannot reliably adapt, stop/supersede it and launch a new bounded attempt. The **previous attempt isn't authoritative** for the new acceptance criteria.
4. Verify actual resulting files and gallery behavior against **the revised requirements** (three variants per city, distinct styles, coherent shared provenance, stacked navigation), not only inspect process state.
5. Present exceptions and decisions to the human. A worker that's alive yet stale should be reported **stale**, not reassuringly “still running.”

This mostly **reinforces** `Supervised Agent Orchestration & Effect Verification`'s one-authoritative-attempt and source-of-truth boundaries, but the *changing criteria during execution* case is a useful concrete evaluation fixture. **Potential isolated experiment**, not sufficient proof that an orchestrator is required.

## Role integrity and "self learning"

The root `AGENTS.md` and generated `role-*.md` show a practical ownership problem:
- A **role file** is not a permissions container; by default Claude loads project instruction hierarchy into subagents, with configurable exclusions. Keep instruction, authorization and host policy separate.
- When an agent writes a new role, it may invent rule ownership and default model choices (e.g. screenshot says Concept should not read `STYLE.md` without asking). Treat proposed modifications as **reviewable code/config changes**, not model self-improvement.
- Prefer known versioned source docs and scoped project rules. For cross-host reuse, separate stable **semantic role description** from host-specific invocation details; don't assume markdown syntax executes the same everywhere.
- Avoid over-fragmentation: Colors and Words depended on same selected concept, so first consolidate subject decisions. Split *only* after evidence of a recurring separable failure.
- Use deterministic tests/hooks for trivial assertions (git status, file presence, formatting, snapshot) rather than an extra general LLM “role.”
- Given prior Forever Security instruction-origin attack research, treat downloaded, newly cloned or generated agent instruction files as **potential untrusted input** until reviewed. This observation is security-relevant but does **not** prove this particular video is malicious.

## Timer and status behavior

Screenshot: “Director heartbeat: read `.director/status/*.md` and post the status board,” `Loop 2-59/5****`, next in ~1m, ends in 3d. Repeated LLM turns to parse unchanged self-reported markdown can add quota costs and confusion without increasing certainty.

**Prefer native task-completion and exception notifications** if available; on-demand status readback when user asks; bounded polling only for external state with no events and a timeout. No blanket recurring background loops for personal projects.

The screenshot footer shows **“Bypass permissions”**. This may simplify a demonstration but is **not** evidence of a safe default. Do not duplicate it for HE or on corporate projects. Permissions, workspace isolation and allowed side effects must be enforced by the host, not merely by a director's prompt.

## Comparison with existing HE research

| Existing owner | Alignment or incremental delta |
| --- | --- |
| `Supervised Agent Orchestration & Effect Verification — 2026-09-21` | Strong existing owner for coordinator/worker authority, source truth, attempt settlement; useful new case: changed user request while work running. |
| `Role-Based Model Allocation & Software Factory Boundaries — 2026-09-24` | Model/effort by **evaluated capability**, not “Opus always for Concept,” or AI's unrestricted guess. |
| `Bounded Execution Envelopes — 2026-09-22` | Actual concurrency, spend, tool and run-duration limits external to model; status heartbeat is not a budget guard. |
| `Experience-Derived Harness Evolution — 2026-09-25` | Memory/role updates require diagnosis + owner + evidence + human promotion, not “I repeated myself, create new agent.” |
| `Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22` | Stage acceptance and independent output checks; the demo's proposed historical evaluator shows a *known missing verification* step. |
| `Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24` | Verified task-cost/performance matter; generic task labels are weak proxies for model requirements. |
| `Harness Portability, Exit Cost & Inspectability — 2026-09-24` | Stable roles and artifacts can be portable; nested agent, status and permission semantics aren't. |
| `Jon Noronha and Gamma — Creative Agent Harnesses, Repair Economics and Useful Friction — 2026-11-08` (same draft PR) | Complementary **real creative production experience**: verify rendered output + human repair cost. MetalSole is a compelling *demo*, no independent quality or rework measurement. |

**Avoid duplicated ownership:** no new `Director` process, hierarchical agent departments, always-on heartbeat, mandatory markdown roles, permanent “self-learning” rules, agent-created auto-installs or broad change-authority.

## Proposed smallest decision-producing experiment (not scheduled or executed)

Use an **existing, bounded, non-sensitive ACR or PMB/HE task fixture** (rather than invent an entire poster factory) with known acceptance criteria.

**Independent variables:**
- **A — direct baseline**: one Claude Code session receives objective and verifies its result.
- **B — minimal director**: same session/worker model(s) with coordinator delegating *one genuinely bounded research/review worker*, using existing native background subagent support.
- **C — only if B demonstrates benefit**: 2–3 workers where inputs/output paths are independent. No nested directors.

**Cases:**
1. a **cohesive task** where independent specialists might introduce contradictory assumptions;
2. a **parallelizable task** with distinct read-only research sources or independent checks;
3. a **mid-flight user correction** that invalidates one worker's requirements but not the others;
4. a **verification failure** that requires exactly one scoped repair and fresh check.

**Freeze for fairness:** identical task corpus, relevant code revision, accepted constraints, model and effort where practicable, tool privileges, approval semantics, budgets, test/checker, human review rubric. Change only coordination style. Workers must report inputs/revision and artifact/evidence location.

**Measure:** verified correctness and accepted requirement revision; contradictions, stale/duplicated edits, provenance quality, false assurances, time to accepted output, token quota/cost, human interruptions/repair time, unhandled timeouts and regressions. Not maximum subagent count or attractive UI.

**Decision rule (provisional):** keep delegation only if it measurably improves completion quality or time-to-accepted-result on a repeated class of tasks without unacceptable cost, security or coordination burden. A null/negative result is success for research. Adjust only existing owning component; don't create an HE orchestrator until observed tasks require one.

**Disposition:** DOCUMENT and prepare **one conditional A/B test**, not an architecture commitment. No tooling installed; no benchmark or execution conducted; no modifications to source ownership/security policy.
