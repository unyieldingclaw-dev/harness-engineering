# Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation — 2026-09-25

## Purpose

This note synthesizes durable Harness Engineering findings from the September 25 review of The Next New Thing's **Top Repos + Fame, Traffic & Agents** batch and the underlying repositories.

Source evidence:

- `01 Research/Sources/The Next New Thing — Top Repos, Fame, Traffic & Agents — 2026-09-25.md`

The source batch spans unrelated products. The useful HE signal comes from recurring architectural boundaries rather than the products themselves:

- portable artifacts versus host-owned execution semantics;
- semantic judgment versus deterministic authority;
- candidate harness changes versus behavioral proof;
- narrative explanation versus durable lifecycle state;
- reusable capability sources versus host-specific wrappers;
- rich retrieval pipelines versus evidence that such infrastructure is actually needed.

This is research synthesis, not authorization to change PMB, work MB, ACR, agent instructions, Skills, permissions, retrieval architecture or orchestration.

## 1. Portable Artifact Does Not Mean Portable Behavior

Claude Code's new `AGENTS.md` support is useful because it can reduce duplicate instruction ownership across clients. But the implementation also demonstrates why file-format convergence is not behavioral convergence.

A host still owns:

- which instruction filenames it discovers;
- precedence when several instruction sources exist;
- root-to-leaf traversal;
- nested-file attachment;
- import handling;
- compaction/restoration behavior;
- subagent inheritance;
- omission rules;
- user/managed/project/local layering;
- when changed instructions become visible to the running session.

Therefore the portable unit is not simply:

```text
AGENTS.md
```

It is closer to:

```text
instruction artifact
+ host discovery semantics
+ precedence
+ lifecycle / attachment behavior
```

### HE implication

Single Ownership still favors one canonical instruction source where practical. But host adapters or client-specific discovery configuration may still be necessary.

Candidate principle:

> **Portable syntax does not imply portable execution semantics.**

This reinforces earlier Skill portability research: the same Skill folder, instruction file or command can move between clients while discovery, tools, model, context, authority and runtime still change behavior.

### Evaluation consequence

When comparing two clients that consume the same `AGENTS.md`, the comparison should record at least the material differences in:

- instructions actually loaded;
- load order / precedence;
- nested attachment behavior;
- model/provider/runtime;
- tools/capabilities;
- execution authority.

Do not infer behavioral equivalence merely from identical repository files.

## 2. Monotonic Authority Across Trust Layers

The `jev-auto-mode` experiment in Claude Code Templates exposes a useful general policy pattern independent of Jev itself.

The user-level policy can define authority. Project-level policy is less trusted and, by default, may only make the policy stricter. A cloned repository cannot silently grant itself more authority.

Conceptually:

```text
managed / user authority
        ↓
project may tighten
        ↓
session / task may operate inside that envelope
```

not:

```text
repository says "allow"
        ↓
repository expands its own authority
```

This is a strong candidate governance principle:

> **Less-trusted configuration should be monotonic with respect to authority: it may restrict inherited authority, but should not silently expand it.**

Examples beyond tool permissions could include:

- network access;
- secret access;
- destructive commands;
- deploy/publish authority;
- MCP capability exposure;
- subagent spawning;
- approval requirements;
- filesystem write scope.

### Governing policy must not be governed by the agent

`jev-auto-mode` also protects the policy files and enforcement code from the model whose actions they govern.

That reinforces a broader HE boundary:

> **An enforcement mechanism should not normally sit inside the mutation authority of the actor it constrains.**

This parallels the `autoresearch` pattern where the experimenter can modify `train.py` but not silently redefine the evaluation mechanism that decides whether the experiment improved.

### Failure behavior matters

A permission layer that crashes and silently allows the action has inverted its purpose.

The reviewed implementation favors:

- last-known-good policy over an empty/broken reload;
- ask/deny on uncertain or failed judgment;
- audit mode before enforcement;
- visible parse/configuration errors.

HE should not universalize "fail closed" to every subsystem: an observability dashboard or optional retrieval adapter may correctly fail open so core work continues. The failure mode must match the responsibility.

Candidate distinction:

```text
security / authority boundary
→ conservative failure

optional analysis / observability
→ degraded-but-operational failure
```

## 3. Probabilistic Judgment Belongs Under Deterministic Invariants

NewsJack's Jev integration and `jev-auto-mode` independently show a useful composition:

```text
ambiguous semantic question
        ↓
probabilistic judgment
        ↓
deterministic policy / floors
        ↓
stable artifact or bounded action
```

NewsJack uses Jev to answer a broad semantic relevance question cheaply, then deterministic post-rules protect expensive failure classes such as unsafe rejection or profile/safety contradictions.

`jev-auto-mode` does the inverse ordering for authority: deterministic rules decide known cases first, and only uncovered ambiguous cases may be judged probabilistically.

Both point to the same design boundary:

> **Use model judgment for ambiguity; use code for invariants.**

### Stable downstream contracts

NewsJack also preserves the existing `coarse_relevance_decisions.json` contract. Downstream stages do not need to know whether a low-cost worker or Jev produced the semantic decision.

This is desirable because model/runtime substitution should not force unrelated architecture changes when the semantic role and output contract remain the same.

Candidate principle:

> **Model substitution should occur behind a stable capability contract where the task semantics are unchanged.**

### Confidence is evidence, not permission by itself

Typed probabilities are valuable for:

- conservative escalation;
- uncertainty floors;
- identifying cases that need deeper review;
- calibration;
- comparing model behavior over time.

But a probability is not automatically an authority grant. Especially for destructive, security-sensitive or irreversible actions, HE should continue to favor deterministic boundaries or explicit approval where practical.

## 4. Behavioral Skill Evaluation Should Be a First-Class Harness Practice

OpenSEO's internal `evaluate-skill` workflow is one of the strongest concrete examples reviewed so far of testing a Skill as a behavioral intervention rather than reviewing its prose.

The key insight is simple:

> **A Skill edit is not better because the file looks clearer; it is better if representative behavior improves without unacceptable regressions.**

### Minimal evaluation envelope

A strong Skill evaluation should consider the following controls where relevant:

```text
candidate Skill frozen at known hash
           ↓
fresh isolated session
           ↓
neutral task prompt
           ↓
known / bounded capabilities
           ↓
representative task(s)
           ↓
output scored independently of trace
           ↓
trace inspected to classify failure
           ↓
holdout / regression task
```

Useful mechanisms from OpenSEO:

- freeze the candidate instructions before execution;
- record exact hashes/identity;
- do not let edits during the run alter the experiment;
- use fresh sessions rather than resumed/forked conversational state;
- isolate task data from evaluator material;
- keep prompts neutral across variants;
- restrict tools to the intended capability surface;
- preserve every run, including failures;
- use a holdout task with a different shape;
- score the produced work before reading execution traces;
- inspect trace afterward to locate the failure class;
- distinguish discovery failure from tool, reasoning and reporting failure.

### Why output-before-trace matters

Reading a trace first can bias the evaluator toward excusing a poor output because the process looked thoughtful, or condemning a good output because the path looked messy.

The artifact/result should first be judged against its acceptance criteria. The trace is then diagnostic evidence.

This gives HE a useful two-stage review model:

```text
Did the work satisfy the task?
        ↓
Why did it succeed or fail?
```

not:

```text
The agent used a respectable process
        ↓
therefore the result is good
```

### Holdouts protect against instruction overfitting

A Skill can become very good at the failure that motivated its latest edit while becoming worse elsewhere.

Therefore:

> **A local fix should be tested both on the motivating case and on at least one materially different case that could expose overfitting.**

No fixed count is promoted here. The number of runs depends on variance, consequence and cost.

### "Add a step, not a warning"

OpenSEO's project-local observation that a shorter process-oriented Skill outperformed a longer caution-heavy one is consistent with HE's current context findings.

It does not prove that shorter instructions always win. It does reinforce the preference to correct ownership/mechanism rather than accumulate negative prose.

A recurring failure may justify:

- a deterministic check;
- a clearer workflow step;
- a routing trigger;
- a tool/interface change;
- a regression fixture;
- or no durable change.

It does not automatically justify another warning paragraph.

## 5. Experience-Derived Evolution Now Has an Evaluation Partner

Earlier HE research established:

> **History proposes harness changes; experiments earn them.**

The current batch fills in an important missing middle.

Harness Miner / transcript analysis can identify candidates:

```text
repeated corrections
repeated retrieval misses
repeated tool failures
obsolete rules
repeated manual procedures
```

But those findings should feed an isolated behavioral evaluation before changing the durable harness.

Conceptually:

```text
real usage history
      ↓
Harness Miner / incident diagnosis
      ↓
candidate owning-component change
      ↓
frozen candidate
      ↓
fresh behavioral eval + holdout
      ↓
result + trace evidence
      ↓
human/governed acceptance
      ↓
durable change
```

This avoids the dangerous loop:

```text
model makes mistake
→ model interprets mistake
→ model writes rule about itself
→ rule becomes new truth
```

### Implication for Harness Miner

Harness Miner should eventually be able to hand off a candidate correction and the evidence that motivated it, but it should not become the evaluator or acceptance authority by default.

Useful future interface concept only:

```text
candidate finding
owner
supporting sessions
representative reproduction
suggested eval cases
validation status
```

No implementation follows yet.

## 6. Structured State Must Own Lifecycle Authority

Paperclip provides unusually concrete evidence for a problem HE has repeatedly encountered: free text is useful context but dangerous as an implicit state machine.

Examples of text that should not silently become authoritative scheduling state include:

- "I'm done";
- "blocked";
- "continue later";
- a generated `nextAction` sentence;
- a handoff summary;
- a comment count interpreted as progress;
- an extracted diagnostic label.

Paperclip's correction is to derive continuation from persisted structured facts and explicit API/tool effects.

Candidate principle:

> **Narrative may describe lifecycle state; durable structured state should own lifecycle authority.**

### Separate structure, dependency, ownership and execution

Paperclip's explicit distinction is useful well beyond its product:

- **structure** — how work is decomposed;
- **dependency** — what must happen before work can continue;
- **ownership** — who currently owes the next action;
- **execution** — whether a live execution path exists.

Collapsing these into one `status` or one prose summary creates ambiguity during retry, handoff, crash recovery and parallel work.

### Known precondition is not a failed attempt

If the system knows before dispatch that:

- a secret is missing;
- approval is outstanding;
- a dependency is unresolved;
- the budget is exhausted;
- the workspace cannot be constructed;
- the assigned execution engine is unavailable;

then starting the agent and recording a runtime failure is the wrong abstraction.

Candidate principle:

> **Known unsatisfied preconditions should be modeled as gates/waits, not consumed execution attempts.**

This improves cost accounting, failure classification and recovery semantics.

## 7. Control Plane Is Not Agent Runtime

Paperclip, Orca and Octop all reinforce the need to distinguish control-plane responsibilities from agent internals.

A control plane may own:

- who owns a task;
- when execution may start;
- dependency state;
- budget/approval gates;
- active-run identity;
- retries/wake ownership;
- cross-agent work objects;
- cost attribution;
- pause/resume.

The agent runtime may own:

- reasoning strategy;
- model interaction;
- local tools;
- project-specific instructions;
- task execution details;
- internal context management.

This separation is more useful than an "AI company" metaphor.

Candidate principle:

> **Orchestration should govern execution relationships without becoming a second owner of each agent's reasoning, memory or project truth.**

No current HE requirement justifies building such a control plane.

## 8. Capability Source Can Outlive Its Host Adapter

Anthropic's Knowledge Work Plugins and Financial Services repositories provide current examples of a useful portability shape:

```text
shared domain capability source
   ├─ skills
   ├─ workflow instructions
   ├─ connectors
   └─ commands
          ↓
interactive host adapter
managed/headless agent wrapper
other compatible client
```

The reusable source can remain stable while the execution surface changes.

This strengthens Single Ownership:

> **Author durable domain procedure once; wrap it for clients rather than independently re-authoring the same expertise for every host.**

But this only works when host-specific differences are explicit. A wrapper may still own:

- system prompt framing;
- available connectors;
- authority;
- subagent topology;
- event/handoff protocol;
- output staging.

### Agent versus capability

The same repositories reinforce the existing HE question:

- Skill: domain knowledge/procedure;
- command: explicit invocation;
- connector/tool: external capability;
- agent: independent objective/state/authority/actor where actually needed.

Do not create a separate agent merely because a domain is specialized.

## 9. Retrieval Infrastructure Should Be Decomposed Before It Is Adopted

WeKnora is useful because its current implementation makes the retrieval path concrete rather than calling the entire thing "RAG."

Potential stages include:

- query understanding;
- rewrite/expansion;
- semantic/keyword/entity retrieval;
- reranking;
- optional web fetch;
- merge/deduplication;
- top-k filtering;
- context assembly;
- citations;
- generation.

This gives HE a better way to investigate a retrieval problem.

Instead of:

```text
retrieval is bad
→ add graph/RAG platform
```

ask:

```text
Did routing fail?
Did the query need rewrite?
Was candidate recall poor?
Did ranking fail?
Was the relevant source retrieved but omitted from context?
Did context contain it but the model ignore it?
Was the source itself stale/wrong?
```

The smallest failing stage should own the experiment.

This directly complements the recent Graphify evidence that a derived retrieval/index layer can be useful without improving total task economics.

### Current disposition

- staged retrieval mechanisms: **ASSESS when evidence points at the corresponding failure**;
- knowledge graph / enterprise RAG / automatic long-term-memory system: **PARK**;
- automatic adoption because the product is sophisticated: **REJECT**.

## 10. Marketplaces Are Discovery Surfaces, Not Trust Anchors

Claude Code Templates and similar catalogs make Skills, commands, agents, hooks and MCP integrations easier to discover.

That creates useful ergonomics, but also a supply-chain boundary.

Signals such as:

- stars;
- downloads;
- marketplace ranking;
- inclusion in a curated catalog;
- popularity in a weekly video;

may help prioritize inspection. They do not prove:

- safety;
- license compatibility;
- maintenance quality;
- instruction quality;
- absence of prompt/tool injection;
- authority scope;
- value for the local harness.

HE should preserve this distinction:

> **Discovery metadata can prioritize review; it cannot substitute for review.**

This ties directly to the Skillspector research direction.

## 11. Cross-Project Implications

### PMB / work MB

No immediate architecture change.

Potential future implications:

- one canonical cross-host instruction source may become attractive, but only after verifying actual host discovery/precedence behavior;
- retrieval stages should be added only against measured retrieval failures;
- transcript/history mining should propose changes, not write durable memory automatically;
- lifecycle/handoff prose should remain derived context rather than authority.

### Harness Miner

This batch gives Harness Miner a clearer role:

1. detect recurring candidate problems from real usage;
2. identify the likely owning component;
3. produce representative reproductions/examples;
4. hand off to a separate behavioral evaluation process;
5. preserve `untested` until evidence exists.

A future Harness Miner should not automatically mutate Skills or policy.

### ACR

Potential later experiments:

- cheap typed screening/routing behind stable finding contracts;
- deterministic floors around model-derived classifications;
- frozen-reviewer/prompt behavioral evaluation with fresh sessions and holdout repos/PRs;
- structured lifecycle state if ACR ever gains autonomous fixer/orchestrator behavior.

Do not interrupt the current AACR-Bench/evidence-quality work merely because these mechanisms are interesting.

### HE itself

The most durable addition is methodological: evaluate harness components as interventions under controlled conditions rather than accumulating them because they appear useful in isolation.

## 12. Candidate Principles

These are research candidates. They are **not yet promoted automatically into `00 Overview/Harness Engineering Philosophy.md.md`.**

### Strong REINFORCE candidates

> **Portable syntax does not imply portable execution semantics.**

> **Use model judgment for ambiguity; use code for invariants.**

> **Narrative may describe lifecycle state; durable structured state should own lifecycle authority.**

> **Known unsatisfied preconditions should be modeled as gates/waits, not consumed execution attempts.**

> **Discovery metadata can prioritize review; it cannot substitute for review.**

### Strong ASSESS candidates

> **Less-trusted configuration should be monotonic with respect to authority: it may restrict inherited authority, but should not silently expand it.**

> **A Skill change should earn persistence through fresh-session behavioral evaluation, including a materially different holdout when overfitting is plausible.**

> **Model substitution should occur behind a stable capability contract where task semantics are unchanged.**

## 13. Research Disposition

- **REINFORCE:** Single Ownership across reusable capability sources.
- **REINFORCE:** host/runtime semantics remain part of the evaluated harness even with shared instruction formats.
- **REINFORCE:** deterministic invariants around probabilistic judgment.
- **REINFORCE:** fresh-session behavioral evaluation of harness changes.
- **REINFORCE:** structured lifecycle state over prose-derived authority.
- **ASSESS:** monotonic trust-layer policy as a general HE governance principle.
- **ASSESS:** reusable Skill evaluation protocol for HE/work MB/ACR experiments.
- **ASSESS:** cheap typed decision models for bounded classification/routing stages.
- **ASSESS:** cross-host canonical instructions only after client semantics are measured.
- **PARK:** Paperclip-like control plane, WeKnora-like RAG/graph infrastructure, Octop AgentTeams, automatic conversation-to-Skill self-evolution.
- **REJECT:** popularity, star count, marketplace curation or weekly trending status as adoption evidence.
- **REJECT:** automatically turning recurring model mistakes into permanent rules without evaluation.
- **REJECT:** letting free-text summaries/comments silently become authority-bearing scheduler state.

## 14. Practical HE Test Loop

The combined evidence from this batch supports a compact default research loop:

```text
1. Observe an actual failure / repeated friction.
2. Identify the owning component.
3. Propose the smallest candidate correction.
4. Freeze the candidate at an exact identity/hash.
5. Re-run in a fresh session with neutral task framing.
6. Include a holdout/regression case when appropriate.
7. Judge the output against acceptance criteria first.
8. Inspect the trace to diagnose why it passed or failed.
9. Preserve failures and uncertainty.
10. Promote only if evidence earns persistence.
```

This is not a mandatory ceremony for trivial deterministic fixes. It is the preferred shape when changing instructions, Skills, routing, policy, retrieval or other harness components whose behavioral effects are probabilistic.

## Bottom Line

The weekly repo list itself does not justify any new HE architecture.

The implementations underneath it do sharpen several HE boundaries:

- one source artifact can serve several hosts, but host execution semantics remain real;
- probabilistic models are strongest inside deterministic envelopes;
- security policy should not be loosenable by less-trusted project state;
- harness changes should be behaviorally evaluated, not accepted because their prose is persuasive;
- structured state should own orchestration authority;
- sophisticated retrieval infrastructure should be decomposed and justified by measured failure;
- discovery surfaces help us find candidates, not decide what deserves adoption.

The most actionable near-term research item is a **small reusable behavioral Skill-evaluation protocol**, because it directly connects existing HE work on Experience-Derived Harness Evolution, Harness Miner, progressive disclosure, regression testing and instruction scar-tissue removal without requiring new runtime architecture.