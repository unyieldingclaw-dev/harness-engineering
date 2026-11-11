# Grant Owen / Angus — Five AI-Use Levels, Skills, Second Brain and Watchers — 2026-11-11

## Evidence and scope

- User-supplied full transcript: **The Future of AI (Do this Before 2027)**, Grant Owen interviewing Angus, https://www.youtube.com/watch?v=zNf4dTjI-NM (reviewed 2026-11-11).
- The speaker describing himself as the **“non-technical tech bro”** is guest Angus. This is an interview and practical experience account, **not** an independent test or a standard.
- Claims below are attributed to the interview. Recommendations for HE/PMB/ACR are analysis, not claims made by the speakers.
- No interviews, applications, subscriptions, CLI tools, databases or skills were installed/tested for this pass. References to work policies or privacy are design considerations, not an audit of this user's environment.

**Bottom line: REINFORCE with limits.** The interview's most useful distinction is **repeatable procedure vs. autonomous decision authority**. It does **not** justify rebuilding PMB in PostgreSQL, adding n8n, adopting a universal skill generator, or moving toward unattended Level 5 as a goal.

## The five levels: useful teaching model, not maturity certification

| Level as stated | Claimed progression | What survives scrutiny | HE/PMB boundary |
| --- | --- | --- | --- |
| 1. AI as a conversational expert, connected to real data (00:35–04:42) | Custom instructions and connectors make a basic assistant useful. | Source-grounded, permissioned retrieval beats an “expert” persona. | Distinguish retrieved source facts, model inference and actual system authority. A chatbot sounding authoritative is not proof. |
| 2. Codify repeated work into Skills (04:42–13:45) | Reuse a real manual SOP instead of inventing a fictional specialist agent. | **Strong.** Observe an actual workflow and outputs first, codify the stable portion, test on representative cases. | The “done it twice ⇒ Skill” rule is only a *candidate trigger*, not an adoption threshold. A script, template, saved query or no new artifact may be better. Skills require maintenance and can grow stale. |
| 3. Company/personal second brain (13:45–33:39) | Hierarchical summaries, Postgres, connectors, progressive drill-down. | **Strong underlying idea:** discover → summarize → selectively inspect source detail; keep retrieval pointers and provenance. | The “one database” is an implementation option, not a new single authority. Keep operational facts with source systems; distinguish stable project knowledge from live data. Five-file PMB need not become a DB. |
| 4. “Watcher” (34:42–49:21) | Scheduled agents observe data and propose insights; skills accumulate corrections. | Read-only scheduled observation can be useful when a real recurring decision and reliable data exist. | Prefer deterministic collectors for exact stats; source freshness and unknown/missing status matter. Route possible corrections as **candidates**, not automatic changes. Does not create an obligation to build always-on infrastructure. |
| 5. No human in the loop (49:21 onward) | Agent identifies and fixes issues autonomously, then reports them. | A limited capability appropriate to known low-risk, reversible tasks when an external boundary enforces scope. | **REJECT universal autonomy ladder.** Human sign-off remains required for high-consequence external effects and ambiguous decisions. A working read-only watcher is not proof a write-capable agent is safe. |

## Claims to resist

### “Ask questions until 95% certain” (04:14)
Useful as a request for sufficient requirements, **not** calibrated confidence. No mechanism verifies a 95% probability. Ask a question only when the answer changes scope, outcome, authorization, or acceptance; otherwise state assumptions.

### “If you did it twice, create a Skill” (07:01)
Frequency alone is insufficient. A candidate Skill should have:
- a repeatable, stable process;
- enough recurrence/value to cover authoring, execution and upkeep;
- a meaningful acceptance condition and counterexample;
- an owning component that is actually a Skill rather than code, tool, hook, check, template or project document.

A successful real workflow is a better starting point than a generic downloadable persona, but **copying its historical artifacts still requires provenance, privacy screening and sample diversity**. One happy path does not establish generality.

### “More context makes AI dumber” (14:36)
Overly absolute. Irrelevant and stale material can distract and cost tokens; *missing* critical context can also degrade answers. The transcript's ~40%-window intuition is not a reliable threshold. Test relevance, precision, cost, source freshness and actual outcomes. Selective retrieval is already central to HE Context Engineering.

### “Put everything into Postgres and let AI generate SQL” (22:15 onward)
Postgres, hosted DB and n8n can be reasonable for event/metric sync **when multiple real data producers and recurring consumers exist**. They are not default prerequisites to a personal memory system. For live status, the authoritative record stays in the source system; synchronized summaries are derived caches with refresh times, errors, deletion/retention rules, and source links. Generated SQL cannot guarantee correct joins, units, access control or temporal consistency without definitions and permission constraints.

### “An agent is a model with tools and persistent memory” (05:45)
This is Angus's simplified teaching definition, not a universal technical requirement. Many useful agents are stateless across executions, with only task-scoped context and tools. Persistent memory should be optional, scoped and explicitly governed.

### “Skills beat agent teams” (33:39)
A repeatable procedure often matters more than a team of named personas, especially for human-facing copy/creative work. This does **not** prove that multiple independent, bounded analysis workers are always harmful. HE already distinguishes independent review scopes from agent committee roleplay. Creative judgment, specialized expertise and external approval remain human-owned where consequential.

### “Self-improving Skills” (48:14)
Logging a correction to a skill's issues table is an **observation**, not verified diagnosis. An AI may propose a fix; it should be promoted only after reproduction/clear evidence, owner mapping, representative tests and human approval. Do not make every user correction a persistent rule or let the Skill edit itself after every task.

### “Local models get you 80% of the way and are faster” (50:49)
An unsourced generalization. Capability varies by model, quantization, context, inference runtime, latency, orchestration and workload. Local may reduce direct data exposure but does not remove supply-chain, access-control, or sandbox risk. Compare on the actual task; do not size infrastructure from a talk-show estimate.

### “Nine out of ten nontechnical builds go wrong” (52:42)
Personal anecdote, not a measured failure rate. The underlying caution is credible: a rapid demo can conceal deployment, identity, data, failure recovery, cost and governance work. Do not quote 90% as an empirical statistic.

## What genuinely transfers to these projects

**HE**
- Already documented: evidence before architecture, progressive disclosure, skill routing/evaluation, event/trace evidence vs memory, deterministic enforcement, bounded autonomy.
- Small reinforcement: a **Skill-candidate decision** should be based on an observed repeated workflow and a cheaper alternative. This is optional advice at an actual workflow-design decision, **not** a global rule.
- New light framing: “Watcher” is *observation*; actionable change requires a separate, authorized gate. No one-way Level 4→5 promotion.
- The host/interface should not hide the provenance of a recommendation; summaries and source links must permit inspection of nuance that was compressed away.

**PMB**
- PMB v1.2.x already has a five-file project memory hierarchy, CLI diagnostics, security review, hooks and an evaluation/pilot story. Completion of its current release takes priority.
- No Postgres, n8n, graph database, generalized second brain or self-writing skills justified.
- Existing PMB Dream research already covers candidate memory promotion and review; this source is not a missing requirement.

**ACR**
- Existing local agent review, recorded findings and benchmark calibration are more relevant than “AI teams bad” generalization.
- Record actual missed findings, false positives, timeouts, and owner; evaluate a smaller reviewer/config change if it improves measured results. No agent-count change inferred.

**Home/Work Multi-Provider Dashboard**
- A read-only watcher-style **view** of source-owned health/usage information fits the *separate* dashboard question.
- This video does not justify a new scheduler, AI-operated fix loop, cross-work/home data replication, or substituting AI summaries for deterministic provider/CLI facts. Work permission and source-interface feasibility remain open.

## Existing HE research / non-duplication

- `01 Research/Context Engineering.md.md` — task-relevant retrieval, provenance and context economics.
- `01 Research/Skill Routing, Gate-Bound Evidence & Orthogonal Review — 2026-09-26.md` — Skill-vs-owner classification and behavioral validation.
- `01 Research/Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation — 2026-09-25.md` — policy outside model judgment and representative Skill checks.
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md` — real traces to candidate corrections, not automatic mutation.
- `01 Research/Sources/PMB Dream — Retrospective Memory Consolidation — 2026-09-01.md` — memory candidates vs approved facts.
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md` — read-only status and source-owned operational facts.

**Disposition: evidence-led reuse, no implementation.** These levels are explanatory; progress is not measured by autonomy, agents, token counts, or the number of connected applications.
