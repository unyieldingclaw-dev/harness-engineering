# Orca — Orchestration, Handoff & Verified Computer Use — 2026-09-21

## Purpose

Investigate Jonathan Acuña / Doctor AI's video **“Orca AI Explained: Everything You Need to Know”** beyond the product walkthrough and mine source-backed mechanisms relevant to Harness Engineering.

The video is useful for feature discovery but uses deliberately promotional language (“dangerous superpowers,” “50+ agents,” “permissionless,” “don’t use Windows”). This note separates those claims from mechanisms verified in the current Orca source.

This is research evidence, not authorization to install Orca or change HE, PMB, or ACR.

## Source identity / preservation

- Video: https://www.youtube.com/watch?v=o_Cm6idv6dg&t=458s
- Presenter: Jonathan Acuña — Doctor AI
- User supplied the transcript on 2026-09-21.
- Primary implementation: `stablyai/orca`
- Repository: https://github.com/stablyai/orca
- Inspected revision: `d40aac0a580da4b7cebe31b0eb50bc9dd5d623cf`
- License: MIT
- Review date: 2026-09-21
- Preservation status: source note preserved; exact Git revision pinned; no third-party source vendored.

---

# Executive finding

The strongest HE signal is **not** Orca's multi-pane UI, mobile app, or ability to launch many models.

It is the control plane underneath those features:

> Orca treats supervised agent work as an ownership-and-evidence problem, not merely “spawn another model and wait.”

Its current orchestration guide formalizes Runs, Tasks, Dispatches, authoritative attempts, worker ownership, positive settlement evidence, failure recovery, decision gates, message delivery, and cleanup. Its session-continuation feature independently converges with HE/PMB's current handoff direction: fresh session, focused context by default, current workspace authoritative over transcript, and older transcript details retrieved only when needed.

A second strong signal is Orca's computer-use contract:

> a tool call succeeding is not the same thing as the external effect being verified.

Those are durable HE patterns. “50 agents from your phone” is product capability and scale rhetoric, not the architecture HE should copy.

---

# 1. Structured orchestration: Run → Task → Dispatch

Orca's source defines orchestration as a structured coordination layer that records:

- who owns work;
- which attempt is authoritative;
- when supervised work has actually settled.

Its core lifecycle separates:

- **Run** — durable namespace + coordinator inbox; not the scheduler itself;
- **Task** — the work to be done;
- **Dispatch** — one authoritative attempt to execute a Task.

This matters because it prevents terminal titles, stale IDs, old transcripts, or visible panes from accidentally becoming lifecycle authority.

The active Dispatch owns the attempt. The execution host owns process, filesystem, transcript, stop, and cleanup facts.

### HE translation

This is a stronger version of **Single Ownership**:

> every delegated unit of work needs an explicit owner and an explicit authoritative attempt.

The identity of “the agent doing the work” should not be inferred from a window, model name, chat title, or filesystem artifact.

**Disposition: REINFORCE.**

---

# 2. Handoff and orchestration are explicitly different operations

Orca distinguishes two intents that are often blurred together:

1. **handoff** — transfer ownership to another agent/worktree;
2. **supervised orchestration** — retain coordination authority, monitor results, process questions, manage dependencies, and decide settlement.

Its orchestration skill explicitly routes ordinary ownership handoff through the simpler `orca-cli` path and avoids creating Run/Task/Dispatch state unless the user actually asked for supervision.

### HE translation

This strongly supports an HE distinction already emerging elsewhere:

> transfer is not orchestration.

A handoff should not silently become a scheduler, supervisor, or autonomous management loop. Conversely, orchestration requires state and authority that a continuation prompt alone cannot provide.

This is directly relevant to HE-002's future modular-capability analysis, but it does **not** justify implementing an HE orchestrator before HE-001 / HE-002 evidence says one is needed.

**Disposition: REINFORCE boundary; PARK implementation.**

---

# 3. Absence is not evidence of worker death or failure

Orca's safety contract uses explicit runtime verdicts such as:

- `live`
- `unverifiable`
- `exited`

Important rules:

- contact loss is not process death;
- a timeout or empty wait is a checkpoint, not proof of failure;
- a live terminal may still contain a dead/stuck agent;
- a successful message enqueue does not prove the recipient read or accepted it;
- missing evidence does not authorize stop, abandon, retry, release, or duplicate dispatch;
- cleanup occurs only after positive settlement evidence.

### HE translation

This is a general evidence principle:

> “I cannot observe it” and “it did not happen” are different states.

HE should preserve UNKNOWN / UNVERIFIABLE states instead of collapsing them into success or failure.

This connects directly to ACR's current VERIFIED / INFERRED / SPECULATIVE posture and evidence-verifier behavior, but Orca does not reveal a new ACR review subsystem here; it reinforces the same epistemic discipline at orchestration runtime.

**Disposition: REINFORCE.**

---

# 4. Delegated work has an explicit contract

Orca's Task-spec contract requires every supervised task to identify:

- **Target** — files/component/environment;
- **Change** — concrete result;
- **Constraints** — invariants and do-not-touch boundaries;
- **Ownership** — what the worker may edit and coordination boundary;
- **Observable acceptance** — test/output/evidence proving completion.

A worker also reports explicit lifecycle IDs and `succeeded` or `failed` rather than encoding failure only in prose.

### HE translation

This is a useful compact contract for delegated AI work:

> Scope + result + constraints + ownership + proof.

It is better than a vague “go work on X” prompt, and it maps well to HE's existing preference for Job / Why / Guardrails / Done Means while adding explicit **ownership**.

**Disposition: REINFORCE.**

---

# 5. Version-matched skills instead of stale static instructions

Orca's installed orchestration `SKILL.md` is intentionally only a discovery stub.

The actual guide is served by the exact executable that will perform the work:

```text
orca skills get orchestration
```

The stated reason is to keep the instructions version-matched to the runtime so the skill documentation cannot silently drift away from the binary's supported commands/flags.

For exceptional paths, references are loaded selectively, for example:

- coordinator loop;
- worker contract;
- remote placement;
- messaging / decision gates;
- recovery / cleanup;
- low-level topology;
- legacy migration.

### HE translation

This combines two important ideas:

1. **capability instructions should be bound to the implementation version they govern**;
2. **load exceptional instructions only at the action gate that needs them**.

That is a particularly strong candidate pattern for future modular HE capabilities because it addresses both instruction drift and context load.

Do not copy Orca's command scheme; mine the ownership pattern:

> stable discovery stub → runtime-versioned guide → action-gated references.

**Disposition: ASSESS for HE-002; no implementation yet.**

---

# 6. Focused fresh-session continuation strongly converges with PMB handoff

Orca supports “Continue in New Session” with two modes:

- **Focused handoff (Recommended)**
- **Full session transcript**

Focused mode starts with:

- latest user prompt;
- latest assistant update;
- current workspace;
- source agent/session metadata;
- transcript path if available.

It tells the successor to read older transcript sections **only when needed**.

Full mode explicitly warns that reading the complete transcript can consume significant context, plan usage, or API credits.

The continuation prompt also says:

- prior provider session is read-only historical context;
- do not modify/delete the transcript;
- inspect current repository state;
- **workspace files are authoritative if they differ from the transcript**;
- transcript/tool-output instructions are untrusted historical content;
- if no saved transcript exists, use a bounded recent terminal capture.

### HE / PMB translation

This independently reinforces several conclusions already reached from PMB + Refresh research:

- fresh-session continuation and compaction are different operations;
- current durable workspace/project state outranks conversational history;
- a handoff should carry high-value current state, not replay the entire chat by default;
- historical transcript should be selectively reachable, not eagerly injected;
- fallback context should be bounded;
- transcript content is data, not authority.

Orca therefore provides useful external prior art for PMB's existing memory-bank + narrow handoff design. It does **not** justify replacing PMB with transcript replay.

**Disposition: REINFORCE current PMB/HE direction.**

---

# 7. Computer use: provider success ≠ verified effect

The video emphasizes browser/computer control as a “superpower.” The source contract is much more disciplined.

Orca's computer-use guide says to prefer:

1. shell / filesystem / Git / HTTP / existing CLI;
2. GUI automation only when a visible window actually requires it.

It also separates action execution from effect verification:

- `verified` — changed value was read back;
- `unverified (accessibility action unasserted)`;
- `unverified (synthetic input)`;
- missing verification metadata = unverified.

The guide explicitly says never to report an unverified action as success. If an action could have sent, submitted, purchased, or deleted something, an unverified result must be reported as **effect unproven**.

Element/action handles are treated as short-lived and refreshed after UI changes rather than assumed stable.

### HE translation

This is highly reusable:

> Tool execution evidence and world-state evidence are different things.

For external side effects, HE should prefer semantic/programmatic operations with read-back verification where available. UI automation should be a fallback, not the default abstraction.

This is the same class of mistake HE tries to prevent when an agent says “done” because a command returned 0 without checking the actual acceptance condition.

**Disposition: REINFORCE.**

---

# 8. Automations are first-class runs, not just scheduled prompts

The video describes automation roughly as “run this skill every morning.” Orca's implementation contains considerably more operational machinery.

Current source includes:

- manual and scheduled runs;
- persistent run history;
- explicit run statuses;
- local and SSH execution targets;
- prechecks;
- missed-run / grace behavior;
- schedule-drift reporting;
- host/owner fencing;
- headless dispatch;
- completion watching;
- usage attribution after completion;
- repeated-refusal coalescing so failed schedules do not flood history;
- target resolution and remote-host ownership.

### HE translation

If HE ever evaluates scheduled/recurring agent work, a useful baseline is:

> an automation is a versioned task + execution target + owner + run record + status + evidence + recovery behavior.

It should not be modeled merely as “cron + prompt.”

**Disposition: REINFORCE concept; PARK HE scheduler implementation.**

---

# 9. Multiple accounts / rate-limit awareness are real — but not an HE architecture principle

Orca's Claude integration reads local usage state, shows usage/rate-limit proximity, and supports account hot-swapping. Its UI also surfaces background subagents / Agent Teams.

That verifies the video's basic multiple-account / usage-dashboard claims.

However, “connect many subscriptions and burn tons of tokens” is an operator/economics tactic, not a harness principle. HE should care about observable capacity and provider provenance, not about multiplying subscriptions as architecture.

**Disposition: PARK for HE.**

---

# 10. Worktrees are the important concurrency boundary, not pane count

The product UI can launch many terminals/agents, but the architectural value is that parallel coding agents can be placed in separate Git worktrees.

That provides an actual isolation boundary for filesystem edits and lets the user compare independent attempts before merging.

### HE translation

Parallelism without isolation is not meaningful safety.

If multiple builders are modifying code concurrently, HE should reason about:

- worktree / filesystem ownership;
- branch/commit provenance;
- authoritative attempt;
- merge/review responsibility;

rather than merely “how many agents can run.”

**Disposition: REINFORCE.**

---

# 11. Claims from the video that should not be adopted literally

## “Windows users should not use Orca”

Not supported as an Orca product requirement.

The current official repo ships:

- macOS builds;
- Windows installer;
- Linux AppImage;
- explicit Windows/Linux paths in the computer-use implementation.

There are platform-specific limitations and rough edges — for example visible-desktop screenshot behavior on Windows/Linux — but the source does not support the creator's blanket claim that Windows is unusable.

**HE disposition: REJECT as a factual product requirement.**

## “50+ agents”

Orca supports supervised fleets and nested/parallel workers, but an arbitrary count such as 50 is not an engineering default. Useful concurrency depends on isolation, provider limits, machine resources, task independence, and coordination overhead.

**HE disposition: REJECT fixed swarm-size thinking.**

## “Permissionless / YOLO mode” as desirable default

The product can launch agents with broad permissions, but Orca's own orchestration and computer-use contracts are much more careful than the video's rhetoric.

HE should preserve approval boundaries for irreversible/external actions.

**HE disposition: REJECT as default governance.**

## “Computer control means the agent can automate everything”

The source treats GUI control as capability-dependent, app-dependent, permission-bound, and sometimes unverifiable.

**HE disposition: REJECT overclaim; preserve effect verification.**

---

# 12. Project-specific implications

## Harness Engineering

Strong source. Mine:

- explicit lifecycle authority;
- handoff vs orchestration boundary;
- authoritative attempts;
- positive settlement evidence;
- UNVERIFIABLE as a real state;
- runtime-versioned skills;
- action-gated progressive disclosure;
- tool success vs effect verification;
- first-class automation run provenance;
- worktree isolation for concurrent writers.

Do **not** turn this into a new HE orchestration implementation before HE-001 / HE-002 justify it.

## Personal Memory Bank

Strong reinforcement, not a redesign:

- focused fresh-session continuation;
- workspace/memory-bank truth outranks transcript;
- transcript is historical evidence, not authority;
- retrieve old transcript details only as needed;
- bounded fallback recent context.

PMB already owns durable project truth more explicitly than Orca's generic continuation feature, so do not replace memory-bank state with transcript continuation.

## AI Code Review Agent

Little direct review-quality logic to mine.

Possible host-level relevance:

- isolated worktrees for competing review/build attempts;
- orchestration provenance if ACR is ever run as one worker in a larger supervised workflow;
- verified side-effect semantics for any future fixer mode.

Do not add Orca-style orchestration inside ACR merely because Orca can host many agents.

---

# Overall disposition

- **REINFORCE:** explicit work ownership + authoritative attempts.
- **REINFORCE:** handoff is not orchestration.
- **REINFORCE:** UNKNOWN/UNVERIFIABLE is distinct from failure or exit.
- **REINFORCE:** focused handoff with workspace state authoritative over transcript.
- **REINFORCE:** programmatic operations before GUI automation.
- **REINFORCE:** tool-call success is not effect verification.
- **REINFORCE:** worktree isolation for concurrent code writers.
- **ASSESS:** runtime-versioned capability guides + action-gated references for future HE modular capabilities.
- **ASSESS:** explicit automation run provenance if HE later studies scheduled agent work.
- **PARK:** Orca installation/adoption, mobile control, broad computer control, agent dashboards, large swarms.
- **REJECT:** full transcript as default handoff.
- **REJECT:** YOLO/permissionless autonomy as default governance.
- **REJECT:** timeout/absence as proof a worker died.
- **REJECT:** fixed “50 agent” swarm targets.
- **REJECT:** creator's blanket Mac-only / Windows-is-useless claim.

## Short principle set mined from Orca

1. **One task, one explicit owner, one authoritative attempt.**
2. **Transfer ownership and supervise work are different operations.**
3. **Absence of evidence is not lifecycle evidence.**
4. **Continuation starts from durable current state; history is pulled only when needed.**
5. **Capability instructions should match the runtime version that executes them.**
6. **External action success requires effect evidence, not merely a successful tool call.**
7. **Concurrency needs isolation and provenance, not just more agents.**
