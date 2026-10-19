# PMB External Research Transfer — 2026-10-19

## Purpose and evidence boundary

Preserve the useful source trail from PMB research PRs **#22, #23, and #24** in HE's existing research hierarchy, without merging several large general-purpose studies into the PMB product repository. This is a *transfer index and distilled source record*, not new HE architecture.

These notes were first assembled in PMB on 2026-08-29. Below, "recorded" means the **original PMB research author reported inspecting** a source; this transfer did **not** independently replay all upstream repository/code inspections. Treat observations as attributed source reports, not newly verified facts or measurements.

## Source identity and version-pinned original research

| Original PMB PR | Research preserved at the original commit | Transfer decision |
|---|---|---|
| [#22](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/22) | [AI That Works](https://github.com/unyieldingclaw-dev/personal-memory-bank/blob/98fbd7d79f529fc5e23916df5c0ec2cd5bb647f4/docs/AI-THAT-WORKS-SOFTWARE-FACTORY.md) and [Codex review](https://github.com/unyieldingclaw-dev/personal-memory-bank/blob/98fbd7d79f529fc5e23916df5c0ec2cd5bb647f4/docs/OPENAI-CODEX-HARNESS-RESEARCH.md) | Retain source versions and distinctive review/feedback-loop evidence; merge conceptual findings with HE's existing assessment work |
| [#23](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/23) | [Memory and out-of-band consolidation](https://github.com/unyieldingclaw-dev/personal-memory-bank/blob/16d0724c6fff5246ffa1216d1e6e7e16f7fde2f6/docs/research/agent-memory-and-context.md), [Codex findings](https://github.com/unyieldingclaw-dev/personal-memory-bank/blob/16d0724c6fff5246ffa1216d1e6e7e16f7fde2f6/docs/research/openai-codex-findings.md), and [staging rationale](https://github.com/unyieldingclaw-dev/personal-memory-bank/blob/16d0724c6fff5246ffa1216d1e6e7e16f7fde2f6/docs/research/README.md) | Retain links and source qualification; no new standing PMB research-inbox workflow |
| [#24](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/24) | [Maka, Modular, FreeLLMAPI evidence](https://github.com/unyieldingclaw-dev/personal-memory-bank/blob/71db188c9b5902458f8b3718b8654a362790edb1/docs/research/EXTERNAL-AGENT-SYSTEMS-MINING-2026-08-29.md) | Preserve Maka and Modular's distinct source mechanics; treat FreeLLMAPI as already covered in HE |

The commit-linked copies provide the detailed provenance and observations. A closed PR is still browseable, but old unreachable Git objects should not be considered a guaranteed independent archive. This note retains the principal source identity and conclusions if those links later become unavailable.

## Distinctive observations to retain

### AI That Works — separating discovery, execution, and evidence

- **Upstream identity:** [ai-that-works/ai-that-works](https://github.com/ai-that-works/ai-that-works), original PMB inspection pinned to commit `51970381041be7b65a2bf06470a5e71930da04e1` (2026-08-24). The original study lists specific episode directories, including issue/PR loops, agent backpressure, observability, harness minimalism, and prompt/model comparisons.
- **Source-reported mechanism:** An *issue loop* identifies and validates work; a separate *PR loop* implements, checks, reviews, and proposes merge. Cheap deterministic backpressure should reject obvious mistakes before expensive judgment.
- **HE relationship:** **SHARPENS** existing artifact-gated SDLC / behavioral-eval work. Logged evidence and acceptance criteria must be attributable to the actual run; don't infer that more agents or more dashboards improve acceptance.
- **Not proved here:** A software factory is necessary for PMB or that the upstream workflows outperform existing HE approaches.

### OpenAI Codex — executable review-target and finding contracts

- **Upstream identity:** [openai/codex](https://github.com/openai/codex), original PMB inspection pinned to commit `f5636bb733c4653a6b91413fed1aaf8842374f2e`. Original notes cite `AGENTS.md` and `codex-rs/skills/src/assets/samples/review-agent/SKILL.md`.
- **Source-reported mechanisms:** Review the *merge-relevant diff*, not whichever working-tree diff is convenient; make findings actionable and introduced by the reviewed change; **no findings** is a valid result; preserve the reviewed target identity and evidence.
- **HE relationship:** **REINFORCES** existing risk-directed review, verifiable evidence, and consumer-specific contracts. Whether ACR implements these correctly belongs to an ACR-specific code/calibration review, not an HE implementation mandate.
- **Related unverified lead:** PMB #23 describes a community-reported Codex instruction-layer/meta-workflow issue; a user report is not first-party proof of general client behavior.

### Apache Maka — one execution authority and durable event history

- **Upstream identity:** [apache/maka](https://github.com/apache/maka); original PMB #24 reports inspecting `README.md`, `ARCHITECTURE.md`, `SECURITY.md` on 2026-08-29; **no upstream commit SHA pinned** in that note.
- **Source-reported mechanism:** A Runtime Host governs execution; a Runtime Event Log is the record from which projections such as UI, model-facing history, recovery, and eval views are derived. Security documentation distinguishes heuristics from actual enforcement.
- **HE relationship:** **SHARPENS** existing canonical/derived-state separation and control/assurance planes. A runtime event log should not be mistaken for a PMB memory design requirement.

### Modular — stable consumption versus active change

- **Upstream identity:** [modular/modular](https://github.com/modular/modular); original PMB #24 reports inspecting `README.md` and `CONTRIBUTING.md` on 2026-08-29; **no upstream commit SHA pinned**.
- **Source-reported mechanism:** Nightly and stable/released consumption paths differ; contribution procedures and scope are explicit. This is *release and API boundary* evidence, not evidence for a model router.
- **HE relationship:** **REINFORCES** existing development-state versus published-state, portability, and scope/ownership research.

### Memory and consolidation — stage claims by provenance

- **PMB #23 evidence:** User-supplied presentation/video/screenshot material discusses structured writable memory, “dreaming”/offline consolidation, and long-running multi-agent tools such as Herdr. The original note explicitly says these are **research leads not independently verified product claims**.
- **HE relationship:** **MERGES WITH** existing `01 Research/Sources/PMB Dream — Retrospective Memory Consolidation — 2026-09-01.md`, `01 Research/Context Engineering.md.md`, and session-isolation research. Any concurrency, attribution, freshness, or consolidation requirements must come from a reproduced PMB problem, not from generic agent-system diagrams.
- **No adoption:** No autonomous dream feature, vector store, multiplexer, or multi-agent memory system is approved.

### FreeLLMAPI — already covered

- **Upstream identity:** [tashfeenahmed/freellmapi](https://github.com/tashfeenahmed/freellmapi). PMB #24 identifies status, retries, sticky routing, explicit handoff, and observable capabilities.
- **HE relationship:** **MERGES WITH**, rather than copying, `01 Research/Sources/FreeLLMAPI — Gateway Routing, Failover & Context Compression — 2026-09-14.md` and `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`.
- **No adoption:** No gateway installation, provider abstraction, or runtime routing required of PMB.

## Disposition

Keep this source-specific evidence in HE. Retire redundant *open PRs* on PMB only after this HE research is committed to a retained branch/PR; do not delete their historic commits or attribute their content to implementation evidence. Leave PMB bug fixes, operational integrations, review gates, and the product's own authoritative knowledge unaffected.

**No architectural decisions, runtime changes, or new validation thresholds.**
