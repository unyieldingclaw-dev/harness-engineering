# Simon Willison — Harness Engineering Findings — 2026-11-16 through 2026-11-22

**Reviewed:** 2026-11-23  
**Source:** [Simon's November 2026 Weblog archive, pages 2–3](https://simonwillison.net/2026/Nov/?page=2), [page 3](https://simonwillison.net/2026/Nov/?page=3)  
**Scope:** Coding agents, sandboxing, model/runtime behavior, verification, evaluation, authority, tool design, local inference, PMB/ACR/GEP applicability.  
**Status:** Research evidence only; no implementation, architecture, or security approval. Claims below are attributed; HE implications are separate.

## 1. External sandboxes for coding agents (November 17)

**Source:** [Simon's linked post, co-authored with Prime Radiant](https://blog.primeradiant.com/external-sandboxes-for-coding-agents/); [v86 implementation](https://github.com/prime-radiant-inc/external-sandbox-v86).

**Observed / Simon and Prime Radiant's claims:** Three experimental implementations (v86/WebAssembly, Apple Containers, Quicksand/QEMU) expose a shared localhost execution protocol. Agents run outside the sandbox, dispatching commands to a guest with explicitly mounted read-only/read-write folders and default-deny networking; an optional allow-list HTTP(S) proxy can substitute host-side credentials. A protocol specification and conformance tests exist. The authors explicitly say these prototypes are not independently audited for sensitive workloads.

**Critical boundary qualification:** Tool coverage differs by client. Claude Code routes Bash, but native Read/Edit still execute on the host; Codex routes process and filesystem operations including apply_patch; Pi can replace its native tools and disable built-ins. A read-write mount still allows destructive changes to mounted files. An allowed upstream may echo a substituted secret. These facts prevent treating the phrase “sandboxed agent” as proof of end-to-end isolation.

**HE analysis:** This is a concrete example of *execution-environment separation* and *composed authority*. A control is only meaningful for the actual operations routed through it. A host-side agent with native file tools is not fully confined by a Bash sandbox. Credential non-exposure to guest/model and permitted use of credentials by a proxy are distinct properties. For local ACR experiments, it is a candidate comparative pattern, not evidence that the current ACR requires or can safely adopt it.

**Classification:** Challenges Current Thinking.  
**Disposition:** **REINFORCE** effective-authority composition and deterministic boundaries; **ASSESS** per-client tool routing, host-native bypass, mount modes, proxy policy, secret substitution and failure behavior if such a boundary is ever evaluated; **PARK** deploying or building an external sandbox without observed need and independent threat-model testing; **REJECT** “all agent actions are sandboxed” as an inference from Bash interception.

## 2. curl Rust rewrites and the moving upstream baseline (November 18)

**Source:** [Simon's quotation of Daniel Stenberg](https://simonwillison.net/2026/Nov/18/), [Stenberg's original article](https://daniel.haxx.se/blog/2026/11/18/actual-curl-rewrites-in-rust/).

**Observed / attributed claim:** Stenberg reported that while AI-assisted efforts translated approximately 185,000 lines of curl C into 340,000 Rust lines over roughly three weeks, the upstream curl project merged over 300 commits including more than 200 bug fixes and some vulnerability fixes. This is the maintainer's reported count, not a measured assessment of every port's quality.

**HE analysis:** A large generated rewrite can appear feature-complete against a frozen snapshot while missing ongoing upstream corrections. The problem is *baseline freshness and parity evidence*, not whether Rust or C is inherently preferable. For GEP, a migration plan must account for moving source behavior, integration tests, compatibility, and ongoing maintenance; for ACR, translated volume is not verified parity.

**Classification:** Relevant.  
**Disposition:** **REINFORCE** provenance, freshness and independent conformance checks; **ASSESS** upstream drift only when an actual rewrite/port task occurs; **REJECT** line counts or translation completion as correctness evidence; **PARK** a general migration-monitoring subsystem.

## 3. Reasoning configuration can create disproportionate cost (November 20)

**Source:** [Simon's llm-gemini 0.38.1 release note](https://simonwillison.net/2026/Nov/20/).

**Observed / Simon's test:** He corrected a Gemini plugin adapter omission: Hydrogen and Neon supported xhigh effort, unlike Carbon. On one SVG example, Hydrogen high used 5,311 output tokens, cost about $0.0399 and took 19 seconds; xhigh used 31,477 output tokens, cost about $0.2361 and took 2m36s. Visual output appeared more detailed. These are single creative examples, not engineering-task evals.

**HE analysis:** Effort support is an adapter-specific capability; a missing effort option can confound model comparisons. A more detailed visual result is not proof of better defect detection or code correctness. The increase in tokens, cost and time reinforces workload-level economics rather than a default “maximum reasoning” policy.

**Classification:** Worth Investigating.  
**Disposition:** **REINFORCE** measured quality/cost/latency comparisons; **ASSESS** supported effort levels and actual applied configuration when evaluating a model adapter; **PARK** automatic effort routing without task evidence.

## 4. LLM 0.37.3: SQLite bug fix changed observed JSON semantics (November 16)

**Source:** [Simon's LLM release note](https://simonwillison.net/2026/Nov/16/), [SQLite 3.54.1 discussion linked therein](https://sqlite.org/).

**Observed / Simon's report:** A SQLite fix changed a scalar subquery containing json_array() from being interpreted as JSON in an outer json_object() to being interpreted as SQL text. This broke LLM logging in the Homebrew-installed configuration; explicit json() wrapping restored intended semantics.

**HE analysis:** A dependency correction can invalidate an application's reliance on undocumented or incorrect prior behavior. This is a bounded regression case for runtime provenance and behavior-based tests, not an argument to freeze dependencies or invent an adapter framework.

**Classification:** Relevant.  
**Disposition:** **REINFORCE** dependency/runtime provenance and behavioral regression checks; **ASSESS** only if analogous serialization/logging paths exist; **PARK** generic dependency governance.

## 5. Token fraud through agent trials (November 20)

**Source:** [Simon's linked Claire Vo account](https://simonwillison.net/2026/Nov/20/).

**Observed / attributed account:** Simon relayed Claire Vo's report of repeated free-trial and promotional-code abuse of ChatPRD, including alleged resale of inference access and third-party commercial workloads. The operational scale and causes are self-reported, not independently audited here.

**HE analysis:** An exposed agent endpoint can carry financial and external-execution authority even without direct filesystem access. This reinforces the prior hard-budget-cap and bounded-resource findings. HE/ACR do not currently have a demonstrated public free-trial exposure requiring a new quota subsystem.

**Classification:** Relevant.  
**Disposition:** **REINFORCE** mechanically bounded external resource authority; **PARK** trial-abuse controls absent a relevant exposed service; **REJECT** treating a prompt-level spending instruction as a hard cap.

## 6. Muru development diary and human-owned acceptance (November 21)

**Source:** [Simon's Muru link roundup](https://simonwillison.net/2026/Nov/21/); [Graham Dumpleton's account](https://grahamdumpleton.me/).

**Observed / attributed account:** Simon points to Graham Dumpleton's extensive AI-assisted engineering diary for Muru. Dumpleton says model-assisted production still depends on his judgment about what is not good enough and sustained polishing informed by prior operational experience. No controlled comparison or reliability audit is provided.

**HE analysis:** This is a credible workflow illustration for GEP: generation can be delegated while acceptance, compatibility, and risk remain human-owned. It reinforces existing research but is not evidence that the specific Muru architecture should be copied.

**Classification:** Relevant.  
**Disposition:** **REINFORCE** human-owned acceptance and evidence; **PARK** Muru as an architecture template.

## Other reviewed material — no promotion

- GPT-6.2 Astra/Sol and Gemini Neon release comparisons (November 17–18): price and subjective creative examples, insufficient ACR task evidence; **No Action** beyond existing model economics research.
- Datasette Lite service-worker/Pyodide rewrite (November 17): useful browser deployment, no demonstrated HE/PMB need; **No Action**.
- PostgreSQL 19 read-your-writes feature and AI-vulnerability-report backlog (November 19): concrete database engineering, but no HE database architecture justification; **No Action**.
- Browser select styling tool (November 22), mathematical proof claims (November 20), and bird sightings: outside scope; **No Action**.

## Deduplication and HE-001 boundary

The existing Context Engineering synthesis already covers composed authority, independent verification, minimum runtime provenance, human-owned acceptance, and complete task economics. The genuinely sharper new observation is **client-specific tool interception: a sandbox around Bash is not a sandbox around native host Read/Edit**. Preserve that qualification in synthesis and assess only when evaluating actual agent execution boundaries. The moving-upstream example is supporting evidence for freshness and parity, not a new subsystem.

**No HE, PMB, ACR, GEP, or runtime architecture changes are authorized by this source record.**
