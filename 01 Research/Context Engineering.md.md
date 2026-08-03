# Context Engineering

## Overview
Context Engineering is the discipline of designing how AI systems receive, discover, retrieve, organize, and apply information during task execution.

Unlike prompt engineering, which focuses primarily on individual requests, context engineering considers the entire information ecosystem, including system prompts, skills, memory, references, tools, retrieval, orchestration, and runtime behavior.

## Key Findings

- Modern models require fewer global instructions.
- Progressive disclosure is preferred over always-loaded guidance.
- Skills are primarily a context-loading mechanism.
- Tool/interface design often matters more than prompt examples.
- Repetition across context sources creates conflicting guidance.
- Rich references are often better than rewritten instructions.

## Research Sources and Observations

### Anthropic — Context Engineering

- Newer models may require fewer persistent global instructions.
- Prefer progressive disclosure over loading specialized guidance upfront.
- Skills provide a mechanism for selectively loading procedural context.
- Supporting references can remain outside active context until needed.
- Tool and interface design can reduce the need for repetitive prompting.
- Duplicated or conflicting instructions can degrade model behavior.

### Cursor — Dynamic Context Discovery

- Context can be dynamically discovered rather than provided entirely upfront.
- Rules, skills, tools, and other context sources can have different loading behavior.
- Context consumption should be inspectable enough to identify unnecessary persistent context.
- Independent context for delegated work may reduce pollution of the primary working context.

### Nate B. Jones — Token Efficiency

- Conversation continuity has a token cost; repeated input is not necessarily unnecessary input.
- Carry forward accepted artifacts and durable state rather than unnecessary conversational history.
- Retrieve large references only when required.
- Prefer deterministic tools for work that does not require model reasoning.
- Context reduction should be evaluated against mistakes, retries, review effort, and repeated work rather than token count alone.
- A skill can influence behavior after invocation but cannot remove context already injected by the surrounding harness.

### Nate B. Jones — AI Second Brain

Relevant Harness Engineering principles:

- Separate durable memory, compute, and interface.
- Prefer routing over unnecessary manual organization.
- Use explicit contracts at deterministic system boundaries.
- Maintain enough provenance to understand important automated decisions.
- Default to safe behavior when uncertain.
- Make automated decisions easy to inspect and correct.
- Build a minimal core workflow before adding optional modules.
- Optimize for maintainability over cleverness.

The Second Brain implementation itself is not currently a Harness Engineering requirement. Its architectural patterns are useful as research references.

### Simon Willison — Agentic Engineering Patterns

- Coding agents should be understood as harnesses combining models,
  system prompts, tools, execution, and iterative feedback.
- Context isolation through subagents can preserve primary working
  context for bounded exploration or specialized tasks.
- Preserve proven, reusable knowledge rather than repeatedly
  rediscovering solutions.
- Completed work can feed a compound engineering loop where useful
  lessons improve future agent execution.
- Small, high-leverage instructions may activate existing model
  capabilities more effectively than extensive procedural guidance.
- Agent-generated code still requires verification, reviewable scope,
  and evidence that the implementation works.
- Reduced implementation cost increases the importance of engineering
  judgment rather than eliminating it.

Harness implication:

These patterns reinforce existing HE-001 investigation areas around
context isolation, durable knowledge, progressive disclosure,
verification, independent review, and continuous improvement.
Subagents and additional orchestration remain mechanisms to evaluate
against observed problems rather than default architectural components.

## Core Concepts

Persistent context should continuously justify its existence.

Information should be evaluated based on:

- Who owns it?
- When should it load?
- Where should it live?
- How is it maintained?
- Can it become a skill, reference, retrieval, or enforcement instead?
- What evidence justifies its continued existence?

## Model Capability Drift

Harness guidance should not be assumed to remain necessary simply
because it was previously necessary.

As model capabilities evolve, persistent instructions, examples,
workarounds, and guardrails may become redundant or may unnecessarily
constrain model judgment.

Periodic context architecture audits should therefore evaluate whether
guidance:

- addresses a currently demonstrated model limitation,
- encodes intentional project or team behavior,
- protects a safety or deterministic requirement,
- prevents a demonstrated failure mode, or
- persists primarily because of historical model limitations.

More capable models alone are not sufficient evidence for removal.
Changes should be supported by observed behavior or evaluation.
## Potential Impact on Harness Engineering

Questions to evaluate during HE-001:

- Which PMB guidance should become skills?
- Which skills should be decomposed into skill subdirectories?
- Which information should become references instead of instructions?
- Which instructions are duplicated?
- What should always load vs be discovered?

## Candidate Architectural Decisions

### Skill Hierarchies

Decision Status: Pending HE-001 Assessment

Description

Allow skills to be decomposed into subdirectories for progressive
disclosure and reduced startup context.

Evidence Required

- Demonstrated reduction in always-loaded context
- Reduced duplication
- Simpler navigation
- No measurable usability regression

### Context Architecture Audit

Decision Status: Pending HE-001 Assessment

Description

Periodically evaluate persistent context to identify opportunities
for progressive disclosure, skill extraction, reference-based
guidance, retrieval, enforcement, or removal.

Evidence Required

- Reduced startup context
- Reduced duplication
- Simpler maintenance
- No degradation in task performance

## Open Questions

- How should skill hierarchies be structured?
- What criteria determine when guidance should become a skill?
- How should context architecture be measured over time?
- What information should survive a session boundary?
- How should PMB distinguish project-level durable state from session-specific working state?
- How should multiple concurrent sessions contribute to shared project state without unnecessary context duplication or conflicting authority?
  
## Status

Research only.
No architectural decisions made.
Await HE-001 evidence.