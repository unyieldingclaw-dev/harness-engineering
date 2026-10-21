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


---

# 23. 2026-10-21 follow-up: pstack is changing fast enough that source snapshots matter

A second user-supplied walkthrough, Rob Shocks' **Pstack Is Agent Overkill. Use It Anyway!**, was checked against the live public pstack source rather than treated as current documentation.

Secondary source:

- https://www.youtube.com/watch?v=lUhXa8GiXns

At the inspected `cursor/plugins` HEAD `190d28736eff128ec94f502a22c7fe0c4b56cd37`:

- pstack reports version `0.15.29`;
- the official README lists **23 playbooks**;
- the official README lists **24 principles**;
- current source includes `/tdd`, `/recall`, `/interrogate`, app-verification generators/maintenance, and beta formal-verification generators/maintenance;
- the same repository now contains a generated **Claude Code adapter** under `harness/pstack/claude-code/`.

Some screenshots and secondary walkthrough counts therefore describe earlier snapshots.

### HE implication

> **For fast-moving skill libraries, cite the repository commit/ref and inspect the current implementation before promoting a video description into architecture.**

---

# 24. TDD in current pstack is narrower than "always test first"

Current `/tdd` is explicitly conditional.

It is intended when:

- the user explicitly asks for TDD / a regression test; or
- a bug has an obvious, cheap local executable test path.

It explicitly says to skip forcing a new test when the path is:

- unclear;
- expensive;
- integration-heavy;
- brittle;
- dependent on production-only state;
- or requires broad unrelated fixture/harness setup.

The key evidence contract is:

```text
intended behavior understood
      ↓
smallest executable regression check
      ↓
failing-before evidence for the intended reason
      ↓
smallest production fix
      ↓
passing-after evidence
```

If a durable failing test is impractical, the skill allows the nearest useful executable regression check:

- script;
- manual reproduction command;
- browser automation;
- snapshot comparison;
- log assertion;
- focused integration check.

It explicitly prefers **no new test over a bad test**.

### HE refinement

> **TDD is valuable when it cheaply turns the defect into executable evidence; it should not become a ritual that manufactures brittle tests merely to satisfy process.**

The test's red state is evidence only when it fails for the intended reason.

---

# 25. Test quality needs both sensitivity and refactor tolerance

pstack's `principle-test-behavior-not-implementation` gives a compact two-sided oracle test.

Imagine two changes:

1. **Break the externally observable behavior.**
   - the test should fail.
2. **Restructure internals while preserving the externally observable behavior.**
   - the test should continue to pass.

This distinguishes a useful behavioral contract from an implementation-pinned test.

The principle also requires expected results to come from the requirement or an independent source of truth, not from the same algorithm under test.

### Transferable principle

> **A useful regression test should be sensitive to the behavior it protects and insensitive to implementation changes that preserve that behavior.**

This sharpens HE's existing distinction between test existence and test/oracle quality.

---

# 26. A new or changed verifier should demonstrate that it can fail

Current pstack goes beyond ordinary red/green TDD in both app verification and formal verification.

The formal-verification generator requires the check to catch a deliberately broken variant before trusting a pass.

The formal-maintenance skill repeats that calibration after the model/property is updated: if the final check passes, deliberately break the guarded property and prove the updated verifier still detects it.

This is highly compatible with HE's existing verifier-calibration and mutation-testing research.

### Durable refinement

> **For consequence-relevant verifiers, a green result is stronger when the verifier has demonstrated sensitivity to a representative known-bad case under the current harness.**

This does **not** imply mutation testing or formal proof for every unit test.

Use this level of calibration where a false green would materially mislead acceptance.

---

# 27. Formal verification is a targeted extension, not a new default testing policy

pstack now ships beta:

- `/create-formal-verification-skill`
- `/maintain-formal-verification-skill`

The generator targets promises where:

- failure is consequential; and
- ordinary tests are unlikely to reach the important state space.

Examples named by the skill include:

- concurrency/orderings;
- retries;
- state machines;
- parsers;
- unsafe code.

The tool is chosen by the question, not by a universal preference:

- event-order reasoning commonly defaults to TLA+;
- code-level invariants may use a verifier operating on the implementation;
- when no suitable verifier exists for the language, the current skill says to exercise the **real code with generated inputs**, not verify a hand-written copy and confuse that with implementation proof.

The skill requires:

- code-to-model traceability;
- explicit bounds/assumptions;
- a reproducible command/tool version;
- a deliberate broken variant proving the verifier can fail;
- covered code/functions;
- a sync commit recording the code version the verification describes.

The maintenance skill treats a stale passing model as a dangerous failure mode and distinguishes:

```text
verification drift
      from
product bug
```

### Public eval evidence and limit

PR #716 reports a preregistered 54-run maintenance-skill evaluation. After correcting scoring artifacts, it reports **no material correctness gain** across the tested cases; the stronger measured effect was process/evidence hygiene:

- updated checks were much more consistently shown to still be able to fail;
- drift vs bug was explicitly separated;
- sync-commit metadata was maintained more reliably.

PR #721 provides a more concrete failure-mode result for tool choice: on one JavaScript case, the older instruction caused 6/9 runs to prove a claim only on a hand-written copy of the function; revised wording led all 9 tested runs to check the shipped code.

### HE disposition

**ASSESS selectively.**

Formal verification is promising where the state space and consequence justify it. The current public evidence does not justify making it a universal HE/PMB requirement.

---

# 28. Recall, session pickup, and handoff are different operations

Current pstack explicitly separates:

## Recall

Consumer-side **reconstruction**.

It mines:

- recent in-scope agent transcripts;
- shared records such as source control, issue trackers, team chat, long-form docs, error tracking;
- live Git/PR/ticket state;

then returns a compact current-state brief.

## Session pickup

Consumer-side **continuation from one known prior execution trail**.

It reconstructs:

- branch/worktree;
- landed vs pending work;
- decisions;
- next execution point;

and verifies inherited claims against the real artifact.

## PMB-style handoff

Producer-side **state transfer at a known boundary**.

PMB's current design intentionally keeps durable project truth in Memory Bank and uses `handoff.md` only for narrow ephemeral in-flight state that the successor cannot recover naturally.

### HE refinement

```text
known transition, predecessor available
        → HANDOFF

known prior trail to continue
        → PICKUP

no trustworthy handoff / many historical threads
        → RECALL / RECONSTRUCTION
```

> **Do not make expensive historical reconstruction the normal path when authoritative durable state and a narrow handoff already exist.**

Recall is a useful fallback/recovery/search capability; it is not evidence that transcript history should become the primary memory system.

---

# 29. Interrogate overlaps code review but is not an acceptance gate

Current `/interrogate`:

- packages the change and stated intent;
- dispatches multiple read-only reviewers;
- uses different configured model families rather than invented reviewer personas;
- gives each the same rubric/context;
- synthesizes agreement and disagreement;
- places findings into Act on / Consider / Noted / Dismissed;
- does **not** auto-apply fixes.

This is a useful adversarial review mechanism.

However it is not equivalent to PMB's current `/code-review` contract, which also includes:

- deterministic baseline-health checks before model review;
- separate review domains;
- an independently pinned Opposition reviewer that attacks premises;
- explicit finding/Blocking semantics;
- a marker tied to the exact reviewed diff;
- acceptance/gating behavior.

### HE implication

> **Multi-model adversarial review is one assurance mechanism. It should not silently replace deterministic gates, domain ownership, or explicit acceptance authority.**

For PMB, `interrogate` is best treated as comparison evidence for a possible cross-model challenge layer, not as a replacement for the existing review gate.

---

# 30. Official pstack now demonstrates a useful portability pattern

The secondary walkthrough showed an unofficial `pstack-claude` port. Current upstream evidence is stronger: `cursor/plugins` itself now publishes a generated Claude Code tree under:

```text
harness/pstack/claude-code/
```

The upstream README says the generated harness tree is regenerated from canonical pstack and CI fails if the generated copy is stale.

Conceptually:

```text
canonical workflow / skill source
          ↓
host adapter generation
          ↓
Claude Code representation
          ↓
drift check in CI
```

This is a useful portability mechanism because host differences are made explicit without requiring the conceptual workflow to be maintained independently in every harness.

### Durable refinement

> **When one workflow must ship across harnesses with different primitives, prefer one canonical semantic source plus explicit generated/maintained adapters and drift checks over independently edited copies.**

This pattern should be evaluated against real host differences; code generation alone does not prove behavioral equivalence.

---

# 31. Current X profile provided additional primary-practitioner context

Public unauthenticated fetching of X returned HTTP 403 in the initial research pass.

Nolan subsequently opened `https://x.com/poteto` in an authenticated browser and the page was readable through the connected browser context.

Visible 2026-10-18 material corroborates that Lauren publicly announced:

- pstack v0.15.27;
- beta `/create-formal-verification-skill`;
- a meta-skill that generates project-local formal verification.

The current profile also links pstack as part of Lauren's current work context.

A visible 2026-10-16 post begins with the same trust-building themes found in the interview/public repository: stronger architecture, types/invariants, and clear boundaries.

### Evidence boundary

Only text actually visible in the loaded profile is treated as observed. Truncated "Show more" text is not reconstructed or quoted as if fetched in full.

---

# 32. MoltenBase is ecosystem prior art for skill governance, not Lauren's system

Rob Shocks' video demonstrates an early-access product called **MoltenBase**:

- https://www.moltenbase.com/

It is a Switch Dimension product, not part of Lauren Tan's pstack repository.

The current early-access site describes:

- invocation counts and last-used timestamps;
- never-called skills;
- cross-harness inventory;
- single source of truth;
- per-agent targets;
- duplicate/stale detection;
- approved skill sets;
- team rollout;
- local-first/plain-file storage.

The site's metrics/dashboard are explicitly presented as an illustrative early-access view; they are not evidence of Nolan's or pstack's actual usage.

### HE relationship

MoltenBase is concrete product prior art for a lifecycle HE already favors:

```text
CREATE
  ↓
USE
  ↓
MEASURE
  ↓
CONSOLIDATE
  ↓
RETIRE
```

It reinforces the value of knowing whether skills are actually invoked before treating a large skill library as useful.

### Boundary

Do not build a skill dashboard merely because a product exists.

Usage count alone cannot establish skill value. A rarely invoked safety/recovery skill may be valuable; a frequently invoked skill may still add cost without improving outcomes.

> **Skill telemetry is useful when it informs admission, drift, consolidation or retirement decisions; invocation count is not a quality score.**

---

# 33. PMB-specific testing-policy implication — research candidate, not implemented

Current PMB already has unusually strong testing/review machinery, but its policy is stricter than current pstack TDD in one important way: PMB's workflow currently states a universal implementation-phase rule that production code should not precede a failing test.

The pstack evidence suggests a more precise version worth evaluating:

```text
BUG / BEHAVIOR CHANGE
      ↓
cheap, stable, acceptance-relevant executable regression path?
      ├── yes → require failing-before evidence for the intended reason
      │         then minimal fix and passing-after evidence
      │
      └── no  → use the nearest acceptance-relevant executable check,
                state why a durable failing test is impractical,
                and do not manufacture a brittle test merely for ritual compliance
```

PMB should retain its existing strengths:

- public/seam behavior over implementation detail;
- independent expected values;
- cross-shell parity;
- exact regression cases;
- deterministic baseline checks;
- opposition review;
- real throwaway-clone execution for destructive gate tests.

The stronger candidate hardening is **oracle quality**, not simply more test files:

1. Can the check observe the behavior actually at risk?
2. Did failing-before fail for the intended reason?
3. Would the test fail on a representative broken behavior?
4. Does it survive an internal refactor that preserves the contract?
5. Is the expected result independent of the code under test?
6. For high-consequence gates, has the verifier demonstrated that it can fail under the current harness?

PMB's current `/test-audit` remains useful as a cheap structural inventory, but file-to-test mapping should not be confused with proof that the tests have a meaningful oracle.

### Current disposition

**ASSESS before modifying PMB policy.**

No pstack installation, TDD-policy rewrite, or formal-verification requirement is authorized by this source alone.
