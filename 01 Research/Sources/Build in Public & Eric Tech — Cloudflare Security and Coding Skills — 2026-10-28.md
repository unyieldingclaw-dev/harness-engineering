# Build in Public & Eric Tech — Cloudflare Security and Coding Skills — 2026-10-28

## Evidence boundary

Source inputs were two user-supplied transcripts (the linked YouTube videos could not be replayed), followed by direct inspection of upstream repositories and supporting GitHub issues on 2026-10-28.

- Build in Public, *Cloudflare's Free AI Security Team Is GitHub's #1 Repo*: https://www.youtube.com/watch?v=Cta0sXM97QE
- EricWTech, *Matt Pocock's 3 New Skills Close the AI Coding Loop*: https://www.youtube.com/watch?v=1MNSJhCGMLY
- Cloudflare public skill: https://github.com/cloudflare/security-audit-skill (especially skills/security-audit/SKILL.md, HUNTING.md, VALIDATION-AND-REPORTING.md).
- Matt Pocock primary skills: https://github.com/mattpocock/skills/tree/main/skills/engineering (implement-spec, pr, retro).
- Other primary repositories: https://github.com/akitaonrails/ai-memory ; https://github.com/Graphify-Labs/graphify ; https://github.com/Comfy-Org/ComfyUI ; https://github.com/ruvnet/ruflo .
- Independent, limited evaluations and failure reports are linked below. They are reported observations, **not independent local replications**.

**Status:** research source note only. No new HE principle is approved, no tool is recommended for default installation, and no changes to PMB, ACR, application code, Docker, or home security configuration are authorized.

## Distinctive evidence worth retaining

### 1. Cloudflare's strongest mechanism is claim refutation, not agent count

The skill explicitly separates: architecture/trust-boundary reconnaissance; coverage-led candidate hunting; **a fresh candidate verifier instructed to disprove each claim**; validated machine-readable finding records; independent record checking; and source-neutral reports. It distinguishes **confirmed**, **needs_validation**, and **rejected**, with unresolved runtime/deployment observations remaining unresolved rather than becoming either severity claims or clean passes. The public skill is the starting point for Cloudflare's larger internal system, not the whole internal harness.

- The value for HE/ACR is the separation of *finding proposal* from *acceptance of evidence*. A reviewer agreeing with its own candidate is not corroboration.
- **Coverage accounting is not the same as observed coverage.** Cloudflare issue #20 raised that an agent can report paths inspected without a corresponding observed source-read trace. The commenter reported one such case in their separate comparison pipeline, **not a measured prevalence for Cloudflare**. Distinguish **assigned surface → source observed → analysis returned → finding verified**; do not grant a "fully covered" label solely on an agent's declaration. https://github.com/cloudflare/security-audit-skill/issues/20
- A single seeded-target comparison in issue #20 reported median cost of $29.95 for the skill's quick mode versus $2.06 for a single-agent baseline, with equal median recall (0.467). Three rounds per arm and the evaluator's own competing pipeline make this insufficient for a universal cost or quality claim. It argues for **budgeted, task-specific experiments**, not default multi-agent escalation.
- The skill's strict execution boundary requires a genuine OS-enforced sandbox for target-controlled execution, sanitized environment, and bounded output. Source-only reviews should not imply execution-level confirmation. Its offline model does not independently ground live external dependency advisories.

**Incremental candidate:** in an ACR security-review experiment, require an independent refutation step for evidence-bearing candidate findings, retain "needs validation" when deployment facts cannot be established, and compare against the existing reviewer baseline on frozen clean/dirty fixtures. Do not add a default reviewer until measured.

### 2. A security fix must preserve intended capability

Cloudflare issue #88 reports that more than a dozen fixes reportedly blocked attacks **and** accidentally removed legitimate product behavior; suites passed because tests covered the prohibited operations but not the allowed users. This is a report from one outside deployment, not an independently established general defect rate. https://github.com/cloudflare/security-audit-skill/issues/88

A useful **two-sided remediation proof** is:

1. Negative control: the undesired operation fails after the change (and was possible or otherwise demonstrably at risk before).
2. Positive control: the legitimate actor and supported path still succeed before **and** after the change, including backward-compatibility/migration cases where relevant.

The smallest *effective* change minimizes legitimate observable behavior lost, not merely diff size or the breadth of refusal. This applies to RLS, auth, privacy controls, PMB command gates, and ACR filtering. Check the upstream state before copying proposals: issue #88 has related implementation work; this source note is not a claim that current Cloudflare releases lack the fix.

### 3. Matt Pocock's skills reinforce existing evidence and ownership contracts

- **implement-spec** treats tickets as a dependency graph and runs ready, independent work in isolated worktrees, integrating onto one branch and reviewing afterward. This is **optional execution topology**, useful only when independence and merge/review economics justify it. It is not a universal feature-work default.
  https://github.com/mattpocock/skills/blob/main/skills/engineering/implement-spec/SKILL.md
- **pr** asks for a concise visual/diff-based change summary where useful, **before/after execution evidence**, and **Merge Danger** (reversibility and blast radius). This is a compact presentation pattern, not a reason for a new acceptance authority or mandatory diagrams on trivial changes.
  https://github.com/mattpocock/skills/blob/main/skills/engineering/pr/SKILL.md
- **retro** inspects actual session evidence for navigation, checks, coding standards, tool economy, and context problems. Deterministically catchable mistakes belong in the owning linter/test/hook, not another always-loaded text rule. Retrospective proposals should be reviewed, not automatically installed. A valid outcome is **no durable change**.
  https://github.com/mattpocock/skills/blob/main/skills/engineering/retro/SKILL.md

These chiefly **reinforce**, rather than replace, existing HE research: *Risk-Directed Review & Release Proof*, *Experience-Derived Harness Evolution*, and *Skill Routing, Gate-Bound Evidence & Orthogonal Review*. The new value is concreteness around positive remediation controls and source-observed coverage, not a new six/seven-stage mandatory pipeline.

### 4. Remaining tools: comparison value, not adoption mandates

| Source | Useful observation | Disposition |
|---|---|---|
| AI Memory (akitaonrails) | Cross-agent claim-once handoff, session capture, Git-backed Markdown authority and derived indexes | **COMPARE** with PMB retrieval/handoff behavior; do not introduce a second durable memory authority. Capture/privacy and encryption-at-rest boundaries require explicit review. |
| Graphify | Deterministic structural index and extracted-vs-inferred relationship provenance | **ALREADY COVERED** in *Serop — Graphify A-B Test, Knowledge-Graph Economics & Tool Steering*. No new installation; an isolated task-class benchmark only if ordinary navigation demonstrates a gap. |
| ComfyUI | Reusable graph-based creative workflows, local execution and optional coding-agent integration | **SEPARATE CREATIVE TOOL**; not an HE/ACR/PMB capability. Untrusted custom nodes can execute code, so locality is not sufficient supply-chain protection. |
| Ruflo | Broad agent routing, hooks, memory and orchestration surface | **PARK/REJECT DEFAULT ADOPTION**: overlaps with existing controls and adds execution/authority complexity without a demonstrated local need. |

Popularity and GitHub star growth are discovery signals only, not evidence of correctness, security or benefit.

## Project-specific disposition

| Owner | Retain | Avoid |
|---|---|---|
| **HE** | Source-linked examples of independent refutation, source-observed coverage, two-sided remediation, risk-directed PR evidence | New universal security workflow, dashboard, mandatory parallelism |
| **ACR** | Candidate calibration experiment: independent adversarial validation against frozen clean/dirty fixtures; track supported findings, false positives, missed issues, cost/latency, and incomplete coverage | Adding permanent agents, changing orchestrator routing or thresholds absent replay evidence |
| **PMB** | Existing Opposition review and single-owner retro correction remain primary; optionally compare claim-once handoff reliability | A second memory store, transcript hoarding, always-on new guardrails |
| **Product apps** | Auth/RLS source-only pilot could use real named trust boundaries and positive/negative controls | Live production probing, secret exposure, automatic fixes |
| **Bastion** | Evidence distinction transfers to audit method | Treating code security audit as proof about router/firewall exposure |

## Proposed bounded trial — not yet approved or executed

1. Freeze a source revision and a small security-sensitive area (for example, invite-only roles/reveal authorization and related RLS migrations), with scope and expected behaviors clearly recorded.
2. Establish a baseline using the existing review flow and a finite set of known-clean and known-defective test cases. Do not use live credentials, hosted endpoint probes or real users' data.
3. Run independent candidate refutation as an **experimental alternative**, not an extra default gate. If target-controlled execution is needed and required sandboxing cannot be enforced, retain an explicit validation blocker.
4. Record coverage as **assigned / source-observed / reviewed / verified** when those levels are actually evidenced, not inferred from final prose.
5. Measure unique validated security findings, false-positive reductions, untouched legitimate paths, context/tool spend, run duration, and operational complexity. Compare with baseline and reject the intervention if incremental benefit is not demonstrated.
6. Any recommended fix must carry both prohibited-path and legitimate-path evidence. Implementation and acceptance remain separate owner-controlled decisions.

## Disposition

- **MINE:** Independent security-claim refutation; source-observed versus agent-declared coverage; source-grounded unresolved state; positive-and-negative remediation checks; compact PR proof/reversibility presentation.
- **REINFORCE:** Source-of-truth ownership for retrospectives; deterministic checks over repeated steering; risk-based review cost; measured, reversible experiments.
- **PARK:** AI Memory / PMB behavioral comparison; Graphify structural workload test; optional parallel implementation topology; creative ComfyUI evaluation.
- **REJECT AS DEFAULT:** Broad Ruflo installation; a second persistent memory authority; new mandatory security swarm; automatic retrospective rule writing; treating popularity or one small benchmark as a go decision.

**Bottom line:** Strengthen *evidence at acceptance boundaries* before expanding the harness. Promote no new mechanism without a reproduced failure class and a measured incremental improvement.