# Lauren Tan (poteto) — pstack, Noodle, Trust Ladder & High-Throughput Agent Engineering — 2026-10-21

## Source set

### Interview supplied by Nolan

Matt Pocock — **LIVE: Poteto (creator of pstack) on shipping 1,000's of PR's a month at SpaceX**

- https://www.youtube.com/watch?v=MN9dGgmLyso&t=748s
- Full transcript supplied in the research session.
- Lauren Tan / poteto public profile supplied by Nolan:
  - https://x.com/poteto

The X profile itself could not be reliably fetched from the research environment because X returned HTTP 403. Tweet-level claims are therefore not promoted unless the interview or public GitHub artifacts independently support them.

### Primary public implementation sources inspected

#### pstack

Official source:

- https://github.com/cursor/plugins/tree/main/pstack
- repository HEAD inspected: `190d28736eff128ec94f502a22c7fe0c4b56cd37`
- pstack plugin metadata at that checkout: `0.15.29`

Relevant inspected artifacts include:

- `pstack/README.md`
- `pstack/skills/poteto-mode/SKILL.md`
- `pstack/skills/create-verification-skill/SKILL.md`
- `pstack/skills/maintain-verification-skill/SKILL.md`
- `pstack/skills/correct/SKILL.md`
- `pstack/skills/automate-me/SKILL.md`
- `pstack/skills/reflect/SKILL.md`
- `pstack/skills/recall/SKILL.md`
- `pstack/skills/show-me-your-work/SKILL.md`
- `pstack/skills/interrogate/SKILL.md`
- `pstack/skills/swarm/SKILL.md`
- `pstack/skills/arena/SKILL.md`
- `pstack/skills/figure-it-out/SKILL.md`
- selected `principle-*` skills
- autonomous-run and orchestration playbooks

#### Noodle

- https://github.com/poteto/noodle
- repository HEAD inspected: `82d2921c52370f23f29086de81ccfb600939c037`

Relevant inspected artifacts include:

- `brain/architecture.md`
- `.agents/skills/schedule/SKILL.md`
- `.agents/skills/execute/SKILL.md`
- `.agents/skills/quality/SKILL.md`
- `.agents/skills/reflect/SKILL.md`
- `.agents/skills/meditate/SKILL.md`
- `.agents/skills/oops/SKILL.md`
- `.agents/skills/worktree/SKILL.md`
- `internal/dispatch/dispatch.go`
- `internal/mode/mode.go`
- archived completed design plans for:
  - scaling the loop;
  - event triggers;
  - work orders;
  - deterministic completion detection;
  - unified involvement levels.

---

# 1. Evidence boundary: the throughput number is not independently verified

The interview centers on Lauren's statement that she shipped roughly 2,500 pull requests in the prior month.

That is a practitioner report about private production work.

Public pstack and Noodle artifacts demonstrate that Lauren has built substantial tooling for:

- verification;
- multi-agent execution;
- model routing;
- worktree isolation;
- autonomous runs;
- review;
- scheduling;
- event-driven orchestration.

They do **not** independently prove the private PR count, production acceptance rate, defect rate, or business value of those PRs.

### HE disposition

Treat the volume claim as:

```text
SELF-REPORTED PRACTITIONER OUTCOME
not
INDEPENDENT PERFORMANCE BENCHMARK
```

The mechanisms can still be evaluated on their own evidence.

---

# 2. Verification is the trust ladder's enabling property

The strongest interview claim is not "run more agents."

Lauren says verification was the first capability that materially let her climb the trust ladder.

Her definition is operational:

- run the real application;
- interact with it the way a user would;
- capture traces/snapshots;
- inspect runtime behavior;
- iterate from observed evidence.

pstack materially implements this idea.

`create-verification-skill` generates project-local, executable instructions covering:

- Launch
- Doctor
- Drive
- Evidence
- Cleanup
- feature-level user journeys

The skill explicitly says the proof must exercise the real user path rather than rely only on internal setters or test-only endpoints.

### HE relationship

This strongly reinforces:

- "Eyes as Evidence Acquisition";
- guides/sensors;
- execution success vs effect verification;
- acceptance-relevant observation;
- environment before model escalation.

### Durable implication

> **Autonomy should rise only as the system can obtain evidence about the real effect of its work.**

Model confidence is not the same thing as verifiability.

---

# 3. Compile deterministic work out of the agent

In the interview Lauren describes extracting mechanical parts of a skill into scripts/CLI code, leaving only judgment-sensitive work to the model.

Public pstack makes this principle explicit.

`principle-build-the-lever` says non-trivial work should often produce a rerunnable tool:

- codemod;
- generator;
- script;
- deterministic checker;
- shared delegate skill.

It also states:

> A deterministic lever beats fan-out.

The reason is not only token savings. A deterministic artifact is:

- consistent;
- rerunnable;
- reviewable;
- shareable across agents;
- easier to verify than repeated hand execution.

### New HE refinement

HE already says deterministic checks should absorb deterministic review work.

This source broadens that principle from **review** to **execution**.

> **Before parallelizing probabilistic workers, ask whether the repeated work can be compiled into a deterministic lever.**

This is particularly relevant to the Misfit Unit: a new specialist should not exist merely to repeat a mechanical transformation that a script can perform exactly.

---

# 4. Repeated corrections should rise toward structure, not accumulate as prose

Lauren describes observing recurring agent mistakes and asking how to make the codebase prevent them.

pstack's `correct` skill operationalizes a hierarchy:

1. eliminate the mistake through architecture;
2. make invalid states unavailable through types;
3. enforce remaining mechanical cases with lint / CI;
4. test the behavior;
5. use docs / agent rules last for genuine judgment.

It requires evidence that the mistake class is recurring and asks new checks to fail on a real past mistake.

This closely matches HE's owning-component / smallest-durable-correction philosophy but provides a useful enforcement ordering.

### Durable refinement

> **When a correction repeats, promote it toward the strongest owning primitive that can make the failure impossible or directly observable; prose is the fallback for irreducible judgment.**

Do not universalize pstack's exact "twice" threshold. Recurrence evidence and consequence matter.

---

# 5. Buffer observations before mutating the environment when the pattern is unclear

A less obvious interview point is especially valuable.

Lauren describes an agent that continually identifies suspicious React patterns but **does not immediately fix each one**. It appends observations to a buffer. Periodically, the collection is reviewed for a larger common cause.

That prevents a high-throughput loop from converting every local smell into an immediate local patch.

### New HE pattern

```text
observe
   ↓
buffer evidence
   ↓
cluster / identify recurrence
   ↓
attribute owning cause
   ↓
smallest durable correction
```

This complements the existing experience-derived harness-evolution loop.

### Durable refinement

> **When individual observations may be symptoms of one systemic failure, aggregate before mutating. High execution velocity should not collapse diagnosis into one-finding/one-fix.**

The buffer can be a file, issue set, query, structured log, or another source-owned artifact. HE does not require a new storage system.

---

# 6. Sampling can replace blocking review only inside a strong verification envelope

Lauren says she cannot manually review every artifact at her reported scale. Instead she samples output rigorously and uses recurring defects to improve the environment.

This is a real scaling pattern, but it has a critical precondition.

The interview later distinguishes reversible "two-way door" changes from harder-to-reverse "one-way door" consequences and acknowledges that high autonomy is much harder when the domain is difficult to verify programmatically.

### HE refinement

Human oversight can shift from:

```text
review every artifact before effect
```

toward:

```text
deterministic / independent gates
       +
sample accepted output
       +
repair systemic failure sources
```

only when the consequence envelope justifies it.

### Transferable principle

> **Sampling is a process-assurance strategy, not a substitute for consequence-relevant gates. The less reversible or less verifiable the effect, the less safely review can move from blocking to sampling.**

This is stronger than a generic "trust ladder."

Autonomy ceiling is constrained by at least:

- reversibility;
- verifiability;
- blast radius;
- deterministic enforcement;
- recovery quality.

---

# 7. Trust belongs to the environment, not to a model persona

The interview repeatedly frames trust as trust in:

- tools;
- constraints;
- verification;
- repeatable process;
- known environment behavior.

pstack's current implementation corroborates that stance:

- a task routes to a playbook;
- playbooks call skills;
- skills use scripts and verification;
- model assignments are configurable by role;
- the same conceptual workflow can be carried across model families.

### HE implication

> **Trust should attach to an evaluated model + harness + runtime + task envelope, not to an agent name or persona.**

This directly argues against treating a Misfit's personality or title as the source of reliability.

---

# 8. Model diversity can strengthen review; persona diversity does not establish independence

pstack's `interrogate` skill says its adversarial signal comes from **model diversity, not assigned personas**.

Reviewers receive the same intent, code, rubric, and code-quality lens. Their findings are then synthesized with disagreement preserved.

That is a useful implementation of a distinction HE has already made conceptually.

### Durable refinement

Separate reviewer roles can provide organizational isolation, but correlated inference still matters.

When genuine independence is important, stronger options include:

- different model family;
- fresh context;
- protected evaluation corpus;
- independent evidence acquisition;
- deterministic oracle;
- human authority.

### Boundary

Different model families are not automatically independent evidence either. They may share training artifacts, benchmarks, or reasoning patterns.

---

# 9. "Context not control" becomes source-connected self-service

Lauren describes the human bottleneck as repeatedly ferrying external context into coding sessions.

Her outer-loop / inner-loop framing is:

```text
OUTER LOOP
Slack / issue tracker / email / user reports / external state
      ↓
source-connected acquisition
      ↓
INNER LOOP
agent engineering / verification / change
```

The important mechanism is not a "company brain."

It is enabling the agent to retrieve the real source itself rather than making the human a context proxy.

### HE relationship

This reinforces:

- source-owned context;
- connectors as capability boundaries;
- event-driven long-horizon continuation;
- current truth outranking stale summary;
- context acquisition on demand.

### Durable refinement

> **Eliminate the human as a context courier before trying to eliminate the human as a decision authority.**

Automation of retrieval and automation of judgment are different moves.

---

# 10. Agent coordination should organize related work above the individual report

The interview gives a concrete reason not to spawn one independent worker for every incoming issue.

Thirty related bug reports may look like thirty tasks locally but collectively reveal one higher-level cause.

Lauren describes coordinator agents that can group related reports and delegate after seeing the larger pattern.

### HE relationship

This sharpens fan-out/fan-in:

> **Partition by independent information value or implementation seam, not by raw event count.**

An event queue is not automatically a worker queue.

Before fan-out, ask whether grouping several observations changes the diagnosis.

---

# 11. Noodle: organize the control plane around durable work, not ephemeral sessions

Noodle's completed scaling redesign contains one of the strongest new architectural ideas in this source set.

The plan states:

> the loop should be organized around orders, not sessions.

The reason is straightforward:

- sessions are runtime implementation details;
- the control plane cares about durable work state and stage completion;
- polling every session becomes O(n) overhead at scale;
- completion events can update durable order state directly.

Current Noodle source reflects this with order/stage state and dispatch planning.

### HE durable refinement

> **Long-horizon orchestration should identify durable work by its task/order identity; agent sessions are attempts or execution handles, not the primary business identity.**

This directly aligns with HE's existing "one Task can have attempts" work and sharpens it for scale.

---

# 12. Push completion; do not poll every worker when the runtime can signal terminal state

Noodle's scale redesign moves from session polling to push-based completion.

That makes orchestration cost track active events and orders rather than total historical/active sessions.

### HE disposition

This is an implementation pattern, not a universal requirement.

Use it when:

- the runtime exposes dependable completion events;
- worker count makes polling material;
- terminal state can be normalized.

Do not build event infrastructure for a four-agent local workflow that does not need it.

### Transferable principle

> **At scale, prefer terminal-state events over repeated global scans when the event source is trustworthy.**

---

# 13. Event streams need an explicit loss contract

Noodle's event-trigger design makes an unusually explicit choice:

- lifecycle events are append-only NDJSON;
- the event stream is intentionally bounded;
- writes are best-effort;
- older events may be dropped;
- if every event must be acted upon, stronger cursor/delivery semantics would be required.

This is valuable because many agent systems casually call a log an "event bus" without stating its delivery guarantees.

### HE durable refinement

> **Every event source should state whether it is evidence, hint, or required delivery. Lossy context and mandatory commands need different transport semantics.**

A scheduling hint may tolerate loss.

An approval, payment, destructive action, or required reconciliation event may not.

---

# 14. Authority changes should invalidate in-flight effects

Current Noodle code implements a monotonic mode epoch.

When the global human-involvement mode changes, effects stamped under an older epoch can be classified as stale instead of being applied under obsolete authority.

Conceptually:

```text
effect proposed under AUTO epoch 7
        ↓
human changes mode → SUPERVISED epoch 8
        ↓
old effect reaches executor
        ↓
epoch mismatch
        ↓
cancel / refuse stale effect
```

This is a genuinely useful control-plane mechanism.

### New HE durable principle

> **When authority changes, pending effects created under the old authority should not silently retain permission. Bind consequential effects to the authority epoch or equivalent approval state that created them.**

This is stronger than merely checking current configuration when the action was first proposed.

It protects long-running and asynchronous systems from stale permission.

---

# 15. Modes should describe involvement, not merely model cleverness

Noodle exposes three global involvement modes:

- auto;
- supervised;
- manual.

The important idea is not the exact names.

A single operator-facing control coherently gates:

- scheduling;
- dispatch;
- retry;
- merge/effect behavior.

This is preferable to several partially overlapping autonomy knobs when users think in terms of desired involvement.

### HE interpretation

If an orchestration product exposes autonomy, present a small understandable consequence model.

Under the hood, individual enforcement points may still differ.

Do not make a single UI switch the only enforcement mechanism.

---

# 16. Worktree isolation is execution isolation, not project-truth isolation

Both pstack and Noodle use isolated worktrees for parallel mutation.

Noodle adds:

- ephemeral branches;
- serialized merges;
- safe cleanup;
- branch/target ownership.

This strongly supports HE's existing position.

### Boundary

Worktree isolation does not solve:

- shared handoff collisions;
- shared mutable memory;
- common external side effects;
- duplicate task ownership;
- evaluation leakage.

### Transferable principle

> **Separate mutable execution state before adding locks; serialize only the shared boundary that truly must remain singular.**

This is consistent with pstack's `separate-before-serializing-shared-state` principle.

---

# 17. Sparse decision trails can make unattended work reviewable

pstack's `show-me-your-work` skill records an append-only trail of decisions, not every tool call.

Fields include:

- timestamp;
- phase;
- decision;
- why;
- evidence pointer;
- result.

The evidence cell is a pointer to a commit, PR, artifact, trace, screenshot, or other proof rather than a prose transcript.

The skill defaults the trail to local/ephemeral and commits it only when the work is ambitious enough that a later reviewer needs it.

### HE relationship

This sharpens the run-retention contract.

> **For unattended work, retain the minimum decision/evidence trail needed for a successor to audit material forks; do not retain the full inference transcript by default.**

---

# 18. Skills should shrink as model capability absorbs implementation detail

Near the end of the interview Lauren says that skills increasingly encode **workflow** rather than low-level command recipes as models improve.

pstack's evolution is consistent with this direction:

- routing/playbook intent remains;
- deterministic mechanics move to scripts;
- environment checks encode constraints;
- model-specific routing is configurable rather than hard-coded into the conceptual workflow.

### HE relationship

This strongly reinforces:

- behavioral scaffolding retirement tests;
- model capability drift;
- portable role/workflow contracts;
- progressive disclosure.

### Durable implication

> **Preserve the workflow property; delete implementation-detail prompting when the model/runtime no longer needs it.**

---

# 19. Transcript mining is evidence for candidate improvements, not automatic policy

Lauren recommends reviewing prior transcripts for:

- repeated corrections;
- recurring interventions;
- working patterns;
- candidate lint rules;
- candidate skills.

pstack implements this through `automate-me`, `reflect`, and `recall`.

Importantly, the public skills contain useful safeguards:

- scope transcript reads to the active workspace;
- do not glob unrelated private projects;
- look for recurrence;
- cross-check signals;
- route structural enforcement before prose;
- require approval before broad skill mutation in some workflows.

### HE relationship

This strongly reinforces Experience-Derived Harness Evolution.

### Boundary

Past chat behavior is observational evidence, not self-validating ground truth.

The correct flow remains:

```text
observed correction
   ↓
classify / reproduce
   ↓
identify owning primitive
   ↓
smallest durable correction
   ↓
fresh verification
```

---

# 20. Implication for the Misfit Unit

This source is unusually relevant to the Misfit Unit because it gives several **anti-agent** rules from someone who runs many agents.

Before adding a Misfit, ask in this order:

1. Can the repeated work become a deterministic lever?
2. Can the environment make the wrong path impossible?
3. Can the agent retrieve the missing source context directly?
4. Can observations be buffered and clustered before creating more execution?
5. Does the task actually partition along independent seams?
6. Does a separate reviewer need genuine inference independence?
7. Only then: does a distinct specialist agent buy measurable value?

### Strong candidate roles supported by this source

- independent adversarial reviewer;
- coordinator for a genuinely multi-workstream mission;
- evidence/context investigator where external source acquisition is substantial.

### Weak candidate roles

- mechanical transformer;
- policy checker that could be lint/CI;
- context courier;
- one agent per incoming event;
- persona-only reviewer.

### Misfit admission refinement

> **A Misfit should exist because a separate inference context or authority boundary is valuable, not because another process can be automated.**

---

# 21. What HE should not copy

## Do not copy pstack wholesale

pstack is opinionated, Cursor-centered, and contains many interlocking skills, model-role assumptions, playbooks, and host-specific behaviors.

Its own ecosystem has public issues showing portability/routing edge cases when assumptions about the host differ.

Mine mechanisms, not the entire stack.

## Do not copy Noodle wholesale

Noodle is a real orchestration framework with:

- scheduler;
- work-order state;
- event log;
- adapters;
- runtimes;
- web UI;
- worktree lifecycle;
- merge queue;
- monitoring;
- persistence.

HE does not currently have evidence that it needs this infrastructure.

## Do not universalize autonomous merge

The interview's autonomous-merge posture is bounded by Lauren's environment and verifiability assumptions.

HE should not convert that into a default for:

- high-consequence work;
- production infrastructure;
- regulated workflows;
- hard-to-verify domains;
- work without reliable rollback.

## Do not universalize "never block on the human"

pstack's principle is explicitly scoped around reversible execution and keeps irreversible/product-direction actions human-owned.

HE should retain its consequence-based approval model.

---

# 22. Disposition

## STRONGLY REINFORCE

- verification before autonomy;
- deterministic mechanics / probabilistic judgment split;
- environment improvement from observed failures;
- source-connected context acquisition;
- worktree isolation;
- model/harness evaluation envelope;
- experience-derived skill evolution;
- progressive removal of stale scaffolding;
- fan-out only where useful.

## NEW DURABLE REFINEMENTS

1. **Deterministic lever before probabilistic fan-out.**
2. **Buffer observations when they may share a systemic cause before mutating locally.**
3. **Sampling can replace blocking review only inside a sufficiently reversible/verifiable envelope.**
4. **Durable work/order identity should outrank ephemeral session identity.**
5. **Event streams need an explicit loss/delivery contract.**
6. **Authority changes should invalidate stale in-flight effects created under the old authority.**
7. **Sparse decision/evidence trails can make unattended work auditable without retaining full transcripts.**
8. **Eliminate the human as a context courier before eliminating the human as decision authority.**

## ASSESS

- authority-epoch pattern for future asynchronous orchestration;
- sampled review for low-consequence/highly verifiable autonomous work;
- buffered recurring-failure discovery;
- cross-model review as one independence technique;
- order-centric coordinator state for the Misfit Unit if it grows beyond a few missions;
- project-local verification skills.

## PARK

- Noodle-style full orchestrator;
- always-on outer-loop connectors;
- autonomous merge;
- hundreds-agent cloud scaling;
- dedicated event bus;
- persistent scheduler agents.

## REJECT AS DEFAULT

- one worker per incoming event;
- persona diversity as independent review;
- rules/docs for failures a stronger structural owner can prevent;
- polling every worker at scale when trustworthy completion events exist;
- using PR count as the success metric;
- adopting pstack/Noodle architecture because the reported throughput sounds impressive.

---

# Bottom line

The most important lesson from Lauren Tan's public work is not "run hundreds of agents."

It is:

> **High agent throughput is downstream of a highly engineered environment: deterministic mechanics, strong verification, direct source access, isolation, explicit authority, and continuous structural correction. Parallelism is the consequence of trustable machinery, not the starting strategy.**

For HE, the strongest new additions are the deterministic-lever test, buffered systemic diagnosis, reversibility/verifiability as the ceiling on sampled supervision, durable order identity above session identity, explicit event-loss semantics, and stale-authority invalidation for asynchronous effects.
