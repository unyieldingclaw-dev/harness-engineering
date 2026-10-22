# The Next New Thing — Top Repos, Fame, Traffic & Agents — 2026-09-25

## Source

**Video:** Top Repos + Fame, Traffic & Agents  
**Creators:** The Next New Thing  
**URL:** https://www.youtube.com/watch?v=hlOk-EFUITQ  
**Companion page:** https://2026-09-25.githubshow.codeshiftagent.com/  
**Reviewed:** 2026-09-25  
**Evidence:** user-provided transcript plus repository inspection below.

This note is a source/evidence pass, not an adoption plan. Repositories already mined elsewhere in HE are cross-referenced rather than re-documenting the same conclusions.

## Executive Findings

The video's strongest HE value is not the weekly popularity list. The useful implementation evidence is concentrated in six areas:

1. **NewsJack + Jev:** a narrow probabilistic decision engine can replace a wide low-cost LLM pass while preserving the existing artifact contract, with deterministic hard-rule floors and an explicit LLM fallback.
2. **OpenSEO:** skill changes can be evaluated behaviorally using fresh isolated sessions, frozen skill copies, neutral prompts, holdouts, captured traces, and preserved failures. This is one of the strongest concrete skill-evaluation harnesses reviewed so far.
3. **Paperclip:** a multi-agent control plane becomes safer when ownership, dependency, execution, budget, approvals, retries and continuation are durable structured state rather than inferred from agent prose.
4. **Claude Code `AGENTS.md`:** shared instruction-file support improves cross-host portability, but the actual loader semantics prove that a common filename does not imply identical discovery, precedence, nesting or lifecycle behavior.
5. **Claude Code Templates / `jev-auto-mode`:** policy can be monotonic: user/managed policy owns authority; project policy may tighten but not silently loosen it. Deterministic rules run before probabilistic judgment, governing files are self-protected, and failures default toward ask/deny rather than silent allow.
6. **WeKnora / Knowledge Work Plugins:** rich retrieval and role-specialization systems work by composing explicit stages, scopes, connectors and specialized context. These are useful references, not evidence that HE needs a knowledge graph, RAG platform or giant skill catalog.

## 1. NewsJack + Jev — Narrow Decision Engine Inside a Stable Pipeline

**Repository:** https://github.com/elvisun/newsjack  
**Inspected commit:** `092d882fc69912622f620c50eb493afe625f99dc`  
**Jev integration commit:** `a0412d154ca69f277d4e12085c154d92b31bcc3c`

The video presents NewsJack as a PR/news workflow and highlights Jev because the coarse relevance decision is much faster and cheaper than a frontier model. The implementation is substantially more interesting than the demo.

### What the implementation actually does

NewsJack's existing detector had a wide **coarse relevance pass** implemented by low-cost LLM workers. The Jev path does not redesign the rest of the pipeline. It introduces:

```text
candidate signals
      ↓
Jev typed questions, one signal at a time
      ↓
deterministic post-rules
      ↓
existing coarse_relevance_decisions.json contract
      ↓
existing filter/cluster/origin/triage/report pipeline
```

Important mechanisms:

- Jev is **opt-in**, not the only route.
- The LLM worker path remains the default/fallback.
- The same downstream JSON artifact is preserved.
- Six typed questions decompose the rubric into decision, reason and supporting yes/no probabilities.
- Deterministic post-rules prevent typed answers from violating hard recall/safety/profile rules.
- Low-confidence rejects are promoted to `monitor_only` rather than silently dropped.
- API failures preserve recall by producing low-confidence `monitor_only` decisions.
- More than 20% failed Jev calls returns non-zero so orchestration can fall back to LLM workers.
- Raw typed answers, model identity, question-set hash, failures, tokens, estimated cost and timing are retained in the output for later evaluation.

### Evidence discipline

The implementation does not treat Jev output as ground truth. It includes an agreement evaluation against prior worker decisions and separately measures the expensive error class: recall misses.

Its first recorded live eval used 176 signals, 0 API failures, about 8.3 seconds wall clock and about $0.013 estimated cost. Exact decision agreement was only 60.2% and survive-vs-drop agreement 76.7%; the implementation retained the conservative path because disagreements were predominantly recall-safe. These are project-local results, not universal performance claims.

### HE implications

- **REINFORCE:** A specialized model should fit into a stable contract rather than forcing downstream architecture to know which model produced the decision.
- **REINFORCE:** Hard invariants belong in deterministic policy even when semantic classification is probabilistic.
- **REINFORCE:** A cheap decision stage should expose confidence/failure and preserve a fallback path.
- **ASSESS:** Jev or another System-One model as a bounded routing/screening stage in ACR or future harness routing.
- **REJECT:** Treating a low-cost classifier as an authority merely because it is fast, typed or calibrated.

This strengthens the prior Jev research rather than creating a separate Jev architecture.

## 2. OpenSEO — Skill Evaluation Is More Valuable Than the SEO Product for HE

**Repository:** https://github.com/every-app/open-seo  
**Inspected commit:** `0ffff93101043aad7600a3b6a499a0cd2887ef49`  
**Key file:** `.agents/skills/evaluate-skill/SKILL.md`

OpenSEO itself is outside HE's mission. Its internal skill-evaluation harness is directly relevant.

### Evaluation shape

The `evaluate-skill` workflow explicitly says a skill edit is only as good as the work it produces. Per evaluation run it:

1. copies the candidate skill into a fresh temporary directory;
2. records skill hashes so instructions cannot drift mid-run;
3. creates a new isolated project seeded only with a short neutral brief;
4. puts a gateway in front of the local MCP and allows only a fixed tool set plus the correct project id;
5. launches a fresh ephemeral Codex session that ignores user config, memories, apps, plugins and multi-agent state;
6. captures the report, MCP calls/responses, event stream and final message.

Isolation rules are explicit:

- frozen instructions;
- fresh conversation, never resume/fork;
- fresh data/project per run;
- same neutral prompt across variants;
- evaluator material outside the worker's reachable folders;
- raw evaluation artifacts ignored from normal repository state.

### Comparison discipline

OpenSEO requires multiple runs on the tuning target plus a holdout with a different shape. It keeps failures rather than selecting the best run. It scores the result **before** examining the tool trace, then uses the trace to classify misses as discovery/tool/reasoning/reporting failures.

Project-local lessons recorded by OpenSEO include:

- a 3,600-word caution-heavy skill performed worse than a 2,200-word skill with a clearer fixed process;
- forcing a shortlist/comparison before drafting retained important diagnoses better than drafting straight from research;
- explicitly retaining rejected candidates prevented material findings from disappearing during compression;
- unstable external measurements should be re-run rather than treated as one-shot truth.

### HE implications

This is one of the strongest practical examples for evaluating Skills without letting surrounding user memory or project state contaminate the test.

- **REINFORCE:** Evaluate behavior, not whether the skill file looks cleaner.
- **REINFORCE:** Freeze candidate instructions and record their identity during an eval.
- **REINFORCE:** Use fresh sessions and holdout tasks to detect overfitting.
- **REINFORCE:** Preserve failed runs; do not cherry-pick the best attempt.
- **REINFORCE:** Inspect execution traces after output scoring to distinguish discovery/tool/reasoning/reporting failure.
- **ASSESS:** A reusable HE skill-evaluation protocol drawing on these isolation mechanisms.
- **REJECT:** Adding more cautions to a skill as the default response to every failure.

This directly complements Experience-Derived Harness Evolution: history proposes candidate changes; isolated behavioral evaluation decides whether they earned persistence.

## 3. Paperclip — Control Plane, Durable Authority and Structured Lifecycle

**Repository:** https://github.com/paperclipai/paperclip  
**Inspected commit:** `4ca404b49ab3ec5513b9cfa824ff2eeaef941f7a`

The video's "agents as a company" framing is not the important HE takeaway. The source code and current design documents show a serious attempt to separate agent execution from governance.

### Control plane versus runtime

Paperclip explicitly does not prescribe an agent's inner runtime. An agent is adapter-backed. Current adapter examples include Claude Code, Codex, OpenCode, Hermes, HTTP/process and others.

The control plane owns things such as:

- identity and assignment;
- status and execution state;
- tasks/dependencies;
- budgets;
- approvals/reviews;
- pause/resume;
- wake/continuation ownership;
- cost attribution.

The adapter/runtime owns how the agent actually works.

This is a clean example of **governance around heterogeneous agents without pretending the control plane is their memory or reasoning engine**.

### Four states that must not be collapsed

Paperclip's current execution-semantics guide explicitly separates:

1. structure — parent/subtask relationship;
2. dependency — blockers;
3. ownership — responsible actor;
4. execution — whether a live path exists to move the work forward.

This is a useful HE correction to overloaded status fields.

### Durable state outranks narrative

A September 2026 Paperclip lifecycle correction is especially relevant. The previous implementation inferred runnable state from words in summaries/results/comments. The replacement makes continuation decisions from persisted structured state and public tool/API effects.

The design rule is explicit:

> text may describe a decision; it must not select or identify the authority-bearing decision.

Changing narrative while holding structured state fixed should not alter wake, retry, repair budget or continuation identity.

### Pre-dispatch gates

Paperclip distinguishes a known configuration problem from a runtime failure. Missing secret bindings, unresolved workspace base refs, budgets, approvals, pauses, dependencies and ownership are checked before dispatch. A known impossible run should become an explicit gate/wait state rather than consuming an execution attempt and then "failing."

### Tasks as durable coordination

Paperclip routes delegation and coordination through tasks/comments rather than a separate ephemeral agent chat bus. This attaches coordination to the work object and provides a durable audit trail. It also keeps structural parentage separate from execution dependency.

### HE implications

- **REINFORCE:** Control-plane authority and agent execution are separate ownership layers.
- **REINFORCE:** Structured state should own scheduling, retry and authority decisions; prose can explain them but should not silently become the state machine.
- **REINFORCE:** Separate structure, dependency, ownership and execution.
- **REINFORCE:** Known preconditions should be gates, not failed attempts.
- **REINFORCE:** Human governance should retain explicit pause, approval and budget authority for consequential multi-agent systems.
- **ASSESS:** Task/work-object-based coordination and cost attribution if HE ever needs multi-agent orchestration evidence.
- **PARK:** Org-chart metaphors, autonomous "AI company" construction and agent fleets until a demonstrated need exists.

Paperclip is now a stronger HE orchestration/control-plane reference than the video makes clear.

## 4. Claude Code `AGENTS.md` — Portability Improved, Semantic Identity Not Proven

**Repository:** https://github.com/anthropics/claude-code  
**Inspected commit:** `7779afb12e3635f46f56ec823979d68350ae000b`  
**Feature introduction:** `a92ea1cdb11ad21f9d583fad2db181dfdac918a6`  
**Key source:** `mods/agents-md/README.md`

The video claims the long-standing multi-agent instruction-file annoyance largely disappears because Claude Code can now read `AGENTS.md`.

The implementation needs a more precise interpretation.

### Actual modes

Claude Code exposes four project-instruction modes through its built-in `agents-md` mod:

- `claude-md`: Claude files only;
- `claude-md-or-agents-md` **(default)**: AGENTS files are used when the project has no project-owned CLAUDE instruction file;
- `claude-md-and-agents-md`: load both;
- `managed-only`: drop user/project/local files and keep managed instructions/memory.

The default is therefore **fallback compatibility**, not "always merge AGENTS.md with CLAUDE.md."

The implementation also documents multiple ways nested AGENTS behavior still differs from native CLAUDE behavior, including when nested files attach, how they survive compaction, added directories, file mentions/IDE selections, symlink resolution and subagent behavior.

### HE implication

This is strong current evidence for an existing HE principle:

> A portable instruction format does not prove portable executed behavior.

The repository artifact is only one layer. The host owns discovery, precedence, nesting, attachment lifecycle and omission behavior.

- **REINFORCE:** Prefer a shared cross-host artifact when it reduces duplicate ownership.
- **REINFORCE:** Record/verify each client's discovery and precedence semantics before assuming equivalence.
- **ASSESS:** Whether work/personal projects can safely converge on AGENTS.md as canonical source with host-specific adapters or fallbacks.
- **REJECT:** "Rename CLAUDE.md to AGENTS.md and every coding agent will behave identically."

## 5. Anthropic Knowledge Work Plugins and Financial Services — Capability Composition

**Repositories:**

- https://github.com/anthropics/knowledge-work-plugins at `da38ec1ee89d41e5380e652a97382695003396e7`
- https://github.com/anthropics/financial-services at `574ed3624aebd0418c7e96cd101262f30210ab26`

Both repositories reinforce a capability-composition pattern rather than a need for dozens of independent agents.

A knowledge-work plugin composes:

```text
manifest
+ skills (domain knowledge / procedures)
+ commands (explicit entry points)
+ connectors (external capabilities)
```

The financial-services repository adds full workflow agents, but the same underlying prompts/skills can be exposed through interactive Cowork/Claude Code or through a managed-agent wrapper. The user-facing host and orchestration layer can change while the workflow/domain capability source remains shared.

Important boundary from the financial-services source: consequential outputs are staged for qualified human review rather than making recommendations, executing transactions or binding risk automatically.

### HE implications

- **REINFORCE:** Reuse one owned capability definition across multiple host adapters where possible.
- **REINFORCE:** Skills own knowledge/procedure; connectors own external capability; commands own explicit invocation; an agent is justified when end-to-end state/objective/authority needs its own actor.
- **REINFORCE:** Domain specialization is strongest when customized to authoritative company tools, terminology and processes rather than copied as a generic role-play prompt.
- **ASSESS:** Cross-host wrapper patterns for the same core Skill/capability source.
- **REJECT:** "More domain plugins" as evidence that HE needs a universal skill pack.

## 6. WeKnora — Rich Retrieval Pipeline, Not a Default Memory Architecture

**Repository:** https://github.com/Tencent/WeKnora  
**Inspected commit:** `4364e61afa4bf1e086c34b4220bb21449aa15b25`

The video loosely describes WeKnora as files + RAG + graph + automatic relationships. The current implementation is broader: enterprise document ingestion, RAG, agents, wiki/graph, long-term memory, connectors, MCP, tracing and sandboxed Skills.

### Retrieval pipeline

WeKnora's current RAG path is explicitly staged:

```text
history
→ memory recall
→ query understanding / rewrite / intent
→ parallel chunk + entity retrieval
→ rerank / weighting
→ optional web fetch
→ merge
→ top-k filtering
→ optional data analysis
→ context assembly
→ generation
```

The pipeline is dynamically assembled based on the request rather than blindly executing every stage. It tracks intermediate retrieval state separately from immutable request configuration and runtime context.

Current releases also consolidate several knowledge tools into a smaller `search_knowledge` / `read_document` / `list_documents` surface and allow explicit knowledge-base/document/tag scopes.

### HE implications

- **REINFORCE:** Retrieval is a pipeline of separable responsibilities, not one magical "RAG" step.
- **REINFORCE:** Dynamic capability exposure can avoid stages that are irrelevant to the current request.
- **ASSESS:** Query rewrite, hybrid retrieval, rerank and retrieval observability only when PMB/MB evidence shows a corresponding retrieval failure.
- **PARK:** Knowledge graph, enterprise RAG infrastructure and automatic long-term-memory extraction as HE architecture.
- **REJECT:** Assuming automatic file relationship discovery is inherently better than source-owned project context and explicit retrieval cues.

This complements, rather than overturns, the recent Graphify finding: derived structure must earn its lifecycle cost on the task classes that need it.

## 7. Claude Code Templates — Marketplace Signal and a Strong Policy Experiment

**Repository:** https://github.com/davila7/claude-code-templates  
**Inspected head:** `f3a78304473e11583496a998479502f68ded8b50`  
**Key policy commit:** `3704ff7c6aef0bfacee0cf70d8bb6566be13fd3c`

The video focuses on the project as a friendlier marketplace for skills, agents, hooks, commands and MCPs. That is useful for discovery, but popularity/download count is not a security or quality proof. A marketplace is a **supply-chain and curation surface**, not an authority.

The deeper HE finding in the current repo is `jev-auto-mode`.

### `jev-auto-mode`

The mod defines `allow / ask / deny` for tool calls, skills, commands and subagents.

Decision order:

1. self-protection of governing policy/mod files;
2. deterministic rules, with **deny > ask > allow** and compound-shell parsing;
3. a configured default;
4. optional Jev judgment for uncovered actions.

Notable trust model:

- the user's policy may define the full policy;
- a project repository's policy may **tighten only** by default;
- a cloned repository cannot silently use its config to grant itself more authority;
- policy files and the mod are protected from model edits;
- internal errors deny rather than silently skip the control;
- judge failure defaults toward `ask` unless the user deliberately chooses otherwise;
- audit mode exists before enforcement.

Jev evaluates semantic hazards such as destructive action, exfiltration, security weakening and out-of-scope behavior. Deterministic user rules remain primary.

### HE implications

This is strong evidence for a general security/configuration concept:

> **Less-trusted configuration should be monotonic: it may restrict authority, but should not silently expand it.**

Also:

- **REINFORCE:** Governing policy must be outside the governed agent's mutation authority.
- **REINFORCE:** Deterministic rules should decide known safety invariants before model judgment.
- **REINFORCE:** Failures in the permission layer should be visible and conservative rather than silently fail-open.
- **ASSESS:** Probabilistic semantic judgment as an `ask/deny` advisory layer for ambiguous actions after deterministic policy.
- **PARK:** Jev as a general permission authority until locally calibrated and threat-modeled.
- **REJECT:** Stars/download counts as proof that a marketplace component is safe to install.

This source also connects directly to the existing Skillspector research thread.

## 8. Octop — Local Multi-Agent Composition, Mostly Reinforcement

**Repository:** https://github.com/TencentCloud/Octop  
**Inspected commit:** `c04aacc604a23ac05c00538f1bd4bc586fee6bb6`

Octop is a self-hosted multi-user/multi-agent assistant built from smaller Harness components for agent runtime, gateway, memory and browser automation. It supports inbound/outbound ACP and can delegate to Claude Code, Codex, OpenCode and other runners.

Useful implemented patterns:

- local/self-hosted control plane;
- per-agent workspaces/providers/channels;
- explicit tool approval and shell guardrails;
- agent runtime separated from gateway/memory/browser components;
- one normalized message processor across web, IM and cron;
- capability delegation to external coding agents through an explicit protocol.

The most provocative items in the README — AgentTeams and automatically distilling conversations into reusable Skills — are roadmap items, not implemented evidence.

### HE disposition

- **REINFORCE:** Separate focused runtime components and explicit adapter/protocol boundaries.
- **ASSESS:** ACP-style bidirectional agent interoperability if cross-host delegation becomes an actual requirement.
- **PARK:** AgentTeams and conversation-to-Skill self-evolution until implemented and evaluated.
- **REJECT:** Treating roadmap text as evidence of a proven self-improving harness.

Orca remains the stronger already-mined source for current multi-agent session/orchestration semantics.

## 9. Repositories Already Mined Elsewhere

### Alibaba Open Code Review — REINFORCE EXISTING

Already covered in:

- `01 Research/Sources/Alibaba Open Code Review & AACR-Bench — ACR Follow-up — 2026-09-21.md`

The video adds popularity/interface commentary but no stronger HE mechanism than the existing source-code/AACR-Bench pass.

### Orca — REINFORCE EXISTING

Already covered in:

- `01 Research/Sources/Orca — Orchestration, Handoff & Verified Computer Use — 2026-09-21.md`

The video reinforces agent-centered UI, worktree isolation, multi-host sessions and usage visibility; it does not replace the deeper runtime/source findings already recorded.

### Addy Osmani Agent Skills — REINFORCE EXISTING

Already present in prior dashboard/cross-host research. Current repository work continues to validate host-specific adapters and cross-platform skill validation. No new HE architecture follows from the video comparison to ECC.

### ECC — REINFORCE / SECURITY FOLLOW-UP

ECC has already been mined for Skills/context patterns. The current repository also has active deterministic security-hook hardening. The video's "remember and improve" framing should not be translated into automatic AGENTS/CLAUDE mutation: correction still belongs in the actual owning component and must be evaluated.

## 10. OpenSEO Product, News/PR, and Other Business Ideas — PARK OUTSIDE HE

OpenSEO as an SEO SaaS alternative and NewsJack's PR business workflow are useful products but are not HE architecture requirements. Their **internal harness mechanisms** were mined above.

The video's final custom call-recorder story reinforces a general economic lesson already present in HE: a custom tool can be worth building even when it costs more if it materially improves fit, control or workflow quality. Cost alone is not the acceptance metric.

## Cross-Source Synthesis

### Portable artifact versus portable behavior

`AGENTS.md`, shared Skills and plugin bundles all improve artifact reuse. They do not prove that different hosts discover, prioritize, inject, omit or execute them identically.

### Model judgment versus deterministic policy

NewsJack and `jev-auto-mode` illustrate two good compositions:

```text
semantic judgment
      ↓
deterministic floors / policy
      ↓
stable contract / controlled action
```

The model handles ambiguity; code owns invariants and authority.

### Discovery versus proof

Marketplaces, skill catalogs and weekly trending repositories are discovery mechanisms. They are not evidence of safety, correctness or local value. Every adopted component still needs source inspection, provenance/license review and local behavioral evaluation.

### Multi-agent control versus more agents

Paperclip and Orca both show that multi-agent usefulness comes from ownership, isolation, status, handoff, budgets, gates and evidence — not from agent count or an org-chart metaphor.

### Harness evolution versus scar tissue

OpenSEO's skill-eval harness and the earlier Experience-Derived Harness Evolution work point to the same loop:

```text
observed need
→ candidate instruction/tool/policy change
→ isolated fresh-run evaluation
→ holdout / regression evidence
→ keep or remove
```

## Disposition Summary

| Source / mechanism | Disposition | Why |
|---|---|---|
| NewsJack Jev coarse filter | REINFORCE / ASSESS | Strong typed-decision + deterministic-floor + fallback pattern |
| OpenSEO skill evaluator | REINFORCE / ASSESS | Excellent fresh-session/holdout skill evaluation mechanics |
| Paperclip control plane | REINFORCE | Structured authority, governance, execution and lifecycle separation |
| Claude Code `AGENTS.md` | REINFORCE / ASSESS | Cross-host artifact portability with explicit semantic caveats |
| Knowledge Work Plugins | REINFORCE | Clear composition of skills, commands and connectors |
| Financial Services | REINFORCE | Same capability source across interactive and managed-agent surfaces; human sign-off |
| WeKnora retrieval pipeline | ASSESS | Rich reference for staged retrieval; infrastructure not currently justified |
| Claude Code Templates marketplace | PARK as marketplace | Useful discovery surface; not a trust anchor |
| `jev-auto-mode` | ASSESS | Strong monotonic-policy experiment; probabilistic authority needs calibration/threat review |
| Octop | PARK / WATCH | Useful composition patterns; novel self-evolution claims are roadmap |
| Alibaba Open Code Review | REINFORCE EXISTING | Already mined deeply with AACR-Bench |
| Orca | REINFORCE EXISTING | Already mined more deeply than this video |
| Addy Agent Skills / ECC | REINFORCE EXISTING | Existing Skills research; no need to duplicate |

## Research Questions Carried Forward

1. Can HE define a reusable **Skill behavioral-evaluation protocol** using frozen instructions, fresh sessions, holdouts and trace-based failure classification?
2. Should HE promote **monotonic authority configuration** — less-trusted layers may tighten, not loosen — as a candidate governance principle?
3. Where can cheap typed semantic judgment reduce expensive model fan-out while hard deterministic rules preserve recall/safety?
4. Can cross-host instructions converge on one source artifact while host-specific discovery/precedence is made explicit and testable?
5. Which multi-agent facts must be durable structured state before parallel/orchestrated work is safe to resume after failure or handoff?

## Guardrail

Do not turn a weekly trending list into architecture. The useful unit is the verified mechanism, its ownership boundary and its measured behavior.