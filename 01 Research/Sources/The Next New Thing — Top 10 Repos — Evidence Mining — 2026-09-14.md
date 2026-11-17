# The Next New Thing — Top 10 Repos — Evidence Mining

**Research date:** 2026-09-14

**Source:** *Top 10 Repos explained: ADHD, Ponytail, and more*

**Video:** https://www.youtube.com/watch?v=aX8Y183qDpY

**Disposition:** REINFORCE + ASSESS + PARK — no architecture or implementation authorized

**Research threads:** Context Engineering / Capability Packaging / Handoff / Evaluation / Harness Boundaries

> **Research posture.** The video and supplied transcript were discovery inputs, not technical
> evidence. Each repository was inspected directly at the pinned revision in the verification
> ledger. This document separates repository evidence from internal Harness Engineering synthesis,
> does not count derivative projects as independent corroboration, and does not promote any finding
> directly into architecture.

## 1. Executive synthesis

This batch is useful because the repositories that the video presents as one category are not one
kind of thing. They span at least seven different harness surfaces:

- output and interaction policy;
- procedural skills;
- full harness operating models;
- capability distribution packages;
- context interception and retrieval infrastructure;
- deterministic artifact pipelines; and
- protocol bridges and operational dashboards.

Treating all of these as “skills” would erase the ownership and execution boundaries Harness
Engineering exists to study.

The strongest findings are:

1. **A handoff can remain small.** Matt Pocock's handoff and context-pointer patterns reinforce the
   existing Harness research: transfer unresolved state and point to durable authoritative
   artifacts instead of copying the project into another summary.
2. **Context management is an active harness layer.** Context Mode intercepts tools, stores and
   retrieves externalized content, and restores selected material across compaction. It is not a
   writing preference or neutral optimization. Any evaluation that adds it has changed the harness
   being evaluated.
3. **Packaging and capability are separate questions.** OpenAI's current plugin structure can bundle
   skills, apps, MCP configuration, agents, commands, hooks, and assets. The old standalone skills
   catalog was deprecated; skills themselves were not “killed.”
4. **Behavioral instructions require empirical evaluation.** I Have ADHD and Ponytail both show
   that concise or simplicity-oriented policies can help in aggregate while still creating
   regressions, invalid cases, or over-literal behavior.
5. **Independent evaluation needs execution truth.** AI Marketing Panel's most transferable idea is
   not simulated customers. It is isolated evaluators, frozen independent findings, recorded
   execution capability, preserved dissent, deterministic orchestration where possible, and later
   calibration against observed outcomes.

None of these findings authorizes a Harness, PMB, or ACR implementation change. The current value is
assessment evidence and sharper vocabulary.

## 2. Source set and method

### Discovery source

The supplied transcript covered these video chapters:

| Time | Topic |
|---|---|
| 00:00 | I Have ADHD |
| 01:40 | ECC |
| 03:02 | Ponytail |
| 05:20 | Zapier MCP |
| 05:55 | HumanLayer Skills |
| 08:53 | OpenAI Plugins |
| 10:17 | Archify |
| 11:36 | Context Mode |
| 13:23 | Matt Pocock Skills |
| 16:07 | Humanizer |
| 17:59 | OpenAI Skills |
| 19:28 | Reckoner |
| 20:05 | Tim Harris Skills |
| 20:36 | Clodex |
| 21:34 | AI Marketing Panel |

### Repository inspection

All fifteen repositories were cloned at current default-branch HEAD and inspected locally. The pass
included relevant READMEs, `SKILL.md` files, manifests, hooks, implementation files, tests,
benchmarks, and operations documents. Exact revisions and primary paths are pinned in §14.

This was a source inspection, not product validation:

- no reviewed package, skill, plugin, hook, proxy, or MCP server was installed;
- no external benchmark was reproduced;
- no provider credential was supplied;
- repository-reported results remain repository claims; and
- source lineage is recorded so derivative material is not counted twice.

## 3. Repository-level disposition

| Repository | Harness surface actually present | Classification | Current disposition |
|---|---|---|---|
| I Have ADHD | Output policy, optional always-on hooks, evaluation harness | RESEARCH REFINEMENT | Mine bounded interaction rules; do not infer universal benefit |
| ECC | Agents, skills, commands, hooks, memory, learning, orchestration, security | PARK | Competing harness operating model; inspect named mechanisms only |
| Ponytail | Simplicity policy, hooks/plugins, tests, self-reported benchmark | REINFORCE + ASSESS | Preserve the decision ladder; evaluate behavioral side effects |
| Zapier MCP | Client plugin/configuration for a hosted MCP service | REINFORCE | Capability distribution is not the capability implementation |
| HumanLayer Skills | Small workflow plugins and instruction-maintenance procedures | ASSESS | Mine instruction-pruning rules; distinguish cues from retrieval |
| OpenAI Plugins | Multi-surface Codex capability packages | ASSESS | Strong input to HE-001 capability-supply-path analysis |
| Archify | Typed IR, deterministic validation/rendering, delivery boundary | CORROBORATION | Existing Harness conclusion stands; do not double-count |
| Context Mode | Interception, sandboxing, storage, retrieval, lifecycle restore | ASSESS | Treat as an active harness subsystem; isolate any future test |
| Matt Pocock Skills | Small reusable workflow capabilities | CORROBORATION | Existing handoff/context-pointer research is strengthened |
| Humanizer | Prose transformation policy | PARK | Deliberate document-editing capability, not global context policy |
| OpenAI Skills | Deprecated catalog/distribution repository | HISTORICAL | Use only to explain the transition to plugins |
| Reckoner | Multi-provider billing dashboard and credential store | PARK / SECURITY REVIEW | Separate operational tool with concentrated credential risk |
| Tim Harris Skills | Non-engineer workflow pack derived substantially from prior work | LINEAGE NOTE | Do not treat adapted skills as independent convergence |
| Clodex | Anthropic-protocol to Codex-backend bridge | PARK / REJECT FOR PRIMARY USE | Useful boundary proof; explicit account and local-proxy risk |
| AI Marketing Panel | Evidence-backed isolated evaluators and calibration workflow | ASSESS FOR ACR | Mine execution/isolation pattern, not persona simulation |

## 4. Behavioral policy is a harness intervention

### 4.1 I Have ADHD

**REPOSITORY EVIDENCE.** The project is more than a short prompt asking for concise answers. It
contains a skill, optional SessionStart/UserPromptSubmit/SubagentStart hooks, and an evaluation
harness. Its policy asks the model to lead with the next action, bound steps, suppress tangents,
keep state visible, and make errors matter-of-fact.

The recorded evaluation does not establish uniform success. The candidate improved the aggregate
score and blocker count, but the repository marks the release gate **FAILED**. One case was
structurally impossible under the runner configuration. The `partial-success` case also regressed;
the recorded behavior is consistent with a policy that pressures the model to state a cause before
the available evidence supports one.

**INTERNAL SYNTHESIS.** A communication policy can change epistemic behavior, not merely formatting.
“State the cause first” may make an answer easier to scan while also increasing pressure to
manufacture certainty. Style-policy evaluation therefore needs correctness and uncertainty handling,
not only brevity and user preference.

**Harness disposition:** ASSESS behavioral policies as context-bearing interventions. Mine individual
rules only in response to an observed communication failure.

### 4.2 Ponytail

**REPOSITORY EVIDENCE.** Ponytail encodes a decision ladder: do not build when unnecessary, reuse
existing capability, prefer platform/standard-library/current dependencies, and make the smallest
sufficient change. Current material adds “understand first” so the model does not optimize a local
diff before understanding the full flow.

The current project-run benchmark reports:

| Metric | Reported change |
|---|---:|
| Lines of code | 54% less |
| Tokens | 22% fewer |
| Cost | 20% lower |
| Time | 27% lower |

These results were not independently reproduced. The correction history is more useful than the
headline: the project replaced an inflated single-shot comparison with a real-agent baseline,
disclosed baseline contamination, and changed the policy after observed over-local optimization.

**INTERNAL SYNTHESIS.** A useful heuristic becomes dangerous when it is converted into an
unconditional law. “Be lazy,” “read everything,” and “make the smallest diff” all require scope and
stopping conditions.

**Harness disposition:** REINFORCE minimum sufficient intervention and ablation. Do not install an
always-on simplicity layer merely because its principle is attractive.

## 5. Handoff should transfer state, not clone knowledge

### Matt Pocock `handoff` and `writing-for-agents`

**REPOSITORY EVIDENCE.** The `handoff` skill writes a temporary transition document, tailors it to
the next consumer, redacts sensitive data, and points to existing specifications, plans, ADRs,
issues, commits, and diffs rather than reproducing them.

`writing-for-agents` names the adjacent mechanism a **context pointer**: a small, always-available
statement that identifies out-of-context material and the condition that should trigger retrieval.
It also distinguishes context load from human cognitive load and treats configuration, directory
structure, and CLI help as authoritative sources that prose should not casually cache.

This is **corroboration**, not a new discovery. `01 Research/Context Engineering.md.md` already
records that:

- handoff can preserve transient state without duplicating durable project artifacts;
- handoff can reference existing artifacts rather than copying them;
- repository configuration can remain separate from reusable capability behavior; and
- model, harness, project context, and conversation are different layers.

It also maps directly to HE-001 Questions 16 and 22: isolate session-specific handoff state while
preserving authoritative project context, and test whether existing artifacts can carry the
handoff without adding new artifact types.

**INTERNAL SYNTHESIS.** The minimal transition packet is likely:

- goal and reason for transfer;
- unresolved working state;
- constraints and definition of done;
- relevant artifact pointers;
- checks already run and checks still required.

This is a hypothesis to assess against actual transitions, not authorization for `mb switch`, a
second handoff command, a new memory system, or a universal packet format.

**Harness disposition:** REINFORCE existing handoff assessment; no new HE-001 question required.

## 6. Context Mode makes context transformation explicit

**REPOSITORY EVIDENCE.** Context Mode is not a concision skill. It registers lifecycle and tool
hooks, redirects high-volume work toward sandbox tools, stores content in SQLite FTS5, retrieves
with BM25, and restores selected context across compaction/session lifecycle events. Its README
currently claims 17 clients and 98% context reduction in the project's own benchmark.

The current platform matrix documents `updatedInput` support for Claude Code. It documents Codex
PreToolUse as deny-only, with input rewriting unsupported. This corrects an earlier interpretation
that assigned the rewrite limitation to Claude Code.

**INTERNAL SYNTHESIS.** Context optimization can own or modify several responsibilities:

- tool-call interception;
- tool-result transformation;
- durable or session-scoped external storage;
- retrieval ranking;
- compaction recovery;
- capability substitution; and
- visibility into what the model actually receives.

Those are active harness responsibilities. A context reducer can change effective capability,
failure modes, provenance, and the independent variable in an evaluation.

**HE-001 mapping:** Questions 1–4, 11–14, 18, and 23 already cover loading, retrieval, enforcement,
runtime behavior, ownership, supply chain, and capability surfaces. Context Mode is useful concrete
prior art for applying those questions; it does not require expanding them.

**Harness disposition:** ASSESS as an external context-management subsystem. Any future comparison
must be isolated, pinned, and evaluated for correctness/rework as well as token reduction.

## 7. Capability packaging is not capability implementation

### 7.1 OpenAI Plugins and OpenAI Skills

**REPOSITORY EVIDENCE.** The standalone `openai/skills` repository is deprecated as the active
catalog/example location and points developers toward `openai/plugins`. The current plugin format
still supports skills, including skill-only plugins. It can also package apps, MCP configuration,
agents, commands, hooks, assets, and supporting files around a required manifest.

The video's “OpenAI Skills are deprecated” summary is directionally true about the old repository
and misleading about the capability type.

**INTERNAL SYNTHESIS.** A plugin is a distribution/ownership envelope. Its contents may expose
several distinct surfaces with different loading, execution, enforcement, and trust behavior.
Counting “one plugin” does not establish that the harness has one capability or one context cost.

This maps directly to HE-001 Question 23 and the existing Capability Supply Paths research:

- implementation;
- interface;
- discovery mechanism;
- execution mechanism;
- enforcement mechanism; and
- distribution mechanism must remain distinguishable.

**Harness disposition:** ASSESS plugin packaging during capability-supply-path analysis. Do not
repackage PMB, ACR, or Harness Engineering from this research alone.

### 7.2 Zapier MCP

**REPOSITORY EVIDENCE.** Zapier's public repository provides plugin manifests, onboarding skills,
agent policy, rules, and client configuration for a hosted MCP service. The server implementation
itself is closed source.

**Harness implication:** the public repository supports claims about discovery, setup, policy, and
the client-side package. It cannot establish the hosted server's internal execution or enforcement
behavior.

**Disposition:** REINFORCE source-boundary discipline and the distinction between distribution and
implementation. This is not currently a Harness capability gap.

## 8. Large frameworks are comparison systems, not drop-in improvements

### ECC

**REPOSITORY EVIDENCE.** ECC currently advertises 68 agents, 292 skills, 94 legacy command shims,
lifecycle hooks, memory, continuous learning, orchestration, and AgentShield scanning. It supports
multiple agent clients through native plugins or synchronized configuration surfaces.

**INTERNAL SYNTHESIS.** ECC is closer to a competing harness operating model than a skill pack. If
layered into PMB/ACR work, it could create overlapping ownership across:

- persistent memory;
- workflow/orchestration;
- rules and hooks;
- capability discovery;
- continuous learning; and
- verification/security policy.

This would make both benefits and failures difficult to attribute.

**Harness disposition:** PARK the framework. Inspect a named mechanism only when HE-001 identifies
a demonstrated gap. Do not use ECC's component count as proof of completeness or maturity.

## 9. Instruction maintenance: pruning, cues, and real loading boundaries

### HumanLayer `improve-claude-md`

**REPOSITORY EVIDENCE.** The skill recommends keeping foundational context small, removing rules
that deterministic tools can enforce, replacing stale code examples with stable file references,
deleting vague instructions, and using `<important if="...">` blocks for conditional relevance. It
also says all inline content remains visible to the model and attention changes with the condition.

**INTERNAL SYNTHESIS.** `<important if>` is an attention/relevance cue, not progressive disclosure.
It may influence adherence, but it does not remove the enclosed content from active context. This
differs materially from Matt Pocock's context pointer, where detailed content remains outside the
loaded layer until retrieved.

The useful synthesis is:

- deterministic tools should own deterministic requirements;
- discoverable facts should not be duplicated into stale prose caches;
- small inline material may use relevance cues; and
- large or branch-specific material should use a real retrieval/loading boundary.

**Harness disposition:** ASSESS instruction pruning and loading semantics separately. Do not treat
XML structure as evidence of context savings.

### Humanizer

**REPOSITORY EVIDENCE.** Humanizer is a substantial prose-transformation policy with checks intended
to remove common AI-writing patterns while preserving claims and substance.

**Harness disposition:** PARK as an intentionally invoked editing capability. It is not evidence for
a persistent global instruction layer or memory mechanism.

## 10. Deterministic artifact boundaries remain valuable

### Archify

**REPOSITORY EVIDENCE.** Archify uses typed JSON intermediate representations, schema validation,
deterministic rendering, layout checks, atomic delivery, and a last-known-good boundary. It can
attach source evidence to authored components and cautions against inferring runtime impact from
visual proximity.

This repository is already analyzed in
`01 Research/Sources/Agent Harness & Workflow Repos — 2026-08-30.md`. That document records the
same core lessons: typed IR, deterministic validators, validation receipts, frozen validated
artifacts, and evidence grounding.

**Delta from this pass:** current-source revalidation supports the prior conclusion; no materially
new architecture question was found.

**Harness disposition:** CORROBORATION ONLY. Do not count the same repository twice when evaluating
convergence. The reusable pattern remains candidate → deterministic validation → publication.

## 11. Evaluation architecture hiding behind a marketing tool

### AI Marketing Panel

**REPOSITORY EVIDENCE.** The panel requires a real decision and first-party customer evidence before
building dossiers. It freezes a run packet, evaluates seats independently, records actual execution
mode and provenance, synthesizes only after reactions close, preserves dissent, applies scoring
separately from reactions, and later records real outcomes for calibration.

Its capability tiers are explicit:

| Tier | Execution | Repository's stated fidelity |
|---|---|---|
| T1 | Isolated parallel subagents | Full |
| T2 | Isolated sequential contexts | Full |
| T3 | Manual sequential execution without runtime isolation | Degraded |

**INTERNAL SYNTHESIS.** The portable mechanism is not famous-person simulation or synthetic customer
authority. It is:

```text
evidence packet
  → isolated evaluators
  → frozen independent findings
  → dissent-preserving synthesis
  → separate deterministic scoring where possible
  → recorded execution provenance
  → calibration against later outcomes
```

This is relevant to ACR as a reference implementation. It creates testable questions about whether
reviewers were actually isolated, whether execution degraded, whether synthesis erased minority
findings, and whether accepted/rejected findings can later be compared with production outcomes.

**Harness disposition:** ASSESS FOR ACR. Do not import customer personas or treat synthetic reaction
as real-user evidence.

## 12. Audience submissions: lineage and trust boundaries

### 12.1 Tim Harris Skills

**REPOSITORY EVIDENCE.** The repository states that 15 skills are adapted from Matt Pocock, 3 from
Lauren Tan, and 5 are original.

**Harness implication:** adapted material is useful distribution/usability evidence, but it is not
independent discovery of the inherited mechanism. Research synthesis should count the lineage once.

The distinct product idea worth retaining is that agents may gather and organize facts while named
human decision-makers retain material decisions.

**Disposition:** LINEAGE NOTE. Mine only distinct mechanisms.

### 12.2 Reckoner

**REPOSITORY EVIDENCE.** Reckoner accepts a broad set of API keys, admin keys, browser session
cookies, bearer/JWT material, cloud access keys, and service-account JSON. UI-entered credentials
are encrypted in SQLite with Fernet. The key is derived from `RECKONER_PASSWORD` using a fixed salt,
or is ephemeral when no password is configured. The same password gates the API; without it, the
auth dependency returns without requiring a token.

**INTERNAL SYNTHESIS.** A unified billing view creates a credential-concentration boundary whose
combined blast radius is larger than any single balance display. This is not evidence that the
project is malicious or unusable. It means convenience changes the trust model.

**Disposition:** PARK pending a dedicated deployment/source-security review before supplying real
credentials, especially before exposing the backend beyond localhost. Unrelated to PMB/ACR context
architecture.

### 12.3 Clodex

**REPOSITORY EVIDENCE.** Clodex translates the Anthropic Messages protocol expected by Claude Code
into the ChatGPT Codex backend's response protocol. It uses ChatGPT subscription OAuth. Its README
explicitly warns that using the backend from a client other than Codex CLI can cause rejection,
rate limiting, or account action. Its loopback proxy has no application authentication, so any local
process able to reach it can spend the subscription while it runs.

**Harness implication:** the repository cleanly demonstrates that model, provider backend, protocol,
and interactive harness are separable layers. The bridge's existence does not establish that the
combination is supported, safe, or operationally preferable.

**Disposition:** PARK as protocol-boundary evidence; REJECT for primary use while native Codex is
available.

## 13. Cross-repository mining

### 13.1 New or strengthened Harness Engineering findings

#### A. Context policy can affect truthfulness

Conciseness and simplicity rules may alter uncertainty handling, investigation depth, or stopping
behavior. They should be evaluated as behavioral interventions, not treated as harmless formatting.

**Classification:** RESEARCH REFINEMENT. This sharpens HE-001's capability-contribution/ablation
assessment; no new architecture is implied.

#### B. Context optimization changes the evaluated harness

Context Mode makes explicit that compression, interception, storage, retrieval, and compaction
recovery can be separate owned capabilities. Adding such a subsystem to a comparison changes the
harness and may invalidate causal attribution.

**Classification:** REINFORCE + ASSESS. Existing HE-001 runtime, context lifecycle, ownership, and
cost-attribution questions already cover the required investigation.

#### C. Capability packages contain multiple supply paths

OpenAI Plugins and Zapier MCP demonstrate that a package can distribute several capability
surfaces while implementing little or none of the remote execution itself.

**Classification:** REINFORCE HE-001 Question 23. Evaluate the bundled surfaces individually.

#### D. Execution provenance is part of evaluator independence

AI Marketing Panel distinguishes isolated parallel, isolated sequential, and unisolated manual
execution. That is stronger than recording only the intended reviewer topology.

**Classification:** ASSESS FOR ACR. A review result should not claim independence solely because
different reviewer labels were requested.

#### E. Corrected evidence is stronger than clean marketing

Ponytail's contaminated baseline correction and I Have ADHD's failed release gate are useful because
they preserve disconfirming evidence. Harness research should prefer auditable correction history
over unqualified benchmark headlines.

**Classification:** REINFORCE Evidence Before Architecture.

### 13.2 No-new-value or bounded-value findings

- Archify is a repeat and should not inflate convergence counts.
- Matt Pocock handoff/context pointers strengthen an existing research conclusion rather than create
  a new one.
- Humanizer is an editing capability, not a context architecture.
- ECC's breadth does not establish that PMB, ACR, or Harness Engineering need a comparable operating
  model.
- Reckoner's credential model and Clodex's protocol bridge belong to security/operational evaluation,
  not memory architecture.
- Zapier MCP belongs to concrete integration work, not generic Harness expansion.

### 13.3 Candidate future experiments

These are assessment candidates, not backlog authorization:

1. Compare an output-policy intervention against a baseline using correctness, calibrated
   uncertainty, retries, and user scanability—not length alone.
2. During a real cross-session/model transition, record the minimum state and artifact pointers the
   receiving harness actually needs; do not invent a transition system first.
3. If HE-001 observes material context pressure, compare a pinned context-management subsystem in an
   isolated experiment with raw-output recovery and task-quality measurement.
4. In ACR, record actual reviewer isolation/execution mode and test whether dissent survives
   synthesis.
5. During capability-supply-path inventory, decompose each plugin into discovery, context loading,
   execution, enforcement, and distribution surfaces.

## 14. Verification ledger

All links are pinned to the inspected revision.

| Repository | Commit | Primary paths |
|---|---|---|
| `ayghri/i-have-adhd` | [`4092de0`](https://github.com/ayghri/i-have-adhd/tree/4092de07ce3ed88389d77c0d623b7af89b40ac0e) | [`SKILL.md`](https://github.com/ayghri/i-have-adhd/blob/4092de07ce3ed88389d77c0d623b7af89b40ac0e/skills/i-have-adhd/SKILL.md), [`evals/RESULTS.md`](https://github.com/ayghri/i-have-adhd/blob/4092de07ce3ed88389d77c0d623b7af89b40ac0e/evals/RESULTS.md), [`evals/README.md`](https://github.com/ayghri/i-have-adhd/blob/4092de07ce3ed88389d77c0d623b7af89b40ac0e/evals/README.md) |
| `affaan-m/ECC` | [`8321021`](https://github.com/affaan-m/ECC/tree/8321021c54d670126ce3b2969d5deb880b4b0c2a) | [`README.md`](https://github.com/affaan-m/ECC/blob/8321021c54d670126ce3b2969d5deb880b4b0c2a/README.md), [Codex plugin README](https://github.com/affaan-m/ECC/blob/8321021c54d670126ce3b2969d5deb880b4b0c2a/.codex-plugin/README.md) |
| `DietrichGebert/ponytail` | [`356918e`](https://github.com/DietrichGebert/ponytail/tree/356918eba965ee1eac64bd3a7f0dd02108350de5) | [`SKILL.md`](https://github.com/DietrichGebert/ponytail/blob/356918eba965ee1eac64bd3a7f0dd02108350de5/skills/ponytail/SKILL.md), [agentic benchmark](https://github.com/DietrichGebert/ponytail/blob/356918eba965ee1eac64bd3a7f0dd02108350de5/benchmarks/results/2026-06-18-agentic.md), [comprehension follow-up](https://github.com/DietrichGebert/ponytail/blob/356918eba965ee1eac64bd3a7f0dd02108350de5/benchmarks/results/2026-06-22-issue-245-217-comprehension.md) |
| `zapier/zapier-mcp` | [`5360f15`](https://github.com/zapier/zapier-mcp/tree/5360f152b96735712e5f925ad728732cb86888df) | [`llms.txt`](https://github.com/zapier/zapier-mcp/blob/5360f152b96735712e5f925ad728732cb86888df/llms.txt), [agent policy](https://github.com/zapier/zapier-mcp/blob/5360f152b96735712e5f925ad728732cb86888df/plugins/zapier/agents/zapier-mcp.agent.md) |
| `humanlayer/skills` | [`3c26291`](https://github.com/humanlayer/skills/tree/3c2629142c5d437428269b1b722b08c0b87f574d) | [`improve-claude-md`](https://github.com/humanlayer/skills/blob/3c2629142c5d437428269b1b722b08c0b87f574d/plugins/improve-claude-md/skills/improve-claude-md/SKILL.md), [`README.md`](https://github.com/humanlayer/skills/blob/3c2629142c5d437428269b1b722b08c0b87f574d/README.md) |
| `openai/plugins` | [`1dc1958`](https://github.com/openai/plugins/tree/1dc195897af4161d039b80d8471ec0a10c9bbc89) | [`README.md`](https://github.com/openai/plugins/blob/1dc195897af4161d039b80d8471ec0a10c9bbc89/README.md), [manifest specification](https://github.com/openai/plugins/blob/1dc195897af4161d039b80d8471ec0a10c9bbc89/.agents/skills/plugin-creator/references/plugin-json-spec.md) |
| `tt-a1i/archify` | [`a07fa1d`](https://github.com/tt-a1i/archify/tree/a07fa1d5b2a10cbea110c5a2be2817397a301cdc) | [`README_EN.md`](https://github.com/tt-a1i/archify/blob/a07fa1d5b2a10cbea110c5a2be2817397a301cdc/README_EN.md), [schema contract](https://github.com/tt-a1i/archify/blob/a07fa1d5b2a10cbea110c5a2be2817397a301cdc/archify/schemas/README.md) |
| `mksglu/context-mode` | [`ba5f5df`](https://github.com/mksglu/context-mode/tree/ba5f5dfd1a0cd3e8a8f812c219d50390ed0a61c8) | [`README.md`](https://github.com/mksglu/context-mode/blob/ba5f5dfd1a0cd3e8a8f812c219d50390ed0a61c8/README.md), [platform matrix](https://github.com/mksglu/context-mode/blob/ba5f5dfd1a0cd3e8a8f812c219d50390ed0a61c8/docs/platform-support.md), [`src/store.ts`](https://github.com/mksglu/context-mode/blob/ba5f5dfd1a0cd3e8a8f812c219d50390ed0a61c8/src/store.ts) |
| `mattpocock/skills` | [`3cca18b`](https://github.com/mattpocock/skills/tree/3cca18b368ae95cdbdebbff572ccafa662551015) | [`handoff`](https://github.com/mattpocock/skills/blob/3cca18b368ae95cdbdebbff572ccafa662551015/skills/productivity/handoff/SKILL.md), [`writing-for-agents`](https://github.com/mattpocock/skills/blob/3cca18b368ae95cdbdebbff572ccafa662551015/skills/productivity/writing-for-agents/SKILL.md) |
| `blader/humanizer` | [`9862685`](https://github.com/blader/humanizer/tree/9862685f575c65a8247f90369951df1b3416e3d6) | [`SKILL.md`](https://github.com/blader/humanizer/blob/9862685f575c65a8247f90369951df1b3416e3d6/SKILL.md), [`AGENTS.md`](https://github.com/blader/humanizer/blob/9862685f575c65a8247f90369951df1b3416e3d6/AGENTS.md) |
| `openai/skills` | [`49f948f`](https://github.com/openai/skills/tree/49f948faa9258a0c61caceaf225e179651397431) | [`README.md`](https://github.com/openai/skills/blob/49f948faa9258a0c61caceaf225e179651397431/README.md) |
| `CaptainASIC/reckoner` | [`8a5d5b0`](https://github.com/CaptainASIC/reckoner/tree/8a5d5b0d77f0461abf98e61709cf02d94c63fddb) | [`README.md`](https://github.com/CaptainASIC/reckoner/blob/8a5d5b0d77f0461abf98e61709cf02d94c63fddb/README.md), [`backend/crypto.py`](https://github.com/CaptainASIC/reckoner/blob/8a5d5b0d77f0461abf98e61709cf02d94c63fddb/backend/crypto.py), [`backend/auth.py`](https://github.com/CaptainASIC/reckoner/blob/8a5d5b0d77f0461abf98e61709cf02d94c63fddb/backend/auth.py) |
| `timharris707/skills` | [`a9317e0`](https://github.com/timharris707/skills/tree/a9317e03733da7f54b5da0eaa8edcb2697495cf5) | [`README.md`](https://github.com/timharris707/skills/blob/a9317e03733da7f54b5da0eaa8edcb2697495cf5/README.md), [`handoff`](https://github.com/timharris707/skills/blob/a9317e03733da7f54b5da0eaa8edcb2697495cf5/skills/run/handoff/SKILL.md) |
| `Aotricx/Clodex` | [`341f4bb`](https://github.com/Aotricx/Clodex/tree/341f4bbf2770c83de91ba8b5b8ce5b217d3b20f9) | [`README.md`](https://github.com/Aotricx/Clodex/blob/341f4bbf2770c83de91ba8b5b8ce5b217d3b20f9/README.md), [`docs/EVIDENCE.md`](https://github.com/Aotricx/Clodex/blob/341f4bbf2770c83de91ba8b5b8ce5b217d3b20f9/docs/EVIDENCE.md) |
| `rodaddy/ai-marketing-panel` | [`002eb2f`](https://github.com/rodaddy/ai-marketing-panel/tree/002eb2ff1eeb103000cdc639a66619016ce0d42e) | [operations manual](https://github.com/rodaddy/ai-marketing-panel/blob/002eb2ff1eeb103000cdc639a66619016ce0d42e/OPERATIONS-MANUAL.md), [wash workflow](https://github.com/rodaddy/ai-marketing-panel/blob/002eb2ff1eeb103000cdc639a66619016ce0d42e/skills/synthetic-customer-panel/workflows/wash-artifact.md), [calibration workflow](https://github.com/rodaddy/ai-marketing-panel/blob/002eb2ff1eeb103000cdc639a66619016ce0d42e/skills/synthetic-customer-panel/workflows/calibrate-panel.md) |

## 15. Corrections and evidence limits to preserve

1. **OpenAI did not kill skills.** It deprecated the standalone catalog in favor of plugin-based
   distribution, and plugins may still contain skills.
2. **Zapier's public repository is not the MCP server implementation.** It is the client-side plugin
   and guidance layer; the hosted server is closed source.
3. **Context Mode's current client limitation is not the earlier one.** Current source documents
   Claude Code input rewriting as supported and Codex PreToolUse input rewriting as unsupported.
4. **I Have ADHD has evaluation evidence, but its recorded release gate failed.** Do not summarize
   it as proven to improve every case.
5. **Ponytail's 54%/22%/20%/27% figures are self-reported project measurements.** Correction history
   increases credibility; it does not turn the results into independent evidence.
6. **Tim Harris is not independent corroboration for the skills adapted from Matt Pocock and Lauren
   Tan.** Preserve lineage instead of manufacturing consensus.
7. **Archify is already in the Harness research corpus.** This pass revalidates the source; it is not
   a second independent example.

## 16. Final disposition

### REINFORCE

- Evidence Before Architecture.
- Minimum sufficient instruction rather than minimum instruction.
- Handoff as transient state plus canonical pointers.
- Deterministic validation for mechanically observable properties.
- Capability-supply-path classification.
- Correction history and negative evidence as first-class research evidence.

### ASSESS

- Behavioral-policy effects on correctness and calibrated uncertainty.
- Context interception/retrieval as an owned harness subsystem.
- Plugin surfaces individually rather than as a single capability.
- Actual evaluator isolation and execution provenance in ACR.

### PARK

- ECC as a complete harness framework.
- Context Mode adoption before a measured context problem and isolated comparison.
- Humanizer as persistent policy.
- Reckoner before a dedicated security/deployment review.
- New plugin packaging or transition infrastructure.

### REJECT FOR CURRENT USE

- Clodex as a primary path while native Codex is available.
- Treating derivative skill collections as independent evidence.
- Treating benchmark headlines as architecture decisions.
- Creating a new handoff command or memory subsystem from this batch.

This document is research input. It changes no Harness Engineering decision and authorizes no PMB,
ACR, or Harness implementation.
