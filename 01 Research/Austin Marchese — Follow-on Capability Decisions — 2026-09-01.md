# Austin Marchese — Follow-on Capability Decisions

**Date:** 2026-09-01  
**Status:** Research / candidate capabilities; no implementation commitment

## Purpose

The Austin Marchese material reviewed on 2026-09-01 produced several ideas worth carrying forward beyond the initial loop-engineering synthesis. This note records what should remain visible in the research record and separates useful primitives from product-specific implementation choices.

## Additional items worth documenting

### 1. `improve-system` as a first-class improvement loop

This is the strongest new candidate from the material.

The useful pattern is not autonomous self-modification. It is a bounded improvement workflow:

```text
recent work / failures / feedback
            ↓
      identify a pattern
            ↓
    propose a small change
            ↓
       evaluate it
            ↓
     approve / reject
            ↓
  update skill / knowledge
            ↓
      record the lesson
```

The important constraints are:

- improvements should be evidence-backed;
- the proposed change should be explicit and reviewable;
- the system should distinguish observation from inference;
- verification should not simply approve its own proposed change;
- changes should be reversible;
- the reason for a change should remain durable.

This should be investigated as a Harness capability before building a generalized autonomous optimizer.

### 2. Loop Training Mode

The material's ON/OFF training mode is useful as a development and calibration mechanism:

- **Training ON:** pause at meaningful steps, expose decisions, require approval, and observe failure modes.
- **Training OFF:** execute without routine approval pauses while retaining objective done-rules, verification, and bounded retries.

This should not become a permanent governance layer. The goal is to use supervision to establish confidence, then remove unnecessary human intervention while preserving objective checks.

### 3. Separate verification

The material reinforces the value of a verifier that is not simply the producer repeating its own work.

However, a separate subagent alone does not establish independence. Useful independence can come from:

- fresh context;
- restricted evidence inputs;
- a different model or model family where appropriate;
- deterministic checks;
- narrow verifier responsibilities;
- calibrated scoring against known examples.

This remains consistent with the repository's existing verification research.

### 4. Board / internal focus group

These are useful as **decision-support patterns**, not authorities.

Potential topology:

```text
                 ┌─ perspective A ─┐
question ────────┼─ perspective B ─┼──→ synthesis
                 ├─ perspective C ─┤
                 └─ perspective D ─┘
```

The value is deliberate disagreement and coverage of multiple lenses. The output should preserve disagreement rather than convert simulated consensus into false certainty.

For acceptance-critical engineering work, objective evidence and verification remain higher authority than a panel of model personas.

### 5. Web ingestion as a reusable capability

Austin's `web-scraping` → `ingest-source` pattern is worth preserving independently of the specific tools used.

A useful boundary is:

```text
Discovery / retrieval
        ↓
Web extraction / rendering
        ↓
Normalized source artifact
        ↓
Ingestion / provenance
        ↓
Durable project knowledge
```

The scraper should not silently become the knowledge system. Extraction and ingestion should remain separable so that the retrieval implementation can be replaced without redesigning the downstream knowledge workflow.

## Web crawling / extraction options

### Firecrawl

Firecrawl itself is open source and self-hostable under AGPL-3.0. Its cloud offering adds hosted capabilities. Self-hosting therefore removes the hosted API dependency but does not remove operational ownership of browsers, queues, storage, security, upgrades, and capacity.

For this repository, Firecrawl should be treated as a candidate implementation rather than a required dependency.

### Crawl4AI

Crawl4AI is the strongest open-source alternative identified for the intended use case. It is designed specifically around AI/LLM-friendly extraction, supports JavaScript-rendered pages, provides Markdown/structured extraction, can run locally, and exposes a Docker server with REST and MCP interfaces.

It is Apache-2.0 licensed, which is materially simpler for many internal/commercial integration scenarios than adopting an AGPL service as a core dependency.

The current project does not need a full crawling platform yet. A small proof of capability should answer whether Crawl4AI provides enough of the desired behavior for:

- arbitrary URL → clean Markdown;
- JavaScript-heavy pages;
- bounded site crawling;
- PDF/page capture where useful;
- stable metadata/provenance;
- local execution;
- MCP access from agent tooling.

### Crawlee

Crawlee is another strong open-source option, especially when the requirement is to build a production crawler rather than consume an LLM-oriented extraction service. It supports HTTP and real-browser crawling, queues, persistence, retries, proxy/session handling, and JavaScript rendering.

It is better viewed as a lower-level crawling framework than a direct Firecrawl replacement. Choosing it would imply owning more of the extraction and normalization layer.

## Recommendation

Do **not** add Firecrawl or Exa to the Harness core yet.

Instead:

1. Document the **web ingestion capability** as a reusable boundary.
2. Prototype **Crawl4AI** as the first self-hosted implementation candidate.
3. Keep Firecrawl as the comparison/reference implementation, especially if its higher-level extraction behavior proves materially better.
4. Keep Exa as a separate **research/discovery** capability rather than coupling semantic search to the crawler.
5. Build `improve-system` as a conceptual/prototype capability before adding generalized autonomous system modification.
6. Treat Board / Focus Group as optional decision-support patterns, not verification infrastructure.

The key architectural principle is **replaceable capability behind an explicit boundary**. The project should own the ingestion contract and provenance semantics, not become dependent on one crawling vendor or framework.

## Research links

- Firecrawl repository: https://github.com/firecrawl/firecrawl
- Firecrawl self-hosting: https://github.com/firecrawl/firecrawl/blob/main/SELF_HOST.md
- Crawl4AI repository: https://github.com/unclecode/crawl4ai
- Crawl4AI Docker/MCP server documentation: https://github.com/unclecode/crawl4ai/blob/main/deploy/docker/README.md
- Crawlee repository: https://github.com/apify/crawlee
