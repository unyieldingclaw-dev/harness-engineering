# Risk-Directed Review & Release Proof — 2026-09-24

## Purpose

Synthesize a cross-source Harness Engineering pattern strengthened by John Kim's AI code-review workflow without turning his exact PR process, feature-gating choices, or multi-agent setup into HE requirements.

Primary evidence:

- `01 Research/Sources/John Kim — Risk-Directed AI Code Review, Proof & Launch Safety — 2026-09-24.md`

Related HE research:

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Sources/Alibaba Open Code Review & AACR-Bench — ACR Follow-up — 2026-09-21.md`

This note is research synthesis. It does not authorize changes to PMB, ACR, CI, PR templates, deployment systems, or feature-flag infrastructure.

---

# Executive synthesis

AI-generated code increases implementation throughput faster than human line-by-line review capacity. The durable response is not to stop reviewing and not to apply identical review depth everywhere.

A stronger model is:

```text
change
  ↓
classify consequence / coupling / reversibility
  ↓
author produces task-relevant evidence
  ↓
deterministic checks establish mechanical properties
  ↓
independent semantic review targets residual risk
  ↓
merge acceptance
  ↓
separate launch authorization when production exposure matters
  ↓
staged observation / rollback when uncertainty remains
```

The core shift is from **review volume** to **risk-directed evidence**.

---

# 1. Review depth is a risk allocation decision

Review effort should increase when:

- failure can affect many consumers or shared state;
- existing behavior changes;
- security/data/permission boundaries are touched;
- rollback is difficult or slow;
- the change crosses infrastructure or integration boundaries;
- verification evidence is weak or indirect;
- the reviewer cannot establish isolation.

Review effort may decrease when:

- scope is genuinely isolated;
- behavior is behind a working gate;
- focused deterministic/runtime/visual evidence is strong;
- rollback is cheap and proven;
- unchanged behavior is covered by reliable controls.

No single risk score is implied. A coarse explicit assessment is preferable to an opaque numeric model unless measurement shows a score improves decisions.

### Candidate principle

> **Review effort should scale with consequence, coupling, reversibility, and evidence quality — not diff size alone.**

**Disposition: STRONGLY REINFORCE.**

---

# 2. Evidence should travel with the claim

The author/producer and independent reviewer have different jobs.

The producer should make the result easy to falsify by supplying compact evidence for the claims it is making.

Potential evidence:

- focused test result;
- integration/runtime path exercised;
- build/static-check result;
- screenshot/video for user-visible behavior;
- invariant or unchanged behavior verified;
- known unverified area;
- rollback/gate state for consequential release work.

Do not require irrelevant evidence simply to fill a template.

### Candidate principle

> **A consequential change should arrive at its review boundary with the evidence needed to evaluate its claims.**

This extends existing HE assertion-vs-evidence work from final completion into the review lifecycle.

**Disposition: REINFORCE.**

---

# 3. Author confidence is not acceptance authority

A producer can report uncertainty, but it cannot establish correctness by declaring confidence.

Prefer evidence-backed statements such as:

```text
verified: X, Y
not verified: Z
known failure/unknown: Q
```

rather than treating `confidence: 95%` as proof.

A confidence signal may be useful for routing or calibration only if an actual consumer and outcome history exist.

### Candidate principle

> **Self-reported confidence can route attention; it cannot settle correctness.**

**Disposition: REINFORCE.**

---

# 4. Fresh review should remove producer bias, not authoritative context

Independent verification should not inherit the producer's implementation conversation by default.

It should still receive the facts required to judge the change:

```text
accepted intent/spec
current repository/diff
relevant policies/invariants
deterministic check results
required behavior
```

It should not be primed by:

```text
producer's conversational rationale
producer's self-approval
hidden assumptions
"why this is obviously correct" narrative
```

### Candidate principle

> **Independent verification should share authoritative task truth, not the producer's implementation bias.**

This is stronger and safer than a simplistic "fresh context" rule.

**Disposition: STRONGLY REINFORCE.**

---

# 5. Deterministic checks and semantic review are complementary

Static checks, linting, type checks, builds, schemas, and focused tests should establish properties they can decide reliably.

Semantic review should concentrate on residual questions such as:

- behavior and correctness;
- architecture/coupling;
- security reasoning;
- intent/spec compliance;
- unsupported assumptions;
- hidden blast radius;
- failure/recovery behavior.

This does not mean deterministic results disappear from review. They become evidence rather than duplicated LLM findings.

### Existing principle strengthened

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

**Disposition: STRONGLY REINFORCE.**

---

# 6. Merge-ready and launch-ready are separate stage transitions

Repository acceptance does not prove safe production exposure.

A useful distinction is:

```text
review accepted
  → merge-ready

release controls + operational evidence
  → launch-ready

observed staged exposure
  → expand / hold / rollback
```

Launch readiness may require:

- production-safe configuration;
- rollback/disable path;
- migration/recovery path;
- monitoring/error visibility;
- performance/security checks;
- release cohort/gate state.

Not every project or change needs staged deployment. The separation matters when production exposure creates additional risk beyond repository correctness.

### Candidate principle

> **Merge acceptance does not authorize production exposure.**

**Disposition: STRONGLY REINFORCE artifact-gated lifecycle.**

---

# 7. Rollout is useful only when it produces decision-relevant evidence

Feature flags, canaries, limited cohorts, and experiments can reduce exposure while evidence is gathered.

They are not safety theater.

A rollout mechanism is useful only when:

- the new behavior is actually isolatable;
- the monitored signal can reveal the risk being tested;
- rollback/disable is real and fast enough;
- the expansion/rollback decision is defined;
- the rollout does not itself add disproportionate complexity.

This connects release engineering to HE's metrics rule:

> **Collect a metric only when a known engineering decision consumes it.**

For small/local tools such as PMB, feature-flag infrastructure is usually unjustified. For deployed user-facing products, it may be valuable much earlier.

**Disposition: REINFORCE; context-specific.**

---

# 8. Pre-release assurance is coverage, not agent count

Useful pre-release domains may include:

- required user flows;
- weird/failure states;
- performance bottlenecks;
- security/permissions/data boundaries.

Those domains do not imply one agent per domain.

Choose the cheapest reliable mechanism:

```text
deterministic test/tool
single reviewer with distinct pass
specialized independent reviewer
runtime probe
human judgment
```

Use multiple agents when independence, specialization, context isolation, or parallelism demonstrably improves evidence.

### Candidate principle

> **Assurance should cover the relevant risk domains; agent count is an implementation detail.**

**Disposition: STRONGLY REINFORCE.**

---

# 9. Automated reviewer/fixer loops need bounded settlement

A recurring reviewer→fixer→revalidate loop can be useful, but only with explicit stop/authority rules.

Minimum concerns:

- what classes of findings may be auto-fixed;
- which attempt owns the current mutation;
- iteration/time ceiling;
- deterministic verification after each fix;
- ambiguity/conflict escalation;
- unresolved finding preservation;
- merge/deploy authority kept separate unless explicitly granted.

This is an instance of HE's bounded execution envelope rather than a new orchestration architecture.

### Candidate principle

> **Review/fix automation should converge inside a bounded settlement loop or stop for adjudication.**

**Disposition: REINFORCE bounded execution; PARK implementation.**

---

# 10. Cross-project disposition

## HE

Retain the risk-and-evidence model as research synthesis. Do not build a universal risk scorer, feature-flag platform, or PR workflow engine.

## PMB

No direct implementation follows. PMB owns project truth, scope/authorization, handoff, and some verification context — not release management.

## ACR

This source is relevant to ACR's evaluation and reporting posture, especially:

- risk-directed semantic review;
- independent reviewer context;
- deterministic evidence separation;
- domain coverage;
- author proof as evidence that must still be independently evaluated.

Do not implement leaf/trunk classification or confidence scoring until labeled evaluation shows it improves finding quality or review allocation.

## Deployed product projects

Consider separate launch-readiness and staged-rollout evidence where actual production exposure warrants it.

---

# Candidate HE principles — research status

> **Review effort should scale with consequence, coupling, reversibility, and evidence quality — not diff size alone.**

> **A consequential change should arrive at its review boundary with the evidence needed to evaluate its claims.**

> **Self-reported confidence can route attention; it cannot settle correctness.**

> **Independent verification should share authoritative task truth, not the producer's implementation bias.**

> **Merge acceptance does not authorize production exposure.**

> **Assurance should cover the relevant risk domains; agent count is an implementation detail.**

> **Review/fix automation should converge inside a bounded settlement loop or stop for adjudication.**

---

# Bottom line

The practical review problem created by AI code volume is not solved by blindly reading every generated line or blindly trusting agents.

The stronger Harness Engineering pattern is:

> **allocate review effort by risk, carry evidence with the change, keep verification independent of author bias, let deterministic systems prove what they can, and require a separate launch decision when production exposure creates additional consequences.**
