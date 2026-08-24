# GitHub AI Repos — Top 10

## Source

**Video:** Top 10 GitHub: AI videos, gorgeous diagrams, token savings and more  
**URL:** https://www.youtube.com/watch?v=SQrFue3LbwI

This is a research intake record. The repositories below are candidates for further investigation; inclusion does not imply adoption.

## Candidates for Follow-up

### OpenViking — ASSESS

**Repository:** https://github.com/volcengine/OpenViking

Potential relevance:

- File-oriented agent context and memory.
- Hierarchical context levels / progressive disclosure.
- Separation of durable content from retrieval/indexing.
- Potentially relevant to PMB context and memory architecture.

Research question:

> Can OpenViking's hierarchical storage/retrieval patterns provide useful design evidence without requiring PMB to adopt its architecture?

### NVIDIA SwitchYard — ASSESS

**Repository:** https://github.com/NVIDIA-NeMo/Switch

Potential relevance:

- Model routing.
- Stage-based routing.
- Escalation based on observed agent/tool behavior rather than only task classification.
- Potential relationship to cheaper-model delegation and recovery/escalation.

Research question:

> What model-routing responsibilities, if any, should a harness own versus an inference gateway such as LiteLLM?

### 5-Persona Advisory Board — ASSESS

**Repository:** https://github.com/harryvondiesel-web

Potential relevance:

- Multiple perspectives for decision analysis.
- Potential separation of evaluation criteria rather than simple persona role-play.

Important constraint:

Do not assume value comes from assigning famous-person or personality personas. Investigate whether genuinely different analytical/evaluation functions provide measurable value.

Research question:

> Can independent analytical roles produce useful disagreement or coverage that a single model perspective reliably misses?

### Shockwave — ASSESS

**Repository:** https://github.com/stephengpope/shock

Potential relevance:

- Local Markdown notes accessible to AI agents.
- Potential comparison point for the PMB / Obsidian / Git / Bishop-Notes model.

Research question:

> What does Shockwave do differently from a plain file-based knowledge workflow, and is any difference materially useful?

### Aura Code — ASSESS LIGHTLY

**Repository:** https://github.com/DusanCar-sudo/aura

Potential relevance:

- Alternative coding-agent architecture.
- Potential comparison of model providers, tools, skills, memory, permissions, and agent-loop boundaries.

Research question:

> Does Aura Code make a materially different or useful choice about agent-harness boundaries?

Do not adopt merely because it is an alternative to existing coding agents.

### Needle — PARK / WATCH

**Repository:** https://github.com/cactus-compute/needle

Potential relevance:

- Very small specialized local models.
- Local tool-calling / edge execution.
- Supports the broader idea that not every AI operation requires a general-purpose reasoning model.

Current disposition:

Interesting infrastructure direction, but no demonstrated PMB/HE requirement for a tiny local model.

Research only if a concrete local classification, extraction, routing, or tool-selection problem emerges.

### Modular / Mojo — WATCH

**Repository:** https://github.com/modular/modular

Potential relevance:

- AI-focused runtime/compiler infrastructure.
- Performance-oriented AI programming.
- Potential future impact on local inference and AI infrastructure.

Current disposition:

Monitor the ecosystem. No current PMB/HE architectural action.

### Semantica — PARK

**Repository:** https://github.com/semantica-agi/sema

Potential relevance:

- Knowledge graphs for structured context.

Current disposition:

Knowledge graphs are potentially useful, but introducing graph infrastructure without a demonstrated retrieval/context problem would risk unnecessary complexity. Revisit only if file-based context plus search/retrieval proves insufficient.

### Minto Pyramid Skill — REINFORCE

**Repository:** https://github.com/millwright-labs/mi

Potential relevance:

- Answer-first communication.
- Organizing reasons and evidence beneath a clear conclusion.

Current disposition:

Useful communication pattern, but not a Harness architecture requirement.

### Diagram Design — REINFORCE

**Repository:** https://github.com/cathrynlavery/diagram-design

Potential relevance:

- Clearer, more useful AI-generated diagrams.
- Visual communication conventions.

Current disposition:

Potentially useful for documentation and communication, but not core Harness Engineering architecture.

## Other Repositories Mentioned

These were included in the source video but are not currently prioritized for PMB/HE research:

- Omarchy — https://github.com/basecamp/omarchy
- MoneyPrinterTurbo — https://github.com/harry0703/MoneyPrinterTurbo
- Public APIs — https://github.com/public-apis/public-apis
- Holehe — https://github.com/megadose/holehe
- Lumina — https://github.com/Bino5150/lumina
- Imagine CLI — https://github.com/AhmedAburady/imagine
- TLDR Radio — https://github.com/mat-nolen/tldr-radio

These are retained here so they are not lost, but they do not currently justify additional Harness Engineering investigation.

## Research Threads

The candidates above naturally cluster into four broader research threads:

### Context & Memory

- OpenViking
- Shockwave

Question:

> How should durable agent knowledge be stored, summarized, indexed, and retrieved without unnecessarily loading everything into model context?

### Model Routing & Escalation

- NVIDIA SwitchYard
- Needle

Question:

> Can bounded work use cheaper or specialized models and escalate when observable evidence indicates the current model is struggling?

### Independent Evaluation

- 5-Persona Advisory Board
- Relationship to Unlazy completion verification and ACR

Question:

> How can genuinely different evaluation perspectives improve coverage without creating unnecessary reviewer/persona complexity?

### Agent Harness Architecture

- Aura Code
- Shockwave
- Modular/Mojo as a longer-term infrastructure watch item

Question:

> What architectural boundaries are emerging in alternative agent harnesses, and which differences are materially useful?

## Disposition Summary

| Candidate | Disposition | Current reason |
|---|---|---|
| OpenViking | ASSESS | Context/memory architecture is directly relevant |
| SwitchYard | ASSESS | Evidence-driven model routing/escalation |
| 5-Persona Advisory Board | ASSESS | Potentially useful independent evaluation roles |
| Shockwave | ASSESS | Direct comparison to local Markdown agent workspace |
| Aura Code | ASSESS LIGHTLY | Alternative harness architecture |
| Needle | PARK / WATCH | Specialized local model; no demonstrated need |
| Modular / Mojo | WATCH | Infrastructure trajectory, not current architecture |
| Semantica | PARK | Knowledge graph may be premature |
| Minto Pyramid Skill | REINFORCE | Useful communication pattern |
| Diagram Design | REINFORCE | Useful documentation pattern |

## Guardrail

Do not turn repository discovery into architecture by default.

The purpose of this record is to preserve candidates for investigation. A candidate should move into a durable Harness Engineering principle only when investigation produces evidence of a problem, meaningful benefit, duplication, misplaced responsibility, or a simplification opportunity.