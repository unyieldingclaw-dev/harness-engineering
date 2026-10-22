# Beyond Coding — Kerim Satirli, Production IaC Gates, Expected Changes & BLUF — 2026-10-22

**Disposition: SHARPENS existing HE owners; ASSESS one minimal negative-test fixture; PARK a dedicated software-factory runtime. No architecture adoption or production change.**

## Source, provenance and reliability

- **Interview:** Patrick Akil, *Beyond Coding*, "How Senior Engineers Use Coding Agents On Prod Infrastructure," guest **Kerim Satirli**, senior developer advocate at HashiCorp (an IBM company): https://www.youtube.com/watch?v=LaFpcqAJeQE
- **Episode published:** October 21, 2026, as listed by the external podcast episode directory: https://podscan.fm/podcasts/beyond-coding . The user's video link opens at 24:10 (`t=1450s`); research covers **the entire supplied transcript**, not just that segment.
- **Primary reviewed material:** user-provided pasted transcript, `Pasted text(20261022-135743).txt`, **SHA-256 `beafb26efde7414f8696cbb6260ca32c0c7d4094a77c89c84ed0f8eb0689c1be`**. Approximate 42-minute conversation; timestamps shown below track the supplied transcript. Automated transcription includes obvious recognition errors, so do not rely on ambiguous proper nouns or quoted technical syntax unless independently checked.
- **Source-status classes:** `INTERVIEW-REPORTED` = guest or hosts' descriptions; `OFFICIAL-DOC-VERIFIED` = described platform contract confirmed in first-party docs; `HE-INFERENCE` = our testable translation; `NOT-REPRODUCED` = actual interviewee environment not inspected.
- **Inspected:** 2026-10-22. No privileged account, Terraform workspace, CI runner, private code, Terraform plan, Cloudflare regression reproduction, real deployment, or A/B evaluation was accessed or executed.
- The speaker's real production design is **not published as a runnable reference implementation by this interview**. Do not imply his particular pipeline implements any named HCP Terraform run-task/policy product, or infer that all his production applies require human review.

## Bottom line up front

The guest distinguishes **probabilistic proposal** from **enforced decision/side effect**. An agent can create changes, locate documentation, diagnose provider regressions, and present a concise risk assessment. A separate, authoritative pipeline should establish allowed actions, check the **expected infrastructure effect**, route exceptions to appropriately authorized humans, perform approved actions, and record what actually happened. **A prompt instruction is not a hard production permission boundary.**

HE already understands this distinction. The **incremental value** is the *specific negative test* around provider-upgrade no-op expectations and the human-facing **BLUF / exception-focused operator contract**, not another general-purpose agent framework.

## Transcript map with provenance and limits

| Video location | Interview-reported observation | Boundary / HE value |
| --- | --- | --- |
| **00:00–00:30; 07:03–10:30** | "Make no mistakes"/"don't delete the cluster" are polite requests. Guest describes moving execution into deterministic quality and CI/CD checks. | `INTERVIEW-REPORTED`. Reinforces external enforcement; his controls and outcomes aren't audited. |
| **00:38–04:18** | Contentful/Terraform, modular providers and API-first systems improved codification and reproducibility. | Plausible systems-design rationale, not evidence every integration is complete, low risk, or API-idempotent. |
| **04:19–06:40** | Cloudflare DNS provider update allegedly produced an unexpected plan diff in a test repository; agent found an upstream issue and recommended holding rollout to other IaC repositories. | **Key test case:** success of a plan is not acceptance of its *contents*. Exact provider release, issue and actual code **not independently verified**. |
| **07:03–12:59** | Nondeterministic generation with deterministic test/quality gates; architecture/specifications make iteration less wasteful. | Already owned by HE Artifact-Gated SDLC, Monotonic Policy and Risk-Directed Release Proof. |
| **13:07–15:40** | AI-authored PRs transfer verification/maintenance effort to open-source owners. cURL given as cultural example. | Real maintainer-load concern, but cURL's specific recent history **changed**; see first-party correction below. |
| **15:41–18:51** | Personal software factories encode organization-specific languages, architecture and API/style requirements. | Reject universal "factory" architecture or coercive per-engineer workflows; provider/compiler constraints belong to actual project owner. |
| **18:52–26:59** | BLUF, concise exception summaries, separation of agent-facing detail from human decisions; preserve detailed source/artifacts. | **Incremental operator-surface pattern**. Do not suppress uncertainty, hidden failure or exact approval parameters. |
| **27:02–29:03** | Most runs in guest's setup are auto-applied, but he describes a deterministic destructive-resource check that escalates some actions (e.g., AKS deletion or backup bucket deletion). | **Risk-scoped exceptions, not blanket approval.** Exact policy and privilege boundary aren't published. |
| **29:03–34:50** | Spec-driven design and architectural choices ahead of implementation; GitHub Spec Kit mentioned. | Strongly overlaps existing HE specification/scaffold and artifact-gate research. Spec Kit is prior art, not a required dependency. |
| **34:51–42:15** | Rust reimplementations, reverse engineering, customizable software, product-market needs. | Broad product/law speculation and analogies; don't convert to an HE architecture, license, or legal conclusion. |

## Primary-documentation checks (as of 2026-10-22)

### Terraform plan semantics: SUCCESS != NO CHANGE != SAFE CHANGE

- Official CLI: https://developer.hashicorp.com/terraform/cli/commands/plan . With `-detailed-exitcode`, **0 = success/no diff**, **1 = failure**, **2 = success/diff**. Checking only process success misses unexpectedly nonempty plans.
- `terraform plan` without `-out` is **speculative**; docs explicitly warn that changes to the target environment can invalidate the eventual outcome. Recheck a final executable plan before applying.
- Official JSON contract: https://developer.hashicorp.com/terraform/internals/json-format . The `resource_changes[].change.actions` list includes `["no-op"]`, `["create"]`, `["update"]`, `["delete"]`, `["delete","create"]`, `["create","delete"]`. A **replacement includes deletion**; a rule looking only for exact `["delete"]` fails dangerously. The plan also exposes `resource_drift`, `after_unknown`, `action_reason`, and sensitive-value markers. JSON schema versions must be validated and unknown major versions refused.
- **High-consequence security caveat:** both saved binary and JSON plans can expose secrets, even where ordinary CLI output redacts them. Official https://developer.hashicorp.com/terraform/language/manage-sensitive-data and https://docs.hashicorp.com/terraform/tutorials/cli/plan say to protect them and never commit plan files to VCS. **Never send a raw saved plan to an external general-purpose LLM merely for a summary.**

### Actual infrastructure enforcement surfaces exist — but the interview does not show which ones it uses

- https://developer.hashicorp.com/terraform/cloud-docs/workspaces/settings/run-tasks documents **pre-plan, post-plan, pre-apply, post-apply** run tasks and advisory vs mandatory modes. **Pre-apply mandatory** checks can block a run if the remote task fails; a post-apply check is too late to prevent the effect.
- https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement/manage-policy-sets documents policy-as-code enforcement, with Terraform policy, Sentinel and OPA; enforcement/override behavior differs by policy engine, evaluation type and user privileges. Therefore "we have a policy" is **not** proof that a destructive action is unconditionally impossible.
- https://developer.hashicorp.com/terraform/cloud-docs/workspaces/settings and https://developer.hashicorp.com/terraform/cloud-docs/workspaces/settings/vcs document auto-apply vs manual apply. **Auto-apply can be compatible with controlled exceptions** if appropriate preventive gates/permissions exist; it is not independently proof of safety.
- **HE-INFERENCE:** production enforcement requires checking **what policy ran, at which phase, with which version/scope, against which exact plan, under whose override authority**, and ensuring the constrained agent cannot change its own allowlist, policy, credentials or gate result.

### Spec Kit and cURL — avoid distorted takeaways

- GitHub Spec Kit: https://github.github.com/spec-kit/ and https://github.github.com/spec-kit/reference/agentic-sdd.html . The currently documented sequence distinguishes specify, plan, tasks, implement, and converge, with optional clarification and assessment. This corroborates available structured tooling, **not** a measured superiority claim or a mandate to install the package.
- cURL's maintainer recorded its 2026 chronology: https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/ — monetary bounty ended in January after junk-report overload; https://daniel.haxx.se/blog/2026/02/25/curl-security-moves-again/ — from March, HackerOne was again the official **reporting channel**, without restored monetary rewards; https://daniel.haxx.se/blog/2026/04/22/high-quality-chaos/ — maintainer reported that the earlier slop flood was no longer the same problem. **This does not mean cURL banned all AI code or external PRs.**
- cURL contribution guidance https://github.com/curl/curl/blob/master/docs/CONTRIBUTE.md explicitly permits AI-assisted contributions that meet the normal coding, verification, testing, license and maintenance conditions. **Don't cite "cURL rejects all AI-authored PRs."** The guest's point about maintainer burden still stands as a general risk, not as a fixed current cURL policy.
- **Independent source evolution illustrates the freshness problem itself:** when a headline about slop stays valid as *history* but a subsequent official source alters current operational facts, preserve both and date the conclusion.

## HE mapping — most concepts already have owners

| Interview lesson | Existing accepted HE owner on `main` | Net new value and decision |
| --- | --- | --- |
| Prompted safety is not enforced safety | `Monotonic Policy, Cross-Host Instructions & Behavioral Skill Evaluation — 2026-09-25.md`; `Budgeted Side Effects, Approval Gates & Receipts — 2026-09-25.md` | **REINFORCE**, no duplicate principle/rule |
| Preview → exact authorization → effect → receipt | `Budgeted Side Effects, Approval Gates & Receipts — 2026-09-25.md`; `Supervised Agent Orchestration & Effect Verification — 2026-09-21.md` | **SHARPEN with executable expected-change counterexample**, not a new engine |
| Spec-before-implementation and deterministic test stages | `Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`; `Sources/Beyond Coding — Carlos Kelkboom, Planning Scaffolds & Iterative Agent Engineering — 2026-10-15.md` | **REINFORCE**, do not mandate another Spec Kit installation |
| Human risk review vs blind approval of all work | `Risk-Directed Review & Release Proof — 2026-09-24.md`; `Navigable Truth, Routine Manifests & Derived Screens — 2026-09-25.md` | **SHARPEN risk-class explanation**, permit lower-risk automation only after evidence |
| Human-readable BLUF over extensive machine record | `Consumer-Specific Representations & Evidence-Preserving Councils — 2026-09-25.md`; `Navigable Truth, Routine Manifests & Derived Screens — 2026-09-25.md` | **SHARPEN** separate artifact-to-human decision contract |
| AI PR burden on maintainers | `Risk-Directed Review & Release Proof`, HE Specialist Bench pilot (unmerged draft #15) | **REINFORCE** measure corrections/reviewer time and downstream maintenance, not outputs |
| Third-party repo/paper historical freshness | `Source Preservation & Evidence Durability — 2026-09-16.md`; current HE corpus audit draft #21 | **ACTIVE AUDIT EXAMPLE** via cURL dated chronology and Terraform policy changes |

## Small HE experiment worth running — not yet implemented

**Probe:** When an agent proposes a routine Terraform provider update, can deterministic logic block unexpected resource changes and reserve destructive actions for independent authorization **without using an LLM as the policy oracle**?

**Suggested minimum test contract (synthetic, disposable plan fixtures only):**

1. **Expected-no-op upgrade**, no changes: `ALLOW` only if explicit target-version/expected-change contract and all other gates pass.
2. **Unexpected `["update"]`** during no-op upgrade: `BLOCK/PARK` even though `terraform plan` itself succeeded.
3. **Pure `["delete"]`** of protected resource: `REQUIRE AUTHORIZED EXCEPTION` or hard-deny per owning policy; neither operator summary nor agent prose authorizes it.
4. **Replacement `["delete","create"]` or `["create","delete"]`**: must be classified as deletion, not disguised as a harmless update.
5. **Unknown fields/schema version, missing policy, or malformed plan**: `BLOCK/UNKNOWN` rather than green.
6. **Approval token for plan A replayed on modified plan B**: reject; bind authorization to plan/target/workspace/policy identity, expiry and actor. Re-evaluate target state at apply time.
7. **Gate bypass and privilege test**: candidate worker cannot modify required policy, skip mandatory gate or acquire credentials that can directly apply outside policy.
8. **Postcondition test**: `apply` command success is not enough; verify actual provider/workspace result and detect drift, unexpected side effects or unresolved unknowns.
9. **Privacy negative**: user-facing summary and logs must not expose secrets or full saved-plan artifacts.
10. **Signal-to-noise test**: only decision-relevant exceptions trigger human requests; suppressed routine details remain retrievable by stable source pointer; quantify missed critical alerts, false positives, human correction time, and irreversible-effect protection.

A minimal **decision record** could have: source ref + versions, expected/no-op invariant, actual action classes and resource identifiers, exception reason, policy version/gate status, exact plan identity (without exposing secret-bearing plan), actor/decision authority, disposition (`allow`/`hold`/`approval required`/`unknown`), and secure pointers to detailed evidence. Do not create a second canonical policy store or use the agent's own "looks safe" judgment.

**Important false-safety cases:** Terraform's first-party [lifecycle documentation](https://developer.hashicorp.com/terraform/language/meta-arguments/lifecycle) confirms that `prevent_destroy` requires the rule to remain in resource configuration and **does not protect the object after the resource configuration is removed**; a resource-class string check may miss provider aliases, moved state, refactors or indirectly destructive effects. External, privileged enforcement should test the actual plan and target, not rely only on a convention or the model's reasoning. Low-risk classifications themselves require review and rollback assessment.

**Go/no-go:** only prototype this if HE-001 or an approved real owner demonstrates actual residual risk that existing CI/HCP Terraform policy rules do not already prevent. An ordinary script/policy and fixture tests would be better than a new "production agent" product. No deployment is authorized by this research note.

## Source limitations and research disposition

- The guest provides a strong practitioner explanation, **not** experimental evidence proving fewer outages or guaranteed safety.
- The precise Cloudflare release/regression and guest's 15–16-repo automation setup could not be reproduced from public artifacts during this review.
- The "most applies automatically" and "don't cross until a human says go" remarks are compatible when construed as **policy-gated exceptions**, but the full guest policy is unavailable; no universal interpretation is verified.
- Not all checks described can be implemented safely with static plan parsing alone: secret exposure, race/drift, authorization, policy bypass, provider side effects and post-apply evidence need their actual authoritative systems.
- The economic/documentation benefits (fewer interruptions, faster output, better operator decisions) remain hypotheses until measured against a comparable baseline.
- **Outcome for HE:** keep this **one historical source note** as a testable example of existing HE principles; **do not** modify HE-001, HE-002, PMB, ACR or enterprise-specific repos, install Terraform, or change production practices in this PR.
