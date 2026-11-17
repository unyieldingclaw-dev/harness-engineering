# Austin Marchese — Five Questions, Scope Selection & Minimum Viable Evals — 2026-11-10

## Source and evidence boundary

- Video: **If You Ask Claude 5 Questions, It'll Make You Build 10x Faster** — Austin Marchese with Ryan Ouyang, https://www.youtube.com/watch?v=fFktpPMQzjI
- Primary observations: user-supplied video description, prompt screenshots, Whop demonstration screenshots, and closing slides, reviewed November 10, 2026. Exact transcript wording and claimed performance gains are **not** independently benchmarked here.
- Vendor context: the video explicitly promotes Whop and Marchese's own products. Whop's first-party CLI description confirms both read and write operations and broad business-account scope: https://whop.com/blog/cli/ and https://whop.com/blog/cli-guide/.
- First-party engineering cross-check: Anthropic's *Building effective agents* and *Demystifying evals for AI agents*: https://www.anthropic.com/research/building-effective-agents and https://www.anthropic.com/research/demystifying-evals-for-ai-agents.

**Disposition: REINFORCE / NARROW — no new HE framework, PMB release requirement, AI orchestration layer, or mandatory prompt sequence.** The distinguishing value is a practical *negative-selection* question before building, plus a clearer trigger for a small risk-directed eval. Neither establishes a measured “10x” gain.

## The five questions, weighed against HE

| Video question | Practical contribution | Existing owner / HE overlap | Disposition |
| --- | --- | --- | --- |
| **1. What is the context and desired outcome?** Interview the user; specify an outcome, metric and deadline. | Useful where success is undefined. A metric can make candidate work comparable. | Project brief, task contract, acceptance criteria and source-owned project state. See *Context Engineering* and *Artifact-Gated SDLC*. | **REINFORCE.** Not every project has one sufficient metric. Safety/correctness constraints remain independent gates. Do not write every transient goal into `CLAUDE.md`; keep it in the owning task/plan or project artifact. |
| **2. What is the most important thing to build?** Consider personal leverage, possible actions, smallest test, and delay implementation. | Good intervention when implementation is cheap and prioritization is weak. | *Evidence Before Architecture*, scope control, model-driven action vs demonstrated need. | **REINFORCE.** “Unique ability” is a prioritization clue, not a substitute for required maintenance, contractual obligations, security, or dependency work. One high-leverage hypothesis needs a baseline and stopping criterion. |
| **3. What should NOT be built?** Inventory existing commitments, unused plans, external tools and overlaps. | Strongest marginal coaching prompt: makes the option to remove/defer explicit before adding features. | HE already emphasizes smallest owner, no redundant subsystems, no speculative architecture, and external tools only when they solve a demonstrated problem. | **KEEP AS AN OPTIONAL PRE-BUILD CHALLENGE.** “A tool does 80%” is a heuristic, not a pass/fail threshold. Validate permissions, portability, maintenance, total cost, reliability, and real gaps. Avoid treating retrospective session omissions as proof of low value. |
| **4. Where are the high-stakes hot zones, and which merit a minimum viable eval?** Identify sensitive changes, preserve approval and verification. | Helpful when it directs extra proof to a consequence-heavy boundary and names input, expected result, checks and reporting. | *Adaptive Autonomy, Diagnostic Gates & Fan-Out/Fan-In*; *Artifact-Gated SDLC, Behavioral Evals & Metrics*; existing PMB tests/pilot and ACR calibration work. | **STRONGLY REINFORCE, BUT NOT A NEW GATE.** Use existing tests/invariants first. Escalate approval at risky mutation or irreversible effect, rather than whenever an agent merely reads a “hot” file. |
| **5. What is the actual bottleneck?** Compare candidate bottlenecks to observed data, effort/return, and meaningful benchmarks. | Good corrective to optimizing a visible but nonlimiting symptom. | HE evidence-before-architecture and experience-derived telemetry; proposed Dashboard must remain a separate read-only product. | **REINFORCE.** Avoidance, low activity and model guesses do not establish causation. Benchmark comparisons need comparable definitions and sources. Quantify when real measurements exist; otherwise label hypotheses. |

## Specific design cautions

1. **Goals have owners.** The video puts a “one number / one deadline” into `CLAUDE.md`. That can turn temporary business priorities into globally loaded instructions. Store a project goal beside its plan or authoritative project state. `CLAUDE.md` should not become a substitute backlog.
2. **“Do not build” is a decision, not dead-code detection.** Existing controls, compatibility paths, CI scripts, fallbacks, and infrequently used safety logic cannot be discarded merely because session logs show little use.
3. **Risk location is not approval scope.** Sensitive areas include auth/credentials, release/CI authority, database schema/data migrations, payment or financial operations, and exposure of external data. Read-only diagnosis can proceed where authorized. Require a human decision before material high-risk writes or deployment, with bounded scope and explicit consequences.
4. **An eval requires evidence.** At minimum: a representative input/changed behavior, known baseline or known-bad condition, test or human rubric, observable report, and clear pass/fail or decision rule. Existing deterministic tests or a tiny fixture may be enough; a reusable eval service often is not. Distinguish a deterministic test from a slower human-reviewed evaluation. Do not invent numerical ROI.
5. **History is incomplete evidence.** Activity logs show what happened, not everything that mattered or work intentionally deferred. Distinguish observed bottlenecks, suspected ones and factors outside the logging surface.
6. **Vendor recommendations have a conflict of incentives.** Whop's CLI can expose business stats and issue mutations. Its Economic Intelligence recommendations are vendor-generated suggestions, not independently validated business benefits. The demo's “Run” flow indicates potentially consequential effects; approval should be scoped to the intended operation rather than authorizing unspecified future charges. This is not a reason to adopt Whop for HE, PMB, ACR or the Dashboard.
7. **10x is promotional.** The video does not supply controlled baseline/time-to-correct-outcome measurements. Faster action or more tasks is not proof of 10x better outcomes.

## Project-specific disposition

- **PMB:** **PROTECT RELEASE.** Five-file memory hierarchy, `mb doctor`, security review, CI and pilot/evidence mechanisms already exist. Do not introduce hot-zone registries, a new eval framework, goal-promotion into startup files, or Dashboard dependencies before the release. Release-blocking defects must be supported by actual failing checks or strong evidence.
- **ACR:** Already has a local diff review surface and specialty findings. Prior calibration discussion includes a small clean/dirty test corpus and timeout considerations. When results justify it, prefer one **measured** improvement in missed critical defects, false positives, runtime or reproducibility using existing evaluation machinery. Do not add a “strategy agent,” more review agents, or a second test framework because of this video.
- **HE:** This is a **source-level reinforcement**, not a new architectural pillar or authorization of HE-002. Fold the “what would we explicitly decline to build?” question into a *specific planning decision* only when scope is at risk. Maintain provenance, human authorization, owner mapping and existing eval boundaries.
- **Multi-Provider AI Engineering Dashboard:** Independent of PMB, ACR and HE. User demand for Home + Work visibility across Claude, Cursor, Gemini and Codex is explicit, but adapter capabilities/permissions remain to be demonstrated. Prioritize a small provider-source feasibility test and read-only evidence view; **do not** import Whop-style task execution, agent routing, scheduler, cross-environment data sharing or one universal token/cost schema on speculation.

## Smallest worthwhile action

**No implementation now.** Retain one optional planning challenge for major discretionary work:

> What measured outcome are we trying to improve? What would we explicitly not build, and which existing tool or smaller change might suffice? What is the smallest safe test that could disprove our preferred plan? Does this touch a high-consequence boundary requiring targeted approval or evidence?

This is a decision aid, **not** a mandatory five-question form, universal checklist, persistent `CLAUDE.md` rule, or new skill. Use only where it could change the decision. If a concrete bottleneck is later found, update the owning task, test, process, or integration rather than creating another governing layer.

## Existing HE research — no repetition proposed

- `01 Research/Context Engineering.md.md` — source ownership, progressive disclosure, context measurement.
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md` — history as evidence, smallest durable correction at the owning component, opportunity blind spots.
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md` — acceptance evidence, risk-directed checks, evaluation isolation, harness effort measurement.
- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md` — diagnose-before-mutate and bounded autonomy.
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md` — keep an external, source-owned dashboard independent of PMB and HE.
- `01 Research/Sources/Austin Marchese — Karpathy LLM Wiki, Autoresearch & Context Engineering — 2026-09-29.md` — separate earlier Austin research; not duplicated here.

**No changes to main, PMB, ACR, the Dashboard, model settings, instructions or runtime are authorized by this note.**
