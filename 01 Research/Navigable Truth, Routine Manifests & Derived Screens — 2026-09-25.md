# Navigable Truth, Routine Manifests & Derived Screens — 2026-09-25

## Purpose

Synthesize the durable Harness Engineering lessons from Pav Rusovs' MAPS AI OS guide and the Orca operator-surface follow-up without adopting either product/framework as HE architecture.

Source evidence:

- `01 Research/Sources/Pav Rusovs — MAPS AI OS, Navigable Memory & Derived Screens — 2026-09-25.md`
- `01 Research/Sources/Orca — Operator Surface, Usage & Session Control — 2026-09-25.md`
- prior Dashboard synthesis: `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`

---

## Core synthesis

A useful AI engineering environment benefits from separating four responsibilities that are often collapsed:

```text
1. AUTHORITATIVE STATE
   project files / MB / Git / CI / provider facts
                ↓
2. NAVIGATION & DERIVED INDEXES
   signposts / maps / search indexes / generated graph
                ↓
3. EXECUTION & ROUTINES
   agents / scheduled work / bounded run contracts
                ↓
4. PRESENTATION & OPERATOR CONTROL
   dashboard / cockpit / alerts / command intents
```

The lower layers should not silently transfer ownership upward.

A navigation map does not become project truth.
A scheduler does not become memory.
A dashboard does not become the database.
An operator surface does not become the agent's reasoning engine.

This is a direct application of **Single Ownership** across runtime layers.

---

# 1. Authoritative state and navigability are different requirements

A system can have correct durable state and still be difficult for an agent to use because the agent cannot locate the right source efficiently.

Therefore HE should assess both:

### Truth quality

- Is there one authoritative owner?
- Is it current?
- Is it durable?
- Can conflicting copies be detected?

### Navigation quality

- Can the agent identify the correct source without user path hints?
- How many search/read steps are required?
- How much irrelevant context is consumed first?
- Are pointers stale?
- Was required information actually retrieved at the point of use?

### Principle candidate

> **Correct context that cannot be found reliably is operationally unavailable context.**

This extends Progressive Disclosure from “do not load everything” to “make the next authoritative source cheap to discover.”

---

# 2. Derived indexes should be disposable

Signpost maps, repository graphs, summaries and semantic indexes can improve navigation, but they should be treated as **materialized views** over source-owned artifacts whenever possible.

A healthy derived index should be:

- rebuildable;
- validated against its sources;
- attributable to source paths/IDs;
- safe to delete and regenerate;
- explicit about freshness;
- unable to silently override the source.

### Principle candidate

> **A derived context structure should improve navigation without acquiring truth ownership.**

This applies equally to MAPS-style signposts, Graphify-style code graphs, embedding indexes, generated repository maps and future Dashboard caches.

---

# 3. Navigation layers need an economic test

The relevant question is not whether a map/graph/index can answer useful questions.

It is whether it reduces enough real work to justify:

```text
build cost
+ refresh cost
+ validation cost
+ routing/tool cost
+ context returned
+ staleness risk
+ operational complexity
```

relative to measured improvement in:

```text
correct-source discovery
+ search/read calls avoided
+ time saved
+ context saved
+ quality/recall improved
+ repeated reuse
```

The MAPS demonstration provides project-local evidence that a signpost map can reduce navigation cost for a specific task. The Graphify experiment provides the opposite local result for a structural graph on ordinary small/medium-repo work.

### Principle candidate

> **Context infrastructure should be evaluated on representative task classes, not on feature capability alone.**

---

# 4. Structured routine manifests are a control surface

Scheduled agent work should not be represented only by a cron entry and an opaque prompt.

A minimal routine contract should make explicit:

- identity;
- purpose;
- schedule/trigger;
- execution host;
- model/effort when applicable;
- allowed capabilities;
- resource/turn/time budget;
- output destination;
- completion/evidence semantics;
- retry/failure behavior;
- run history/provenance.

This converges with Orca's first-class automation-run semantics.

### Principle candidate

> **Automation should be inspectable as data before it is executable as behavior.**

That makes review, governance, migration and Dashboard presentation easier without requiring the dashboard to own the automation.

---

# 5. Unattended availability and authority are separate axes

An agent can run 24/7 without needing 24/7 authority to create external side effects.

Good unattended work is often:

- read;
- classify;
- calculate;
- test;
- prepare;
- draft;
- write bounded local artifacts;
- surface an attention item.

Higher-impact actions such as publish/send/delete/pay/merge/deploy should remain behind the appropriate authority boundary unless explicit evidence justifies automation.

### Principle candidate

> **Increase availability independently from authority.**

This is consistent with monotonic policy, least privilege and Orca's distinction between successful tool execution and verified external effect.

---

# 6. Presentation should be lossless if removed

The strongest Dashboard lesson from MAPS is “show, don't store.”

For HE, that becomes:

> **If the presentation layer disappears, authoritative engineering state should remain recoverable from its owning systems.**

A Dashboard/Cockpit may cache for performance, but its cache should be reconstructable.

Every displayed value should ideally carry:

- source;
- observed/generated timestamp;
- freshness/staleness;
- authority/derivation class;
- degraded/unknown state when collection fails.

This directly reinforces the existing `AI Engineering Observability & Dashboard Boundary` note.

---

# 7. Read plane and command plane should be separate

A read-only dashboard is easy to reason about.

Once the UI gains buttons, it participates in execution.

The clean boundary is:

```text
source-owned read models
       ↓
presentation

user action
       ↓
bounded command intent
       ↓
owning runtime re-validates policy/current state
       ↓
execution
       ↓
result/effect evidence
```

A button should not directly mutate project truth merely because it is convenient UI code.

### Principle candidate

> **Operator intent is input to the owning executor, not a shortcut around its policy boundary.**

Keep the parked Cockpit read-only for MVP if it is eventually built.

---

# 8. Execution status and acceptance status should not be overloaded

Across MAPS, Paperclip, Orca and HE's own verification work, the same distinction keeps appearing:

```text
process/run finished
≠
artifact satisfies objective evidence
≠
human/project authority accepts completion
```

Systems should model these separately where the distinction matters.

### Principle candidate

> **Do not let `done` collapse runtime settlement, verification and acceptance into one ambiguous state.**

---

# 9. Cross-machine identity should be logical, not machine-path-specific

A shared context/navigation layer must survive different roots, OSes and worktrees.

Therefore HE should prefer:

- repository-relative paths;
- stable artifact IDs;
- logical project roots;
- explicit path mapping where unavoidable;

rather than embedding one machine's absolute filesystem identity into durable shared state.

Similarly, cross-machine Git synchronization needs explicit writer/conflict semantics rather than naive bidirectional scheduled commits.

**REINFORCE:** portability requires more than copying the same files to two machines.

---

# 10. Implications for the parked Dashboard / AI Engineering Cockpit

The Dashboard remains **PARKED**, but its architecture is now clearer.

If later authorized, the preferred order is:

```text
1. define source adapters and contracts
2. expose provenance/freshness
3. prove recurring operational questions
4. evaluate existing surfaces such as Orca
5. build the smallest read-only projection
6. add command intents only after read-only value is proven
```

Orca should be evaluated as a baseline because it already provides substantial generic runtime/session/worktree/provider plumbing.

A separate Cockpit becomes justified only if recurring HE-specific needs remain, such as:

- PMB state/health/handoff;
- ACR evidence/calibration state;
- cross-host context occupancy;
- grounded cross-project re-entry;
- source-authority/freshness visibility Orca does not provide.

Do not build a custom visual shell merely to duplicate Orca's generic session/operator features.

---

# 11. Implications for PMB / work MB

MAPS does not justify another memory subsystem.

It reinforces:

- one owner for each durable fact;
- small navigation surface;
- progressive retrieval;
- deterministic pointer/integrity checking;
- durable state over transcript history.

A generated navigation map should be evaluated only if PMB/work-MB evidence shows a real source-discovery problem.

If tested, compare current routing against the candidate map using representative required-retrieval tasks rather than adopting a map because it looks organized.

---

# 12. Implications for Harness Miner

Harness Miner can help answer whether navigation/automation infrastructure is actually needed by measuring real behavior, for example:

- repeated path guessing;
- search/read depth before authoritative source;
- stale-reference failures;
- routines that repeatedly fail/retry;
- user corrections caused by missed context;
- recurring work that is still manual;
- model calls that deterministic code could replace.

Harness Miner should consume run/navigation evidence. It should not become the map, scheduler or dashboard itself.

---

## Disposition summary

- **REINFORCE:** authoritative truth and navigation are distinct responsibilities.
- **REINFORCE:** derived indexes should be rebuildable/disposable.
- **REINFORCE:** context/navigation infrastructure must earn its lifecycle cost.
- **REINFORCE:** routine definitions should be structured, bounded and inspectable.
- **REINFORCE:** unattended availability does not require broad authority.
- **REINFORCE:** presentation consumes state; it does not own state.
- **REINFORCE:** read plane and command-intent plane should remain distinct.
- **REINFORCE:** runtime completion, verification and acceptance are separate concepts.
- **REINFORCE:** cross-machine identity should use logical/project-relative identifiers where possible.
- **ASSESS:** generated signpost maps for demonstrated source-discovery friction.
- **ASSESS:** Orca as the generic operator-surface baseline before a custom Cockpit.
- **PARK:** custom Dashboard/Cockpit implementation.
- **PARK:** HE-owned scheduler or always-on agent infrastructure.

## Status

Research synthesis only.

No PMB, work-MB, ACR, Dashboard or Harness Miner implementation changes authorized by this note.
