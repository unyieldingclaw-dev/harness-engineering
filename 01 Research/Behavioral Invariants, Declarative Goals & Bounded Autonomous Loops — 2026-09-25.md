# Behavioral Invariants, Declarative Goals & Bounded Autonomous Loops — 2026-09-25

## Purpose

Synthesize durable Harness Engineering implications from the Karpathy-inspired behavioral-guideline research and Andrej Karpathy's actual `autoresearch`, `nanochat`, and minimalist repository designs.

Primary evidence:

- `01 Research/Sources/Eric Tech & Karpathy-Inspired Guidelines — Behavioral Guardrails and Autoresearch — 2026-09-25.md`
- `multica-ai/andrej-karpathy-skills` at `2c606141936f1eeef17fa3043a72095b4765b9c2`
- `karpathy/autoresearch` at `228791fb499afffb54b46200aca536f79142f117`
- `karpathy/nanochat` at `92d63d4e8bb4df75c3b71618f31ddde2378b2bcd`
- `karpathy/llama2.c` at `350e04fe35433e6d2941dce5a1f53308f87058eb`

Related HE research:

- `00 Overview/Harness Engineering Philosophy.md.md`
- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`

This is research synthesis only. It does not authorize modifying project instruction files or adding autonomous experiment loops.

---

# Executive synthesis

Two apparently different ideas converge:

1. a small always-on behavioral kernel that constrains *how* an agent approaches work;
2. a highly autonomous execution loop whose safety comes from a narrow mutation surface and strong external acceptance function.

The combined architecture is:

```text
small stable behavioral invariants
        +
clear declarative objective
        +
explicit authority/mutation boundary
        +
resource budget
        +
independent/verifiable acceptance criteria
                ↓
        model chooses tactics
                ↓
       observe measurable result
                ↓
          keep / reject / revert
```

The central finding is:

> **The strongest harness does not necessarily prescribe more steps. It can instead make goals, boundaries and evidence precise enough that the model needs less procedural micromanagement.**

---

# 1. Always-on context should be small and invariant-like

Always-loaded instructions have a special cost because they compete with every task for attention/context and can interact with every procedure.

The best candidates for always-on guidance are stable properties that should hold across many tasks, for example:

```text
surface material ambiguity rather than silently inventing it
prefer the simplest sufficient solution
stay inside authorized scope
establish observable completion evidence
```

These differ from procedures such as:

```text
how to debug
how to perform a release
how to run a security review
how to create a worktree
how to investigate an incident
```

Procedures are better progressively disclosed when relevant.

### Candidate principle

> **Always-on context should encode stable behavioral invariants, not full workflows.**

This strengthens Progressive Disclosure.

---

# 2. Invariants still need behavioral evidence

“Broadly useful” is not enough to justify permanent startup context.

A behavioral invariant can impose costs:

- unnecessary clarifying questions;
- ritual planning text;
- slower trivial tasks;
- conflicts with project-specific instructions;
- duplicated guidance already enforced by tests/tools;
- reduced autonomy when inspection could resolve uncertainty.

Therefore the invariant kernel should itself be subject to evaluation and ablation.

Potential measures:

- unrelated changed lines;
- user corrections caused by unstated assumptions;
- speculative features/abstractions introduced;
- acceptance criteria satisfied/missed;
- unnecessary user interruptions;
- token/context/latency overhead;
- conflicts/duplication with task-local Skills.

### Candidate principle

> **An always-on rule should earn permanent context through recurring need or evaluated behavioral effect.**

This connects Continuous Improvement with Experience-Derived Harness Evolution.

---

# 3. Declarative goals can replace procedural choreography

When the harness can specify:

```text
objective
allowed scope
constraints
success criteria
verification
```

it can often leave implementation tactics to the model.

This reduces brittle workflow scaffolding and allows model capability improvements to matter without rewriting the harness every generation.

### Candidate principle

> **Prefer declarative goals with verifiable postconditions over procedural micromanagement when the execution envelope is sufficiently bounded.**

This is consistent with HE's existing adaptive-autonomy finding:

> reduce procedural prescription before reducing authority or acceptance constraints.

---

# 4. Autonomy is enabled by boundaries, not by removing them

`autoresearch` is a useful counterexample to the idea that high autonomy means broad permissions.

The agent may experiment continuously, but only inside a deliberately narrow envelope:

```text
mutable:      one experiment file
immutable:    evaluation/data harness
branch:       isolated
budget:       fixed
metric:       explicit
baseline:     required
outcome:      keep / discard / crash
rollback:     built into loop
```

This structure reduces the need for human micro-approval because the model cannot redefine the acceptance mechanism or mutate arbitrary surfaces.

### Candidate principle

> **Autonomy grows safely when the mutable surface narrows and acceptance becomes more objective.**

This is a stronger framing than either “loosen the leash” or “add more guardrails.”

The right optimization is to move control toward the boundaries that actually determine risk.

---

# 5. Separate policy, execution and evaluation authorities

The `autoresearch` design implicitly separates three roles:

```text
program.md     human-owned policy/process
train.py       agent-owned experimental mutation
prepare.py     fixed evaluation authority
```

HE should generalize the separation rather than the filenames.

For consequential autonomous loops:

```text
policy/control plane
       ≠
mutable execution object
       ≠
acceptance/evaluation authority
```

These roles can be implemented by the same software process in low-risk cases, but their ownership should remain conceptually explicit.

### Candidate principle

> **The producer should not be able to silently redefine the evidence that declares its own work successful.**

This aligns with HE's producer/verifier/acceptance distinction and ACR evidence work.

---

# 6. Baseline-first turns improvement into a comparative claim

Without a baseline, “better” often means “looks plausible after the change.”

For optimization/evolution loops:

```text
baseline
   ↓
candidate mutation
   ↓
same acceptance path
   ↓
comparison
```

This pattern applies well beyond model training:

- harness rules;
- prompt changes;
- Skills;
- retrieval mechanisms;
- context layers;
- model/routing changes;
- review strategies;
- tool steering.

### Candidate principle

> **A system-change experiment should establish the comparison baseline before the candidate can claim improvement.**

This reinforces recent Graphify and harness-ablation research.

---

# 7. Simplicity is a lifecycle property, not LOC worship

The Karpathy-inspired rule says “minimum code that solves the problem.” Karpathy's own projects repeatedly choose narrow, understandable implementations and avoid general frameworks when the generality is not the point.

HE should preserve the underlying economic idea rather than literal line-count heuristics.

Complexity costs include:

- context required to understand the system;
- configuration surface;
- number of routing choices;
- dependency count;
- test matrix;
- maintenance/update burden;
- hidden ownership boundaries;
- integration failure modes;
- human review burden.

A more complex design may still win when it buys measurable capability or safety.

### Candidate principle

> **Cognitive complexity is a harness cost.**

> **Simplicity is an acceptance tradeoff, not a line-count target.**

---

# 8. Surgical scope improves causal evidence

Surgical changes are not only a code-review preference.

They improve the ability to answer:

> Did this change cause the observed result?

A wide diff introduces more possible causes, unrelated regressions and review noise.

Therefore narrow mutation is valuable for:

- production changes;
- behavioral evals;
- harness experiments;
- ablations;
- configuration tests.

### Candidate principle

> **Keep experimental and corrective mutation as narrow as practical so outcome attribution remains meaningful.**

This connects surgical coding practice directly to Evidence Before Architecture.

---

# 9. Goal-driven execution should not become plan theater

A task can be goal-driven without requiring a long plan artifact.

For a small deterministic fix, a sufficient contract may be:

```text
reproduce failure
apply scoped fix
original failure passes
regression suite passes
```

For a consequential multi-step change, explicit intermediate checks may be warranted.

The harness should scale planning detail with uncertainty, consequence and coupling.

### Candidate principle

> **Require enough planning to expose risk and verification, not planning prose for its own sake.**

This keeps the guideline compatible with adaptive autonomy.

---

# 10. Experience-derived mining can decide whether invariants are needed

The recently identified Harness Miner pattern creates a natural evaluation loop.

Rather than globally installing behavior rules because they are popular, mine real sessions for recurring classes such as:

```text
silent assumption corrections
scope creep / drive-by changes
premature abstraction
unnecessary configurability
weak completion claims
missing reproduction before fixes
```

Then test a compact invariant against representative cases.

If it helps, retain it.
If it does not, remove it.
If the failure has a stronger deterministic owner, fix that instead.

### Combined loop

```text
real sessions
   ↓
recurring failure class
   ↓
candidate invariant / owning correction
   ↓
baseline + treatment evaluation
   ↓
behavioral evidence
   ↓
keep / reject / move to stronger owner
```

This is a practical way to prevent generic best-practice advice from becoming permanent prompt scar tissue.

---

# 11. Relationship to HE principles

## Evidence Before Architecture

Baseline-first experiments and narrow mutation make improvement claims testable.

## Single Ownership

Policy, mutable project state, evaluation and durable memory should retain distinct owners.

## Progressive Disclosure

Keep broad invariants small; load procedures and references only when relevant.

## Continuous Improvement

Rules and workflows should be tested, simplified, or removed as model capability and host features change.

## Human Review Over Automation

Humans define/authorize the objective, legal mutation surface and consequential acceptance boundary. Autonomous loops can operate inside that envelope.

## Assessment Before Architecture

Mine actual failure patterns before adding new always-on guidance or workflow frameworks.

---

# 12. Cross-project implications

## PMB / work MB

No immediate change.

Potential experiment only if measured sessions show the relevant failure classes:

- compact behavioral-invariant block;
- compare against existing project guidance;
- remove duplicates;
- measure bad assumptions, scope creep, speculative complexity, completion quality and unnecessary clarification.

Do not paste the community `CLAUDE.md` wholesale.

## ACR

Possible later research:

- whether unrelated diff surface is a useful signal;
- whether review findings identify speculative/unnecessary architectural additions;
- whether candidate fixes stay traceable to the reported defect.

No immediate implementation recommendation.

## Harness Miner

High fit as an evidence source for deciding whether broad behavior guardrails are actually needed and whether existing ones still earn context.

## HE-002

This research is directly relevant to modular capability architecture:

- stable invariants can remain small/always-on;
- procedures can be modular/progressively disclosed;
- autonomous loops can remain narrow and task-specific;
- routing more frameworks into every task would undermine the intended separation.

---

# Research disposition

## STRONGLY REINFORCE

- small stable invariants versus large always-loaded workflows;
- declarative goals + objective postconditions;
- narrow mutable surfaces;
- fixed/independent acceptance mechanisms;
- baseline-first experiments;
- reversible keep/discard loops;
- cognitive complexity as a harness cost;
- surgical changes as causal/evidence support.

## ASSESS

- compact behavioral-invariant kernel using local session evidence;
- ablation of existing always-on rules;
- bounded autonomous loops in other domains with strong evaluators;
- policy/execution/evaluator separation as an explicit HE architecture pattern.

## PARK

- generic self-improving/autonomous harness framework;
- overnight autonomous loops where acceptance is subjective;
- framework stacks whose overlapping procedures have not been measured.

## REJECT

- popularity as proof that a rule belongs in startup context;
- broad permissions as the definition of autonomy;
- agents modifying their own acceptance criteria during the run;
- raw line count as complexity truth;
- planning ceremony for trivial work;
- untested combinations of overlapping Skills/frameworks.

---

# Candidate HE principles — research status

> **Always-on context should encode stable behavioral invariants, not full workflows.**

> **An always-on rule should earn permanent context through recurring need or evaluated behavioral effect.**

> **Prefer declarative goals with verifiable postconditions over procedural micromanagement when the execution envelope is sufficiently bounded.**

> **Autonomy grows safely when the mutable surface narrows and acceptance becomes more objective.**

> **The producer should not be able to silently redefine the evidence that declares its own work successful.**

> **A system-change experiment should establish the comparison baseline before the candidate can claim improvement.**

> **Cognitive complexity is a harness cost.**

> **Keep experimental and corrective mutation as narrow as practical so outcome attribution remains meaningful.**

These remain research candidates until corroborated by additional sources and/or local behavioral evidence.

---

# Bottom line

The four Karpathy-inspired guidelines are useful because they are compact behavioral invariants.

Karpathy's actual GitHub provides the deeper architecture:

> **Give the agent a clear goal, a small legal surface, an independent measurement function and reversible state. Then get out of the middle and let it work.**

That is a concrete bridge between HE's Progressive Disclosure, adaptive autonomy, evidence-driven evaluation and continuous simplification.
