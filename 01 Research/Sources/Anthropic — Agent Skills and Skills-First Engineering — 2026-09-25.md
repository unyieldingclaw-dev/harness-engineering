# Anthropic — Agent Skills and Skills-First Engineering — 2026-09-25

## Purpose

Capture the Harness Engineering implications of the user-provided video
**Anthropic Engineer Explains: What to Build Instead of AI Agents** without
treating its headline or workflow advice as an architecture mandate.

This note separates primary Anthropic/Agent Skills evidence from the video's
secondary interpretation. It is research and assessment input only.

## Source identity

### Primary sources

- Anthropic, **Equipping agents for the real world with Agent Skills**:
  https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills
- Anthropic, **Skill authoring best practices**:
  https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices
- Agent Skills open format overview:
  https://agentskills.io/home
- Agent Skills evaluation guidance:
  https://agentskills.io/skill-creation/evaluating-skills

### Secondary video

- User-provided video: **Anthropic Engineer Explains: What to Build Instead of AI Agents**:
  https://www.youtube.com/watch?v=HIRDzMtuWFk
- User-provided transcript reviewed: `Pasted markdown(20260915-121614).md`
- Transcript SHA-256 at review: `457877a2433fcb5677e9c8cc71ac19e0a9b48cab53e195b7bae86b58855375`

The video is useful as an interpretation and operational example. The
Anthropic documentation and Agent Skills specification are the authority for
the format and progressive-disclosure claims.

## Claims and observations from the video

### 1. Specialize the capability before creating a specialist actor

The opening claim that Anthropic "stopped building agents" is intentionally
provocative. The video immediately narrows it: a general-purpose runtime can
often perform many jobs, so a separate agent should not be created for every
task. The proposed stack is:

```text
model / runtime
       +
task-specific skill
       +
bounded tools, references, and scripts
```

This is consistent with Anthropic's description of Skills as modular
capabilities that package instructions, metadata, and optional resources.
It is not evidence that agents, orchestration, or independent workers are
obsolete.

**HE interpretation:** Prefer capability composition when the same runtime can
perform the work with a smaller context and simpler ownership boundary. Create
a separate actor only when independent context, authority, state, objective,
parallelism, or evaluation is itself the demonstrated requirement.

### 2. Promote proven deterministic work into reusable utilities

At approximately 1:26–2:55, the video describes saving a repeatedly recreated
slide-styling script inside a Skill so later runs execute a proven utility
instead of regenerating equivalent code. The surrounding advice is ordinary
DRY engineering applied to an agent workflow:

- preserve a utility after it has demonstrated value;
- point the Skill at the utility rather than asking the model to rediscover it;
- rerun the task and compare the important behavior;
- keep the model focused on the task-specific variation.

**HE interpretation:** A saved script is not automatically trustworthy. It is
another executable component with ownership, version, dependency, security,
and regression concerns. The relevant promotion rule is repeated, stable,
verified usefulness—not merely that one output looked good.

### 3. Progressive disclosure makes capability discovery cheaper

At approximately 3:03–4:53, the video explains the three practical layers:

1. startup metadata such as name and description;
2. the full `SKILL.md` when a task matches;
3. references, scripts, templates, or other resources only when needed.

The video uses overlapping descriptions as the failure mode and recommends
descriptions that state both what the Skill does and when it should trigger.
It proposes three routing checks:

- an obvious request that should trigger;
- a differently worded request that should still trigger;
- an unrelated request that should not trigger.

Anthropic's primary documentation corroborates the progressive-disclosure
and deterministic-script mechanisms. The open format similarly defines a
Skill as a folder centered on `SKILL.md`, with optional scripts, references,
and assets.

**HE interpretation:** Capability metadata is part of the harness, not mere
documentation. Discovery, activation, execution, and enforcement must be
assessed separately. A Skill that cannot be found is unavailable in practice;
a Skill that activates on neighboring work can create context pollution or
conflicting authority.

### 4. Corrections should become durable only in the owning layer

At approximately 4:55–6:12, the video recommends diagnosing a failed run and
placing the correction in the smallest durable location:

- update procedural instructions when the process is wrong;
- add a reference when project-specific context is missing;
- add a clear rule when the same mistake recurs;
- save or repair code when the implementation is unreliable;
- rerun the original task and verify the fix.

The technique is valuable, but "update the Skill" is not a universal answer.
The actual owner may be:

- the Skill or its metadata;
- project configuration or instruction precedence;
- installation/discovery/distribution;
- a script, dependency, or deterministic validator;
- a runtime/tool boundary;
- a one-time execution error that should not be made durable.

**HE interpretation:** Durable correction requires evidence or reproducibility,
an identified owning component, a minimal change, and a fresh-session
verification pass that also checks for routing regressions. This is controlled
knowledge maintenance, not autonomous self-improvement.

### 5. Format portability is not behavior portability

At approximately 6:14–6:42, the video distinguishes the portability of an
open Skill folder from the behavior of the model and runtime that consume it.
The same Skill can be available to compatible harnesses while models still
differ in interpretation, capability, tool access, and reliability.

**HE interpretation:** A portable artifact is not proof of a portable
execution. Cross-harness or cross-model tests should record relevant runtime,
provider, tool, and instruction conditions and should expose hidden
assumptions rather than silently generalizing from one successful run.

### 6. Verification must occur before the result is returned

At approximately 6:43–9:00, the video frames the first output as an internal
draft. It recommends:

- establish acceptance criteria;
- produce the first version;
- inspect it using a verification method relevant to the artifact;
- fix every issue found;
- run another pass;
- return only after the criteria are met, with a short verification summary;
- state exactly what remains when something cannot be verified.

Examples include rendering slides and inspecting screenshots, checking
research claims against primary sources, running tests, or using additional
review perspectives for subjective work.

**HE interpretation:** The durable principle is evidence-directed verification,
not "run more agents" or "look at the output more times." Acceptance criteria
must come from the user request, authoritative specification, or an
independently established quality bar before iteration. A producer must not
silently weaken the criteria after seeing its own output. Persona or
subagent feedback can provide additional signals, but it is still model
judgment rather than external proof; the human remains the final judge for
taste, strategy, and other irreducibly subjective decisions.

## What this changes in Harness Engineering

This source strengthens existing HE concepts rather than introducing a new
top-level architecture:

- **Capability before actor:** test whether a bounded capability can be
  composed in the existing runtime before adding a new actor or orchestration
  surface.
- **Progressive disclosure:** treat capability metadata, activation, and
  deeper resources as distinct context-supply stages.
- **Owning-layer correction:** diagnose process, missing context, rule,
  implementation, configuration, discovery, and execution failures before
  editing durable artifacts.
- **Executable reuse:** promote scripts and validators only after repeated,
  stable, verified value; audit them as code and supply-chain boundaries.
- **Routing regression:** evaluate direct, paraphrased, neighboring, and
  unrelated prompts after changing descriptions or routing metadata.
- **Evidence before completion:** require task-relevant evidence and an
  explicit incomplete state when the acceptance criteria cannot be verified.
- **Portability boundaries:** distinguish open artifact format from behavior
  equivalence across providers, models, runtimes, and tool surfaces.

## Research disposition

- **REINFORCE:** bounded Skills, progressive disclosure, model-legible
  capability metadata, durable knowledge outside transient conversation, and
  evidence-directed completion verification.
- **ASSESS:** whether PMB/ACR currently separate capability from orchestration;
  whether Skill descriptions route accurately; when a repeated utility merits
  promotion; which component owns a durable correction; and which runtime
  conditions materially affect cross-model or cross-harness behavior.
- **PARK:** a universal Skill registry, a blanket rule to eliminate agents,
  automatic self-modifying Skills, and persona consensus as proof of
  correctness.
- **REJECT:** the video's headline as a general architectural conclusion;
  "the model looked at it" as verification; and the assumption that every
  successful one-off should become persistent Skill content.

No PMB, ACR, or HE architecture change follows automatically from this
source. HE-001 should determine whether these mechanisms solve an observed
problem and which existing component, if any, should own them.
