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
16. Can session-specific handoff state be isolated without losing access to authoritative project-level context?
17. Which persistent instructions constrain model judgment unnecessarily, and which constraints protect intentional project behavior or deterministic requirements?
18. How does information flow from repository inputs to final model context, and which components transform, filter, enrich, or enforce that context along the way?
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

---

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

Define the minimum execution and context metadata necessary to explain
or reproduce materially different AI behavior without turning the
Harness into an observability platform.



---

## Status

IN PROGRESS