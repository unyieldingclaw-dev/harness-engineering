# Agent Harness & Workflow Repos — 2026-08-30

## Purpose

Research notes mined from the requested public repositories. This is a source-derived research artifact, not a recommendation to adopt any project wholesale.

Primary question: what concrete patterns are worth carrying into harness engineering, PMB, and the work Memory Bank without importing unnecessary complexity?

## Repositories reviewed

| Repository | Primary signal | Confidence |
|---|---|---|
| DeepSeek Harness | Plugin-oriented harness architecture | High |
| Omarchy | Opinionated workstation + authoritative manual / operational docs | Medium |
| anydoc | Agent Skill + deterministic local document normalization | High |
| Herdr | Persistent agent runtime, pane state, agent-native control | High |
| Orca | Parallel isolated worktrees, orchestration UI, review/annotation loop | High |
| Claudex Loop | Cross-model adversarial review, bounded rounds, human gates | High |
| OpenMontage | Pipeline/stage skills, capability envelope, approval gates, auditable decisions | High |
| OmniRoute | Context/tool-output compression and deterministic stacked pipelines | High |
| Archify | Typed IR + deterministic validation + evidence-grounded artifacts | High |
| Claude of Tanks | Lean agent index, directory-scoped skills, generated facts, exact command index | High |

---

# 1. DeepSeek Harness

Source: https://github.com/deepseek-ai/deepseek-harness

DeepSeek Harness describes itself as an open-source agent harness using an **everything-is-a-plugin** architecture, powered by Cordis. It is explicitly a developer preview with compatibility-breaking changes expected. The repository points agents to `AGENTS.md`, a development guide, and architecture documentation. fileciteturn193file0L2-L6

### Useful lessons

- **Plugin boundary as a composability mechanism.** The interesting idea is not “everything must be a plugin”; it is the deliberate ability to add capabilities through a common extension boundary.
- **Architecture documentation is part of the operating model.** The repo explicitly routes contributors/agents to architecture and development documentation rather than expecting the README to carry everything.
- **Agent instructions are first-class.** `AGENTS.md` is a named entry point rather than an implicit convention.
- **Compatibility honesty matters.** The project explicitly labels itself developer-preview quality and warns about breaking changes. Harness research should distinguish stable contracts from experiments.

### Do not copy blindly

The plugin-everything architecture could become indirection for its own sake. PMB should only introduce an extension boundary where there is an actual independently evolving capability.

---

# 2. Omarchy

Source: https://github.com/omacom/omarchy

Omarchy is an opinionated Linux distribution. Its README makes `manual/` the authoritative source and organizes documentation into basics, applications, configuration, and operational topics including updates, troubleshooting, security, snapshots, and unattended installs. fileciteturn194file0L2-L6

### Useful lessons

- **One authoritative source of truth.** Explicitly identify where operational truth lives instead of allowing duplicated instructions to drift.
- **Documentation organized by operating concern.** Installation, daily use, configuration, security, troubleshooting, and recovery are separated.
- **Opinionated defaults can reduce cognitive load.** A harness can provide a small set of known-good paths instead of exposing every possible configuration.
- **Operational documentation deserves equal status to feature documentation.** This matters for agent systems because recovery and maintenance are part of the product.

### PMB relevance

This reinforces keeping PMB's operational instructions deterministic and discoverable, while avoiding a giant “everything manual” that becomes another context-bloat source.

---

# 3. anydoc

Source: https://github.com/firecrawl/anydoc

anydoc is a Rust document converter that normalizes many office/document formats into GitHub-Flavored Markdown through a shared document model and serializer. It also ships as an Agent Skill usable by Claude Code, Codex, Cursor, OpenCode, and other compatible agents. The README emphasizes local conversion and content-based format detection. fileciteturn195file0L2-L2

### Useful lessons

- **Normalize once, consume many ways.** A common intermediate representation prevents every downstream consumer from implementing its own document parsing.
- **Keep deterministic work out of the LLM.** Parsing and normalization are ordinary software problems; the agent should consume the normalized result.
- **Skills can be thin adapters over deterministic tooling.** The Skill teaches an agent how to invoke the converter rather than replacing the converter with prompt instructions.
- **Local-first is valuable for context and privacy.** The normal conversion path stays local; OCR is an explicit exception.
- **Capability detection should be explicit.** The tool distinguishes supported formats and error classes rather than pretending every input is equivalent.

### PMB relevance

Strong precedent for a thin Skill layer over deterministic scripts/tools. This is especially relevant to reducing session startup context: the Skill can describe *how to use* a capability while the tool performs the heavy work.

---

# 4. Herdr

Source: https://github.com/herdrdev/herdr

Herdr presents itself as a runtime for coding agents. It keeps terminals and sessions alive in a background server, tracks panes as working/blocked/idle, lets agents control Herdr through CLI/socket APIs, and supports existing CLI agents rather than replacing them. It is a single Rust binary and supports local and remote use. citeturn0search11

### Useful lessons

- **Separate agent runtime from agent implementation.** Herdr owns session/process lifecycle while Claude Code/Codex/etc. remain the actual agents.
- **Persist state outside the conversational context.** Session continuity is a runtime concern, not something that should be stuffed into prompts.
- **Expose machine-readable state.** Working/blocked/idle is a much better coordination primitive than forcing an agent to infer terminal state from screenshots or prose.
- **Agent-native control can be narrow and explicit.** CLI/socket operations give agents bounded authority over the runtime.

### PMB relevance

This is a useful architectural boundary: PMB should not become the process supervisor merely because it stores memory. Runtime/session state and durable memory are different concerns.

---

# 5. Orca

Source: https://github.com/stablyai/orca

Orca is an AI orchestrator that runs multiple CLI agents side-by-side, each in its own Git worktree. It supports parallel worktrees, persistent terminal splits, remote SSH worktrees, diff annotation, agent-driven CLI operations, notifications, and usage/rate-limit tracking. It supports many CLI agents rather than binding itself to one. fileciteturn196file0L2-L2

### Useful lessons

- **Isolation is the primitive for parallel agent work.** Separate worktrees are safer than asking multiple agents to mutate one working tree.
- **Orchestration can sit above existing agents.** The harness does not need to replace Claude Code or Codex.
- **Human review should attach to concrete artifacts.** Diff-line annotation is a better review surface than a generic chat transcript.
- **Remote execution and local execution can share the same work model.** Worktree/session identity is more durable than a particular machine.
- **Usage visibility belongs in the orchestration layer.** Tracking agent usage and rate limits helps manage operational constraints without putting that logic into every agent.

### PMB relevance

The parallel-worktree pattern is relevant to future PMB experiments involving competing agents, but it is not a reason to add parallelism now. Isolation should precede concurrency.

---

# 6. Claudex Loop

Source: https://github.com/chaseai-yt/claudex-loop

Claudex Loop is a particularly relevant pattern. It has Claude establish intent and a plan, then has Codex attack the locked plan in a read-only sandbox. Review rounds are bounded, the builder/reviewer roles are separated, and the final artifact is cross-inspected by the rival model. The repo explicitly states the invariant: whoever made the thing does not check the thing. It keeps `PLAN.md` and `PLAN-REVIEW-LOG.md` as durable artifacts. fileciteturn197file0L2-L6

### Useful lessons

- **Cross-model disagreement is more valuable than adding more same-model reviewers.** Different models/providers create a useful independence boundary.
- **Role separation is the key invariant.** Builder ≠ reviewer.
- **Read-only review is a strong safety boundary.** Reviewers should not silently repair what they are supposed to evaluate.
- **Bound review loops.** `MAX_ROUNDS` prevents fake convergence and infinite reviewer churn.
- **Persist the argument, not just the verdict.** A review log preserves why a plan changed.
- **Human gates belong at load-bearing decisions.** The system minimizes user interruptions rather than eliminating them.

### PMB / ACR relevance

This is directly relevant to the planned multi-model evaluation experiment and to ACR architecture. It also supports the principle that “more agents” is not automatically better; independence and bounded roles matter more.

---

# 7. OpenMontage

Source: https://github.com/calesthio/OpenMontage

OpenMontage describes a pipeline-driven agentic production system. Its agent guide says to read the contract first, avoid improvising the workflow, inspect the actual capability envelope, select a pipeline before using tools, and then read the relevant manifest/stage skill. It also uses approval gates and multi-point self-review with explicit validation and auditable provider decisions. fileciteturn198file0L2-L2

### Useful lessons

- **Capability envelope before action.** Ask “what can the current environment actually do?” before planning around assumed capabilities.
- **Pipeline selection before tool use.** This reduces ad hoc agent behavior.
- **Stage-specific Skills.** Load only the instructions needed for the current stage rather than a giant global instruction set.
- **Approval gates for irreversible/costly work.** Generate/commit expensive artifacts only after a human checkpoint.
- **Self-review should use concrete checks.** Their workflow validates rendered output, audio, subtitles, delivery promises, etc., instead of relying only on an LLM saying “looks good.”
- **Provider decisions can be auditable.** Record why a provider/tool was selected rather than leaving the decision opaque.

### PMB relevance

The stage-specific Skill pattern is highly relevant to context-bloat reduction. It is a concrete example of progressive disclosure: global instructions stay small; specialized instructions load only when the stage requires them.

---

# 8. OmniRoute

Source: https://github.com/diegosouzapw/OmniRoute

OmniRoute contains an unusually explicit context-compression architecture. Its current documentation describes separate engines for command/tool output (RTK) and prose (Caveman), with deterministic stacked pipelines such as `rtk -> caveman`. RTK targets noisy terminal/build/test/git output; Caveman targets natural-language condensation. It also supports previews, configurable thresholds, raw-output recovery, and specialized tool-result compression. citeturn1search0turn1search4turn1search8

### Useful lessons

- **Compress by data type, not with one universal summarizer.** Terminal output and prose have different structure and should be treated differently.
- **Compression should happen before context-limit failure.** OmniRoute exposes an auto-trigger threshold rather than waiting until the session is already broken.
- **Deterministic filters are preferable where structure is known.** A `git diff` filter can preserve changed files and actionable hunks without asking an LLM to summarize the whole thing.
- **Stacking can be explicit and testable.** `RTK -> Caveman` is a pipeline with defined stages rather than magic compression.
- **Raw-output recovery is important.** Compression without a recovery path can turn a debugging aid into information loss.
- **Compression should be observable.** Preview/test endpoints and analytics make it possible to verify that compression is actually helping.

### Critical PMB implication

This is one of the strongest external signals for the context-bloat problem already observed in PMB/work MB. The target should not be “make prompts shorter” generically. It should be **control the composition of startup context and tool output by source type, with thresholds and measurement**.

Do not blindly copy OmniRoute's reported savings. Their numbers are project-specific and eligibility-dependent. The architecture is the useful part; PMB should measure its own results.

---

# 9. Archify

Source: https://github.com/tt-a1i/archify

Archify is an Agent Skill that creates architecture/workflow diagrams from repositories or descriptions. It uses a typed JSON intermediate representation and deterministic validation. Its Skill instructs the agent to validate after every candidate edit and immediately before handoff; failed validation must not be described as success. It also supports evidence-grounded source tracing and revision comparison. citeturn1search6turn1search7

### Useful lessons

- **Typed intermediate representations create a hard boundary between agent intent and generated artifact.**
- **Deterministic validators should own objective correctness.** The LLM can propose; the validator decides whether the artifact satisfies structural rules.
- **Validation receipts are useful evidence.** A compact machine-readable result can replace a long prose claim that something was checked.
- **Freeze validated artifacts.** Archify explicitly says a passing final validation freezes the candidate; this prevents “validated, then accidentally modified” states.
- **Evidence grounding is stronger than visual plausibility.** Architecture diagrams should trace back to source evidence instead of inventing topology.

### PMB / harness relevance

This maps cleanly to PMB's broader goal of making memory artifacts trustworthy: structured state + deterministic checks + receipts is stronger than “the agent says the state is correct.”

---

# 10. Claude of Tanks

Source: https://github.com/Kevin-Liu-01/Claude-of-Tanks

This repository is unusually useful as an example of a large agent-built project with a lean agent entry point. `AGENTS.md` explicitly says to keep the index lean, load linked files on demand, prune no-op instructions, and keep generated facts in generated blocks. It uses directory-scoped `SKILL.md` files and keeps full command catalogs out of the main agent index. citeturn1search1turn1search3

### Useful lessons

- **Lean index, on-demand detail.** This is probably the clearest external confirmation of the progressive-disclosure pattern we want for PMB/work MB.
- **Directory-scoped Skills beat one giant instruction file.** Load only the rules for the area being changed.
- **Generated facts should be separated from authored instructions.** This reduces accidental editing and makes regeneration explicit.
- **Command catalogs should not live in the always-loaded index.** Keep the pointer in the index and the exhaustive list elsewhere.
- **Use exact tools for exact questions.** The repo explicitly separates `rg` for exact strings from higher-level knowledge tooling.
- **Keep the agent index boring.** Its job is routing, not teaching the entire system.

### PMB/work MB relevance

Very high. The user's recent startup-context bloat makes this pattern directly actionable: the always-loaded layer should contain routing rules and invariants, not encyclopedic project knowledge.

---

# Cross-repository synthesis

## Patterns that recur strongly enough to matter

### 1. Progressive disclosure

DeepSeek, anydoc, OpenMontage, Archify, and Claude of Tanks all support the same broader idea in different forms: keep the global layer small and load specialized detail only when needed.

**PMB rule:** the startup context should be an index/router, not a knowledge dump.

### 2. Deterministic work outside the model

anydoc, Archify, OmniRoute, and OpenMontage repeatedly separate objective processing/validation from LLM reasoning.

**PMB rule:** if a task can be performed deterministically, do not spend model context and tokens asking an LLM to approximate it.

### 3. Context compression should be selective

OmniRoute is the clearest example: terminal output, prose, tool results, and other payload classes receive different treatment.

**PMB rule:** measure startup bloat by source category and attack the largest contributors first. Do not add a generic summarizer merely because context is large.

### 4. Durable artifacts beat conversational memory

Claudex Loop uses explicit plan/review artifacts; Archify uses typed IR and receipts; OpenMontage uses pipeline manifests and decision logs.

**PMB rule:** important state should exist in small, explicit, inspectable files rather than only in conversation history.

### 5. Independent review is more valuable than reviewer count

Claudex Loop's builder/reviewer separation is the strongest example. The lesson is not “use two models everywhere.” It is “avoid having the same actor grade its own work.”

**ACR experiment:** compare same-model, cross-model, and independent-harness review using identical questions and measure factual accuracy rather than reviewer volume.

### 6. Bounded authority

Herdr gives agents bounded runtime control. Claudex keeps review read-only. OpenMontage uses approval gates before costly actions.

**PMB rule:** agents should get the minimum authority needed for the current phase.

### 7. Evidence and receipts

Archify validates artifacts; OpenMontage records provider decisions; OmniRoute exposes compression previews/analytics; Claudex preserves the review argument.

**PMB rule:** prefer compact evidence/receipts over long prose claims of correctness.

### 8. Isolation before parallelism

Orca's worktree-per-agent model demonstrates the safe foundation for parallel agents.

**PMB rule:** if we ever add parallel agent execution, isolate state first. Do not start by letting several agents share a mutable workspace.

---

# Highest-value takeaways for PMB

1. **Attack startup context composition first.** The recent 40% startup-context problem is not theoretical; it is an operational defect. Build measurement around where those tokens come from.
2. **Keep the always-loaded memory layer lean.** Use indexes, routing pointers, and invariants. Move detailed knowledge behind explicit retrieval.
3. **Adopt stage/directory-scoped Skills rather than expanding one global Skill.** This is strongly supported by OpenMontage and Claude of Tanks.
4. **Separate deterministic processing from LLM reasoning.** Especially for file inventory, state extraction, validation, formatting, and repetitive transformations.
5. **Introduce compression by payload class only after measuring the actual bloat.** OmniRoute provides a useful design reference, not a reason to copy its implementation.
6. **Use compact receipts for machine-verifiable state.** Avoid making an LLM reread long narrative logs to discover whether a prior step succeeded.
7. **Preserve durable artifacts for important decisions.** Plans, review logs, manifests, and validation receipts are better than relying on chat history.
8. **Use independent reviewers when correctness matters.** More reviewers of the same model are not automatically more independent.
9. **Bound loops and authority.** MAX_ROUNDS, read-only review, approval gates, and explicit opt-outs are recurring safety patterns.
10. **Do not turn every good idea into PMB architecture.** These projects contain useful patterns, but many are solving different scale/problems. Adopt only what addresses an observed PMB failure or a concrete upcoming requirement.

---

# Follow-up experiments worth recording

- Measure PMB startup context by category: core instructions, memory index, retrieved memory, project docs, tool metadata, Skills, and generated state.
- Compare a monolithic Skill against progressive-disclosure Skills on identical tasks; measure startup tokens, task completion, errors, and latency.
- Test deterministic compression on large tool outputs before considering LLM summarization.
- Run the planned multi-model review experiment with a fixed set of standard questions and classify findings as correct / partially correct / wrong / unsupported.
- Test whether durable receipts let a fresh session recover project state with materially less context than replaying narrative history.
- If parallel agents are introduced later, require isolated worktrees or equivalent state isolation as a prerequisite.

## Source-quality note

This document intentionally separates repository observations from recommendations. Repository claims are based on the repositories' current public documentation/code surfaces available during this research pass. Marketing claims, benchmark savings, star counts, and “best” language are not treated as evidence that a pattern should be adopted. The architecture and operational mechanisms are the primary research signal.
