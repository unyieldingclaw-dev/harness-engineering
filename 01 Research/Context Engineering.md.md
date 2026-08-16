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

### RoboNuggets — Gauntlet Loop

- Separating builder and evaluator perspectives can reduce self-approval.
- Evaluation is more useful when grounded in an explicit, task-relevant
  quality bar.
- Blind comparison can reduce evaluator bias during iterative refinement.
- Builder/critic loops and subagent fan-out can support refinement where
  repeated independent evaluation materially improves the result.
- Iterative optimization should begin from a strong direction,
  specification, design, or reference; otherwise the loop may optimize
  toward the wrong objective.

Harness implication:

Independent evaluation and explicit quality bars are useful patterns
for workflows that benefit from iterative refinement. Builder/critic
loops and subagent fan-out remain execution mechanisms to justify
against observed needs rather than default Harness architecture.

## Core Concepts

Persistent context should continuously justify its existence.

Information should be evaluated based on:

- Who owns it?
- When should it load?
- Where should it live?
- How is it maintained?
- Can it become a skill, reference, retrieval, or enforcement instead?
- What evidence justifies its continued existence?

### Capability Supply Paths

The same underlying capability may be exposed to an AI workflow
through multiple harness surfaces.

For example, a capability such as Context7 may be available through
a connector, plugin, skill, or directly configured MCP server.

These surfaces should not automatically be treated as equivalent or
assumed to be additive.

Harness assessment should determine:

- Which capability supply paths are active.
- Whether multiple paths expose overlapping functionality.
- Which component owns the underlying capability.
- Which surface controls discovery and activation.
- Whether duplicate capability exposure creates conflicting behavior,
  redundant context, routing ambiguity, or unnecessary tool selection.
- Whether multiple surfaces are intentional or merely different
  distribution mechanisms for the same capability.

The existence of multiple interfaces to a capability is not, by itself,
evidence of a problem.

HE-001 should distinguish:

- duplicate capability implementation;
- duplicate capability exposure;
- multiple interfaces to one capability;
- and intentional composition of distinct capabilities.

### Current State Must Be Distinct From History

Long-running and multi-session AI work requires a distinction between:

- Stable instructions
- Current project state
- Context or resource map
- Historical record

The current state should represent what is true now: active goals,
decisions, unresolved questions, next actions, and relevant boundaries.

Historical records preserve what happened and why, but should not
automatically carry the same authority as current state.

Fresh sessions should inherit current state rather than depending on
reconstruction from conversational history.

This separation is especially important when multiple concurrent
sessions work on the same project.
### Durable Knowledge Is a Harness Responsibility

Individual model executions are temporary.

Long-term capability emerges when useful discoveries survive beyond a
single execution through explicit harness mechanisms rather than
remaining inside a model's transient context.

Session outputs should become durable knowledge only after appropriate
review, provenance, and evidence justify preservation.

The harness—not the model—owns long-term knowledge management.

Implications:

- Preserve valuable discoveries rather than conversation history.
- Treat durable knowledge as an architectural responsibility.
- Separate temporary execution state from shared project knowledge.
- Evaluate candidate knowledge before promoting it into persistent context.

### Independent Evaluation and Quality Bars

Generation and evaluation should not always be performed from the
same perspective.

For workflows where quality can be evaluated against meaningful
external criteria, separating the builder from the evaluator can
reduce self-approval and support iterative improvement.

Evaluation should use an explicit, task-relevant quality bar rather
than open-ended instructions to continue improving.

Builder/critic loops and subagent fan-out are execution mechanisms,
not default architecture. They should be introduced only when the
task benefits from independent evaluation or iterative refinement
and the additional execution cost is justified.

A strong initial direction, specification, design, or reference
should precede iterative optimization. Repeated evaluation can
otherwise improve an output toward the wrong objective.

### Memory Consolidation

Session knowledge and durable project knowledge should not be assumed
to have the same lifecycle.

Individual sessions and concurrent workstreams may produce discoveries,
decisions, corrections, and temporary state that should remain isolated
until their durable value is established.

Where consolidation is needed, candidate knowledge should be evaluated
for duplication, contradiction, staleness, ownership, and continued
relevance before becoming persistent project context.

Consolidation should preserve provenance and favor reviewable,
reversible changes over autonomous rewriting of durable knowledge.

Candidate knowledge should be treated as a review artifact rather than
automatically becoming durable project context.

### Workflow Structure Should Justify Automation

Multi-step AI work can often be understood as jobs, transitions,
state, checks, loops, and human decision points.

Making that structure explicit can improve reliability when a workflow
has demonstrated requirements for sequencing, isolation, independent
verification, branching, recovery, or approval.

Explicit workflow structure does not imply that an orchestration
framework is required.

Prefer the simplest execution model that solves the observed problem.
Run and evaluate workflows manually or through existing tools before
encoding them into dedicated orchestration infrastructure.

Automation should follow demonstrated workflow structure rather than
define it prematurely.

### Composable Harness Capabilities

Harness capabilities may be composed, replaced, or temporarily disabled
when their boundaries and dependencies are explicit.

Composability should be evaluated as a means of reducing unnecessary
coupling, context, and execution surface rather than treated as an
architectural objective by itself.

A capability boundary is valuable when it permits independent ownership,
evaluation, replacement, isolation, or enforcement without introducing
greater coordination complexity than the boundary removes.

Runtime composition should preserve traceability of the active
capabilities and their effects on execution.
### Human Direction and Measurable Feedback

AI systems can execute increasingly large portions of engineering work,
but human judgment remains important at points where goals, architecture,
quality, or long-term maintainability are determined.

Prefer explicit outcomes, measurable acceptance criteria, reviewable
intermediate checkpoints, and vertical increments when these reduce the
cost of correcting a wrong direction.

Where subjective expectations can be converted into deterministic checks,
tests, rubrics, or other observable signals, prefer those mechanisms over
relying entirely on model judgment.

Automation should increase execution leverage without removing the
human's ability to understand, redirect, and verify the work.
### Preserve Valuable Engineering Synchronization

Not all engineering friction is waste.

Some coordination creates shared understanding by exposing
misunderstandings, conflicting assumptions, architectural
disagreements, and incomplete reasoning.

Harness Engineering should automate repeatable coordination when
doing so removes unnecessary work, but should not eliminate
interaction that materially improves shared understanding.

The objective is to reduce coordination overhead without removing
valuable synchronization.

### Model Capability Drift

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

- Can repeated project workflows be discovered and converted into
  skills based on observed behavior rather than only pre-declared guidance?
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

## Assessment Observations

Applying HE-001 to a mature repository demonstrated that
understanding the context supply chain, ownership boundaries,
runtime assumptions, deterministic behavior, and context ownership
provided significantly more architectural insight than reviewing
prompts alone.

Assessment also demonstrated that separating:

- Observed behavior
- Inference
- Recommendation

produces clearer architectural analysis and reduces premature
solutioning.

Future Harness Engineering assessments should prioritize
observation before architectural recommendation.
## Status

Research only.
No architectural decisions made.
Await HE-001 evidence.

---
## Research Sources and Observations
Core Concepts
Potential Impact
Candidate Architectural Decisions
Open Questions
Assessment Observations
Status

Matt Pocock / Eric Tech — Modular AI Engineering Skills
...
DeepSeek Harness — Developer Preview
## Matt Pocock / Eric Tech — Modular AI Engineering Skills

### Source

Research reviewed from:

- Matt Pocock's public Claude Code skills repository
- Eric Tech — "Matt Pocock's Claude Code Skills Beat Superpowers Now"
- Related concepts presented in Matt Pocock's skills, including `grill-me`,
  `to-spec`, `to-tickets`, `implement`, `code-review`, writing-for-agents,
  and engineering vocabulary.

This research is being evaluated for architectural patterns and ideas.
Matt Pocock's implementation is not being adopted or copied.

### Core Finding

A potentially useful distinction exists between **workflow orchestration**
and **engineering capabilities**.

A prescribed AI workflow might require:

    brainstorm → spec → plan → implement → review → commit

A modular capability model instead exposes independently useful capabilities:

    context
    spec
    implement
    diagnose
    review
    architecture analysis

The Harness may then select or compose capabilities based on the current
situation rather than requiring every task to pass through every stage.

This distinction should be evaluated during HE-001 rather than treated as
an architectural decision.

### Modular Capability Principle

A capability should be independently useful where practical.

The existence of relationships between capabilities does not require a
mandatory end-to-end workflow.

For example:

- implementation may consume a specification;
- review may consume the specification and implementation;
- architecture analysis may consume the resulting code.

These are legitimate dependencies.

The architectural concern is unnecessary workflow coupling, not dependency
itself.

### Orchestration vs Capability

Harness Engineering should distinguish between:

**Capability**
- performs a bounded engineering activity;
- can be invoked independently;
- has a clear purpose and inputs/outputs;
- should not assume that unrelated capabilities have already run.

**Orchestration**
- determines when capabilities should be combined;
- supplies appropriate context;
- establishes sequencing where sequencing is actually required;
- preserves deterministic controls and project boundaries.

The Harness should coordinate capabilities without becoming a mandatory
process pipeline.

### Candidate Engineering Patterns

The following concepts are candidates for evaluation rather than adopted
architecture:

- progressive clarification of ambiguous requirements;
- one meaningful question at a time when interactive discovery is required;
- recommending concrete options rather than presenting blank questions;
- preserving the agreed "what" separately from implementation-specific "how";
- vertical feature/tracer-bullet slices rather than layer-only work decomposition;
- explicit dependency and blocking relationships between work items;
- test-first / red-green-refactor as an implementation discipline where
  appropriate;
- fresh-context review to reduce builder-context contamination;
- independent review dimensions for specification/intent and engineering
  standards;
- established engineering vocabulary as compact guidance for code quality;
- deep-module, interface, and seam analysis where architectural reasoning
  is actually required;
- periodic architecture-health analysis rather than continuous autonomous
  refactoring;
- concise capability instructions rather than procedural prompt bloat;
- lightweight capability selection/routing without imposing a universal
  workflow.

### Durable WHAT vs Implementation HOW

A durable planning artifact should preserve the agreed intent, requirements,
constraints, decisions, and acceptance criteria.

It should avoid unnecessarily freezing implementation details that are
expected to change as the code evolves.

This supports durable handoff and reduces the risk of a planning artifact
becoming stale because it describes the implementation rather than the
intent.

### Fresh-Context Review

Independent review should be evaluated separately from implementation
context.

A reviewer's effectiveness may be reduced when it inherits the author's
conversation, assumptions, intermediate reasoning, and self-justifications.

This reinforces the existing Harness Engineering interest in context
isolation and independent verification.

This does not imply that every review requires a new agent or separate
runtime. The implementation mechanism remains an assessment question.

### Specification / Intent vs Standards

Code review may contain at least two conceptually distinct questions:

1. **Specification / Intent**
   - Does the implementation do what was requested?
   - Does it satisfy the agreed behavior and acceptance criteria?

2. **Engineering Standards**
   - Is the implementation technically sound?
   - Does it violate applicable security, reliability, maintainability,
     architectural, or coding standards?

These dimensions may be independently useful even when performed by the
same review system.

This is a candidate ACR assessment dimension, not a decision to create
additional reviewer agents.

### Engineering Vocabulary as Compressed Context

Established engineering terminology may allow a capability to express
substantial engineering knowledge with significantly less procedural
instruction.

Examples include:

- Shotgun Surgery
- Feature Envy
- Data Clumps
- Divergent Change
- Duplicated Code
- Long Method
- Large Class
- Long Parameter List
- Primitive Obsession
- Message Chains
- Speculative Generality

The relevant question for Harness Engineering is not whether these specific
terms should be adopted.

The question is whether established engineering vocabulary can provide
higher-signal guidance than lengthy procedural instructions.

### Minimum Sufficient Instruction

The objective should not be the shortest possible skill or instruction.

The objective is **minimum sufficient instruction**.

Guidance should contain the information that materially changes behavior
while avoiding:

- redundant explanation;
- repeated project context;
- procedural instructions already enforced elsewhere;
- historical workarounds that no longer address demonstrated failures.

### Architecture Analysis as a Distinct Capability

Architecture analysis should be considered a capability that can be invoked
when architectural concerns justify it.

It should not automatically become part of every implementation or code
review.

A potential operating model is:

    implementation
        ↓
    normal verification/review
        ↓
    periodic or triggered architecture analysis

This avoids turning normal development into continuous architectural
governance.

### Boundaries

The following are explicitly not conclusions of this research:

- Do not copy Matt Pocock's skills.
- Do not reproduce his skill names or directory structure merely for
  consistency.
- Do not mandate his workflow.
- Do not create a skill for every engineering activity.
- Do not replace deterministic enforcement with model judgment.
- Do not duplicate existing PMB mechanisms as generic Harness capabilities.
- Do not duplicate ACR capabilities in the Harness.
- Do not introduce orchestration infrastructure without demonstrated need.
- Do not treat modularity as an excuse to remove useful sequencing or
  dependency management.

### Relationship to Existing Harness Research

This research reinforces several existing Harness Engineering themes:

- Progressive Disclosure
- Single Ownership
- Evidence Before Architecture
- Independent Verification
- Context Isolation
- Durable Artifacts
- Model Capability Drift
- Continuous Improvement

It also introduces a specific architectural question for HE-001:

> Should Harness Engineering primarily expose modular capabilities and
> selectively compose them, rather than encode a prescribed end-to-end AI
> development workflow?

This remains a research question pending evidence.

