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

## Core Concepts

Persistent context should continuously justify its existence.

Information should be evaluated based on:

- Who owns it?
- When should it load?
- Where should it live?
- How is it maintained?
- Can it become a skill, reference, retrieval, or enforcement instead?
- What evidence justifies its continued existence?

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
  
## Status

Research only.
No architectural decisions made.
Await HE-001 evidence.