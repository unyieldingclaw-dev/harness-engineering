# Work Memory Bank — Context & Skills Review Checklist

**Purpose:** Review the work Memory Bank (MB) for context/token efficiency and Skill structure without requiring access to the work repository.

**Assessment basis:** PMB is the observable reference implementation because PMB was derived from the work MB. This document is therefore a **proxy inspection guide**, not a claim about the work MB's current implementation.

**Constraint:** The work MB source code is not available to Harness Engineering. Do not infer exact implementation details that cannot be observed through PMB behavior, shared design history, or user-provided evidence.

---

## Why This Exists

PMB and the work MB have produced an observed operational problem: fresh sessions can begin with roughly **40% of the available context window already consumed** by startup context.

That makes startup context a first-class review target.

The objective is not to make MB as small as possible. The objective is to determine whether the information loaded automatically is:

- necessary;
- current;
- uniquely owned;
- appropriately scoped;
- cheap enough to justify its startup cost;
- and actually improving task reliability.

---

# Review Sequence

## 1. Establish a Fresh-Session Baseline

Before changing anything, capture a clean startup measurement.

Record:

- context window size;
- tokens/percentage consumed immediately after initialization;
- model and runtime;
- persistent instruction sources;
- memory sources;
- Skill metadata;
- tool/MCP/plugin definitions;
- hooks or generated startup content;
- any other automatically injected material.

Do not compare a long-running session to a fresh session and call the difference "startup overhead."

### Red flag

If startup context is already consuming a large fraction of the window before task work begins, treat that as a harness-level issue rather than a user prompting issue.

The previously observed PMB/MB experience of approximately **40% startup usage** should be treated as significant evidence requiring investigation, not as a target to normalize.

---

# 2. Inventory Every Persistent Context Source

For each automatically available source, record:

| Source | Loaded when? | Approx. tokens | Owner | Purpose | Evidence of necessity |
|---|---|---:|---|---|---|
| Global instructions | | | | | |
| Project instructions | | | | | |
| Memory index | | | | | |
| Memory entries | | | | | |
| Skill metadata | | | | | |
| Tool/MCP definitions | | | | | |
| Plugin metadata | | | | | |
| Hooks/generated text | | | | | |
| Other | | | | | |

Do not begin by asking "what can we delete?"

First ask:

> **Why is this loaded now?**

---

# 3. Classify Information by Loading Behavior

Every important artifact should fall into an intentional category:

### A. Always loaded

Required for essentially every task.

Examples:

- critical project constraints;
- essential security rules;
- stable repository navigation;
- genuinely universal behavioral requirements.

### B. Skill-triggered

Required only for a recognizable class of tasks.

Examples:

- release workflow;
- database migration procedure;
- specialized review procedure;
- deployment procedure.

### C. Retrieved/read on demand

Detailed factual or procedural material that is occasionally required.

Examples:

- API references;
- historical decisions;
- detailed schemas;
- edge-case documentation.

### D. Deterministic execution

Work that should normally be performed by code/tools rather than explained to the model.

Examples:

- validation;
- formatting;
- file inventory;
- mechanical transformations;
- static analysis.

### E. Historical record

Useful for understanding what happened but not authoritative for current behavior.

Historical material should not automatically become startup context merely because it exists.

---

# 4. Inspect the Global/Project Instruction Layer

Use PMB as the proxy for identifying patterns likely inherited by MB.

Look for:

- instructions that explain obvious model behavior;
- generic coding advice;
- repeated instructions found elsewhere;
- procedures that are relevant only to one workflow;
- long examples that could be replaced by clearer interfaces;
- historical workarounds whose original failure mode no longer exists;
- instructions that exist only because an older model behaved poorly;
- rules whose owner is unclear;
- contradictory guidance across files.

### Important safeguard

Do **not** remove guidance merely because current models are more capable.

Preserve guidance when it:

- encodes intentional project behavior;
- protects safety;
- enforces a deterministic requirement;
- prevents a demonstrated failure mode;
- captures project-specific knowledge that the model cannot reasonably infer.

Ask whether the instruction still has a job.

---

# 5. Review Skills as Context Routers

For every Skill, inspect four layers separately:

### Metadata

- Is the description short enough to be cheap at startup?
- Does it clearly state what the Skill does?
- Does it clearly state when it should trigger?
- Is it broad enough to trigger when needed without becoming a noisy catch-all?

### SKILL.md

- Is the body focused on the actual procedure?
- Does it contain information the model already knows?
- Does it duplicate project instructions or references?
- Is it approaching the ~500-line / ~5,000-token range where deeper decomposition should be considered?
- Does it explain when deeper references should be read?

### References

- Are detailed facts/specifications outside the main Skill body?
- Are references loaded only when needed?
- Is the same information duplicated in both Skill and reference?
- Can large references be searched or targeted rather than read wholesale?

### Scripts/tools

- Is the model being taught to perform a deterministic operation that code could perform more reliably?
- Is the same helper logic repeatedly recreated during Skill execution?
- Could the result of a deterministic script replace a large amount of procedural context?

---

# 6. Look for Skill Explosion

Do not respond to context bloat by creating a Skill for everything.

For each proposed Skill ask:

1. Is this actually a reusable capability?
2. Does it have a recognizable trigger condition?
3. Does it contain a meaningful procedure rather than a few instructions?
4. Is the procedure currently duplicated elsewhere?
5. Would moving it into a Skill reduce always-loaded context?
6. What is the Skill's metadata/startup cost?
7. How often is it actually used?
8. Does the Skill introduce another layer the user must understand?

A Skill that is rarely used, poorly triggered, or mostly duplicate guidance may make the harness worse even if it technically supports progressive disclosure.

---

# 7. Look for Path / Domain Scoping Opportunities

Where the implementation supports scoped Skills or rules, identify guidance that is only relevant to:

- particular file types;
- particular directories;
- particular commands;
- particular workflows;
- particular project phases.

The question is not "can this be scoped?"

The question is:

> **Does scoping materially reduce irrelevant context without creating confusing activation behavior?**

---

# 8. Audit Duplication and Ownership

Search for the same concept appearing in multiple locations:

- global instructions;
- project instructions;
- memory;
- Skills;
- commands;
- tool descriptions;
- hooks;
- scripts;
- documentation;
- generated context.

For each repeated concept, determine:

- authoritative owner;
- consumers;
- whether consumers need a copy or can reference the owner;
- whether duplicated copies can drift;
- whether the duplication is intentional for runtime reasons.

### Rule

> **One authoritative home; many consumers where practical.**

Do not eliminate duplication automatically if a runtime genuinely requires it. Record why it exists.

---

# 9. Separate Current State From History

Check whether MB makes the following distinctions clear:

- stable project instructions;
- current project state;
- resource/context map;
- active decisions;
- unresolved questions;
- historical records;
- session-specific state.

A historical record should not silently acquire the authority of current state.

A fresh session should be able to identify what is true **now** without reading the entire history of how the project got there.

This is particularly important for a Memory Bank because its purpose naturally encourages accumulation.

---

# 10. Inspect Memory Loading Strategy

This is the highest-priority MB-specific investigation.

For every memory category ask:

- Is it loaded automatically?
- If yes, why?
- How frequently is it relevant?
- Could an index or summary replace the full content?
- Could the content be retrieved by task/topic?
- Is the memory current?
- Is it duplicated elsewhere?
- Is it historical rather than operational?
- Does the agent need the entire memory entry or only a small portion?
- What happens when memory grows 2x, 5x, or 10x?

### Failure pattern to look for

```text
Useful memory
      ↓
kept permanently available
      ↓
more memory added
      ↓
startup context grows
      ↓
agent has less room for task/code/evidence
      ↓
more context management is added
      ↓
context grows again
```

The correct response is not necessarily more sophisticated memory management. First determine whether too much information is being loaded too early.

---

# 11. Inspect Context Indexes

Indexes can be cheaper than full documents, but an index can also become a second memory dump.

Check:

- index size;
- description length;
- duplication with underlying documents;
- stale entries;
- whether the index actually improves retrieval;
- whether the agent can navigate from the index to the authoritative source;
- whether every item really needs to be advertised.

### Important lesson from Codex

Even the **Skill catalog** is treated as having a context budget. Apply the same thinking to a Memory Bank index.

An index is not free merely because it is an index.

---

# 12. Inspect Tool and MCP Context

Do not blame memory for all startup bloat.

Measure tool/MCP/plugin definitions separately.

Look for:

- large tool descriptions;
- duplicated tools;
- tools that expose capabilities the agent rarely needs;
- plugins that automatically add large tool surfaces;
- multiple interfaces to the same capability;
- descriptions that include procedural documentation better suited to a Skill/reference.

The relevant question is:

> **How much context does the capability cost before the agent ever uses it?**

---

# 13. Inspect Deterministic Output Noise

Even if startup context is healthy, session growth can become pathological through tool output.

Look for commands that return:

- entire build logs;
- entire test logs when only failures matter;
- large file listings;
- generated files;
- repeated status information;
- verbose scripts whose useful signal is only a few lines.

Prefer structured, bounded results where practical.

Do not blindly truncate output: preserve the evidence needed to diagnose failures.

---

# 14. Measure Before and After Any Change

Every meaningful context optimization should have a baseline and a comparison.

At minimum:

```text
Startup context:
  before: ____ tokens / ____ %
  after:  ____ tokens / ____ %

Task result:
  success: yes/no
  correctness: ____
  verification: ____
  corrective turns: ____
  retries: ____
```

A lower token count is not sufficient evidence of improvement.

If reliability falls or corrective work rises, the optimization may have moved cost rather than removed it.

---

# 15. Do Not Copy PMB Problems Into MB Blindly

PMB is a guide, not proof of the work MB's implementation.

When PMB reveals a problem, classify it before assuming MB has the same problem:

### Likely inherited architectural pattern

Examples:

- duplicated context layers;
- always-loaded memory architecture;
- Skill/index structure;
- persistent instruction organization.

These are reasonable things to inspect in MB.

### PMB-specific implementation problem

Examples:

- PMB-specific command behavior;
- PMB-specific storage format;
- PMB-specific retrieval implementation.

Do not assume MB has the same defect.

### Provider/runtime-specific behavior

Examples:

- Claude-specific loading behavior;
- plugin-specific injection;
- model-specific context accounting.

These require separate verification before being treated as MB architecture.

---

# 16. Prioritize the MB Investigation

If time is limited, investigate in this order:

1. **Fresh-session startup context size.**
2. **What exactly contributes to that startup size.**
3. **Memory/index loading behavior.**
4. **Global/project instruction size and duplication.**
5. **Skill metadata and activation structure.**
6. **Tool/MCP/plugin definition overhead.**
7. **Historical/current-state separation.**
8. **Deterministic tool output growth.**
9. **Scoped Skills/rules opportunities.**
10. **Only then consider new retrieval, orchestration, or memory infrastructure.**

This order deliberately attacks observed cost before hypothetical architecture.

---

# 17. Candidate Findings to Record During Review

Use these classifications:

### OBSERVED

Measured or directly demonstrated in PMB/MB behavior.

### INFERRED

Strongly suggested by the PMB-derived architecture but not directly verified in MB.

### HYPOTHESIS

Plausible risk requiring investigation.

### RECOMMENDATION

A change supported by observed evidence.

### DEFERRED

Potential improvement with insufficient evidence or poor cost/benefit.

This prevents the MB review from turning PMB observations into unverified facts.

---

# 18. Desired End State

The review should not produce "the smallest Memory Bank."

The desired outcome is:

```text
Fresh session
    │
    ├── small universal context
    │
    ├── cheap capability discovery
    │
    └── clear map to durable knowledge
             │
             ▼
       task determines need
             │
       ┌─────┴─────┐
       ▼           ▼
    Skill       direct retrieval
       │           │
       └─────┬─────┘
             ▼
       minimal useful context
             │
             ▼
       deterministic tools where possible
             │
             ▼
       evidence / verification
```

The model should have access to deep knowledge without having to carry all deep knowledge at session startup.

---

# Final Review Questions

Before modifying the work MB, answer these explicitly:

1. What percentage of the context window is consumed at fresh startup?
2. What are the top five startup contributors?
3. Which startup content is required for nearly every task?
4. Which startup content is only occasionally relevant?
5. How much memory is loaded versus merely indexed?
6. How much Skill metadata is loaded?
7. How large are the Skills when activated?
8. How much tool/MCP/plugin metadata is loaded?
9. Where is information duplicated?
10. What is the authoritative owner of each duplicated concept?
11. Which content is historical but being treated as current?
12. Which procedures could be moved behind a Skill or reference boundary?
13. Which deterministic operations should be performed by code/tools instead of model reasoning?
14. Which Skills or memory categories are too broad?
15. Which capabilities can be scoped to a task, domain, or path?
16. What measurable evidence says a proposed context reduction will preserve or improve reliability?
17. What new complexity would the proposed optimization introduce?
18. Is the proposed change solving an observed problem or a hypothetical one?

## Exit Criterion

Do not declare the MB context architecture "fixed" because token usage decreased.

The review is complete only when we can explain:

- what is loaded;
- why it is loaded;
- when deeper information is loaded;
- who owns it;
- how it stays current;
- what it costs;
- and what evidence shows the resulting context is sufficient for the work.
