# David Ondrej — Eight Skills Deep Evidence Pass

**Research date:** 2026-09-02  
**Source:** David Ondrej, “Don’t use AI Agents without using these 8 skills” (YouTube, August 2026), plus inspection of the public `davidondrej/skills` repository.  
**Status:** Research / assessment input; no architecture adopted by default.

## Purpose

Assess the eight skills David highlights in the video as harness-engineering evidence rather than as a package to install wholesale.

The useful question is not “Which eight skills should we copy?” It is:

> **Which underlying capabilities, controls, and interaction patterns are independently valuable to our harness, and what evidence would justify adopting them?**

The video presents eight skills:

1. `global-agent-guardrails`
2. `git-worktree`
3. VPS server management
4. `goal-loop`
5. `setup-help`
6. `decisions`
7. `anti-sleep`
8. `deepapi`

The current public repository has evolved beyond the video and now contains substantially more skills, so the video should be treated as a dated selection from a living repository rather than a complete catalog.

## Source-derived observations

### 1. Global agent guardrails

David's first skill is a pre-tool-call/pre-exec guard intended to block catastrophic shell operations while allowing agents to run with reduced interactive approval. The examples include destructive recursive deletes, disk/filesystem destruction, fork bombs, piping remote content into a shell, destructive Git history operations, remote branch/tag deletion, permission ownership destruction, and related GitHub CLI hazards.

The current implementation uses a shared dangerous-patterns file and a hook/adapter model. The public skill explicitly describes the mechanism as a safety “bouncer,” not a sandbox, and acknowledges that obfuscation can evade regex-style matching.

**Assessment:**

- **ADOPT / REINFORCE:** Structural pre-execution controls are stronger than prompt-only prohibitions.
- **USEFUL REFINEMENT:** Keep policy data separate from matching/wiring machinery so one policy source can feed multiple agent adapters.
- **HIGH-VALUE RESEARCH:** Cross-harness payload normalization. A guard that claims support for several agents must prove that it extracts the actual command from each agent's hook payload.
- **REJECT:** Treat a denylist as a complete security boundary. It is a catastrophic-accident control, not a malicious-agent sandbox.
- **CONVERGENT:** Reinforces the existing principle that an authority boundary that matters to correctness must be structurally enforced.

This independently reinforces the earlier finding that PMB's BLOCK / CONFIRM / WARN model should not be flattened into a binary denylist.

### 2. Git worktree

The skill establishes one task = one worktree = one agent session. The primary checkout is treated as an integration point rather than a scratchpad. Worktrees are short-lived, changes are reviewed before merge, and nothing auto-merges.

The detailed implementation goes beyond simply creating a worktree: it calls out copied environment files, fresh dependencies, local database/service identity, port collisions, generated files, caches, and hook path assumptions. It also explicitly warns that worktrees isolate files but do not isolate shared Git state, shared services, or ports.

**Assessment:**

- **ALREADY PRESENT / CONVERGENT:** Worktree isolation is already a major harness finding from Ras Mic/Ralphy research.
- **ADOPT AS PRINCIPLE:** Isolation must include operational dependencies, not just the Git working directory.
- **USEFUL REFINEMENT:** Model isolation as an explicit contract with dimensions: files, Git state, credentials, processes, ports, databases, network access, and publication authority.
- **HIGH-VALUE RESEARCH:** Determine which isolation dimensions are actually required for each class of PMB/ACR work rather than imposing maximum isolation everywhere.
- **REJECT:** Assuming a worktree is a security sandbox.

### 3. VPS server management

In the video, David uses separate VPS environments for long-running agents and describes a skill for remotely connecting to and managing those environments. The motivation is operational isolation, persistent execution, and easier management of multiple remote agents.

This is primarily an execution-topology capability, not a software-development methodology.

**Assessment:**

- **ADOPT AS CONCEPT:** Execution environment is a first-class harness concern and should be distinguishable from the model and from the agent's instructions.
- **USEFUL REFINEMENT:** Harness execution profiles should be able to describe local vs remote execution, persistent vs ephemeral runtime, and relevant isolation properties.
- **HIGH-VALUE RESEARCH:** Persistence and recovery semantics for long-running agents: what survives terminal closure, host sleep, process failure, network interruption, and machine replacement?
- **REJECT FOR NOW:** A VPS requirement or vendor-specific VPS management skill. PMB does not need remote infrastructure merely because it is useful to David's multi-agent topology.

This reinforces the existing Execution Profile / execution identity work from the Ras Mic, oMLX, and DeepSeek/Qwen research.

### 4. Goal loop

The video presents `/goal` as a way to transform a vague instruction into a persistent loop with a concrete, verifiable end condition. The current public skill describes a `plan → act → test → review → iterate` loop with lifecycle states and explicit guidance to use it only when the task is sufficiently long, the stop condition is verifiable, and the repository is agent-ready.

The current skill also explicitly warns that `/goal` is not a budget command, safety boundary, “run forever” mechanism, or replacement for planning. It includes anti-reward-hacking language and requires concise documentation in goal prompts.

**Assessment:**

- **ADOPT AS PRINCIPLE:** Long-running autonomous execution requires an explicit contract and a verifiable stop condition.
- **ADOPT / REINFORCE:** Verification must be part of the loop rather than a final afterthought.
- **USEFUL REFINEMENT:** Model goal execution as a bounded execution contract: objective, success criteria, validation method, resource budget, mutation scope, escalation/stop conditions, and publication authority.
- **HIGH-VALUE RESEARCH:** How should PMB distinguish mechanical long-running work from exploratory work where an artificial scalar stop condition would distort behavior?
- **REJECT:** Unlimited autonomous continuation as a default.
- **REJECT:** Treating a goal prompt itself as a safety boundary.

This strongly converges with the earlier Ralphy finding that unlimited iteration is a negative pattern and with the Karpathy research on bounded experimental loops.

### 5. Setup-help

`setup-help` is intentionally small. It maintains a canonical checklist internally while exposing only one coherent current step and a short headline-only list of remaining steps. It re-audits the current step and remaining list against the canonical checklist after each interaction and adds newly discovered prerequisites immediately.

**Assessment:**

- **HIGH-VALUE CANDIDATE:** Persistent task-state representation that survives local clarification without losing the larger workflow.
- **ADOPT AS PRINCIPLE:** Separate “current interaction detail” from “complete task state.”
- **USEFUL REFINEMENT:** This is a concrete pattern for reducing context loss during interactive setup and troubleshooting.
- **HIGH-VALUE RESEARCH:** Whether this should generalize into a reusable Task State / Task Contract primitive or remain a specialized setup interaction pattern.
- **REJECT:** Adding a generic universal checklist engine without evidence that PMB needs one.

This is particularly interesting for PMB because it addresses the same failure mode seen in context-heavy workflows: local conversational detail can crowd out durable task state.

### 6. Decisions

The `decisions` skill asks the agent, after substantial work, to identify only important choices it is genuinely uncertain about and to avoid listing decisions where it already believes the solution is best. It is manual-only and deliberately concise.

The video frames this as changing the human review target: instead of asking a human to inspect thousands of generated lines, surface the small number of consequential judgments that deserve human attention.

**Assessment:**

- **HIGH-VALUE CANDIDATE:** Decision extraction as a distinct review artifact.
- **ADOPT AS PRINCIPLE:** Human review should concentrate on consequential uncertainty and authority-bearing choices, not merely diff volume.
- **USEFUL REFINEMENT:** Pair decision extraction with existing plan/spec/review artifacts rather than creating an independent decision system by default.
- **HIGH-VALUE RESEARCH:** Determine whether agent-reported uncertainty correlates with actual defect/architecture risk. The agent's statement that it is confident is not evidence that it is correct.
- **CONVERGENT:** Closely related to the previously identified decision-elicitation capability and to `ask-then-build` research.
- **REJECT:** Replacing independent code/review/evidence checks with a self-reported decision list.

Important distinction: **decision review is an attention-allocation mechanism, not a correctness oracle.**

### 7. Anti-sleep

The video demonstrates a macOS-specific mechanism using `caffeinate`. The current implementation is more disciplined than a simple background command: it uses a one-shot LaunchAgent, tracks state, supports duration or process-bound operation, verifies after launch, and has a fallback when launchctl bootstrap fails.

**Assessment:**

- **USEFUL MICRO-PATTERN:** Operational helpers should be state-aware, deterministic, and verify their own effect.
- **ADOPT AS GENERAL PRINCIPLE:** Long-running execution needs explicit runtime-liveness semantics and post-action verification.
- **REJECT FOR HARNESS CORE:** macOS anti-sleep itself is an environment-specific convenience, not a core harness capability.
- **HIGH-VALUE RESEARCH:** Runtime liveness should be represented independently of “agent is running.” A process can be alive while the useful task is stalled.

The deeper lesson is therefore about **runtime supervision and liveness**, not about `caffeinate`.

### 8. DeepAPI

The video presents DeepAPI as a single API surface for web search, deep research, scraping, browser actions, email, image generation, and related capabilities. The skill uses progressive disclosure so endpoint instructions are loaded only when relevant. The current repository includes a main router plus generated/reference material for specific endpoint families.

The current skill also contains explicit credential handling guidance and says not to expose the API key.

**Assessment:**

- **ADOPT AS PRINCIPLE:** External capabilities should be presented to agents through stable, documented capability boundaries rather than forcing the model to rediscover API usage repeatedly.
- **USEFUL REFINEMENT:** A capability adapter can reduce repeated model reasoning and context cost when a deterministic external service is repeatedly used.
- **HIGH-VALUE RESEARCH:** Define a generic external-capability contract covering authentication, scope, rate/cost limits, input/output schema, provenance, failure semantics, and side-effect authority.
- **REJECT FOR NOW:** DeepAPI as a PMB dependency. It is a vendor/product choice, and the video contains product claims that are not independently established by this research pass.
- **REJECT:** “One API key gives the agent everything” as a default security architecture. Capability aggregation can increase blast radius if authority is not separately scoped.

This converges with prior Firecrawl/Exa research: **web ingestion/research is a replaceable capability boundary, not a reason to hard-wire a particular provider into the harness.**

## Cross-skill findings

### 1. The eight skills are really four capability families

The video's list looks heterogeneous, but the underlying capabilities cluster naturally:

| Family | Skills | Harness lesson |
|---|---|---|
| Safety & authority | global-agent-guardrails | Structural pre-execution enforcement |
| Isolation & runtime | git-worktree, VPS, anti-sleep | Execution environment and liveness are first-class |
| Autonomous execution | goal-loop | Explicit objective + verification + bounded continuation |
| Human interaction & external capability | setup-help, decisions, deepapi | Preserve task state, surface consequential judgment, expose deterministic capabilities through adapters |

This is more useful to the harness than copying eight skill folders.

### 2. The strongest new finding is decision review

The earlier David research identified **decision elicitation** from `ask-then-build`. This video adds the complementary **post-work decision extraction** pattern.

Together they suggest a potentially useful lifecycle:

```text
Before work:
  consequential ambiguity
      ↓
  elicit decisions
      ↓
  human chooses

During work:
  execute against agreed decisions

After work:
  extract consequential uncertain decisions
      ↓
  human reviews only those choices
      ↓
  independent verification still runs
```

This should be researched as a review-attention mechanism, not adopted as a substitute for independent verification.

### 3. The strongest new runtime finding is liveness

Across VPS management, anti-sleep, Herdr, Ralphy, and persistent goal loops, a recurring problem appears:

> **“Process exists” is not equivalent to “work is progressing.”**

A mature harness may need distinct runtime states such as running, progressing, blocked, waiting, failed, completed, and stale/stalled. This is a research candidate, not an implementation commitment.

### 4. Goal loops require a stronger contract than “keep going”

The useful unit is not a loop command. It is a bounded contract:

```text
Objective
+ Success criteria
+ Verification method
+ Mutation scope
+ Resource budget
+ Retry policy
+ Escalation / stop conditions
+ Publication authority
```

The loop mechanism is replaceable. The contract is the harness capability.

### 5. Isolation is multidimensional

Git worktrees solve one important class of collision, but David's own material demonstrates that real execution isolation also involves environment files, dependencies, databases, ports, services, credentials, network access, and publication paths.

Therefore:

> **Do not call an execution environment “isolated” without specifying what is isolated.**

This reinforces the existing Isolation Contract candidate from Ralphy research.

### 6. Skills are not necessarily the unit of architecture

David's repository demonstrates an effective skill packaging strategy, but the underlying harness concepts cut across skill boundaries. For example:

- guardrails → enforcement capability;
- worktree → isolation capability;
- VPS → execution environment capability;
- goal loop → bounded execution capability;
- decisions → human-judgment interface;
- DeepAPI → external capability adapter.

The harness should model capabilities and authority first; Skills can be one delivery mechanism.

## New candidate capabilities

### Candidate: Decision Review Artifact

A structured, concise artifact identifying consequential choices made during a change that merit human review.

**Constraint:** self-reported uncertainty is evidence about attention allocation, not proof of risk or correctness.

### Candidate: Runtime Liveness Contract

A runtime state model that distinguishes process existence from useful progress and provides observable evidence for running/blocked/stalled/completed states.

### Candidate: Goal Execution Contract

A portable contract for long-running agent work independent of any particular `/goal` or Ralph implementation.

### Candidate: External Capability Contract

A standard interface for external services that specifies capability, authentication scope, cost/rate limits, input/output contract, provenance, failure behavior, and side-effect authority.

### Candidate: Isolation Contract

A declared set of isolation dimensions rather than a binary isolated/not-isolated flag.

## Evidence gaps

The following are not established by this video/repository pass and require further testing or independent evidence:

1. Whether decision-review output actually predicts architectural defects or risky choices.
2. Whether long goal loops improve net delivery quality after accounting for review/rework cost.
3. How often agent self-reported uncertainty misses the most important decision.
4. Which isolation dimensions are necessary for specific harness workloads.
5. Whether persistent VPS execution materially improves reliability versus local persistent runtimes.
6. Whether a unified external API materially improves context efficiency after accounting for provider abstraction and security scope.
7. Whether runtime liveness states can be measured reliably enough to drive automation.
8. Whether goal-loop contracts should be standardized or remain tool-specific.

## Relationship to existing Harness Engineering findings

This pass reinforces rather than replaces prior work:

- **Ras Mic / Ralphy:** isolation contracts, bounded execution, retry classification, publication authority, execution identity, capability negotiation.
- **Karpathy / autoresearch:** bounded budgets, explicit evaluator, reward-hacking resistance, provenance, and human-controlled improvement boundaries.
- **Anthropic AI-Native SDLC:** committed artifacts, human accountability, continuous verification, hooks as gates, independent review, and bounded maintenance loops.
- **Agent Skills & Context Efficiency:** progressive disclosure, deterministic scripts, capability catalogs, context-cost measurement, and avoiding unnecessary Skills.
- **Earlier David Ondrej pass:** multi-harness command extraction, reviewer independence, decision elicitation, coordination authority, progressive disclosure, and over-abstraction risk.

## Disposition summary

**ADOPT / REINFORCE**

- Structural pre-execution enforcement over prompt-only safety claims.
- Multidimensional isolation contracts.
- Bounded, verifiable long-running execution.
- Decision review as a human-attention mechanism.
- Persistent task state separate from current conversational detail.
- Runtime liveness as a first-class concern.
- Stable external capability boundaries with explicit authority and provenance.
- Progressive disclosure and deterministic scripts where they reduce repeated model work.

**ASSESS / PRIORITY RESEARCH**

- Decision Review Artifact.
- Runtime Liveness Contract.
- Goal Execution Contract.
- External Capability Contract.
- Isolation Contract formalization.
- Relationship between self-reported uncertainty and actual risk.

**PARK**

- VPS infrastructure as a PMB requirement.
- macOS anti-sleep as a harness feature.
- DeepAPI as a vendor dependency.
- A generic universal checklist engine.

**REJECT / CONSTRAIN**

- Regex guardrails as a complete sandbox.
- Worktree = full isolation.
- Goal loops with unlimited continuation by default.
- Self-reported decisions as a correctness oracle.
- Universal “one API key for everything” authority aggregation.
- Copying the eight skills wholesale merely because they are popular.

## Bottom line

David Ondrej's eight skills are more valuable to Harness Engineering as **evidence of recurring capability patterns** than as eight artifacts to install.

The highest-value additions to our research model are:

1. **Decision Review** — focus human attention on consequential uncertainty.
2. **Runtime Liveness** — distinguish a live process from useful progress.
3. **Goal Execution Contract** — define bounded autonomous work independently of a loop implementation.
4. **Multidimensional Isolation** — explicitly state what the environment does and does not isolate.
5. **External Capability Contract** — separate capability access from authority, credentials, cost, and side effects.

The central lesson is consistent with the rest of the harness research: **the durable abstraction is not the skill name; it is the capability, contract, evidence, and authority boundary underneath it.**
