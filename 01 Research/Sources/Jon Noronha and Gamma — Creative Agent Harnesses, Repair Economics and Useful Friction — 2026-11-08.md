# Jon Noronha and Gamma — Creative Agent Harnesses, Repair Economics and Useful Friction — 2026-11-08

**Status:** source-specific research note for HE; **no build authorization**. Not a proposal to adopt Gamma, an orchestration platform, visual-review model, analytics stack, custom model router or new governance mechanism.

**User-supplied interview:** [Greg Isenberg with Jon Noronha, *$100M ARR Founder: The Next Big AI Opportunity*](https://www.youtube.com/watch?v=st61aKW8P5A), Oct 28, 2026; full user-supplied ~49-minute transcript plus detailed description/chapters, reviewed November 8, 2026. Noronha is co-founder and Chief Product Officer of Gamma. Interview sponsored by Anthropic; the speaker is a builder with first-hand operational knowledge, but his unpublished internal metrics are still **self-report**, not reproducible benchmark data.

**Independent and first-party cross-checks:**
- Gamma, [*Creative Evals Are Broken*](https://gamma.app/insights/creative-evals-are-broken), Priyaa Kalyanaraman, Nov 3, 2026 — **strongest new technical evidence**, defining *acceptance*, *repair ratio*, instruction following, craft, contextual judgment, and example task-specific mechanism selection.
- [Gamma 5 launch](https://gamma.app/insights/introducing-gamma-5-your-story-your-style-your-tools), Oct 6, 2026 — confirmed redesigned creative agent, human editing, contextual connectors, style variability, import/export.
- [Gamma official Agent usage help](https://help.gamma.app/en/articles/15002203-how-do-i-create-with-agent-in-gamma), checked Nov 8 — **Plan** (questions, plan review, approval) versus **Express** (fast draft, no plan/approval), **Research** in Plan only; mobile browser starts Express by default; content can be edited manually and selectively.
- [Gamma effort picker](https://help.gamma.app/en/articles/16951195-what-is-the-effort-picker-and-how-do-i-use-it), checked Nov 8 — Lite/Standard/Max explicitly trade speed, price/credits and quality; model choices differ by role/media and effort.
- [Gamma's 2025 $100M ARR announcement](https://gamma.app/insights/how-we-built-a-usd100m-business-differently) corroborates the organizational scale; **it does not prove any particular 2026 harness design caused Gamma's revenue**.
- [SiliconANGLE Gamma 5 interview](https://siliconangle.com/2026/10/06/exclusive-gamma-revamps-presentation-platform-to-make-output-look-more-human/), Oct 6, corroborates a sandboxed agent, sources/connectors and automated layout/crop checks.

## Executive decision

**Useful new empirical lens:** A generated output can satisfy a benchmark or complete a workflow while leaving substantial human rework. Evaluate both **acceptability without repair** and **cost to repair**, and distinguish explicit instructions, objective craft defects and subjective contextual judgment. This fits HE's existing artifact-gated lifecycle, but is a more informative quality/economic measurement for *subjective artifacts*.

**Useful negative experiment:** Gamma reports its first broad *fast mode* (skipping clarification and outline review) **lost on user satisfaction and paid conversion** despite faster generation. It deployed a narrower Express variant especially for new mobile users. This argues for **risk-/ambiguity-/context-directed human clarification**, not automatically minimizing interactive steps or mandating plans for every task.

**Useful model-fit case:** the right mechanism is constrained by output representation and user editing needs (SVG vs PNG), factual/creative needs (real photo vs invented moodboard), availability of context, determinism and verifiability—**not just model intelligence and per-token price**.

**Not new architecture:** sandbox, dynamic forms, retrieval, parallel model calls, multiple roles, model tiering, iterative verification, prompt-first engineering and A/B testing already have HE research owners. Gamma's internal scale gives examples, **not** approval to recreate the full 20-model/200-call system.

## 1. What the creator says actually happens

Noronha describes an illustrative **20-slide presentation** workflow at 4:25–8:23:

1. Provision a cloud sandbox with user request, attached source and relevant context. Ask clarifying questions via an adaptive form when audience/purpose/style materially affect results; read connected systems such as Slack/Notion/Linear.
2. Generate an outline and offer users an editable direction. Retrieve and adapt a visual style/template from a large library using search/embeddings/vision.
3. Compose text, layout, charts, images and diagrams through parallel tool/model calls; maintain coherent artifact context.
4. Run visual inspection on **the rendered slide output**; identify actual failures (cut-off person, contrast, text alignment, padding), propose scoped repairs, re-render and recheck.
5. Present the editable artifact. Let human users change style or substance without discarding all previous work.

His rough **~20 models / ~200 calls** is a **single illustrative fully repaired generation**, *not* a documented cost, latency, quality, or capacity requirement; first draft after ~100 calls by his description. We did not inspect private orchestration code, traces, inference bills or a trial. The *scope of work* changed with several models and iterative review; cannot infer that model count drove quality.

**What transfers:** explicit task intent; fit-for-task context and tool choices; inspect the **artifact**, not only messages; bounded issue repair; human acceptance. For high consequence tasks, keep external acceptance criteria independent of the producer. **What does not:** a fixed 5-stage Gamma pipeline, 200 model calls, an inference gateway, managed sandbox/cloud, vector template search, generative UI or image tools without a demonstrated HE/PMB/ACR need.

## 2. New first-party technical source: acceptance + repair ratio

In [*Creative Evals Are Broken*](https://gamma.app/insights/creative-evals-are-broken), Gamma frames every task against **speed, cost and quality** expectations; quality has two outcome measures:

- **Acceptance:** Does the artifact clear the user's quality threshold with no further edits?
- **Repair ratio:** How much work to make an unaccepted first attempt useful, compared with doing the work from scratch?

Gamma evaluates along **instruction following**, **craft**, and **judgment**, with concrete examples:
- Explicit constraints: every requested slide includes the footer logo; translated text must fit without destroying the intended layout.
- Craft: legibility, crop/overlap, contrast, alignment, type hierarchy, brand conformity.
- Judgment: actual photo for factual geography vs image generation for speculative scenery; editable SVG icon/diagram versus fixed PNG when brand colors must update; retrieval from meetings and live analytics before drafting a report.

The article says Gamma can observe post-generation editing behaviors (what users repair, how long they spend), but **does not publish** a full statistical model, per-model repair curves, acceptance threshold, user segmentation, or experiment results. Do not claim precise observed repair savings.

**HE evaluation inference:** End-to-end verification should sometimes include *how much human repair remains*. Potential **conditional** examples:
- ACR: **human triage minutes per true actionable finding**, false-positive burden, rework after acceptance; still preserve precision/recall, severity and security misses as higher-priority gates.
- PMB: omitted/stale source correction burden or requery cost **only if** actual user sessions demonstrate recurring context-recovery problems.
- Web/UI feature: rendered smoke tests and measured manual fixes for overflow, keyboard focus, viewport issues, contrast. A generic vision model is not a replacement for deterministic a11y checks or human assessment.
- HE coding: time-to-correctly-accepted change including patch revision, test repair, review effort; not simply agent turn count or “the PR passed.”

**Important caveat:** More user edits can reflect *investment or personalization* rather than failure; fewer edits can reflect abandonment rather than quality. Paid conversion is a **business metric**, not a trustworthy stand-alone correctness/security test. Interpret metrics alongside user outcomes, task type and failures; do not add surveillance of individuals.

**Research disposition:** add this as a candidate evaluation **lens** in existing artifact-gated/metrics ownership, not a new mandatory HE metric collection subsystem. ACR evaluation corpus is a more reasonable first use case than a global HE dashboard.

## 3. Fast Mode: the cost of deleting useful decision points

At 26:25–29:10 Noronha describes an internal experiment: a faster flow removed some early questions and outline approval. Their hypothesis was lower time to first result would increase value. He reports **lower satisfaction and paid conversion overall**, then using a faster option for a **new mobile-user segment**.

The currently documented product has **Plan, Express and Research** modes; Plan asks for approval before constructing, Express skips the plan review and produces a draft, mobile starts Express. Gamma's docs confirm **the shipping behavior**, but the experimental lift, sample size, confidence, treatment balance, revenue attribution and cohort composition remain unpublished speaker claims.

**HE inference:** “fewer interrupts” is not the true optimization target. Some short human decisions remove high-cost ambiguity (wrong audience, wrong software behavior, incorrect brand/goal, high-risk filesystem mutation) while other approvals are merely repetitive ceremony. Separate:
- **Useful friction:** human goals/acceptance or irreversible/high-path-dependence choices before costly execution.
- **Waste:** re-asking known facts, prompting to approve low-risk actions under already-granted scope, asking for opinions that don't affect the outcome.
- **Context sensitivity:** mobile/short task vs expensive durable deliverable, ambiguous vs clear goal.

**Strong caveat:** higher conversion doesn't prove higher correctness, and Gamma is creative software, not regulated change management. Do not translate this result into a universal forced planning ritual. Instead test *clarification or not* on real ambiguous tasks and measure **verified result plus rework**, not wall clock alone.

## 4. Model portfolio and output-medium eligibility

Noronha says Gamma uses many model providers but generally shallow prompts, compositional context and simple per-role task decomposition; custom model training/fine-tuning followed proven scale and specialized tasks (slide visual QA). “98% value from prompts” is a **subjective rough framing**, not demonstrated allocation or an HE target. “New model tested within an hour” depends on existing test/rollout instrumentation and large user population; not a reasonable cadence for a solo engineer.

A more specific first-party example matters: Gamma's Nov 3 eval article describes **PNG vs SVG** for brand-system outline icons. A PNG generator may look correct but cannot adapt to style changes or preserve infinite scaling; a code-capable model making SVG is eligible where editability, sharpness and theming are requirements. A **real photo** is preferable to an invented photorealistic scene when historical/factual accuracy matters; an image generator is appropriate for speculative moodboards.

**Portable HE implication:** Model selection should be constrained by **artifact representation contract, deterministic tool suitability, verifiable outputs and current source fidelity** before optimizing for quality/cost/latency. This is an extension/example of `Model-Tiered Workflows & Independent Factory Assurance`, not grounds for a universal prompt-driven model router.

A narrow decision table for one *observed* workload might be enough:
```text
task outcome / must-preserve properties
      ↓
legal/allowed output representations
      ↓
deterministic method sufficient?  → use it
      ↓ if not
evaluated model+tool candidates
      ↓
generate  → artifact-specific verify  → human approval where needed
```
Don't copy Gamma's model counts or multistage agent orchestration as proof of optimal design.

## 5. Taste ≠ absence of evaluation

At 15:49–18:40 Noronha explicitly says taste is difficult to score objectively, but also says Gamma still evaluates:
- adherence to provided brand/instructions;
- whether an agent asks clarifying questions for ambiguous cases;
- output craft/visual defects;
- human choices, time spent editing, eventual use/sharing;
- randomized experiments and retention/conversion.

**HE correction:** Do not interpret “taste beats evals” as “evals are useless.” Most requirements have objective subparts and subjective residuals. Assess objective constraints with deterministic tests when possible, subjective outcomes with humans/representative user studies, and avoid an LLM jury pretending to establish ground truth. Check calibrated samples and protect against reward hacking: optimizing for “number of edits” can make the product suppress edits rather than improve artifacts.

**PMB relevance:** preferences and “voice” can be used to *propose* the next context for an authorized task, not silently promote inferred styles to universal rules. Preserve data scope, consent, source ownership, and user correction/overrides. An agent impersonating the user in Slack/emails requires explicit reply approval; personalization is not independent authority to send.

## 6. A/B experimentation: good operational habit, not universal requirement

Gamma describes launching 10–20 experiments at a time, trying new small models on bounded tasks, and keeping model-specific results; **no published per-experiment datasets, methods or statistical significance** were supplied in the interview.

Useful pattern for actual HE/ACR experiment:
1. Identify one observed pain or uncertainty (e.g., high rework rate for a specific ACR finding category).
2. Freeze representative tasks, context, inputs, verifier and human review semantics.
3. Change one variable (clarification, visual QA pass, prompt/model, tool candidate).
4. Record both **automatic pass/fail** and downstream human correction burden, cost, false positives/regressions and task time.
5. Keep the winner only if meaningful benefit survives; otherwise revert, don't build a service to monitor it.

**Bias caveats:** For adaptive experiments, watch selection/traffic effects, novelty, learning curves, multiple-comparison false positives, gradual rollout, and whether monetization is being confused with task quality. A small solo project does not need continuous large-sample A/B experimentation; paired blind human review of representative artifacts can be enough.

## 7. Product opportunity talk — interesting but largely out of HE scope

- Vertical niches: own unique data/relationships and workflow expertise, not merely model wrappers. Generic handwaving about “big industry” isn't evidence of a specific pain/market.
- Personal Slack assistant learning voice: requires source/permission/retention/voice boundaries and human-send approval; not an HE or PMB feature request.
- Furniture-in-room visualization: a generated scene can **look** dimensionally convincing while being wrong. True 58-inch versus 68-inch placement requires trusted product dimensions plus calibrated scene geometry/reference scale or physical measurement; pixel lengths alone in an uncalibrated AI image cannot prove real-world fit. **No tool proposal.**
- 20 models/200 calls: creative quality is the product's value proposition and engineering economics differ from HE. For coding, lower complexity with independent acceptance is often preferable; [minimal-harness research](../Turing%20Post%20%E2%80%94%20Harness%20Minimalism%2C%20Long-Horizon%20State%20%26%20Control%20Planes%20%E2%80%94%202026-10-19.md) gives a counterexample to orchestration inflation.
- Model-provider competition helps pricing, but tool semantics, policies, and distribution differences create real portability costs.

## 8. HE ownership and acceptance

Existing owners should **not** be duplicated:
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md` — artifact-level acceptance, behavioral evals, end-to-end value.
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md` — role/capability selection and independent verifier.
- `01 Research/Role-Based Model Allocation & Software Factory Boundaries — 2026-09-24.md` — simple per-task model allocation.
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md` — tool-call success vs observed accepted effects, supervisory ownership.
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md` — targeted improvements from observed failures and traces, human-promoted rules.
- `01 Research/Sources/Austin Marchese — Anthropic Growth Workflows, Sub-Threshold Work & Bespoke Tools — 2026-09-29.md` — already addresses narrow targeted tools and outcome-driven automation.

### Disposition / priority

**RETAIN (high-value evidence):** **acceptance plus repair effort**, useful friction versus reflexive fast-pathing, and first-party creative eval split between instruction, craft and contextual judgment.

**REINFORCE:** task-specific verification, feedback before expensive irreversible outputs, user-editable deliverables, input/output contract ownership, and *role/workload* model evaluation.

**PARK:** direct Gamma-like visual review for a specific UI project only if repeat defects warrant a small pilot; any ACR quality metric change until baseline/real review cost is known; personal voice-assistant/product opportunities.

**REJECT as HE roadmap:** a 20-model/200-call creative agent, generic design taste scorer, comprehensive automated A/B platform, experimental model auto-router, automatic fine-tuning, new global memory/persona service, AI-managed Slack replies or uncalibrated dimension-verified room renders.

**All source lessons are research hypotheses for HE unless independently tested on HE/PMB/ACR workloads.** No implementation or behavioral modifications performed.
