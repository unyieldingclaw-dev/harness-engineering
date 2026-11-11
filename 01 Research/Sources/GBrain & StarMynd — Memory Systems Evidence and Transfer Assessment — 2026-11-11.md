# GBrain & StarMynd — Memory Systems Evidence and Transfer Assessment — 2026-11-11

**Status:** Research only; not an adoption or implementation decision. Captured while the proposed Home/Work AI Engineering Cockpit is parked until PMB close-out. No product installation, code change, data ingestion, or work-environment access is implied.

**Question:** What evidence from GBrain and StarMynd should Harness Engineering preserve, and does any of it justify a new PMB, ACR or Dashboard mechanism?

## Source / evidence ledger

| Source | Verified public surface / limitation |
| --- | --- |
| [garrytan/gbrain](https://github.com/garrytan/gbrain) and [engine guide](https://github.com/garrytan/gbrain/blob/master/docs/ENGINES.md) | Public MIT source and documentation describe a Bun/TypeScript memory service with a file-backed Markdown knowledge layer, local PGLite or PostgreSQL backend, keyword search, optional embedding/reranking, citations, and typed relationships. Repository documentation is evidence of design/implementation claims, **not** proof that these mechanisms improve our projects. |
| [GBrain README benchmark discussion](https://github.com/garrytan/gbrain) | Author-reported BrainBench relationship retrieval R@5 improves **0.6626 → 0.7241**; the benchmark corpus is synthetic and task-specific. Graph-oracle upper bounds are **not** production scores; R@5 is **not** end-to-end answer correctness, security, latency or cost evidence. |
| [StarMynd Infinite Brain OS](https://github.com/starmynd-org/infinite-brain-os) and [getting started](https://github.com/starmynd-org/infinite-brain-os/blob/main/START-HERE.md) | MIT, Git-backed Markdown/YAML starter with explicit schema/validator, namespace routing, operators approving canon, agent workflow examples, and no mandatory database/server. The repo's document/rule graph is not evidence of operational effectiveness in our environments. |
| [Infinity OS Foundation download status](https://www.infinity-os.org/download) | The public `infinity-os` is described as **alpha** with older tags and no packaged release. The new Infinity OS version is **not published** and factory blueprints/standard are planned. Do not conflate these with the MIT Infinite Brain OS starter. |
| [StarMynd ABI](https://www.starmynd.com/starmynd-abi) | Separately marketed, private-beta managed ingestion/data-modeling service; advertised founding price **from $895/month**, not a capability bundled with the free starter. Commercial examples and promises are vendor-reported, not independently validated here. |
| Existing HE research | [Observability & Dashboard Boundary (2026-09-18)](../AI%20Engineering%20Observability%20%26%20Dashboard%20Boundary%20%E2%80%94%202026-09-18.md), [Home/Work Cockpit Scope (2026-11-10)](../AI%20Engineering%20Cockpit%20%E2%80%94%20Home%20and%20Work%20Multi-Provider%20Scope%20%E2%80%94%202026-11-10.md), and this PR's Mansel notes already own most dashboard/source-authority decisions. |

Source check performed 2026-11-11. Live repositories and vendor pages are moving targets; pin code commits and rerun the assessment if an adoption decision is proposed.

## Distinct systems — avoid category errors

### GBrain: actual memory/retrieval system

- Canonical Markdown/source files, explicit fact corrections and withdrawal, provenance, and read/query surfaces. PGLite is a local in-process option; PostgreSQL is an alternative for larger/shared installs.
- Hybrid keyword/semantic search, reciprocal-rank fusion, typed graph traversal and optional cited synthesis are **implemented/product-described capabilities**, but not reasons to add a database or graph to PMB.
- The database can serve as an index of file-backed knowledge, but the product documentation distinguishes DB-only records/operational state that still need backup. Do not claim complete reversibility from Markdown export alone.
- Installation can modify agent instructions, MCP entries and related host setup. Optional providers, embeddings, enrichment and connected clients can introduce text disclosure, compute cost and higher operational complexity. Nothing was installed or inspected locally.
- **Potential lesson:** correction/withdrawal must invalidate stale retrieval; test a query on evidence-required tasks, not merely successful search hits. **Rejected as a current plan:** migrating PMB to GBrain, adding PGLite/Postgres, graph traversal or autonomous consolidation without a reproduced failure and comparative benefit.

### StarMynd Infinite Brain OS: file-based governance and handoff pattern

- Git-backed typed files, schema validation, `INDEX` routing, source pointers instead of live values, and explicit promotion into operator-approved canon.
- Proposed lifecycle (raw evidence / support / synthesis / canon / approved canon) has a useful **separation-of-authority** idea. The distinction between validation rules and human decisions deserves inspection.
- The published starter has example entities, workflows and documentation; not every aspirational feedback loop, factory, data integration or agent control feature is a running product.
- **Potential lesson:** few explicit transitions and machine-checked frontmatter when they demonstrably prevent provenance errors. **Rejected as a current plan:** copying its entire knowledge ontology, governance ladder, departmental graph, swarms, agent runtime, or process machinery into PMB/HE.

### Commercial or future StarMynd offerings

- Company Brain/client deployments and StarMynd ABI are service offerings with different access/price conditions; **not** proof that the open starter includes a working corporate data plane.
- Infinity OS and proposed factories are not a deployable, verified substitute for a multi-provider engineering dashboard.
- Treat all vendor throughput, payback, operational-outcome and data-correctness claims as unverified without a reproducible evaluation.

## Transfer decisions by owner

| Owning component | Mine / measure | Do **not** turn into a requirement |
| --- | --- | --- |
| **HE (research)** | Source-authority mapping, evidence labels, human-approved promotion, provenance/correction test patterns. Compare with existing HE notes before adding policy. | Another permanent governance tier, universal agent routing or new implementation framework. |
| **PMB** | Evaluate retrieval **only** on held-out tasks requiring hidden detail; compare read-all control vs bounded startup **plus** retrieval; mutation-test broken pointers; audit compaction and write-rate preservation. These are already captured in PMB closed PRs [#83](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/83) and [#95](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/95), and [backlog via #140](https://github.com/unyieldingclaw-dev/personal-memory-bank/pull/140). | Reopen rejected NS-35 tiering by relabeling it; install external memory, vector store, graph or background “dreaming” before evidence. Do not block PMB release for this research. |
| **ACR** | Keep review result/evidence provenance and deterministic result interpretation. | Embed a memory substrate or let a new brain authorize code modifications/reviews. |
| **Separate Dashboard** | Observe source-health, freshness, errors, usage and limitations from native provider/Git/PMB/ACR sources **when available**. Link to owning evidence. | Become the memory source of truth, agent orchestrator, connector permissions broker, transcript archive, retrieval engine or automated fix loop. |

## Evidence-first test questions for later, not now

1. **Observed need:** Which recurring wrong or unanswerable decision is caused by missing/retrieved context, versus bad writes or misunderstood authority? Show cases.
2. **Counterfactual:** Does plain deterministic file search or better local source pointers solve it with lower overhead?
3. **Evaluation:** Compare evidence-required answer correctness, authority selection, abstention, recall, context cost, latency, human correction and failure on deliberately broken pointers; distinguish vendor benchmark from our held-out results.
4. **Security:** Can provenance and access boundaries be proven without exporting local/work material, changing agent global instructions, or retaining sensitive prompts?
5. **Decision:** Adopt only the smallest independently useful mechanism in the owning project; otherwise archive these ideas and stop.

**Disposition:** Preserve evidence in HE; no architecture adoption, no PMB/ACR change, no dashboard integration. The important reusable ideas are already largely present in existing HE/PMB research. A coherent new system is not proof of a missing problem.
