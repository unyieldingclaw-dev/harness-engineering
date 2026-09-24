# John Kim — Risk-Directed AI Code Review, Proof & Launch Safety — 2026-09-24

## Purpose

Mine the durable Harness Engineering mechanisms from John Kim's video:

- **Video:** `How I Review AI Code - (Meta Senior Staff Engineer)`
- **Creator:** John Kim
- **YouTube:** `https://www.youtube.com/watch?v=b2QkhmQ0sT0`
- **User-provided transcript/screenshots:** 2026-09-24

This is practitioner evidence, not architectural authority. Preserve the useful mechanisms; do not adopt the video's metaphors, thresholds, or product-specific workflow as HE doctrine.

Related HE research:

- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Sources/Alibaba Open Code Review & AACR-Bench — ACR Follow-up — 2026-09-21.md`

---

# 1. Review depth should follow risk, not uniform diff inspection

Kim frames code as a tree: "trunk" code has broad downstream consequences; "leaf" code is more isolated. The useful abstraction is not the tree metaphor itself. It is that review depth should vary with the **blast radius and reversibility of the change**.

Useful risk dimensions include:

- number and criticality of downstream consumers;
- whether existing production behavior changes;
- data/security/permission sensitivity;
- whether the change crosses shared infrastructure or state boundaries;
- whether the change is actually isolated or merely appears local;
- whether the change is behind a functioning gate;
- whether rollback is cheap and proven;
- quality of task-relevant verification evidence.

A small diff can be high risk. A large generated leaf implementation can be comparatively low risk if it is isolated, strongly verified, and easy to disable.

### Durable implication

> **Review effort should scale with consequence, coupling, reversibility, and evidence quality — not line count alone.**

**Disposition: STRONGLY REINFORCE existing risk-directed verification.**

---

# 2. "Leaf" is a claim that must be established

The video suggests leaf code can move faster when isolated, gated, visually/runtime verified, and cheap to roll back.

The important caution is that `leaf` should not be inferred only from file location or apparent component size. A new endpoint, UI component, config flag, or helper can still alter authentication, shared state, persistent data, network contracts, or privileged behavior.

A faster review path is justified only when isolation/reversibility is **observable**, not assumed.

### Candidate rule

> **Reduced review depth requires evidence that the change is bounded and recoverable.**

**Disposition: REINFORCE.**

---

# 3. The author should package proof near the review boundary

Kim's strongest practical pattern is that the author/authoring agent should not merely submit a diff. It should package task-relevant proof with the change.

Examples from the video include:

- focused logic/integration tests;
- actual runtime path exercised end-to-end;
- screenshot or short video for user-visible behavior;
- explicit statement of what was intentionally left unchanged;
- statement of what was verified and what was not.

This fits HE's existing assertion-vs-evidence and effect-verification work.

The durable mechanism is a compact **proof package**, not a verbose PR narrative.

A useful proof package can include:

```text
why / intended outcome
what changed
what intentionally did not change
focused checks + expected/observed result
runtime path exercised when relevant
visual evidence when relevant
known unverified areas / residual risk
rollback/gate state when consequential
```

Do not require every evidence type on every change. Proof should be selected for the property being claimed.

### Candidate principle

> **A consequential change should arrive at review with the evidence needed to evaluate its claims.**

**Disposition: REINFORCE / candidate wording.**

---

# 4. Self-reported confidence is not proof

Kim asks authoring agents to report confidence. That can be useful as a signal of uncertainty or as routing metadata, but model confidence must not be treated as evidence of correctness.

Prefer:

```text
verified properties
unverified properties
observed failures/unknowns
specific evidence
```

over an unsupported numeric confidence score.

If confidence is retained, its legitimate uses are narrow:

- prompt the author to disclose uncertainty;
- route ambiguous work to human/independent review;
- compare calibration over time if backed by real outcome data.

### Durable implication

> **Confidence may describe uncertainty; it must not substitute for evidence.**

**Disposition: REINFORCE assertion-vs-evidence discipline.**

---

# 5. Deterministic checks should support review, not consume semantic review capacity

The video recommends using type checks, linting, focused tests, and production builds while keeping stylistic/mechanical nits out of the human/semantic review conversation.

This independently reinforces existing HE guidance:

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

Important nuance: deterministic checks remain part of the acceptance evidence even when they are not LLM review findings.

A semantic reviewer should receive or be able to inspect their result when relevant, but should spend reasoning budget on properties that deterministic tools cannot decide reliably.

**Disposition: STRONGLY REINFORCE.**

---

# 6. Independent review should be fresh from implementation rationale, not blind to project truth

Kim argues that the reviewer should not inherit the implementation conversation because prior context biases the reviewer toward the author's reasoning.

That aligns strongly with HE's producer-vs-verifier distinction and ACR's adversarial/opposition direction.

But "fresh context" must not mean context-free review.

The independent reviewer still needs the authoritative inputs required to judge the change:

- current diff/current repository state;
- accepted intent/spec/requirements;
- relevant project policies and invariants;
- deterministic check results/evidence;
- known scope and expected behavior.

What it should **not** inherit by default is the author's conversational justification, hidden assumptions, or self-approval narrative.

### Candidate principle

> **Independent verification should share authoritative task truth, not the producer's implementation bias.**

**Disposition: STRONGLY REINFORCE.**

---

# 7. PR templates are useful when they structure evidence, not ceremony

Kim demonstrates a small PR template covering:

- why;
- smallest useful change;
- intentionally unchanged behavior;
- validation/proof;
- fresh review.

The value is not the exact template. The value is reducing free-form narrative and making expected evidence predictable.

Avoid templates that become checklist theater or duplicate data already owned by CI/Git/tickets.

A field belongs only if a reviewer or gate actually consumes it.

**Disposition: ASSESS per-project; no universal HE template required.**

---

# 8. Automated PR "babysitting" is a bounded-loop problem

The video suggests periodically checking a PR, applying reviewer-requested fixes, rerunning validations, updating the PR, and resubmitting.

The useful workflow exists, but an unbounded reviewer↔fixer loop can:

- oscillate between incompatible suggestions;
- expand scope;
- repeatedly rewrite correct code;
- consume unbounded tokens/time;
- act on ambiguous reviewer comments;
- allow a reviewer to become de facto mutation authority.

If such automation is used, HE's bounded execution rules should apply:

- explicit fix scope/classes;
- authoritative current attempt;
- maximum iterations/time;
- deterministic revalidation after mutation;
- stop/escalate on ambiguity or conflicting findings;
- no autonomous merge/deploy authority merely because checks are green;
- preserve unresolved reviewer findings rather than silently clearing them.

### Durable implication

> **Review/fix automation should be a bounded settlement loop, not perpetual mutual correction.**

**Disposition: REINFORCE bounded execution; PARK implementation until a real workflow requires it.**

---

# 9. Merge-ready and launch-ready are different states

This is one of the strongest additions from the video.

A change can be acceptable to merge while still lacking the evidence/controls required for safe production exposure.

Useful separation:

```text
implementation complete
      ↓ verified
review accepted / merge-ready
      ↓ release controls + launch evidence
launch-ready
      ↓ staged exposure
observed production behavior
```

Launch readiness may require things not necessary for merge acceptance:

- feature gate/kill switch;
- rollback path;
- migration/recovery plan;
- production configuration;
- monitoring/error visibility;
- performance validation;
- security/privacy review;
- staged cohort/canary readiness.

### Candidate principle

> **Merge acceptance does not authorize production exposure.**

**Disposition: STRONGLY REINFORCE artifact-gated lifecycle.**

---

# 10. Rollout can be an evidence-producing stage

Kim recommends treating launches as experiments: gate, start small, watch, then expand or roll back.

The useful generalization is not that every change needs A/B testing or LaunchDarkly.

It is:

> when production uncertainty remains and the product/runtime supports it, reduce exposure, observe the relevant signals, and keep rollback cheap.

A staged rollout only reduces risk if:

- the gate truly isolates the new behavior;
- the watched signals can detect the important failure mode;
- the cohort is meaningful for the risk being tested;
- rollback/disable actually works;
- the decision to expand/rollback is explicit.

This connects directly to HE's metrics rule: collect a metric only when a known decision consumes it.

**Disposition: REINFORCE for deployable applications; not a PMB/HE universal requirement.**

---

# 11. Pre-launch "attack the product" should be risk-domain coverage, not agent count

The video proposes multiple agents checking:

- user flows/features;
- weird states and bugs;
- performance bottlenecks;
- security inputs/permissions/data.

Those are sensible assurance domains.

But the durable HE lesson is not "use multiple agents."

The same coverage can be produced by:

- deterministic tests/tools;
- one reviewer with distinct passes;
- specialized reviewers;
- runtime probes;
- human review where judgment matters.

Use separate agents only when isolation, specialization, parallelism, or independent verification materially improves the evidence.

### Durable implication

> **Pre-release assurance should cover distinct risk domains; the number of agents is an implementation choice, not a success metric.**

**Disposition: STRONGLY REINFORCE existing HE position.**

---

# 12. Cross-project implications

## HE

Strong corroboration for:

- risk-directed verification;
- evidence packages at stage boundaries;
- producer/verifier separation;
- deterministic checks outside probabilistic review;
- explicit merge-vs-launch lifecycle states;
- staged rollout as evidence when applicable;
- bounded reviewer/fixer loops.

## ACR

Relevant, but does **not** justify an immediate architecture rewrite.

Useful assessment questions:

- Can ACR's review prioritize or report semantic risk/blast radius without inventing a fragile global score?
- Does ACR receive enough authoritative task/spec context while remaining independent of author rationale?
- Can deterministic CI/static results be consumed as evidence without duplicating them as findings?
- Would an author-proof section in PR context help ACR verify claims rather than simply trust them?
- Are current domain passes (correctness/security/architecture/etc.) giving risk-domain coverage without unnecessary duplication?

Do not implement "leaf/trunk" labels or self-reported confidence thresholds without evaluation evidence.

## PMB

No direct PMB feature follows from this source.

It reinforces the general separation of:

- task authorization;
- execution;
- verification evidence;
- review acceptance;
- release/side-effect authority.

PMB should not become a release-management or feature-flag system.

---

# Overall disposition

## STRONGLY REINFORCE

- Review effort should be risk-directed.
- Producer proof and independent verification are complementary.
- Fresh reviewer context should exclude implementation bias while retaining authoritative task truth.
- Deterministic checks should absorb deterministic review work.
- Merge-ready and launch-ready are different lifecycle states.
- Pre-release assurance should target risk domains, not maximize agent count.

## ASSESS

- Compact proof packages / PR templates where reviewers actually consume them.
- Whether ACR should expose or consume change-risk/blast-radius evidence.
- Whether author-provided runtime/visual proof can improve independent review when treated as untrusted evidence rather than truth.

## PARK

- Automated PR babysitting until a concrete workflow requires it.
- General feature-flag infrastructure for HE/PMB.
- Mandatory multi-agent pre-launch sweeps.

## REJECT

- Line count as a proxy for review depth.
- "Leaf" status inferred without evidence of isolation/reversibility.
- Model self-confidence as correctness evidence.
- Copying static-check findings into semantic review merely to look thorough.
- Giving an automated reviewer/fixer loop unlimited iterations or merge authority.
- Treating merge-ready as automatically launch-ready.

---

# Bottom line

The strongest contribution is a shift from code-volume review to **risk-and-evidence review**:

> **Spend deep review where failure can travel far or recovery is hard; require task-relevant proof at the review boundary; keep the verifier independent of author bias; and treat production exposure as a separate, observable, reversible lifecycle decision.**
