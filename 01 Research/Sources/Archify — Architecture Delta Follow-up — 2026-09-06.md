# Archify — Architecture Delta Follow-up — 2026-09-06

## Research question

Can Archify's Before / Delta / After architecture comparison become a useful ACR signal for detecting architecture drift between an approved architecture and an implementation change?

## Short answer

**Yes as a comparison pattern; not yet as a direct implementation for ACR.**

The important correction from the first Archify pass is that Archify's current Architecture Delta is a **typed-architecture-model comparator**, not a general code-to-architecture extractor. It compares two validated Archify architecture snapshots, requires stable authored IDs, normalizes the snapshots, and classifies authored changes such as added, removed, changed, moved, and rerouted relationships. It can attach revision-pinned provenance when both sides have verified repository evidence.

That is valuable, but it does not by itself answer the harder ACR question:

> Given an approved architecture and a changed repository, can we deterministically establish which architectural relationships the code change actually introduced or removed?

That second problem requires a code-to-architecture mapping/evidence layer. Archify explicitly warns that authored graph reachability must not be called runtime impact, blast radius, or breakage without independent code-analysis evidence. urlArchify DESIGN.mdhttps://github.com/tt-a1i/archify/blob/c6519401f7b91b9d43011657880893b0a8955548/DESIGN.md

**Disposition: HIGH-VALUE RESEARCH, with a narrower candidate boundary than the original proposal.**

---

## 1. What Archify actually proves

Archify's comparator requires both inputs to be `schema_version: 1` architecture diagrams and `diagram_type: architecture`. Components and connections require authored stable IDs for comparison; the comparator fails when there is no shared component ID because it cannot prove that the two snapshots describe the same system. It then canonicalizes the models and classifies component, connection, and boundary changes.

The comparator explicitly separates classifications:

- component semantic changes;
- component evidence changes;
- component geometry changes;
- connection topology changes;
- connection semantic changes;
- connection geometry/routing changes;
- boundary scope/geometry changes;
- presentation changes;
- provenance changes.

The result also carries comparator/canonical versions, completeness, proof level, identity rules, and limitations. urlArchify architecture-delta.mjshttps://github.com/tt-a1i/archify/blob/c6519401f7b91b9d43011657880893b0a8955548/archify/delta/architecture-delta.mjs

This is a strong **model-delta primitive**.

It is not yet an implementation-delta primitive.

---

## 2. The key distinction: model delta vs implementation delta

There are two different problems that are easy to conflate:

### A. Model delta

```text
Architecture snapshot A
        ↓
Architecture snapshot B
        ↓
Canonicalize
        ↓
Exact structural delta
```

Archify does this well.

### B. Implementation delta

```text
Approved architecture
        ↓
Changed repository
        ↓
Extract structural evidence from code/config
        ↓
Map evidence to architecture components
        ↓
Normalize
        ↓
Compare intended vs observed architectural change
```

This is the hard part.

A source diff is not an architecture diff. A new import may represent an implementation detail, a new architectural dependency, or a false positive caused by a shared library. Conversely, an architectural relationship can change through configuration, runtime wiring, generated code, HTTP/API contracts, messaging, or infrastructure without a simple source import showing it.

Therefore ACR should not initially attempt to infer a complete architecture graph from arbitrary repositories.

---

## 3. Independent evidence strongly supports the harder problem's difficulty

The literature on static architecture conformance checking distinguishes planned architecture from implemented architecture and identifies mapping between them as a central problem. A 2017 empirical study of ten dependency-analysis tools found substantial variation in dependency detection accuracy; on its benchmark the tools reported, on average, 77% of dependencies, while reporting within the FreeMind case averaged 72%. The study identified difficult dependency types and detection challenges. citeturn1search5

A later CMU state-of-the-art summary similarly describes static analysis, architecture-specific tests/fitness functions, and dependency-structure analysis as useful but requiring tuning; it also notes that conformance tests can become brittle as architecture and implementation evolve. citeturn1search47

**Implication:** whole-repository architecture extraction should not be treated as a deterministic oracle merely because a tool produces a graph.

---

## 4. Existing tools reveal a more feasible boundary

### Structurizr

Structurizr deliberately treats the architecture model as code and renders multiple views from one model. Its documentation explicitly identifies AI-assisted drift detection as a possible use case: an agent can compare code or infrastructure against the Structurizr model and raise an alert when they diverge. citeturn0search1turn0search2

The important design choice is that Structurizr is a **target architecture model**, not an automatically inferred source of truth.

### dependency-cruiser

dependency-cruiser demonstrates a mature deterministic enforcement layer for source dependency relationships. It supports forbidden, allowed, required, reachability, circularity, and other dependency rules, with severity and explanatory comments. citeturn0search0turn0search3

This suggests a stronger ACR design than "ask an LLM whether the architecture changed": use deterministic dependency evidence wherever the language/toolchain supports it, then use semantic reasoning only where deterministic analysis cannot establish the claim.

### Erode

Erode is particularly close to the proposed ACR use case. Its repository describes comparing pull-request or local Git diffs against LikeC4 or Structurizr models to surface undeclared dependencies and structural changes. Its pipeline resolves architecture components, scans dependency changes, analyzes them against the model, and can propose model updates. fileciteturn435file0

This is useful corroboration, but it also confirms that an explicit **code-change → architecture-component mapping** layer is necessary.

---

## 5. ACR should therefore use a layered architecture-drift signal

The research suggests this narrower design:

```text
                 APPROVED INTENT / ARCHITECTURE
                              │
                              ▼
                    Declared architecture
                              │
                 ┌────────────┴────────────┐
                 │                         │
                 ▼                         ▼
       Deterministic source evidence   Existing mappings
       imports / dependencies /        component ownership /
       config / API edges              service boundaries
                 │                         │
                 └────────────┬────────────┘
                              ▼
                    Observed change set
                              │
                              ▼
                   Architecture comparison
                              │
                  ┌───────────┴───────────┐
                  ▼                       ▼
           Deterministic rule         Semantic question
             violation?              "Is this intended?"
                  │                       │
                  └───────────┬───────────┘
                              ▼
                       ACR finding
```

The important boundary is:

> **Deterministic evidence establishes what changed structurally; semantic review establishes whether that structural change is consistent with the approved intent.**

That is much safer than asking an LLM to reconstruct the entire architecture and then trusting its graph.

---

## 6. The minimum viable ACR experiment

Do not build a general architecture extractor.

Test one narrow class of architectural relationship where deterministic evidence is reasonably strong.

For a TypeScript repository, for example:

1. Define 3–5 architectural components.
2. Define allowed/forbidden component dependencies.
3. Define an explicit mapping from source directories/packages to components.
4. Create baseline dependency evidence.
5. Create PRs that intentionally introduce:
   - an allowed dependency;
   - a forbidden dependency;
   - an unrelated refactor;
   - a dependency that looks suspicious but is semantically legitimate;
   - a change that modifies the approved architecture intentionally.
6. Run deterministic extraction on base and head.
7. Produce the normalized relationship delta.
8. Compare the delta to the declared architecture change.
9. Give an independent semantic reviewer only the relevant evidence and intended change.
10. Measure false positives, false negatives, unsupported cases, and reviewer disagreement.

This would tell us whether the signal is useful before creating any permanent ACR architecture feature.

---

## 7. What the comparator should report

A useful architecture-delta receipt should distinguish at least:

- `added-relationship`
- `removed-relationship`
- `changed-target`
- `changed-boundary`
- `new-component`
- `removed-component`
- `unmapped-change`
- `evidence-incomplete`
- `intentionally-declared-change`
- `undocumented-change`

The last two are not deterministic facts unless the approved intent/architecture explicitly declares them.

This is another application of the emerging Harness Engineering rule:

> **Do not turn an inference into a fact merely because a tool produced a value.**

The receipt should expose evidence quality separately from architectural judgment.

---

## 8. Provenance becomes essential

Archify's model comparator already records raw and semantic hashes, byte counts, repository revisions, comparator version, canonicalization version, completeness, and proof level when evidence is available. urlArchify architecture-delta.mjshttps://github.com/tt-a1i/archify/blob/c6519401f7b91b9d43011657880893b0a8955548/archify/delta/architecture-delta.mjs

That is a useful provenance model for ACR.

An architecture finding should ultimately be bindable to:

```text
source revision
+ candidate diff
+ extraction/tool version
+ architecture model revision
+ mapping/rules revision
+ deterministic evidence
+ semantic reviewer evidence
```

Without that binding, an architecture finding can become impossible to reproduce or audit.

This is stronger evidence for the existing ACR Evidence Contract candidate than evidence for a new architecture subsystem.

---

## 9. Architecture delta should not replace architecture review

There are three distinct questions:

1. **Did the code introduce a structural relationship?**
   - deterministic analysis where possible.
2. **Is that relationship allowed by the architecture?**
   - deterministic conformance rules where expressible.
3. **Was the architectural change intentional and appropriate?**
   - specification/intent review and human judgment.

No single architecture-delta tool should be expected to answer all three.

This maps directly onto the ACR distinction between deterministic gates, orthogonal semantic review, and intent/specification fidelity.

---

## 10. What we should reject

Do not adopt any of these conclusions from the research:

- "Generate a full architecture graph with an LLM and compare it to the baseline."
- "Architecture diagrams are deterministic evidence."
- "Any new dependency is architecture drift."
- "A graph extraction tool's confidence score is equivalent to verification."
- "Archify's model comparator proves code-level architecture drift."
- "Architecture drift can be reduced to one scalar score."

The evidence points toward **bounded, claim-specific architectural conformance checks**, not an architecture oracle.

---

## 11. Revised research disposition

### Archify Architecture Delta

**KEEP — HIGH-VALUE RESEARCH.**

Archify contributes a strong deterministic **model-delta** primitive and useful provenance/normalization patterns.

### Code-to-Architecture Extraction

**PRIORITY RESEARCH, not architecture adoption.**

The independent evidence shows this is feasible in bounded domains but not reliable enough to treat as a universal deterministic truth source.

### ACR Architecture Drift Signal

**CANDIDATE EXPERIMENT.**

Test a narrow dependency/conformance class first. Do not build a general architecture extractor.

### Erode / Structurizr / dependency-cruiser

**RELATED PRIOR ART.**

They should be recorded as separate corpus entries if this research direction survives the experiment, because they represent materially different approaches:

- Structurizr: declared target architecture model;
- dependency-cruiser: deterministic dependency-rule enforcement;
- Erode: AI-assisted code-diff-to-architecture-model comparison;
- Archify: deterministic architecture-model delta and evidence/presentation contract.

---

## 12. Corpus relationships

| Source / finding | Relationship | Reason |
|---|---|---|
| Archify Architecture Delta | SHARPENS | Concrete deterministic model-delta implementation |
| Karpathy fixed evaluator/candidate | SHARPENS | Exact candidate/provenance discipline |
| ACR Evidence Contract | SHARPENS | Architecture findings need exact evidence binding |
| ACR architecture-drift concern | EXTENDS | Adds normalized relationship-delta concept |
| dependency-cruiser | ORTHOGONAL / PRIOR ART | Deterministic source dependency conformance |
| Structurizr | ORTHOGONAL / PRIOR ART | Declared architecture model as code |
| Erode | ORTHOGONAL / PRIOR ART | Code-diff → architecture-model analysis |
| PMB Durable State / Authority / Projection | SHARPENS | Model delta is derived state; authority must remain explicit |

---

## Sources

1. Archify repository: https://github.com/tt-a1i/archify
2. Archify architecture delta implementation: https://github.com/tt-a1i/archify/blob/c6519401f7b91b9d43011657880893b0a8955548/archify/delta/architecture-delta.mjs
3. Archify design system / evidence rules: https://github.com/tt-a1i/archify/blob/c6519401f7b91b9d43011657880893b0a8955548/DESIGN.md
4. Archify delivery contract: https://github.com/tt-a1i/archify/blob/main/archify/references/delivery-contract.md
5. Erode repository: https://github.com/erode-app/erode
6. Structurizr documentation: https://docs.structurizr.com/
7. dependency-cruiser: https://github.com/sverweij/dependency-cruiser
8. Pruijt, "The accuracy of dependency analysis in static architecture compliance checking": https://onlinelibrary.wiley.com/doi/full/10.1002/spe.2421
9. CMU SEI, "Why Architecture": https://insights.sei.cmu.edu/documents/4504/2022_017_001_973555.pdf

## Bottom line

The Archify follow-up makes the original idea better by narrowing it.

**The opportunity is not "ACR automatically understands the architecture."**

It is:

> **Use deterministic structural evidence to compute a bounded architecture delta, then use independent semantic review to determine whether that delta matches the approved intent.**

That is a tractable Harness Engineering research direction. It preserves the separation we keep finding across the corpus: **facts/evidence first, inference second, judgment last.**
