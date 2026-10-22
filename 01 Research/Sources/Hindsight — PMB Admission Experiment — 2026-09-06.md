# Hindsight — PMB Admission Experiment — 2026-09-06

## Purpose

Translate the strongest Hindsight pattern into a bounded PMB experiment without adopting its memory layout or granting autonomous canonical-memory mutation.

## Hypothesis

A retrospective process that distinguishes **observed/verified events** from **proved durable lessons** can improve PMB memory quality while reducing duplication and repeated rediscovery, provided memory admission remains governed and net context cost is measured.

## Experimental pipeline

```text
Completed session
      ↓
Evidence extraction
      ↓
Root-cause candidates
      ↓
Durability test
      ↓
Proved / suggested classification
      ↓
Existing-topic match
      ↓
Candidate memory delta
      ↓
Human approval
      ↓
Canonical PMB mutation
```

## Required evidence

Every candidate lesson must identify:

- the concrete session event(s) supporting it;
- the correction/dead-end that exposed the issue;
- the proposed upstream cause;
- the preventive behavior;
- whether the lesson was repeated, causally confirmed, or fix-verified;
- the existing PMB topic it should extend, if any;
- the expected context/retrieval cost.

## Admission rule under test

A candidate may be described as **suggested** when one plausible occurrence exists but the session does not establish a broader rule. Suggested candidates must not silently become canonical firm rules.

A candidate may be described as **proved** only when the session provides stronger evidence such as recurrence, confirmed root cause, or a fix observed to work. This is an experimental rule derived from Hindsight's procedure, not yet a PMB governance rule.

## Evaluation set

Use a representative sample containing:

1. clean sessions with no durable lesson;
2. one-off mistakes;
3. repeated mistakes;
4. verified process corrections;
5. environment/tool findings;
6. decisions that should remain decisions rather than become generic memory;
7. existing-memory topics where the candidate should merge;
8. genuinely new durable lessons;
9. apparent lessons contradicted by later evidence;
10. lessons whose retrieval cost exceeds demonstrated value.

## Metrics

### Memory quality

- durable-lesson precision;
- missed durable lessons;
- false generalization rate;
- duplicate/near-duplicate rate;
- contradiction rate;
- stale-memory rate.

### Operational value

- repeated error reduction in later sessions;
- time/tokens saved by avoiding rediscovery;
- retrieval frequency;
- percentage of retained memories actually used;
- user approval/rejection rate.

### Context economics

Measure both:

- cost of retrospective analysis;
- cost of storing/retrieving/loading the resulting memory.

The relevant outcome is **net lifecycle value**, not whether the retrospective itself sounds useful.

## Governance boundary

Until the experiment demonstrates acceptable precision and maintenance cost:

- retrospective analysis may produce candidates;
- candidates may be written to a review artifact;
- canonical PMB memory requires explicit approval;
- the retrospective must not modify project code;
- suggested lessons must not be silently promoted to firm rules.

## Corpus relation

This experiment operationalizes the Hindsight evidence pass while preserving the PMB Dream finding that automatic canonical memory mutation is not yet justified.

**Disposition: CANDIDATE EXPERIMENT — not an adopted PMB architecture change.**
