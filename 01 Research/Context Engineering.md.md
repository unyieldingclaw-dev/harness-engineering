# Context Engineering

## Overview

Context Engineering is the discipline of designing how AI systems receive, discover, retrieve, organize, and apply information during task execution.

Unlike prompt engineering, which focuses primarily on individual requests, context engineering considers the entire information ecosystem, including system prompts, skills, memory, references, tools, retrieval, orchestration, and runtime behavior.

## Key Findings

- Modern models may require fewer global instructions than earlier model generations.
- Progressive disclosure is preferred over always-loaded guidance.
- Skills can provide selectively loaded context and bounded capabilities.
- Tool/interface design often matters more than prompt examples.
- Repetition across context sources creates conflicting guidance.
- Rich references are often better than rewritten instructions.

## Harness as a System

AI behavior should be evaluated as the result of an execution system,
not solely as a property of the underlying model.

A materially different result may arise from differences in:

- model;
- inference provider or runtime;
- context;
- system and persistent instructions;
- tools;
- skills;
- retrieval;
- orchestration;
- execution environment;
- evaluation and feedback loops.

Therefore, model comparisons and workflow evaluations should distinguish
the model from the harness surrounding it.

This does not imply that every execution detail must be captured or that
the Harness should become an observability system.

Capture provenance only to the degree necessary to explain or reproduce
materially different behavior.

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

#### Context Cost Requires Measurement

Token-cost optimization should begin with measured execution data rather than assumptions about where tokens are being spent.

Relevant measurements may include:

- persistent instruction size;
- tool and MCP definition overhead;
- model and effort configuration;
- subagent model selection;
- hook-generated output;
- cache creation and cache reads;
- input and output tokens;
- context size across the session;
- scheduled or background execution.

The goal is not to create an observability platform. Capture only the metadata necessary to explain materially different execution cost or behavior.

- **REINFORCE:** Measure before optimizing context cost.
- **ASSESS:** Minimum execution/context provenance required by HE-001.
- **REJECT:** Treating token count alone as proof that context is wasteful.

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

Evidence trail:

- `01 Research/Sources/Simon Willison - Harness Engineering Findings - August 2026.md`
- `01 Research/Sources/Simon Willison — Harness Engineering Findings — 2026-09-01 through 2026-09-14.md`
- `01 Research/Sources/Simon Willison — Harness Engineering Findings — 2026-09-15 through 2026-09-21.md`
- Earlier Agentic Engineering Patterns research retained in this synthesis.

#### Durable findings

**The harness is the behavioral unit.** Coding-agent behavior emerges from the model, provider/runtime, persistent and task context, tools, skills, authority, execution environment, and feedback loop. Model identity alone is insufficient when any of those conditions materially differ.

**Execution provenance should be minimal but discriminating.** Capture only what is needed to explain or reproduce a material behavioral difference. Provider/backend identity, reasoning configuration, available capabilities, security boundaries, repository state, and evaluation method may matter. This is not a mandate for general-purpose telemetry or hidden reasoning capture.

**Effective authority is compositional.** The authority available to an agent may exceed the explicit tool list when filesystem, networking, DNS/host configuration, proxies, package systems, credentials, and command execution can be combined. Security assessment must evaluate the composed environment rather than each permission in isolation.

**Deterministic enforcement belongs below model judgment where practical.** A model or model-adjacent classifier may contribute risk signals, but it should not be the sole boundary for safety-critical authority when filesystem, network, credential, process, or sandbox constraints can enforce the boundary mechanically.

**Verification must produce relevant evidence.** An agent performing an inspection step or claiming completion does not establish correctness. Tests, invariants, acceptance checks, runtime observations, targeted inspection, and reproducible commands should be selected according to the property being verified.

**Independent executable evidence is stronger than another model opinion.** A workflow in which one actor demonstrates a defect with a failing test and another repairs it provides a clearer boundary than model-to-model approval alone. Multiple models do not automatically create independent verification.

**Verification should be risk-directed.** Review effort should concentrate on semantic-risk boundaries and required properties, not raw change volume or line-by-line inspection by default.

**Cheap implementation raises the value of scope discipline.** When agents reduce implementation cost, cost stops acting as a natural constraint against unnecessary features. Planning must still protect conceptual integrity and determine whether a change belongs in the system.

**Tool interfaces should be legible to the model.** Self-describing tool output may reduce errors, especially for weaker models, but can consume more context. Interface shape should be evaluated against reliability and total execution cost rather than token compactness alone.

**Skills are bounded capabilities, not automatically agents.** Narrow reusable skills and context isolation can be useful mechanisms. Subagents, fleets, and orchestration remain mechanisms to justify against demonstrated need, verification value, and execution cost.

**Persistent guidance needs continuing justification.** Guidance may compensate for current model limitations, encode intentional behavior, enforce deterministic requirements, or prevent demonstrated failures. Newer model capability is not sufficient evidence for removal.

**Model-generated continuation state is derived context.** Compaction,
handoff, and memory summaries can omit, distort, or introduce instructions.
They should not silently outrank authoritative project state, acceptance
criteria, policy, or source artifacts. A summary may be useful without being
authoritative.

**Instruction behavior has a client-owned discovery layer.** Repository
artifacts, discovery rules, precedence, and executed implementation are
different ownership boundaries. A client or runtime version can change which
instructions are loaded without changing the repository file.

**Voluntary restraint is not containment.** A model stopping after recognizing
that it reached a real system is a useful behavior signal, not a deterministic
security boundary. Effective authority must be assessed from the capabilities
the environment permits and composes.

**Secret usability and secret exposure are separate properties.** A bounded
process may need to use a credential without exposing the value to model
context, generated artifacts, logs, or unrelated tools. This is an assessment
question, not a requirement to adopt a particular credential UI.

**Generated artifact volume can exceed human understanding.** Faster production
of specifications, code, tests, and reports does not itself create conceptual
integrity, shared understanding, or safe delivery. Human-owned acceptance and
meaningful evidence remain necessary for consequential work.

#### Harness implications

These findings reinforce HE-001 investigation of context isolation, durable knowledge, execution provenance, provider/runtime conditions, effective authority, deterministic enforcement, evidence acquisition, independent evaluation, model-legible tools, and conceptual integrity.

They do not establish requirements for a VM, observability platform, provider router, subagent fleet, universal provider pinning, or preservation of hidden chain-of-thought.

#### Research disposition

- **REINFORCE:** Treat model + context + tools + authority + runtime + verification as the evaluated execution system.
- **REINFORCE:** Separate generation, evidence acquisition, and evaluation where the distinction improves confidence.
- **REINFORCE:** Preserve valuable, proven knowledge outside transient conversation.
- **ASSESS:** Minimum provenance needed to distinguish provider/backend and runtime effects in controlled evaluation.
- **ASSESS:** Effective authority created by combinations of otherwise ordinary capabilities.
- **ASSESS:** Whether ACR findings can produce independently reproducible evidence of failure and closure.
- **ASSESS:** Whether tool schemas trade a small context increase for a material reduction in model error.
- **ASSESS:** Whether generated summaries or handoffs can silently replace authoritative state; how client/version discovery and precedence affect executed instructions; whether credential paths minimize model-context exposure; and whether generated artifacts remain understandable and reviewable.
- **PARK:** Hardware-isolated sandboxing, model routing infrastructure, and additional orchestration until an observed requirement justifies them.
- **REJECT:** Treating model assertion, nominal model name, more agents, or more review tokens as sufficient evidence of correctness or reproducibility.

### Anthropic — Agent Skills and Skills-First Engineering

Evidence trail:

- `01 Research/Sources/Anthropic — Agent Skills and Skills-First Engineering — 2026-09-25.md`
- Anthropic, [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- Anthropic, [Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Agent Skills open format](https://agentskills.io/home)
- User-provided secondary video: [Anthropic Engineer Explains: What to Build Instead of AI Agents](https://www.youtube.com/watch?v=HIRDzMtuWFk)

This is an incremental synthesis of the existing Skills and progressive-
disclosure research, not a new architecture proposal.

**Capability before actor.** A general-purpose runtime plus a bounded,
task-specific capability can be simpler than a separate agent for every job.
An independent actor remains justified only when independent context,
authority, state, objective, parallelism, or evaluation is the demonstrated
requirement.

**Progressive disclosure is a context-supply property.** Skill metadata,
activation, full instructions, references, scripts, and enforcement are
separate stages. Discovery descriptions should identify both what a capability
does and when it applies. Direct, paraphrased, neighboring, and unrelated
prompts are useful routing tests.

**Reusable utilities need promotion criteria.** Saving a proven script can
reduce regeneration and variation, but a one-off success is not enough to make
code durable. Promotion should require repeated, stable, verified usefulness,
an owner, versioning, and regression coverage. The script is executable code
and a possible supply-chain boundary, not merely a prompt fragment.

**Corrections belong in the owning layer.** A failed run may implicate a Skill,
metadata, project configuration, discovery/installation, deterministic code,
runtime, or a one-time execution error. Diagnose and reproduce before editing;
make the smallest durable correction; rerun in a fresh session; and check that
the change does not create routing regressions. This is controlled knowledge
maintenance, not autonomous self-improvement.

**Format portability does not prove behavior portability.** An open Skill
folder can move between compatible harnesses while model, provider, tools,
runtime, and instruction-precedence differences still change execution. A
cross-harness result needs the conditions required to interpret the comparison.

**Verification is evidence-directed.** Establish acceptance criteria from the
request, authoritative specification, or independently established quality bar
before iteration. Use artifact-relevant evidence, fix demonstrated failures,
rerun, and report what remains unverified. Additional model personas can add
signals for subjective work but do not become external proof merely by being
separate agents.

#### Research disposition

- **REINFORCE:** Bounded capabilities, progressive disclosure, model-legible
  routing metadata, durable knowledge outside transient conversation, and
  evidence before completion.
- **ASSESS:** Capability-versus-actor boundaries; promotion criteria for
  reusable utilities; owning-layer diagnosis; routing regression tests; and
  runtime conditions for cross-model or cross-harness portability.
- **PARK:** Universal Skill registries, automatic self-modifying Skills,
  persona consensus as proof, and the claim that agents are generally obsolete.
- **REJECT:** Treating the video's headline, a successful one-off, or an
  agent's self-inspection as sufficient architecture or verification evidence.

No PMB, ACR, or HE architecture change follows automatically from this
material. It refines the existing modular-capability and verification
questions for HE-001.

### Austin Marchese — Loop / Graph Engineering

Research reviewed from:

- Austin Marchese — "You're Prompting Claude Wrong. Use this Stanford Method Instead"
- YouTube: https://www.youtube.com/watch?v=5nB8Rs4l0_M

This research is evaluated as evidence about workflow topology, dependency structure, iterative execution, and agent orchestration. It is not being adopted as a requirement for PMB or ACR.

#### Research Findings

**Workflow topology**

Useful workflow shapes include:

- sequential chains;
- parallel fan-out with convergence;
- conditional branching;
- bounded iteration loops.

The useful architectural question is not whether a workflow can be represented as a graph, but whether the information dependencies between steps justify the selected topology.

**Dependency before parallelism**

A workflow step should be considered sequential because it actually requires information produced by a prior step, not merely because the workflow was originally designed in that order.

Parallel execution can reduce unnecessary serialization, but independent execution can also duplicate work or lose required context when hidden dependencies exist.

**Branching and routing**

Conditional routing can reduce unnecessary capability exposure, but increasing routing complexity can outweigh its benefit.

Specific heuristics such as a fixed maximum number of branches should not be treated as architectural constraints without evidence.

**Bounded iteration**

Iterative workflows are useful when output can be evaluated against a meaningful quality bar and the loop has explicit termination conditions.

Infinite or poorly bounded iteration creates execution, cost, and reliability risks.

#### Harness Implications

HE-001 should evaluate:

- actual information dependencies between workflow stages;
- unnecessary serialization;
- valid opportunities for parallel execution;
- context availability during delegated execution;
- convergence requirements;
- conditional capability exposure;
- loop evidence and evaluation;
- retry, rollback, escalation, and termination behavior;
- whether orchestration complexity provides measurable value.

A graph representation or graph orchestration framework is not implied.

#### Research Disposition

- **ADOPT:** Use dependency relationships rather than assumed workflow order when assessing execution topology.
- **ASSESS:** Sequential, parallel, conditional, and iterative execution patterns where demonstrated workflow behavior makes topology relevant.
- **PARK:** A dedicated graph orchestration framework; useful conceptually, but no demonstrated requirement for PMB/ACR infrastructure.
- **REJECT:** Fixed branch-count heuristics as architectural rules.

### RoboNuggets — Gauntlet Loop

- Separating builder and evaluator perspectives can reduce self-approval.
- Evaluation is more useful when grounded in an explicit, task-relevant quality bar.
- Blind comparison can reduce evaluator bias during iterative refinement.
- Builder/critic loops and subagent fan-out can support refinement where repeated independent evaluation materially improves the result.
- Iterative optimization should begin from a strong direction, specification, design, or reference; otherwise the loop may optimize toward the wrong objective.

Harness implication:

Independent evaluation and explicit quality bars are useful patterns for workflows that benefit from iterative refinement. Builder/critic loops and subagent fan-out remain execution mechanisms to justify against observed needs rather than default Harness architecture.

### Matt Shumer

Primary reference:
https://somethingbig.ai/gauntlet-loop

YouTube / secondary demonstrations should be treated as supporting material rather than the authoritative description of the technique.

Focus:

- Agentic execution loops
- Builder / critic separation
- Long-running agent workflows
- External quality bars
- Sub-agent decomposition
- AI coding workflow evolution

Use for:
Research into iterative agent execution, evaluation boundaries, sub-agent orchestration, and externally grounded quality criteria.

Assessment rule:
Treat demonstrated techniques as research evidence. Do not adopt Gauntlet Loops, sub-agent fleets, or recursive execution solely because they produce impressive demonstrations. Evaluate measurable quality improvement, cost, boundedness, and applicability to PMB/ACR.

### Unlazy — Assertion vs Evidence

Research reviewed from:

- Leon Linx — Unlazy
- AI Labs — "GitHub's #1 Trending Author's New Claude Skill Is Insane"
- YouTube: https://www.youtube.com/watch?v=c47uqR7XB_c
- Repository: https://github.com/Leonxlnx/unlazy

Core finding:

A model's claim that work is complete must be distinguished from evidence that the requested outcome was actually achieved.

Relevant failure modes include:

- omitted scope;
- silently reduced scope;
- difficult portions skipped while easy portions are completed;
- file changes being mistaken for successful outcomes;
- model self-assessment being treated as completion evidence;
- superficial checks passing while the requested outcome remains unmet;
- failed verification being summarized as successful completion.

The useful pattern is:

    model assertion
    "I finished it."
           ≠
    completion evidence
    "Here is the observable proof."

Unlazy's gates illustrate one concrete implementation pattern: define an outcome, specify a check, define the expected result, run the check, and record evidence. The implementation itself is not being adopted.

Harness implication:

Completion verification should be treated as a distinct assessment concern. Where practical, completion should be established through observable evidence, deterministic checks, or independent evaluation rather than only the model's statement that the work is done.

#### Research Disposition

- **REINFORCE:** Deterministic evidence is stronger than model assertion when an outcome is mechanically observable.
- **ASSESS:** Where PMB/ACR currently rely on model-reported completion; whether deterministic completion evidence already exists; whether failed verification produces retry, escalation, rollback, or explicit incomplete status.
- **PARK:** Adopting Unlazy's full tree/gate architecture or its exact skill/file structure.

### Nate B. Jones — Multi-Model / Context Portability

### Sources

- "Stop Paying $200 For Work An $18 Model Can Do Inside Claude Code And Codex."
- YouTube: https://www.youtube.com/watch?v=4HvFqhtCb-A&t=927s

This research is evaluated as evidence about model/harness separation, context portability, bounded delegation, provider switching, and execution economics.

### Research Findings

- Model, harness, project context, and conversation are distinct layers.
- Project context stored in files can be reloaded by another model or session; conversation-only decisions generally cannot.
- Switching providers or models mid-session can have context and cache consequences and should not be assumed to be cost-neutral.
- Bounded, testable tasks with clear definitions of done are stronger candidates for cheaper models than ambiguous investigations or hidden-state troubleshooting.
- Fully loaded model economics include retries, validation, review, context/cache effects, and rework, not merely nominal token price.
- A concise handoff containing goal, current state, relevant files, constraints, definition of done, and checks can transfer a bounded job without transferring an entire conversation.
- Subagents receive intentionally bounded context; forks or other continuity mechanisms may have different context/caching characteristics.

### Research Disposition

- **REINFORCE:** Model, harness, project context, and conversation should be analyzed as distinct layers.
- **REINFORCE:** Durable project knowledge should live outside transient conversation when portability or recovery matters.
- **ASSESS:** Minimum context/provenance required to move bounded work between models or sessions without losing authoritative state.
- **ASSESS:** Criteria for selecting cheaper models for bounded work with clear acceptance criteria.
- **PARK:** Provider-specific GLM launcher/profile configuration as a PMB or Harness requirement.

## Nate B. Jones — Five Software Shapes / Four Durable Project Files

### Source

- "Nobody Laid Out The Five Kinds Of Software You Can Make. So I Did."
- YouTube: https://www.youtube.com/watch?v=joRXo6x7Pgk&t=1240s

The video describes five broad software shapes: local tool, web app, native phone app, background service, and hardware project. The durable Harness Engineering value is the decision principle: choose the simplest technical shape that satisfies the actual requirement.

### Four-File Context Model

Nate proposes a small durable project context model using:

- `project.md` — project purpose, current and desired state, runtime target, and privacy constraints;
- `decisions.md` — significant choices, options, recommendations, and rationale;
- `scenarios.md` — real situations the software must handle, serving as practical acceptance/test cases;
- `CLAUDE.md` / `AGENTS.md` — current agent-specific behavioral instructions.

The important distinction is that the first three represent durable project truth while the last describes how the current agent should behave.

This is a candidate context architecture to evaluate against PMB's existing artifacts. Do not copy the filenames or structure without first determining whether PMB already provides equivalent project state, decision, scenario, and instruction mechanisms.

### Research Disposition

- **ASSESS:** Whether PMB's current durable artifacts cleanly distinguish project purpose/state, decisions, scenarios/acceptance behavior, and agent-specific instructions.
- **REINFORCE:** Durable context should be portable across sessions/models while agent-specific behavior remains distinct.
- **REINFORCE:** Start with the simplest technical/workflow shape that satisfies demonstrated requirements.
- **PARK:** Adopting the exact four filenames or replacing existing PMB artifacts solely to match this model.
- **REINFORCE:** Avoid unnecessary multi-agent complexity when a simpler single-agent workflow satisfies the need.

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

The same underlying capability may be exposed to an AI workflow through multiple harness surfaces.

For example, a capability such as Context7 may be available through a connector, plugin, skill, or directly configured MCP server.

These surfaces should not automatically be treated as equivalent or assumed to be additive.

Harness assessment should determine:

- Which capability supply paths are active.
- Whether multiple paths expose overlapping functionality.
- Which component owns the underlying capability.
- Which surface controls discovery and activation.
- Whether duplicate capability exposure creates conflicting behavior, redundant context, routing ambiguity, or unnecessary tool selection.
- Whether multiple surfaces are intentional or merely different distribution mechanisms for the same capability.

The existence of multiple interfaces to a capability is not, by itself, evidence of a problem.

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

The current state should represent what is true now: active goals, decisions, unresolved questions, next actions, and relevant boundaries.

Historical records preserve what happened and why, but should not automatically carry the same authority as current state.

Fresh sessions should inherit current state rather than depending on reconstruction from conversational history.

This separation is especially important when multiple concurrent sessions work on the same project.

### Durable Knowledge Is a Harness Responsibility

Individual model executions are temporary.

Long-term capability emerges when useful discoveries survive beyond a single execution through explicit harness mechanisms rather than remaining inside a model's transient context.

Session outputs should become durable knowledge only after appropriate review, provenance, and evidence justify preservation.

The harness—not the model—owns long-term knowledge management.

Implications:

- Preserve valuable discoveries rather than conversation history.
- Treat durable knowledge as an architectural responsibility.
- Separate temporary execution state from shared project knowledge.
- Evaluate candidate knowledge before promoting it into persistent context.

### Independent Evaluation and Quality Bars

Generation and evaluation should not always be performed from the same perspective.

For workflows where quality can be evaluated against meaningful external criteria, separating the builder from the evaluator can reduce self-approval and support iterative improvement.

Evaluation should use an explicit, task-relevant quality bar rather than open-ended instructions to continue improving.

Builder/critic loops and subagent fan-out are execution mechanisms, not default architecture. They should be introduced only when the task benefits from independent evaluation or iterative refinement and the additional execution cost is justified.

A strong initial direction, specification, design, or reference should precede iterative optimization. Repeated evaluation can otherwise improve an output toward the wrong objective.

### Memory Consolidation

Session knowledge and durable project knowledge should not be assumed to have the same lifecycle.

Individual sessions and concurrent workstreams may produce discoveries, decisions, corrections, and temporary state that should remain isolated until their durable value is established.

Where consolidation is needed, candidate knowledge should be evaluated for duplication, contradiction, staleness, ownership, and continued relevance before becoming persistent project context.

Consolidation should preserve provenance and favor reviewable, reversible changes over autonomous rewriting of durable knowledge.

Candidate knowledge should be treated as a review artifact rather than automatically becoming durable project context.

### Workflow Structure Should Justify Automation

Multi-step AI work can often be understood as jobs, transitions, state, checks, loops, and human decision points.

Making that structure explicit can improve reliability when a workflow has demonstrated requirements for sequencing, isolation, independent verification, branching, recovery, or approval.

Explicit workflow structure does not imply that an orchestration framework is required.

Prefer the simplest execution model that solves the observed problem. Run and evaluate workflows manually or through existing tools before encoding them into dedicated orchestration infrastructure.

Automation should follow demonstrated workflow structure rather than define it prematurely.

### Composable Harness Capabilities

Harness capabilities may be composed, replaced, or temporarily disabled when their boundaries and dependencies are explicit.

Composability should be evaluated as a means of reducing unnecessary coupling, context, and execution surface rather than treated as an architectural objective by itself.

A capability boundary is valuable when it permits independent ownership, evaluation, replacement, isolation, or enforcement without introducing greater coordination complexity than the boundary removes.

Runtime composition should preserve traceability of the active capabilities and their effects on execution.

### Human Direction and Measurable Feedback

AI systems can execute increasingly large portions of engineering work, but human judgment remains important at points where goals, architecture, quality, or long-term maintainability are determined.

Prefer explicit outcomes, measurable acceptance criteria, reviewable intermediate checkpoints, and vertical increments when these reduce the cost of correcting a wrong direction.

Where subjective expectations can be converted into deterministic checks, tests, rubrics, or other observable signals, prefer those mechanisms over relying entirely on model judgment.

Automation should increase execution leverage without removing the human's ability to understand, redirect, and verify the work.

### Preserve Valuable Engineering Synchronization

Not all engineering friction is waste.

Some coordination creates shared understanding by exposing misunderstandings, conflicting assumptions, architectural disagreements, and incomplete reasoning.

Harness Engineering should automate repeatable coordination when doing so removes unnecessary work, but should not eliminate interaction that materially improves shared understanding.

The objective is to reduce coordination overhead without removing valuable synchronization.

### Model Capability Drift

Harness guidance should not be assumed to remain necessary simply because it was previously necessary.

As model capabilities evolve, persistent instructions, examples, workarounds, and guardrails may become redundant or may unnecessarily constrain model judgment.

Periodic context architecture audits should therefore evaluate whether guidance:

- addresses a currently demonstrated model limitation,
- encodes intentional project or team behavior,
- protects a safety or deterministic requirement,
- prevents a demonstrated failure mode, or
- persists primarily because of historical model limitations.

More capable models alone are not sufficient evidence for removal. Changes should be supported by observed behavior or evaluation.

### Research Disposition

Research findings are classified as:

- **ADOPT** — incorporated into documented Harness principles or decisions.
- **ASSESS** — incorporated into HE-001 or another formal assessment.
- **PARK** — retained as relevant research without current implementation justification.
- **REJECT** — explicitly determined not applicable or insufficiently supported.
- **REINFORCE** — confirms or strengthens an existing Harness principle or assessment without creating a new requirement.

A parked or rejected finding should retain enough context to explain why it was not pursued and may be reconsidered if new evidence, requirements, or observed operational problems change its relevance.

## Potential Impact on Harness Engineering

Questions to evaluate during HE-001:

- Which PMB guidance should become skills?
- Which skills should be decomposed into skill subdirectories?
- Which information should become references instead of instructions?
- Which instructions are duplicated?
- What should always load vs be discovered?
- Which client/runtime owns capability discovery, instruction precedence, and
  the implementation that actually executes?
- Which generated summaries or handoffs are derived context rather than
  authoritative project state, and how is reconciliation established?
- What evidence is sufficient to promote a repeated utility or correction into
  durable capability content?
- Which routing tests distinguish true activation from false positives and
  neighboring capability overlap?
- Which acceptance criteria and verification evidence are established before
  a producer begins iterating?

## Candidate Architectural Decisions

### Skill Hierarchies

Decision Status: Pending HE-001 Assessment

Description

Allow skills to be decomposed into subdirectories for progressive disclosure and reduced startup context.

Evidence Required

- Demonstrated reduction in always-loaded context
- Reduced duplication
- Simpler navigation
- No measurable usability regression
- Can repeated project workflows be discovered and converted into skills based on observed behavior rather than only pre-declared guidance?

### Context Architecture Audit

Decision Status: Pending HE-001 Assessment

Description

Periodically evaluate persistent context to identify opportunities for progressive disclosure, skill extraction, reference-based guidance, retrieval, enforcement, or removal.

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
- How should PMB/ACR keep generated compaction and handoff summaries from silently outranking authoritative artifacts?
- Which credential paths allow required work without exposing secret values to model context, logs, or unrelated tools?
- When does a reusable utility have enough repeated, verified value to justify durable ownership and maintenance?

## Assessment Observations

Applying HE-001 to a mature repository demonstrated that understanding the context supply chain, ownership boundaries, runtime assumptions, deterministic behavior, and context ownership provided significantly more architectural insight than reviewing prompts alone.

Assessment also demonstrated that separating:

- Observed behavior
- Inference
- Recommendation

produces clearer architectural analysis and reduces premature solutioning.

Future Harness Engineering assessments should prioritize observation before architectural recommendation.

## Status

Research only.
No architectural decisions made.
Await HE-001 evidence.

---
## Matt Pocock / Eric Tech — Modular AI Engineering Skills

### Source

Research reviewed from:

- Matt Pocock's public Claude Code skills repository
- Eric Tech — "Matt Pocock's Claude Code Skills Beat Superpowers Now"
- Related concepts presented in Matt Pocock's skills, including `grill-me`, `to-spec`, `to-tickets`, `implement`, `code-review`, writing-for-agents, and engineering vocabulary.

This research is being evaluated for architectural patterns and ideas. Matt Pocock's implementation is not being adopted or copied.

### Core Finding

A potentially useful distinction exists between **workflow orchestration** and **engineering capabilities**.

A prescribed AI workflow might require:

    brainstorm → spec → plan → implement → review → commit

A modular capability model instead exposes independently useful capabilities:

    context
    spec
    implement
    diagnose
    review
    architecture analysis

The Harness may then select or compose capabilities based on the current situation rather than requiring every task to pass through every stage.

This distinction should be evaluated during HE-001 rather than treated as an architectural decision.

### Modular Capability Principle

A capability should be independently useful where practical.

The existence of relationships between capabilities does not require a mandatory end-to-end workflow.

For example:

- implementation may consume a specification;
- review may consume the specification and implementation;
- architecture analysis may consume the resulting code.

These are legitimate dependencies.

The architectural concern is unnecessary workflow coupling, not dependency itself.

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

The Harness should coordinate capabilities without becoming a mandatory process pipeline.

### Candidate Engineering Patterns

The following concepts are candidates for evaluation rather than adopted architecture:

- progressive clarification of ambiguous requirements;
- one meaningful question at a time when interactive discovery is required;
- recommending concrete options rather than presenting blank questions;
- preserving the agreed "what" separately from implementation-specific "how";
- vertical feature/tracer-bullet slices rather than layer-only work decomposition;
- explicit dependency and blocking relationships between work items;
- test-first / red-green-refactor as an implementation discipline where appropriate;
- fresh-context review to reduce builder-context contamination;
- independent review dimensions for specification/intent and engineering standards;
- established engineering vocabulary as compact guidance for code quality;
- deep-module, interface, and seam analysis where architectural reasoning is actually required;
- periodic architecture-health analysis rather than continuous autonomous refactoring;
- concise capability instructions rather than procedural prompt bloat;
- lightweight capability selection/routing without imposing a universal workflow.

### Durable WHAT vs Implementation HOW

A durable planning artifact should preserve the agreed intent, requirements, constraints, decisions, and acceptance criteria.

It should avoid unnecessarily freezing implementation details that are expected to change as the code evolves.

This supports durable handoff and reduces the risk of a planning artifact becoming stale because it describes the implementation rather than the intent.

### Fresh-Context Review

Independent review should be evaluated separately from implementation context.

A reviewer's effectiveness may be reduced when it inherits the author's conversation, assumptions, intermediate reasoning, and self-justifications.

This reinforces the existing Harness Engineering interest in context isolation and independent verification.

This does not imply that every review requires a new agent or separate runtime. The implementation mechanism remains an assessment question.

### Specification / Intent vs Standards

Code review may contain at least two conceptually distinct questions:

1. **Specification / Intent**
   - Does the implementation do what was requested?
   - Does it satisfy the agreed behavior and acceptance criteria?
2. **Engineering Standards**
   - Is the implementation technically sound?
   - Does it violate applicable security, reliability, maintainability, architectural, or coding standards?

These dimensions may be independently useful even when performed by the same review system.

This is a candidate ACR assessment dimension, not a decision to create additional reviewer agents.

### Engineering Vocabulary as Compressed Context

Established engineering terminology may allow a capability to express substantial engineering knowledge with significantly less procedural instruction.

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

The relevant question for Harness Engineering is not whether these specific terms should be adopted.

The question is whether established engineering vocabulary can provide higher-signal guidance than lengthy procedural instructions.

### Minimum Sufficient Instruction

The objective should not be the shortest possible skill or instruction.

The objective is **minimum sufficient instruction**.

Guidance should contain the information that materially changes behavior while avoiding:

- redundant explanation;
- repeated project context;
- procedural instructions already enforced elsewhere;
- historical workarounds that no longer address demonstrated failures.

### Architecture Analysis as a Distinct Capability

Architecture analysis should be considered a capability that can be invoked when architectural concerns justify it.

It should not automatically become part of every implementation or code review.

A potential operating model is:

    implementation
        ↓
    normal verification/review
        ↓
    periodic or triggered architecture analysis

This avoids turning normal development into continuous architectural governance.

### Boundaries

The following are explicitly not conclusions of this research:

- Do not copy Matt Pocock's skills.
- Do not reproduce his skill names or directory structure merely for consistency.
- Do not mandate his workflow.
- Do not create a skill for every engineering activity.
- Do not replace deterministic enforcement with model judgment.
- Do not duplicate existing PMB mechanisms as generic Harness capabilities.
- Do not duplicate ACR capabilities in the Harness.
- Do not introduce orchestration infrastructure without demonstrated need.
- Do not treat modularity as an excuse to remove useful sequencing or dependency management.

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

> Should Harness Engineering primarily expose modular capabilities and selectively compose them, rather than encode a prescribed end-to-end AI development workflow?

This remains a research question pending evidence.

### Agent Skills Ecosystem / Matt Pocock — Additional Observations

Capture these observations:

- Skills can function as progressive-disclosure boundaries rather than simply reusable prompts.
- Handoff can preserve transient session state without duplicating durable project artifacts.
- Handoff can reference existing artifacts rather than copying their contents.
- Skill routing can be separated from capability execution.
- Repository-specific configuration can be separated from reusable skill behavior.
- Explicit state machines can provide bounded workflow transitions where evidence gates matter.
- High install counts identify research candidates but do not establish architectural value.
- Capability modularity should be evaluated against workflow coupling, context cost, maintenance, and routing complexity.

**Source:** Matt Pocock `skills` repository and the skills ecosystem.

## Greg Isenberg — Claude Code AI Employee Model

### Source

Research reviewed from:

- Greg Isenberg — "Claude Code New Features, Explained"
- YouTube: https://www.youtube.com/watch?v=SkY-tR9kf-k

This research is evaluated as evidence about emerging Claude Code workflow patterns. It is not being adopted as PMB or ACR architecture.

### Core Model

Isenberg describes an AI coding workflow as an operating system around the model rather than a single prompt.

The model consists of:

- workspace;
- memory;
- brief;
- ticket;
- eyes;
- review;
- schedule;
- permissions;
- skills;
- connectors;
- hooks.

The useful Harness Engineering question is whether these represent distinct responsibilities in PMB/ACR or merely different names for existing capabilities.

### Context and Working State

The proposed model separates several forms of project information:

- persistent working instructions;
- current roadmap / priorities;
- customer / domain context;
- task-specific briefs;
- review standards;
- historical / operational information.

The research reinforces the HE-001 requirement to determine which information is always loaded, conditionally loaded, retrieved, persisted, session-specific, project-level, authoritative, or advisory.

The proposed `CLAUDE.md`, `roadmap.md`, and `review.md` pattern should not be reproduced automatically. HE-001 should determine whether PMB already provides equivalent mechanisms and whether introducing additional artifacts would create duplication.

### Brief → Ticket → Execution → Verification → Review

The workflow describes a useful distinction between:

1. understanding the assignment;
2. planning the change;
3. defining a bounded ticket;
4. implementing the change;
5. inspecting the resulting system;
6. reviewing the diff;
7. determining whether human review is required.

This reinforces the existing HE-001 investigation of workflow orchestration, capability boundaries, deterministic verification, independent evaluation, bounded execution, and human decision points.

The sequence should not be treated as a mandatory universal workflow.

The assessment should determine which transitions provide demonstrated value and which exist primarily because the example workflow was designed as a fixed sequence.

### "Eyes" as Evidence Acquisition

The research uses "eyes" to describe the ability of an agent to inspect the result of its work through mechanisms such as application preview, browser interaction, test execution, console inspection, network inspection, and user-flow verification.

This is useful terminology for HE-001 because it distinguishes generation from evidence acquisition.

A harness should not assume that successful file modification means the requested outcome was achieved.

Evaluate whether PMB/ACR workflows currently provide sufficient access to relevant evidence and whether evidence is deterministic, externally observable, model-generated, trusted, or independently verified.

### Review as a Separate Quality Boundary

The research describes review as a separate stage from implementation.

It emphasizes reviewing the diff, checking against project standards, identifying unexpected changes, separating must-fix / should-fix / acceptable findings, and escalating higher-risk changes to deeper review.

This reinforces HE-001's investigation of independent evaluation and quality bars.

It does not establish that every change requires a separate reviewer, agent, or review workflow.

### Scheduled Work / Routines

The research describes recurring agent work such as morning briefs, weekly issue reviews, and pull-request reviews.

The important Harness Engineering observation is that scheduled execution can operate on durable project artifacts without requiring continuous interactive model sessions.

Evaluate whether PMB or ACR has a legitimate need for recurring execution and, if so, whether existing automation mechanisms are sufficient.

Do not introduce autonomous scheduled execution merely because Claude Code supports it.

### Parallel Sessions and Isolation

The research describes multiple concurrent Claude Code sessions using separate worktrees so that each session can operate on an independently scoped assignment.

This is directly relevant to HE-001's known concurrent-session problem.

Evaluate whether PMB session state is isolated; whether project-level context can remain shared; whether session-specific state has an explicit identity; whether separate execution environments prevent state collision; whether handoff artifacts can identify their originating session; and whether worktree isolation solves code isolation without solving context/handoff isolation.

Worktree isolation should not be assumed to solve PMB's handoff collision problem. They address different forms of concurrency.

### Permissions as Bounded Authority

The research divides actions into safe actions, actions requiring approval, and human-owned actions.

This reinforces the HE-001 distinction between deterministic enforcement and model-mediated judgment.

Evaluate whether current PMB/ACR permissions and execution boundaries provide explicit authority limits or merely rely on persistent instructions.

Do not replace deterministic enforcement with model judgment where the boundary can be enforced mechanically.

### Skills, Connectors, and Hooks

The research distinguishes:

**Skills**
- reusable ways of performing bounded work.

**Connectors**
- access to external systems and information.

**Hooks**
- deterministic actions around execution, such as formatting, validation, or checks.

This distinction is directly relevant to HE-001 Question 23 concerning multiple capability supply paths.

The assessment should determine whether PMB currently exposes equivalent capabilities through multiple surfaces and whether those surfaces are implementations, interfaces, discovery mechanisms, enforcement mechanisms, or distribution mechanisms.

Do not treat skills, connectors, and hooks as interchangeable.

### Engineering Implications

The strongest Harness Engineering observations from this research are:

- project context should explain the system before task execution;
- task scope should be bounded enough to permit meaningful review;
- generation should be followed by evidence acquisition;
- review should have an explicit quality boundary;
- concurrent execution requires isolation;
- authority should be bounded;
- reusable capabilities should be distinguished from orchestration;
- recurring work should be evaluated as a workflow capability rather than assumed to require autonomous agents.

These observations reinforce existing HE-001 questions rather than establishing new architecture.

### Assessment Status

Research input only.

No PMB or ACR architecture should be changed solely because of this workflow.

HE-001 should determine whether these concepts correspond to existing capabilities, duplicated capabilities, missing capabilities, or merely different terminology for existing mechanisms.

## Nate B. Jones — Multi-Model / Context Portability

### Sources

- "Stop Paying $200 For Work An $18 Model Can Do Inside Claude Code And Codex."
- YouTube: https://www.youtube.com/watch?v=4HvFqhtCb-A&t=927s

This research is evaluated as evidence about model/harness separation, context portability, bounded delegation, provider switching, and execution economics.

### Research Findings

- Model, harness, project context, and conversation are distinct layers.
- Project context stored in files can be reloaded by another model or session; conversation-only decisions generally cannot.
- Switching providers or models mid-session can have context and cache consequences and should not be assumed to be cost-neutral.
- Bounded, testable tasks with clear definitions of done are stronger candidates for cheaper models than ambiguous investigations or hidden-state troubleshooting.
- Fully loaded model economics include retries, validation, review, context/cache effects, and rework, not merely nominal token price.
- A concise handoff containing goal, current state, relevant files, constraints, definition of done, and checks can transfer a bounded job without transferring an entire conversation.
- Subagents receive intentionally bounded context; forks or other continuity mechanisms may have different context/caching characteristics.

### Research Disposition

- **REINFORCE:** Model, harness, project context, and conversation should be analyzed as distinct layers.
- **REINFORCE:** Durable project knowledge should live outside transient conversation when portability or recovery matters.
- **ASSESS:** Minimum context/provenance required to move bounded work between models or sessions without losing authoritative state.
- **ASSESS:** Criteria for selecting cheaper models for bounded work with clear acceptance criteria.
- **PARK:** Provider-specific GLM launcher/profile configuration as a PMB or Harness requirement.

## Nate B. Jones — Five Software Shapes / Four Durable Project Files

### Source

- "Nobody Laid Out The Five Kinds Of Software You Can Make. So I Did."
- YouTube: https://www.youtube.com/watch?v=joRXo6x7Pgk&t=1240s

The video describes five broad software shapes: local tool, web app, native phone app, background service, and hardware project. The durable Harness Engineering value is the decision principle: choose the simplest technical shape that satisfies the actual requirement.

### Four-File Context Model

Nate proposes a small durable project context model using:

- `project.md` — project purpose, current and desired state, runtime target, and privacy constraints;
- `decisions.md` — significant choices, options, recommendations, and rationale;
- `scenarios.md` — real situations the software must handle, serving as practical acceptance/test cases;
- `CLAUDE.md` / `AGENTS.md` — current agent-specific behavioral instructions.

The important distinction is that the first three represent durable project truth while the last describes how the current agent should behave.

This is a candidate context architecture to evaluate against PMB's existing artifacts. Do not copy the filenames or structure without first determining whether PMB already provides equivalent project state, decision, scenario, and instruction mechanisms.

### Research Disposition

- **ASSESS:** Whether PMB's current durable artifacts cleanly distinguish project purpose/state, decisions, scenarios/acceptance behavior, and agent-specific instructions.
- **REINFORCE:** Durable context should be portable across sessions/models while agent-specific behavior remains distinct.
- **REINFORCE:** Start with the simplest technical/workflow shape that satisfies demonstrated requirements.
- **PARK:** Adopting the exact four filenames or replacing existing PMB artifacts solely to match this model.
- **REINFORCE:** Avoid unnecessary multi-agent complexity when a simpler single-agent workflow satisfies the need.

## Harness Engineering — Research Sources

Sources we consider useful for ongoing Harness Engineering research.

## Primary Sources

### Simon Willison

Weblog: https://simonwillison.net/

Focus:

- Agentic engineering
- Coding agents
- Context engineering
- Tool design
- Security
- Sandboxing
- Prompt injection
- Model/runtime behavior
- AI-assisted engineering

Assessment:
High-value primary source. Follow regularly.
Follow via weblog rather than YouTube.

## Secondary / Scout Sources

- Matt Pocock
- Andrej Karpathy
- Dex Horthy / HumanLayer
- Nate B. Jones
- Matt Shumer
- Austin Marchese
- Dream Labs AI
- RoboNuggets
- NeuralNine
- Greg Isenberg
- Other AI workflow / agent-content sources

These sources are research inputs, not architectural authorities.

## Evaluation Rule

Popularity does not establish technical authority.

A source may be useful because it:

- demonstrates a novel implementation;
- identifies an emerging pattern;
- provides useful terminology;
- exposes a real operational problem;
- challenges an existing assumption.

Research findings must still be independently evaluated before they influence Harness Engineering architecture.

## Current Follow Recommendations

1. Simon Willison — Follow
2. Matt Pocock — Follow
3. Andrej Karpathy — Follow

Review this list periodically as the quality and relevance of sources change.

## Research Disposition

Research findings are classified as:

- **ADOPT** — incorporated into documented Harness principles or decisions.
- **ASSESS** — incorporated into HE-001 or another formal assessment.
- **PARK** — retained as relevant research without current implementation justification.
- **REJECT** — explicitly determined not applicable or insufficiently supported.
- **REINFORCE** — confirms or strengthens an existing Harness principle or assessment without creating a new requirement.

A parked or rejected finding should retain enough context to explain why it was not pursued and may be reconsidered if new evidence, requirements, or observed operational problems change its relevance.

