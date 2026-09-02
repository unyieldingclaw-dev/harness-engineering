# David Ondrej — Agentic Engineering Prior-Art

**Date:** 2026-09-02  
**Status:** Evidence pass / research; no implementation commitment  
**Primary sources:** `davidondrej/skills`; Vectal Labs, *My Agentic Engineering setup* (2026-08-30); related repository evidence reviewed during the pass.

## Scope

This pass examines David Ondrej's published agent-skills repository and current agentic-engineering setup for reusable Harness Engineering concepts. Video claims are treated as author testimony unless corroborated by repository evidence.

## 1. Multi-harness command extraction exposes an enforcement asymmetry

David's `hooks/deny-dangerous.sh` attempts to extract command input from multiple harness payload shapes (`.tool_input.command`, `.toolInput.command`, and `.command`). PMB's deterministic dangerous-command guard currently extracts only `tool_input.command`.

PMB also ships Cursor rule files. However, Cursor's advisory rule mechanism is distinct from its actual hook mechanism. Therefore the presence of Cursor rules must not be interpreted as proof that PMB's deterministic guard covers Cursor.

### Finding

**MISSING CAPABILITY — Multi-harness hook-input normalization.**

A harness that claims support for multiple agent environments should explicitly distinguish:

- advisory instruction support;
- deterministic hook support;
- actual payload/schema compatibility.

If a Cursor hook supplies a different command field and PMB extracts an empty string, the guard can fail open while Cursor still appears supported at the documentation/rules layer.

### Harness principle

> Support claims should be tied to the enforcement mechanism actually exercised by the target harness, not merely to the presence of advisory configuration files.

This is a concrete instance of the broader structural-vs-instructional enforcement distinction.

## 2. Externalized dangerous-command policy is a useful refinement

David's dangerous-command hook keeps patterns in a separate `dangerous-patterns.txt` file rather than embedding all patterns directly in the shell implementation.

### Disposition

**USEFUL REFINEMENT — Investigate external policy data.**

Potential benefits:

- policy is independently inspectable;
- pattern changes are easier to review and diff;
- matcher implementation remains separate from policy data;
- policy can be tested as data without changing matching machinery.

Do **not** flatten PMB's BLOCK / CONFIRM / WARN model into David's binary block/allow behavior. PMB's confirmation tier expresses an authority boundary that a binary denylist cannot.

## 3. Reviewer diversity is not the same as reviewer independence

David's `total-review` skill runs independent reviewers and deduplicates findings. Its value is strongest when the reviewers differ in model or failure characteristics.

This sharpens an existing PMB finding: multiple domain labels do not necessarily create independent evidence. Reviewers can share the same model, briefing, source material, and incorrect premises and therefore converge on the same false conclusion.

### Finding

**HIGH-VALUE RESEARCH — Verification independence should be assessed by premise diversity, not reviewer count.**

A useful distinction is:

```text
reviewer diversity
≠
domain-label diversity
≠
independent evidence
```

True corroboration requires meaningful independence in the underlying reasoning path, evaluator, premises, or evidence source.

## 4. Explicit noise filtering addresses recursive-review degradation

David's review workflow explicitly filters findings considered overthinking/noise and reports the number discarded. His setup also argues against recursive review because repeated review can cause models to invent increasingly marginal or imaginary defects.

The latter independently converges with PMB's own opposition conclusion that recursive review had begun consuming the work rather than improving it.

### Disposition

**PRIORITY RESEARCH — Review-noise filtering and termination.**

Do not immediately implement a filter. A suppressive filter can hide real defects. First establish:

- what makes a finding actionable;
- who/what may suppress it;
- whether suppressed findings remain auditable;
- how false-negative risk is measured;
- what terminates additional review rounds.

The external convergence is strong evidence against treating unlimited recursive review as inherently higher assurance.

## 5. Model pinning corroborates controlled verification

David explicitly pins reviewer models in his setup and rejects uncontrolled downgrade behavior for important review work.

This independently corroborates PMB's existing practice of pinning the opposition reviewer model where verification quality depends on a known model configuration.

### Disposition

**CONVERGENT — No immediate architecture change.**

The stronger Harness lesson is to record the execution identity of verification, rather than merely recording that “a reviewer ran.”

## 6. Guardrails and push-lock separate safety from publication serialization

David describes a global pre-tool-call guard that blocks dangerous operations and a push lock that serializes merge/push/CI/deploy/health-check activity across many agents. The push-lock implementation was not established as repository evidence during this pass and therefore remains an author claim requiring direct verification.

If verified, it illustrates two different controls:

```text
pre-tool guard
  → prevents prohibited operations

publication lock
  → serializes shared publication authority
```

### Disposition

**PRIORITY FOLLOW-UP — Publication serialization.**

This should remain distinct from worktree isolation. Worktrees protect concurrent source state; publication serialization protects a shared external boundary.

## 7. Read-only production access is a concrete authority-boundary example

David describes granting agents read-only access to production Postgres so they can reality-check behavior without receiving write authority.

### Harness implication

This is a useful concrete example of:

```text
tool availability
≠
mutation authority
```

The same resource can be available for observation while its mutation interface remains prohibited.

## 8. Decision elicitation is a useful bounded human-in-the-loop pattern

David's `ask-then-build` skill identifies consequential non-obvious decisions, presents bounded options with a recommendation, asks one decision at a time, records the decision, and only then produces an implementation prompt.

### Candidate capability

**Decision Elicitation:**

```text
detect consequential ambiguity
        ↓
surface decision
        ↓
bounded options + recommendation
        ↓
human decides
        ↓
persist decision
```

This is consistent with PMB's human-accountability model and is better understood as a bounded decision-support capability than as a generic “interview” skill.

## 9. Agent state and coordination authority deserve explicit modeling

David's operating setup emphasizes visibility into agent states and prioritization of agent attention. His orchestration skills also make project, provider, model, reasoning, worktree, permissions, task prompt, parent relationship, and execution environment explicit when launching workers.

### Harness implication

This strengthens the case for treating **coordination authority** separately from execution authority:

- create work;
- delegate;
- start/cancel workers;
- inspect worker state;
- adjudicate results;
- integrate changes;
- authorize publication.

An agent may have permission to edit a task branch without having permission to merge or publish it.

## 10. Progressive disclosure is concrete context control

David's skill-authoring guidance emphasizes routing descriptions, one concern per skill, progressive disclosure, and using code for deterministic/repetitive operations while leaving judgment to the model.

### Disposition

**CONVERGENT / USEFUL REFINEMENT.**

This supports the existing anti-bloat direction and the Harness distinction between deterministic enforcement and model judgment. It does not justify adding a large new skill layer.

## 11. Over-abstraction is a real risk in skill/harness design

David explicitly recommends not installing every skill and favors small, focused skills. His authoring guidance treats the skill description primarily as a routing contract rather than a full workflow specification.

This corroborates the existing PMB/Harness direction against mega-skills and context-heavy universal instructions.

## 12. Findings by disposition

### Missing capability / priority research

1. Multi-harness hook-input normalization and explicit support matrix.
2. Verification independence measured by premise/evaluator diversity.
3. Review-noise filtering with false-negative safeguards.
4. Publication serialization across concurrent agents.
5. Explicit coordination authority model.
6. Decision-elicitation capability for consequential ambiguity.

### Useful refinements

7. Externalized dangerous-command policy data.
8. Explicit execution identity for verification.
9. Progressive disclosure and one-concern-per-skill discipline.
10. Read-only access as a worked authority-boundary pattern.

### Convergent — no immediate change

11. Reviewer model pinning.
12. Approval before remediation.
13. Avoiding recursive review as a default quality strategy.

### Reject / constrain

14. Universal YOLO/skip-permissions operation protected only by a binary denylist.
15. Treating third-party review scores as a universal termination condition.
16. Assuming high agent count is itself a quality or productivity guarantee.
17. Treating advisory Cursor rules as equivalent to deterministic Cursor hook enforcement.

## Conclusion

The strongest contribution from this prior art is not another skill collection. It is evidence for a more explicit Harness control model in which **model capability, tool availability, execution authority, and coordination authority are distinct**, while verification quality additionally depends on **independence of premises and evaluators**.

The Cursor finding is particularly important because it demonstrates how a system can appear multi-harness-compatible while its strongest enforcement mechanism remains tied to one harness's payload schema. That is precisely the kind of structural asymmetry the Harness Engineering research should track explicitly.

No implementation commitment is made by this research note.
