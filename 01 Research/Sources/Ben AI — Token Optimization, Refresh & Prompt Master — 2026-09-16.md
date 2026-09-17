# Ben AI — Token Optimization, Refresh & Prompt Master — 2026-09-16

## Purpose

Source review for Harness Engineering of:

- Ben AI — *Paste This Into Claude, Never Hit a Token Limit Again*
- Video: https://www.youtube.com/watch?v=Jr-jyTL2MYI
- Resource page: https://benai.notion.site/Token-Optimization-3d71124570fe8096bef6e530c782a10d
- Downloaded skills supplied for direct inspection:
  - `refresh-skill.zip`
  - `interview-me.zip`
  - `prompt-master.zip`

This artifact records source evidence and HE-relevant findings only. The downloaded skills were inspected statically; they were not installed or executed.

### Source fingerprints

- `refresh-skill.zip` SHA-256: `fbfa45e6bb0e47c3eaffa4088d80d58e7fef2ef86d9be7dc5ac2624008fff958`
- `interview-me.zip` SHA-256: `c242c4df3d3113ea877bc322279711b6fde790068a43cd0aa83460a2f0edad1c`
- `prompt-master.zip` SHA-256: `746cfc9693abc419a3219f8fa715e06e4acaf11bbedad6ad668858ccaf46b000`

---

# Executive finding

**Mine selectively.**

The clickbait headline is not the useful part. The strongest reusable ideas are:

- observe actual context-window pressure rather than guessing from cumulative token spend;
- prefer a fresh-session handoff over indefinite compaction when durable state already exists outside the conversation;
- make handoffs destination-aware and verify that referenced artifacts will actually be reachable in the successor session;
- read before asking clarification questions;
- ask one high-value question at a time when ambiguity materially changes the work;
- distinguish generic verification rituals from evidence-directed verification;
- keep hard rules for expensive mistakes, but explain the reason and avoid unnecessary emphasis;
- cap scope, deliverable size, and report-back size for long-running tasks.

Several parts should **not** be copied into PMB/HE as-is:

- a universal token or message-count threshold for session rollover;
- automatic file copying/writing during a handoff without prior approval;
- broad claims that all Claude 5 models need no explicit verification;
- self-modifying skills that rewrite their own instructions from a single correction or praise event;
- self-evaluation by the same skill and same model family as sufficient evidence of quality.

---

# 1. Token-limit framing: useful problem, overstated headline

The resource page correctly identifies real cost/context mechanics:

- long sessions repeatedly resend conversation state;
- unrelated work should not accumulate in one session;
- tool/MCP/instruction overhead contributes to startup context;
- subagents can isolate verbose operations;
- model choice, effort, prompt caching, and context management affect cost.

However, the headline **"never hit a token limit again"** is not a literal engineering claim. Context windows, subscription/rate limits, and provider quotas remain hard constraints.

The video also uses an informal "dumb zone" / several-hundred-thousand-token framing. No universal threshold is established by the source.

### Stronger signal available from Claude Code

Current Claude Code status-line data exposes direct session measurements including:

- `context_window.context_window_size`;
- `context_window.used_percentage`;
- `context_window.remaining_percentage`;
- current input/output/cache token components;
- rate-limit usage;
- prompt-cache state.

Anthropic documentation:
https://code.claude.com/docs/en/statusline

This is a better basis for HE/PMB research than cumulative session token spend or a fixed 300K/500K rule.

### HE implication

**Context pressure is a runtime state, not a universal token-count threshold.**

Measure the host-reported context occupancy when available. Use it as a signal for human-visible session management rather than automatically assuming quality failure at a fixed absolute token count.

---

# 2. Refresh skill — direct inspection

Source file:
`refresh/SKILL.md` version `0.5.0`

The skill's stated goal is to start a clean Claude session with only the context the next task needs rather than using `/compact` indefinitely.

## What it actually does

The implementation is more thoughtful than the video summary.

### Destination-oriented handoff

It asks one primary question:

> What is the goal of your next session?

The successor goal is used to determine what context should be transferred rather than simply summarizing the entire current conversation.

### Three transfer levels

- `lite` — goal, next three steps, open decisions, bare paths;
- `full` — adds completed work, key decisions, and why each referenced file matters;
- `ultra` — adds dead ends, data/log paths, and suggested skills.

### Reachability/persistence check

Before emitting a file reference, Refresh distinguishes:

- persistent local disk;
- remote/shared sources;
- ephemeral sandbox or chat-only data.

It also distinguishes a handoff that **stays here** from one that **travels** to another machine/person/context.

For traveling handoffs it rejects sender-local absolute paths and prefers:

- repo-relative/shared identifiers;
- shared URLs/remote ids;
- embedded content when necessary;
- bundled files when local-only material must travel.

This is a strong, concrete handoff design pattern.

### Dedupe rule

For same-environment continuation it drops always-loaded context such as root `CLAUDE.md` / `AGENTS.md` from the handoff because the successor will receive that context automatically.

This is progressive disclosure applied to session transfer.

### Output shape

The generated prompt has bounded sections for:

- goal/location;
- current state;
- next steps;
- files to open;
- carried-over data that exists nowhere else;
- dead ends for `ultra`;
- skills for `ultra`.

## What is worth mining

**REINFORCE:** A handoff should be shaped by the successor's goal, not by the desire to summarize history.

**REINFORCE:** Every pointer in a handoff needs a reachability contract. A path is useless if the successor cannot access the same filesystem/source.

**REINFORCE:** Do not duplicate context the successor loads automatically.

**REINFORCE:** Preserve rejected/dead-end approaches only when retrying them is plausible and costly.

**REINFORCE:** Fresh-session continuation is materially different from in-place compaction.

## What should not be copied as-is

### Handoff can mutate the project without prior approval

`Refresh` allows the `Write` tool and directs itself to copy ephemeral/chat-only assets into a persistent project folder when needed, then tell the user what it did.

That is useful in a general assistant but conflicts with HE/PMB's bounded-authority model. A context-transfer operation should not gain implicit authority to modify a project merely because a referenced asset is ephemeral.

**HE correction:** propose the persistence action first when it requires a project write, or embed the minimal transferable state when practical. Handoff generation should not silently expand into project mutation.

### UX rule conflict

The skill says to ask exactly one question, but its destination logic permits a second clarification question when destination is unclear. This is minor but demonstrates why UX slogans should not override correctness.

### Absolute paths are too permissive for repositories

For "stays here," Refresh permits absolute local paths. For a repository-backed project, repo-relative paths are generally more durable across worktrees, machines, and users. Absolute paths should be reserved for genuinely machine-local state.

---

# 3. Relationship between Refresh and PMB Handoff

Refresh is built for a generic conversation where the chat may be the main state store. PMB already has durable project state in `memory-bank/`.

That changes what a good handoff should contain.

## Generic Refresh model

```text
conversation + touched files
        -> synthesize durable + transient state
        -> continuation prompt
        -> fresh session
```

## PMB model

```text
durable project truth (memory-bank/) ----+
                                         +--> fresh session
transient in-flight state (handoff.md) --+
```

PMB should therefore avoid importing Refresh's full re-summarization behavior. Doing so would duplicate authoritative project state and create drift risk.

### What PMB should mine from Refresh

- successor-goal awareness;
- reachability/portability check for handoff references;
- repo-relative paths when possible;
- concise "avoid repeating" evidence for plausible failed paths;
- explicit distinction between durable state and chat-only state;
- fresh-session transfer as a planned context-management mechanism.

### What PMB should keep

- `memory-bank/` remains authoritative for durable decisions, progress, constraints, and next-step rationale;
- `handoff.md` remains narrow and ephemeral;
- a handoff should not become a second Memory Bank.

---

# 4. Interview Me skill — direct inspection

Source:
`interview-me/SKILL.md`

This is a small skill with several strong behaviors.

## Useful mechanics

- **Read before asking.** Do not consume user questions on discoverable facts.
- **One question per turn.** Avoid a wall of simultaneous clarification questions.
- **Maximum 5–7 questions.** Bound discovery overhead.
- **Prioritize questions that change the work.** Audience, decision, existing artifacts, scope, and proof of done outrank curiosity.
- **Push through vague language once.** Ask what "better" means and how it will be observed.
- **Blind-spot pass.** Identify missing information that could materially change the outcome.
- **Play back a concise brief before execution.** Job, why, guardrails, done-means.
- **Skip the interview when the task is already clear.**

Anthropic's field guide does support the core interview pattern: ask one question at a time and prioritize answers that could change the architecture.

Source:
https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns

## Governance concern

The skill says:

> Everything the user does not claim is your call.

That grants more authority than HE should assume. Users may not know which implementation decisions are load-bearing before the investigation begins.

### HE correction

Default model authority should be limited to **routine, reversible choices inside the approved scope**. Material scope, architecture, security, irreversible actions, external side effects, or decisions that change the acceptance criteria still warrant escalation even if the user did not pre-claim them.

---

# 5. Prompt Master — direct inspection

Contents inspected:

- `prompt-master/SKILL.md`
- `prompt-master/EVAL.md`
- `references/rulebook.md`
- `references/retired-instructions.md`
- `references/job-brief.md`
- `references/interview.md`
- `references/file-audit.md`
- `references/rules-to-reasons.md`
- `references/voice-and-format.md`
- example rewrite

## High-value mechanics

### Scope/exit/report-back caps

The Job Brief explicitly separates:

- job;
- why;
- guardrails;
- done-means.

For long runs it caps scope, deliverable size, and report-back size. This is a practical defense against agent overreach and unnecessary output.

### Hard rules are retained where failure is expensive

`rules-to-reasons.md` correctly does **not** recommend softening every rule. It preserves hard constraints for destructive actions, external sends, money, permissions, client-facing work, and legal constraints, while adding the reason when useful.

That aligns with HE's distinction between judgment and deterministic/high-cost boundaries.

### File audit requires approval before editing

For a `CLAUDE.md`, skill, or project instruction file, Prompt Master first classifies instructions and proposes replacements. It explicitly waits for approval before editing.

That is compatible with governed assistance.

### Evidence-grounded progress reporting

Prompt Master's long-run addendum says progress claims should be supported by tool results and that failed/skipped work should be reported faithfully.

This is useful and materially different from ritualistic "double-check everything" prompting.

---

# 6. Verification: the source itself proves the distinction matters

Ben's public rule says to avoid lines such as:

- "double-check your work";
- "verify before responding";
- mandatory verification subagents.

There is real model-specific evidence behind this for **Claude Opus 5**. Anthropic's Opus 5 guidance says explicit re-verification can cause over-verification and wasted tokens because Opus 5 already self-corrects strongly.

Source:
https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5

However, Prompt Master over-generalizes this evidence.

`rulebook.md` states that **Claude 5** verifies its own work, while its cited source is specifically the **Opus 5** guide.

`retired-instructions.md` similarly deletes verification instructions across the broader Claude 5 framing.

Anthropic's general current-model prompting guidance is more nuanced: it recommends explicit self-checking against test criteria for coding/math and then identifies **Opus 5 as the exception** where migrated verification instructions can cause over-verification.

Source:
https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices

Anthropic's current Claude Code cost guidance also recommends giving verification targets and testing incrementally.

Source:
https://code.claude.com/docs/en/costs

## Prompt Master's own internal contradiction is instructive

The same skill that says to strip generic verification also has a rule named:

> **Make it prove it.**

For long-running tasks it appends a mandatory evidence audit to progress claims.

This is not actually contradictory once verification is separated into two categories:

### Ritual verification

- repeat the same reasoning because the prompt says "double-check";
- run a generic verification subagent regardless of task/risk;
- perform another pass with no new evidence source or acceptance criterion.

### Evidence-directed verification

- run the affected tests;
- reproduce the reported failure;
- compare output to explicit acceptance criteria;
- inspect a material security/enforcement boundary;
- use an independent executable check;
- challenge a load-bearing premise with a deliberately separate evaluator;
- audit progress claims against actual tool results.

**HE finding:** remove ritual duplication, not verification itself.

This directly reinforces existing HE research that verification must produce task-relevant evidence and should be risk-directed.

---

# 7. Prompt Master's self-improvement mechanism should be rejected

The skill instructs itself to modify its own files when:

- the user corrects a step;
- a user correction is a "hard rule";
- the user says a prompt/audit was genuinely good;
- Anthropic guidance changes.

It can also save successful examples into its own reference set.

This is a governance hazard when treated as automatic behavior.

A single correction, praise event, or context-specific success does not establish a durable rule. Automatically persisting it creates:

- local overfitting;
- instruction accretion;
- poisoning risk from untrusted/current-task context;
- hidden behavioral drift;
- weak provenance for why a rule became permanent.

### HE correction

Self-improvement observations may be **proposed** as candidates. Durable instruction changes require explicit review, evidence, and ownership. The component that owns a rule should change only when the failure/success is reproducible or otherwise well-supported.

This strongly aligns with HE's existing failure-correction principle: fix the smallest durable owning component only when evidence supports the change.

---

# 8. Prompt Master's eval is not strong evidence

`EVAL.md` asks a fresh session to use three subagents to test whether Prompt Master:

- follows its own steps in order;
- loads the intended reference files;
- removes selected phrases;
- adds a why sentence;
- caps output/report-back.

This is a useful smoke test for **instruction adherence**.

It is not an evaluation of whether the rewritten prompt produces better downstream task outcomes.

Weaknesses:

- the system evaluates itself against its own instructions;
- the subagents do not create independent ground truth;
- the eval tests textual/process properties, not task quality;
- it does not compare cost, correctness, retries, or defect discovery against a baseline;
- the same assumptions that shaped the skill also shape the acceptance test.

### HE implication

Separate:

1. **skill conformance eval** — did the skill execute its intended procedure?;
2. **outcome eval** — did that procedure improve the actual task result?;
3. **cost/context eval** — did it reduce tokens/latency without increasing correction/review cost?

Passing (1) does not establish (2) or (3).

---

# 9. Claude Code context observability is the most actionable PMB finding

PMB currently has a policy-level handoff trigger based on context pressure, but the policy depends on the context percentage being noticed/reported.

Claude Code's status-line API provides a deterministic, zero-model-token display mechanism for `context_window.used_percentage`.

Anthropic states that the status-line script runs locally and does not consume API tokens.

Source:
https://code.claude.com/docs/en/statusline

### Candidate PMB experiment

Do **not** add `300K tokens -> automatic handoff`.

Instead, expose current context occupancy in the status line and make the existing handoff threshold visible, for example:

```text
[Sonnet] project | 38% context
[Sonnet] project | 42% context | handoff candidate
```

This keeps authority with the user/model at a natural task boundary while making the existing policy observable.

Do not automatically terminate the session or create a handoff merely because the percentage crosses a line until pilot evidence shows that behavior is beneficial.

### Why percentage beats absolute token count

Claude Code currently supports different context-window sizes, so `300K` means materially different pressure in a 200K versus 1M context configuration. The host-calculated percentage is normalized to the actual active context-window size.

---

# 10. Compaction versus fresh-session handoff

Anthropic's general prompting guidance explicitly notes that a fresh context window can be preferable to compaction because current models can rediscover durable state from the local filesystem.

Source:
https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices

Anthropic's Claude Code cost guidance also recommends `/clear` between unrelated tasks and custom `/compact` instructions when continuity is required.

Source:
https://code.claude.com/docs/en/costs

### HE interpretation

Do not frame this as **compact bad / refresh good**.

Use the operation that matches the state model:

- unrelated task -> clear/fresh session;
- durable project state already persisted -> fresh session + narrow handoff can be attractive;
- in-flight conversational state not otherwise persisted -> targeted compaction may be appropriate;
- imminent context exhaustion -> compaction remains a recovery/safety mechanism.

PMB has more reason than a generic chat to prefer planned fresh-session rollover because its durable state already lives outside the conversation.

---

# Research disposition

## REINFORCE

- Context pressure should be observable using host/runtime state when possible.
- A fresh-session handoff should be destination-aware and artifact-reachability-aware.
- Durable state and transient handoff state should remain separate.
- Read before asking.
- Clarification should focus on unknowns that materially change the plan.
- Hard rules remain justified where failure is expensive.
- Scope, deliverable, and report-back caps are useful for long-running tasks.
- Verification should be evidence-directed and risk-directed, not ritualized.
- Skill conformance and task-outcome evaluation are different things.

## ASSESS

- Add a lightweight Claude Code status-line context-pressure indicator to PMB and measure whether the existing handoff threshold is useful during controlled sessions.
- Add successor-goal and reachability checks to PMB handoff without re-summarizing durable Memory Bank content.
- Preserve compact dead-end evidence only when retry risk is meaningful.
- Audit PMB instructions for generic duplicate-verification language while preserving test/review/evidence requirements.
- Compare fresh-session handoff vs targeted `/compact` on equivalent long tasks using correctness, retries, token/context cost, and human intervention.

## PARK

- Multiple Refresh fidelity levels in PMB until a real need appears.
- Automatic session rollover at a percentage threshold.
- Portable handoff bundling machinery unless cross-machine/team transfer is an observed PMB use case.

## REJECT

- Universal 300K/500K "dumb zone" thresholds.
- "Never verify" as a Harness rule.
- Generalizing Opus 5 over-verification guidance to every model/workflow without eval evidence.
- Handoff operations that silently gain authority to write/copy project files.
- Automatic self-modification of skills from one correction or praise event.
- Same-model/self-referential skill evals as proof of downstream quality.

---

# Overall assessment

**Refresh:** high research value; several mechanisms are worth mining, but PMB's durable-state architecture is stronger than Refresh's generic re-summarization model.

**Interview Me:** high value as a bounded clarification pattern; constrain model authority more tightly than the source does.

**Prompt Master:** mixed. Strong scope, reason, evidence, and audit mechanics; weak generalization around verification and unacceptable automatic self-modification for a governed harness.

**Highest-value PMB candidate:** make context pressure visible using Claude Code's real `context_window.used_percentage`, then test the existing handoff policy rather than inventing an absolute token threshold.
