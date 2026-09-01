# Austin Marchese — Loop Engineering & Skill Mining

**Date:** 2026-09-01  
**Status:** Research / assessment; no architecture adoption implied

## Executive assessment

Austin Marchese's material is useful for the Harness Engineering work, but mostly because it makes several existing principles operational and testable rather than because it introduces a new architecture.

The strongest ideas for this repository are:

1. **Verification is the limiting factor for loops.** An iterative workflow is only useful when there is a meaningful acceptance signal and a bounded termination condition.
2. **Durable run memory should be treated as an engineering artifact.** The simple output + run-history pattern is consistent with the repository's existing durable-artifact and continuous-improvement themes.
3. **Skills are useful as bounded capability/progressive-disclosure boundaries.** The value is in making a repeated operation explicit and reusable, not in creating a skill for every task.
4. **`improve-system` is the most interesting candidate for PMB.** It provides a concrete mechanism for converting repeated experience and session history into controlled changes to skills/knowledge.
5. **Separate verification is useful, but "different agent" is not sufficient evidence of independence.** Model-family diversity, fresh context, narrow evidence inputs, deterministic checks, and calibrated evaluators matter more than merely spawning another subagent.
6. **Boards/focus groups are decision-support mechanisms, not authorities.** They can expose competing perspectives, but their outputs must remain advisory and source-grounded.
7. **Loop Training Mode is a sensible development mechanism, not a permanent governance layer.** Start supervised, measure behavior, then reduce intervention while retaining objective checks and retry bounds.

## Relationship to existing Harness Engineering research

The repository already treats Austin's earlier graph-engineering material as research rather than as a mandate. The new material reinforces that position.

The useful abstraction is still **workflow topology driven by actual information dependencies**, not a general-purpose graph framework. Sequential chains, parallel fan-out, routing, and bounded iteration are execution patterns. They should be selected because the work requires them, not because the architecture has a graph abstraction available.

This material also reinforces existing themes:

- progressive disclosure;
- durable artifacts;
- context isolation;
- independent verification;
- bounded authority;
- evidence before architecture;
- continuous improvement;
- model capability drift.

## Mined pattern: the minimum viable loop

A practical loop can be reduced to:

```text
trigger
  ↓
execution skill
  ↓
artifact/output
  ↓
verification
  ├── pass → publish / complete
  └── fail → bounded retry / correction
                 ↓
              verify again

run memory ← result + failure/lesson data
```

The architecture does **not** require a graph engine. The same behavior can be implemented with an explicit orchestration skill or ordinary scripts where the workflow is small and deterministic.

### Required properties

A loop candidate should have:

- a repeatable trigger;
- a stable or explicitly parameterized task;
- a meaningful done-rule;
- an output artifact or observable state change;
- bounded retries/termination;
- enough data/tool access to perform the work;
- a durable run record when later runs are expected to improve.

The repository should continue to reject the assumption that every repeated task deserves a loop.

## Verification: the most important mining result

The creator's repeated emphasis on verification is stronger than the surrounding branding. The loop does not create quality; it creates **opportunities to apply an acceptance test repeatedly**.

That leads to a useful hierarchy for Harness Engineering:

### Tier 1 — deterministic acceptance

Use when possible:

- tests;
- type checks;
- lint/static analysis;
- schema validation;
- file/state assertions;
- build/deploy health;
- exact content or structural checks.

### Tier 2 — narrow model evaluator

Use when the property is semantic but can be expressed narrowly enough for an evaluator to judge against evidence.

The evaluator should answer a constrained question rather than provide an unconstrained review essay.

### Tier 3 — broader model review

Use for qualities such as clarity, maintainability, completeness, or competing design considerations where no deterministic oracle exists.

### Tier 4 — human checkpoint

Use where the decision is materially consequential or where a wrong branch choice would invalidate downstream work.

This is particularly important for non-quantifiable tasks. The creator's recommendation to place human verification at high-leverage decision points is compatible with bounded-authority design.

## Separate-agent verification: useful, but insufficient

The source recommends having a different AI analyze the output. That is a useful control, but it should not be treated as independent verification by itself.

A second subagent can share:

- the same model family and failure modes;
- the same misleading evidence;
- the same system assumptions;
- the same flawed rubric;
- the same generated work/context.

For ACR and other high-value verification, a stronger pattern is:

```text
producer
   ↓
artifact + evidence
   ↓
independent verifier context
   ↓
structured verdict
   ↓
deterministic publication/gate
```

Where practical, improve independence by varying model family, constraining verifier inputs, using deterministic checks first, and maintaining evaluator fixtures/calibration tests.

## `improve-system`: highest-value candidate

This is the part of the video worth pursuing rather than merely noting.

The source's five-mode pattern can be translated into a Harness Engineering capability without adopting Austin's naming or implementation literally:

| Mode | Harness interpretation | Value |
|---|---|---|
| Audit | Find stale, conflicting, duplicated, or orphaned knowledge/skills | High |
| Skill Review | Compare a skill's intended behavior with recent failures/usage | High |
| Experience | Capture explicit user feedback and lessons | High |
| Historical Review | Mine recent runs/sessions for recurring failure or rework patterns | High |
| Foundation | Detect missing project constraints/context needed for reliable execution | Medium–High |

### Why it matters

The important change is from **memory as passive notes** to **memory as evidence for controlled system improvement**.

A useful implementation loop would be:

```text
collect evidence
    ↓
classify observed problem
    ↓
propose smallest useful change
    ↓
run targeted validation/eval
    ↓
human approval for durable system changes
    ↓
record change + reason + evidence
```

The critical word is **observed**. The system should not rewrite skills simply because a model believes they could be better.

### Guardrails

`improve-system` should not:

- autonomously rewrite foundational rules after a single failure;
- treat model-generated opinions as operational evidence;
- create duplicate skills for similar tasks;
- turn every conversation into a permanent memory entry;
- silently change acceptance criteria;
- optimize for shorter prompts at the expense of behavior;
- use its own generated changes as proof that the changes were correct.

This is a strong candidate for a future PMB capability, but it should be evaluated against actual recurring failures before becoming a broad self-modifying subsystem.

## Board of Advisors: useful pattern, dangerous framing

The board idea is useful when the problem genuinely benefits from **multiple explicit perspectives**. It is not useful merely because multiple agents sound more sophisticated.

### Good uses

- architectural tradeoff review;
- product strategy alternatives;
- competing design philosophies;
- challenge of an initial proposal;
- identifying assumptions the primary agent overlooked.

### Poor uses

- objective acceptance testing;
- security gates without concrete evidence;
- replacing deterministic checks;
- creating fake authority around public personalities;
- asking multiple agents the same vague question and treating majority vote as truth.

The best implementation would preserve each perspective separately and make the synthesis expose:

- claims;
- supporting evidence;
- disagreements;
- uncertainty;
- recommended action;
- unresolved questions.

### Important epistemic boundary

"Clone an expert" should be interpreted as **construct a source-grounded perspective from an expert's published material**, not as reproducing the actual expert's judgment.

For this repository, a better name may eventually be **Perspective Panel** or **Challenge Panel** rather than Board of Advisors, depending on the actual use case.

## Internal focus group

The focus-group pattern is a specialized form of multi-perspective review. It is potentially useful for product/content work but is less directly relevant to core Harness Engineering than `improve-system` or verification.

Its strongest side effect is forcing explicit identification of the intended audience or evaluator. That is useful. The persona simulation itself is less trustworthy than the underlying audience criteria and evidence.

For PMB, treat this as a possible later capability rather than a foundational harness primitive.

## Loop Training Mode

The screenshots' training-mode idea is operationally sound as a **development and calibration mode**:

- default ON while the loop is new;
- inspect each step;
- skip steps whose done-rule already passes;
- retry only failures;
- cap retries;
- observe repeated successful runs;
- switch OFF only after behavior is understood;
- keep the acceptance checks and retry bounds after autonomy is enabled.

The repository should resist turning this into a permanent approval bureaucracy. The goal is to learn whether the loop is safe enough to reduce intervention, not to institutionalize a human click at every step.

## Output + memory

The source's simple two-artifact pattern is worth retaining conceptually:

1. **Output** — what the loop produced.
2. **Run record** — what happened, what failed, what was learned, and what should matter next time.

The run record should be structured enough to support later analysis, but not so elaborate that recording the run costs more than the work itself.

A minimal record can include:

- run ID/date;
- input/trigger;
- skill/version;
- outcome;
- verifier result;
- retries/interventions;
- failure category;
- lesson/change candidate;
- links to output/evidence.

## Web research / ingestion stack

Austin's six-skill sequence treats web discovery and source ingestion as separate capabilities. That separation is correct conceptually:

```text
find source → retrieve/clean source → classify/store → use as evidence
```

The retrieval layer should not own knowledge-base policy, and the ingestion layer should not silently turn every retrieved page into durable project knowledge.

## Exa assessment

Current Exa documentation describes an AI-oriented search API with search, contents extraction, agent/deep-search workflows, and coding-agent use cases. Exa currently offers a free starter allowance and usage-based developer pricing; the current pricing page lists Search at $7/1,000 requests, Contents at $1/1,000 pages, and Agent at $0.012–$1/run depending on effort. citeturn1search0turn1search6

### Fit

**Strong fit for:**

- semantic web discovery;
- finding relevant papers, docs, GitHub material, and technical sources;
- research loops where the search query itself is part of the task;
- coding-agent research that needs current documentation.

**Less compelling for:**

- simply fetching a URL already known;
- workflows where the existing web/search tooling already provides adequate retrieval;
- PMB storage or memory by itself.

**Assessment: ASSESS / likely useful utility, not a core harness primitive.**

Exa becomes more interesting if we build a recurring research/ingestion loop where search quality and semantic retrieval are an actual bottleneck. Until then, adding it simply because it is part of Austin's stack is premature.

## Firecrawl assessment

Firecrawl currently provides scrape, crawl, map, search, browser interaction, and structured extraction capabilities. Its official Claude plugin is available in the Claude plugin marketplace and exposes `/firecrawl:scrape`, `/firecrawl:crawl`, `/firecrawl:search`, `/firecrawl:map`, and `/firecrawl:agent`. citeturn1search12turn1search14

Current self-serve pricing is credit-based: the free plan provides 1,000 credits/month; Scrape/Crawl/Map consume 1 credit per page, and Search consumes 2 credits per 10 results. citeturn1search1

### Fit

**Strong fit for:**

- JavaScript-heavy sites;
- crawling known sites or documentation trees;
- converting web pages into clean Markdown for ingestion;
- extraction where ordinary page fetching is unreliable;
- source acquisition as a dedicated ingestion utility.

**Less compelling for:**

- ordinary web searches;
- replacing every existing fetch/search mechanism;
- being embedded directly into the harness core.

**Assessment: ASSESS as a targeted ingestion utility.**

Firecrawl is the more obvious complement to an `/ingest-source`-style capability because its job is closer to **reliable acquisition and normalization** than general research reasoning.

## Exa vs. Firecrawl

| Need | Better fit | Reason |
|---|---|---|
| Discover relevant sources | Exa | Search is the primary capability |
| Search technical material semantically | Exa | AI-oriented retrieval and coding-agent use cases |
| Fetch a known difficult page | Firecrawl | Scrape + rendering/extraction |
| Crawl a documentation/site tree | Firecrawl | Crawl/map primitives |
| Turn pages into ingestion-ready Markdown | Firecrawl | Explicit scraping/extraction pipeline |
| Deep research synthesis | Exa | Agent/deep-search capabilities |
| Core harness dependency | Neither | Keep vendor services at the capability edge |

## Plugin recommendation

Do **not** install both as permanent harness dependencies yet.

Instead:

1. Build or identify the actual recurring research/ingestion task.
2. Measure where the current workflow fails: discovery, retrieval, JavaScript rendering, extraction, or synthesis.
3. Add the smallest capability that removes the observed bottleneck.
4. Keep the service behind a skill/tool boundary so the harness does not become coupled to one provider.
5. Record source URLs and provenance regardless of provider.

If we want to experiment now, **Firecrawl is the cleaner first experiment for source ingestion**, while **Exa is the cleaner first experiment for semantic research/discovery**.

## Recommended disposition

| Concept | Disposition | Rationale |
|---|---|---|
| Bounded loops | **ADOPT principle** | Already compatible with evidence-gated execution |
| Explicit done-rules | **ADOPT principle** | Core requirement for useful iteration |
| Separate verifier | **ADOPT with stronger independence rules** | Useful, but not automatically independent |
| Output + run memory | **ADOPT principle** | Supports durable learning without requiring a memory platform |
| Loop Training Mode | **ASSESS** | Useful calibration mechanism; avoid permanent bureaucracy |
| `improve-system` pattern | **PRIORITY RESEARCH** | Strongest candidate for controlled continuous improvement |
| Board of Advisors | **ASSESS / PARK** | Useful decision support, weak as an authority mechanism |
| Internal focus group | **PARK** | Specialized; lower priority for core harness work |
| `/web-scraping` | **ASSESS** | Capability boundary is useful; implementation should follow observed need |
| `/ingest-source` | **ASSESS** | Strong fit for durable evidence acquisition |
| Compound Engineering plugin | **ASSESS, not adopt wholesale** | Process is useful; plugin duplication is unnecessary without evidence |
| Exa | **ASSESS** | Strong research/search capability; not a core primitive |
| Firecrawl | **ASSESS** | Strong targeted ingestion/crawling capability |
| Graph orchestration framework | **PARK** | No demonstrated need for infrastructure abstraction |

## Bottom line

The most valuable idea here is not "Claude Loops." It is the combination of:

**repeatable capability + explicit acceptance signal + bounded iteration + durable run evidence + controlled improvement.**

That is already close to the direction of this Harness Engineering repository. Austin's material gives us useful concrete patterns for making those principles operational, especially the `improve-system` concept.

The Board of Advisors is interesting, but I would keep it downstream of the core evidence/verification model. It should help generate competing perspectives, not become another layer that declares something correct.

For plugins, treat Exa and Firecrawl as replaceable edge capabilities. If a real PMB workflow demonstrates a retrieval bottleneck, use the provider that solves that bottleneck rather than importing the creator's entire stack.
