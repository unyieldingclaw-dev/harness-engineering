# ECC — Selective Skill Delivery, Unknown Health & Evidence Boundaries — 2026-10-22

## Purpose and decision boundary

Distill durable HE findings from an 88-second user-supplied video and a **code-level inspection** of `affaan-m/ECC` pinned at `90606c8315ee09cafc303515c5f33024db6b9882`.

**Primary evidence:** [The Next New Thing AI — ECC Skill Pack and Repo Evidence — 2026-10-22](Sources/The%20Next%20New%20Thing%20AI%20%E2%80%94%20ECC%20Skill%20Pack%20and%20Repo%20Evidence%20%E2%80%94%202026-10-22.md). That note distinguishes VIDEO-OBSERVED claims, verified source code, unexecuted tests and unverified production behavior.

**Disposition:** **MINE** two concrete extensions to existing HE assessment criteria; **ASSESS** one bounded cross-harness capability-delivery pilot; **REINFORCE** existing eval/continuation/skill boundaries; **PARK** full ECC and autonomous learning. **No runtime adoption or HE-002 architecture change is approved by this research.**

## Executive finding

ECC's biggest contribution to HE is **not** “261K stars,” a bigger roster of skills, or a new standard plan→implement→review prompt. HE already has the conceptual workflow.

The code shows a more interesting set of *contracts*:

1. **Capability installation should be auditable across its full lifecycle:** requested, resolved, file-plan previewed, installed, fingerprinted/owned, diagnosed, repaired and removed without destroying user-owned state.
2. **Capability and telemetry absence are first-class states:** an adapter can be unsupported, unverified or stale; a skill with no runs is **UNMEASURED**, not healthy.
3. **Evidence must not pretend to be authority:** an offline-verifiable execution receipt confirms protected bytes and linkage, not correct execution. A quality gate requiring unavailable isolation should remain unavailable.

These are sharper, directly demonstrated mechanisms for principles HE already holds: Single Ownership, Assessment Before Architecture, Behavioral Skill Evaluation, source-owned observability, versioned capabilities, and Evidence Before Architecture.

## Findings and proposed dispositions

| Finding, with source-level basis | Existing HE owner | Net new value | Disposition |
| --- | --- | --- | --- |
| ECC installer plan and install state: [`install-plan.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/install-plan.js), [`install-state.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/install-state.js) | `Harness Portability, Exit Cost & Inspectability`; enterprise **EHE-05** cross-agent skill management | Make **install-state receipts and exact removal ownership** explicit *acceptance evidence*, not just conceptual capability registry | **MINE / ASSESS** small pilot |
| Hosted capability matrix vs native Codex hook mismatch: [`harness-adapter-compliance.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/harness-adapter-compliance.js), [`codex-hooks.json`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/hooks/codex-hooks.json), [`codex-native-hooks.test.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/tests/codex-native-hooks.test.js) | `Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation`; HE-001 distribution map | Separate **declared / code-shipped / fixture-tested / real-host-verified** and attach verification freshness to each capability | **MINE** missing matrix semantics |
| No data ≠ healthy: [`health.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/skill-evolution/health.js) comments identify upstream #2463 false green | `Experience-Derived Harness Evolution`; HE observability and skill evaluation | Make **UNMEASURED / UNKNOWN** a deliberate acceptance and dashboard state, with tests for absent recorder | **MINE NOW** as evidence criterion |
| Learned/imported skill provenance plus local runs: [`provenance.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/skill-evolution/provenance.js), [`tracker.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/skill-evolution/tracker.js) | HE Skills lifecycle; EHE-05 | Concrete minimal metadata: source, author, version/hash, recorded outcome, correction, scope, observation coverage | **REINFORCE**, adapt selectively |
| Hash-linked eval capsule and receipts, runner isolation disabled: [`capsule.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/capsule.js), [`receipt.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/receipt.js), [`gate.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/gate.js) | `Artifact-Gated SDLC, Behavioral Evals & Metrics`; `Source Preservation & Evidence Durability` | Explicit **integrity vs authenticity vs acceptance** separation, refusal when assurance cannot be established | **REINFORCE / ASSESS** after real receipt consumer |
| Dependency-free skill metadata ranking: [`context-retrieval.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/context-retrieval.js) | `Derived Index Economics`; `Skill Routing, Gate-Bound Evidence & Orthogonal Review` | Simple, deterministic lexical + character morphology with score attribution is testable without embeddings | **ASSESS** only if a real skill discovery miss occurs |
| Project-scoped, confidence-scored instincts and cross-project candidates: [`instinct-cli.py`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/skills/continuous-learning-v2/scripts/instinct-cli.py) | `Experience-Derived Harness Evolution` | Reinforces project isolation but risks promoting frequent correlation into global behavior; normal promotion asks for y/N, `--force` bypasses | **REJECT** automatic rule/skill admission; **PARK** learning runtime |
| Native Windows observer caveat: [continuous-learning SKILL](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/skills/continuous-learning-v2/SKILL.md) | HE-001 runtime fitness / host parity | A nominally successful hook start may leave no running observer under native Windows; test real lifecycle, not process spawn status | **MINE** negative test case |
| Third-party cross-provider reviewer: [`ASTRA-REVIEW-GUIDE.md`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/docs/ASTRA-REVIEW-GUIDE.md) | ACR independent review; `Risk-Directed Review & Release Proof` | Fresh context and explicit export consent reinforce privacy-aware independent review; no proof of superior accuracy | **REINFORCE**, no ACR replacement |

### 1. Proposed small extension to HE's evidence vocabulary

**Add to future HE assessments and EHE-05 pilots; do not require a new global metadata schema before a consumer exists:**

`capability support` needs separate slots:

- **declared** — README/manifest promises support;
- **shipped** — the adapter/skill/hook actually exists in the pinned source;
- **fixture-tested** — a scoped, known-revision test exercises the adapter contract;
- **observed-in-host** — a test in the actual installed harness proves the required event and behavior;
- **unsupported / unverified / stale** — explicit non-success states with a reason and last verification evidence.

**Concrete ECC counterexample:** its Codex support matrix remains “Instruction-backed” from 2026-05-12 while current source includes a native `SessionStart` plugin hook and an integration test. The proper HE response is **not** “Codex parity is complete”; it is “claims and source diverge—measure the installed current host.”

Acceptance: where a capability is consequential, a mismatched classification, missing verified host, or expired evidence must become visible. Never convert unknown to “supported” merely because a file exists.

### 2. Proposed small extension to skill/verification health

**Assessment invariant:**

> A skill, hook, review gate, or evaluator with **no valid outcome records** is not healthy. It is **unmeasured** until the telemetry producer and outcome semantics have been tested.

Required negative fixture for any future implementation:

- Disable/omit the event writer while keeping the dashboard/report renderer running. **Expected:** UNMEASURED / UNKNOWN, **not** success/green.
- Inject one failed and one corrected run. Confirm that “command executed” and “accepted verified outcome” are not conflated.
- Preserve source/skill version identity to prevent a successful old version disguising an untested new one.

Don't build an HE dashboard merely to display this. Add the invariant to the real evaluation and deployment gate when such a consumer is chosen.

### 3. Proposed EHE-05 pilot refinement — *only if enterprise owner accepts*

The enterprise [EHE-05 skill governance candidate](../04%20Work%20Transfer/Enterprise%20Harness%20Engineering%20Track.md.md) already asks for one canonical skill source with host adapters, provenance, drift checks and behavioral verification. ECC makes the missing **distribution lifecycle** specific.

**Stage 0: inventory (read-only).** Take 2–3 already-approved, existing reusable skills across Claude Code/Cursor/Codex in the approved environment. Map canonical owner, version/hash, installed location, manifest/adapter, effective policy, host version, and observed activation behavior.

**Stage 1: print-only operation planning.** For each skill and target host, produce an inspectable proposed file/dependency operation list and warn for unsupported semantics, duplicate hooks, collisions, unmanaged files and dependency drift. **No apply/remove implementation yet.**

**Stage 2: safe fixture installation only if stage 1 reveals repeated problems.** Use a disposable workspace to demonstrate desired add, verify, upgrade, repair and uninstall semantics. Record exact operations and expected file hashes. Simulate an operator-edited target file. **Must not silently overwrite/delete it.** Ensure the installed hook works within a real host, not only as a Node fixture.

**Stage 3: behavior comparison.** Against a control that uses existing manual distribution, compare missed skill activation, unrelated activation, workflow result, install/update drift, human corrections, token overhead, safety violations and maintenance effort. Add at least one held-out task and a negative/mutation case.

**Go/no-go:** if existing scripts, host-native discovery or simple repository links solve it with equal verified reliability, stop without adding an installer product. A registry and install manager should never own canonical project policy, memory or task execution.

This is a **portable methodology proposal**, not a transfer of internal implementation details or authorization to change Work-MB.

### 4. Evaluation/receipt pilot — only after a verified consumer exists

ECC's capsule code has real, useful integrity properties, but its secure candidate execution gate is **not runnable**. This distinction supports HE's existing acceptance principle:

```
source artifact + version
    ↓
executed verifier + independent expected result
    ↓
observed postcondition
    ↓
compact receipt of what ran and what it covered
    ↓
approved acceptance decision (separate authority)
```

A checksum-only receipt must not be described as a quality proof. Do not import ECC's hash-chain framework just because it exists; consider only if an HE/ACR/PMB workflow has a concrete **cross-session or cross-machine evidence-consumption problem**. Real production acceptance and tamper resistance need stronger sources and trust boundaries than a local self-writable hash chain.

## Proposed verification matrix

| Probe | What must be observed | What would falsify the design |
| --- | --- | --- |
| Adapter claim drift | Current installed behavior linked to source ref and host version | Old matrix says “supported” while actual hook silently never fires |
| Install plan preview | Exact target operations with **zero** side effects | Dry run mutates config or omits generated/mirrored artifacts |
| Ownership-aware repair/uninstall | Removes only managed files; edited/operator-owned target preserved | Silent deletion/overwrite of unrelated or changed file |
| Telemetry absence | No measurements produce UNKNOWN / UNMEASURED | No writer + green “healthy” status |
| Skill evaluation | Fresh-run result tied to skill hash + independent criterion | Old success rate carried into changed skill without new evidence |
| Negative evaluator | Known bad case fails for the intended reason | Mutated violation still gets a PASS |
| Scope isolation | Project A's observation does not become B's rule | Automatic global policy mutation after frequent observations |
| Host startup/compaction | Actual host invokes supported hooks and captures expected context | Unit test passes but user-installed host drops event |
| Evidence receipt | Missing artifact, altered receipt, or wrong authority produces explicit failure | Digest is treated as proof of acceptance, or gate claims isolation it lacks |

**Cost cap:** start with the *smallest existing mechanism*. Do not turn these into a permanent checklist for every trivial edit; apply them where incorrect host behavior, irreversible side effects or false assurance justify them.

## Existing HE coverage — what *not* to duplicate

Already well established:

- `01 Research/Skill Routing, Gate-Bound Evidence & Orthogonal Review — 2026-09-26.md`: progressive disclosure, routing misses, source/version identity, behavioral skill evaluation.
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`: observations are evidence rather than policy; human-governed learning; avoid automatic self-modification.
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`: canonical skill owner, per-host adapter, drift and exit cost.
- `01 Research/Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation — 2026-09-25.md`: effective authority must remain monotonic across trust layers; copying Skill text does not prove host parity.
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`: independent acceptance, actual postconditions, mutation/verifier sensitivity.
- `02 Assessments/HE-001 — Baseline Harness Assessment.md.md`: still **IN PROGRESS**; distribution and capability evaluation already on scope.
- `03 Decisions/Decisions Index.md.md`: **HE-002 pending HE-001**; nothing about ECC resolves that gate.

Avoid a second general-purpose “always plan/test/review” rule set, duplicate PMB memory store, additional reviewer harness, agent factory, mandatory memory observer, or ECC-derived deployment/control-plane product. These capabilities already have owners or lack a verified need.

## Priority and action summary

1. **Now, in HE:** preserve this research and integrate the two discriminating tests (actual hosted capability support and no-data health) into future evidence reviews **without creating more permanent instructions**.
2. **Next, if the EHE-05 owner accepts and approved access is available:** execute an **inventory/read-only operation-planning pilot** for 2–3 skills in their own environment; do not copy enterprise data into public HE.
3. **Later, if HE-001 finds a concrete evaluation-consumer need:** compare lightweight verifiable receipts and simple host-native scripts before exploring ECC's capsule machinery.
4. **Park:** automated instinct promotion, generic skill factory, full ECC install, full runtime/agent package, GUI/dashboard, new database or MCP surface.
5. **Reject:** interpreting ECC's popularity as quality, interpreting fixture-tested adapter presence as full host parity, or claiming a disabled `gate run` is a working secure execution gate.

## Evidence status

**MINE/REINFORCE is a research disposition only.** No ECC tests or PMB/ACR/Work-MB runtime evaluations were executed. Nothing in this file promotes a project-governing standard or commits HE to a new product. Approval and real-workload evidence remain prerequisites for implementation.
