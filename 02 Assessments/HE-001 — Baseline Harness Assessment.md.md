# HE-001 — Baseline Harness Assessment

## Objective

Determine how PMB and ACR currently construct, select, inject,
retrieve, persist, compact, and enforce AI context and harness
behavior before proposing Harness Engineering changes.

---

## Systems

### Personal Memory Bank

Repository: personal-memory-bank  
Branch: main

### AI Code Review Agent

Repository: ai-code-review-agent  
Branch: main

---

## Questions

1. What context is always loaded?
2. What context is conditionally loaded?
3. What context is retrieved on demand?
4. What behavior is deterministically enforced?
5. Where is functionality duplicated?
6. Which capabilities are candidates for independent skills, and which existing workflows should remain orchestration rather than become skills?
7. Is ACR meaningfully independent from the authoring harness?
8. What context is necessary for effective independent review?
9. Which complexity is justified?
10. Which complexity has no demonstrated value?
11. Which execution and runtime configurations materially affect each AI workflow?
12. Who owns context selection, context compaction, persistence, and retrieval?
13. What minimum execution metadata would allow us to explain or reproduce materially different AI behavior?
14. Which harness behaviors depend on provider-specific capabilities versus provider-independent abstractions?
15. How does PMB handle multiple concurrent sessions working on different features within the same project?
16. Can session-specific handoff state be isolated without losing access to authoritative project-level context, and can a session reliably determine which state belongs to it?
17. Which persistent instructions constrain model judgment unnecessarily, and which constraints protect intentional project behavior or deterministic requirements?
18. How does information flow from repository inputs to final model context, and how are stable instructions, current project state, resource maps, and historical records selected and distinguished along that path?
19. Which current workflows are procedural because the workflow itself
    provides value, and which are procedural primarily because multiple
    capabilities have been coupled together?
20. Which current instructions describe behavior the model already
    understands, versus constraints that protect intentional behavior
    or demonstrated failure modes?
21. Where can established engineering vocabulary replace procedural
    instruction without reducing reliability?
22. Which existing artifacts can serve as the durable handoff between
    capabilities without introducing new artifact types? 
23. Can the same underlying capability enter the harness through
    multiple supply paths, and if so, how are those paths identified,
    owned, selected, and prevented from creating unnecessary or
    conflicting capability exposure?
24. How are PMB capabilities distributed and discovered when multiple
    PMB installations, versions, or capability surfaces are available,
    including global and project-local installations and plugin,
    command, skill, and other harness surfaces?

    Determine:
    - Which installation or surface takes precedence.
    - Whether precedence is deterministic and documented.
    - How version mismatches are detected.
    - Whether an older global installation can shadow a newer
      project-local implementation.
    - How command, skill, and other capability collisions are resolved.
    - Whether users can reliably determine which PMB implementation
      executed.
    - Whether distribution, discovery, and execution ownership are
      clearly separated.

    Treat observed precedence behavior as a correctness concern, not
    merely a usability issue.

---

## Additional Assessment Dimensions

The assessment is not limited to prompts, instructions, tools, and memory.

For each workflow, identify:

- Model
- Inference provider/runtime
- Model variant or quantization (when applicable)
- Context allocation and management
- Prompt and policy layers
- Tool availability
- Memory/retrieval behavior
- Harness orchestration

Determine which layers materially affect behavior and which are implementation details that should remain outside Harness Engineering.

Identify where each layer is configured, modified, or selected during execution.

Evaluate whether persistent guidance exists because it remains necessary for current model behavior or because it reflects historical limitations of earlier models.

Do not remove guidance solely because newer models are more capable.

Preserve constraints that encode:

- Intentional project behavior
- Safety requirements
- Deterministic behavior
- Demonstrated failure prevention

For each layer, identify the single owning component whenever practical.

Examples include:

- AI client
- Inference provider
- Runtime
- PMB
- ACR
- Development harness
- Underlying model

### Context and Capability Cost

Evaluate the operational cost of harness surfaces in addition to their
functional behavior.

For each context source or capability, determine where practical:

- whether it contributes to initial context;
- whether it is loaded on every session or only when triggered;
- whether it can be retrieved on demand;
- whether it materially affects context allocation;
- whether it introduces redundant or overlapping guidance;
- whether it creates capability-selection ambiguity;
- whether its contribution justifies its persistent availability.

Distinguish between:

- installed capability;
- available capability;
- discovered capability;
- loaded context;
- invoked capability;
- demonstrated useful capability.

Do not treat token reduction as an objective by itself.

Evaluate context reduction only when it preserves required behavior,
reliability, intentional constraints, and demonstrated failure prevention.

Where provider pricing or caching behavior materially affects execution
cost, record the relevant conditions as part of execution provenance rather
than embedding provider-specific cost assumptions into PMB or ACR
architecture.

### Modular Capability Assessment

HE-001 must distinguish between:

- capabilities that are independently useful;
- capabilities that legitimately depend on prior artifacts;
- orchestration that provides useful sequencing;
- orchestration that exists primarily because the workflow was designed
  around a fixed sequence.

Do not assume that modularity is inherently superior.

Evaluate whether modular capability composition would:

- reduce unnecessary workflow coupling;
- reduce always-loaded context;
- simplify maintenance;
- allow small tasks to avoid unnecessary process;
- preserve useful dependencies and sequencing;
- preserve deterministic enforcement;
- avoid capability duplication;
- avoid creating routing complexity that exceeds the value provided.

Also evaluate whether a prescribed workflow currently provides benefits that
would be lost by moving toward modular capability composition.

### Capability Contribution / Ablation Assessment

Where a capability, skill, instruction set, or context source appears
unnecessarily large or procedurally prescriptive, evaluate whether its
individual components materially contribute to successful execution.

Where practical, use an evidence-based ablation approach:

- Establish a representative baseline.
- Define what successful execution means.
- Classify deterministic versus model-mediated behavior.
- Remove or isolate candidate guidance, context, or capability components.
- Execute representative tasks under comparable conditions.
- Record failures or degradation attributable to the removed component.
- Restore only components supported by observed evidence.
- Preserve intentional constraints and demonstrated failure prevention.

Distinguish between:

- content required for deterministic execution;
- guidance required for model judgment;
- guidance that protects known failure modes;
- guidance that improves output quality or project-specific behavior;
- guidance that provides no demonstrated benefit.

Do not optimize for minimum size alone.

The objective is to minimize unnecessary context and procedural
instruction while preserving required behavior, reliability, and
project-specific intent.

For subjective or "taste" criteria, identify where human judgment
remains necessary rather than treating an automated grader as the sole
authority.

---

## Harness Surface Classification

Every harness surface will be classified as:

- ALWAYS
- TRIGGERED
- RETRIEVED
- ENFORCED
- UNNECESSARY

---

## Known Operational Issues

### Concurrent Session Handoff Collision

PMB has been observed in real project usage with multiple concurrent AI
sessions working on different features within the same project.

Current handoff behavior can cause session state to become jumbled,
overwritten, or incorrectly consumed by another session, resulting in
context confusion.

HE-001 must determine:

- Which handoff information is project-level versus session-specific.
- How concurrent session state is currently identified and stored.
- Where overwrites or ambiguous ownership can occur.
- How a new session determines which prior session state is relevant.
- What minimum isolation is required without duplicating shared project context.

No implementation approach is assumed.

---

## Assessment Phases

The assessment proceeds in the following order:

1. Repository Orientation
2. Context Sources Inventory
3. Harness Surface Classification
4. Context Lifecycle
5. Ownership Matrix
6. Deterministic vs Model-mediated Classification
7. Runtime Analysis
8. Context Supply Chain
9. Findings
10. Recommendations

Recommendations are intentionally deferred until the baseline
assessment is complete.

---

## Evidence Standard

Do not recommend a change merely because an implementation appears
complex.

Recommendations require an observed problem, measurable cost,
duplication, misplaced responsibility, or demonstrated opportunity to
simplify without losing capability.

Significant architectural observations should identify the supporting
implementation whenever practical, such as:

- Source file
- Configuration
- Workflow
- Documented behavior

Assessment reports should clearly distinguish:

- Observed behavior
- Inference
- Recommendation

Assessment reports should include repository evidence supporting significant findings whenever practical.

### Research Finding — Agent-Optimized Instructions and Capability Discovery

Recent evolution of external agent skill systems indicates a distinction
between persistent agent instructions, on-demand capabilities, and
agent-optimized documentation.

Evaluate whether PMB and ACR currently place information in the correct
surface:

- persistent instructions;
- triggered capabilities;
- retrieved resources;
- workflow orchestration;
- human-facing documentation.

In particular, evaluate whether persistent instructions can be reduced by
moving task-specific procedural guidance into discoverable capabilities
without reducing reliability.

Also evaluate whether capability metadata and invocation behavior are
portable across development harnesses or depend on provider-specific
mechanisms.

Do not adopt external skill structures solely because they are popular.
Use them as evidence when assessing PMB and ACR's existing capability
boundaries.
### Reproducibility / Conditions Disclosure

For materially different AI outcomes, determine whether the available
execution record contains enough information to distinguish model behavior
from harness, context, tool, runtime, configuration, and evaluation effects.

Where practical, identify the minimum reproducibility recipe for a
workflow, including:

- Model and model version
- Inference provider/runtime
- Relevant model configuration
- Harness/workflow version
- Context sources and significant context selection
- Tool availability
- Relevant execution constraints
- Evaluation method and evaluator
- Benchmark/test version
- Repository/project state when relevant

Do not treat complete observability as the goal.

The objective is to determine the minimum provenance required to explain
or reproduce materially different behavior.

Classify undocumented conditions as:

- Known and reproducible
- Known but not reproducible
- Unknown but potentially material
- Unlikely to materially affect the result

Treat materially undisclosed execution conditions as a limitation on the
strength of the finding, not automatically as evidence that the underlying
result is invalid.


---
### Loop-Based Execution

Evaluate whether any PMB or ACR workflow contains an implicit
iteration loop of:

goal → action → evidence → evaluation → next action.

For each identified loop, determine:

- what establishes the goal;
- what actions may be taken;
- what evidence is produced;
- who or what evaluates the evidence;
- what state persists between iterations;
- what constitutes success;
- what constitutes failure;
- what causes retry, rollback, escalation, or termination;
- whether the loop is bounded;
- whether iteration state is isolated from unrelated sessions;
- whether tool output is treated as evidence rather than trusted instruction.

Do not introduce autonomous looping merely because a workflow can be
expressed as a loop. Require demonstrated value, measurable verification,
and explicit termination conditions.

## Deliverables

- Baseline PMB Harness Surface Map
- Baseline ACR Harness Surface Map
- Context Sources Inventory
- Deterministic vs Model-mediated Classification
- Context Supply Chain
- Initial Skill Candidate Inventory
- Duplication Inventory
- Execution & Context Provenance Baseline
- Modular Capability vs Prescribed Workflow Assessment
- Capability Ownership / Orchestration Boundary
- Capability Distribution & Precedence Map

Define the minimum execution and context metadata necessary to explain
or reproduce materially different AI behavior without turning the
Harness into an observability platform.



---

## Status

IN PROGRESS