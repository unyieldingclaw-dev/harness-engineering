# HE-001 — Baseline Harness Assessment

## Objective

Determine how PMB and ACR currently construct, select, inject, retrieve,
persist, compact, and enforce AI context and harness behavior before
proposing Harness Engineering changes.

## Systems

### Personal Memory Bank
Repository: personal-memory-bank
Branch: main

### AI Code Review Agent
Repository: ai-code-review-agent
Branch: main

## Questions

1. What context is always loaded?
2. What context is conditionally loaded?
3. What context is retrieved on demand?
4. What behavior is deterministically enforced?
5. Where is functionality duplicated?
6. Which workflows are candidates for skills?
7. Is ACR meaningfully independent from the authoring harness?
8. What context is necessary for effective independent review?
9. Which complexity is justified?
10. Which complexity has no demonstrated value?
11. Which execution and runtime configurations materially affect each AI workflow?
12. Who owns context selection, context compaction, persistence, and retrieval?
13. What minimum execution metadata would allow us to explain or reproduce materially different AI behavior?
14. Which harness behaviors depend on provider-specific capabilities versus provider-independent abstractions?
15. How does PMB handle multiple concurrent sessions working on
    different features within the same project?

16. Can session-specific handoff state be isolated without losing
    access to authoritative project-level context?
    
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

For each layer, identify the single owning component whenever possible.

Examples include the AI client, inference provider, runtime,
PMB, ACR, the development harness, or the underlying model.

## Harness Surface Classification

Every harness surface will be classified as:

- ALWAYS
- TRIGGERED
- RETRIEVED
- ENFORCED
- UNNECESSARY

## Known Operational Issues

### Concurrent Session Handoff Collision

PMB has been observed in real project usage with multiple concurrent
AI sessions working on different features within the same project.

Current handoff behavior can cause session state to become jumbled,
overwritten, or incorrectly consumed by another session, resulting
in context confusion.

HE-001 must determine:

- Which handoff information is project-level versus session-specific.
- How concurrent session state is currently identified and stored.
- Where overwrites or ambiguous ownership can occur.
- How a new session determines which prior session state is relevant.
- What minimum isolation is required without duplicating shared
  project context.

No implementation approach is assumed.
## Evidence Standard

Do not recommend a change merely because an implementation
appears complex.

Recommendations require an observed problem, measurable cost,
duplication, misplaced responsibility, or demonstrated opportunity
to simplify without losing capability.

## Deliverables

- Baseline PMB Harness Surface Map
- Baseline ACR Harness Surface Map
- Initial Skill Candidate Inventory
- Duplication Inventory
- Execution & Context Provenance Baseline

Define the minimum execution and context metadata necessary to explain or reproduce materially different AI behavior without turning the Harness into an observability platform.

## Status

IN PROGRESS