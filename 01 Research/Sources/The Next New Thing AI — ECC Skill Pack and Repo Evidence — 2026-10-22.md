# The Next New Thing AI — ECC Skill Pack and Repo Evidence — 2026-10-22

## Scope and provenance

- **User-supplied recording:** `ScreenRecording_10-22-2026 06-24-43_1(1).mp4`, 88.08 seconds, 512 × 1112, H.264/AAC, 63,858,890 bytes.
- **SHA-256:** `47056fac31439c8925aa3482c922edca8c9d09df2426da14053f8b61131b6a7e`.
- **Source shown in the recording:** an @TheNextNewThingAI video/Short discussing **ECC / Everything Claude Code**, displaying the GitHub project `affaan-m/ECC`, a VS Code agent coding demonstration, and ECC's “Start Using ECC” instructions. The recording's original publication URL/date and full spoken transcript are **not verified**. Visual frame sampling supports the observations below; no audio transcription was performed.
- **Repository inspected:** [affaan-m/ECC](https://github.com/affaan-m/ECC), `main` commit [`90606c8315ee09cafc303515c5f33024db6b9882`](https://github.com/affaan-m/ECC/commit/90606c8315ee09cafc303515c5f33024db6b9882), committed 2026-10-20 UTC.
- **Review performed:** 2026-10-22. Official GitHub source files and repository structure read through the connected GitHub repository interface. **No ECC installation, local test execution, cross-host integration run, or deployed security audit was performed.**
- **Reuse:** root [LICENSE](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/LICENSE) is MIT. Individual skills may carry additional file-level license metadata; check exact files and attribution if adapting text or code. Prefer mining principles over copying modules.

## What the video shows (not proof of operational quality)

| Approx. timestamp | Visual observation | Limit |
| --- | --- | --- |
| 5–11 s | GitHub card for `affaan-m/ECC`; visible on-screen narration about a feature built end-to-end | The card/star count and narrative are discovery signals, not independent benchmarks |
| 12–27 s | VS Code coding demonstrations with a creator video inset; terminal/status visible | Cannot establish test correctness, effect verification or reproducibility from short frames |
| 29–35 s | ECC repository banner: “A whole engineering process, bolted onto your AI coder” | Product framing; actual components must be checked in source |
| 40–66 s | “Start Using ECC” table for tasks such as brainstorming, feature work, review, repair, docs and sessions; speaker discussion | Shows workflow exposure, not enforcement proof |
| 68–88 s | Closing discussion | No measurable before/after result shown |

The recording is about **ECC, not Mastra** (a different video reviewed separately). Do not merge the two evidence trails.

## Repository footprint and actual mechanisms

The checked repository contains **311 `skills/` child entries and 71 `agents/` entries** at the inspected commit. The README markets 97 compatibility command shims; its dynamically changing counters and marketing claims are **not** independent quality measurements. The repository includes `hooks/`, `scripts/`, `manifests/`, `schemas/`, `tests/`, `docs/`, adapter directories and multiple install paths. Its current plugin declares version **2.2.3** in [`.codex-plugin/plugin.json`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/.codex-plugin/plugin.json).

### A. Selective installation with ownership and change receipts — implementation exists, not yet universal parity

Direct source: [`docs/SELECTIVE-INSTALL-ARCHITECTURE.md`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/docs/SELECTIVE-INSTALL-ARCHITECTURE.md), [`scripts/install-plan.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/install-plan.js), [`scripts/install-apply.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/install-apply.js), [`scripts/lib/install-state.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/install-state.js), [`manifests/install-profiles.json`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/manifests/install-profiles.json).

- Source-owned install manifests/profiles and request normalization; dependency/target resolution; read-only operation planning; installation-state records; doctor, repair and uninstall paths.
- `install-plan.js` explicitly warns that some target plans remain *scaffold-level* and are not byte-for-byte identical to legacy `install.sh` copies; the architecture note says some target-specific merge/remove semantics remain scaffold-level.
- Profiles separate a **minimal, hook-free** baseline from opt-in hooks and broader stacks. Avoid stacking plugin and full manual installation: official README warns this can duplicate hooks.
- **Mined mechanism:** exact requested → resolved → deployed → current state, with ownership and ability to repair/remove only managed artifacts. Not proof of safe unattended distribution on every host.

### B. Cross-harness compliance records — and a confirmed upstream freshness contradiction

Direct source: [`scripts/lib/harness-adapter-compliance.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/harness-adapter-compliance.js), [`docs/architecture/harness-adapter-compliance.md`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/docs/architecture/harness-adapter-compliance.md), [`hooks/codex-hooks.json`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/hooks/codex-hooks.json), [`tests/codex-native-hooks.test.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/tests/codex-native-hooks.test.js), [`.codex-plugin/plugin.json`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/.codex-plugin/plugin.json).

- Compliance states: **Native**, **Adapter-backed**, **Instruction-backed**, **Reference-only**.
- Records include supported assets, unsupported surfaces, installation path, verification commands, risk notes, owner, source documents and `last_verified_at`.
- **Evidence of drift:** the generated/public matrix labels Codex **Instruction-backed** and the underlying Codex entry says native hook enforcement is unavailable, with `last_verified_at: 2026-05-12`. Current code now ships a native **Codex SessionStart** hook referenced by `.codex-plugin/plugin.json`, with a dedicated two-case test for present/missing `PLUGIN_ROOT`. These two representations disagree on current support.
- **Do not overcorrect the other way:** the presence of hook JSON and a source test does **not** verify actual behavior across Codex versions, policy settings, compaction or real installations. The code-level correction is narrower: the old classification is not an adequate current-state statement.
- **Mined mechanism:** support claims must distinguish declared/shipped/fixture-tested/observed-in-host and must have freshness/retest triggers. Generated documentation is insufficient if the source record itself is stale.

### C. Skill provenance, outcomes and health — useful *unknown* semantics

Direct source: [`scripts/lib/skill-evolution/provenance.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/skill-evolution/provenance.js), [`tracker.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/skill-evolution/tracker.js), [`health.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/skill-evolution/health.js), [`scripts/skills-health.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/skills-health.js).

- Learned/imported skills must have provenance with source, author, creation timestamp and confidence. The value is descriptive, not inherently verified authority.
- Skill-run tracking distinguishes `success`, `failure`, `partial` and human feedback `accepted`, `corrected`, `rejected`; records are capped at 5,000 in a local JSONL sink and use owner-only file permissions. A missing event may reflect a quiet skill **or a failed recorder**; the observed record alone cannot distinguish them.
- **Concrete false-green correction:** `summarizeHealthReport` explicitly classifies no 30-day runs as **unmeasured**, not healthy. Its code comments cite issue **#2463**, when a missing telemetry writer produced a green dashboard across every skill.
- **Mined mechanism:** an observed result, human correction and absence of telemetry are distinct states; provenance and measurement coverage are prerequisites to health claims. Confidence scores are not empirically calibrated probabilities without a separate calibration study.
- **Limit:** a self-reported success is not an independent acceptance oracle; telemetry is local, sampling may be biased, and a missing event pipeline requires negative tests.

### D. Evaluation capsules and receipts — substantial code, intentionally disabled runner

Direct source: [`scripts/lib/eval-harness/capsule.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/capsule.js), [`receipt.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/receipt.js), [`gate.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/gate.js), [`scripts/eval-harness.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/eval-harness.js).

- A capsule is a local append-only **SHA-256 hash-linked journal** with typed lineages and a reproducible projection. It detects tamper/reordering/truncation within the local journal; the source explicitly notes that replacing an entire log can defeat this without external witnessing.
- Offline-verifiable receipts can bind a run/capsule, journal digest, artifact digest and a gate-receipt digest. The module explicitly warns **signatures attest to who vouched for bytes, not that the run was correct**.
- **Crucial boundary:** the CLI `gate run` calls `requireSupportedIsolation()`; [`gate.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/eval-harness/gate.js) **always throws `gate.isolation_required`** because no verified OS containment backend is implemented. The source correctly rejects fake isolation from a copied directory or caller-supplied flag.
- **Mined mechanism:** preserve execution/evaluation proof separately from unverified run claims and fail clearly when a required containment primitive is unavailable.
- **Not supported:** saying ECC currently provides a working secure autonomous evaluation execution platform on this code path.

### E. Retrieval is implemented, but for *skill metadata*

Direct source: [`scripts/lib/context-retrieval.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/lib/context-retrieval.js), [`skills/iterative-retrieval/SKILL.md`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/skills/iterative-retrieval/SKILL.md).

- In-process lexical BM25F-style ranking of registry fields plus hashed character n-gram similarity, fused with reciprocal rank. No model, network or embedding weights are required for this routine.
- Explicit exact-name anchors, query-coverage floor, deterministic scoring and bounded returned descriptions; source says n-grams do not solve genuine synonym understanding.
- The four-phase iterative-retrieval skill teaches **dispatch → evaluate → refine → loop** with a three-cycle recommendation.
- **Mined mechanism:** lightweight discovery is possible without installing a vector database. **Limit:** this is a ranking system for a *skill catalog*, not evidence that body-text PMB archives or live policy are retrievable with the same correctness.

### F. Continuous learning is opt-in, scope-aware, but not fit for policy authority

Direct source: [`skills/continuous-learning-v2/SKILL.md`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/skills/continuous-learning-v2/SKILL.md), [`instinct-cli.py`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/skills/continuous-learning-v2/scripts/instinct-cli.py), [`scripts/hooks/pre-compact.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/hooks/pre-compact.js).

- `continuous-learning-v2` uses observation hooks, an observer, project-scoped instincts with scores and opt-in generation of evolved skills/commands/agents.
- Its default observer configuration is `enabled: false`. The documented native Windows Git Bash/MSYS2 background-observer process **may report successful startup yet fail to survive** the hook's Job Object; WSL2/Linux/macOS are preferred. No observer runtime was tested in this review.
- Promotion candidates are identified when the **same instinct ID**, deduplicated per project, exists in two or more projects and mean confidence reaches 0.8; this does not establish that its content has consistent meaning or valid cross-project scope. **Important nuance:** normal `promote` execution is an explicit CLI action with an interactive y/N confirmation; `--force` can skip it. This is not proof that project-to-global promotion is safe, reviewed for meaning or appropriate for an organizational standard.
- `pre-compact.js` generates an LLM summary, with path/worktree matching to avoid selecting the most recent *foreign* session. It also writes summaries to session temporary state. This is not stronger than the owning project's current canonical memory and may carry summary loss/authority confusion.
- **Disposition candidate:** mine project isolation and opt-in/visibility ideas; **reject automatic admission** of learned habits into binding rules or global team policy.

### G. Independent review is a separate, privacy-relevant execution surface

Direct source: [`docs/ASTRA-REVIEW-GUIDE.md`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/docs/ASTRA-REVIEW-GUIDE.md), [`scripts/astra-review.js`](https://github.com/affaan-m/ECC/blob/90606c8315ee09cafc303515c5f33024db6b9882/scripts/astra-review.js).

- ECC offers a fresh-context **cross-provider** code reviewer via Codex CLI and a read-only reviewer sandbox. The command requires explicit consent to send code to a separate provider and supports a dry-run preview of the payload.
- Source notes acknowledge that review `PASS` and successful fix invocation are not equivalent to independently verified correctness.
- **Limit:** no measured comparative defect-discovery results in the inspected sample. This is **not a reason to replace ACR's existing independent review boundary** or to send restricted corporate code to an unapproved provider.

## Confidence and limitations

- **VIDEO-OBSERVED:** ECC overview/card, IDE sample, start-guide table; NOT a measured product test.
- **REPOSITORY-VERIFIED:** exact listed source files and explicit behavior visible in code; checklists, specs and comments are evidence of *intent*, tests are source-level evidence, not executed results.
- **REPORTED:** project adoption/productivity claims, package counts in moving README, claimed tool success.
- **NOT VERIFIED:** actual install/repair safety across supported OS/harnesses, full security posture, benchmark uplift, live agent behavior, test-suite pass, latency, production Codex hook equivalence, integrity of downstream unpinned npm packages.

## Relevant HE ownership

This is a **source note**, not an instruction to merge, install ECC, copy the 311 skills, modify PMB/ACR/Work-MB or promote governance policy. Its bounded assessment and implementation candidates are in:

- `01 Research/ECC — Selective Skill Delivery, Unknown Health & Evidence Boundaries — 2026-10-22.md`

HE already has adjacent research in `Skill Routing, Gate-Bound Evidence & Orthogonal Review`, `Experience-Derived Harness Evolution`, `Harness Portability, Exit Cost & Inspectability`, and `Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation`. Reuse their authority and evaluation principles; don't duplicate them.
