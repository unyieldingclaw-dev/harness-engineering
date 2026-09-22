# Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22

## Purpose

Synthesize the durable Harness Engineering implications from the first-party Claude Academy AI-Native SDLC material without copying Anthropic's file layout or duplicating Superpowers capabilities already present in the user's workflow.

Primary evidence:

- `01 Research/Sources/Claude Academy AI-Native SDLC — First-Party Deep Pass — 2026-09-22.md`

Related HE research:

- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`

---

# Executive synthesis

The strongest pattern is not a specific `intent.md → spec.md → plan.md` folder structure.

It is a staged lifecycle in which:

1. each stage has an explicit artifact or observable state;
2. one source owns each fact;
3. acceptance of the current stage authorizes transition to the next;
4. the producer continuously checks its work;
5. independent verification challenges the finished result;
6. deterministic checks absorb deterministic review work;
7. consequential actions remain behind explicit gates;
8. real solved work becomes behavioral regression evidence;
9. metrics close the loop only when they change an engineering decision.

This maps cleanly onto Superpowers + PMB + ACR without requiring another workflow framework.

---

# 1. Stage transitions should be explicit

A useful generalized lifecycle is:

```text
intent / outcome
      ↓ accepted
specification / design
      ↓ accepted
implementation plan
      ↓ authorized
implementation
      ↓ verified
review
      ↓ accepted
release
      ↓ observed
maintenance evidence
      ↓
new intent when warranted
```

The harness does not need to own every artifact. It needs to know which source owns the artifact and what observable condition means the stage is complete.

### Candidate principle

> **Accepted artifacts should define stage transitions.**

This is stronger than "follow a workflow" because it makes transitions inspectable and reduces hidden conversational state.

**Disposition: REINFORCE.**

---

# 2. Do not duplicate authorities across PMB, Superpowers, tickets, and Git

The current stack already has several legitimate owners:

- Superpowers: brainstorming/shared understanding, design/specification, implementation planning, TDD/implementation workflow.
- PMB: durable project memory, execution scope/authorization, handoff, governance, hooks.
- Git: branch/commit/worktree state.
- CI: build/test/check state.
- ACR/review tooling: review findings/evidence.

Creating duplicate `intent.md`, `spec.md`, or `plan.md` artifacts purely because another workflow uses those names would create competing truth.

### Candidate principle

> **One fact should have one owning source; linked systems may reference it but should not silently become competing authorities.**

**Disposition: STRONGLY REINFORCE.**

---

# 3. Feedback loop and verifier are different layers

The producer should be able to inspect and correct its own work while implementing.

Typical producer loop:

```text
change
  ↓
run check
  ↓
compare to expected result
  ↓
fix if needed
  ↺
```

Independent verification then checks the finished result from a fresh context or separate authority boundary.

### Candidate principle

> **Producer self-checking and independent verification solve different failure modes.**

This complements the existing HE distinction between execution success and effect verification.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Checks need acceptance semantics

A command by itself is not a complete verification contract.

Weak:

```text
run tests
```

Stronger:

```text
run tests → zero failing tests
run lint  → no disallowed findings
run build → successful exit/build marker
```

The harness should avoid letting the producer silently redefine what "green" means.

### Candidate principle

> **Verification should define the observable postcondition, not merely name the command to run.**

**Disposition: REINFORCE.**

---

# 5. Behavioral evals are distinct from structural CI

Current harness CI commonly answers mechanical questions:

- does a file exist?
- is syntax valid?
- does a hook run?
- are secrets absent?
- does a script pass shellcheck?
- does a template match expected structure?

Those checks are necessary but insufficient for behavior-shaping configuration.

Changes to instructions, skills, hooks, routing, or models can leave every structural test green while changing agent behavior materially.

### Candidate principle

> **Harness configuration is executable behavior and should have behavioral regression tests.**

A behavioral corpus should come from real solved tasks and incidents, not invented examples chosen only to satisfy a target count.

For each durable case, retain only what is needed to reproduce the behavior under test:

```text
starting state / fixture
prompt or task
expected or accepted outcome
observable checks
important failure mode
```

### PMB implication

The PMB pilot can become the beginning of this evidence base. Do not interrupt the pilot to build a framework first.

**Disposition: ASSESS after pilot.**

---

# 6. Deterministic checks should absorb deterministic review work

If CI, a linter, formatter, schema checker, or hook can decide a property reliably, an LLM reviewer should not spend tokens rediscovering it unless the semantic context changes the meaning.

Use probabilistic review capacity for questions that actually require interpretation:

- subtle correctness;
- security reasoning;
- intent/spec compliance;
- architecture drift;
- cross-file consequences;
- unsupported assumptions;
- residual risk.

### Candidate principle

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

This is directly relevant to ACR noise reduction.

**Disposition: STRONGLY REINFORCE.**

---

# 7. Metrics are decision inputs, not a product requirement

The Claude Academy maintenance loop uses system metrics to detect degradation and initiate diagnosis or new work. The durable mechanism is not the observability vendor.

A useful metric needs:

1. **owner/source** — where the fact comes from;
2. **baseline/expected meaning** — what normal looks like;
3. **freshness/provenance** — when/how it was measured;
4. **decision boundary** — what changes when the metric leaves the acceptable range;
5. **verification** — how the corrective action is confirmed.

Without those semantics, adding telemetry creates inventory rather than leverage.

### Candidate principle

> **Collect a metric only when a known engineering decision consumes it.**

**Disposition: REINFORCE.**

---

# 8. PMB metrics should remain lightweight and source-owned

PMB is primarily a local/repository harness and CLI, not a production SaaS runtime. Its useful metrics today are mostly available from existing deterministic sources:

- `mb status` / `mb doctor` health signals;
- startup context size;
- stale memory/handoff state;
- task-contract/scope warnings;
- hook/CI pass/fail;
- handoff/resume behavior;
- archive/reference retrieval when required;
- behavioral-eval pass/fail when that corpus exists;
- pilot outcomes such as human intervention and task completion quality.

Do not make PMB sessions append telemetry manually. If a metric matters, derive it from the owning system or add a deterministic machine-readable interface when an actual consumer exists.

### Tooling disposition

**Do not add Datadog or Sentry to PMB/HE now.**

Reasons:

- there is no production service whose runtime failure stream requires those platforms;
- most useful PMB/HE facts already exist in Git, CI, CLI output, hooks, or test artifacts;
- external telemetry would add cost, credentials, privacy/security surface, retention questions, and operational maintenance before there is a demonstrated need;
- it risks turning observability into a subsystem rather than answering an observed problem.

If a future PMB-hosted service or external Dashboard becomes operationally important, reassess from the concrete failure/diagnostic need.

**Disposition: REJECT tool-first adoption; PARK external observability platform.**

---

# 9. Product applications have a different threshold

A deployed user-facing application can justify an error/telemetry service much sooner than PMB or HE because production failures may otherwise be invisible.

For a small application, an error-focused tool can be sufficient before a full observability platform is warranted.

The decision should be driven by questions such as:

- Are production exceptions currently invisible?
- Do users report failures before the team can detect them?
- Is distributed tracing actually needed?
- Are latency/resource metrics necessary for a real SLO or debugging problem?
- Is there enough scale/operational complexity to justify maintaining the integration?

Do not infer that "AI-native SDLC uses metrics" means every project needs enterprise telemetry software.

---

# 10. Metrics worth observing in PMB's pilot before building anything

The pilot can collect a small set of evidence without introducing new infrastructure:

- task completed / not completed;
- human intervention points;
- handoff required and whether resume succeeded;
- stale or conflicting state encountered;
- archive/reference required and whether it was retrieved;
- scope warning/violation occurrences;
- number of repeated failed attempts before escalation;
- verification result and whether producer self-check disagreed with independent review;
- wall time if easy to capture;
- provider usage only when the provider exposes it directly and the value helps a decision.

Do not add unsupported pass/fail thresholds just to create a dashboard.

**Disposition: MEASURE FIRST, DESIGN LATER.**

---

# 11. Cross-project disposition

## HE

Document metrics semantics and artifact-gated transitions as research principles. Do not build an observability platform.

## PMB

Use pilot evidence and existing CLI/CI sources. Assess behavioral evals after the pilot. Do not add Sentry/Datadog now.

## ACR

Continue benchmark-style measurement around finding quality, false positives/fabrication, timeouts, clustering, and accepted outcomes. External telemetry platform not justified by current evidence.

## User-facing applications

Evaluate production error visibility separately. If a real app has invisible production exceptions, an error-focused integration may be appropriate; do not generalize that requirement back into PMB/HE.

---

# Candidate HE principles — research status

> **Accepted artifacts should define stage transitions.**

> **One fact should have one owning source; linked systems may reference it but should not silently become competing authorities.**

> **Producer self-checking and independent verification solve different failure modes.**

> **Verification should define the observable postcondition, not merely name the command to run.**

> **Harness configuration is executable behavior and should have behavioral regression tests.**

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

> **Collect a metric only when a known engineering decision consumes it.**

---

# Bottom line

The Claude Academy material strengthens the case for artifact-gated work, behavioral harness evals, explicit verification semantics, and evidence-driven maintenance.

For the current HE/PMB environment, the right metrics strategy is deliberately small:

> **measure observed workflow outcomes from systems that already own the facts; add external observability only when a concrete runtime problem cannot be diagnosed reliably without it.**

That keeps observability subordinate to engineering need instead of turning telemetry into another governance product.