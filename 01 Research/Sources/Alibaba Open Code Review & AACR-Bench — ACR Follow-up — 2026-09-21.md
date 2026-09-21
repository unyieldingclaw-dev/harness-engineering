# Alibaba Open Code Review & AACR-Bench — ACR Follow-up — 2026-09-21

## Purpose

Deepen the September 19 `Brainstorm Video — ACR Deep Evidence Pass` without rewriting its original evidence snapshot.

This follow-up focuses on two things discovered by going further below the video/README level:

1. current Alibaba Open Code Review mechanisms that materially differ from ACR's current implementation;
2. Alibaba's separate `AACR-Bench` repository as a possible external evaluation corpus for ACR.

This is research evidence only. It does **not** authorize ACR changes.

## Source identity / preservation

- Original user source: `Brainstorm_Video.txt`
- Video: The Next New Thing — `Top Repos Explained: Code Review, World Camera, Better AI Builds, etc.`
- `alibaba/open-code-review` inspected at `01cf7ff8b94c5087205eaf47a6e67f94dabb2a32`
- `alibaba/aacr-bench` inspected at `68a569759289a83654a59d06db2a72910edf0a4a`
- ACR comparison point: `unyieldingclaw-dev/ai-code-review-agent`, main tree `b34340bd50975f21668d806b094a6dcbda7a8540`

Earlier source note remains authoritative for the September 19 snapshot:

- `01 Research/Sources/Brainstorm Video — ACR Deep Evidence Pass — 2026-09-19.md`

---

# 1. Biggest new finding: AACR-Bench is directly useful to ACR

Alibaba publishes a separate code-review benchmark rather than evaluating only on synthetic defects or its own repository.

Current inspected dataset characteristics:

- 200 real pull requests;
- 50 active open-source projects;
- 10 programming languages;
- complete repository context retained;
- 2,145 review comments in the published data overview;
- human-expert + LLM-assisted annotation with three rounds of cross-validation;
- evaluation across line positioning, issue classification, and diff/file/repository context levels.

The benchmark's core metrics include:

- precision;
- recall;
- line-level precision;
- line-level recall;
- noise / unmatched rate.

This is unusually well aligned with ACR's known problem space.

## ACR comparison

ACR already has a strong internal calibration framework. `calibration/calibrate.ts` contains positive and negative fixtures, clean cases aimed at hallucination detection, deterministic-tool fixtures, and explicit regression cases for removed-code / pre-image mistakes.

That internal suite is valuable because it is:

- deterministic enough to reproduce locally;
- targeted at known ACR failure modes;
- inexpensive enough to run repeatedly during development.

But it is still primarily **ACR-authored synthetic/regression evidence**.

AACR-Bench provides something different:

> external, real-PR, repository-level evaluation with human-annotated review findings.

The two should be complementary, not substitutes.

## Candidate ACR experiment

Do **not** start by running all 200 PRs.

Start with a small stratified slice that deliberately covers:

- several languages ACR supports well locally;
- diff-level issues;
- file-level issues;
- repository-context issues;
- clean/noise-sensitive examples;
- line-positioning cases.

Measure at minimum:

- precision / false-positive rate;
- recall;
- line precision and line recall;
- noise rate;
- wall-clock time;
- model/runtime used;
- timeout / incomplete-review rate;
- whether context level changes the result.

Preserve ACR's local calibration fixtures as the fast regression suite; use AACR-Bench as an external validation layer.

### Risks / cautions

- ACR output will need an adapter into the benchmark's expected finding format.
- ACR's evidence-basis semantics (`VERIFIED` / `INFERRED` / `SPECULATIVE`) do not map one-to-one to Alibaba's benchmark labels; do not erase that distinction to fit the benchmark.
- Benchmark-specific tuning can overfit. Do not put benchmark examples into reviewer prompts.
- Record exact ACR/model/runtime versions for every run.
- Alibaba's product optimizes strongly for precision and accepts lower recall; ACR should not inherit that objective automatically.

**Disposition: ASSESS — high priority.**

---

# 2. Open Code Review makes review coverage an explicit deterministic artifact

Alibaba's current architecture separates review scope from reviewer reasoning.

Before the model reviews anything, deterministic code decides:

- which changed files are reviewable;
- which are excluded and why;
- whether a file is unsupported, binary, deleted or too large;
- which review rules apply;
- whether the resulting group fits the context budget.

`ocr review --preview` exposes this scope before spending model tokens.

## ACR implication

ACR already exposes increasingly good incompleteness metadata and supports ignore/policy/chunk behavior. The useful question is whether a user can answer, deterministically and before/after a run:

> "Exactly what code was eligible, what was actually reviewed, what was skipped, and why?"

This is more important than copying Alibaba's particular filters.

**REINFORCE:** review coverage should be observable, not inferred from a successful exit code.

**ASSESS:** whether ACR's current JSON/report surfaces make that coverage as explicit as the underlying implementation already does.

---

# 3. Semantic grouping is narrower and more useful than generic multi-agent fan-out

Alibaba's `internal/agent/grouping.go` groups related changed files before dispatch.

Key properties of the current implementation:

- the grouping call sees file metadata rather than full diffs;
- small changes skip the grouping call entirely;
- missing files are deterministically restored as single-file groups;
- invalid/duplicate model assignments do not lose coverage;
- maximum group size is bounded;
- context/token budget is enforced after grouping;
- grouping failure falls back to one-file-per-group review;
- grouping usage and decisions are observable.

The point is not more agents. The point is preserving **related context** while bounding each review unit.

## ACR implication

ACR's current oversized-diff chunk path is file-boundary safe, but group membership is mainly driven by order/size rather than semantic relationship.

A controlled experiment is justified only for oversized, multi-file changes where related-file separation plausibly harms review quality.

Do not add a routing model to ordinary reviews that already fit in context.

**Disposition: ASSESS.**

---

# 4. Alibaba actively repairs / filters review comments; ACR currently annotates

This is the sharpest architectural contrast.

Alibaba's comment pipeline includes:

1. deterministic line resolution from quoted `existing_code`;
2. an optional model-based re-location step when deterministic anchoring fails;
3. a post-review `REVIEW_FILTER_TASK` that removes comments judged provably incorrect;
4. another line-resolution pass before rendering.

The post-processing can run asynchronously so it does not block the main review loop.

## ACR comparison

ACR currently takes the safer report-only posture:

- `evidenceLocation.ts` deterministically stamps location as `verified`, `mismatch`, or `unknown` but deliberately does not rewrite the location or drop the finding;
- `evidenceVerifier.ts` independently checks claim vs cited evidence, but `NOT_SUPPORTED` is currently report metadata rather than an automatic publication filter;
- both choices were made because false-negative suppression is considered more dangerous than visibly imperfect metadata.

That means Alibaba does **not** prove ACR should start deleting findings. It does prove there is a concrete alternative worth measuring.

## Candidate experiment

Replay labeled clean + dirty fixtures under hypothetical policies without changing production behavior:

- baseline publication;
- annotate-only evidence verifier;
- demote `NOT_SUPPORTED` findings;
- suppress `NOT_SUPPORTED` findings;
- optional re-location only when the deterministic location check is `mismatch` and the new anchor is independently confirmable.

Measure:

- false-positive reduction;
- true-positive loss;
- severity changes;
- line-attribution improvement;
- verifier disagreements with deterministic evidence;
- unavailable/timeouts;
- latency.

A model verifier failure or uncertainty must never silently clear a blocker.

**Disposition: ASSESS — high semantic risk.**

---

# 5. Path-scoped review rules are a possible progressive-disclosure mechanism

Alibaba's `.opencodereview/rule.json` demonstrates path-scoped rules that can require, for a specific file family:

- style consistency;
- documentation synchronization;
- targeted test coverage;
- protocol-specific regression guards.

The useful mechanism is not the exact schema. It is that project-specific review guidance can be loaded **only when the changed path makes it relevant**.

## ACR implication

ACR already has:

- specialist agents;
- profile selection;
- policy filtering;
- PMB/static/semantic project context;
- deterministic analyzers.

Do not add another rules engine merely because Alibaba has one.

Assess this only if measurement shows irrelevant project guidance is consuming context or degrading signal.

**Disposition: PARK pending evidence.**

---

# 6. Adaptive review effort and repeated passes

Alibaba's current architecture conditionally adds a planning step for larger changes and uses review effort to select one, two or three review rounds.

A subtle implementation choice matters more than the numeric thresholds:

> later review rounds receive already-confirmed findings but do **not** inherit the first-round plan, because the plan can become a coverage ceiling.

This is relevant to ACR's independent-review philosophy.

If ACR ever adds repeated passes, the next pass should have a changed objective / fresh search space, not simply "repeat the same review harder."

Possible examples:

- first pass = defect discovery;
- second pass = challenge uncovered assumptions / missed changed behavior;
- opposition pass = attack load-bearing findings or absences.

Do not copy Alibaba's line thresholds or round counts.

**Disposition: REINFORCE independent/adversarial review; ASSESS only if a repeated-pass experiment is proposed.**

---

# 7. Deterministic harness and model reviewer can be decoupled

Alibaba's delegation mode lets Open Code Review retain deterministic file selection and rule resolution while another host agent/model performs the semantic review.

This is a useful architectural seam:

```text
review scope / rules / evidence contracts
        !=
review model / provider
```

ACR already has provider interfaces and local-model configuration, so no new abstraction is justified from this alone.

**REINFORCE:** keep review harness contracts separable from a specific model.

**REJECT:** building a provider/orchestration platform merely to imitate delegation mode.

---

# 8. Security: model output is also untrusted input

Alibaba's `ASSURANCE_CASE.md` explicitly models several trust boundaries:

- repository/diff content may be adversarial;
- the LLM provider/response is semi-trusted;
- model-suggested file paths are validated;
- local viewer/browser exposure has its own boundary;
- response structure and line bounds are validated before downstream use.

ACR already implements several equivalent or stronger protections, including prompt-injection sanitation and re-sanitizing generated finding text before a second verifier model consumes it.

The durable lesson is:

> sanitizing the original diff is not enough if model-generated output later crosses another tool/model/security boundary.

**Disposition: REINFORCE.**

---

# 9. What this changes from the September 19 note

The September 19 note's three leading experiments remain valid:

1. semantic grouping for oversized diffs;
2. bounded second-stage evidence retrieval;
3. measured publication influence for independently unsupported findings.

This deeper pass adds one item **ahead of them as an evaluation foundation**:

> **AACR-Bench as an external real-PR benchmark complementing ACR's internal fixtures.**

Why this goes first conceptually: the other architecture experiments are much easier to judge if ACR has an external evaluation surface that measures precision, recall, line accuracy, noise, and context level.

This does **not** mean benchmark integration should block ongoing ACR bug fixes or current calibration work.

---

# ACR handoff candidate

ACR should inspect these sources itself before implementation decisions:

- `https://github.com/alibaba/open-code-review` at `01cf7ff8b94c5087205eaf47a6e67f94dabb2a32`
- `https://github.com/alibaba/aacr-bench` at `68a569759289a83654a59d06db2a72910edf0a4a`

Specific current ACR files to compare:

- `src/core/evidenceLocation.ts`
- `src/core/evidenceVerifier.ts`
- `src/core/chunkRunner.ts`
- `src/core/diffSplit.ts`
- `src/core/policyFilter.ts`
- `src/core/contextLoader.ts`
- `calibration/calibrate.ts`
- `calibration/evidenceVerifierCalibration.ts`
- `calibration/fixtures/`

Requested output from that ACR-native review should be:

- overlap already implemented;
- genuine gaps supported by current source;
- smallest controlled experiments;
- explicit rejection of mechanisms that add complexity without measured benefit;
- no implementation until the comparison is complete.

---

## Overall disposition

- **ASSESS — highest value:** AACR-Bench as an external real-PR evaluation layer.
- **ASSESS:** semantic related-file grouping for oversized diffs.
- **ASSESS:** active re-location / post-review filtering only through labeled replay experiments.
- **ASSESS:** whether bounded second-stage context retrieval resolves genuinely context-dependent findings.
- **REINFORCE:** deterministic scope/coverage, model-independent harness contracts, adversarial review, trust-boundary validation.
- **PARK:** path-scoped project rules until context-noise evidence exists.
- **REJECT:** copying Alibaba wholesale, copying its thresholds, making a model reflection pass authoritative by default, or optimizing ACR solely for Alibaba's precision-first tradeoff.
