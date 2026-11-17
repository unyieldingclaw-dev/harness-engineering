# Archify — Harness Pattern Deep Evidence Pass — 2026-09-06

## Source

- Repository: https://github.com/tt-a1i/archify
- Owner: `tt-a1i`
- Default branch reviewed: `main`
- Current development version stated by README: `v2.17.0-dev.1`
- Primary artifacts examined:
  - `README.md`
  - `archify/SKILL.md`
  - `DESIGN.md`
  - `benchmarks/ordinary-model-floor/README.md`
  - repository search evidence for delivery, validation, receipts, and contract tests

## Why this source matters

Archify is a visualization/rendering product, but its implementation exposes several harness patterns that generalize beyond diagrams. The important subject is not the renderer. It is the contract around an agent-produced candidate: typed representation, deterministic validation, bounded repair, frozen candidates, independent verification, evidence binding, explicit failure states, and controlled publication.

**Corpus relationship:** this entry is primarily **SHARPENS / ORTHOGONAL TO** existing Harness Engineering research. It provides an independent problem-domain implementation of several patterns already observed in coding-agent and AI-SDLC systems. It should not be counted as wholly independent evidence for every convergent claim because the project may share common agent-tooling conventions and because its product domain is different. The strongest independent signal is where the visualization problem forces explicit separation of semantic, deterministic, browser, and perceptual claims.

---

## 1. System boundary: agent proposes; deterministic machinery owns artifact integrity

Archify's README describes a pipeline in which an agent creates typed JSON IR and Archify deterministically compiles it into HTML/SVG. Its operational path is explicitly separated into Generate, Validate, optional Preview, Deliver, and Iterate. Validation produces machine-readable diagnostics; delivery renders a same-directory candidate and atomically replaces the target only after checks pass.

The skill makes the contract even sharper: a candidate is written before renderer internals are inspected; validation is run after every candidate edit and immediately before handoff; a passing final validation freezes the candidate; delivery is a final acceptance command.

### Harness implication

This is a concrete example of a general boundary:

> **The model owns proposal; deterministic infrastructure owns claims it can mechanically establish.**

The useful abstraction is not JSON specifically. It is a typed, inspectable candidate representation between probabilistic generation and deterministic verification.

**Disposition:** ADOPT AS DESIGN PRINCIPLE / SHARPENS existing execution-and-discipline separation.

---

## 2. Candidate freezing is a first-class integrity control

The Ordinary-Model Floor benchmark freezes the first candidate when the agent invocation ends. Post-hoc edits, including human edits, are prohibited before verification. Correction attempts may be retained for diagnosis, but they cannot replace attempt 1.

The skill similarly says that a passing final validation freezes the candidate and that delivery freezes the exact specification bytes into a private same-directory snapshot before rendering/checking.

### Harness implication

A harness should distinguish:

- candidate produced by the agent;
- candidate subsequently repaired;
- artifact derived from the candidate;
- verified artifact;
- published artifact.

Without this distinction, evaluation can accidentally measure a repaired artifact while reporting it as model output.

This is directly relevant to ACR and to future Harness Engineering evaluations: **evaluation must bind to the exact candidate under test.**

**Disposition:** SHARPENS Karpathy-style frozen-candidate/evaluator separation and existing ACR evidence/provenance direction.

---

## 3. Multiple verification layers are claims-specific, not redundant reviewers

Archify explicitly separates several claims:

1. typed/schema and deterministic artifact validity;
2. browser/runtime evidence from `visual-check`;
3. perceptual visual review by a capable human/image reviewer.

The skill states that `deliver` proves deterministic artifact checks, `visual-check` proves bounded browser behavior, and perceptual review remains a separate claim. The benchmark's first-pass gate requires semantic correctness, deterministic validation, and an identified reviewer with no defects. A skipped visual review can never be upgraded to a pass.

This is stronger than simply saying "use multiple reviewers." Each evaluator has a different epistemic target.

### Harness implication

Reviewer independence should be framed as **independence of claim/evidence path**, not merely number of model calls.

For ACR, useful axes remain things such as:

- deterministic/static checks;
- specification/intent fidelity;
- semantic correctness;
- architecture/relationship correctness;
- adversarial review;
- runtime evidence where applicable.

The Archify evidence suggests that a verification stage should declare what it proves and what it does not prove.

**Disposition:** STRONG SHARPENING of reviewer-independence and evidence-contract research.

---

## 4. Truthful failure taxonomy

The benchmark distinguishes operational failures (`timeout`, `no_candidate`, `provider_error`) from semantic, deterministic-validation, and visual-review failures. If an invocation ends without a candidate, the benchmark preserves the raw operational record and emits a failure receipt; it does not fabricate an invalid candidate to keep the matrix complete.

The report separately aggregates operational, semantic, deterministic-validation, and visual-review failure clusters.

### Harness implication

A harness should not collapse:

`agent could not produce an artifact`

into:

`agent produced an incorrect artifact`.

Those are different failure classes with different remediation and evaluation implications.

This reinforces the Ralphy-derived **Failure Taxonomy** candidate.

**Disposition:** SHARPENS / CONVERGENCE.

---

## 5. Bounded repair loop with an explicit non-improvement stop

Archify does not use an unbounded "keep trying until it works" rule. The skill instructs focused corrections using diagnosed subjects and supported fixes. Continue only while the objective error count reaches a new minimum; if two consecutive rounds fail to improve the best count, stop and report unresolved diagnostics truthfully.

This is a useful bounded optimization contract:

`candidate → diagnose → constrained repair → measure → retain best → stop on non-improvement`.

### Harness implication

This is a more precise form of the goal-loop pattern than a generic autonomous loop. The stop condition is tied to an objective metric, and the repair surface is constrained by diagnostics.

It supports a general design principle:

> **Iteration should be bounded by an explicit objective, improvement rule, and stop condition—not by an assumption that more agent turns imply better output.**

**Disposition:** SHARPENS Ralphy/goal-loop research. Do not adopt Archify's exact two-round threshold as a universal value.

---

## 6. Progressive disclosure is enforced as an authoring boundary

The skill's fast path says to select a diagram type, read one matching schema and example, write the candidate before inspecting renderer internals, and validate. Optional viewer-runtime, authoring-contract, and delivery-contract references are read only when their features are needed.

It explicitly prohibits reading renderer source, validator source, tests, and benchmarks before the first candidate except for specified exceptions.

### Harness implication

This is a concrete implementation of progressive disclosure rather than a documentation slogan.

It reduces startup/context exploration cost and creates a stable authoring contract around the internal machinery. The important lesson for PMB/Harness Engineering is not to hide information arbitrarily, but to expose **the smallest sufficient contract first** and defer internals until a concrete need appears.

**Disposition:** CONVERGENCE with David `effective-agent-skills`, Nate minimum-sufficient-instruction, and Matt context/phase-boundary work.

---

## 7. Typed IR is useful when the output domain has a stable schema

Archify's central representation is typed JSON IR. The schemas define valid fields, enums, topology, and geometry contracts. This gives deterministic validators a stable target and lets the renderer remain downstream from the authored representation.

The broader lesson is:

> **Use a typed intermediate representation when it creates a meaningful verification boundary.**

Do not generalize this into "all harness operations need JSON IR." PMB memory, research prose, and ACR findings have different representations and should not be forced through an artificial universal schema.

**Disposition:** ADOPT AS DESIGN PRINCIPLE, not implementation requirement.

---

## 8. Evidence is explicitly opt-in and revision-pinned

Archify distinguishes ordinary source-free artifacts from evidence-backed artifacts. When repository evidence is requested, nodes can carry `SRC n` markers and open Git-verified files/line ranges pinned to a public commit. The design rules repeatedly require focus, reachability, routes, stories, source links, receipts, and counts to derive from authored or locally verified evidence.

This is unusually relevant to the PMB Filing Contract research.

The pattern is effectively:

`authored fact → verified evidence → derived presentation`

rather than:

`presentation → implied fact`.

It supports the emerging PMB rule:

> **Don't state a value you don't own. Point at the owner, or carry a test that binds you to it. Measurements own what they stamp.**

Archify does not prove that wording is correct for PMB, but it provides a concrete external example of the underlying discipline: derived displays should not silently manufacture factual authority.

**Disposition:** SHARPENS PMB Durable State / Authority / Projection research.

---

## 9. Architecture Delta is more interesting than the diagram renderer

Archify compares two validated architecture snapshots as Before / Delta / After and classifies exact authored changes such as added, removed, changed, moved, and rerouted facts. Its README describes a machine receipt for the comparison.

The potentially general Harness Engineering pattern is:

`approved/known baseline → implementation candidate → normalized representation → exact delta → verification`

This is materially different from ordinary code diff review. A source diff answers "which lines changed?" A normalized architecture delta can answer "which declared relationships changed?"

### ACR/Harness candidate

Research whether architecture-delta semantics could become an additional ACR signal:

- baseline architecture/specification;
- extracted post-change architecture;
- normalized relationship delta;
- comparison against intended architecture change;
- deterministic mismatch report.

This should remain research until feasibility and extraction reliability are demonstrated. Do not infer that Archify can reliably extract architecture from arbitrary repositories merely because it supports repository evidence.

**Disposition:** HIGH-VALUE RESEARCH.

---

## 10. Last-good artifact and atomic publication

Archify's preview mode retains the last verified artifact when the current candidate is incomplete or invalid. Delivery renders and checks a same-directory candidate, then atomically replaces the target only after acceptance.

This creates a clean distinction between:

- current candidate;
- last-known-good artifact;
- published artifact.

### Harness implication

This is useful anywhere an agent continuously mutates an artifact that humans may inspect. The viewer should not become a partial-output channel that accidentally presents invalid state as current truth.

The principle generalizes better than the exact implementation:

> **When a mutable artifact is continuously regenerated, preserve the last verified state until the new state passes its acceptance contract.**

**Disposition:** HIGH-VALUE CANDIDATE for artifact publication patterns; orthogonal to worktree isolation.

---

## 11. Publication is separate from generation and verification

Archify makes `deliver` an explicit final acceptance step and makes opening the result (`--open`) opt-in. It also states that opener failure does not invalidate the artifact's successful delivery.

This creates separate concerns:

`generate → verify → publish → present`

A failure of presentation does not rewrite the truth of artifact verification.

### Harness implication

This reinforces the broader distinction between:

- execution authority;
- verification authority;
- publication authority;
- presentation/UI actions.

This maps strongly to the existing Harness Engineering separation between model capability and execution authority.

**Disposition:** SHARPENS Execution Profile / Publication Authority candidates.

---

## 12. Contract tests bind prose to implementation

Repository search shows dedicated tests for delivery contracts, preview contracts, authoring safety, and skill metadata. The delivery-contract test reads both the Skill and delivery contract and checks their relationship rather than relying solely on documentation.

This is significant because it is exactly the problem observed in PMB: prose facts can drift while mechanized facts remain correct.

Archify demonstrates a useful mitigation:

> **When a behavioral contract is represented in prose and code, test the relationship between them where practical.**

This is stronger than asking contributors to keep documentation synchronized manually.

**Disposition:** HIGH-VALUE CONVERGENCE with PMB's measured prose-drift findings.

---

## 13. Benchmark design: evidence eligibility is itself a gate

The Ordinary-Model Floor benchmark does not merely score candidates. It verifies that the benchmark run itself is complete and valid.

Evidence eligibility requires every configuration to have exactly one valid attempt-1 receipt for every manifest case. The benchmark harness is outside the model-visible tree; the external runner owns authentication, model selection, timeouts, prompt delivery, and transcript retention.

This yields two layers of validity:

1. **candidate validity** — did the artifact pass the product's acceptance gates?
2. **experiment validity** — was the measurement itself conducted under the required protocol?

That distinction is important for Harness Engineering evals. A good result from an invalid experiment should not become evidence.

**Disposition:** SHARPENS evaluation/provenance discipline from Karpathy and Anthropic research.

---

## 14. Reference fixtures are explicitly not benchmark evidence

Archify's benchmark repository contains reference fixtures used to prove that the suite and verifier are wired correctly, but explicitly says those fixtures are not benchmark evidence and must not be published as model results.

This is a small but valuable provenance rule: **test infrastructure can prove the evaluator works without being confused with evidence about the evaluated system.**

This maps to ACR and Harness Engineering tests: fixtures demonstrate gate behavior; they do not demonstrate model quality or real-world effectiveness.

**Disposition:** SHARPENS evidence provenance.

---

## 15. Semantic aliases do not replace topology

The benchmark permits accepted vocabulary aliases where multiple technical labels are legitimate, but still requires every required node to bind once and every required relationship to exist in the declared direction.

This is a useful general pattern:

> **Normalize surface vocabulary without weakening structural requirements.**

For ACR, this is analogous to allowing equivalent implementation terminology while retaining exact requirements around behavior, interfaces, dependency direction, or policy boundaries.

**Disposition:** ORTHOGONAL / useful verifier design pattern.

---

## 16. What Archify does NOT establish

This source should not be over-read.

- It does not establish that typed JSON IR is universally superior to other representations.
- It does not establish that two repair rounds is an optimal universal bound.
- It does not establish that visual review should be an ACR gate; its product domain specifically requires visual correctness.
- It does not establish that architecture extraction from arbitrary codebases is complete or reliable.
- It does not establish that Archify's skill should be installed into PMB or Harness Engineering.
- It does not establish a need for a new PMB subsystem, directory, index, metadata schema, or startup-loaded artifact.
- Its benchmark results are a small, dated sample; they should be treated as diagnostic evidence rather than broad model-performance claims.

---

## 17. Corpus mapping

| Archify observation | Existing corpus relationship | Disposition |
|---|---|---|
| Agent → typed candidate → deterministic validation | Execution/discipline separation; ACR evidence contract | SHARPENS |
| Frozen attempt-1 candidate | Karpathy fixed candidate/evaluator | SHARPENS |
| Claim-specific independent verification | Reviewer independence; ACR orthogonal review | STRONG CONVERGENCE |
| Operational vs semantic failure clusters | Ralphy Failure Taxonomy | SHARPENS |
| Bounded non-improvement repair loop | Ralphy goal-loop | SHARPENS |
| Progressive disclosure | David effective-agent-skills; Nate minimum sufficient instruction | CONVERGENCE |
| Revision-pinned source evidence | PMB authority/provenance research | SHARPENS |
| Last-good artifact | Publication/verification boundary | HIGH-VALUE CANDIDATE |
| Atomic publication | Publication Authority / execution boundary | SHARPENS |
| Architecture Before/Delta/After | ACR architecture-drift concern | HIGH-VALUE RESEARCH |
| Contract tests for Skill ↔ contract | PMB prose-drift findings | HIGH-VALUE CONVERGENCE |
| Benchmark evidence eligibility | Karpathy/Anthropic eval discipline | SHARPENS |
| Reference fixtures excluded from evidence | Evidence provenance | SHARPENS |
| Semantic aliases vs exact topology | Verifier design | ORTHOGONAL |

---

## 18. Proposed follow-up research questions

1. **Architecture delta feasibility:** Can a repository's architecture be extracted with sufficient determinism and confidence to support an ACR baseline-vs-result check?
2. **Candidate binding:** What minimum provenance fields are required to bind a review finding to the exact candidate/spec/evaluator version under test?
3. **Contract binding:** Which PMB countable/prose-maintained facts can be cheaply tested against their actual authority without creating a new startup context burden?
4. **Last-good publication:** Where, if anywhere, does PMB or ACR expose mutable intermediate artifacts to humans such that last-known-good semantics would materially reduce risk?
5. **Failure taxonomy:** Can existing ACR failures be separated cleanly into execution, deterministic, semantic, evidence, and publication classes?

---

## Bottom line

Archify is not primarily valuable to Harness Engineering because it makes better architecture diagrams.

It is valuable because it is a concrete, unusually explicit implementation of a broader pattern:

**probabilistic proposal → typed candidate → deterministic verification → bounded repair → frozen candidate → independent claim-specific verification → controlled publication → provenance**

The strongest new research targets are **Architecture Delta**, **last-known-good publication**, and **contract tests that bind operational prose to implementation**.

The correct response is to mine these patterns into the existing corpus—not to install Archify, copy its Skill, or create new PMB architecture around it.
