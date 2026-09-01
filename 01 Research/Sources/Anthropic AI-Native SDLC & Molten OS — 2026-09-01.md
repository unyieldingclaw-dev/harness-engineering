# Anthropic AI-Native SDLC & Molten OS — 2026-09-01

## Source set

- User-supplied transcript/screenshot extraction from **Claude Code's New INTENT.MD, What is It?** covering Anthropic's AI-Native SDLC playbook: intent → spec → plan → implementation → test/review → deploy → maintenance.
- Official Anthropic playbook referenced by the video: https://claude.com/blog/the-ai-native-sdlc-playbook
- **Molten OS Core**: https://github.com/switch-dimension/molten-os-core

The supplied transcript is a third-party walkthrough of the Anthropic playbook; where the official playbook is referenced, treat the walkthrough's interpretation separately from Anthropic's own text.

## Important findings

### 1. The bottleneck moves from implementation to the rest of the SDLC

The central thesis is that coding/build time can shrink dramatically with agents, so planning, specification, testing, governance, review, deployment, and maintenance become proportionally more important.

**Harness implication:** do not optimize only the build agent. The harness should improve the entire artifact/control chain.

### 2. Intent is an explicit artifact

The walkthrough presents `intent.md` as the output of an originator/agent discovery conversation. The originator may be a customer, product manager, engineer, or other stakeholder. The originator reviews and corrects the generated intent before it becomes the input to later stages.

**Useful principle:** capture intent explicitly before implementation context becomes fragmented across conversation history.

### 3. Artifact chain enables clean context handoff

The walkthrough explicitly describes an artifact chain:

```text
intent.md
   ↓
spec.md
   ↓
plan.md
   ↓
implementation
   ↓
tests / review / release artifacts
```

Each downstream agent can start from the appropriate artifact rather than inheriting the complete prior conversation.

This is highly relevant to context economics: **handoff artifacts are a mechanism for reducing conversational coupling and avoiding repeated full-history context.**

### 4. Plan quality should be judged by handoff completeness

A strong plan should contain enough information about files to change, work order/tasks, risks/constraints, and proof/checks that another engineer or agent can implement it without reopening the discovery conversation.

**Harness implication:** measure whether planning artifacts are independently actionable rather than measuring their length.

### 5. Governance belongs in the execution path

The transcript describes versioning plans/intents/specs, tracking ownership, using policies/skills during generation, and using hooks to prevent unauthorized actions such as changing restricted folders or upgrading packages without approval.

The key principle is:

> Put critical governance at enforcement points, not only in prose instructions.

This matches the distinction already present in our research between guidance and actual enforcement.

### 6. Subagents and worktrees should be used for independent work

The walkthrough describes breaking plans into independent tasks, using subagents, and using Git worktrees when changes can safely happen in parallel.

**Reusable rule:** isolate state before adding concurrency.

### 7. Verification should be continuous, not a single late phase

The AI-native SDLC model moves testing into implementation rather than waiting for a separate downstream QA phase. Deterministic lint/build/test checks are emphasized, with browser/E2E checks where warranted.

The transcript also recommends continuous evals for skill changes and model upgrades, using a fixed set of representative historical issues/outcomes.

**Harness implication:** treat evaluator fixtures as part of the harness, not as optional after-the-fact testing.

### 8. Deployment should remain gated by explicit governance

The model described includes separate PR/security/CI review instances and hooks that can block deployment until approval or required gates are satisfied.

**Useful distinction:** automation can accelerate the path to a gate without removing the gate.

### 9. Maintenance can become an event-driven loop

The walkthrough presents an aspirational maintenance model in which alerts, tickets, Slack messages, schedules, or metric changes can trigger an agent to diagnose a problem, create an intent, propose action, and proceed within pre-defined authority boundaries.

This is a concrete bridge between the SDLC model and the **Loop Engineering / `improve-system`** research already captured in this repository.

### 10. No one-size-fits-all harness

The walkthrough explicitly notes that teams may use Superpowers, BMAD, custom processes, loops, graph approaches, or other systems. The important point is standardization and fit to the environment rather than adopting one vendor's exact workflow.

## Molten OS Core — repository findings

The current public repository describes **Molten OS Core** as a set of AI agent Skills intended to move from a raw product idea to a validated, testable landing page. It separates a **product pipeline** from cross-cutting utility Skills and writes stage artifacts under `molten-docs/`. fileciteturn229file0L2-L2

### Product pipeline

The repository's product Skills are explicitly staged:

1. `molten-validate` — adversarial product validation / scoring.
2. `molten-brand` — audience, pain, positioning, message, voice.
3. `molten-design` — design system / preview.
4. `molten-landing` — landing-page creation/audit.

The repo says these are run in order and each writes artifacts under `molten-docs/`. fileciteturn229file0L2-L2

### Utility separation

Two utility Skills are intentionally outside the product pipeline:

- `molten-search` — routes search/research to appropriate backends.
- `molten-skill-manage` — installs/updates/removes/list/finding Skills.

This is a useful structural pattern: **pipeline-specific Skills should not become a giant miscellaneous utility bag.** fileciteturn229file0L2-L2

### Relevance to Harness Engineering

Molten OS reinforces several patterns already found elsewhere:

- narrow, named stage Skills;
- explicit handoff artifacts;
- a pipeline for work that genuinely has ordered dependencies;
- utility Skills kept separate from pipeline stages;
- consistent naming to make capability boundaries discoverable;
- adversarial validation before building;
- artifacts stored outside the immediate conversation.

The repo also demonstrates a useful **decision-support before implementation** pattern: validate the idea and identify the riskiest assumption before committing to brand/design/implementation work. fileciteturn229file0L2-L2

## Combined implications for PMB / Harness Engineering

### Adopt / reinforce

**Artifact-chain handoffs.**
Use small durable artifacts to transfer intent, constraints, plans, and verification data between independent sessions/agents rather than forwarding full transcripts.

**Enforced governance.**
When a rule matters, place a hook/check/permission gate at the point where violation would occur.

**Continuous verification.**
Keep a small representative eval set so changes to skills, models, prompts, or workflow behavior can be regression-tested.

**Stage-scoped Skills.**
A Skill should describe the capability needed for a stage/task rather than becoming a universal project manual.

**Separate pipeline and utility capabilities.**
This keeps orchestration understandable and prevents unrelated utilities from bloating every workflow.

**Maintenance loops.**
Treat event-driven maintenance as a future loop pattern, with explicit authority and verification boundaries.

### Assess

**`intent.md` for PMB planning workflows.**
Potentially useful for significant changes where a human/agent discovery conversation currently leaves intent scattered across chat. Do not add an intent artifact to every trivial task.

**Continuous evaluator fixtures.**
Could become part of PMB's own regression discipline for Skills/context changes, especially given the observed startup-context bloat problem.

**Automatic artifact generation.**
Useful where the next artifact has stable structure and deterministic acceptance checks; avoid creating paperwork simply to satisfy a process template.

### Do not copy blindly

- the full AI-native SDLC artifact chain for every tiny PMB change;
- all Molten OS Skills as a package;
- mandatory human gates on low-risk deterministic operations;
- agent-generated intent as authoritative without originator correction;
- subagent parallelism before state isolation and concurrency controls exist.

## Key design principle added to the research corpus

> **A context handoff should transfer the smallest durable artifact that contains the information needed for the next stage—not the entire conversation that produced it.**

This deserves to be considered alongside the existing startup-context and Skill-size work because it directly attacks unnecessary context carry-over.

## Suggested experiment

Take one representative PMB engineering task and compare:

### A. Conversation carry-forward

Next agent receives a large portion of the previous session/context.

### B. Artifact handoff

Next agent receives only:

- intent/requirements;
- approved plan;
- relevant project/context index;
- required Skill;
- verification criteria.

Measure:

- startup/input tokens;
- task success;
- errors/omissions;
- number of clarifying turns;
- total wall-clock time;
- human interventions.

The hypothesis is that **a sufficiently complete artifact handoff can reduce context load without reducing task quality**.

## Sources

- Anthropic AI-Native SDLC Playbook: https://claude.com/blog/the-ai-native-sdlc-playbook
- Switch Dimension Molten OS Core: https://github.com/switch-dimension/molten-os-core
- User-provided transcript/screenshot extraction, 2026-09-01

## Disposition

**High-value research.** Add to the core Harness Engineering synthesis, but do not adopt a specific vendor workflow wholesale.
