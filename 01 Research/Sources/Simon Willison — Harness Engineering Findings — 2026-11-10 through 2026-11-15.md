# Simon Willison — Harness Engineering Findings — 2026-11-10 through 2026-11-15

## Scope and status

This source record captures Simon Willison material reviewed for Harness Engineering during 2026-11-10 through 2026-11-15. It preserves source observations separately from Harness Engineering analysis and disposition.

These findings are research evidence only. They do not authorize PMB, ACR, or other architecture changes.

## Sources

- [LLM 0.37.2](https://simonwillison.net/2026/Nov/11/llm/)
- [datasette-llm 0.1a11](https://simonwillison.net/2026/Nov/14/datasette-llm/)
- [aws-emulator](https://simonwillison.net/2026/Nov/11/aws-emulator/)
- [Prompting Claude Fable 5.2](https://simonwillison.net/2026/Nov/10/prompting-claude-fable-52/)
- [Datasette security advisory GHSA-mw6v-83vp-7c4c](https://github.com/simonw/datasette/security/advisories/GHSA-mw6v-83vp-7c4c)
- [LLM security advisory GHSA-v9h7-jj4g-f862](https://github.com/simonw/llm/security/advisories/GHSA-v9h7-jj4g-f862)
- [LLM security advisory GHSA-r639-rr7r-x72w](https://github.com/simonw/llm/security/advisories/GHSA-r639-rr7r-x72w)

## Findings

### 1. LLM 0.37.2 credential leakage across OpenAI-compatible adapters

**Observed claim/evidence:** Simon reported two bugs where a missing provider-specific credential could fall back to `OPENAI_API_KEY`, causing the wrong credential to be sent to a third-party OpenAI-compatible endpoint. The issue also affected an alternate chat-completions path. Version 0.37.2 fixes the problems.

**Harness Engineering analysis:** Protocol compatibility does not establish credential isolation. Provider selection, endpoint selection, adapter path, and credential resolution are separate security-relevant behaviors.

**Research implication:** HE-001 should test that credentials fail closed when provider-specific configuration is absent, that the selected endpoint receives only its intended credential, and that alternate execution paths preserve the same boundary. Do not infer safety from an OpenAI-compatible API surface.

**Classification:** Challenges Current Thinking  
**Disposition:** REINFORCE fail-closed credential isolation; ASSESS provider/endpoint/key-resolution tests; PARK a general credential-broker architecture.

### 2. Datasette `owner_id` confusion and private stored-query disclosure

**Observed claim/evidence:** A Datasette parameter intended to filter by owner also influenced authorization identity. An unauthenticated caller could access private stored-query metadata (title, description, and SQL), although the issue did not permit execution of the query or access to underlying tables.

**Harness Engineering analysis:** Requesting actor, resource owner, and requested resource are distinct security concepts. A filter or metadata parameter must not redefine the actor used for authorization.

**Research implication:** Evaluate policy composition and identity boundaries explicitly. Test restrictive, permissive, conflicting, and empty/unauthenticated cases. This is a security research input, not evidence of a defect in HE-001 or PMB.

**Classification:** Challenges Current Thinking  
**Disposition:** REINFORCE explicit actor/resource/requester separation; ASSESS composed authorization tests; PARK any product-specific authorization redesign.

### 3. Ownership framing changes model behavior

**Observed claim/evidence:** Simon highlighted Anthropic documentation reporting that Claude Fable 5.2 consolidated duplicate code in fewer than 10% of runs without explicit ownership framing, versus approximately 75% when instructed that it was the owner and long-term maintainer.

**Harness Engineering analysis:** Responsibility framing may change behavior, but the result is a narrow behavioral observation. Ownership language can also encourage unnecessary refactoring, scope expansion, or unauthorized durable changes.

**Research implication:** Treat ownership framing as a testable hypothesis. Evaluate it on bounded tasks with explicit authority and acceptance criteria before changing persistent guidance. Do not equate “ownership” with permission to modify project policy or architecture.

**Classification:** Worth Investigating  
**Disposition:** ASSESS bounded ownership-framing experiments; PARK a default ownership instruction.

### 4. datasette-llm 0.1a11 and local model adapters

**Observed claim/evidence:** datasette-llm added support for OpenAI-compatible endpoints, including local LM Studio, Ollama, and llama.cpp models.

**Harness Engineering analysis:** This demonstrates integration capability and local execution options, not reviewer quality, reproducibility, or suitability for ACR.

**Research implication:** When comparing local and hosted execution, record endpoint, adapter, model, tools, runtime, and evaluation conditions. Existing local-model support is sufficient for targeted experiments; installing another application is not justified by this source alone.

**Classification:** Relevant  
**Disposition:** REINFORCE conditions disclosure; ASSESS local-vs-hosted evaluation; PARK new local-model architecture.

### 5. AWS `aws-emulator` as a local service emulator

**Observed claim/evidence:** Simon described the official Rust-based AWS local emulator as a way to avoid live paid API keys. Docker is required on macOS and Windows; the project showed unusually high recent commit volume.

**Harness Engineering analysis:** Local emulation can reduce live credentials and improve repeatability, but emulator fidelity is an independent question. Commit volume is not evidence of correctness.

**Research implication:** Assess emulators by the behaviors needed for a bounded workflow, including failure modes and fidelity to the live service. Local execution may reduce authority and cost while creating false confidence if the emulator omits relevant behavior.

**Classification:** Worth Investigating  
**Disposition:** REINFORCE local isolation and fidelity testing; ASSESS task-specific emulator coverage; REJECT commit volume as a quality proxy; PARK adoption of this emulator.

### 6. Decision models and task economics

**Observed claim/evidence:** Simon discussed model pricing, reasoning budgets, and task selection around newer decision-oriented models such as Muse Ember and Kimi.

**Harness Engineering analysis:** Nominal token price, benchmark score, or model label does not establish total workflow cost or quality. Retries, latency, verification, correction, and failure consequences are part of the execution economics.

**Research implication:** If HE-001 evaluates model selection, compare complete verified tasks under controlled conditions and include rework and evaluation cost. This does not justify a model router.

**Classification:** Worth Investigating  
**Disposition:** REINFORCE end-to-end task-cost evaluation; ASSESS routing/escalation only if an observed workflow need emerges; PARK universal model routing.

## Material not promoted

The reviewed OSM referrer-policy, Datasette URL-fragment, animation, and kernel-porting items did not materially change current Harness Engineering questions. They are not promoted into durable findings.

## Consolidated dispositions

- **REINFORCE:** fail-closed credential isolation; explicit requester/owner/resource separation; conditions disclosure; local-emulator fidelity testing; evaluation by total verified task cost.
- **ASSESS:** provider/endpoint/credential-resolution paths; authorization composition; bounded ownership framing; local-versus-hosted conditions; task-specific emulator fidelity; routing only if an observed need exists.
- **PARK:** credential broker; default ownership framing; universal model router; adoption of `aws-emulator` without a demonstrated requirement.
- **REJECT:** treating API compatibility as credential isolation; commits, token price, or benchmark score as proof of quality; treating local emulation as automatically faithful.

## Relationship to HE-001

This source reinforces existing HE-001 questions about execution provenance, effective authority, deterministic enforcement, modular capability, and independent evaluation. It adds no architecture requirement and no implementation decision.
