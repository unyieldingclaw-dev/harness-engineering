# Adaptive Autonomy, Diagnostic Gates & Fan-Out/Fan-In — 2026-09-25

## Purpose

Synthesize durable Harness Engineering implications from recent Claude Code operating-pattern research without promoting one creator's workflow into HE doctrine.

Primary evidence:

- `01 Research/Sources/Nick Saraev — Claude Code Leash, Diagnosis, Parallelism & Fan-Out-Fan-In — 2026-09-25.md`

Related HE research:

- `00 Overview/Harness Engineering Philosophy.md.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`

This is research synthesis only. It does not authorize broader permissions, a new orchestration framework, PMB changes, ACR changes, or replacing MCP integrations.

---

# Executive synthesis

Several seemingly separate practices collapse into one architecture:

```text
clear outcome + bounded authority
          ↓
model chooses method
          ↓
ambiguous failure?
  yes → diagnose before mutate
          ↓
independent work?
  yes → parallelize under explicit ownership
          ↓
needs breadth/diversity?
  yes → fan out
          ↓
structured evidence + provenance
          ↓
fan in / adjudicate
          ↓
verification + acceptance authority
```

The central idea is:

> **Give capable models freedom inside a well-defined execution envelope; spend structure at ambiguity, shared-state, evidence and acceptance boundaries rather than prescribing every step.**

This is compatible with HE's existing principles of Evidence Before Architecture, Single Ownership, Progressive Disclosure, Human Review Over Automation and Assessment Before Architecture.

---

# 1. Adaptive autonomy: loosen the path, not the boundary

As models improve, some procedural scaffolding stops being load-bearing.

A harness should therefore distinguish:

## Procedural guidance

“How should the model go about the work?”

Examples:

- exact sequence of file reads;
- mandatory planning prose;
- step-by-step implementation choreography;
- legacy prompt workarounds.

These should be periodically challenged by behavioral evidence.

## Authority and acceptance boundaries

“What may the model change, and what establishes completion?”

Examples:

- allowed scope;
- protected files/policies;
- external side effects;
- tests/postconditions;
- review/merge authority;
- evidence requirements.

These do not become unnecessary merely because the model is smarter.

### Candidate principle

> **Reduce procedural prescription before reducing authority or acceptance constraints.**

This extends HE's existing Model Capability Drift work.

---

# 2. Diagnose-before-mutate is an evidence gate, not a universal ceremony

When the requested fix is underspecified or the failure has multiple plausible owners, mutation should follow diagnosis rather than precede it.

Recommended pattern:

```text
observe/reproduce
      ↓
state the failure precisely
      ↓
identify candidate causes
      ↓
locate ownership
      ↓
select authorized correction
      ↓
mutate
      ↓
rerun original evidence path
```

The diagnostic stage is valuable because it separates **what is wrong** from **what should change**.

### When not to require it

Do not add a diagnosis artifact when:

- the failure is deterministic and already localized;
- the corrective scope is explicit;
- the repair is low-risk and directly verifiable.

### Candidate principle

> **Use diagnosis as a mutation gate when cause, ownership or desired scope is uncertain.**

This is a direct operational expression of Assessment Before Architecture.

---

# 3. Capability-surface minimization should respect ownership boundaries

The useful optimization is not “MCP bad, Skill good.”

The stronger model is:

```text
external capability owner
        ↓
minimum stable tool/API surface
        ↓
optional task-local Skill/procedure
        ↓
model
```

MCP, API wrappers, CLIs, scripts and Skills solve different problems.

### Decision heuristic

- **Live authenticated data/action surface:** MCP/API/CLI remains the capability owner.
- **Repeatable task procedure:** Skill is a good procedure layer.
- **One or two stable deterministic calls:** narrow wrapper/script may reduce routing/context/security surface.
- **Broad evolving service:** retain MCP and constrain discovery/allowlist/instructions rather than rebuilding the service inside a Skill.

### Candidate principle

> **Shrink exposed capabilities without creating a second or incorrect owner for the capability.**

This extends Single Ownership and Progressive Disclosure.

---

# 4. Parallelism needs an ownership taxonomy

“Run tasks in parallel” is too vague for a harness rule.

HE should distinguish four classes:

| Class | Default stance | Main risk |
|---|---|---|
| Read/research | parallel-friendly | duplicate/low-quality evidence |
| Independent verification | parallel-friendly | correlated reviewers / duplicated assumptions |
| Disjoint mutation | conditional | hidden semantic dependencies / merge conflicts |
| Shared-state mutation | usually serialize or strongly orchestrate | ownership races / stale attempts |

For mutating work, file separation is only a proxy. True parallel safety depends on behavior/interface dependency.

### Mutation contract

Parallel mutation should have, where warranted:

```text
explicit task scope
explicit owner
isolated worktree/branch or equivalent
known shared interfaces
integration/merge owner
post-merge verification
```

### Candidate principle

> **Parallelism is safe to the degree that ownership, dependencies and settlement are explicit.**

This strengthens the existing Orca-derived orchestration work.

---

# 5. Fan-out/fan-in is a reusable HE pattern

Fan-out/fan-in deserves to be distinguished from ordinary parallel implementation.

Its purpose is **breadth or diversity of judgment**, not simply wall-clock speed.

## Fan-out

Create multiple bounded explorations that are independent enough to add new information.

Possible fan-out strategies:

- divide the search space;
- ask independent agents to attack the same hypothesis;
- assign specialist review lenses;
- explore conventional and non-obvious alternatives separately.

Each worker should have a small, sharp context and an evidence requirement.

## Fan-in

Do not merely concatenate worker prose into a “mega prompt.”

The fan-in boundary should retain:

```text
worker/scope identity
claim/candidate
source or artifact pointer
observed evidence
confidence/authority class
conflicts
uncertainty
coverage gaps
```

Then the adjudicator can:

- deduplicate;
- identify correlated evidence;
- resolve or surface conflicts;
- challenge weak claims;
- ask targeted follow-ups;
- identify unexplored regions;
- produce a decision/recommendation within its authority.

### Candidate principle

> **Explore widely, adjudicate narrowly; preserve provenance through the fan-in boundary.**

---

# 6. Fan-in authority must remain explicit

A strong or expensive model is not automatically the authority that can declare a consequential task complete.

Useful hierarchy:

```text
scouts / specialists
      ↓
evidence
      ↓
synthesizer / adjudicator
      ↓
acceptance mechanism
      ↓
human/deterministic gate when required
```

For low-risk research, the synthesizer's output may be sufficient.

For code changes, deployments, security decisions or irreversible external actions, the fan-in model's conclusion should remain one input to acceptance.

### Candidate principle

> **Synthesis authority and acceptance authority are separate unless explicitly combined.**

This directly aligns fan-out/fan-in with HE's existing authority model.

---

# 7. Cheap-scout / expensive-judge is a cost strategy, not an architecture law

The common pattern:

```text
cheap workers → expensive judge
```

is attractive because fan-out consumes many calls.

HE should retain the more general model-tiering rule:

```text
role/workload
+ required judgment
+ verification strength
+ consequence
+ measured fitness
        ↓
model/effort choice
```

A cheap model that systematically misses relevant evidence can make fan-out cheaper and the final answer worse. A strong model that only deduplicates structured deterministic results can be wasteful.

### Candidate principle

> **Cost-optimize each fan-out/fan-in role only after role-specific fitness is measured.**

---

# 8. Independence is a quality property, not just an execution detail

Fan-out can fail when workers share the same path dependency:

- same prompt framing;
- same retrieved sources;
- same stale project context;
- one worker's conclusions copied into the next;
- common hallucinated premise.

For diversity-sensitive tasks, the harness should preserve some independence before aggregation.

Potential techniques:

- fresh worker contexts;
- distinct search scopes;
- separate reviewer lenses;
- delayed exposure to other worker conclusions;
- explicit request for disconfirming evidence.

Do not force artificial diversity where the task merely needs sectioning.

### Candidate principle

> **Use independence deliberately when the purpose of fan-out is to reduce path dependence.**

---

# 9. Fan-out must be bounded by expected information value

Anthropic's multi-agent research work shows that parallel agents can improve breadth-heavy research, but at materially higher token cost, and notes that coding often has fewer naturally parallelizable workstreams.

Therefore fan-out needs a stopping rationale.

Possible stopping conditions:

- coverage target met;
- no new unique evidence after a wave;
- source classes exhausted;
- confidence gap closed;
- budget/time ceiling reached;
- remaining uncertainty requires a different evidence source rather than more agents.

### Candidate principle

> **Scale fan-out while new independent evidence is worth more than the added coordination and token cost.**

Do not use worker count as a capability metric.

---

# 10. Relationship to HE's existing principles

## Evidence Before Architecture

Diagnosis and fan-out should gather evidence before selecting architecture/correction.

## Single Ownership

Parallel workers may explore many possibilities, but ownership of current project truth, task mutation and final acceptance should remain explicit.

## Progressive Disclosure

Skills, MCP tool search, narrow worker contexts and focused handoffs all reduce irrelevant always-loaded context.

## Continuous Improvement

“Loosen the leash” is a model-capability-drift maintenance loop: remove scaffolding that no longer improves evaluated outcomes.

## Human Review Over Automation

Fan-in can assist judgment but does not automatically erase human authority for consequential architecture decisions.

## Assessment Before Architecture

Diagnose-before-fix is the same principle applied at incident/task scale.

---

# 11. PMB implications

No implementation change is justified from this source alone.

The research reinforces:

- successor-oriented fresh-session handoff;
- current project truth over transcript/history;
- progressive retrieval rather than full-context injection;
- failure corrections should land in the owning component rather than accumulating in startup context;
- diagnosis/ownership state may be worth preserving when a handoff interrupts debugging.

Potential pilot question:

> When a session discovers a failure but has not yet fixed it, does the handoff preserve enough evidence about reproduction, diagnosis confidence, owning component and next verification step for a fresh session to continue without replaying the entire transcript?

Measure before changing PMB.

---

# 12. ACR implications

No immediate implementation change.

ACR is already a fan-out system in one sense: multiple domain agents inspect the candidate from different perspectives.

A worthwhile later audit question is:

> **Does ACR's fan-in preserve independent evidence and disagreement, or can orchestration/formatting flatten provenance into a homogeneous finding list?**

This should be investigated only after the current AACR-Bench / evidence-quality work, because better evaluation should precede orchestration changes.

Potential metrics if later assessed:

- unique finding contribution per reviewer;
- duplicated/correlated findings;
- reviewer disagreement;
- source/evidence retention through synthesis;
- false-positive rate before/after fan-in;
- findings lost or incorrectly merged;
- cost/latency per useful unique finding.

---

# Research dispositions

## STRONGLY REINFORCE

- reduce obsolete procedural scaffolding as model capability improves;
- preserve authority/acceptance boundaries while doing so;
- diagnose before mutation when cause/scope is ambiguous;
- progressive disclosure and minimum exposed capability surface;
- fresh-context handoff;
- corrections belong in the owning component;
- parallel research/verification when independent.

## ASSESS

- fan-out/fan-in as a formal HE pattern;
- provenance-preserving fan-in schema;
- parallel mutation contract;
- independence controls for diversity-sensitive exploration;
- ACR fan-in evidence preservation;
- minimum PMB debugging state needed across handoff.

## REJECT

- autonomy measured by fewer constraints;
- parallelism measured by number of agents;
- file disjointness as proof of semantic independence;
- majority vote as truth;
- expensive model as automatic acceptance authority;
- universal MCP-to-Skill conversion;
- every failure becoming a persistent prompt rule.

---

# Candidate principle set — research status only

> **Reduce procedural prescription before reducing authority or acceptance constraints.**

> **Use diagnosis as a mutation gate when cause, ownership or desired scope is uncertain.**

> **Shrink exposed capabilities without creating a second or incorrect owner for the capability.**

> **Parallelism is safe to the degree that ownership, dependencies and settlement are explicit.**

> **Explore widely, adjudicate narrowly; preserve provenance through the fan-in boundary.**

> **Synthesis authority and acceptance authority are separate unless explicitly combined.**

> **Use independence deliberately when the purpose of fan-out is to reduce path dependence.**

> **Scale fan-out while new independent evidence is worth more than the added coordination and token cost.**

These should remain research candidates until they survive additional source comparison and/or local behavioral evidence.

---

# Bottom line

The useful shift is not toward more agent complexity.

It is toward **adaptive structure**:

- less micromanagement where the model has demonstrated competence;
- more explicit gates where ambiguity, authority or shared state makes mistakes expensive;
- parallel exploration where independence creates information value;
- structured fan-in where evidence must be reconciled;
- acceptance kept separate from synthesis when consequences require it.

That is a cleaner Harness Engineering direction than either extreme: rigid step-by-step prompting everywhere or permissionless swarms everywhere.
