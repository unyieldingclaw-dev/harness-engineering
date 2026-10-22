# HE Corpus Provenance, Freshness & Open Research Reconciliation — 2026-10-22

**Status: INITIAL INVENTORY + TARGETED SOURCE CHECKS COMPLETED; FULL CORPUS VERIFICATION NOT COMPLETED.**  
**Owner:** HE research / evidence review. **No HE-002 decision, feature, or runtime adoption.**

## Purpose and audit integrity

Answer whether HE's expanding research corpus can distinguish **dated observations**, **currently valid implementation claims**, **supported synthesis**, and **approved decisions**, and finish the initial disposition of older open PRs before treating any notes as accepted architecture.

An audit is not a search-and-replace to make historical sources match today's products. Preserve pinned old versions and clearly record *what changed*, current primary authority and whether the change affects the conclusion. Never re-date an old observation as if it were a new experiment.

## Verified repository baseline (GitHub, 2026-10-22 UTC)

- Repository: [HE](https://github.com/unyieldingclaw-dev/harness-engineering); **main** at `ef1d5b82350aac22c7a3a10dd3e3e5a02d8b7e12` (2026-10-21 13:21 UTC).
- Read-only GitHub recursive tree: **111 entries, 104 tracked blobs**, nontruncated. The repository is overwhelmingly Markdown research, with no general runtime test suite currently demonstrated by this investigation.
- **11 open PRs**, all **draft** at the last live snapshot: #5, #6, #7, #8, #9, #10, #12, #13, #15, #19, #20. Most add historical source notes; #20 also proposes a 16-line HE-001 assessment extension.
- Each of the eight older PR branches was brought forward onto main through history-preserving commits in the prior review. This is **mechanical branch hygiene**, not source-claim validation or endorsement. All remain unmerged.
- [HE-001](../02%20Assessments/HE-001%20%E2%80%94%20Baseline%20Harness%20Assessment.md.md) is **IN PROGRESS**; [HE-002](../03%20Decisions/Decisions%20Index.md.md) is **PENDING HE-001**. No decisions have been changed.
- The [publication boundary](../Start%20Here.md) prohibits publishing nonpublic employer/client implementation details or plans. This audit uses public HE and public upstream materials only.

## Initial older-PR disposition — research investigation

The eight older PRs are **committed, pushed, and readable**; they are not uncommitted workspace files. The question is whether their **incremental evidence** belongs in canonical HE research, not whether a merge operation is technically possible.

| PR | Material reviewed in this investigation | Distinct potentially useful evidence | Existing HE ownership / duplication | Decision today |
| --- | --- | --- | --- | --- |
| [#5 — David Ondrej](https://github.com/unyieldingclaw-dev/harness-engineering/pull/5) | Eight-skill source note: guardrails, worktrees, goal loop, decisions, liveness, API authority | Multi-dimensional isolation and liveness *versus useful progress* | Existing David Ondrej prior art, bounded envelopes, budgeted side effects, authority gates | **PRESERVE HISTORICAL SOURCE / HOLD MERGE**; verify exact current source implementation and actual incremental tests |
| [#6 — Corpus & Matt](https://github.com/unyieldingclaw-dev/harness-engineering/pull/6) | Corpus admission criteria and early Matt Pocock note | Independent provenance, source count ≠ independent support, MERGE/SUPERSEDE ownership | `Start Here`, philosophy, source-preservation and later Matt source | **CONSOLIDATE WITH EXISTING OWNER**, not another always-on research law; earlier unresolved private/session citations were subsequently corrected in this PR |
| [#7 — Archify](https://github.com/unyieldingclaw-dev/harness-engineering/pull/7) | Typed candidate/validation, architecture delta, external architecture-conformance survey | Explicit candidate freezing, last-known-good publication, architecture delta versus mere image diff | Artifact-Gated SDLC; receipt/verification and ACR boundary | **PRESERVE PINNED PRIOR ART / HOLD**; second note has corrected transient source links, but claims have not been experimentally reproduced |
| [#8 — Hindsight + OpenMAIC](https://github.com/unyieldingclaw-dev/harness-engineering/pull/8) | Five documents across two products; governed memory admission and durable execution | Fenced attempt identity, recovery, retrospective extraction without automatic policy mutation | Supervised Orchestration, Experience-Derived Evolution, PMB owner | **SPLIT FINDINGS BY OWNER; HOLD**, PMB admission matrix is not a production-approved policy |
| [#9 — Rta-Smriti](https://github.com/unyieldingclaw-dev/harness-engineering/pull/9) | Alpha-era local reality substrate/context compiler, freshness and provenance | Task-bounded evidence compilation and explicit uncertainty | Derived Index Economics, context authority, PMB memory/source separation | **PRESERVE AS VERSIONED PRIOR ART; HOLD**; revisit fresh release and actual consumer before architecture |
| [#10 — Radiant](https://github.com/unyieldingclaw-dev/harness-engineering/pull/10) | Mac-first harness, execution-projected task state, user block state, ship-state distinction | Useful failure states and runtime/publishing semantics | Supervised Orchestration, Bounded Execution, artifact acceptance | **PRESERVE PRIOR ART / HOLD**; platform and source-version limitations prevent universalizing |
| [#12 — Context efficiency](https://github.com/unyieldingclaw-dev/harness-engineering/pull/12) | Three notes including explicit Ponytail re-review and OmniRoute | **The corrected second pass itself:** README-level interpretation materially changed after reading AGENTS/implementation; externalization and lossy compression require independent acceptance | Context Engineering, Derived Index Economics, model routing | **CONSOLIDATE CORRECTIONS / HOLD**; do not admit all 1,572 lines as new HE policy, no measured counterfactual in HE |
| [#13 — Top ten repositories](https://github.com/unyieldingclaw-dev/harness-engineering/pull/13) | Ten-repo comparison, some overlap with #7, #12 and #20 | Different intervention surfaces; context interception != style guidance; evaluator independence | Skill Routing, supply-chain, ACR evaluation | **CONSOLIDATE UNIQUE PINNED EVIDENCE / HOLD**; copied/derivative sources not independent corroboration |

**Conclusion from the older PR investigation:** They contain **real source evidence worth retaining**, but the strongest general principles are **already represented on main**. No eight-PR batch earns automatic merge; any incremental source note can be admitted only with owner mapping, version and acceptance review. These are review dispositions, **not assertions that the third-party implementations have been executed or that every individual claim was reverified**.

## Current-documentation spot checks (limited, material)

**2026-10-22 primary-documentation cross-checks:**

1. **Codex hooks are a real current product surface**, but installation/activation is not proven merely by hook JSON. Official documentation describes `SessionStart`, `PreToolUse`, managed versus trusted hooks, plugin-bundled hooks, project trust, concurrent matching hooks and exact-hash review of changed hook definitions. Sources: [official Hooks documentation](https://learn.chatgpt.com/docs/hooks) and [OpenAI plugin packaging documentation](https://developers.openai.com/plugins/build/plugins). **Correction for HE:** old broad claims that Codex has no native hooks are **version-dependent and currently stale**. The ECC PR #20 research appropriately stops at source code/test existence and calls for real-host observation.
2. **Codex plugin deployment differs by surface**: OpenAI's documentation says enabled plugin hooks may work in Codex / ChatGPT Work execution, but hook scripts must exist in that execution environment; a web-only plugin installation does not deploy executable scripts, and nonmanaged hook definitions require trust. This directly strengthens the HE-001 declared → shipped → fixture-tested → observed-in-host distinction. [Official plugin documentation](https://developers.openai.com/plugins/build/plugins).
3. **Mastra legacy workflow syntax is a dated demonstration**, not a current example. First-party [vNext announcement dated 2025-04-29](https://mastra.ai/blog/vNext-workflows) retires `.step()`, removes `.after()`, introduces `.then()` and `.branch()`, and says **each truthy branch can execute in parallel**. PR #19 already records this issue. **Do not** claim exclusive branch selection based on a diagram or screenshot.
4. **Licenses are per package/feature, not repository-wide**: Mastra's [`LICENSE.md`](https://github.com/mastra-ai/mastra/blob/6338514d87a00247ad0c71dabdf7303d0151ca71/LICENSE.md) separates Apache-2.0 material from enterprise `ee/`-scoped [licensing](https://github.com/mastra-ai/mastra/blob/6338514d87a00247ad0c71dabdf7303d0151ca71/ee/LICENSE). PR #19's adversarial correction resolves the prior overly broad inference.

**Scope limit:** These are selected high-risk **source spot checks**, not a claim that all 104 main files, all citations, all papers, or all upstream dependencies have been current-version verified. Current documentation can change after the recorded audit date.

## Completed mechanical corpus scan — 104/104 tracked blobs on main

Read **every tracked blob** from main using GitHub's file interface and searched for *nonportable, session-scoped citation markers* (e.g. `filecite...`, `cite...`, `turnNsearchM`) plus obvious malformed escaped Markdown. This is a **text-pattern integrity scan**, not a successful link crawl, independently authenticated citation resolution or full semantic/current-source audit.

**Concrete defect: four mainline research documents still contain transient citations:**

| Mainline file | Citation-marker occurrences in scan | Examples and consequence |
| --- | ---: | --- |
| `01 Research/Austin Marchese — Loop Engineering & Skill Mining — 2026-09-01.md` | **8** | Temporary web citation markers accompany Exa and Firecrawl capabilities/prices; product behavior/pricing is time-sensitive and **must be reverified**, not silently copied |
| `01 Research/Sources/Agent Harness & Workflow Repos — 2026-08-30.md` | **24** | Session-scoped `filecite` and search references for multiple external projects (e.g. DeepSeek Harness, Archify, OmniRoute); inaccessible source identity undermines later verification |
| `01 Research/Sources/Anthropic AI-Native SDLC & Molten OS — 2026-09-01.md` | **8** | `filecite` references for Molten OS claims are not portable verification links |
| `01 Research/Sources/PMB Dream — Retrospective Memory Consolidation — 2026-09-01.md` | **14** | Session-scoped search markers support Anthropic Dreams and memory API claims; since these concern preview/active services, they need exact current documentation, release, and tested-usage boundaries |

**Total: 54 marker occurrences across 4 files.** Raw occurrence counts are not independently verified individual claims; some lines contain multiple markers. The inspected 104-file tree included Obsidian configuration and Markdown, not all 104 are source-note documents.

**Required correction:** retain text as historical claims marked **SOURCE NOT REVALIDATED** until each marker is replaced by a stable official document/repository URL, pinned version/section when possible, or an explicit `UNVERIFIED` tag. **Never** replace a citation with a plausible but unchecked source. Fix price/API statements only after checking current provider documentation. The four files should be changed through a separately reviewable PR or a source-owner-approved correction in this audit branch; no automated regex deletion of evidence is safe.

Other mechanical findings: the scan did **not** observe literal escaped backtick syntax or literal `\\n\\n` formatting errors in the sampled pattern across mainline blobs. This does **not** mean all Markdown renders correctly. Internal and external link reachability, anchor correctness, GitHub source path existence, fence balancing and citation-target correctness were **not** exhaustively executed.

## Evidence debt and failure modes

| Finding | Severity to process | Observed support | Response / owner |
| --- | --- | --- | --- |
| **Eight source-note PRs remained unmerged for weeks** and overlapped research that has since landed on main | High | Eight source-only PR file lists, source overviews; current main topics | Deliberate per-source admission and de-duplication; HE research owner |
| **No single freshness ledger** distinguishing historical observations from live API/host claims | High | Dated source notes and newer contradictory Codex hook documentation | Add a small derived ledger *only if it has a maintainer/consumer*, no more permanent context |
| **HE-001 lengthy yet incomplete** | High | Current document status, scope/deliverables | Prioritize falsifiable core capability/source map; do not use more research to avoid the baseline |
| **Moving product surfaces** (Codex lifecycle hooks/plugins, Mastra workflow API, enterprise licensing) | High | Current first-party documentation above | Pin revisions, host/version/feature and test; add explicit revalidation triggers |
| **Unportable citations and ambiguous evidence** appeared in older research drafts | Medium | Prior review corrected transient file/session citations in #6/#7/#12 | Scan citation syntax, URLs and file existence; missing source => unverified, not silently erase claim |
| **Large overlapping comparative source bundles** | Medium | #12 and #13 intersect with later corpus | Preserve unique source evidence while refusing duplicate doctrine |
| **Potential unsupported research conclusions** | Unknown until sampled | No source-by-source full verification in this audit | Maintain UNKNOWN, predefine falsifiers and test real experiments |

## Full audit to run — staged with explicit gates

### Phase A — Corpus and branch inventory (read-only, complete)

Inventory all tracked mainline documents **and** each open PR's proposed file changes. For each artifact record: canonical path/owner, type (source note/synthesis/assessment/decision), publication scope, source URLs, pinned SHA/tag/date, last actual verification, any successor, and whether it is operative or historical. Output **counts and a machine-readable list with missing metadata**, not an inflated quality score. Verify no hidden private system details are exposed in public files.

### Phase B — Deterministic integrity (complete)

Test Markdown relative links, percent-encoding/case/path, heading anchors when material, duplicate identities, broken GitHub source paths, transient `turnNsearchM` / `filecite` / session-only citations, malformed code fences, accidental escaped Markdown, generated-artifact pointers, and file/codepath names that no longer exist. **Do not interpret a reachable URL as proof of the claim**. Only automate checks whose error/false-positive behavior is independently tested; a small script is preferable to a new HE CI ecosystem unless repeat failures justify it.

### Phase C — Claim-by-claim external freshness (risk-prioritized)

Prioritize **HIGH**: safety and policy enforcement, host hook/plugin/skill support, current installation instructions, authentication/authority boundaries, data retention/privacy, license, side effects, failed evaluations, and vendor API changes. **MEDIUM**: ongoing benchmark, release- and cost-related comparisons, performance claims, model/tool compatibilities. **LOW/HISTORICAL**: pinned past interviews and papers whose purpose is a dated comparison. For each sampled claim record *source citation + checked date + revision/scope + observed contradiction + disposition*. A literature paper does **not** become obsolete merely because a newer paper exists; compare methods, population, limitations and replication.

### Phase D — Independent contradiction / ablation pass

Generate concrete counterexamples against HE's own prescriptions: typed output without correct effect; reviewer PASS with broken verification; missing telemetry falsely green; Codex hook in repo but disabled/untrusted in host; preserved old policy shadowing newer canonical owner; stale derived retrieval; source cross-citation counted as independent corroboration. Check HE-001/HE-002 dependency and public/private publication boundary.

### Phase E — Canonicalize *after* evidence

Use `PRESERVE`, `SHARPEN OWNER`, `SUPERSEDE WITH REDIRECT`, `CONSOLIDATE`, `PARK`, `REJECT`; never destroy historical provenance. Only apply updates to existing owning documents; preserve original source notes and record deltas. Non-evaluable claims stay **UNVERIFIED**. Do not merge all older branches simply because they are now rebased.

### Phase F — Sustain freshness with minimal overhead

Revalidate on **source release/adapter change**, real host failure, conflicting verified evidence, security/licensing change, or pending implementation decision. Routine periodic recheck may help *active operational statements*, but shouldn't force full recrawls of historical notes. Prefer a small audit trail, citations and few durable ownership rules—not a new status dashboard or continuously-running agent.

## Concrete acceptance tests

1. **Historical versus live:** a 2025 workflow example remains recorded with its original API, yet use-in-2026 must flag obsolete implementation.
2. **Claim versus host truth:** a plugin contains a hook but real Codex refuses to trust/install it; status must stay unsupported/unverified, not green.
3. **Silent loss:** a citation is nonportable or its repository path 404s; tool cannot mark it validated.
4. **False green:** an intentionally disabled telemetry writer causes `UNMEASURED`, and a known invocation reveals recorder failure rather than quiet usage.
5. **Conflicting authority:** historical source says something now contradicted by a pinned owner; user gets current binding authority and dated provenance.
6. **Research independence:** same presenter/repo derivative repeated across three notes counts as **one lineage** unless independent measurement exists.
7. **No operational bleed:** public research contains neither workplace-private examples nor raw confidential agent logs.
8. **Cost discipline:** audit automation must detect real recurring errors before being installed as always-on hook/CI; otherwise stay manual and bounded.

## Open limitations and next decision

**Completed in this audit:** GitHub corpus/open-PR enumeration; initial older-PR owner mapping and disposition; ten mainline priority documents sampled for thematic ownership; HE-001/HE-002 status verified; targeted current primary-doc checks around Codex hook/plugin behavior, Mastra API and licensing.

**Not completed:** exhaustive external validation of every dated repository and paper; machine-executed link/lint pass of all main/PR files; approved local PMB/ACR/Work-MB integration tests; measured representative HE experiment; human merge/architecture authorization.

**Recommendation:** proceed with full Phase A/B across HE and risk-prioritized Phase C/D. Do not promote candidate adoption solely from this document. The audit's best early win is one auditable owner/state per claim—not more rules or more agents.
