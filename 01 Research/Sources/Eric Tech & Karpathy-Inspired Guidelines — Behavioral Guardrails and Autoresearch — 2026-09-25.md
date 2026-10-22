# Eric Tech & Karpathy-Inspired Guidelines — Behavioral Guardrails and Autoresearch — 2026-09-25

## Purpose

Mine Harness Engineering lessons from Eric Tech's video **“Karpathy's Skill Just Fixed Claude Code's Biggest Problem”**, the community `multica-ai/andrej-karpathy-skills` repository used by the video, and Andrej Karpathy's own GitHub projects where the underlying operating style can be inspected directly.

This note intentionally separates:

- what the video claims;
- what the community guideline repository actually contains;
- what is directly observable in Karpathy-owned repositories;
- HE interpretation.

No implementation is authorized by this research.

---

# Sources reviewed

## Video / transcript

- Eric Tech — `https://www.youtube.com/watch?v=EsgUfrwsV5A`
- User-supplied transcript reviewed 2026-09-25.

## Community guideline repository

`multica-ai/andrej-karpathy-skills`

Pinned commit:

`2c606141936f1eeef17fa3043a72095b4765b9c2`

Important files:

- `CLAUDE.md`
- `skills/karpathy-guidelines/SKILL.md`
- `EXAMPLES.md`

## Karpathy-owned repositories inspected

### `karpathy/autoresearch`

Pinned commit:

`228791fb499afffb54b46200aca536f79142f117`

Important files:

- `README.md`
- `program.md`

### `karpathy/nanochat`

Pinned commit:

`92d63d4e8bb4df75c3b71618f31ddde2378b2bcd`

### `karpathy/llama2.c`

Pinned commit:

`350e04fe35433e6d2941dce5a1f53308f87058eb`

### `karpathy/micrograd`

Pinned commit:

`7bc720e951fe422b8f8814aa5aa1b64121d26b4c`

---

# Attribution correction

The repository presented in the video as “Karpathy's skill” is **not an Andrej Karpathy repository**.

It is a community repository under `multica-ai`. Its Skill states that the guidelines are **derived from Andrej Karpathy's observations on LLM coding pitfalls**.

Therefore HE should preserve the attribution as:

> **Karpathy-inspired community guidelines**, not “Karpathy's CLAUDE.md” or an official Karpathy coding harness.

The distinction matters because popularity or attribution should not substitute for direct evidence of authorship or effectiveness.

**Disposition: REINFORCE source provenance.**

---

# 1. The four community guidelines

The community `CLAUDE.md` / Skill encodes four compact behavioral rules.

## Think Before Coding

Mechanism:

- surface assumptions;
- ask when uncertain;
- expose multiple interpretations rather than silently choosing;
- identify simpler alternatives;
- stop when essential ambiguity remains.

Useful HE interpretation:

This is an **ambiguity-management invariant**, not a requirement to produce ritual planning prose before every trivial edit.

A strong implementation should trigger extra clarification when ambiguity or consequence is material rather than forcing a verbose preamble for every task.

**Disposition: REINFORCE, with adaptive application.**

## Simplicity First

Mechanism:

- implement only requested behavior;
- avoid speculative features;
- avoid abstractions for one-off use;
- avoid unrequested flexibility/configurability;
- simplify when the implementation is disproportionately larger than the requirement.

The repository's examples emphasize timing: patterns or abstractions may be valid eventually, but premature structure adds cost before the need exists.

Useful HE interpretation:

> Complexity should have an observed owner and requirement.

Do not turn raw LOC counts into a quality metric. A 200-line implementation can be correct and necessary. The durable criterion is whether each architectural element earns its cost against a real requirement.

**Disposition: STRONGLY REINFORCE.**

## Surgical Changes

Mechanism:

- touch only what the requested change requires;
- avoid drive-by refactors, formatting changes, comment rewrites or style changes;
- match existing style;
- clean only new orphans caused by the current change;
- mention unrelated debt instead of silently changing it.

Useful HE interpretation:

This reduces:

- review surface;
- accidental side effects;
- merge conflict probability;
- attribution ambiguity;
- difficulty proving that a change caused an observed effect.

But this is not “never refactor.” If refactoring is necessary or explicitly requested, it becomes part of the authorized scope and should have its own evidence/acceptance criteria.

**Disposition: STRONGLY REINFORCE bounded mutation.**

## Goal-Driven Execution

Mechanism:

- convert vague requests into observable success criteria;
- reproduce bugs before fixing where practical;
- use tests/postconditions as completion evidence;
- for multi-step work, associate steps with verification.

Useful HE interpretation:

This is much more important than forcing a particular planning framework.

A sufficiently good success contract lets the model choose its own working path while preserving a strong acceptance boundary.

**Disposition: STRONGLY REINFORCE.**

---

# 2. Always-on invariant versus progressively disclosed procedure

The video usefully distinguishes the four behavioral rules from larger triggered workflows such as planning/debugging frameworks.

That distinction is valuable, but the video's proposal to combine multiple overlapping frameworks should not be adopted wholesale.

A cleaner HE separation is:

```text
always-on context
    = small, stable behavioral invariants

progressively disclosed capability
    = task-specific procedures, skills, tools and references
```

Candidate always-on material may include concepts such as:

- do not silently invent critical assumptions;
- prefer the simplest sufficient change;
- stay inside authorized scope;
- define/verify observable success.

Task-local procedures may include:

- brainstorming;
- debugging;
- TDD;
- security review;
- migration procedure;
- worktree workflow;
- release procedure.

### HE risk

If every invariant points to several framework Skills, routing ambiguity and duplicate procedure can replace the original problem with more harness overhead.

**Disposition:**

- **REINFORCE:** distinguish invariants from procedures.
- **ASSESS:** whether a compact invariant kernel measurably improves real tasks.
- **REJECT:** stacking overlapping workflow frameworks merely because each is individually useful.

---

# 3. Karpathy's actual GitHub shows a stronger pattern than the community guidelines

The most useful HE source in this pass is `karpathy/autoresearch`.

Its architecture creates substantial autonomy by making the execution envelope unusually narrow and measurable.

## Fixed ownership

The repository identifies only three central artifacts:

```text
prepare.py   fixed data/evaluation/runtime; agent must not modify
train.py     sole experiment surface; agent modifies
program.md   human-authored agent instructions
```

This establishes a clean separation between:

- evaluation authority;
- mutable experiment object;
- human-owned policy/control instructions.

The harness does not make the agent safe by micromanaging every experimental idea. It makes the **space of legal mutation explicit and small**.

## Baseline first

The first run establishes baseline performance before experimentation.

That makes later keep/discard decisions comparative rather than intuitive.

## Fixed budget

Experiments use a fixed five-minute training budget.

This reduces confounding and bounds resource consumption while still giving the agent wide latitude inside the mutable surface.

## Objective acceptance

The main metric is `val_bpb`, lower is better. The evaluation harness is explicitly protected from agent modification.

This is an important authority boundary:

> The same agent may propose and implement the experiment, but it does not control the metric that decides whether the experiment improved the system.

## Keep / discard / crash ledger

Each experiment records:

- commit;
- metric;
- memory;
- status (`keep`, `discard`, `crash`);
- short description.

Worse/equal experimental branches are reset; better results advance the working branch.

## Context-efficient log handling

`program.md` redirects noisy run output to a file, then extracts only key metrics with `grep`. Full error context is read only when a run crashes, using a bounded tail.

This is Progressive Disclosure applied to runtime output.

## Simplicity is part of acceptance

`program.md` explicitly treats complexity as a cost:

- tiny metric gains can be rejected if they require ugly/hacky complexity;
- deleting code while preserving or improving the result is considered a strong win;
- equal performance with much simpler code can be worth keeping.

That is stronger than the generic “use fewer lines” interpretation in the community guideline.

## High autonomy under hard boundaries

After initial setup confirmation, `program.md` tells the agent to continue experimenting autonomously until interrupted.

HE should not copy “NEVER STOP” as a general agent rule.

The reason this can work here is the surrounding containment:

```text
single mutable file
+ fixed branch
+ immutable evaluator
+ no dependency installation
+ fixed time budget
+ baseline
+ objective metric
+ revert on worse outcome
```

The durable lesson is:

> **Autonomy can increase when mutation scope, resource bounds and acceptance evidence become stronger.**

**Disposition: STRONGLY REINFORCE.**

---

# 4. `program.md` as a lightweight Skill / control plane

Karpathy's `autoresearch` README explicitly describes `program.md` as essentially a super-lightweight Skill.

But its role is more interesting than the label:

```text
human edits program.md
agent edits train.py
fixed evaluator decides experiment outcome
```

This separates:

- policy/process design;
- execution;
- evaluation.

That is a useful HE model for autonomous loops.

A harness can be powerful without a large orchestration framework when:

- the task surface is narrow;
- the acceptance function is explicit;
- state transitions are cheap/reversible;
- the model has enough room to choose tactics.

**Disposition: REINFORCE minimal control planes.**

---

# 5. Karpathy's broader repository style reinforces cognitive simplicity

The same design preference appears outside `autoresearch`.

## nanochat

Karpathy describes `nanochat` as a simple experimental LLM-training harness. It intentionally avoids becoming an exhaustively configurable framework and emphasizes accessibility in terms of **cognitive complexity**, not merely monetary cost.

Its interface concentrates many decisions behind one primary complexity dial (`--depth`) rather than exposing a broad configuration surface.

This does not mean every production system should have one configuration option. The useful HE mechanism is:

> Expose the smallest interface that corresponds to meaningful user decisions; derive incidental complexity internally when it can be done safely.

## llama2.c

The project deliberately hard-codes the target architecture into a small pure-C inference implementation rather than creating a general inference framework with broad abstraction/configuration surfaces.

Again, the point is not to copy the architecture; it is evidence of an explicit tradeoff in favor of understandability and narrow purpose.

## micrograd / related minimalist projects

Karpathy's teaching/experimental projects repeatedly minimize abstraction and dependency count to expose the mechanism being studied.

HE implication:

> **Cognitive complexity is an operational harness cost.**

A framework can be technically capable while making agent/human reasoning harder because it exposes too many policies, knobs, routes and extension points.

**Disposition: REINFORCE simplicity as a harness property, not merely a code-style preference.**

---

# 6. Declarative goals versus procedural micromanagement

The community guideline's “Goal-Driven Execution” and the actual `autoresearch` architecture converge on a useful pattern:

```text
objective
+ legal mutation surface
+ resource budget
+ acceptance metric
        ↓
agent chooses tactics
```

This is stronger than telling an agent exactly which files to read, which commands to run and which implementation steps to follow when those details are not load-bearing.

It directly reinforces HE's recent adaptive-autonomy research:

> reduce procedural prescription before reducing authority or acceptance constraints.

Candidate HE principle:

> **Prefer declarative goals with verifiable postconditions over procedural micromanagement when the execution envelope is sufficiently bounded.**

---

# 7. Behavioral invariants should earn always-on context

The video recommends placing the four principles in `CLAUDE.md`, making them always present.

That can be reasonable because the rules are broad behavioral invariants rather than detailed workflow instructions.

But HE should not treat “sounds universally useful” as proof that always-loaded context is free.

Potential effects to measure:

- fewer unnecessary changed lines;
- fewer speculative abstractions/features;
- fewer user corrections caused by silent assumptions;
- higher rate of explicit success criteria / reproduction before mutation;
- more unnecessary clarification questions;
- increased latency or verbosity on trivial tasks;
- interactions/conflicts with existing project instructions.

### Connection to Experience-Derived Harness Evolution

Harness Miner or a similar evidence loop could identify whether real sessions repeatedly show:

- assumption drift;
- drive-by mutation;
- speculative complexity;
- weak completion criteria.

Only then should a project test a compact invariant block, ideally with control/ablation evidence.

**Disposition: ASSESS.**

---

# 8. Video claims to reject or soften

## “These rules make the model never hallucinate when coding”

Unsupported. Behavioral guidance may reduce some failure classes but cannot guarantee absence of hallucination or incorrect implementation.

**REJECT as absolute claim.**

## “Combine all the frameworks”

Not justified by evidence in the video.

Overlapping planning/debugging/review frameworks can add routing ambiguity, duplicated rules and extra context.

**REJECT as default architecture. Evaluate mechanisms independently.**

## Treating CLAUDE.md as the model's “personality/soul”

Useful metaphor for always-on instruction scope, but not technically precise. It is context/instruction, not a modification to model weights or enduring personality.

**REJECT literal interpretation.**

## Mandatory clarification on every ambiguity

Use consequence and uncertainty. Trivial, reversible ambiguity can often be resolved by inspecting source or choosing a conventional path. Asking the human unnecessarily can reduce autonomy without improving outcomes.

**ASSESS adaptively, not ritualistically.**

---

# 9. Cross-project implications

## HE

Strong additions:

- stable behavioral invariants versus progressively disclosed procedures;
- cognitive complexity as a harness cost;
- declarative goals + observable postconditions;
- narrow mutation surface as an autonomy enabler;
- evaluator/control-plane separation;
- baseline-first and keep/discard experiment loops;
- simplicity as part of acceptance rather than raw LOC minimization.

## PMB / work MB

No immediate rule insertion is justified.

A project could later test a compact invariant kernel if session evidence shows recurring assumption/scope/complexity/completion failures.

Do not automatically paste the community CLAUDE.md into MB or PMB; compare against existing instructions first and remove duplication.

## ACR

Potential later research only:

- detect scope creep / unrelated diff surface;
- detect speculative complexity as an advisory concern;
- assess whether proposed findings map to the requested change.

Do not distract current AACR-Bench/evidence-quality work without observed ACR failures in these categories.

## Harness Miner

Potential proactive categories:

- user repeatedly correcting unstated assumptions;
- repeated drive-by edits/reverts;
- implementation size/architecture repeatedly reduced by user;
- tasks reported complete without satisfying explicit acceptance checks;
- rules that appear to add caution/latency without changing outcomes.

History should propose these candidates; ablation/evals should decide whether any always-on rule earns permanence.

---

# Research disposition

## STRONGLY REINFORCE

- explicit source attribution/provenance;
- simplest sufficient solution;
- surgical scope;
- observable success criteria;
- baseline-first experimentation;
- immutable/independent acceptance mechanisms;
- narrow mutable surfaces enabling greater autonomy;
- context-efficient output filtering;
- cognitive complexity as a harness cost.

## ASSESS

- compact always-on behavioral invariant kernel;
- measuring assumption/scope/complexity/completion failure classes in real sessions;
- ablation of behavioral guardrails against representative tasks;
- extending bounded-autonomy patterns beyond research loops when acceptance functions are strong.

## PARK

- generic autonomous infinite-loop framework;
- universal framework-stack routing across Superpowers/GSD/G-Stack/etc.;
- generalized “Karpathy style” package inside HE.

## REJECT

- attribution of the `multica-ai` repository as Karpathy's own Skill/CLAUDE.md;
- “never hallucinate” guarantees;
- always asking clarification regardless of consequence;
- raw LOC as a simplicity score;
- adding multiple overlapping workflow systems without evidence;
- treating autonomy as the absence of boundaries.

---

# Candidate HE principles — research status

> **Always-on context should encode stable behavioral invariants, not full workflows.**

> **Prefer declarative goals with verifiable postconditions over procedural micromanagement when the execution envelope is sufficiently bounded.**

> **Autonomy grows safely when the mutable surface narrows and acceptance becomes more objective.**

> **Cognitive complexity is a harness cost.**

> **Simplicity is an acceptance tradeoff, not a line-count target.**

These remain research candidates pending corroboration and/or local behavioral evaluation.

---

# Bottom line

The four community rules are useful, but they are not the deepest result.

Karpathy's actual GitHub shows a more durable harness pattern:

```text
human-owned policy
+ narrow mutation scope
+ fixed resource budget
+ independent evaluation
+ baseline
+ reversible experiments
        ↓
high agent autonomy
        ↓
keep only measured improvements
```

That is considerably more relevant to Harness Engineering than simply installing another CLAUDE.md block.
