# Rta-Smriti Brain — Harness, Evidence & Context Compilation Deep Evidence Pass

**Date:** 2026-09-06  
**Repository:** https://github.com/sulabhdubey/rta-smriti-brain  
**Current release reviewed:** `v1.0.4-alpha`  
**Primary evidence:** repository README, architecture, usage guide, AGENTS example, release verification record, and release notes.  
**Evidence boundary:** this is prior-art mining, not an endorsement of the implementation or a claim that every documented behavior was independently reproduced here.

## Corpus relationship

**SUPERSEDES / SHARPENS / MERGES WITH:** none; this is a new corpus entry.

**Relationship to existing entries:**

- **Hindsight — Durable Memory Self-Improvement:** **MERGES WITH / SHARPENS.** Hindsight supplies the retrospective admission problem: an observed event is not automatically a durable rule. Rta-Smriti supplies a broader evidence-state vocabulary (`pratyaksha`, `sabda`, `anumana`, `smriti`, `kalpana`) and applies trust distinctions to retrieval/context delivery, not only retrospective memory consolidation.
- **OpenMAIC — Durable Agent Runtime:** **MERGES WITH / SHARPENS.** OpenMAIC separates durable execution state from conversational context. Rta-Smriti similarly separates a durable project brain from bounded context compilation, and adds explicit freshness, provenance, evidence authority, and task-contract controls.
- **Anthropic AI-Native SDLC:** **CONVERGES WITH.** Both treat committed/durable artifacts as bridges between agent stages, distinguish operational knowledge from transient context, and use explicit verification/evaluation boundaries.
- **Matt Pocock:** **CONVERGES WITH / SHARPENS.** Rta-Smriti's task contracts, stop conditions, evidence requirements, freshness checks, and bounded context packs reinforce the need for explicit workflow boundaries and context management without duplicating Matt's implementation skills.
- **Ras Mic / Ralphy:** **CONVERGES WITH / ORTHOGONAL.** Rta-Smriti is primarily evidence/state infrastructure rather than an autonomous execution loop. Its capability grants, leases, root binding, and publication-safe boundaries complement the execution/isolation contracts mined from Ralphy.
- **PMB Filing Contract:** **HIGH-VALUE SHARPENING.** The repo provides a concrete example of canonical durable state, derived projections, bounded context projections, freshness state, provenance, and read-only versus mutation boundaries. It does not justify adopting a second PMB data store.

## Executive finding

Rta-Smriti Brain is best understood as a **local project-reality and evidence substrate with a governed context compiler**, not merely a memory database and not an agent harness.

Its most important contribution to Harness Engineering is a three-level separation:

```text
Durable project state / evidence
            |
            v
Deterministic, bounded evidence projection
            |
            v
Task-specific governed context
            |
            v
Agent / harness execution
```

The repository explicitly says the Project Cognition layer is a deterministic read projection rather than another mutable source of truth, and that its context compiler prepares evidence and constraints without planning, routing models, executing project tools, or mutating repositories. [Architecture](https://github.com/sulabhdubey/rta-smriti-brain/blob/main/docs/ARCHITECTURE.md)

This is highly relevant to our evolving Harness model because it gives us a concrete prior-art instance of **evidence authority separated from execution authority** and **durable state separated from model context**.

---

# 1. Project Reality: cognition is a projection, not a second database

Rta-Smriti's v1 Project Cognition layer combines the latest indexed source snapshot, bitemporal claims, normalized observations, structured work state, durable decisions, and governed media records. It emits bounded views including readiness, digital-twin reconciliation, knowledge coverage, decision debt, change-impact hints, conflicts, and truncation metadata.

The important design choice is that cognition is a **read projection over existing evidence** rather than a new mutable source of truth.

The readiness concept is also stronger than database health. Missing checkpoints, stale/uncertain evidence, unresolved high-authority conflicts, incomplete work state, and omitted bounded inputs can degrade or block continuation readiness. Routine cognition uses the latest completed index, while consequential work requires live/deep freshness verification.

### Harness implication

PMB/Harness should distinguish at least:

1. **Storage health** — can we read the durable state?
2. **Evidence freshness** — is the state current enough for this action?
3. **Evidence authority** — how much should this evidence be trusted?
4. **Continuation readiness** — is there enough reconciled information to continue safely?
5. **Execution readiness** — does the agent actually have the authority/tools to act?

These are different states. A healthy memory database is not necessarily current, trustworthy, or sufficient for consequential work.

**Disposition: ADOPT DESIGN PRINCIPLE.**

---

# 2. Pramana: evidence provenance is first-class

Rta-Smriti classifies context into five evidence states:

- `pratyaksha` — directly observed from code, tests, files, or tools
- `sabda` — trusted instruction, documentation, or human guidance
- `anumana` — inference
- `smriti` — prior memory
- `kalpana` — hypothesis/creative possibility

The supplied agent instructions explicitly tell agents to preserve these distinctions when writing memory. Retrieved context is guidance, not proof; stale indexed files must be re-read before acting.

### Why this matters

This is stronger than a generic confidence score. It encodes **epistemic provenance** rather than pretending all context can be ranked on one scalar axis.

A test result and a remembered design decision may both be useful, but they do not have the same evidentiary status. A hypothesis may be useful for exploration while being completely inappropriate as a governance constraint.

This directly sharpens the Hindsight distinction:

> **Verified occurrence is not the same thing as justified generalization.**

Hindsight asks whether a lesson deserves persistence. Rta-Smriti additionally asks what kind of evidence the persisted item represents and how that status should affect downstream context.

### PMB implication

A useful future PMB evidence model may need separate fields for:

- source/provenance
- observation versus inference
- verification state
- freshness
- authority
- historical status
- downstream eligibility

Do **not** collapse these into a single `confidence` field.

**Disposition: ADOPT DESIGN PRINCIPLE / HIGH-VALUE RESEARCH.**

---

# 3. Durable brain != model context

This is probably the strongest single prior-art contribution.

Rta-Smriti stores project reality in SQLite and then builds bounded context packs. The usage guide describes the context pack as a task-specific handoff, while the architecture describes the governed compiler as a separate boundary that takes a read-only snapshot and selects evidence under explicit constraints.

The system therefore does not equate:

`everything remembered` = `everything sent to the model`.

Instead:

`durable evidence` → `selection` → `bounded task context`.

The compiler also records metadata explaining inclusion, exclusion, redaction, downgrade, deduplication, section allocation, and truncation.

### Harness implication

This reinforces our recent context-cost work. Startup context should not be the only mechanism by which durable project knowledge reaches an agent.

Potential architecture concept:

```text
Durable memory/evidence
       |
       +--> indexing / provenance / freshness
       |
       v
Task evidence selection
       |
       +--> authority filter
       +--> freshness filter
       +--> scope/privacy filter
       +--> token budget
       +--> truncation
       +--> provenance receipt
       |
       v
Agent context
```

This is **not** a recommendation to build Rta-Smriti inside PMB. It is a reason to investigate whether PMB should eventually expose a bounded **context/evidence compilation capability** rather than continuously enlarging startup instructions.

**Disposition: PRIORITY RESEARCH.**

---

# 4. Governed Context Compiler

The most interesting implementation-level primitive is the governed context compiler.

An operator registers an agent-consumption profile and authorizes an immutable task contract containing:

- objective
- acceptance criteria
- evidence requirements
- stop/escalation conditions
- prohibited repetition
- privacy scope
- informational grants
- token economics

A short-lived host capability binds compilation to the exact project, contract, principal, session, scope, expiry, and revocation state.

The compiler captures one read-only project snapshot, verifies repository/database fences, normalizes candidate evidence, applies privacy/scope filtering before scoring, and ranks with multiple explicit signals. Mandatory controls can cause the compiler to abstain rather than fabricate a fit.

It also produces explanation receipts without retaining private payloads by default.

Crucially, the compiler **does not execute the task**.

### Harness implication

This provides a concrete prior-art pattern for separating:

- evidence authority
- context compilation authority
- execution authority
- model routing authority
- repository mutation authority

We have already been moving toward these distinctions. Rta-Smriti makes the separation operational rather than merely conceptual.

### Important caution

Its implementation is more elaborate than PMB currently needs. We should mine the boundary and contract concepts, not reproduce its entire compiler.

**Disposition: HIGH-VALUE RESEARCH / CANDIDATE EXPERIMENT.**

---

# 5. Task contracts are stronger than prompts

The governed compiler's immutable task contract is particularly relevant to our authority model.

The contract carries acceptance criteria, evidence requirements, stop/escalation conditions, privacy scope, and token economics. Authorization is a separate operator action.

This resembles our existing distinction between a user's intent/spec and an agent's execution, but adds a formal **consumption contract** describing what information the agent is permitted and expected to consume.

### New distinction worth preserving

We may eventually need three related contracts:

```text
Task Contract
  What is being attempted and what counts as acceptable.

Evidence Contract
  What evidence is required, trusted, fresh enough, and in scope.

Execution Contract
  What tools, mutations, concurrency, isolation, and publication authority are permitted.
```

Ras Mic/Ralphy work already points toward an Execution Contract. Rta-Smriti provides strong prior art for the Evidence Contract.

**Disposition: ADOPT AS RESEARCH DIRECTION.**

---

# 6. Freshness is an independent safety dimension

Rta-Smriti treats freshness as more than a timestamp.

It maintains canonical project/root identity, per-checkout identity, stat manifests, SHA-256 content caching, deep freshness checks, changed/missing/added/blocked file reporting, and explicit root-rebinding procedures.

Filesystem events bypass metadata shortcuts and bind content-hash reads to the repository root. Consequential work can require deep freshness rather than relying on the latest completed index.

The usage guide makes an important boundary explicit:

> A fresh index does not prove tests passed or an external job completed.

That is a very useful distinction.

### Harness implication

Freshness should not be treated as a universal “green” state. Different evidence types have different freshness semantics:

- source bytes
- tests
- CI
- external deployment state
- human decisions
- model-generated summaries
- historical memory

A fresh source index cannot attest to CI. A recent memory record cannot attest to current code.

**Disposition: ADOPT DESIGN PRINCIPLE.**

---

# 7. Canonical identity and checkout identity

Rta-Smriti distinguishes repository lineage from checkout identity. A clone and a Git worktree can share history while representing different active filesystems.

The system refuses silent checkout switching and requires explicit `root-rebind` for intentional movement, with backup, worker shutdown, matching repository lineage, and atomic reindexing.

### Connection to our Harness work

This is directly relevant to the worktree/isolation lessons from Matt and Ralphy:

- worktree isolation is not itself security isolation;
- a durable memory/index must not silently follow a different checkout;
- candidate branches and parallel worktrees need identity binding;
- context assembled for one checkout should not silently be reused for another.

This also gives a useful concept for execution provenance:

**project lineage + checkout identity + source revision** should be separable identifiers.

**Disposition: HIGH-VALUE IMPLEMENTATION RESEARCH; not a PMB architecture requirement yet.**

---

# 8. Read-only evidence versus mutation capability

Rta-Smriti's MCP server is read-only by default. Memory writes, repository ingestion, thread ingestion, and continuity control are separately granted capabilities.

Agent-authored memory is downgraded to unverified `anumana`; it cannot self-assert source authority. Agents cannot mutate policy, attest required checks, or override policy blocks.

The generated context compiler capability is also separately delegated by an operator and bound to an exact contract ID and digest.

### Harness implication

This is strong evidence for **capability separation**:

```text
Read evidence
Write memory
Ingest repository
Ingest thread
Control continuity
Compile governed context
Execute project work
Override governance
Publish
```

These should not automatically travel together merely because one agent is doing the work.

This aligns strongly with the Harness principle that **tool availability and execution authority are separate dimensions**.

**Disposition: ADOPT DESIGN PRINCIPLE.**

---

# 9. Continuity is an event/state problem, not just transcript storage

The Codex continuity adapter reads bounded JSONL sessions, maintains byte cursors, preserves incomplete final records, redacts common credential shapes, bounds oversized tool output, and records truncation explicitly.

Automatic checkpoints are created only after the newest matching transcript has been fully consumed and a terminal/inactivity/shutdown condition occurs. They retain no verified evidence by default and require operator verification.

This is materially stronger than “save the conversation.”

### Connection to OpenMAIC

OpenMAIC's durable runtime separates lifecycle state, event stream, append-only entries, and projections. Rta-Smriti independently converges on a similar idea for agent continuity.

### Harness implication

A future Harness continuity substrate should probably distinguish:

- raw event history
- derived checkpoint
- verified evidence
- current continuation state
- durable lesson/memory

The checkpoint is not the transcript. The summary is not automatically evidence. The event log is not the current state.

**Disposition: PRIORITY RESEARCH.**

---

# 10. Governance / Action Gate

Rta-Smriti has typed policies describing constraints, failed approaches, fragile paths, required checks, and prohibited repetition.

Its Action Gate evaluates proposed actions against trusted policy evidence, readiness, Git state, freshness, expiry, scope, and provenance. Only high-trust verified hash-backed policy evidence can independently block; weaker records become warnings.

Owner overrides are recorded as receipts. Agent MCP tools cannot mutate policy, attest checks, or override blocks.

### Harness implication

This is a useful prior-art instance of a layered enforcement model:

- deterministic policy evaluation
- evidence trust levels
- action-scoped receipts
- explicit owner override
- agent inability to self-attest governance

This overlaps our existing governance direction and therefore should **not** become another parallel PMB gate architecture.

**Disposition: CONVERGES WITH EXISTING GOVERNANCE; MINE SPECIFIC PATTERNS ONLY.**

---

# 11. Graph intelligence: useful hints, explicit limitations

Rta-Smriti derives files, symbols, imports, calls, tests, configuration, memories, and evidence links. Graph queries are bounded by depth and node count and expose relation filters and confidence labels.

It explicitly refuses to present these edges as compiler-perfect analysis.

This is another useful epistemic boundary:

**approximate structural evidence can be useful without being promoted to proof.**

This connects to the Archify research: architecture/code extraction should remain deterministic where possible, expose unsupported cases, and avoid turning an inferred graph into a trusted architecture fact.

**Disposition: ORTHOGONAL CONVERGENCE WITH ARCHIFY; retain as supporting evidence, not a new architecture.**

---

# 12. Retrieval diagnostics are unusually inspectable

Rta-Smriti reports retrieval mode/provider, embedding coverage, parser fallback, freshness, latency, rank components, source hashes, normalized query terms, and per-result selection reasons.

Its benchmark is explicitly framed as regression evidence rather than proof of market superiority.

### Harness implication

This reinforces a principle already emerging from our corpus:

> **When an agent-facing selection mechanism affects behavior, its provenance and limitations should be inspectable.**

For context compilation, useful receipts may include:

- source selected
- source excluded
- why selected/excluded
- authority
- freshness
- truncation
- deduplication
- privacy redaction
- tool/provider version
- task/contract identity

**Disposition: ADOPT DESIGN PRINCIPLE / RESEARCH.**

---

# 13. Import/export demonstrates authority downgrade

Selective bundles contain memories, checkpoints, and policies but not source code. Import stages data in an in-memory SQLite copy before atomic commit.

Critically, unsigned imported memories are downgraded to unverified `smriti`; imported checkpoints and policies are quarantined for owner review rather than gaining authority.

This is a strong example of **provenance surviving data movement**.

### Harness implication

Moving durable state across boundaries should not silently preserve authority.

Potential generalized rule:

```text
transported evidence != locally verified evidence
```

This is especially relevant if PMB ever supports shared memory, bundles, cross-project workspaces, or handoff packages.

**Disposition: ADOPT DESIGN PRINCIPLE.**

---

# 14. Memory lifecycle and feedback

Rta-Smriti records helpful, neutral, and harmful outcomes. It can conservatively decay old, unverified inference/hypothesis records that have not been reinforced while protecting verified evidence and higher-trust records.

This is interesting but should not be adopted wholesale.

The useful abstraction is that memory can have **outcome feedback** without allowing automatic feedback to rewrite authority.

That aligns with Hindsight's distinction between observed outcome and durable generalization.

**Disposition: HIGH-VALUE RESEARCH; no automatic PMB mutation yet.**

---

# 15. Security boundaries worth mining

The repo contains several concrete defensive patterns that are relevant to a Harness but should not be conflated with a general OS sandbox:

- reject symbolic/reparse/hard-linked database/control files;
- bind project identity to canonical root and checkout identity;
- reject project-local executable discovery for certain parser adapters;
- avoid shell execution for native LSP invocation;
- bound JSON-RPC frames, process time, response size, traversal, and total input size;
- keep the console loopback-only;
- require per-launch capability tokens;
- redact common credentials before durable queuing;
- separate managed worker lifecycle from privileged OS services;
- explicitly state that same-OS-account processes are not mutually isolated;
- treat repository text as evidence rather than executable instruction.

The repository itself is careful to state that installed Python isolated mode is **not an OS sandbox**, and that local workers are not privileged services.

This is exactly the kind of honest-boundary language we want in Harness Engineering.

**Disposition: SUPPORTING PRIOR ART; do not import implementation wholesale.**

---

# 16. Release/evidence discipline

The current `v1.0.4-alpha` release verification record is unusually explicit about evidence boundaries. It records exact commit/tag/PR identifiers and reports test counts, platform coverage, UX journeys, security/privacy checks, artifact hashes, and anonymous post-publication acceptance.

It also explicitly distinguishes:

- launcher isolation from OS sandboxing;
- freshness from correctness;
- synthetic benchmark regression from superiority claims;
- checksums from platform code signing.

The release notes similarly describe the patch as a narrow launcher-integrity change and state what it does **not** prove.

This is a useful example of **claims bounded by evidence**.

**Disposition: ADOPT AS RESEARCH / EVIDENCE-DISCIPLINE SUPPORT.**

---

# 17. What we should NOT adopt

Do not treat this repo as a PMB reference implementation to copy.

Specifically reject for now:

1. **A second PMB database/data store.** The value is the state/evidence model, not SQLite itself.
2. **A full Project Reality cockpit.** Useful product UX, not a Harness requirement.
3. **A full graph intelligence subsystem.** Keep bounded structural evidence research separate.
4. **Automatic memory decay as a governance mechanism.** Requires experiments first.
5. **Full context compiler implementation.** Research the boundary before adding machinery.
6. **Rta-Smriti as a PMB dependency.** Maintain replaceable boundaries.
7. **Single confidence scalar.** Its evidence taxonomy is more informative than scalar confidence.
8. **Treating fresh index state as proof of current project correctness.** The repo itself rejects this.
9. **Treating worktree/root binding as security isolation.** Identity and isolation are separate concerns.
10. **Assuming imported memory retains authority.** Provenance must survive transport.

---

# 18. Candidate Harness model emerging from the evidence

The strongest synthesis from Rta-Smriti plus our existing corpus is:

```text
                 DURABLE STATE
                      |
          +-----------+-----------+
          |                       |
       evidence                work state
          |                       |
          +-----------+-----------+
                      |
              reconciliation
                      |
              freshness / trust
                      |
              task evidence set
                      |
             evidence contract
                      |
            bounded compilation
                      |
          provenance / receipt
                      |
                 AGENT CONTEXT
                      |
                EXECUTION HARNESS
                      |
        +-------------+-------------+
        |             |             |
      tools        isolation     authority
        |             |             |
        +-------------+-------------+
                      |
                 verification
                      |
                  publication
```

The important point is that **context generation becomes a governed projection**, not merely prompt assembly.

---

# 19. Experiments suggested by this corpus entry

These are research candidates, not approved architecture changes.

### Experiment A — Evidence-aware context pack

Take an existing PMB task and generate two packs:

- current PMB startup/context path;
- bounded task-specific evidence pack with explicit authority/freshness/provenance.

Measure:

- input tokens
- task success
- factual errors
- stale-context errors
- omitted required evidence
- user correction count
- startup versus per-task cost

This directly tests whether the richer selection mechanism pays for itself.

### Experiment B — Evidence contract

Define a minimal contract:

- objective
- acceptance criteria
- required evidence
- minimum freshness
- prohibited evidence classes
- stop/escalation conditions
- token budget

Have the compiler produce either a pack or **abstain**.

Measure false inclusion, false exclusion, stale evidence inclusion, and abstention rate.

### Experiment C — Durable state versus context

Measure PMB startup context against task-scoped context retrieval.

This is particularly important because our own PMB work has already exposed startup context bloat. A mechanism that reduces repeated startup material but introduces retrieval/compiler cost should be judged on **net lifecycle context cost**, not retrieval elegance.

### Experiment D — Provenance downgrade on transfer

Prototype a handoff artifact whose evidence authority changes when transported into another project/session unless independently verified.

### Experiment E — Checkpoint versus transcript

Compare:

- raw transcript continuation;
- generated checkpoint;
- verified checkpoint;
- task-specific evidence compilation.

Measure context size, continuation accuracy, stale assumptions, and correction work.

---

# 20. Updated corpus dispositions

| Finding | Disposition | Rationale |
|---|---|---|
| Memory/evidence are not equivalent | **ADOPT DESIGN PRINCIPLE** | Strong epistemic boundary |
| Pramana-style evidence classes | **SHARPENS / HIGH-VALUE RESEARCH** | Adds provenance vocabulary without requiring same names |
| Durable state != model context | **ADOPT DESIGN PRINCIPLE** | Directly addresses context bloat and bounded delivery |
| Deterministic context projection | **PRIORITY RESEARCH** | Potential missing Harness capability |
| Evidence Contract | **HIGH-VALUE RESEARCH** | Complements Execution Contract |
| Freshness as independent state | **ADOPT DESIGN PRINCIPLE** | Prevents stale evidence masquerading as current truth |
| Canonical root + checkout identity | **IMPLEMENTATION RESEARCH** | Strong operational pattern; not core PMB architecture yet |
| Capability separation | **ADOPT DESIGN PRINCIPLE** | Reinforces authority/tool distinctions |
| Continuity event/checkpoint separation | **PRIORITY RESEARCH** | Converges with OpenMAIC |
| Action Gate | **CONVERGES WITH EXISTING GOVERNANCE** | Do not create duplicate gate system |
| Retrieval receipts | **ADOPT DESIGN PRINCIPLE** | Selection affecting agent behavior should be inspectable |
| Imported-memory authority downgrade | **ADOPT DESIGN PRINCIPLE** | Provenance must survive transport |
| Outcome feedback | **HIGH-VALUE RESEARCH** | Converges with Hindsight but needs experiments |
| Full Rta-Smriti architecture | **DO NOT ADOPT** | Too broad and creates unnecessary PMB substrate duplication |
| Rta-Smriti as dependency | **DO NOT ADOPT** | Keep architecture/tool boundaries replaceable |

---

# 21. Source map

Primary:

- Repository: https://github.com/sulabhdubey/rta-smriti-brain
- README: https://github.com/sulabhdubey/rta-smriti-brain/blob/main/README.md
- Architecture: https://github.com/sulabhdubey/rta-smriti-brain/blob/main/docs/ARCHITECTURE.md
- Usage guide: https://github.com/sulabhdubey/rta-smriti-brain/blob/main/docs/USAGE_GUIDE.md
- Agent instructions example: https://github.com/sulabhdubey/rta-smriti-brain/blob/main/AGENTS.rta-smriti.example.md
- Release verification: https://github.com/sulabhdubey/rta-smriti-brain/blob/main/docs/RELEASE_VERIFICATION.md
- v1.0.4-alpha release notes: https://github.com/sulabhdubey/rta-smriti-brain/blob/main/docs/RELEASE_NOTES_v1.0.4-alpha.md
- Releases: https://github.com/sulabhdubey/rta-smriti-brain/releases

## Bottom line

Rta-Smriti is valuable prior art because it demonstrates a system boundary we have been approaching from several directions:

> **Durable project knowledge should be reconciled and compiled into bounded, provenance-bearing evidence for a task; that evidence is still not execution authority.**

That is more important to PMB/Harness Engineering than any individual feature in the repository.

The next architectural question should therefore not be “Should PMB become Rta-Smriti?” It should be:

> **Do we need a governed evidence/context compilation capability between PMB durable state and the agent harness, and can we prove that it reduces net context cost without weakening trust or authority boundaries?**

That question is now sufficiently grounded to become an explicit research thread rather than an architectural assumption.
