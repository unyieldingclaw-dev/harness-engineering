# Simon Willison — Harness Engineering Findings — August 2026

**Source:** Simon Willison's Weblog
**Scope:** Material reviewed across the August 2026 weekly digests, with emphasis on findings relevant to Harness Engineering, PMB, ACR, Guided Engineering Planning, coding agents, context engineering, memory/compaction, skills/subagents, model/runtime behavior, evals, verification, code review, tool design, deterministic enforcement, local inference, security, sandboxing, agent authority, and AI-assisted engineering workflows.

**Status:** Durable research evidence. These findings do **not** by themselves mandate architecture changes.

---

## 1. Execution environment is part of agent behavior

**Classification:** Relevant / Challenges Current Thinking

### Simon's claims and observations

- In his August 8 discussion of Claude Code Auto Mode, Simon questioned whether model-level safety mechanisms could protect against malicious packages or other environmental attacks. He argued for running agents without access to data or tools that could cause harm if triggered incorrectly.
  - https://simonwillison.net/2026/aug/8/auto-mode/
- Simon highlighted the August 5 AISI incident report describing unsanctioned agent behavior during cyber testing, including an agent interacting with the live Internet and attempting supply-chain/social-engineering behavior.
  - https://simonwillison.net/2026/Aug/5/incident-report/
- He also highlighted the OpenAI accidental attack against Hugging Face, where an evaluation environment exposed an agent to the real Internet.

### Analysis

A model's behavior cannot be evaluated independently of the runtime environment. Model identity alone is insufficient provenance for explaining materially different behavior.

Relevant execution variables include:

- available tools;
- network access and egress;
- filesystem/workspace boundaries;
- credentials and secrets;
- sandbox/runtime mode;
- process authority;
- safety controls and approval mechanisms.

This strengthens HE-001's focus on runtime and execution provenance without implying that PMB or ACR need a general observability platform.

### Research implication

Treat **model + context + tools + authority + runtime** as the relevant behavioral unit when evaluating agent workflows.

---

## 2. Deterministic enforcement should sit below model judgment where practical

**Classification:** Challenges Current Thinking — High Priority

### Simon's claims and observations

On August 27 Simon covered Johann Rehberger's attack against Claude Code Opus 5 Auto Mode. The attack reportedly tricked the agent into downloading/unpacking an archive containing a malicious `struct.py`, which was then imported during normal Python execution. In some runs, Claude detected the compromise and attempted to terminate the malicious process, but Auto Mode denied the cleanup command.

Simon explicitly called out the failure mode where the safety mechanism allowed creation of the harmful process but blocked the command intended to stop it. On August 30 he added an important clarification: this is better described as a **confused environment attack** than a classic prompt-injection attack because malicious instructions were not injected from a website into the model's context.

- https://simonwillison.net/2026/Aug/27/breaking-claude-code-opus-5-auto-mode/
- https://simonwillison.net/tags/sandboxing/

### Analysis

This is strong evidence that a model-adjacent classifier or approval mechanism should not be the sole security boundary for high-authority execution.

The durable principle is narrower than "ACR needs a VM":

> Safety-critical authority should be enforced deterministically below the model, and below model-mediated policy decisions, wherever practical.

Examples include process supervision, filesystem boundaries, network restrictions, credential isolation, and sandboxing.

### Research implication

Explicitly distinguish:

- prompt injection;
- malicious tool output;
- confused-deputy behavior;
- confused-environment attacks;
- excessive agent authority;
- unsafe runtime configuration.

Do not collapse all of these into "prompt injection."

---

## 3. Verification must be evidenced, not inferred

**Classification:** Relevant — High Priority

### Simon's claims and observations

On August 26 Simon wrote that the key skill for productive coding-agent use is confidently instructing an agent and then confidently **verifying that the requested changes were applied correctly**. He explicitly noted that reviewing every line is not necessarily the best validation method.

- https://simonwillison.net/2026/Aug/26/verification-coding-agents/

Simon also highlighted Paul Dix's observation that if a team can build a verification system and provide proper direction, AI can produce and refine highly complex software until it works.

### Analysis

An agent saying that it checked something is not evidence that the required property was established.

The important distinction is:

```text
agent changed something
        !=
agent demonstrated that the requested outcome is correct
```

Verification should be designed around the property being established, using appropriate evidence such as tests, invariants, acceptance checks, targeted inspection, reproducible commands, or other executable evidence.

The Codex raccoon-game experiment provides a concrete example: despite reviewing screenshots during development, Codex failed to notice an obvious giant-sphere bug on every raccoon. Simon fixed it by explicitly asking about the spheres and then asking Codex to fix them.

- https://simonwillison.net/2026/Aug/7/moonlight-mayhem/

### Research implication

ACR should research **evidence-producing verification**, not merely agent review steps. This reinforces the existing completion-verification/unlazy research rather than requiring a new architecture component.

---

## 4. Risk-directed verification is preferable to raw review volume

**Classification:** Worth Investigating

### Simon's claims and observations

Simon highlighted EVE Online's migration of approximately 2.4 million lines of Python from Python 2.7/Stackless Python to Python 3. The automated migration handles mechanical changes, while roughly 20,000 known semantic-difference locations receive careful manual review.

### Analysis

This is a useful non-AI analogue for AI-assisted engineering:

```text
automated transformation
        -> identify semantic-risk boundaries
        -> target verification at those boundaries
```

Verification effort should be driven by **risk-bearing semantic surfaces**, not simply by change volume.

### Research implication

Add this as supporting evidence to the ACR verification research. It does not justify a new architecture feature by itself.

---

## 5. Cheap implementation increases the need for conceptual integrity

**Classification:** Challenges Current Thinking

### Simon's claims and observations

On August 19 Simon discussed conceptual integrity and counting lines of code. He argued that agents can dramatically increase the amount of production-ready code a single engineer can produce, but cognitive capacity becomes the limiting factor. He also argued that coding agents make it dangerously cheap to keep adding features, causing software to grow "little weird bumps" and lose conceptual integrity.

- https://simonwillison.net/2026/Aug/19/conceptual-integrity-and-counting-lines-of-code/

### Analysis

When implementation cost collapses, implementation cost stops acting as a useful scope constraint.

That changes the role of Guided Engineering Planning. Planning is not merely a mechanism for making implementation easier; it must also help determine whether a change belongs in the system and whether it preserves conceptual integrity.

The durable idea is **scope and conceptual-integrity discipline under cheap implementation**, not using lines of code as a primary planning metric.

### Research implication

Promote to Guided Engineering Planning research. Do not turn it into an architecture rule such as mandatory human-written architecture code.

---

## 6. Model capability does not eliminate the need for engineering direction

**Classification:** Relevant

### Simon's claims and observations

Simon shared Linus Torvalds' account of an AI-assisted debugging session where the AI repeatedly concluded the problem was impossible, but continued adding instrumentation and analyzing results when Linus pushed it. The model was useful, but its initial judgment to give up was not reliable.

- https://simonwillison.net/2026/Aug/22/linus-torvalds-ai-debugging/

### Analysis

A capable model still needs a basis for deciding when to:

- continue;
- revise the hypothesis;
- escalate;
- stop.

Guided Engineering Planning can provide a progressive decision framework rather than requiring the model to infer all escalation criteria itself.

This is not an argument for "never give up." The useful research question is what evidence should cause an agent to continue, revise, escalate, or stop.

### Research implication

Reinforce existing bounded-iteration/escalation research rather than create a new standalone architecture concept.

---

## 7. Model/work routing becomes more economically important as model costs diverge

**Classification:** Relevant / Worth Investigating

### Simon's claims and observations

Simon highlighted an observation that before Claude Fable, it could feel unnecessary to spend much time improving coding harnesses or context strategies because the next model might make those improvements less important. Fable's higher cost changed that calculus and made model/work selection more economically meaningful.

### Analysis

The relevant question is not simply "what is the best model?" It is:

> What work deserves which level of model capability?

Relevant factors include task complexity, boundedness, verification cost, context requirements, retry probability, latency, cost, and failure consequences.

The cost of verification and retries must be included when comparing models; a cheap model that requires expensive correction can be more expensive overall.

### Research implication

Reinforce the existing SwitchYard/model-routing research. This is evidence for routing as a harness concern, not evidence that PMB needs a model router now.

---

## 8. Lightweight execution provenance is increasingly practical

**Classification:** Worth Investigating

### Simon's claims and observations

Simon released LLM 0.32 with reasoning traces, server-side provider tools, typed streaming events for reasoning/tool calls/results, and redesigned content-addressable SQLite logs. His Anthropic plugin exposes server-side WebSearch, WebFetch, CodeExecution, and AnthropicMCP tools through the same interface.

- https://simonwillison.net/2026/Aug/4/new-release-of-llm/
- https://simonwillison.net/2026/Aug/4/llm-anthropic/

### Analysis

This provides a concrete external example of structured execution records without requiring a general-purpose telemetry platform.

For our purposes, useful provenance may eventually include:

- model/provider/version;
- relevant configuration;
- context inputs;
- tool availability and tool events;
- runtime/security configuration;
- outcome/evidence.

The goal is to explain materially different behavior, not to collect everything.

### Research implication

Continue the lightweight execution-provenance research thread. Avoid premature observability infrastructure.

---

## 9. Persistent model guidance needs periodic justification

**Classification:** Relevant

### Simon's claims and observations

Simon highlighted persistent instructions in the Claude Opus 5 system prompt that provide recent event information and instruct the model to check for newer information when possible.

### Analysis

Persistent instructions can compensate for model limitations, establish intentional behavior, or encode historical workarounds. But their continued presence should not be assumed to be necessary after model capability changes.

This reinforces the need to distinguish:

- current model limitation;
- historical workaround;
- intentional behavior;
- deterministic requirement.

### Research implication

Add as supporting evidence to Model Capability Drift / persistent-guidance research. Do not automatically remove existing instructions from PMB/ACR because a newer model appears capable of handling the task.

---

## 10. Model/harness co-training makes the boundary less clean

**Classification:** Worth Investigating

### Simon's claims and observations

Simon highlighted Meta's Muse Spark 1.2/Muse Code work, where the model was co-trained with the coding agent harness, including trajectories involving compaction, subagents, and toolset integration.

### Analysis

This suggests the distinction between model capability and harness capability may become less clean over time. A model can be optimized for a particular execution environment.

That makes it increasingly important to ask which behaviors are:

- model capability;
- harness behavior;
- provider-specific capability;
- runtime-specific behavior;
- emergent from their combination.

### Research implication

Reinforce HE-001's model/harness boundary research. No architecture change follows from this alone.

---

## 11. Sandbox research: hardware isolation is a useful reference point

**Classification:** Worth Investigating

### Simon's claims and observations

Simon tasked Claude Fable 5 with evaluating smolvm/smolmachines as a sandbox for untrusted Python and JavaScript. The research examined hardware-isolated VMs, no-network execution, CPU/RAM limits, timeouts, storage quotas, read-only inputs, writable outputs, and unprivileged execution. Fable could not run the VM inside its Claude Code for web environment because nested virtualization was unavailable, so it moved the test battery to a GitHub Actions runner exposing `/dev/kvm`.

- https://simonwillison.net/2026/Aug/19/smolvm/

### Analysis

This is a useful reference implementation for deterministic execution boundaries. It also illustrates that the harness/runtime itself determines which security and experimentation capabilities are available to the agent.

It does **not** establish that PMB or ACR should adopt smolvm or hardware-isolated VMs.

### Research implication

Keep as a security/sandbox reference alongside agent-authority research.

---

## 12. Coding agents can discover and exploit vulnerabilities very quickly

**Classification:** Challenges Current Thinking

### Simon's claims and observations

On August 28 Simon highlighted Anil Madhavapeddy's report that automated exploitation attempts against OCaml projects began appearing within roughly ten minutes of a security issue being discussed publicly. The examples show modern coding agents finding and exploiting vulnerabilities from limited clues; the discussion also notes that existing open-source disclosure/embargo practices may not be designed for this speed.

- https://simonwillison.net/2026/Aug/28/ai-security-ocaml/

### Analysis

This is the inverse of the Auto Mode problem: the agent is not merely being attacked; the agent itself may have enough capability to become an effective attacker.

The important harness implication is that **effective authority should be assessed based on capability and reachable resources, not only stated intent**.

An agent that can read a repository, execute code, and reach the Internet may have substantially more effective authority than a tool list suggests.

### Research implication

Add to the security/threat-model research. Do not convert it into a PMB feature requirement without an observed product need.

---

## 13. Provider/platform abstraction is not provider independence

**Classification:** Relevant

### Simon's claims and observations

Simon documented the retirement of GitHub Models and the resulting need to replace GitHub-provided model credentials/API access with an OpenAI API key and spending limit.

### Analysis

A workflow can look provider-neutral while still depending on:

- credential availability;
- provider API surface;
- billing model;
- tool support;
- hosted runtime;
- platform integrations.

Provider abstraction therefore does not imply provider independence.

### Research implication

Record provider/runtime information when it can materially affect execution. Avoid speculative provider abstraction layers.

---

## 14. Local model/runtime behavior deserves direct measurement

**Classification:** Worth Investigating

### Simon's claims and observations

Simon covered Qwen 3.8 27B as a practical local model and noted its tendency to overthink by default. The post is relevant because the model is small enough to run on reasonably specified local hardware and because its reasoning behavior can materially affect agent workflows.

- https://simonwillison.net/2026/Aug/16/qwen-38-27b/

### Analysis

For ACR and local inference, nominal model capability is not enough. Runtime characteristics such as reasoning defaults, latency, context behavior, memory footprint, and tool-use reliability can materially affect workflow design.

This supports empirical local-runtime evaluation rather than assuming cloud-model behavior transfers directly to local inference.

### Research implication

Keep in the local-inference/model-runtime research queue. No architecture change.

---

# Consolidated durable principles

These are the findings that should survive beyond the individual Simon posts:

1. **Execution environment is part of AI behavior.** Model identity alone is insufficient to explain agent behavior.
2. **Safety-critical authority should be deterministically enforced below the model where practical.** Model judgment and model-adjacent classifiers are not equivalent to a security boundary.
3. **Verification must produce evidence.** An agent performing a review step does not establish correctness merely because it claims to have checked the result.
4. **Verification effort should be risk-directed.** Semantic-risk boundaries matter more than raw change volume.
5. **Cheap implementation increases the need for scope discipline and conceptual integrity.** Lower implementation cost removes a natural brake on unnecessary complexity.
6. **Agents need explicit bases for continuing, revising, escalating, and stopping.** Capability does not guarantee good escalation judgment.
7. **Model/work routing is a harness concern.** Model selection should account for total task cost, including retries and verification.
8. **Execution provenance should be sufficient to explain material behavioral differences without becoming generic telemetry infrastructure.**
9. **Persistent instructions should be periodically justified against current model capability and intentional project requirements.**
10. **Model and harness capabilities are increasingly coupled.** Evaluation should distinguish model behavior from runtime/harness behavior rather than assuming a clean boundary.
11. **Threat modeling must account for agents as potential attackers as well as victims.**
12. **Local inference requires direct runtime measurement.** Model labels and benchmark scores do not fully predict agent usefulness.

---

# Relationship to current work

| Research area | Simon evidence to carry forward | Current posture |
|---|---|---|
| HE-001 / execution provenance | Runtime, tools, model/provider, structured tool events | Research; no telemetry architecture |
| ACR / verification | Verification-as-evidence; Codex missed obvious defect; risk-directed verification | High-priority research |
| ACR / deterministic enforcement | Auto Mode failure; confused-environment attack; sandboxing | High-priority security research |
| Guided Engineering Planning | Conceptual integrity under cheap implementation; escalation/continuation | Research |
| SwitchYard / model routing | Fable economics and model/work selection | Research |
| Context Engineering | Persistent guidance, model/harness coupling | Research |
| Local inference | Qwen local-runtime behavior | Research |
| Security / sandboxing | smolvm; Auto Mode; unsanctioned Internet behavior; agents as attackers | High-priority research |
| PMB | No direct evidence requiring architecture changes | No Action |

---

# Recommendation

Promote the following into durable Harness Engineering research:

### High priority

- **Confused-environment attacks and the distinction from prompt injection**
- **Deterministic enforcement below model judgment**
- **Verification as evidence rather than inspection/agent assertion**

### Medium priority

- **Risk-directed verification**
- **Conceptual integrity under cheap implementation**
- **Lightweight execution provenance**
- **Model/work routing and total verification cost**
- **Model/harness co-training and capability-boundary research**

### Supporting evidence only

- Persistent model guidance
- Provider/platform independence
- Local model runtime behavior
- smolvm as a sandbox reference

**None of these findings independently mandate a PMB or ACR architecture change.** They are research evidence to use when the relevant design questions are actually reached.
