# Nick Saraev — Claude Code Leash, Diagnosis, Parallelism & Fan-Out/Fan-In — 2026-09-25

## Purpose

Mine durable Harness Engineering lessons from Nick Saraev's video **“I Spent $31,141 On Claude Code To Learn This”** while separating useful operating patterns from product-specific claims and overgeneralizations.

Primary user source:

- Video: https://www.youtube.com/watch?v=45K3zHckCnQ
- Creator: Nick Saraev
- Transcript supplied by the user on 2026-09-25

First-party corroboration checked:

- Anthropic, **Building Effective Agents**
- Anthropic, **How we built our multi-agent research system**
- Anthropic, **Building a C compiler with a team of parallel Claudes**
- Anthropic, **Harness design for long-running application development**
- Anthropic, **Scaling Managed Agents: Decoupling the brain from the hands**
- Claude Code MCP documentation, including tool search/deferral
- Anthropic/Claude material on Skills, subagents, parallel sessions and worktrees

Related HE research:

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- `00 Overview/Harness Engineering Philosophy.md.md`

This note is research evidence. It does **not** authorize loosening permissions, replacing MCP integrations with skills, adding multi-agent orchestration, or changing PMB/ACR.

---

# Executive finding

Five ideas are materially useful to HE:

1. **Loosen procedural micromanagement as models improve, while preserving authority and safety boundaries.**
2. **Separate diagnosis from mutation when the problem is ambiguous.**
3. **Use the narrowest capability surface that still owns the required external action; MCP and Skills are not interchangeable.**
4. **Parallelize only work that is genuinely independent or isolated.**
5. **Fan out for breadth/diversity, then fan in through an explicit synthesis/adjudication authority.**

The most important new synthesis is:

> **Loosen the execution path, not the acceptance boundary. Explore widely, adjudicate narrowly.**

---

# 1. “Loosen the leash” is valuable if interpreted correctly

Saraev's recommendation is to stop prescribing every implementation step and instead give capable models a strong definition of done, important constraints, and permission to choose the method.

The useful abstraction is:

```text
objective
+ definition of done
+ invariants / forbidden outcomes
+ authority / permission boundary
+ verification path
        ↓
model chooses execution path
```

This is stronger than a long procedural script when the model already knows how to perform the task.

It also matches first-party Anthropic evidence that harness assumptions can go stale as models improve. Anthropic's recent harness work explicitly recommends stress-testing scaffolding that encodes assumptions about what a model cannot do, and removing components when they no longer improve outcomes.

### Critical distinction

“Loosen the leash” must **not** mean:

- broader filesystem or production permissions by default;
- fewer tests;
- weaker acceptance criteria;
- silent scope expansion;
- autonomous authority over irreversible actions;
- removal of evidence/verification gates.

The safer formulation is:

> **Reduce procedural prescription before reducing authority constraints.**

A more capable model may need fewer instructions about *how* to work while still needing the same or stronger limits on *what it may change* and *what evidence proves completion*.

### HE disposition

**STRONGLY REINFORCE Model Capability Drift and Assessment Before Architecture.**

Candidate principle:

> **Constrain outcomes and authority; prescribe the execution path only where evidence shows the model needs it.**

---

# 2. Diagnose before fix should become an explicit mutation gate when ambiguity is high

Saraev recommends asking the model to enumerate problems before letting it change anything. The user can then reject diagnoses that are not actually part of the desired fix.

This is useful because `fix it` collapses several decisions:

```text
symptom interpretation
root-cause hypothesis
scope selection
solution choice
mutation
```

Once the model mutates the system, it becomes harder to distinguish the original defect from new changes and unnecessary “improvements.”

### Stronger HE pattern

```text
symptom / failure
      ↓
reproduce or collect evidence
      ↓
diagnosis / candidate causes
      ↓
identify owning component
      ↓
human or explicit policy selects scope
      ↓
smallest durable correction
      ↓
rerun original failure / acceptance check
```

This aligns directly with HE's existing **Assessment Before Architecture** principle and with the user's established failure-review prompt: classify the failure source, identify the owning component, make the smallest durable correction only when evidence supports it, then rerun the original task fresh.

### Do not over-ritualize

A diagnosis artifact is not needed for every trivial, already-reproduced defect. If a compiler/test gives a deterministic cause and the repair is tightly bounded, an additional model-produced diagnosis may add ceremony without evidence.

### HE disposition

**STRONGLY REINFORCE.**

Candidate principle:

> **When failure ownership or scope is ambiguous, establish diagnosis before authorizing mutation.**

---

# 3. “Prototype with MCP, then turn it into a custom skill” is directionally useful but technically too broad

Saraev argues that MCP is useful for rapid prototyping but can be context-heavy and that repeated workflows should often become lean custom skills.

The durable idea is good:

> **Prototype with a broad capability surface; productionize only the minimum capability the repeated workflow actually needs.**

But MCP and Agent Skills are different layers.

## MCP owns live capabilities

MCP is appropriate when the agent needs:

- authenticated access to external systems;
- live data or actions;
- OAuth/session handling;
- remote APIs/databases;
- server-pushed events;
- a reusable external tool boundary.

## Skills own procedure and selective expertise

A skill is appropriate for:

- repeatable procedural instructions;
- scripts/wrappers;
- project/domain knowledge;
- task-specific workflow;
- progressive disclosure of instructions/resources.

A skill can call a CLI/script/API wrapper, or teach the agent how to use MCP tools, but it does not magically replace the external integration layer.

### Current Claude Code changes weaken the blanket context-cost argument

Current Claude Code enables MCP **tool search/deferral by default** on supported first-party models. Only tool names and server instructions load at session start; individual tool definitions are discovered on demand. That means the old pattern “every connected MCP dumps all tool definitions into context” is no longer universally true.

There are still reasons to prune MCP servers:

- startup/connectivity overhead;
- security/trust surface;
- irrelevant capability exposure;
- routing ambiguity;
- provider/proxy environments where tool deferral falls back to upfront loading;
- maintenance and auth burden.

### Better HE decision rule

```text
Need live external system capability?
  yes → MCP/API/CLI capability boundary
             + optional Skill for procedure/routing
  no  → Skill/script may be sufficient

Repeated workflow uses only 1–2 stable operations?
  → consider a narrow deterministic wrapper

Broad evolving service surface / OAuth / events?
  → keep MCP; narrow discovery/allowlist/instructions
```

### HE disposition

**REINFORCE minimum capability surface. REJECT “MCP → Skill” as a universal maturation path.**

Candidate principle:

> **Minimize the exposed capability surface without moving capability ownership into the wrong layer.**

---

# 4. Scoped parallel tasks are useful only when scope is truly independent

Saraev recommends running mutually exclusive scoped tasks concurrently and merging afterward.

First-party Anthropic material supports parallel work when subtasks are independent. Claude Academy specifically distinguishes parallel sessions in separate Git worktrees from subagents used as scoped helpers. Anthropic's multi-agent research also cautions that coding tasks often contain fewer genuinely parallelizable units than breadth-first research.

### Four different parallel patterns should not be conflated

## A. Parallel read-only exploration

Examples:

- inspect different subsystems;
- research alternatives;
- search logs/docs;
- independent reviewers.

Low merge risk; strong candidate for fan-out.

## B. Parallel verification

Examples:

- security review;
- architecture drift;
- correctness review;
- separate test/evidence checks.

Useful because independence can reduce shared blind spots.

## C. Parallel mutation of disjoint scopes

Requires stronger controls:

- explicit file/component ownership;
- ideally separate worktrees/branches;
- known shared-file hotspots;
- dependency ordering;
- merge/reconciliation step;
- deterministic verification after integration.

## D. Parallel mutation of shared state

High conflict and coordination risk. Avoid unless the orchestration layer can explicitly manage ownership and settlement.

### “Mutually exclusive files” is not sufficient

Two tasks can touch different files but still conflict semantically through:

- shared interfaces;
- schemas;
- generated code;
- dependency versions;
- migrations;
- tests/configuration;
- behavior contracts.

The real test is dependency/ownership independence, not merely file disjointness.

### HE disposition

**REINFORCE bounded parallelism.**

Candidate principle:

> **Parallelize independent evidence gathering freely; parallelize mutation only with explicit ownership, isolation, and integration verification.**

---

# 5. Fan-out / fan-in is the strongest idea in the video

Saraev's pattern is:

```text
task
  ↓
fan out to several cheaper/short-context workers
  ↓
collect approaches/evidence
  ↓
fan in to a stronger model
  ↓
synthesis / decision
```

This has strong first-party support. Anthropic's `Building Effective Agents` identifies parallelization and orchestrator-worker patterns as useful for sectioning search spaces and collecting multiple perspectives. Anthropic's multi-agent research system similarly uses a stronger lead agent with parallel subagents for breadth-first research and reports strong gains on its internal research eval.

However, Anthropic also reports the cost: multi-agent systems can use much more token budget, and coding has fewer naturally parallelizable tasks than research.

The HE value is therefore **not** “always use five cheap agents.”

It is the information architecture:

> **Spend breadth cheaply where independent exploration has value; concentrate expensive judgment at the point where evidence must be reconciled.**

## 5.1 Fan-out has several distinct purposes

### Search-space sectioning

Each worker explores a different region/source/question.

Good for:

- research;
- architecture alternatives;
- repo/domain discovery;
- incident hypothesis generation.

### Independent attempts / diversity

Several workers solve the same question independently.

Good for:

- reducing path dependence;
- discovering alternative hypotheses;
- review/critique;
- adversarial testing.

Do not treat majority vote as truth unless calibrated.

### Specialist perspectives

Different workers apply different lenses:

- security;
- correctness;
- maintainability;
- performance;
- user experience.

This maps closely to ACR's multi-domain reviewer architecture.

## 5.2 Fan-in must preserve provenance

A “mega prompt” that flattens all worker output can destroy important information:

- which source supports which claim;
- whether two workers independently found the same evidence or copied the same source;
- whether reports conflict;
- what was searched but not found;
- confidence/authority class;
- whether a statement is evidence or hypothesis.

A stronger fan-in input is structured:

```text
worker / scope
claim or candidate
source / evidence pointer
confidence / authority class
known uncertainty
conflicts / duplicates
coverage gap
```

The synthesizer can then deduplicate, challenge conflicts, and identify missing coverage instead of merely summarizing prose.

## 5.3 The expensive model should synthesize, not automatically own final authority

For research or reversible design choices, the strong model may make the synthesis call.

For consequential actions, HE's existing authority model still applies:

```text
cheap scouts / specialists
        ↓
strong synthesizer / reviewer
        ↓
explicit acceptance authority
        ↓
deterministic or human gate where required
```

A more expensive model is not automatically an acceptance authority.

## 5.4 Model cost is secondary to measured role fitness

Saraev frames scouts as cheap and the synthesizer as expensive. This is a good optimization heuristic, not a law.

HE already has the stronger rule:

> **Route by demonstrated workload fitness, not model reputation or stage label.**

Some specialist search tasks may require a stronger model; some synthesis tasks may be deterministic enough not to require one.

### Candidate HE principle

> **Explore widely, adjudicate narrowly. Preserve evidence identity through the fan-in boundary.**

### HE disposition

**HIGH-VALUE ASSESS / likely principle candidate.**

---

# 6. Evals: strongly aligned, but 10 runs is a heuristic, not a statistical standard

The video correctly warns against trusting a single successful output and recommends repeated evals before standardizing a workflow.

HE already strongly supports behavioral evals for instructions, skills, models and harness configuration.

Do not encode “10 runs” as a universal requirement. Sample size should depend on:

- expected variance;
- consequence of failure;
- evaluation cost;
- observed effect size;
- whether the task is deterministic enough for fixture-based regression.

**Disposition: STRONGLY REINFORCE behavioral evaluation; reject fixed run count.**

---

# 7. Self-checking: useful producer feedback, not independent verification

Saraev recommends giving Claude a way to inspect and revise its own work through screenshots, tests, Lighthouse, examples, etc.

This is valuable and already matches HE's producer feedback loop.

Do not collapse this into independent assurance. The same agent can preserve its own assumptions and blind spots.

Use:

```text
producer change → deterministic/observable feedback → producer repair
```

then, where risk warrants:

```text
fresh/independent review → acceptance evidence
```

**Disposition: STRONGLY REINFORCE existing distinction.**

---

# 8. “Let the code be the context” contains a good warning and a bad universal conclusion

The warning is valid: duplicated notes/specs that claim to describe current behavior can drift from the actual implementation.

But the conclusion “bury things directly into the script” is too broad.

Code is authoritative for what the program currently does. It is often **not** authoritative for:

- why a decision was made;
- rejected alternatives;
- product intent;
- future constraints;
- governance;
- security policy;
- cross-repository ownership;
- operational procedures.

Inline comments can drift too.

### HE rule

> **Put each fact in the source that actually owns it; avoid shadow documentation pretending to own the same fact.**

This matches HE's Single Ownership principle better than “code is all context.”

**Disposition: REINFORCE anti-duplication; REJECT code-as-universal-authority.**

---

# 9. Skills are not obsolete

The video says Skills are a naive/older way of operationalizing tasks and are “not the best way” anymore.

Current first-party Anthropic evidence contradicts that as a general claim. Anthropic reports hundreds of skills in active internal use and describes Skills as a major Claude Code extension point. Current skills use progressive disclosure: the name/description are available for discovery, while the full body/resources load when invoked.

HE should therefore keep the existing nuanced view:

- use skills for reusable task-local procedure/knowledge;
- do not put all project truth into skills;
- keep deterministic behavior in code/tools/hooks where appropriate;
- measure routing and retrieval behavior;
- retire skills whose value disappears as models improve.

**Disposition: REJECT “skills are obsolete.”**

---

# 10. Fresh handoff strongly reinforces current PMB/HE direction

Saraev recommends summarizing current state and starting a fresh session when context degrades.

This independently aligns with HE/PMB and the Orca source review:

```text
current project truth
+ concise successor-oriented state
+ reachable history
        ↓
fresh session
```

The video suggests manually correcting the handoff summary before transfer, which is useful because it recognizes the summary itself can be wrong.

HE should retain the stronger current rule: repository/current project state outranks stale handoff text, and full-transcript transplantation is not the default.

**Disposition: STRONGLY REINFORCE. No PMB redesign implied.**

---

# 11. `/btw` is a product feature with a useful HE abstraction

The specific command is not an HE concern.

The useful abstraction is **side-channel context isolation**: educational/status questions that do not need to affect the main task should not pollute the task's primary working context.

This is another instance of progressive disclosure/context locality.

**Disposition: REINFORCE concept; no HE feature required.**

---

# 12. CLAUDE.md and “learn from mistakes”: promote durable rules to the owning layer, not always CLAUDE.md

Saraev recommends keeping CLAUDE.md lean and converting mistakes into positive instructions rather than accumulating negative scar tissue.

The direction is useful, but the destination should be chosen by ownership.

After an observed failure, ask:

```text
Was the cause:
  process?
  missing context/retrieval?
  weak rule?
  unreliable code/tool?
  configuration?
  execution error?

Which component owns the missing/incorrect fact?
```

Then make the smallest durable correction **there**.

Examples:

- deterministic defect → code/test/hook;
- project-wide behavioral rule → CLAUDE.md/rules;
- repeatable procedure → Skill;
- external capability → MCP/API/CLI tool;
- durable project decision → owning project documentation/memory;
- one-off execution mistake → maybe no permanent rule at all.

### HE implication

> **Do not turn every failure into startup context.**

This is directly aligned with the user's existing failure-analysis prompt and HE's ownership/context principles.

**Disposition: STRONGLY REINFORCE with ownership correction.**

---

# 13. Backup model/tool reinforces portability, but shared instruction files need semantic care

Maintaining a second coding agent can improve resilience against outages, rate limits and provider-specific failures.

That fits HE's portability/exit-cost work.

However, blindly symlinking `CLAUDE.md` and `AGENTS.md` assumes both hosts interpret identical content with identical precedence/semantics. That should be verified rather than assumed.

A better long-term pattern may be:

```text
shared provider-neutral project truth
        ↓
thin host-specific instruction adapters where semantics differ
```

Do not create adapters unless actual divergence exists.

**Disposition: REINFORCE portability; ASSESS instruction compatibility.**

---

# Cross-project implications

## Harness Engineering

High-value additions:

- adaptive procedural autonomy / “loosen path, preserve boundary”;
- diagnosis-before-mutation for ambiguous failures;
- minimum-capability-surface decision rule for MCP vs Skill/tool wrappers;
- parallelism taxonomy by read/verify/mutate scope;
- fan-out/fan-in with provenance-preserving synthesis;
- side-channel context isolation;
- failure-learning correction must land in the owning component.

## PMB

No immediate change.

The source reinforces:

- fresh-session handoff;
- concise successor state;
- current project truth outranks handoff/history;
- do not stuff every learned lesson into startup context;
- retrieval/context should remain progressive.

Potential future pilot observation: whether PMB makes diagnosis/ownership state easy enough to transfer without carrying the entire debugging transcript.

## ACR

Fan-out/fan-in is already structurally adjacent to ACR's multi-domain reviewers, but this source alone does not justify changing ACR.

Potential future question after current benchmark work:

- does ACR's orchestration preserve independent reviewer evidence and conflicts well enough during fan-in, or does synthesis flatten provenance?

Do not modify ACR from this video alone.

---

# Overall disposition

## STRONGLY REINFORCE

- definitions of done over unnecessary micromanagement;
- revalidate harness scaffolding as models improve;
- diagnose before mutation when ownership/scope is ambiguous;
- producer self-checking plus separate independent assurance;
- progressive disclosure/context hygiene;
- fresh-session handoff;
- corrections belong in the owning component;
- parallel read/research/verification where independent.

## ASSESS

- fan-out/fan-in as a formal HE pattern with provenance-preserving synthesis;
- parallel mutation contract using ownership + worktree isolation + integration verification;
- minimum-capability-surface decision matrix for MCP/Skill/script/tool boundaries;
- whether ACR fan-in currently preserves reviewer provenance/conflict information sufficiently.

## REJECT / CORRECT

- “loosen the leash” as permission expansion;
- skills are obsolete;
- code is the universal source of truth;
- MCP should always be replaced by a Skill after prototyping;
- mutually exclusive files alone prove tasks are parallel-safe;
- majority vote from multiple agents is truth;
- the most expensive model is automatically the final authority;
- every model mistake should become a CLAUDE.md rule;
- fixed eval run counts as statistical doctrine.

---

# Candidate principle formulations — research status

> **Constrain outcomes and authority; prescribe the execution path only where evidence shows the model needs it.**

> **When failure ownership or scope is ambiguous, establish diagnosis before authorizing mutation.**

> **Minimize the exposed capability surface without moving capability ownership into the wrong layer.**

> **Parallelize independent evidence gathering freely; parallelize mutation only with explicit ownership, isolation, and integration verification.**

> **Explore widely, adjudicate narrowly. Preserve evidence identity through the fan-in boundary.**

These require cross-source review before promotion to formal HE principles.
