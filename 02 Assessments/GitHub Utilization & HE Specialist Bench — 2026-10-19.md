# GitHub Utilization & HE Specialist Bench Assessment — 2026-10-19

## Objective

Assess observable GitHub use in the three public engineering repositories named below, identify proportionate improvements, and define an exploratory, controlled experiment for role-specific HE agents. Do not infer private-repository posture from this public assessment.

Tracking experiment: [Issue #14 — HE Specialist Bench vs current workflow](https://github.com/unyieldingclaw-dev/harness-engineering/issues/14)

Source evidence pass: [Ryan Doser & Aaron Makelky — GitHub Work Infrastructure and Agent Office Prior Art — 2026-10-19](../01%20Research/Sources/Ryan%20Doser%20%26%20Aaron%20Makelky%20%E2%80%94%20GitHub%20Work%20Infrastructure%20and%20Agent%20Office%20Prior%20Art%20%E2%80%94%202026-10-19.md)\n\nThis assessment is intentionally conservative:

- no feature is recommended merely because GitHub supports it;
- no agent is admitted merely because a role can be named;
- no setting or architecture should be changed without a concrete property it protects or measurable friction it removes.

---

# 1. Repositories inspected

Historical snapshot taken 2026-10-19 through the connected GitHub account. These counts are **not live-state claims** and must be refreshed before changing repository settings, deleting branches, or designing new cross-repo tooling. Mainline evolution after the snapshot is expected. **Private-repository settings and counts are intentionally excluded from this public assessment.**

| Repository | Visibility | Main protected | Branches | Open PRs | Open issues | Actions workflows | Delete branch on merge |
|---|---|---:|---:|---:|---:|---:|---:|
| harness-engineering | Public | No | 24 | 8 | 3 | 0 | No |
| personal-memory-bank | Public | Yes | 49 | 15 | 0 | 1 | No |
| ai-code-review-agent | Public | Yes | 4 | 1 | 1 | 4 | No |

Notes:

- GitHub's repository `open_issues_count` includes pull requests; the separate issue/PR searches above distinguish them.
- HE currently has no `.github/` directory.
- PMB has `pmb-health.yml`.
- ACR has `calibrate.yml`, `ci.yml`, `release.yml`, and `review.yml`.
- HE has Wiki enabled despite the repository itself being the canonical documentation corpus.

---

# 2. Current maturity by GitHub capability

## Repository as durable truth — STRONG

All three public projects inspected here already use Git repositories as durable project state.

PMB and ACR go farther with repository-local AI guidance, tests, standards, and CI.

HE is especially strong as a versioned evidence corpus.

No additional "AI memory repository" is needed merely to obtain portability.

## Pull requests — STRONG, but branch lifecycle is weak

HE uses PRs effectively as research evidence/review boundaries.

PMB also has a high number of active branches and PRs.

The branch count is not itself a defect, but the combination of:

- many merged/experimental branches over time;
- `delete_branch_on_merge=false` in HE, PMB, and ACR;
- repeated research/worktree branch naming;

creates discoverability cost and stale-branch ambiguity.

### Candidate correction

Enable automatic deletion of merged head branches for HE, PMB, and ACR **after confirming no workflow depends on preserved branch refs**.

This does not delete open/unmerged work.

## Main-branch protection — INCONSISTENT

- PMB: protected.
- ACR: protected.
- HE: not protected **as of the 2026-10-19 snapshot**. Recheck the effective ruleset/branch-protection state before action.

For a solo developer, requiring another person's approval can create ceremony with no independent reviewer available.

The useful protection target is therefore not "maximum settings." It is:

- prevent accidental force-push/deletion;
- prefer PR-mediated changes where review history matters;
- require deterministic checks only when those checks exist and are meaningful;
- avoid rules that make emergency/local recovery harder without adding assurance.

HE currently has no CI checks, so requiring nonexistent checks would be meaningless.

## GitHub Actions — TARGETED USE ONLY

PMB and ACR already use Actions for deterministic project checks.

HE has none.

HE should not add CI simply to look mature. A first HE workflow would only be justified if it catches a recurring deterministic defect.

Potential future candidates:

- broken internal repository links/paths;
- duplicate canonical-owner declarations;
- source-manifest path integrity;
- basic Markdown/reference integrity.

Do not automate subjective evidence acceptance.

## Issues — UNDERUSED AS WORK INTENT

HE has a small number of meaningful issues, including research and experiment intent.

This is the right use.

Recommended issue classes:

- defect / observed failure;
- research question;
- experiment;
- decision required.

Avoid turning every note into an issue.

## GitHub Projects — HIGH POTENTIAL

GitHub Projects can combine issues and PRs from multiple repositories and expose table, board, and roadmap views.

This is a better candidate for Nolan's cross-project "what is active / blocked / next?" problem than another memory layer.

### Proposed project

**AI Engineering Portfolio**

Minimum custom fields:

- Status: Backlog / Ready / Active / Blocked / Review / Done
- Repository: HE / PMB / ACR / Other (private work is out of scope unless assessed in its approved environment)
- Work Type: Defect / Feature / Research / Experiment / Maintenance
- Priority: P0 / P1 / P2 / P3
- Evidence State: Unknown / Investigating / Supported / Rejected

Optional later:
- Target date;
- Human decision needed.

### Views

Start with only three:

1. **Now** — board by Status, filtered to Active/Blocked/Review.
2. **Research & Experiments** — table filtered by Work Type.
3. **By Project** — table grouped by Repository.

Do not create a roadmap until dates actually drive decisions.

## Wiki — NO CURRENT VALUE FOR HE

HE already has canonical repository documents.

An unused GitHub Wiki creates a second potential documentation authority.

Recommendation: disable HE Wiki unless a concrete reader-facing use case emerges.

## Discussions — NO CURRENT NEED

No demonstrated collaboration problem requires Discussions.

## Pages — PARK

A public HE reference site could eventually be useful, especially if HE becomes a public portfolio artifact.

It is not currently an engineering need.

## Releases / tags — PROJECT-SPECIFIC

PMB and ACR benefit from versioned releases because they ship runnable capabilities.

HE is primarily a research/decision corpus; release ceremony is unnecessary unless it begins publishing stable reference snapshots.

---

# 3. GitHub operating model

The recommended ownership model is:

```text
Issue
= work intent / question / defect / experiment

Branch
= isolated mutation

Pull Request
= proposed change + evidence + review boundary

Main
= accepted repository truth

Project
= cross-repository work visibility

Action
= deterministic machine check

Release/tag
= stable distributable milestone
```

This avoids using one artifact for multiple meanings.

---

# 4. Repository guidance strategy

GitHub now distinguishes:

- always-on repository instructions;
- reusable prompt files;
- specialist custom agents.

HE should preserve the same conceptual distinction even across non-GitHub hosts.

## Always-on guidance should contain only invariants

Examples:

- evidence before architecture;
- distinguish source evidence from inference;
- do not merge/publish without authorization;
- preserve existing canonical ownership;
- do not create new files when an existing owner should be updated.

Do not put full research procedures into always-loaded guidance.

## Reusable procedures belong in prompts/skills

Examples:

- source mining;
- opposition review;
- branch cleanup audit;
- source-preservation pass.

## Specialist behavior belongs in agent role contracts only when measured

Do not create a custom agent to save five lines of prompt text.

---

# 5. HE Specialist Bench hypothesis

## Hypothesis

For selected HE research tasks, a small set of role-bounded fresh-context specialists may improve evidence quality and reduce cross-role contamination enough to justify their additional execution cost.

The hypothesis is **not** that more agents are better.

---

# 6. Bench design

Initial bench:

| Specialist | Primary job | Mutation authority | Acceptance authority |
|---|---|---|---|
| Research Scout | Discover and qualify sources | None | None |
| Evidence Miner | Deep source inspection and synthesis | Draft only | None |
| Opposition Reviewer | Try to disprove findings | None | None |
| Documentation Integrator | Place accepted findings in canonical owners | Branch/PR proposal | None |

## Coordinator

**Human/main session initially.**

Do not add a coordinator agent in the first experiment.

If manually choosing the correct specialist proves burdensome or error-prone, routing itself can become a later treatment.

This prevents the experiment from bundling specialization and routing into one variable.

---

# 7. Role contracts

## Research Scout

Input:
- research question;
- allowed source scope;
- freshness constraints.

Output:
- candidate sources;
- primary/secondary classification;
- source identity/version where possible;
- why each source may answer the question;
- unresolved provenance gaps.

Stop when:
- the source pack is sufficient for a deeper evidence pass;
- no credible source is found;
- the question needs narrowing.

Must not:
- write HE conclusions;
- modify repository files;
- treat popularity as evidence.

## Evidence Miner

Input:
- approved source pack;
- exact question;
- existing HE relationships if supplied.

Output:
- source-established facts;
- inferred mechanisms clearly separated;
- contradictions/limits;
- corpus relationships;
- candidate disposition.

Must not:
- silently import source claims as HE truth;
- create implementation commitments;
- accept its own findings.

## Opposition Reviewer

Input:
- candidate findings;
- original source pack;
- evaluation questions.

Output:
- confirmed support;
- weakened claims;
- unsupported claims;
- alternative interpretations;
- duplicated/non-independent evidence;
- governance/complexity concerns;
- unresolved points.

Must operate in a fresh context where practical.

Must not:
- repair the producer's text while evaluating it;
- assume agreement is correctness.

## Documentation Integrator

Input:
- accepted findings only;
- canonical repository tree;
- provenance requirements.

Output:
- proposed file updates;
- ownership rationale;
- links/relationships;
- explicit non-adoptions;
- PR-ready change set.

Must:
- update an existing canonical owner when one exists;
- create a new artifact only when no existing owner fits;
- preserve evidence boundaries.

Must not:
- merge;
- convert a candidate into a decision without human authorization.

---

# 8. Experiment design

## Primary question

Does a role-bounded specialist bench produce **better accepted HE findings per unit cost** than the current strong single-agent workflow?

## Experimental controls

Make the specialist roles an **experimental treatment**, not a default workflow. Pre-register the hypotheses, assignment, scoring, stopping rules, and disqualifying regressions before held-out evaluation. Pair or randomize order on representative tasks to reduce model/runtime/time drift, and use an evaluator who cannot tell which arm produced an output where practical.

Hold constant where practical:

- source set;
- research question;
- model;
- model effort/reasoning configuration;
- harness/client;
- repository commit;
- tool access;
- time budget.

Do not introduce model routing in the first comparison. That would confound role architecture with model capability.

## Control condition

```text
one strong agent
  → discover
  → inspect
  → synthesize
  → opposition pass in fresh session
  → propose documentation
```

This reflects the current successful HE pattern rather than a deliberately weak baseline.

## Treatment condition

```text
Research Scout
      ↓ source pack
Evidence Miner
      ↓ candidate findings
fresh Opposition Reviewer
      ↓ adjudication input
human acceptance
      ↓
Documentation Integrator
```

No automatic coordinator.

## Task corpus

Use at least three representative tasks:

1. narrow source verification;
2. video + repository evidence mining;
3. multi-source synthesis with possible duplicate/non-independent evidence.

Prefer prospective/held-out sources. Do not choose tasks because the proposed specialists have already succeeded on them. Blind acceptance reviewers to treatment/control provenance where practical.

If a historical task is used, prevent **both** control and treatment agents, and their evaluator, from seeing any accepted HE answer before scoring. Treat documents created by the same presenter/interview as dependent evidence and count them only once when assessing corroboration.

## Metrics

### Correctness / evidence

- unsupported factual claims;
- source/inference misclassification;
- missed contradictory evidence;
- inaccurate source attribution;
- duplicate evidence counted as independent;
- human corrections required.

### Usefulness

- accepted durable findings;
- rejected/noise findings;
- findings that changed a decision or prevented a bad one;
- useful counterexamples surfaced.

### Cost / execution

- input/output tokens where available;
- provider cost where measurable;
- wall time;
- tool calls;
- repeated reads;
- retries;
- number of model invocations.

### Harness behavior

- routing errors;
- unnecessary specialist invocation;
- information lost at handoff;
- duplicated work;
- context contamination;
- unauthorized mutation attempts.

## Primary comparison metric

Do not optimize for token count alone. Define the independent human acceptance rubric **before running the held-out comparison**. Calibration cases may establish which cost/quality indicators can actually be measured and their variance; once settled, freeze the rubric, material-error definition, weighting (if any), scoring procedure, and primary comparison direction before exposing the held-out tasks to either arm.

Primary decision measure: **evidence-bearing findings accepted without material correction, relative to measured total execution cost and human correction effort**, reported alongside the raw numerator, denominator, uncertainty, and per-case error categories. A single aggregate ratio can hide high-consequence failures: report fabricated evidence, authority mistakes, missed counterevidence and privacy violations separately with non-negotiable no-go boundaries.

Do not infer statistically significant superiority from a three-task demonstration. Treat the minimum corpus as an **exploratory pilot**, and require additional diverse, blinded/scored replications before persistent specialist admission.

---

# 9. Admission / retirement rules

A specialist is retained only if it demonstrates a recurring contribution.

Examples:

- Opposition Reviewer repeatedly catches material errors -> retain.
- Research Scout saves little context/time and duplicates Miner work -> merge/remove.
- Documentation Integrator repeatedly prevents misplaced files -> retain.
- Automated router introduces collisions -> reject or simplify.

Every specialist should have:

- a demonstrated problem it solves;
- a representative evaluation case;
- a retirement test after meaningful model/harness changes.

---

# 10. GitHub-specific implementation path — only after experiment success

If the role contracts prove useful, host adapters can be tested.

Possible GitHub adapter:

```text
.github/
  agents/
    research-scout.agent.md
    evidence-miner.agent.md
    opposition-reviewer.agent.md
    documentation-integrator.agent.md
```

But this is **not** the canonical architecture.

Equivalent adapters could exist for Claude Code or Codex.

The portable contract is the role behavior and authority boundary, not the filename.

---

# 11. Recommended GitHub improvement sequence

## P0 — Inventory and hygiene

1. Audit stale/merged branches before deletion.
2. Enable delete-head-branch-on-merge where safe.
3. Review protection on HE `main` using current, effective GitHub rule evidence.
4. Disable HE Wiki unless there is a concrete use.
5. Keep current PRs as the evidence boundary; do not bypass them for convenience.

## P1 — Cross-repository visibility

Create **AI Engineering Portfolio** GitHub Project with the five minimal fields and three initial views defined above.

Populate it from existing active issues/PRs rather than creating duplicate draft tasks.

## P1 — HE work intent

Use HE issues for actual experiments/questions, not documentation copies.

Reuse [HE Issue #14](https://github.com/unyieldingclaw-dev/harness-engineering/issues/14) for the Specialist Bench experiment; do not create a duplicate issue.

## P2 — Deterministic checks

Only add HE Actions after identifying a recurring machine-checkable defect.

## P2 — Agent adapters

Run the Specialist Bench experiment first.

If the roles earn their keep, create the smallest host-specific adapters.

## PARK

- GitHub Wiki content;
- Discussions;
- HE Pages site;
- agent dashboard;
- automatic agent router;
- large specialist roster;
- synthetic agent utilization tracking.

---

# 12. Decisions explicitly not made

This assessment does not authorize:

- deleting any existing branch;
- changing branch protection;
- enabling/disabling repository features;
- creating GitHub Actions;
- installing GitHub Copilot;
- adding `.github/agents/`;
- changing PMB or ACR architecture;
- automatic routing or autonomous multi-agent execution.

Those are separate reversible changes and should be made only after their specific preconditions are satisfied.

---

# 13. Bottom line

GitHub can provide more value to the engineering workflow without becoming another governance system.

The highest-value **candidate** next moves, subject to current-state verification and independent prioritization, are:

1. treat Issues/PRs/Projects/Actions as distinct work surfaces with one meaning each;
2. clean branch lifecycle rather than adding more repository machinery;
3. evaluate whether one cross-project GitHub Project reduces observed visibility friction before configuring another project-management surface;
4. test four specialist role contracts before building an "agent army";
5. keep the human/main session as the initial coordinator;
6. retain only specialist roles that outperform the current workflow on evidence quality per total cost.

The HE-compatible interpretation is:

> **Use GitHub to make work durable and navigable. Use agents to create independent information value. Add neither repository ceremony nor agent count without evidence.**
