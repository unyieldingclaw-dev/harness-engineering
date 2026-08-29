# Agent Skills & Context Efficiency — Reputable Guidance

**Research date:** 2026-08-29  
**Status:** Research / assessment input; no architecture adopted by default

## Purpose

Capture the most useful current guidance from reputable agent-platform sources on Skills, progressive disclosure, context budgets, and context-efficient harness design.

The goal is not to copy one vendor's implementation. The goal is to identify converging engineering principles that can be tested against PMB and, later, the work Memory Bank (MB).

---

## Primary Sources

### Anthropic — Agent Skills

Source: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview

Anthropic describes Skills as filesystem-based, modular capabilities that load progressively:

1. **Metadata** — name and description are available at startup for discovery.
2. **SKILL.md** — loaded when the Skill is triggered.
3. **Resources/scripts** — loaded or executed only when needed.

Anthropic currently describes approximately **100 tokens per Skill** for startup metadata and recommends keeping the loaded Skill body under **5,000 tokens**. It also recommends moving detailed material into references and using scripts for deterministic work so the script source itself does not consume model context.

Useful principle:

> Capability should be cheap to discover and expensive only when actually used.

### Anthropic — Skill Creator

Source: https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md

Important guidance:

- The Skill description is the primary triggering mechanism.
- Put both **what the Skill does** and **when it should be used** in the description.
- Keep the main Skill instructions concise.
- Keep `SKILL.md` below roughly 500 lines; split deeper material into references when approaching that boundary.
- Avoid duplicating information between `SKILL.md` and reference files.
- Bundle deterministic/repetitive code as scripts instead of repeatedly asking the model to recreate it.
- Test Skills with representative prompts and iterate based on observed performance rather than judging the text alone.
- Treat repeated work across evaluations as evidence that a reusable script or resource may belong in the Skill.

The important point is that Anthropic's own Skill Creator treats Skill authoring as an **evaluated engineering artifact**, not simply prompt writing.

### OpenAI — Codex Skills

Source: https://github.com/llms-txt-archive/openai-platform/blob/main/codex/skills.md

Codex also uses progressive disclosure:

- initial context contains Skill name, description, and path;
- full `SKILL.md` is loaded only when the Skill is selected;
- scripts and references are separate resources.

Codex explicitly constrains the **initial Skill catalog** to approximately **2% of the model context window**, or **8,000 characters when the context window is unknown**. If many Skills are installed, descriptions are shortened first and some Skills may be omitted from the initial list.

This is an especially useful finding because it demonstrates that the **capability index itself is considered context overhead**.

### Microsoft — Agent Skills

Source: https://learn.microsoft.com/en-us/agent-framework/agents/skills

Microsoft independently describes a four-stage progressive-disclosure model:

1. Advertise — roughly 100 tokens per Skill.
2. Load — under roughly 5,000 tokens recommended for `SKILL.md`.
3. Read resources — only when required.
4. Run scripts — only when required.

Microsoft also recommends keeping `SKILL.md` under 500 lines and moving detailed reference material into separate files.

This is useful independent corroboration rather than evidence that PMB should adopt Microsoft's exact implementation.

### Cursor — Agent Skills

Source: https://prod.cursor.com/docs/skills

Cursor describes Skills as portable, version-controlled packages that can include instructions, scripts, templates, and references. It emphasizes progressive loading and supports **path-scoped Skills**, allowing guidance to surface only when the agent is working on matching files.

Cursor also distinguishes Skills from Rules:

- Rules: short coding guidelines and constraints.
- Skills: multi-step workflows and procedures.

The path-scoping mechanism is particularly relevant to reducing irrelevant context exposure.

### OpenAI — Harness Engineering

Source: https://openai.com/index/harness-engineering/

OpenAI reports that a large `AGENTS.md` approach failed because:

- context is scarce;
- excessive guidance crowds out task/code/relevant documentation;
- when everything is important, nothing is important;
- monolithic guidance becomes stale;
- a large blob is difficult to mechanically verify.

Their resulting pattern is to treat a short `AGENTS.md` as a **map/table of contents**, with deeper repository knowledge maintained elsewhere.

This is directly relevant to PMB and MB because a memory or instruction system can fail in the same way even when the information itself is useful.

---

## Converging Principles

Across Anthropic, OpenAI, Microsoft, and Cursor, the strongest common pattern is:

### 1. Cheap discovery, expensive activation

The agent needs enough metadata to discover a capability, not the full capability at startup.

### 2. Progressive disclosure

Load the smallest useful layer first. Deeper references should be available without being automatically injected.

### 3. Separate procedure from reference material

The active Skill should contain the workflow and selection logic. Detailed specifications, examples, schemas, and edge cases can live in references.

### 4. Separate instructions from deterministic execution

If code can perform a deterministic operation reliably, use a script/tool and return its result instead of spending model context explaining or recreating the operation.

### 5. Avoid duplication

Information should have one authoritative home whenever practical. Duplicated guidance increases context cost and creates contradiction risk.

### 6. Bound the capability catalog

A large Skill inventory is not free. Discovery metadata itself consumes context and can become an architectural bottleneck.

### 7. Scope context where possible

File/path/domain/task-scoped activation is preferable to globally available guidance when the guidance is only relevant to a subset of work.

### 8. Evaluate Skills empirically

A Skill is successful only if it improves behavior enough to justify its context and maintenance cost.

### 9. Keep global/project instructions small

Persistent instructions should be a map to durable knowledge and critical constraints, not an encyclopedia.

---

## What This Means for PMB

PMB has a concrete observed problem that makes this research unusually relevant:

> **Startup Context Bloat:** PMB and the work Memory Bank have produced sessions that begin at roughly **40% of the available context window before meaningful task work starts**.

This is user-observed operational evidence, not a theoretical concern.

Therefore, PMB should be assessed against the following questions before adding more persistent guidance or Skills:

- What is loaded at startup?
- How much does each source contribute?
- Which content is truly required for every task?
- Which content is only required for specific workflows?
- Which content is historical rather than current?
- Which content duplicates another source?
- Which information could become a reference loaded on demand?
- Which procedures could become Skills?
- Which deterministic operations could become scripts/tools?
- Which Skills are merely alternative names for existing commands/workflows?
- Is the Skill catalog itself becoming significant startup overhead?

### Important constraint

Do **not** interpret the vendor recommendations as a mandate to convert everything into Skills.

A poorly designed Skill can still be large, frequently triggered, duplicated, or unnecessary. Progressive disclosure is a mechanism, not a reason to create more artifacts.

---

## Proposed Context Budget Model

For assessment purposes, distinguish at least four costs:

```text
Startup Cost
  + Task Activation Cost
  + Retrieval / Reference Cost
  + Session Growth Cost
```

### Startup Cost

Context injected before the task begins:

- system/persistent instructions;
- global/project memory;
- Skill metadata;
- tool/MCP definitions;
- plugin metadata;
- hooks or generated context.

### Task Activation Cost

Context introduced because the current task triggers a Skill, workflow, or other capability.

### Retrieval / Reference Cost

Detailed material read only when needed.

### Session Growth Cost

Conversation history, tool output, repeated file reads, generated artifacts, and other accumulated context.

The four costs should not be conflated. A system that minimizes one while exploding another has not necessarily improved.

---

## Strong Candidate Design Rules

These are **assessment rules**, not yet architecture:

1. **Persistent context must justify its startup cost.**
2. **A capability should be discoverable without loading its full procedure.**
3. **Detailed knowledge should be loaded when needed, not because it exists.**
4. **A Skill should contain only the procedure necessary to use the capability.**
5. **Reference material should not be duplicated inside the Skill body.**
6. **Deterministic work should prefer deterministic execution.**
7. **Capability catalogs need explicit size awareness.**
8. **Scope-specific guidance should not be globally available unless there is a reason.**
9. **Context optimization must be measured against task quality, not tokens alone.**
10. **Do not add a new abstraction unless it removes a demonstrated problem.**

---

## What We Should Measure in PMB

A future PMB context audit should report, at minimum:

| Measurement | Purpose |
|---|---|
| Fresh-session startup tokens | Establish baseline overhead |
| Startup percentage of context window | Identify severe bloat |
| Persistent instruction tokens | Find global/project overhead |
| Memory tokens | Identify always-loaded memory cost |
| Skill metadata tokens | Quantify discovery overhead |
| Tool/MCP definition tokens | Identify capability overhead |
| Activated Skill tokens | Measure procedural cost |
| Reference retrieval tokens | Measure deep-context cost |
| Tool output tokens | Identify noisy deterministic output |
| Session growth | Identify accumulating context |
| Duplicate content | Identify redundant context |
| Task success / correctness | Ensure optimization does not degrade outcomes |
| Corrective turns / retries | Capture hidden cost of over-aggressive trimming |

A token reduction without a quality-preserving result is not automatically an improvement.

---

## Research Disposition

**ADOPT / REINFORCE**

- Progressive disclosure as a design principle.
- Lightweight persistent instructions.
- Separation of Skill metadata, procedure, references, and deterministic scripts.
- Measurement of startup context as a distinct operational metric.
- Explicit attention to Skill-catalog overhead.
- Empirical Skill evaluation.
- Path/domain/task scoping where supported and justified.

**ASSESS**

- PMB Skill structure and loading behavior.
- PMB memory/index startup footprint.
- Whether current PMB artifacts can be reorganized without adding new artifact types.
- Whether some current workflows are better represented as Skills, references, or deterministic commands.
- Whether a context audit should become part of HE-001.

**PARK**

- Vendor-specific Skill runtimes.
- Automatic model routing solely for token savings.
- Large retrieval infrastructure.
- Autonomous memory consolidation.
- New orchestration layers introduced solely to optimize context.

**REJECT**

- A universal giant "master prompt" as the solution to context limits.
- Treating arbitrary Skill-count or file-size thresholds as universal laws.
- Optimizing token count without measuring correctness and rework.
- Creating Skills merely because Skills are fashionable.

---

## Bottom Line

The strongest current industry signal is not "write better prompts."

It is:

> **Design the harness so the model can discover what it needs without carrying everything it might need.**

For PMB, this is no longer merely an architectural preference. The observed ~40% startup context consumption makes context budgeting and progressive disclosure a concrete engineering concern.
