# Beyond Coding — Carlos Kelkboom, Planning Scaffolds & Iterative Agent Engineering — 2026-10-15

## Source

- Video: **Lead Architect Reveals His Rules for Coding Agents**
- Channel/podcast: Beyond Coding / Patrick Akil
- Guest: Carlos Kelkboom
- Published: 2026-10-14
- URL: https://www.youtube.com/watch?v=mkN4uRZYPfs
- Primary working evidence for this note: user-supplied transcript captured 2026-10-15 (1,312 lines).
- External corroboration checked: SlopCodeBench paper and public runner/problem repositories.

This is a source/deep-dive note. It is **not** an instruction to adopt Carlos's full workflow, invent a new orchestration layer, mandate deep scaffolding for every task, or treat his speculative programming-language views as HE architecture.

---

# Executive synthesis

The strongest HE contribution is not "agents are better at one-shot coding."

It is:

> **Planning should explicitly decide which structural choices are too path-dependent to let an implementation agent invent accidentally.**

Carlos's workflow front-loads selected structure before implementation:

- directory/project/module boundaries;
- comments describing responsibilities;
- class/function names or signatures;
- important data/type shapes;
- representative examples;
- diagrams and dependency direction;
- runtime/deployment assumptions.

The implementation agent then gets freedom **inside** that scaffold.

This is useful because the expensive failure is not merely wrong syntax or a failing test. It is a locally plausible implementation that satisfies the immediate request while establishing a structure that becomes increasingly expensive to extend.

A second durable contribution is that scaffold depth should depend on intent:

- disposable experiment / learning spike → little structural investment;
- durable feature or subsystem → more explicit boundaries;
- long-lived/high-consequence work → executable architectural constraints where practical.

This avoids turning scaffolding into architecture ceremony.

---

# 1. Source evidence: what Carlos actually scaffolds

The transcript is unusually concrete.

At lines 219-241 Carlos says he creates files and directory structure himself, adds comments, sometimes names classes/functions or supplies signatures, and for compiler work sketches folders, lexer structure, intermediate-representation flow, language specification and examples before asking the agent to implement.

At lines 731-755 he describes a deeper version:

- generate projects/crates;
- name projects;
- place Markdown inside projects explaining boundaries;
- sketch a diagram of responsibility placement;
- create folder structure;
- create expected core files;
- sometimes define a key function/type signature such as the lexer/token representation.

The point is not that these exact artifacts are universally required.

The point is:

> **The planner deliberately fixes selected structural degrees of freedom before high-volume generation begins.**

**HE disposition: MINE.**

---

# 2. Why he does it: immediate-task optimization creates path dependence

Carlos reports recurring failure modes when agents are allowed to invent structure while solving only the current request:

- duplication;
- weak cohesion;
- God objects;
- replacement/duplication of existing scripts;
- multiple implementations of the same concept;
- architecture that becomes hard to recognize or reason about.

The strongest anecdote is at lines 328-338: after a month of iterating on a compiler in a language he did not understand well, a later review reported that one list feature had effectively been implemented five times.

This is anecdotal evidence, not a benchmark.

But it describes a failure mode that aligns with HE's broader concern: **local correctness can coexist with accumulating structural debt.**

**HE disposition: MINE as mechanism; do not treat anecdote as measured prevalence.**

---

# 3. External corroboration: SlopCodeBench supports the problem, not Carlos's exact solution

The video references SlopCodeBench.

The 2026 paper evaluates coding agents across repeated specification extensions rather than only one-shot complete tasks. The published abstract reports:

- no tested agent completed an entire problem end-to-end;
- highest checkpoint solve rate 17.2% in the reported paper setup;
- structural erosion increased in 80% of trajectories;
- verbosity increased in 89.8%;
- a prompting intervention improved initial quality but did not stop degradation.

The public runner describes the benchmark as measuring path dependence, non-convergence and structural stability under iterative specification refinement.

Important limitation:

- the public runner itself calls SCBench an initial/open evaluation primitive rather than a finalized benchmark;
- an independent 2026 audit raises construct, selection, oracle and grader concerns.

Therefore HE should use SlopCodeBench as **corroborating evidence that iterative extension quality is a real evaluation dimension**, not as proof that one-shot generation or Carlos's specific scaffolding depth is optimal.

### Durable implication

> **An implementation can pass the current checkpoint while degrading the cost and reliability of the next change. Planning and evaluation therefore need an extension-horizon view, not only current-task correctness.**

---

# 4. New HE candidate: the Scaffold Contract

The useful abstraction is a compact planning artifact, not necessarily new files or code.

A **Scaffold Contract** identifies structural decisions that should be fixed before implementation because accidental invention would create expensive path dependence.

Candidate fields:

~~~text
Scaffold Contract
  ├─ ownership / responsibility boundaries
  ├─ dependency direction
  ├─ core domain/data shapes
  ├─ important public interfaces
  ├─ integration seams
  ├─ runtime/deployment constraints
  ├─ representative usage examples
  ├─ executable invariants where cheap
  └─ known-plausible extension pressures
~~~

Not every task needs every field.

The contract may live inside an already-owned design/spec/implementation-plan artifact. HE should **not** create a competing scaffold file merely because this source uses scaffolding language.

### Durable implication

> **Planning should identify the few structural decisions whose accidental invention would make later change expensive, then constrain implementation around those decisions.**

---

# 5. Scaffold depth should be selected, not standardized

The transcript explicitly distinguishes quick outcomes from work intended to go "long and far" (lines 699-710).

That yields a useful non-bureaucratic scale:

| Work intent | Minimum useful scaffold |
|---|---|
| Spike / throwaway experiment | objective, minimal interface/boundary, explicit permission to discard |
| Normal feature | ownership, interfaces/types, dependency direction, representative verification |
| Durable subsystem | above + data contracts, integration/deployment seams, likely extension points |
| Cross-cutting / high-consequence | above + invariants, migration/rollback, security/observability boundaries, architecture checks where justified |

These are **examples, not mandatory HE stages**.

The determining question is:

> **How expensive would the wrong structural choice be to discover after the implementation agent has generated a large amount of code?**

### Durable implication

> **Scaffold depth should scale with expected lifetime, change pressure, consequence and cost of structural reversal.**

---

# 6. New planning maneuver: extension-pressure test

Before implementation begins, challenge the proposed scaffold with a small number of changes that are already plausible from known requirements or roadmap evidence.

Example:

~~~text
Current requirement:
  one reviewer/provider

Known-plausible extension:
  local + hosted reviewers are already expected

Pressure question:
  can reviewer #2 fit behind the same contract,
  or would routing/calibration logic be duplicated across the system?
~~~

If the answer exposes obvious structural friction, change the scaffold **before** high-volume implementation.

This is not permission to design for imagined futures.

Rules:

- use one to three evidence-backed likely changes;
- do not invent generalized abstractions solely to pass hypothetical scenarios;
- if no plausible extension pressure is known, skip the test.

### Durable implication

> **Pressure-test the scaffold against known-plausible change, not speculative universality.**

---

# 7. Logical scaffold and physical scaffold are different artifacts

A useful separation is:

## Logical scaffold

Planning/design representation:

- module/folder responsibility;
- interfaces;
- types;
- dependency arrows;
- examples;
- invariants;
- runtime constraints.

This should be reviewable before implementation.

## Physical scaffold

Optional materialization in an isolated worktree:

- folders/projects;
- interface/type declarations;
- empty adapters/modules;
- selected signatures;
- architecture/dependency tests;
- representative test skeletons.

For durable work, a worker can materialize the scaffold first, run cheap deterministic checks, then stop.

Only after the scaffold is accepted should substantive implementation begin.

### Scaffold Gate

> **Does the materialized structure match the accepted scaffold contract closely enough that the implementation agent is filling an agreed structure rather than silently inventing a new one?**

This should remain lightweight. A spike may skip the physical scaffold entirely.

---

# 8. Convert stable scaffold rules into deterministic constraints when justified

Later in the interview (lines 1147-1155), Carlos describes trying to move failures from nondeterministic instruction into deterministic structure: a script, harder scaffold, or other repeatable constraint.

This aligns strongly with existing HE evidence:

~~~text
semantic design decision
       ↓
repeated proven requirement
       ↓
cheapest enforceable mechanism

type / schema / compiler check
dependency rule
architecture test
lint/static analysis
contract test
CI guard
~~~

### Durable implication

> **When a scaffold rule becomes stable and mechanically checkable, prefer executable enforcement over repeatedly asking a model to remember it.**

Do not mechanize taste or uncertain design judgment prematurely.

---

# 9. Worktrees make discard cheap enough to improve the learning loop

Carlos uses isolated worktrees for large generated changes and discards work that no longer resembles the scaffold rather than endlessly patching it (lines 1101-1145).

His recovery loop is particularly HE-compatible:

~~~text
implementation diverges
        ↓
discard isolated attempt
        ↓
fresh context
        ↓
ask what failed:
  scaffold?
  conflicting ideas?
  instruction/context?
        ↓
correct owning component
        ↓
rerun
~~~

This matches existing HE failure-learning principles.

### Important refinement

"Code I don't understand" is a personal heuristic, not an acceptance metric.

HE should translate it into:

> **If implementation materially violates the accepted scaffold or becomes too opaque to verify economically, prefer rejecting the isolated attempt and correcting the owning planning/context component over accumulating patches on an untrusted trajectory.**

---

# 10. Parallel worktrees: useful pattern, not default architecture

Carlos describes three branches/worktrees implemented independently and a fourth agent comparing them (lines 716-724).

This reinforces existing HE bakeoff/fan-out evidence:

~~~text
same accepted scaffold / acceptance target
       ↓
independent attempts
       ↓
same verification envelope
       ↓
evidence-preserving comparison
       ↓
select / combine only with explicit ownership
~~~

Do not infer that every feature should produce three implementations.

Use fan-out where uncertainty or expected upside justifies the cost.

---

# 11. Main conversational agent as non-mutating planner

Carlos's main agent is intentionally prevented from changing files. It converses, teaches/learns, retrieves context and only later delegates implementation (lines 1007-1058).

The valuable mechanism is not the literal "main agent must never touch files" rule.

It is:

> **Planning/clarification and mutation can have different authority surfaces.**

This protects the reasoning phase from an implementation-biased agent that tries to turn every conversation into code.

HE already has bounded authority and supervised execution concepts; this source adds practical evidence that a non-mutating planning mode can improve focus.

### Reject as universal rules

- a fixed 90% confidence threshold before delegation;
- "main agent never touches files" for every workflow;
- twelve-hour delegated runs as a default.

Those are Carlos's implementation choices, not independently validated HE constants.

---

# 12. Distributed graph/context approach: mostly reinforcement

Carlos describes a distributed knowledge/context graph built from:

- Markdown nodes;
- front-matter metadata;
- keyword/indexed search;
- paths/hashes/URLs as edges;
- ranking before deeper retrieval;
- diffs/commits before full-code reads;
- progressive disclosure rather than loading all knowledge.

This strongly reinforces existing HE work on:

- progressive disclosure;
- derived indexes;
- structural retrieval;
- source-addressable evidence;
- current diffs as high-value context.

It does **not** justify a graph database or a new HE knowledge subsystem.

**Disposition: REINFORCE ONLY.**

---

# 13. Review should test scaffold survival, not only line correctness

Near the end of the interview Carlos argues that review/evaluation should inspect whether the original scaffold is still standing as code evolves, not only whether current unit tests pass.

That aligns with the SlopCodeBench problem statement and HE's existing architecture-drift concerns.

Potential evidence:

- forbidden dependency detection;
- module ownership violations;
- duplicated domain implementations;
- complexity concentration;
- architecture-test failures;
- scaffold-to-implementation traceability;
- mutation/verifier calibration for critical invariants.

### Durable implication

> **For long-lived agent-built systems, verification should include whether accepted structural invariants survived the change, not only whether behavior currently passes.**

---

# 14. What should NOT be mined

## REJECT / PARK

- "Agents are bad at changes and good at one-shot" as a universal law.
- Maximizing task size merely because one-shot generation can work well.
- Fixed 90% model-confidence gating without calibration evidence.
- Deep scaffolding for disposable experiments.
- Treating line-by-line human familiarity as the only valid verification method.
- Building a graph database because Carlos uses graph language.
- His proposal to remove heap allocation/dynamic memory from future languages as an HE design principle.
- Treating Rust opinions as harness guidance.
- Treating reduced human collaboration as inevitable.
- Using token-credit FOMO as a reason to launch work.

---

# 15. HE integration recommendation

## MINE into canonical planning/eval owner

Add a planning-time **Scaffold Contract** concept to the artifact-gated SDLC research:

~~~text
problem/objective
      ↓
evidence + constraints
      ↓
architecture decisions
      ↓
scaffold contract
      ↓
small extension-pressure test
      ↓
optional physical scaffold
      ↓
scaffold gate
      ↓
implementation decomposition
      ↓
implementation
      ↓
behavior + scaffold-survival verification
~~~

## REINFORCE existing owners

- Bounded Execution: planning vs mutation authority can differ.
- Context Engineering / Derived Index: distributed progressive retrieval and diff-first context.
- Fan-out/Fan-in: worktree bakeoffs.
- Failure learning: reject bad isolated attempts and correct the owning scaffold/context component.
- Artifact-Gated Eval: architecture/scaffold survival as consequence-relevant evidence.

No new orchestrator, graph subsystem, or mandatory planning file is justified.

---

# Candidate HE principles — research status

> **Front-load the few structural decisions whose accidental invention would make future change expensive; let agents generate freely inside those boundaries.**

> **Scaffold depth should scale with lifetime, change pressure, consequence and reversibility—not with enthusiasm for architecture.**

> **Pressure-test a scaffold against known-plausible change before high-volume implementation, but do not generalize for hypothetical futures.**

> **When a stable scaffold rule is mechanically checkable, move it from prose into deterministic enforcement.**

> **A current-task pass does not prove extension fitness; long-lived systems need evidence that accepted structure survives change.**

---

# Bottom line

Carlos's strongest contribution to HE is a practical answer to a planning question:

> **What should be decided before the coding agent starts?**

Not everything.

Only the structural choices whose accidental invention would create expensive path dependence later.

That turns "plan first" into a more actionable workflow:

**decide the high-cost structural freedoms → pressure-test them → optionally materialize the scaffold → verify it → then let the implementation agents move fast inside the accepted shape.**
