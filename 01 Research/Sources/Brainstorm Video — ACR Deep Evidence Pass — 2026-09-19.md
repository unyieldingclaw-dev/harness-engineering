# Brainstorm Video — ACR Deep Evidence Pass — 2026-09-19

## Purpose

Revisit the repository set from **The Next New Thing — "Top Repos Explained: Code Review, World Camera, Better AI Builds, etc."** specifically through the lens of the user's AI Code Review Agent (ACR), rather than the original PMB-oriented mining pass.

The earlier pass correctly captured general review and harness principles but did not go deep enough on code-review mechanisms. This pass compares source implementations against the actual current ACR codebase and records only mechanisms that appear genuinely additive or worth controlled evaluation.

This is research evidence, not authorization to modify ACR.

## Source set

Primary video/transcript supplied by the user:

- `Brainstorm_Video.txt`
- Video: https://www.youtube.com/watch?v=1fHsIveXRa8&t=268s

Repositories examined or cross-referenced:

| Repository | Revision used | ACR-specific signal |
|---|---|---|
| `alibaba/open-code-review` | `a003b9341a65130b024829101ea35494b56569e1` | Strong: semantic grouping, bounded context tools, comment reflection/filter, rule scoping, explicit trust boundaries |
| `affaan-m/ECC` | `b2279eb1d7504eaf363c298eaa9b8210ebb29f77` | Moderate: multi-dimension review + adversarial verification with fail-closed blocker semantics |
| `addyosmani/agent-skills` | `c004a74784a08295d52749b04cda634125b9a581` | Moderate/mostly reinforcing: spec/tests-first review, five axes, specialist fan-out, explicit review output contract |
| `mksglu/context-mode` | `6f0cc6841c687e754059f36714a11233fda1a02b` | Low direct ACR signal; stronger as context-efficiency/telemetry prior art already recorded elsewhere |
| `max-sixty/worktrunk` | `d92b628d02749e83926638f9a73c6e7281e3d1ad` | Low direct ACR signal; isolation/runtime prior art rather than review logic |
| `SuperLogicAI/Logic-Loop` | `bfad04e53342e054a5a43953a63de1a9cf44a539` | Low direct ACR logic signal; useful telemetry/session prior art already mined |
| `sonpham-org/claude-dashboard` | `1f6c8f14332ff7432805e531725fb21cb6c4cadf` | No material ACR review mechanism; dashboard/session-state prior art |
| `Codpal-Limited/deckgauge` | `2228cf308a2d25da348beefe8e7fa50c367d3dcc` | No sufficiently strong unique ACR mechanism established in this pass |
| `trainingsites/campus-ai-os` | `509318d2ecf2ceb7c93c39fb998946ef584f4ae6` | Outcome/report-schema ideas, but no stronger review mechanism than current ACR |
| `CaptainASIC/reckoner` | `8a5d5b0d77f0461abf98e61709cf02d94c63fddb` | Provider/usage adapter prior art, not ACR review logic |

Current ACR comparison point:

- `unyieldingclaw-dev/ai-code-review-agent`
- main at review time: `b34340bd50975f21668d806b094a6dcbda7a8540`

---

# 1. Alibaba Open Code Review — strongest source

The video described Alibaba Open Code Review at a high level: automated review, security findings, CLI/reporting, and possible Git-hook integration. The source implementation is substantially more interesting than that summary.

## 1.1 Semantic review-task grouping

Alibaba does not simply split a large diff by line count or hand every file to every review call.

`internal/agent/grouping.go` builds **semantic groups of related changed files** before review. Important implementation details:

- A group is explicitly modeled as semantically related diffs reviewed in one LLM call.
- Small changes avoid the grouping LLM when the call is unlikely to buy useful information.
- Larger change sets call the LLM using **file metadata, not diff bodies**.
- The grouping response uses compact integer file indices instead of repeating paths.
- Files omitted by the model are automatically restored as their own groups.
- Duplicate/invalid indices are ignored rather than corrupting coverage.
- A hard maximum limits files per group (`maxFilesPerGroup = 10`).
- A token-budget valve splits groups that would exceed review context.
- Any grouping failure falls back to per-file review rather than failing the review.
- Grouping decisions emit telemetry and can be represented in review output.

The architecture documentation gives the reason: a handler, service and associated test can be reviewed in one shared conversation so cross-file behavior is visible without loading every changed file into every review task.

### ACR comparison

ACR's current large-diff `--chunk` implementation is intentionally simpler:

- split only on `diff --git` file boundaries;
- greedily pack sections in original order up to the configured line budget;
- never split a single file section;
- run the existing review pipeline once per chunk;
- merge results afterwards.

That implementation is correct for coverage and has substantial defensive merge logic, but **chunk membership is not semantic**. Related files can land in separate review calls solely because of ordering/size.

For normal, non-chunked reviews this is not a gap: ACR already supplies the full reviewed diff. The potential gap exists specifically when the diff is large enough to require chunking/truncation.

### Candidate experiment

Compare current file-boundary greedy chunking against a **semantic grouping experiment** only on oversized multi-file diffs.

Required metrics:

- dirty-fixture / known-defect recall, especially cross-file defects;
- clean false-positive rate;
- duplicate findings across groups;
- context/input size;
- latency;
- grouping failure/fallback rate;
- file coverage completeness;
- whether semantically related implementation + test/config files actually remain together more often.

Do not adopt semantic grouping merely because it sounds smarter. If current grouping does not produce measurable misses, the extra LLM/routing layer is unjustified.

**Disposition: ASSESS.**

---

## 1.2 Bounded on-demand context during review

Alibaba's main review loop exposes a deliberately constrained read-only context toolkit:

- `file_read` — read a bounded range of the post-change file;
- `file_read_diff` — inspect another changed file's diff;
- `file_find` — locate candidate files by name/path;
- `code_search` — bounded repository text search;
- plus separate tools for emitting comments and ending the task.

Important safeguards:

- context tools are read-only;
- context gathered from other files is explicitly **not a new comment target**;
- `file_read` caps returned lines;
- `file_find` and `code_search` cap result volume;
- planning has a smaller read-only tool surface than the main review phase;
- failures are returned as ordinary tool results rather than crashing review;
- context is acquired only when the reviewer needs to resolve a question.

### ACR comparison

ACR's current `BaseAgent.run()` builds one user prompt from prepared `input.context` + the diff and calls `provider.chat(...)` once with structured output. Deterministic tool-backed agents can override that path, but the ordinary specialist reviewer does not have an on-demand repository read/search loop.

ACR already has mature static context construction and deterministic evidence validation, so the useful question is **not** whether to give reviewers broad repository browsing.

The narrower question is whether findings currently downgraded, flagged or fabricated because of missing cross-file evidence could be resolved by a small, read-only, capped evidence-acquisition step.

### Candidate experiment

Do not expose general repository tools to every reviewer first.

Prefer a targeted experiment:

1. identify cases from existing fixtures/history where a finding remains `INFERRED` / `SPECULATIVE`, or evidence verification reports `NOT_SUPPORTED`, specifically because required context is outside the supplied diff/context;
2. give a second-stage adjudicator a bounded context request capability (for example one changed-file diff lookup or symbol/file search);
3. re-evaluate only that finding;
4. measure whether the additional evidence improves classification without increasing unrelated findings.

Metrics:

- number of ambiguous findings resolved;
- true-positive recovery;
- false-positive suppression;
- retrieval count/bytes/tokens;
- latency;
- how often retrieval finds genuinely relevant evidence;
- whether the reviewer starts expanding scope beyond the changed behavior.

**Disposition: ASSESS, bounded second-stage retrieval only.**

---

## 1.3 Post-review reflection / comment filter

Alibaba includes a post-processing stage after review comments are generated. Its architecture describes a `REVIEW_FILTER_TASK` that inspects generated comments against the diff and removes comments that are provably incorrect before rendering.

This is materially different from merely asking the original reviewer to double-check itself. It is a separate filtering boundary after generation.

### ACR comparison

ACR already has stronger mechanisms than a generic reflection pass in several areas:

- deterministic file-existence/path checks;
- deterministic claim-class filters for specific impossible mechanisms;
- pre-image-only evidence filtering;
- cross-agent corroboration and dedup;
- deterministic evidence-location annotation;
- an independent evidence verifier for model-produced findings.

However, the current evidence verifier is intentionally **report-only**. `runEvidenceChecks()` returns metadata about eligible findings whose claim/evidence pair is not supported; it never filters or changes the `findings` array. `runner.ts` returns the synthesized findings unchanged alongside `evidenceCheckFilter` metadata.

Default evidence verification is also severity-gated to High/Critical because lower severities are more numerous and would multiply latency.

Therefore Alibaba exposes a legitimate experimental question for ACR:

> Should a sufficiently strong, independently validated post-review verdict ever affect publication/severity, rather than only annotate the report?

This question is particularly relevant because ACR's adversarial-clean work has measured substantial mismatch/unknown and fabricated-finding behavior. But existing evidence-verifier validation is not enough by itself to authorize automatic suppression.

### Candidate experiment

Use existing clean/dirty fixtures to compare:

A. current ACR publication;
B. current ACR + evidence-check annotation only;
C. hypothetical suppression/demotion when the verifier says `NOT_SUPPORTED`;
D. if available, a cheaper bounded classifier/reflection stage followed by current verifier for disputed findings.

Pre-commit the decision metrics before implementing production behavior:

- clean false-positive reduction;
- dirty true-positive retention / false-negative increase;
- changes by severity;
- rate at which the verifier contradicts a genuinely correct finding;
- unavailable/timeout behavior;
- cost and latency;
- whether deterministic evidence disagrees with the model filter.

Any production suppression policy should preserve a fail-safe path for unavailable/uncertain verification and should never let model uncertainty silently erase a blocker.

**Disposition: ASSESS. Do not make the current verifier authoritative without fixture evidence.**

---

## 1.4 File-scoped rules and specialist scope

Alibaba can bind rules to matching file paths and apply only relevant rules to each review task. Agents can also select/filter relevant files.

ACR already has specialized agents, profiles, policy filtering, deterministic analyzers and agent-specific include/exclude behavior. Recent ACR work has explicitly hardened the visibility of partial agent-policy exclusions.

The remaining potentially useful distinction is not "add specialist agents"; ACR already has many. It is whether **project-specific standards should be retrieved/scoped to the changed files** rather than supplied as generic global review context.

No demonstrated ACR failure currently justifies a new rule engine solely from this source.

**Disposition: PARK unless PMB/ACR measurements show irrelevant project guidance is materially degrading review.**

---

## 1.5 Security/trust-boundary documentation

Alibaba's `ASSURANCE_CASE.md` is useful prior art for documenting the reviewer's own security boundaries:

- repository/diff content is semi-trusted/untrusted input;
- provider output is validated;
- file paths are checked against repository boundaries;
- security claims name the mechanism enforcing them;
- optional local viewers have their own browser/network trust boundary;
- automated verification is separated from architectural/security claims.

ACR already contains explicit sanitizer logic, deterministic sources, output validation, tool availability reporting and evidence checks. The useful lesson is primarily documentation/governance: security claims should point to enforcement rather than prompt intent.

**Disposition: REINFORCE HE deterministic-enforcement and evidence principles.**

---

# 2. ECC — useful overlap, little reason to copy

ECC's `workflows/orch-review.workflow.js` implements a multi-dimension review followed by adversarial verification of Critical/High findings.

Notable mechanics:

- reviewer dimensions run independently and in parallel;
- high/critical findings must contain concrete evidence + proof at the schema boundary;
- security review is conditionally added when the diff/path content triggers it;
- failures of a required review dimension fail closed rather than silently approving incomplete coverage;
- findings are deduplicated before verifier calls so duplicate reports do not waste verification budget;
- Critical/High findings are then independently challenged;
- a blocker is cleared only when the verifier can affirmatively refute it at high confidence;
- uncertainty, verifier failure and unavailable verification do **not** clear a blocker.

### ACR comparison

Most of this is already represented, often more deeply, in current ACR:

- specialist reviewers and conditional/policy-scoped execution;
- deterministic analyzers;
- dedup/corroboration;
- explicit evidence fields;
- independent High/Critical evidence verification;
- agent/run incompleteness metadata;
- fail-open reporting when the verifier itself is unavailable so failure is not mistaken for a negative verdict.

ECC therefore reinforces ACR's direction more than it reveals a missing subsystem.

One nuance worth retaining: **uncertainty must not be treated as refutation**. ACR already distinguishes unavailable verification from `NOT_SUPPORTED`; keep that distinction load-bearing in any future publication filter.

**Disposition: REINFORCE, no direct implementation recommendation.**

---

# 3. Addy Osmani Agent Skills — review contract, not new architecture

The `code-reviewer` persona and `code-review-and-quality` skill reinforce several useful review-contract ideas:

- understand task/spec before judging code;
- inspect tests as evidence of intent;
- review across explicit axes rather than one vague "review this" prompt;
- separate required findings from optional/nits;
- prefer a few high-conviction issues over finding volume;
- record a verification story;
- separate builder and reviewer perspectives where useful;
- treat dependencies/lockfiles as reviewable change surface.

ACR already implements substantially richer specialist review, evidence classification, deterministic analysis and publication filtering. Importing this workflow would mostly duplicate current capability.

Potentially useful evaluation question: whether ACR reports make the **verification story / coverage incompleteness** sufficiently legible to a human, not merely available in JSON metadata. Recent ACR work on `filteredFiles`, `agentsPlanned`, `agentStatus`, tool availability and chunk coverage is already moving in that direction.

**Disposition: REINFORCE, no new ACR subsystem.**

---

# 4. Other Brainstorm repositories — ACR disposition

## Context Mode

Strong context-efficiency and retrieval/analytics prior art, already mined for HE/dashboard work. No unique code-review mechanism surfaced that beats the more direct Alibaba patterns for ACR.

**PARK for ACR.**

## Worktrunk

Worktree/process isolation is useful for running agents safely and concurrently, but it is runtime/orchestration infrastructure rather than a review-quality mechanism.

**PARK for ACR.**

## Logic Loop / Claude Dashboard / Reckoner

Useful sources for session state, usage, telemetry and a future engineering cockpit. They do not materially improve ACR's review reasoning pipeline in this pass.

ACR should expose machine-readable results that a future cockpit can consume; the cockpit should not be embedded in ACR.

**PARK as adjacent observability.**

## Campus AI OS

Structured outcome/report ideas are useful generally, but no stronger finding/evidence contract than ACR's current result envelope was established in this pass.

**PARK for ACR.**

## DeckGauge

No source-level mechanism was established strongly enough in this pass to justify an ACR change. Do not mine from the headline/dashboard alone.

**PARK pending a concrete mechanism.**

---

# 5. What ACR already does better than the external examples

The external mining should not erase the fact that current ACR is already unusually defensive.

Current source includes, among other mechanisms:

- deterministic analyzers alongside model reviewers;
- explicit evidence basis (`VERIFIED` / `INFERRED` / `SPECULATIVE`);
- changed-file membership validation;
- structural rejection of specific unsupported claim classes;
- protection against findings quoting deleted/pre-image code as current behavior;
- cross-agent corroboration;
- same-location dedup with evidence-basis preservation;
- publication filtering;
- evidence-location checking;
- independent High/Critical evidence verification;
- structured incompleteness and partial-coverage metadata;
- chunk merge semantics hardened against misleading whole-run claims;
- mutation-tested regression work around several review-gate defects.

Therefore the research target is not "make ACR more like Alibaba/ECC." It is to isolate mechanisms that solve a measured ACR failure better than the current design.

---

# 6. Recommended ACR evaluation order

## Priority 1 — semantic grouping for oversized diffs

Why first:

- clear implementation contrast;
- narrow scope;
- easy to A/B against existing chunk path;
- directly testable with known cross-file defects;
- no need to redesign finding semantics.

## Priority 2 — bounded second-stage context acquisition

Why second:

- potentially addresses missing-context false positives / unsupported findings;
- aligns with progressive disclosure;
- much higher risk of scope/context explosion than semantic grouping;
- should be tested as adjudication, not as unrestricted reviewer browsing.

## Priority 3 — evidence-verifier influence on publication

Why third:

- directly relevant to false-positive suppression;
- highest semantic risk because incorrect suppression creates false negatives;
- requires labeled clean/dirty evidence before changing publication behavior;
- a cheap System One/Jev-style filter could later be evaluated here, but should not be conflated with Alibaba's architecture.

---

# 7. Candidate prompts for the ACR project

These are investigation prompts, not implementation instructions.

### A. Semantic grouping

> Evaluate whether ACR's oversized-diff review would benefit from semantic review-task grouping before specialized agents. Compare current `splitByFileBoundary` / `runChunked` behavior against the pattern in Alibaba Open Code Review where related implementation, test and config files are grouped using file metadata, with hard group/token limits and deterministic fallback. Do not implement first. Inspect current chunking, orchestration, truncation, policy filtering and calibration fixtures. Determine whether semantic grouping adds information not already available in the current path. If justified, propose the smallest controlled A/B experiment. Measure cross-file dirty-fixture recall, clean false-positive rate, duplicate findings, context/input size, latency, file-coverage completeness and grouping-fallback behavior. Preserve current file-boundary safety and fail-open coverage guarantees.

### B. Bounded evidence retrieval

> Evaluate whether ACR needs a bounded, read-only on-demand context mechanism for adjudicating findings whose truth depends on code outside the supplied diff/context. First prove whether current `contextLoader`, per-file context, project docs and current reviewers already provide equivalent evidence. If a gap exists, do not add unrestricted repo browsing. Design a second-stage experiment in which an adjudicator may request a tightly capped changed-file diff/read or symbol/file search only for an existing ambiguous finding. Measure how often retrieval resolves `INFERRED`/`SPECULATIVE` or `NOT_SUPPORTED` cases correctly, false-positive suppression, true-positive recovery, retrieval/token cost, latency and scope expansion. Retrieved context is evidence only; it must not create unrelated new findings.

### C. Post-review filtering

> Evaluate whether ACR's existing evidence-check results should ever affect finding publication or severity rather than remain report-only. Do not change behavior first. Use existing clean/dirty/adversarial fixtures to compare current publication with a hypothetical policy that demotes or suppresses findings only when an independent verifier returns `NOT_SUPPORTED`. Measure clean false-positive reduction, dirty true-positive retention, false negatives introduced, results by severity/basis, verifier disagreement with deterministic evidence, and unavailable/timeout cases. Unavailable or uncertain verification must never silently clear a blocker. Treat the current verifier's prior synthetic validation as insufficient on its own to authorize production suppression.

---

## Overall disposition

- **ASSESS:** semantic grouping for oversized multi-file ACR reviews.
- **ASSESS:** bounded second-stage evidence retrieval for context-dependent findings.
- **ASSESS:** whether independently unsupported findings can safely influence publication after fixture validation.
- **REINFORCE:** specialist review, adversarial verification, explicit evidence, incomplete-coverage visibility, deterministic enforcement.
- **PARK:** generic dashboard/runtime/context tooling as ACR internals.
- **REJECT:** copying Open Code Review wholesale, adding broad reviewer repo access by default, or turning model reflection into an authoritative gate without measured false-negative behavior.
