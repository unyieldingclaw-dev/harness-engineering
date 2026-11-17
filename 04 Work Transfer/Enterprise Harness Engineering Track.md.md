# Enterprise Harness Engineering Track

## Purpose

Provide a portable Harness Engineering methodology that can be applied inside the enterprise environment without transferring private enterprise information into the public project.

This track translates public HE research into **bounded enterprise assessments and pilots**. It does not prescribe adoption of PMB, pstack, MoltenBase, Noodle, or any other external system wholesale.

## Enterprise usage assumptions (not environment inventory)

Any enterprise assessment must begin **inside the approved private workspace** by independently inventorying actual agents, model gateways, repository hosting, information access, review requirements, and operational policies. **Do not publish** which tools are deployed, favored, absent, or available within a specific organization, or any internal measurements, in this public research repository.

## General research problem

Existing public HE research identifies recurring risks when AI-assisted engineering scales. These are candidate risks to test, **not assertions about a specific employer's incidents, measured review performance, or current deployment**.

The broader HE research also indicates recurring risks when AI-assisted engineering scales:

- green tests can still protect the wrong behavior;
- verification machinery can drift;
- skills duplicated across agent hosts can diverge;
- repeated model work may be better expressed as deterministic tooling;
- transcript/context reconstruction can outrank live project truth if authority is unclear;
- more agents can increase coordination cost before they increase useful throughput.

## Boundary

No proprietary code, architecture, prompts, configuration, credentials, or organization-specific information will be stored in this public repository.

The public project will produce methodology and assessment templates.

Enterprise-specific analysis will occur inside the approved enterprise environment.

---

# 2026-10-21 Work Transfer — Verification, Skill Governance & Bounded Automation

## Research inputs

Primary HE sources:

- `01 Research/Sources/Lauren Tan (poteto) — pstack, Noodle, Trust Ladder & High-Throughput Agent Engineering — 2026-10-21.md`
- `01 Research/Sources/Rob Shocks — Pstack, MoltenBase & Verification-First Agent Engineering — 2026-10-21.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`

Related PMB assessment:

- **PMB Issue #82 — Assess testing-policy hardening: conditional TDD, oracle quality, verifier sensitivity**
  - https://github.com/unyieldingclaw-dev/personal-memory-bank/issues/82

## Executive recommendation

Do **not** begin by increasing agent count or importing an agent framework.

First make the engineering environment easier to verify, harder to misuse, and less dependent on repeated model judgment.

The recommended enterprise direction is:

1. make TDD evidence-driven rather than ceremonial;
2. strengthen oracle quality and verifier sensitivity;
3. create repeatable real-artifact verification for important repositories;
4. give verification assets explicit lifecycle ownership;
5. compile stable repeated work into deterministic levers before adding agents;
6. govern reusable skills as versioned engineering artifacts across Claude Code, Cursor and Codex;
7. preserve independent review and use cross-model review only as an additional signal;
8. distinguish handoff from historical recall/source reconstruction;
9. keep sparse decision/evidence trails for long or unattended work;
10. raise autonomy only when reversibility, verifiability and consequence controls justify it.

---

# Candidate enterprise backlog

## EHE-01 — Assess evidence-driven TDD policy

### Objective

Replace an unconditional test-first rule with a policy that requires the **best executable evidence for the behavior being changed**.

### Candidate decision rule

```text
cheap, stable, acceptance-relevant regression path?
  ├─ yes → require failing-before evidence for the intended reason
  │        then minimal fix + passing-after evidence
  └─ no  → use the nearest credible executable verification,
           state why a durable failing test is impractical,
           and do not manufacture a brittle test for process compliance
```

Examples of valid fallback evidence:

- focused reproduction script;
- integration check;
- browser/runtime automation;
- snapshot comparison;
- log assertion;
- throwaway environment;
- existing acceptance harness.

### Guardrail

This is not permission to skip verification. It is stricter about evidence quality and less ceremonial about test shape.

### Acceptance for policy change

Before changing enterprise policy:

- review representative historical fixes where failing-before evidence helped;
- review cases where a new unit test would have duplicated a stronger runtime/integration verifier or produced brittle coverage;
- confirm no reduction in assurance for consequence-relevant gates, hooks, parity checks, release controls or security boundaries;
- independently review the policy change.

---

## EHE-02 — Harden oracle quality and verifier sensitivity

### Objective

Ensure tests and gates can detect the failure they claim to prevent.

For new or materially changed consequence-relevant verifiers, require evidence that:

- the checker fails when protected behavior is broken;
- failure occurs for the intended reason;
- expected values come from an independent requirement/oracle where practical;
- the check survives internal refactoring that preserves the public contract;
- a representative known-bad variant or historical failure demonstrates sensitivity.

Useful techniques:

- replay a historical defect;
- bounded mutation;
- deliberately invalid fixture;
- invert/remove a critical condition;
- reorder a modeled transition.

### Boundary

Do not mutation-test every unit test. Scale this to the consequence of a false green.

---

## EHE-03 — Create project-local real-artifact verification contracts

### Objective

Make important repositories able to prove behavior through the real user/system path instead of relying only on compile/test status.

A minimal verification contract should answer:

- **Launch** — how to start the real system;
- **Doctor** — how to confirm the environment is testable;
- **Drive** — how to exercise the relevant path;
- **Evidence** — what observable postcondition proves success;
- **Cleanup** — how to restore state safely.

The implementation may be:

- scripts;
- CLI;
- browser automation;
- integration runner;
- deterministic runbook;
- existing CI harness.

It does not need to be an AI skill.

### Candidate principle

> Model confidence, successful tool invocation, compilation and green tests are not equivalent to verified system effect.

---

## EHE-04 — Give verification assets explicit lifecycle ownership

### Objective

Prevent stale verifiers from creating false confidence.

For material verification assets, record enough information to identify:

- protected property;
- owning system/repository;
- current source/version relationship;
- owner;
- change triggers that require verifier review;
- sensitivity evidence for a known-bad state where consequence warrants it.

### Rationale

Verification code can drift in either direction:

- false red after a legitimate product change;
- false green after the checker stops observing the real property.

No new verification platform is required.

---

## EHE-05 — Pilot governed cross-agent skill management (MoltenBase-inspired)

### Objective

Manage reusable engineering skills across Claude Code, Cursor and Codex from one canonical source without creating unrelated copies.

MoltenBase is useful as a **problem demonstration**: repeated manual management of skills across agent hosts becomes its own operational burden.

The enterprise pilot should be product-neutral.

### Minimum managed metadata

Each reusable skill/capability should have:

- stable name / ID;
- purpose / trigger;
- owner;
- canonical source;
- source version / commit;
- supported hosts;
- project vs user/global scope;
- dependencies;
- required tools / permissions;
- verification method;
- trust/review state;
- last reviewed metadata.

### Required behaviors

- deploy from canonical source;
- show what each host currently sees;
- diff before update;
- refuse silent overwrite of unmanaged content;
- detect drift;
- roll back;
- distinguish canonical semantics from host-specific adapters;
- test host behavior where semantics differ.

### Explicit non-goals

The skill manager must not become:

- project memory;
- a task scheduler;
- an autonomous agent;
- a secret store;
- a universal prompt marketplace;
- an automatic self-modification mechanism;
- proof that behavior is equivalent across hosts simply because text is identical.

---

## EHE-06 — Add deterministic-lever review before new agent fan-out

### Objective

Before adding a worker/agent for repeated work, determine whether the work should be compiled into deterministic tooling.

Candidate deterministic owners:

- script;
- CLI;
- codemod;
- generator;
- static analysis;
- linter;
- query;
- validator;
- migration tool;
- reusable verifier.

### Decision rule

```text
Does the step require fresh judgment?
  ├─ no  → prefer deterministic execution
  └─ yes → model-mediated execution may be appropriate
```

### Candidate principle

> Use model capacity for judgment; use deterministic machinery for stable mechanics.

---

## EHE-07 — Keep independent review; assess cross-model interrogation as an extra signal

### Objective

Preserve independent acceptance authority while testing whether model diversity improves defect discovery.

Candidate experiment:

- implementation agent performs producer self-check;
- independent review remains mandatory where already required;
- on selected consequence-relevant changes, add a fresh-context or different-model adversarial review;
- preserve disagreement instead of majority-voting it away;
- compare incremental findings, false positives and cost.

### Boundary

Do not treat persona diversity as independence.

Do not replace deterministic checks or established independent review with multi-model consensus.

---

## EHE-08 — Separate handoff from recall/source reconstruction

### Objective

Avoid bloated handoffs and stale transcript authority.

Use:

- **handoff** for narrow in-flight state at a known transition;
- **recall** for reconstruction across historical evidence when no trustworthy handoff exists or broader recovery is needed;
- **current source-owned state** as the authority for live facts.

Recall may inspect:

- repository state;
- issues/tickets;
- team discussions;
- incident/error systems;
- decision records;
- product/runtime evidence.

### Candidate principle

> Do not pay historical reconstruction cost when current durable state plus a narrow handoff already answers the successor's job.

---

## EHE-09 — Preserve sparse decision/evidence trails for long or unattended work

### Objective

Make material autonomous work auditable without retaining every model thought/tool call.

For significant unattended work, retain only the decision points needed by a reviewer/successor:

- timestamp;
- phase;
- decision;
- reason;
- evidence pointer;
- result.

Evidence should point to durable artifacts such as:

- commit;
- pull request;
- test result;
- trace;
- screenshot;
- ticket;
- log;
- verification artifact.

### Boundary

Do not treat full transcripts as the operational record.

---

## EHE-10 — Add a cheap probe when architecture depends on an uncertain premise

### Objective

Falsify uncertain technical assumptions before architecture depends on them.

Candidate flow:

```text
uncertain technical premise?
  ├─ no  → normal planning / implementation
  └─ yes → smallest cheap probe that can falsify it
             ↓
           frame constraints
             ↓
           scaffold / implement
```

This complements planning rather than replacing it.

### Boundary

Do not adopt a general "no planning" policy.

Planning should bind expensive path-dependent choices while remaining revisable when executable evidence contradicts assumptions.

---

## EHE-11 — Assess bounded formal verification only for compact high-consequence invariants

### Objective

Evaluate formal verification where ordinary tests struggle with a compact state space and false assurance would be costly.

Candidate properties may include:

- authorization / approval invalidation;
- concurrency ordering;
- stale authority;
- state-machine transition invariants;
- exactly-once or at-most-once behavior where relevant.

A pilot should:

- model one property only;
- identify assumptions/bounds;
- map transitions to current implementation;
- demonstrate that a deliberately broken variant is rejected;
- record the source commit/model relationship;
- distinguish model drift from product defect after later changes.

### Boundary

Do not make formal verification a general SDLC requirement.

---

## EHE-12 — Risk-direct the amount of AI process applied to a change

### Objective

Avoid applying maximum agent machinery to trivial work.

Scale rigor based on:

- consequence;
- reversibility;
- blast radius;
- uncertainty;
- verification difficulty;
- architectural surface area;
- expected lifetime.

Potential progression:

```text
low consequence + easy rollback + strong tests
    → normal implementation + normal review

higher consequence / cross-system / harder to verify
    → stronger runtime verification + independent review

irreversible / authority-changing / difficult to verify
    → blocking approval + stronger gates + bounded autonomy
```

Sampling accepted output can be a process-assurance strategy only when consequence-relevant gates remain strong.

---

# Testing-layer vocabulary

Use explicit language so teams do not treat every form of assurance as "a test."

- **Regression test** — protects known behavior / bug path.
- **Integration / runtime verification** — exercises the real user/system path.
- **Deterministic gate** — machine-enforced invariant or policy.
- **Independent / adversarial review** — semantic challenge from a separate inference or authority boundary.
- **Formal verification** — bounded/exhaustive property reasoning where justified.

Not every change needs every layer.

---

# Recommended pilot sequence

## Phase 1 — Policy and inventory

1. Identify current testing/review requirements in one representative repository.
2. Inventory material deterministic gates and existing real-artifact verification.
3. Identify reusable skills currently duplicated across Claude Code, Cursor and Codex.
4. Identify one or two consequence-relevant verifiers suitable for known-bad sensitivity testing.
5. Identify one repeated AI/manual step that is a candidate deterministic lever.

## Phase 2 — Small pilots

Run bounded pilots for:

- evidence-driven TDD on representative fixes;
- one project-local verification contract;
- one verifier sensitivity / drift-maintenance exercise;
- a small governed skill registry containing only a few proven capabilities;
- one cross-model adversarial review comparison if independent review owners approve.

## Phase 3 — Evaluate before standardizing

Evaluate:

- defect detection quality;
- false positive/noise rate;
- verifier failures that exposed real risk;
- maintenance cost;
- agent-host drift;
- human review burden;
- runtime/token cost;
- rollback/recovery quality;
- whether deterministic tooling replaced repeated probabilistic work.

Do not standardize a mechanism merely because it is technically coherent.

---

# What not to import

Do not adopt as enterprise defaults:

- pstack wholesale;
- MoltenBase as a required product;
- Noodle-style full orchestration without demonstrated scale need;
- universal no-planning;
- universal TDD;
- universal formal verification;
- one agent per event;
- autonomous merge;
- persona-only reviewer independence;
- transcript/history as project truth;
- automatic skill creation from every repeated correction;
- automatic skill updates without review/provenance;
- a skill registry that owns memory or task execution.

---

# Transfer principle

The enterprise target is not "more agents."

It is:

> **A smaller number of well-bounded AI workflows operating inside an environment that has strong executable evidence, deterministic mechanics, portable/versioned capabilities, explicit authority, and independent review where consequence requires it.**

That is the precondition for safely increasing automation later.
