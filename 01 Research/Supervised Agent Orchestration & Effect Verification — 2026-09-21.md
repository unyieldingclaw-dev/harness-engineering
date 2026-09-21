# Supervised Agent Orchestration & Effect Verification — 2026-09-21

## Purpose

Translate the durable mechanisms found in the Orca deep-source review into Harness Engineering terms without adopting Orca's product architecture or feature set.

Primary evidence:

- `01 Research/Sources/Orca — Orchestration, Handoff & Verified Computer Use — 2026-09-21.md`
- `stablyai/orca` inspected at commit `d40aac0a580da4b7cebe31b0eb50bc9dd5d623cf`

Related HE research:

- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`

This note is synthesis, not authorization to build an orchestrator, install Orca, change PMB, or change ACR.

---

# Executive synthesis

The useful lesson from Orca is not "run more agents."

It is that multi-agent work becomes reliable only when the harness makes **authority, ownership, evidence, and settlement explicit**.

Five mechanisms are especially durable:

1. one delegated unit has one explicit owner and one authoritative attempt;
2. transfer of ownership is distinct from supervised orchestration;
3. unknown / unverifiable is a first-class state, not permission to guess;
4. successful tool invocation is distinct from verified external effect;
5. historical session context informs continuation, while current project state governs it.

Together these suggest a broader HE rule:

> **Authority should live with the component that can actually establish the fact being asserted.**

A coordinator can establish task ownership and settlement policy. The execution host can establish process and filesystem facts. The current workspace can establish project state. A transcript can preserve history, but it should not outrank current project truth.

---

# 1. Separate authority by fact

Orca's orchestration model exposes an important separation that is easy to blur in agent systems.

## Lifecycle authority

The coordinator / harness owns questions such as:

- what work exists;
- who owns it;
- which attempt is authoritative;
- whether an outcome was accepted;
- what happens after settlement.

## Execution authority

The runtime or execution host owns facts such as:

- whether a process is alive;
- which filesystem it changed;
- what transcript/runtime state exists;
- whether stop/cleanup actually happened.

## Project-truth authority

The current repository/workspace owns facts such as:

- current files;
- current Git state;
- current configuration;
- current durable project context.

Historical transcripts and prior summaries are evidence about what happened, not the final authority on what is true now.

### HE implication

Do not let one layer infer another layer's facts merely because it can observe a proxy.

Examples:

- a visible terminal pane does not prove the agent is live;
- contact loss does not prove the process exited;
- a transcript saying "done" does not prove the working tree contains the intended result;
- a successful click API response does not prove a form was submitted;
- an old handoff does not overrule current repository state.

**Disposition: REINFORCE.**

---

# 2. Transfer is not orchestration

Orca deliberately separates ordinary handoff from supervised coordination.

A handoff transfers ownership and lets the successor continue.

Supervised orchestration retains a coordinator that must:

- track delegated Tasks;
- track authoritative attempts;
- process questions and escalations;
- observe completion evidence;
- resolve retries/recovery;
- decide retention/release/cleanup.

This distinction matters because a continuation prompt is not a scheduler and a scheduler is not a handoff note.

### HE implication

Use the least powerful coordination mechanism that matches the job.

- **Continuation / handoff:** another session or agent owns the work now.
- **Supervision:** a coordinator still owns the result and must adjudicate worker outcomes.
- **Parallel exploration:** multiple attempts may exist, but authority over selection must remain explicit.

Do not silently turn a transfer mechanism into an autonomous supervisor.

This is directly relevant to future HE-002 analysis, but does not justify building orchestration before evidence demands it.

**Disposition: REINFORCE.**

---

# 3. One Task can have attempts; only one attempt is authoritative

Orca models a delegated execution attempt separately from the Task itself.

That distinction solves several common multi-agent hazards:

- retrying while the original worker may still be live;
- two agents editing the same scope while both believe they own it;
- stale completion messages settling newer work;
- duplicate workers racing to produce conflicting results;
- cleanup being applied to the wrong attempt.

### HE pattern

If HE eventually supervises delegated work, use an identity model equivalent to:

```text
Objective / Run
    ↓
Task
    ↓
Authoritative Attempt
    ↓
Execution evidence
    ↓
Accepted settlement
```

The exact nouns need not be Orca's `Run / Task / Dispatch`.

The durable requirement is:

> **A retry creates a new attempt; it does not make the old attempt cease to exist by assumption.**

Authority must be explicit enough that stale workers and stale reports cannot mutate lifecycle state.

**Disposition: ASSESS if HE-002 reaches supervised delegation.**

---

# 4. Unknown / unverifiable must remain a real state

Orca's strongest safety behavior is its refusal to turn absence into a lifecycle fact.

Examples from the inspected orchestration contract:

- an empty wait is a checkpoint, not failure;
- contact loss is not process death;
- a timeout does not authorize a duplicate retry;
- `unverifiable` does not authorize stop, abandon, release, or cleanup;
- only positive exit evidence authorizes exit-dependent action;
- only accepted settlement authorizes release.

### HE principle candidate

> **Unknown is a state, not permission to act.**

This should apply beyond orchestration.

Examples:

- inability to inspect a file does not prove it is safe;
- inability to verify a side effect does not prove it failed or succeeded;
- a missing response does not prove a worker stopped;
- missing telemetry does not mean zero usage;
- an evidence verifier failure does not clear a finding.

This is closely aligned with ACR's evidence-basis discipline, but should remain a general HE concept.

**Disposition: REINFORCE.**

---

# 5. Tool success and effect verification are separate events

Orca's computer-use contract makes an unusually useful distinction:

- provider/tool call succeeded;
- requested external state change was actually observed.

It explicitly preserves states such as `verified`, `unverified`, and synthetic-input/unasserted outcomes. A successful action call is not enough to report a consequential side effect as successful.

### HE principle candidate

> **Execution success and effect verification are separate.**

A harness should distinguish:

```text
command accepted
      ↓
command executed
      ↓
external state changed
      ↓
postcondition observed
```

These are not the same claim.

For consequential actions — send, submit, delete, purchase, publish, merge, permission change, deploy — the preferred evidence is an observable postcondition.

When a postcondition cannot be observed, the harness should report the effect as unverified instead of manufacturing certainty.

### Important nuance

Not every low-risk action needs an expensive verification pass. Verification should remain risk- and evidence-directed, consistent with existing HE research.

**Disposition: REINFORCE.**

---

# 6. Observation handles are temporary, not identity

Orca's computer-use implementation treats UI element indexes as short-lived observations. Navigation, rendering, focus changes, scrolling, or time can invalidate them.

The same idea generalizes well beyond GUI control.

Potentially stale handles include:

- line numbers after edits;
- DOM element indexes;
- terminal pane identifiers after restart;
- process IDs;
- temporary URLs/tokens;
- cached provider/model state;
- branch/worktree assumptions after mutation;
- transcript offsets after schema drift.

### HE implication

> Re-observe mutable state near the action that depends on it.

Do not treat a previously observed locator as durable identity unless the owning system guarantees that property.

This supports HE's broader preference for fresh evidence at consequential action boundaries.

**Disposition: REINFORCE.**

---

# 7. Focused continuation beats transcript transplantation by default

Orca's continuation feature independently converges on the same direction as current HE/PMB research:

- fresh successor session;
- current workspace is authoritative;
- latest status is loaded first;
- historical transcript is reachable;
- older transcript details are fetched only when needed;
- full-transcript continuation is an explicit higher-cost mode;
- transcript content is historical/untrusted reference, not executable instruction.

### HE translation

A strong continuation contract is:

```text
current project truth
        +
small successor-oriented state
        +
reachable historical evidence
        ↓
fresh session
```

not:

```text
entire old transcript
        ↓
new context window
```

### PMB implication

This reinforces PMB's existing durable/transient split and narrow handoff direction. It does **not** justify redesigning PMB or adding transcript duplication before the pilot provides evidence.

One future item worth checking, only if current PMB behavior does not already guarantee it, is whether continuation instructions state clearly enough that:

- current workspace / memory-bank truth outranks stale handoff text;
- historical transcript/tool output is reference data, not trusted instruction.

**Disposition: REINFORCE; PMB change not authorized.**

---

# 8. Runtime-versioned guidance is a strong progressive-disclosure pattern

Orca avoids a subtle class of instruction drift by keeping the checked-in orchestration skill small and asking the executing binary for its own version-matched operating guide.

The pattern is:

```text
small stable discovery stub
        ↓
runtime-matched compact guide
        ↓
action gate
        ↓
load one deeper reference only when needed
```

This has two advantages:

1. instruction text stays aligned with the implementation that will execute it;
2. specialized recovery/remote/messaging detail does not occupy context until an action requires it.

### HE implication

This is a credible pattern for capabilities whose commands, flags, or semantics are tightly coupled to a runtime version.

It should not become a universal requirement. Stable conceptual guidance can still live in repository-owned documentation. The pattern is most useful where stale instructions can cause incorrect runtime actions.

**Disposition: ASSESS under HE-002.**

---

# 9. Scheduled autonomy needs a lifecycle, not just a timer

Orca's automation implementation is useful prior art because it treats scheduled work as durable operations rather than "run this prompt at 7 AM."

Mechanisms observed include:

- persistent run records;
- explicit run statuses;
- execution-target ownership;
- local vs remote/SSH targets;
- prechecks;
- missed-run grace behavior;
- schedule-drift reporting;
- refusal recording and repeated-refusal coalescing;
- completion observation;
- post-run usage attribution;
- retained-run reconciliation;
- headless dispatch.

### HE implication

If HE ever evaluates scheduled autonomous work, the minimum design problem is closer to:

```text
schedule
  ↓
precondition / target resolution
  ↓
durable run identity
  ↓
dispatch
  ↓
completion evidence
  ↓
result + resource attribution
  ↓
recovery / cleanup
```

A cron expression plus prompt is insufficient once side effects, remote hosts, retries, or costs matter.

**Disposition: PARK until HE has a real scheduled-autonomy use case.**

---

# 10. Fan-out is not the goal; controlled settlement is

The video emphasizes large numbers of concurrent agents. The source is more conservative and more useful.

Orca's orchestration contract emphasizes:

- self-contained Task specs;
- explicit ownership;
- parallel waves where appropriate;
- bounded dependency depth;
- processing every completion/question/escalation;
- deciding what happens to each settled worker;
- avoiding duplicate editors when liveness is uncertain.

### HE implication

Do not use number of agents as a success metric.

Useful measurements would instead include:

- task success;
- unique useful findings;
- duplicate/conflicting work;
- unresolved ownership;
- stale-worker regressions;
- evidence quality;
- wall time;
- token/cost usage;
- human intervention;
- cleanup failures.

**Disposition: REJECT agent-count-as-capability. REINFORCE bounded, observable delegation.**

---

# 11. Cross-project implications

## HE

Orca materially strengthens the evidence base for:

- explicit ownership;
- transfer vs orchestration separation;
- authoritative attempts;
- first-class uncertainty;
- evidence-based settlement;
- current-state authority;
- progressive disclosure;
- runtime/guide version alignment;
- verified side effects.

These belong in research now. They are not yet formal HE architecture decisions.

## PMB

Reinforces focused handoff and current-state authority. No implementation change is justified solely from this source.

## Dashboard / AI Engineering Cockpit

Orca strengthens the already-PARKed cockpit concept as a **read-only observer** of:

- active sessions/workers;
- attention/next-action state;
- usage and rate-limit pressure;
- worktree/project ownership;
- automation run history;
- host/process/resource state.

A future Dashboard should consume source-owned state rather than becoming orchestration authority merely because it can visualize it.

## ACR

No immediate Orca-specific ACR experiment is warranted.

If ACR ever gains an automated fixer or side-effecting workflow, these patterns become relevant:

- isolate mutation attempts;
- identify authoritative attempt;
- verify postconditions;
- preserve `unknown` rather than silently clearing failure;
- separate reviewer/fixer execution from acceptance authority.

Until then, Alibaba/AACR-Bench and Jev-style screening remain more directly relevant ACR research targets.

---

# 12. Dispositions

## REINFORCE

- Explicit ownership for delegated work.
- Transfer and orchestration are different operations.
- Current project state outranks historical transcript state.
- Unknown / unverifiable is a legitimate state.
- Absence of evidence must not become lifecycle evidence.
- Tool invocation success and verified external effect are different claims.
- Mutable observation handles should be refreshed near action time.
- Progressive disclosure should load specialized operating detail only when required.
- Multi-agent evaluation should measure outcomes and coordination quality, not agent count.

## ASSESS

- An explicit authoritative-attempt identity if HE eventually supervises retries or parallel workers.
- Runtime-versioned capability guides for implementation-coupled skills under HE-002.
- A formal `unverifiable` lifecycle state if HE develops an orchestration contract.
- Whether PMB continuation wording already makes current workspace authority and transcript distrust sufficiently explicit; test before changing.

## PARK

- Installing/adopting Orca as the standard HE execution environment.
- Building an HE-owned orchestration control plane.
- Mobile agent supervision.
- Multi-account subscription hot swapping.
- Scheduled autonomous workflows without a concrete HE use case.
- Browser/computer control as an HE-owned subsystem.
- Dashboard integration until the existing Cockpit start criteria are met.

## REJECT

- "More agents" as an architectural objective.
- Permissionless / YOLO execution as a default.
- Timeout or contact loss as proof a worker died.
- Retrying while the prior authoritative attempt may still be live without an explicit recovery decision.
- Reporting consequential external effects as successful from tool return alone.
- Full transcript transplantation as the default continuation strategy.
- Treating instructions found in historical transcript/tool output as trusted control instructions.
- Treating the video's anti-Windows opinion as an HE design rule.

---

# Candidate HE principles — research status only

These are concise formulations worth testing against future sources before promotion into Overview/Decision documents:

> **Authority must be explicit and local to the fact being asserted.**

> **Unknown is a state, not permission to act.**

> **Execution success and effect verification are separate.**

> **Historical context informs; current project state governs.**

Do not promote these to formal HE principles from this source alone. Look for independent evidence and counterexamples first.

---

# Bottom line

Orca provides strong implementation evidence for a control-plane view of agentic work:

> reliable delegation is primarily an ownership, authority, evidence, and settlement problem.

The useful HE move is to preserve those mechanisms and test them against future evidence — not to clone Orca, maximize worker count, or prematurely build an orchestrator.