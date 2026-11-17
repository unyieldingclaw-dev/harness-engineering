# Matt Pocock — Mastra Typed Workflows, Graphs & API Drift — 2026-10-22

## Source and evidence boundary

- **User-supplied artifact:** `ScreenRecording_10-22-2026 06-20-15_1.mp4`, a **60.07-second** portrait screen recording of a Matt Pocock / `@mattpocockuk` short about **“Mastra, the AWESOME new TypeScript AI Agent framework.”**
- **Artifact fingerprint (SHA-256):** `b0f5a4c4e9c2e6b6c82f4117e7f8a1337e6d7355b2256dbf36748336e51dbbd0`.
- **Reviewed:** 2026-10-22, by sampling video frames and reading visible captions and example code. **Audio was not independently transcribed**; no complete transcript or runnable code accompanies this source.
- **Related public post (independently opened; exact original-video equivalence not verified):** [Matt Pocock's Mastra post on LinkedIn](https://www.linkedin.com/posts/mapocock_mastra-the-awesome-new-typescript-ai-agent-activity-7340729332274741249-sjDT), publicly visible as an approximately year-old post at review time; its timestamp does **not** establish the exact creation date of this screen recording. Original YouTube source URL **not verified**.
- **First-party technical verification:** [Mastra current workflow-control-flow documentation](https://mastra.ai/docs/workflows/control-flow), [Mastra vNext workflow migration announcement](https://mastra.ai/blog/vNext-workflows), [Studio documentation](https://mastra.ai/docs/studio/overview), [human-in-the-loop documentation](https://mastra.ai/docs/agents/human-in-the-loop), and [Mastra GitHub repository](https://github.com/mastra-ai/mastra) (`main` observed at `6338514d87a00247ad0c71dabdf7303d0151ca71` on review date). Current documentation is a moving target.
- **Preservation choice:** This concise source/evidence note and fingerprint are retained, **not** a copy of the video or an endorsement of the short. Frame timestamps are approximate.
- **Prior HE source by the same speaker:** `01 Research/Sources/Matt Pocock — Agent Environment, Intent & Verification Interview — 2026-10-19.md`. The two Pocock items are not independent practitioner corroboration.

## What the recording actually shows

| Approx. recording time | Visible evidence | Evidence class |
| --- | --- | --- |
| 0–10 s | Presenter describes a TypeScript agent language/framework, showing Mastra's framework page | **VIDEO-OBSERVED** capability claim, not a benchmark |
| 11–22 s | TypeScript `new Agent({ name, instructions, model, memory, workflow })` example; narrator references AI SDK and memory | **VIDEO-OBSERVED** sample API, version not specified in recording |
| 23–36 s | Workflow uses `.step(...)`, `.then(...)`, `.after(...)`, `.commit()`; diagram displays conditional “success” and “retry” paths converging on “finalize” | **VIDEO-OBSERVED** legacy composition/visualization |
| 39–60 s | Presenter says a local development environment can support UI building and testing through an ordinary REST API | **VIDEO-OBSERVED** claim, corroborated at a product-capability level by current Studio docs |

**The short does not establish:** correctness, runtime isolation, permissions, resumability across crashes, audit completeness, cost reduction, prompt-injection resistance, independent effect verification, or a controlled comparison against HE/PMB/ACR.

## Critical correction — pictured API is obsolete

**FIRST-PARTY VERIFIED:** Mastra's [vNext announcement](https://mastra.ai/blog/vNext-workflows) says that its redesigned workflows **retired `.step()` in favor of `.then()`**, **removed `.after()`**, and introduced `.branch()` and `.parallel()` with typed step schemas. The redesign was announced **April 29, 2025**; the recording upload date is **not** the release date of the demonstrated API. Replaying an old short on October 22, 2026 does not make its implementation current.

Current docs expose `createWorkflow` + `createStep`, typed input/output schemas and `.then()`, `.branch()`, `.parallel()`, loops and `.commit()`. For current branching, **more than one condition may be true and execute**, unless mutually exclusive conditions or an explicit first-matching behavior is used. Do not assume the legacy diagram means exclusive branching.

**HE takeaway:** a compelling short demonstrates a *concept* but may encode a superseded API. Verify the source version, failure semantics and actual current contract before elevating it to a pattern or reference implementation.

## Adversarial correction criteria

- Treat the workflow graph as **claimed/intended topology** until instrumented step traces establish which branches and side effects actually ran.
- A truthful conditional workflow may run **multiple** truthy branches; add a counterexample with both conditions true before claiming exclusive if/else semantics.
- A typed output can still be semantically wrong. Verify actual postconditions and authority on an independent evidence path.
- Approval prompts require exact operation binding and protected policy enforcement; a UI confirmation alone must not be treated as authorization.
- A successful suspended-run demonstration without persistent storage does not prove crash-safe continuation or single authoritative attempt.
- Compare a no-framework baseline (small deterministic script/host-native workflow), and measure total runtime ownership, failure recovery and exit cost before adopting a long-lived engine.
- The source is *one* practitioner clip plus first-party product documentation; do not use it as independent evidence of quality improvement.

## First-party capabilities worth inspecting if HE ever needs an execution reference

1. **Typed step boundaries.** Mastra requires typed schema matching between workflow steps. This can make data dependencies and invalid transitions more observable. **LIMIT:** Type validation is not authorization, user approval, state ownership, or proof of business-level correctness.
2. **Graph-based orchestration.** A workflow DSL can make sequential, conditional, parallel and fan-in structure inspectable; Studio can visualize executed paths. **LIMIT:** a graph picture alone cannot show whether the work truly should have run, who owned it, or whether downstream effects happened.
3. **Local test/debug surface.** Mastra Studio exposes live execution, step output, tracing, errors and local API operations. **LIMIT:** observability is not an independent evaluation oracle, and a UI is not necessarily the right HE product boundary.
4. **Suspend/approval.** Mastra has human-in-the-loop tool approvals and storage-backed suspended runs. Its own documentation states that **restart-safe resumption requires persistent storage**; default in-memory state can be lost. **LIMIT:** suspend/resume infrastructure does not settle HE's separate authoritative Task/Attempt identity, exact approval scope, or effect verification requirements.
5. **Provider/tool framework.** Mastra offers agent definitions, tools, routing and memory as TypeScript services. **LIMIT:** adopting the runtime creates a new dependency surface without any HE-001 evidence of a needed replacement for existing coding hosts.
6. **License boundary.** The pinned repository's [`LICENSE.md`](https://github.com/mastra-ai/mastra/blob/6338514d87a00247ad0c71dabdf7303d0151ca71/LICENSE.md) expressly separates **Apache-2.0 material** from **Enterprise Edition features** under any `ee/` directory, governed by [`ee/LICENSE`](https://github.com/mastra-ai/mastra/blob/6338514d87a00247ad0c71dabdf7303d0151ca71/ee/LICENSE). `packages/core/package.json` declares `Apache-2.0`, but the GitHub repository's API reports `NOASSERTION` at the whole-repository level. Assess the exact package/feature and its licensing boundary if piloted; repository visibility is **not** blanket permission to use Enterprise Features.

## Mapping to existing HE ownership

| Existing HE work | What Mastra adds as external comparator | Disposition |
| --- | --- | --- |
| **HE-001 — Baseline Harness Assessment** (`02 Assessments/HE-001 — Baseline Harness Assessment.md.md`) | A concrete example of how typed dependency topology, conditional execution and graph inspection *could* work. HE-001 already asks which workflow stages are truly dependent and which can run concurrently. | **REINFORCE** an existing assessment dimension, no new required deliverable |
| **HE-002 — Modular Capability Architecture** (`03 Decisions/Decisions Index.md.md`, **PENDING HE-001**) | Useful reference for an optional composition layer only if measured work shows a need for an executable orchestrator. | **ASSESS after HE-001**; do not accelerate HE-002 or turn its candidate into an approved decision |
| **Supervised Agent Orchestration & Effect Verification** (2026-09-21) | Inspectable steps can support evidence and transitions, but do not replace authoritative Task/Attempt identity, explicit settlement, UNKNOWN state, or verification of external effects. | **REINFORCE**, not new doctrine |
| **Artifact-Gated SDLC, Behavioral Evals & Metrics** (2026-09-22) | Typed I/O gives a structural boundary, useful only when paired with *acceptance semantics* and observed postconditions. | **REINFORCE** |
| **AI Engineering Observability & Dashboard Boundary** (2026-09-18) | Local Studio is an example of a **derived operational view** built from runtime evidence, not an authority for project truth. | **REINFORCE / PARK UI** |
| **Harness Portability, Exit Cost & Inspectability** (2026-09-24) | Mastra's workflow and resume/storage semantics are a potential vendor/runtime lock-in path even if model-provider routing is portable. | **ASSESS replacement cost if considering a runtime** |
| **Bounded Execution Envelopes** (2026-09-22) | A framework graph does not itself enforce budgets, protected operations or right-to-act. | **KEEP control/execution/assurance boundaries separate** |

## Testable HE question for a later HE-001/HE-002 comparison

**Do actual multi-step AI engineering tasks benefit from typed, inspectable execution graphs beyond the artifacts, scripts and host-native workflows HE already has?**

If HE-001 finds a task that warrants evaluation, run a **small, disposable, non-production comparator**:

- Select one real but sanitized/reproducible multi-step task with conditional branches and a required externally observable postcondition.
- Compare (A) current host-native workflow + explicit artifact/checks against (B) a minimal typed workflow implementation. Do not replace the production host or add a generic gateway.
- Measure step/data contract failures detected, correctness and verified effect, diagnostic time, missing or duplicate execution, human approval integrity, context/compute overhead, resume behavior after forced interruption, tracing usefulness, maintenance burden and lock-in/export effort.
- Plant a type mismatch, two simultaneously-true route predicates, missing/stale approval, a successful tool call with absent postcondition, and lost in-memory resume state. The verifier must catch the *behavioral* problem; an attractive flowchart does not pass.
- If equivalent assurance is achievable by a small deterministic script or existing harness feature, prefer that and **reject** new runtime dependence.

**Go/no-go:** implementation only after HE-001 provides an actual workload/reliability problem, the simpler comparator is measured, and the chosen owner of orchestration is explicit. Do not use this research note as a decision to build a dashboard, install Mastra, add new standard instructions, refactor PMB or add ACR features.

## Disposition

- **MINE:** typed data/step contracts, graph-as-derived-view, real execution tracing, demonstrated API-freshness risk.
- **REINFORCE:** supervised orchestration authority boundaries, artifact-based acceptance, verification outside the model, runtime exit-cost assessment.
- **ASSESS:** an optional typed-orchestration comparator **only if HE-001 exposes a real need**.
- **PARK:** Studio/dashboard integration, new runtime or server, persistent workflow platform, model gateway and generic agent framework adoption.
- **REJECT:** copying `.step()`/`.after()` from the clip; treating a visual graph, an approval option, or a successful API call as proof of valid effects.

**No production code, tests or Mastra runtime were executed for this review. No HE-001/HE-002 decision or work-transfer obligation changes here.**
