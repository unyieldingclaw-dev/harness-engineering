# Claude Academy AI-Native SDLC — First-Party Deep Pass — 2026-09-22

## Source set

Primary source material:

- Claude Academy — **The AI-Native SDLC Playbook** (14 lessons), shown directly in the user-supplied screenshots.
- User-supplied transcript of **Claude Code: The Complete AI-Native SDLC Guide** by Eric Tech (2026-09-22), used as commentary/discovery only.
- Existing installed capability noted by the user: **Superpowers** has been part of the working setup since day one.

Related HE research:

- `01 Research/Sources/Anthropic AI-Native SDLC & Molten OS — 2026-09-01.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`

This note distinguishes first-party Claude Academy guidance from the video's own interpretation and from Superpowers-specific practices.

---

# 1. Core thesis: build time compresses; judgment and control become more important

Claude Academy frames the AI-native SDLC around a changed bottleneck. Agentic implementation can dramatically compress the build stage, so proportionally more engineering attention moves to:

- requirements and intent;
- design and specification;
- review;
- verification;
- release controls;
- deployment safety;
- maintenance and feedback.

The useful HE lesson is not "agents make software fast." It is:

> **When implementation accelerates, the cost of weak intent, weak verification, and weak release controls becomes more visible.**

**Disposition: REINFORCE.**

---

# 2. The artifact chain is the durable mechanism

The course uses a staged artifact chain:

```text
intent
  ↓
specification
  ↓
implementation plan
  ↓
code + tests
  ↓
review result
  ↓
release/deployment state
  ↓
observed production outcome
  ↓
new intent when needed
```

The important mechanism is not the literal filenames. The important property is that one accepted artifact becomes the bounded input to the next stage.

This reduces conversational coupling and makes stage transitions inspectable.

### HE candidate

> **Accepted artifacts should define stage transitions.**

A downstream stage should not need to reconstruct the upstream decision process from chat history when the accepted artifact already contains the required truth.

**Disposition: REINFORCE; candidate principle.**

---

# 3. One source of truth per artifact

Claude Academy explicitly warns against maintaining competing truths across the repository and external trackers. The pattern is:

- choose where the authoritative artifact lives;
- link other systems to it;
- do not make both copies authoritative.

This strongly corroborates recent HE findings around local authority and source ownership.

### HE candidate

> **One fact should have one owning source; linked systems may reference it but should not silently become competing authorities.**

Examples:

- intent may live in-repo or in a ticketing system;
- task authorization may live in a PMB contract;
- implementation plan may live in Superpowers' plan artifact;
- Git owns branch/commit state;
- CI owns check status.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Do not copy the file layout blindly

The course demonstrates `intent.md`, `spec.md`, `plan.md`, and `REVIEW.md`, but those are implementation artifacts, not universal requirements.

The user's existing setup already overlaps heavily:

- Superpowers covers brainstorming/shared understanding, design/specification, implementation planning, TDD, and verification workflows.
- PMB owns durable project context, task scope/authorization, handoff, hooks, and governance.
- PMB/ACR review tooling provides independent semantic review and opposition/evidence checks.

Adding a second `intent/spec/plan` stack beside Superpowers would create duplicate authority rather than useful structure.

**Disposition: REJECT duplicate artifact hierarchy.**

---

# 5. Distinguish intent, design, plan, and authorization

The course's artifact separation is useful because these artifacts answer different questions:

- **Intent:** why this change exists, desired outcome, affected users/systems, constraints, open questions.
- **Specification/design:** what behavior/system should exist.
- **Implementation plan:** how the change will be executed and proven.
- **Execution authorization:** what the current worker/session may actually change.

PMB task contracts belong in the fourth category. They should not absorb the first three.

### HE implication

Do not collapse semantic intent, technical design, execution strategy, and permission into one giant artifact merely because one tool can store them all.

**Disposition: REINFORCE separation of concerns.**

---

# 6. Producer feedback loop and independent verification are different controls

Claude Academy explicitly distinguishes two mechanisms.

## Producer feedback loop

During implementation, the same session continuously checks its work using available deterministic or observable signals such as:

- tests;
- lint;
- build;
- screenshots / visual diffs;
- other task-specific checks.

The implementation loop fixes failures before presenting the work as complete.

## Independent verifier

After implementation, a fresh verifier checks the completed result from a new context rather than inheriting the producer's assumptions.

### HE candidate

> **Producer self-checking and independent verification solve different failure modes.**

Self-checking cheaply catches ordinary implementation errors. Independent verification challenges the premise and final state.

This aligns with existing HE work separating execution success from effect verification and with PMB's isolated review/opposition design.

**Disposition: STRONGLY REINFORCE.**

---

# 7. "Green" must be defined externally

A screenshot emphasizes a subtle but strong rule: verification commands should include the expected result. A bare command leaves the agent to decide what passing means.

Example pattern:

```text
make test  → expected: 0 failed
make lint  → expected: no findings / defined acceptable result
make build → expected: successful build marker / exit state
```

### HE candidate

> **Verification should define the observable postcondition, not merely name the command to run.**

This is a practical extension of effect verification: a check without acceptance semantics is weaker than it appears.

**Disposition: REINFORCE.**

---

# 8. Eric's TDD interpretation is stricter than the Academy's general feedback-loop guidance

The video describes the Claude Academy playbook as requiring TDD, then demonstrates Superpowers' strict test-first workflow.

The screenshots of Claude Academy emphasize a broader requirement: give Claude a feedback loop and a way to verify its work. Superpowers' TDD skill adds a stronger rule: write the test first, watch it fail, then implement minimally to make it pass.

These should not be conflated.

### HE implication

- **Feedback-loop requirement:** broadly useful across task types.
- **Strict TDD:** use where the task/domain benefits and the team's chosen workflow requires it.

Do not promote a stricter Superpowers rule into universal Anthropic doctrine.

**Disposition: PRESERVE SOURCE DISTINCTION.**

---

# 9. Behavioral harness evals are the strongest new operational implication

Claude Academy recommends collecting approximately 20–50 **real previously solved tasks** with accepted outcomes, then rerunning them when behavior-shaping configuration changes, including changes to:

- `CLAUDE.md`;
- skills;
- hooks;
- model/configuration choices.

The source explicitly says not to invent the corpus: previously solved work provides evidence of known behavior. Incidents should become durable regression cases.

### HE candidate

> **Harness configuration is executable behavior and should have behavioral regression tests.**

This is different from normal structural CI.

Structural CI answers:

> Is the harness syntactically and mechanically valid?

Behavioral evals answer:

> Does the changed harness still cause the agent to behave acceptably on work we already understand?

### PMB relevance

PMB already has substantial deterministic CI and hook testing. A future behavioral eval corpus could test real effects of changes to instructions, skills, routing, handoff behavior, and governance.

Do not manufacture 20–50 synthetic tasks simply to match the course. Start from real pilot/production work and grow the corpus only when a task or incident earns permanent regression value.

**Disposition: HIGH-VALUE ASSESS after PMB pilot.**

---

# 10. PR review should spend probabilistic capacity where deterministic checks cannot

Claude Academy's review material separates review passes and explicitly allows teams to define what not to report, including generated files and findings CI already enforces.

### HE candidate

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

Use CI/hooks/linters for reliably machine-checkable properties. Use LLM review for properties such as:

- correctness reasoning;
- subtle edge cases;
- security reasoning;
- spec/intent compliance;
- architecture drift;
- cross-file behavior;
- residual risk.

This is especially relevant to ACR, where review noise and unsupported findings are measurable failure modes.

**Disposition: STRONGLY REINFORCE.**

---

# 11. Hooks and human approval gates belong at consequential boundaries

The course distinguishes advisory skills from deterministic hooks and demonstrates hooks used for:

- plan updates;
- blocking restricted edits;
- running lint/checks;
- blocking credentials;
- pausing production actions until a human authorizes them.

This strongly matches PMB's layered governance direction.

### HE implication

> Put hard controls at the action boundary they govern; do not rely on upstream prose to carry authority across an entire autonomous run.

**Disposition: STRONGLY REINFORCE.**

---

# 12. Deployment autonomy should be tiered, not binary

Claude Academy's deployment guidance is more bounded than the video summary implies. The course shows:

- headless Claude Code in CI;
- sandboxed execution;
- limited access;
- ordinary PR/test/review controls for fixes;
- deployment exposed through explicit tools;
- production deployment blocked until authorized;
- status and rollback tools available after release;
- rollback paths tested before they are needed.

This independently corroborates the bounded-execution work captured elsewhere in HE.

### HE candidate

> **More autonomy requires clearer authority boundaries, not broader implicit permission.**

**Disposition: STRONGLY REINFORCE.**

---

# 13. Metrics close the loop, but the source does not require a particular observability vendor

The course's final stage uses production/system metrics to detect degradation and turn evidence back into diagnosis or new intent. A sample `bands.yaml` demonstrates baseline-driven thresholds and escalating actions.

The durable mechanism is:

```text
observable metric
    ↓
baseline / expected band
    ↓
meaningful deviation
    ↓
bounded diagnostic or escalation action
    ↓
verified correction or new work item
```

The source demonstrates the concept with operational telemetry; it does not establish that every project needs Datadog, Sentry, or another external observability platform.

### HE implication

Metrics are valuable only when they have:

- an owning source;
- a baseline or acceptance meaning;
- freshness/provenance;
- an action boundary;
- a reason the metric changes a decision.

A metric that no decision consumes is telemetry inventory, not engineering leverage.

**Disposition: REINFORCE metrics-with-semantics; REJECT tool-first observability.**

---

# 14. Cross-project implications

## Harness Engineering

Strong additions/reinforcements:

- accepted artifacts as stage boundaries;
- one owner/source of truth per artifact;
- producer feedback loop vs independent verifier;
- externally defined verification postconditions;
- behavioral harness evals from real solved tasks;
- deterministic checks should absorb deterministic review work;
- tiered autonomy with explicit consequential-action gates;
- metrics tied to baselines and decisions rather than collected for their own sake.

## PMB

Do not add a parallel `intent/spec/plan` hierarchy while Superpowers already covers those stages.

Potential post-pilot assessment:

- build a small behavioral eval corpus from real solved tasks/incidents;
- verify that PMB configuration changes preserve desired agent behavior;
- keep task contracts scoped to authorization rather than absorbing intent/design/plan;
- prefer deterministic measurement from CLI/hooks/CI over model-written telemetry.

## ACR

Relevant directions:

- reduce review work that deterministic tooling can own;
- benchmark reviewer behavior against real accepted/rejected findings;
- preserve known incidents/failures as regression fixtures;
- distinguish reviewer execution from independent acceptance/verification.

## Dashboard / Cockpit

The course strengthens the case for a future read-only observer of source-owned metrics and execution state. It does not justify making the dashboard an authority or building it before a real consumer/use case exists.

---

# Candidate HE principles — research status

These deserve future corroboration/counterexample testing before promotion to formal HE doctrine:

> **Accepted artifacts should define stage transitions.**

> **One fact should have one owning source; linked systems may reference it but should not silently become competing authorities.**

> **Producer self-checking and independent verification solve different failure modes.**

> **Verification should define the observable postcondition, not merely name the command to run.**

> **Harness configuration is executable behavior and should have behavioral regression tests.**

> **Do not spend probabilistic review capacity rediscovering deterministic failures.**

> **More autonomy requires clearer authority boundaries, not broader implicit permission.**

---

# Bottom line

Claude Academy's playbook is high-value first-party evidence, but most of its value is in the mechanisms rather than the literal file names.

The strongest new operational direction for the user's current stack is **behavioral regression testing of harness configuration using real previously solved work**.

The strongest architectural direction is an **artifact-gated lifecycle with one owner per fact and explicit verification semantics**.

The correct response is to integrate those mechanisms with Superpowers + PMB + ACR, not to create a second SDLC artifact system.