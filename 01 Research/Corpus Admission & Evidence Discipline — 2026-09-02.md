# Harness Engineering Research Corpus — Admission & Evidence Discipline

**Date:** 2026-09-02

## Why this exists

The research corpus is beginning to accumulate enough material that discovery itself can become a source of architecture bloat. The corpus therefore needs an admission discipline, not merely a collection mechanism.

The existing research philosophy already says evidence should precede architecture and that research findings should be independently evaluated before influencing design. This document tightens that rule based on recent review experience.

## 1. Verified evidence must be distinguished from synthesis

A source-derived fact, an interpretation, and a cross-source synthesis are different evidence classes.

A finding must identify which claims were directly inspected and which were inferred from a source inventory, surrounding context, or cross-source comparison.

A plausible synthesis must not be recorded as a verified source finding until the underlying artifacts have been inspected.

This matters because recent review rounds demonstrated that claims that are merely "probably right" can survive multiple reviewers while still being false.

## 2. Convergence is not a source count

Do not treat `≥2 sources` as sufficient corroboration.

Multiple sources can share the same assumptions, discourse lineage, examples, or copied techniques. Agreement under a shared premise is not independent evidence.

Prefer one of these stronger forms of convergence:

### Independent provenance

Evidence originates in materially different problem domains, technical disciplines, incentives, or institutional contexts.

Examples:

- ML training-loop evaluation constraints and infrastructure sandboxing;
- independent product/platform design and practitioner workflow design.

### Independent measurement

The claim is supported by a reproducible experiment, observed operational result, deterministic test, benchmark, or other measurement whose outcome does not depend on the source's assertion alone.

### Weak convergence

Multiple sources from the same practitioner discourse may still be useful corroboration, but should be treated as one evidentiary lineage unless independence is established.

**Rule:** source count is metadata; evidentiary independence is the admission criterion.

## 3. Every corpus entry must map to existing knowledge

Before admitting a new finding, identify its relationship to the existing corpus:

- `SUPERSEDES` — replaces an older finding or decision;
- `SHARPENS` — adds precision, mechanism, boundary, or evidence to an existing finding;
- `MERGES WITH` — combines overlapping findings into a stronger canonical entry;
- `ORTHOGONAL TO` — addresses a materially distinct concern;
- `NO EXISTING MATCH` — genuinely new territory, with justification.

"No existing match" should be the exception, not the default.

This prevents the corpus from turning every useful refinement into another permanent principle.

Examples from the current research:

- Sandboxing/capability isolation SHARPENS the artifact-scoped authority work rather than creating an unrelated authority principle.
- Specification/intent review SHARPENS `[NS-39]`.
- Canonical vocabulary SHARPENS `[NS-42]`.

## 4. Context-bearing mechanisms require a cost gate

A proposed canonical vocabulary or context layer is not automatically beneficial merely because it reduces duplication in downstream prose.

Its own startup context is a cost and must be included in the evaluation.

For PMB, the relevant constraint is especially concrete: the startup-context ratchet rejects increases in the measured startup context, while `[NS-42]` identifies write volume as the underlying memory-growth constraint.

Therefore a vocabulary/context mechanism should not be adopted until it demonstrates a **net benefit** against the relevant baseline.

For the current CONTEXT.md hypothesis, the proposed measurement is:

- establish the pre-change memory-write baseline, including the observed `progress.md` growth recorded by `[NS-42]`;
- measure the vocabulary file's startup-context cost;
- measure reduction in duplicated explanatory content and subsequent write volume;
- evaluate whether the reduction exceeds the added startup cost and maintenance burden;
- reject or redesign if the mechanism merely moves context from one file to another.

This is an **admission precondition**, not a follow-up optimization.

## 5. Research admission should be harder than discovery

A finding should enter the durable corpus only when it has:

1. source provenance;
2. verified-vs-inferred status;
3. relationship to existing corpus entries;
4. an evidence-independence assessment;
5. a bounded disposition;
6. a demonstrated reason to retain it.

For measurable claims, include the measurement method or identify the experiment required before adoption.

Discovery can remain broad. Durable corpus growth must be selective.

## 6. Disposition taxonomy

Use:

- `ADOPT` — incorporated into a documented principle or decision;
- `ASSESS` — routed to HE-001 or another formal assessment;
- `PARK` — retained as relevant research without current implementation justification;
- `REJECT` — explicitly determined insufficient, inapplicable, or contradicted;
- `GAP` — demonstrates a missing capability or unanswered architectural question;
- `SUPERSEDE` — replaces an existing finding or decision;
- `MERGE` — consolidates overlapping findings into a canonical entry.

`SUPERSEDE` and `MERGE` are corpus operations, not merely dispositions: the older material must be updated, redirected, or retired so the corpus does not retain competing near-duplicates.

## 7. Governance complexity is itself an evaluation signal

The attached PMB project review warns that governance/process complexity is beginning to accumulate faster than operational value, with the risk that governance becomes the product rather than supporting it. fileciteturn389file0

This should not trigger an immediate architectural response. It should become a standing evaluation criterion:

> Does this proposed Harness mechanism remove demonstrated operational friction, or does it primarily add governance to manage governance?

Every new durable process should justify its own complexity.

## 8. Operational artifacts belong to the same discipline

The project review also identified accidental operational artifacts under `.claude/worktrees/...` as a repository hygiene concern. fileciteturn389file0

Treat this as evidence for the existing isolation/working-tree research, not as a reason to invent a new repository subsystem without inspection.

## 9. Research workflow

```text
Discover
  ↓
Inspect source
  ↓
Separate fact / inference / synthesis
  ↓
Assess evidentiary independence
  ↓
Map to existing corpus
  ↓
Measure where measurable
  ↓
Assign disposition
  ↓
Admit / sharpen / merge / supersede / park / reject
```

The corpus should optimize for **durable understanding**, not maximum coverage.

## Relationship to existing Harness research

- **Evidence Before Architecture:** strengthened with verified-vs-inferred provenance and independence testing.
- **Independent Verification:** extended from implementation review to research corroboration.
- **Progressive Disclosure / Context Cost:** reinforced by the requirement that any canonical context layer pay for its own startup cost.
- **Continuous Improvement:** bounded by admission and merge/supersede discipline.
- **Single Ownership:** reinforced by requiring each finding to have a canonical home rather than accumulating duplicates.

## Non-goals

- Do not create a new automated governance engine solely because this policy exists.
- Do not require multiple sources when one independent measurement is stronger evidence.
- Do not treat source popularity, install counts, or repeated practitioner agreement as technical validation.
- Do not automatically expand HE-001 with every research observation.
- Do not turn every refinement into a new principle.
