# Matt Pocock — Deep Evidence Pass

**Date:** 2026-09-02  
**Source:** `mattpocock/skills` public repository  
**Purpose:** Mine underlying Harness Engineering capabilities, not copy the skill system.

## Evidence discipline

Earlier synthesis attributed a complete lifecycle to the Matt research before all of the relevant source files had been inspected. That attribution was too strong.

The following files were subsequently verified directly:

- `skills/productivity/grilling/SKILL.md`
- `skills/engineering/wayfinder/SKILL.md`
- `skills/engineering/to-spec/SKILL.md`
- `skills/engineering/to-tickets/SKILL.md`
- `skills/engineering/implement/SKILL.md`

Therefore the lifecycle below is now supported as a source-derived synthesis rather than an inference from a README skill inventory alone.

## Verified findings

### Grilling — decision frontier

The `grilling` skill describes iterative inquiry as a design tree. It works in rounds and defines the frontier as decisions whose prerequisites are already settled. The agent asks questions that can be answered without guessing at unresolved prerequisites.

**Harness relevance:** decision elicitation can be bounded by currently answerable decisions rather than asking every conceivable question up front.

**Disposition:** USEFUL REFINEMENT. Do not copy the workflow or create a universal grilling stage.

### Wayfinder — planning under uncertainty

`wayfinder` is explicitly for efforts too large for one agent session where the route to the destination is still unclear. It creates a shared map of decision tickets and resolves decisions until the route is clear. It distinguishes planning from execution and emphasizes naming the destination first.

**Harness relevance:** large-work planning can be represented as a decision frontier rather than premature implementation decomposition.

**Disposition:** USEFUL REFINEMENT. Assess against existing PMB planning artifacts before introducing another planning layer.

### to-spec — synthesis without re-interview

`to-spec` turns the current conversation and codebase understanding into a specification without reopening alignment. It identifies testing seams, prefers existing seams, and asks the user to confirm those seams before publishing the spec.

**Harness relevance:** durable specification can preserve settled intent without forcing a redundant discovery interview.

**Disposition:** USEFUL REFINEMENT. Existing PMB chain should be compared before adding a new capability.

### to-tickets — vertical slices and blocking edges

`to-tickets` turns an agreed plan/spec into tracer-bullet vertical slices. Tickets are independently verifiable and explicitly declare blockers. The tracker is used as a shared dependency graph.

**Harness relevance:** execution planning can preserve dependency relationships and independently verifiable increments rather than produce a flat task list.

**Disposition:** USEFUL REFINEMENT. Existing planning/implementation mechanisms must be checked for duplication.

### implement — decided work versus redesign

`implement` is explicitly for executing work that has already been decided. Its documented flow uses TDD where appropriate, regular typechecking and focused tests, a full suite at the end, code review, and a commit. It does not reopen the plan.

**Harness relevance:** execution authority can be separated from planning authority; implementation should not silently redesign settled intent.

**Disposition:** USEFUL REFINEMENT. This reinforces the existing separation of planning, implementation, and review.

## Cross-cutting synthesis

The verified files support a coherent pattern:

`inquiry → decision frontier → durable intent/spec → dependency-aware work → implementation → review`

This is a **synthesis of verified source material**, not a claim that Matt's repository should be adopted as a prescribed PMB workflow.

The important underlying concepts are:

1. Ask only currently answerable consequential questions.
2. Separate facts from decisions.
3. Preserve settled intent before execution.
4. Represent dependencies explicitly when work is large enough to need them.
5. Keep implementation from reopening upstream decisions.
6. Keep review as a separate evaluation activity.

## Important boundaries

Do not:

- copy Matt's skill names or directory structure;
- mandate this lifecycle for every task;
- create a skill for every engineering activity;
- treat the repository's popularity or install count as evidence of architectural value;
- replace deterministic enforcement with model judgment;
- duplicate existing PMB or ACR mechanisms.

## Relationship to existing corpus

- **Specification / intent review:** SHARPENS existing `[NS-39]`; it is not a new territory.
- **Canonical vocabulary:** SHARPENS `[NS-42]`; adoption requires measured net context benefit.
- **Decision elicitation:** SHARPENS the existing human-decision-authority and governed-improvement work.
- **Dependency-aware planning:** POTENTIAL REFINEMENT; assess against current planning artifacts before adding anything.
- **Implementation as execution of decided intent:** REINFORCES existing separation of planning, implementation, and review.
- **Sandbox/capability isolation:** ORTHOGONAL to Matt's engineering-discipline findings; Sandcastle provides infrastructure evidence for this separate concern.

## Evidence quality

The source is highly useful practical evidence, but several findings come from the same AI-coding workflow discourse as David Ondrej and related practitioners. Agreement among these sources should therefore not be counted as independent corroboration without a distinct provenance or measurement basis.

## External source references

- https://github.com/mattpocock/skills/blob/main/skills/productivity/grilling/SKILL.md
- https://github.com/mattpocock/skills/blob/main/skills/engineering/wayfinder/SKILL.md
- https://github.com/mattpocock/skills/blob/main/skills/engineering/to-spec/SKILL.md
- https://github.com/mattpocock/skills/blob/main/skills/engineering/to-tickets/SKILL.md
- https://github.com/mattpocock/skills/blob/main/skills/engineering/implement/SKILL.md
