# Personal Agents — Nod, Hermes, Claude Duet & 9Router — 2026-11-17

**Status:** Source-specific research / evidence preservation only. **No adoption, installation, integration, code change, new policy, or product requirement.** This note belongs in HE's research collection; it is not authorization to expand the separate AI Engineering Cockpit, PMB or ACR.

**Question:** What transferable harness-engineering evidence is present in current personal-agent runtimes and model-routing demonstrations, beyond their marketing comparison?

## Sources and evidence classification

| Evidence | What it supports | What it does not establish |
| --- | --- | --- |
| [Chase AI: “n8n Just Released a FREE + Open Source Version…” (Nov 2026)](https://www.youtube.com/watch?v=ynEEbqKQSaY), user-provided screenshots and transcript | Creator walkthrough of Nod onboarding, model picker, scheduled tasks, integrations, channels, usage detail and Vault; portrays Nod as an alternative to managed cloud assistants. | Independent security testing, reliable production task completion, absolute local privacy, or parity with Dots/Duet. |
| [Nod repository](https://github.com/n8n-io/nod), [self-hosting documentation](https://nod.n8n.io/docs/deploy/self-host/), [security review guide](https://nod.n8n.io/docs/trust/security-review/), [Vault](https://nod.n8n.io/docs/trust/vault/) | Inspectable self-host stack, server-owned secrets/policy, isolated browser/workspace, action trust/approval concepts and remote clients. | That every harmful effect can be prevented, that all app data remains local, or that a laptop below published requirements will operate acceptably. |
| [Eric Michaud: “Hermes Agent is SO MUCH Better Than You Think!” (Nov 2026)](https://www.youtube.com/watch?v=IcCzzwWgaws), user-provided screenshots and transcript | Creator's workflow: research → Obsidian note → Telegram notification; demonstrations of gateway, skills, scheduled jobs, subscription/plugin picker and model routing. Creator states a Hostinger sponsorship. | Representative completion/latency, security superiority, verified self-improvement, independent cost comparison, or proof that a subscribed model can be used without provider restrictions. |
| [Hermes repository](https://github.com/NousResearch/hermes-agent), [Docker](https://hermes-agent.nousresearch.com/docs/user-guide/docker), [security](https://hermes-agent.nousresearch.com/docs/user-guide/security), [memory](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory), [skills](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills) | Actual agent/gateway architecture, persisted configuration and state, tool and remote-message surfaces, selectable execution backends, approval controls and agent-managed memory/skills. | That GitHub activity or funding proves correctness; that an approval prompt protects against all host effects. |
| [Hermes Claude Subscription DirectSDK (Experimental)](https://hermes-agent.nousresearch.com/marketplace/claude-subscription-directsdk) | An experimental CLI/Agent-SDK pathway for Claude Pro/Max subscription turns, distinct from ordinary API billing. | Contractual permission or uniform behavior for all providers/plans; production-readiness. |
| [9Router repository](https://github.com/decolua/9router), [combos documentation](https://github.com/decolua/9router/blob/master/gitbook/content/en/features/combos.md) | Four advertised routing strategies—fallback, round robin, model-assisted smart selection, and parallel Fusion with judge—and configurable provider credentials. | Safe token handling, guaranteed savings or free capacity, behavioral equivalence, cross-provider tool compatibility, or verified benchmark improvements. |
| [Anthropic: Introducing Claude Duet (Oct 21, 2026)](https://claude.com/resources/articles/claude-duet), [safety description](https://claude.com/resources/articles/claude-duet-safety-privacy-and-security), [getting started](https://support.claude.com/en/articles/17631495-get-started-with-claude-duet) | Managed, proactive research-preview agent, provider-managed computer and security controls, shared Claude-plan limits and approval design. | Creator's secondhand reports of approval fatigue, speed and reliability; those require direct measurements. |
| Video screenshots of model benchmarks, PR volume, funding and promotional hosting | Claims and presentation context worth checking if later relevant. | Technical quality, accuracy, availability, time-bound price or value-for-money. Screenshots are not benchmark methods or audit logs. |

**Review date:** 2026-11-17. The linked repositories, plugin contracts and service eligibility evolve rapidly. Recheck and pin commits when performing a reproduction; this is not a frozen implementation audit.

## Product boundaries: they are not interchangeable

| Product | Main trust / execution owner | Consequence |
| --- | --- | --- |
| **Nod** | Self-hosted Nod server; model requests, action trust checks, browser/workspace containers and Vault have documented boundaries. | Strong candidate to inspect **server-enforced action authorization** and uncertain-effect handling. Self-hosted does not mean all task contents avoid external model, Composio, or app providers. |
| **Hermes** | Self-hosted agent/gateway; operator chooses backend and model providers. Default/local execution may reach host-user resources unless separate OS/container restrictions apply. | Strong candidate to inspect **self-modifying skill/memory behavior, multi-channel state, OS isolation and gateway limits**. Skills and memory approvals do not become universal filesystem ACLs. |
| **Claude Duet** | Vendor-operated runtime and authorization/privacy machinery; user controls interaction and approvals, not implementation. | Useful baseline for actual task utility and friction, **not** proof that self-hosted is inherently better or worse. |
| **9Router** | Model/provider gateway, not a personal agent. | Relevant to execution provenance, quota-scope and routing semantics; do not turn an inference gateway into HE's or the Cockpit's responsibility. |

The video collapses “proactive managed service,” “self-hosted runtime,” “model gateway” and “automation platform” into near equivalents. They overlap in user tasks but differ in who owns security, state, execution, costs and recovery.

## Findings to preserve

### F1 — Approval is different from containment (REINFORCE)

Nod documents a server-side trust check over proposed actions, private credential entry, origin checks, and durable claim/uncertain-state behavior intended to prevent duplicate external effects. The threat-model documentation points to specific code/tests; its effectiveness under adversarial use is **not independently reproduced here**.

Hermes separately documents command approvals and execution backends. Its official security policy says in-process heuristics are not hard security boundaries and profiles under one OS user are not tenants. An agent with write access through another permitted tool can reach effects beyond a specific skill-management approval gate.

Claude Duet documents a vendor-managed classifier and approval architecture; the creator's “asks permission too often” reports are not measurements from his own use.

**Transfer:** map (a) requested effect, (b) actor and real OS/app authority, (c) non-writable enforcing component, (d) approval scope, (e) execution receipt, and (f) uncertain outcome/retry rule. Prefer a deterministic refusal path for destructive actions. Do **not** conclude that one runtime is safer based only on the presence/absence of approval popups.

**Already owned by HE:** [Budgeted Side Effects, Approval Gates & Receipts (Sep 25)](../..//01%20Research/Budgeted%20Side%20Effects%2C%20Approval%20Gates%20%26%20Receipts%20%E2%80%94%202026-09-25.md) and [Bounded Execution Envelopes (Sep 22)](../..//01%20Research/Bounded%20Execution%20Envelopes%20%E2%80%94%202026-09-22.md). No new global policy required.

### F2 — Self-improving skills are mutable software inputs, not verified learning (REINFORCE / TEST)

Hermes supports agent-authored procedural skills and compact persistent memory. Its documentation exposes **skills.write_approval** and **memory.write_approval**; the gates can stage a proposed write, but tool-specific gates do not by themselves constrain generic shell/file operations. A skill scanner is a heuristic, not comprehensive authority enforcement. A frozen memory snapshot at session start also means a write may not affect the current session's system prompt.

The showcased sequence “job finished → review → patched its own skill” illustrates a mechanism, **not evidence** that later runs are better or that the patch was correct. False lessons can become durable misrouting/scar tissue.

**Transfer:** preserve the exact source failure, evidence and owner; stage the smallest candidate correction; test original failure and regression cases in a fresh run; promote only with human or deterministic authorization. Single-run corrections and recurring patterns deserve different thresholds. Include “no durable correction” as a valid answer.

**Already owned by HE:** [Experience-Derived Harness Evolution (Sep 25)](../..//01%20Research/Experience-Derived%20Harness%20Evolution%20%E2%80%94%202026-09-25.md). PMB retains its own provenance and memory authority; no Hermes-derived memory subsystem is authorized.

### F3 — Model combos represent different causal experiments (REINFORCE / ASSESS)

9Router documentation distinguishes:

1. **Fallback:** retry a different route/model after errors or quota events.
2. **Round robin:** distribute requests between routes, often changing behavior across calls.
3. **Smart selection:** an auxiliary model chooses a route under specified policies.
4. **Fusion:** ask several models in parallel and use a judge to synthesize.

These are **not** equivalent to “Claude reasoning + Gemini vision + GPT coding.” That would require a separately defined decomposition, role/context contract, and result ownership. A fusion judge can discard disagreements; a fallback can change capabilities, tool schemas, context behavior and cost during a task. Examples in router docs are illustrative—not verified actual spend or free-tier guarantees.

**Transfer:** preserve selected execution identity, fallback reason, capability mismatch, context/handoff state, and model-specific outputs when comparing decisions. Prefer same-model retries or explicit compatible alternatives before silent heterogeneous substitution. For architecture/review research, independent parallel opinions plus an evidence-backed human synthesis are easier to audit than opaque fusion.

**Already owned by HE:** [FreeLLMAPI — Gateway Boundaries & Runtime Routing (Sep 14)](../..//01%20Research/FreeLLMAPI%20%E2%80%94%20Gateway%20Boundaries%20%26%20Runtime%20Routing%20%E2%80%94%202026-09-14.md), [Model-Tiered Workflows & Independent Factory Assurance (Sep 24)](../..//01%20Research/Model-Tiered%20Workflows%20%26%20Independent%20Factory%20Assurance%20%E2%80%94%202026-09-24.md), and [Harness Portability, Exit Cost & Inspectability (Sep 24)](../..//01%20Research/Harness%20Portability%2C%20Exit%20Cost%20%26%20Inspectability%20%E2%80%94%202026-09-24.md).

### F4 — Always-on is a deployment property, not proof of a useful assistant (ASSESS)

Nod's documented self-host path needs a running machine and a stack that includes browser/workspace/database components. Hermes has a containerized gateway with persistent mounted state and optional browser tools. Both can run on an always-on host, with inference delegated to a hosted provider. **Containerized, self-hosted and “local LLM” are different claims.**

Operational tests must measure actual idle/peak RAM, browser cold-start and concurrency, CPU saturation, scheduler recovery, network exposure, restart/backup/restore and expiry of connected credentials. Resource requirements are not performance benchmarks. On old or memory-constrained hardware, start with hosted inference and a minimal profile; do not assume another VM helps.

Hosted VPS pricing in the video is promotional, time-bound and explicitly sponsored. A self-hosted machine also incurs maintenance and model usage costs, even without an incremental hosting bill.

**Transfer:** specify the actual *repeated job* and acceptable delay, reliability and maintenance cost before choosing on-premises vs VPS vs managed product. Do not infer a platform obligation from hardware that is already powered on.

### F5 — External credentials and subscription reuse need precise statements (CAUTION)

- Nod's Vault can protect passwords from direct model exposure, but the server/authorized browser must use credentials; connected-app services and external model providers remain part of the data flow.
- Hermes provider paths differ: native API keys, Codex/ChatGPT login, and an **experimental Claude Subscription DirectSDK plugin** are not interchangeable contracts.
- 9Router is an additional credential/routing intermediary. Quota estimates and routing transparency are not substitutes for checking the account's real plan, provider terms, data handling and billable usage.
- Neither open-source status nor a claim that “passwords never reach the AI” is sufficient security verification.

**Transfer:** when evaluating access, record exactly which account grants authority, the intermediary storing a token, effective privileges, model billing path, revocation/rotation, and what information crosses trust boundaries. Avoid linking workplace credentials or telemetry to an unsanctioned personal runtime.

### F6 — Gateway continuity is not the same as portable state (REINFORCE)

Hermes documents a shared gateway across channels and on-disk memory/skills. That is a concrete continuity mechanism, but does not demonstrate perfect retention of task context or portability across alternative harnesses. The video also shows a claimed automated migration from other agents; source-backed compatibility and skipped credentials require case-by-case testing.

**Transfer:** verify export/import of durable artifacts, exact routing and approval state, source/skill version, pending tasks and rollback. A model switch or migration must not silently carry provider-internal hidden state or falsely claim behavioral equivalence.

## Claims to avoid promoting

| Claim from the videos/screenshots | Research disposition |
| --- | --- |
| “Hermes gets smarter every time” | **Unproven outcome.** Agent-written skills/memory are implemented; durable accuracy improvement needs held-out replay and rollback evidence. |
| “A laptop can do exactly what a managed agent does” | **Overbroad.** Continuous power alone does not replace hosted availability, browser/runtime capacity, integrations, reliability, security or operations. |
| “Every subscription can be used with any model, no added cost” | **Unproven as a universal claim.** Authentication pathways, limits, terms and billing differ, and a Claude plan plugin is marked experimental. |
| “Nod is as safe as Dots/Duet because Vault and VM” | **Unsupported equivalence.** Distinct threat models and untested controls. |
| “9Router fusion/model combos guarantee better output or 99% savings” | **Unsupported.** Illustrative marketing/examples, no comparable workload, cost ledger or correctness eval. |
| GitHub stars, huge one-week PR counts, financing, single creator demo | **Activity/promotional evidence only**, not test coverage, code quality or operational outcomes. |
| Creator reports Duet slow and approval-heavy | **Anecdotal/secondhand** in this video; official product documentation does not establish measured impact. |

## Smallest evaluation worth doing later (optional, not scheduled)

**Baseline:** first try the same tasks in a native product already accessible to the operator. No hosted agent or router is justified if native scheduling/notifications answer the question.

| Scenario | Measurement | Pass/fail boundary |
| --- | --- | --- |
| Public-page monitor, no account | Correct change detection, duplicate alerts, provenance, retries and machine restart | No invented change and no silent skipped run |
| Research → saved note → notification | Source verifiability, actual artifact and destination, timing, user correction | The file exists and notification corresponds to verified final result |
| Disposable-account side effect | Exact action/target approval, denied action, replay after interruption | Unapproved send/write never executes; uncertain outcome never silently repeats |
| Deliberately flawed skill improvement | Candidate diff, owning component, replay and negative-control regression | Incorrect one-off lesson is rejected; correct change has evidence |
| Same task with explicit model substitution | Completion, tool compatibility, context handoff, route ID, latency and real cost | Changes are visible and not misrepresented as equivalent |
| Memory/export restore | Fresh-process recovery, data provenance, key/secret omission | Recovered state is correct and secrets are not leaked |

**Stop conditions:** no observed recurring need; native tool works; too many approvals or silent failures; insufficient memory/CPU; privileged or unsupported credential flows; inability to revoke/restore; plugin/router dependence outweighs utility. Do not enable self-improvement, external connectors, or broad filesystem access during the baseline.

## Ownership / disposition

| Owner | Mine | Explicitly avoid |
| --- | --- | --- |
| **HE research** | Map real authority boundary, tool-specific approval vs OS enforcement, failure→candidate correction→replay, route provenance and portability. | A new HE safety framework, compulsory agent platform, or duplicative policy. |
| **PMB** | None as implementation. Compare mechanisms only if a reproduced PMB failure warrants it. | Importing Hermes memory, agent-written canon, or another governance ladder. |
| **ACR** | Reuse existing result provenance and deterministic eval approach; controlled model substitution as a research question. | Adding a general autonomous gateway/memory system to code review. |
| **Separate AI Engineering Cockpit** | Observe provider-reported usage/failure/freshness **if** supported native sources answer an actual question. This note is only a design reference. | Scheduling agents, connecting credentials, prompt relaying, model routing, automatic actions or Work/Home data transfer as Dashboard features. |
| **Personal experimentation (outside HE)** | Compare Duet native with one self-hosted agent on a bounded public-data task. | Simultaneously installing Nod, Hermes and 9Router; treating a pilot as production. |
| **Work environment** | Abstract engineering patterns only, subject to applicable authorization. | Any personal-agent install or company-data export without approval. |

**Decision:** Preserve this source-level evidence and add no new system. The most informative next measurements are (1) rejected versus approved memory/skill mutations, and (2) observable behavioral changes during fallback/fusion. Neither is evidence of an unmet operational need today.
