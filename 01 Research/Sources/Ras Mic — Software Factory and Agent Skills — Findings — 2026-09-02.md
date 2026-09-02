# Ras Mic — Software Factory and Agent Skills — Findings

**Date:** 2026-09-02  
**Status:** Research / candidate capabilities; no implementation commitment  
**Primary sources:** Ras Mic video and `michaelshimeles/skills`; follow-on investigation of `michaelshimeles/ralphy`

## Executive assessment

Ras Mic's work is valuable less as a package to copy than as evidence for a compact engineering operating model:

```text
Isolate → Build → Prove → Ship
```

The important contribution is the decomposition of a software factory into small, trigger-focused capabilities composed by a thin workflow document rather than one monolithic skill. The repository README describes six principal skills and an `AGENTS.md` that composes them into the four-beat lifecycle. The skills repository is therefore best treated as a source of reusable engineering patterns, not as an architecture to import wholesale.

The associated Ralphy project broadens the evidence considerably: task ingestion, multiple agent engines, model overrides, parallel execution, worktree isolation, sandbox fallback, browser automation, retries, project rules, GitHub issue synchronization, PR creation, dry-run/no-commit modes, and execution limits form a portable execution substrate around coding agents.

The most promising findings for Harness Engineering are:

1. task isolation as a first-class capability;
2. evidence as a structured artifact rather than a prose claim;
3. before/after baseline capture as a generalized measurement pattern;
4. bounded evaluation/remediation loops;
5. graceful fallback when an execution strategy hits scale or environment limits;
6. explicit separation of workflow orchestration from individual skills;
7. multi-engine execution behind a common task contract;
8. observation of repeated human behavior as a source of candidate workflow/skill improvements.

These are candidates for further evidence work, not adopted architecture.

## Source set

### Ras Mic video

User supplied video: `https://www.youtube.com/watch?v=blI10_91xgA`

The video presents the idea that a software factory is a portable workflow consisting of an `AGENTS.md` plus five core beats/capabilities: isolate, build, prove, ship, and coordinate. The demonstrated workflow is intended to be harness/model agnostic, while the actual implementation still relies on concrete infrastructure such as Git, GitHub, and Greptile.

### GitHub skills repository

`https://github.com/michaelshimeles/skills`

The README identifies these current workflow skills:

- `new-feature`
- `code-structure`
- `evidence-driven-testing`
- `before-and-after`
- `greploop`
- `greploop-apps`

The README states that `AGENTS.md` ties them together as:

```text
isolate (`new-feature`)
    → build (`code-structure`)
    → prove (`evidence-driven-testing`)
    → ship (`before-and-after` + `greploop`)
```

### Ralphy

`https://github.com/michaelshimeles/ralphy`

Ralphy describes itself as an autonomous AI coding loop and supports single tasks and task lists/PRDs, multiple task formats, GitHub Issues, parallel agents, worktrees, branch-per-task, sandbox mode, browser automation, notifications, multiple AI engines, model overrides, engine-specific arguments, retries, iteration limits, project rules, and several safety/operational switches.

## 1. Software factory as a composed workflow

The strongest architectural lesson is not any individual skill. It is composition.

A thin workflow layer names the lifecycle; individual skills own specific behaviors.

```text
                    AGENTS.md
                        │
       ┌────────────────┼────────────────┐
       ▼                ▼                ▼
    Isolate            Build            Prove
       │                │                │
       └────────────────┴────────┬───────┘
                                ▼
                              Ship
```

This provides a useful alternative to the common failure mode of accumulating instructions in one enormous `CLAUDE.md` or mega-skill.

Potential Harness principle:

> Compose small, trigger-focused capabilities behind a small lifecycle contract; do not turn the lifecycle itself into one giant instruction set.

## 2. `new-feature`: task isolation

The `new-feature` skill requires every task to start from current `origin/main`, checks open PRs for overlapping files, refuses to proceed when concurrent work overlaps, creates an isolated worktree/branch for other harnesses, and verifies the resulting branch before execution. It explicitly warns that worktrees do not isolate shared ports, databases, or other shared resources.

This is more than Git hygiene. It is a concurrency protocol.

### Candidate capability

**Task Isolation / Collision Detection**

```text
synchronize
   ↓
establish task identity
   ↓
inspect concurrent work
   ↓
check scope overlap
   ↓
establish isolated execution context
   ↓
verify isolation
   ↓
execute
```

Potential applications extend beyond PMB and ACR:

- multi-agent coding;
- autonomous task runners;
- experiments;
- batch transformations;
- generated documentation;
- repository maintenance.

### Important limitation

Worktree isolation is not full environment isolation. Shared services, ports, databases, credentials, caches, and external state remain possible collision surfaces. Any generalized task-isolation capability must therefore distinguish **filesystem/Git isolation** from **runtime/resource isolation**.

## 3. `evidence-driven-testing`: evidence is a first-class artifact

The skill is substantially broader than UI screenshots. Its protocol records the exact revision/environment, establishes test targets as testable statements, captures before state, performs live interactions, records timestamped assertions, distinguishes passed/failed/untested, generates a report and manifest, and provides headless and non-UI evidence paths.

For non-UI changes, the skill calls for measured numbers, output pairs, transcript excerpts, or reproducible probes rather than screenshots.

The key abstraction is:

```text
Claim
  ↓
Evidence acquisition
  ↓
Assertion
  ↓
Result
```

This aligns strongly with the Harness evidence-first direction.

### Candidate generalized Harness primitive

**Claim → Evidence → Result** should be investigated as a cross-domain evidence model.

Examples:

```text
Claim: startup context decreased.
Evidence: measured token/context values before and after.
Result: PASS.
```

```text
Claim: ACR catches the seeded secret.
Evidence: controlled fixture + scanner/reviewer output.
Result: PASS.
```

```text
Claim: UI flow succeeds.
Evidence: live interaction + timestamped assertion.
Result: PASS.
```

The important point is that the evidence object should be independently inspectable; a model statement that something worked is not itself proof.

## 4. Before-state capture is a generalized measurement pattern

The video and skill emphasize capturing the failure/baseline before applying the fix, when that evidence is cheapest to obtain.

The reusable pattern is broader than UI:

```text
Baseline
   ↓
Change
   ↓
Re-measure
   ↓
Compare
```

Potential domains:

- performance;
- context/token cost;
- memory quality;
- security findings;
- API behavior;
- generated artifacts;
- tests;
- UI;
- reviewer findings.

This should be investigated as a Harness-level evidence pattern.

## 5. `code-structure`: useful meta-pattern, not universal architecture

The skill prescribes a two-layer service architecture: actions own domain rules and orchestration while services own reusable operational mechanics. It also gives extraction triggers and anti-patterns such as god services and over-abstraction.

The useful transferable lesson is not the service-layer architecture itself.

It is:

> A project can encode its preferred architectural vocabulary into a small, trigger-focused skill that agents apply when relevant.

Harness Engineering should not elevate one project's architecture into a universal agent rule.

## 6. `greploop`: bounded feedback control

`greploop` iterates over PR review, remediation, re-review, and thread resolution, with a maximum of five iterations. The current implementation uses Greptile's 5/5 confidence and zero-unresolved-comments result as its stopping condition.

The transferable capability is:

```text
implement
   ↓
evaluate
   ↓
classify findings
   ↓
remediate
   ↓
re-evaluate
   ↓
bounded termination
```

The external score should not automatically become a universal quality gate. A reviewer score can be useful evidence while still being an imperfect evaluator.

### Candidate principle

> Iterative quality loops should terminate on independent, explicit acceptance criteria rather than on a model/reviewer declaring itself satisfied.

This is directly relevant to ACR and autonomous Harness loops.

## 7. `greploop-apps`: graceful degradation at scale boundaries

The companion skill exists because the normal Greptile path has a file-count limitation. It uses a different trigger path for oversized PRs.

The transferable lesson is:

> When the preferred execution path reaches a known scale boundary, preserve the capability contract and substitute an alternate execution strategy where possible.

Potential Harness pattern:

```text
normal path
   ↓
capacity/scale boundary
   ↓
chunk / alternate evaluator / staged execution
   ↓
same evidence contract
```

This is a candidate pattern, not a reason to reproduce the Greptile-specific implementation.

## 8. Ralphy: execution substrate

Ralphy extends the software-factory concept into a task execution layer.

### Task sources

It accepts single tasks plus Markdown, Markdown folders, YAML, JSON, and GitHub Issues. JSON and YAML can encode parallel groups; Markdown tasks can be tracked per file.

Potential reusable concept:

> Separate **task representation** from **agent execution**.

A Harness can accept a durable task contract and map it onto different execution engines.

### Multi-engine abstraction

Ralphy supports Claude Code, OpenCode, Cursor, Codex, Qwen-Code, Factory Droid, GitHub Copilot, and Gemini CLI. It also supports model overrides and engine-specific arguments.

This is strong evidence for a distinction between:

```text
Task intent
   ↓
Execution adapter
   ↓
Selected agent/model
   ↓
Observed result
```

The useful architectural idea is an explicit execution boundary, not the specific list of supported CLIs.

This reinforces our existing separation of model identity from execution identity.

### Model override

The `--model` facility demonstrates that the model can be treated as a variable of execution rather than as the whole harness identity.

Potential Harness implication:

```text
Task
 + execution profile
 + model selection
 + tool permissions
 + verification policy
```

This is relevant to HE-001 execution/runtime provenance.

## 9. Parallel execution and worktrees

Ralphy can run multiple agents in parallel, assigning each a task-specific worktree and branch. It supports branch-per-task, PR creation, no-merge mode, and configurable parallelism.

The strongest lesson is not "run three agents." It is that parallelism needs an explicit isolation model and a defined merge/hand-off policy.

Potential requirements for a future generalized parallel executor:

- unique task identity;
- isolated filesystem state;
- branch/worktree ownership;
- shared-resource awareness;
- bounded concurrency;
- explicit merge authority;
- conflict handling;
- provenance of which agent produced which artifact.

## 10. Sandbox mode: isolation has performance tradeoffs

Ralphy uses a lighter sandbox strategy when full Git worktrees are too expensive for large repositories. Dependencies can be symlinked read-only while mutable source is copied. It also reports automatic fallback to sandbox mode when worktree operations fail.

This is an interesting execution-layer lesson:

> Isolation is a capability with multiple implementations and cost profiles.

Potential future Harness abstraction:

```text
Isolation contract
   ├─ Git worktree
   ├─ lightweight sandbox
   ├─ container
   └─ VM
```

The Harness should reason about the isolation guarantee required, not hard-code one mechanism.

## 11. Browser automation as an optional execution capability

Ralphy can auto-detect or explicitly enable/disable browser automation and exposes browser actions such as navigation, element snapshots, clicks, typing, and screenshots.

The useful pattern is capability negotiation:

```text
Task requires browser?
        ↓
Is browser capability available?
        ├─ yes → enable
        └─ no  → headless/alternative/untested path
```

This fits the existing Harness distinction between available tools and execution authority.

## 12. Retry semantics and fatal-error classification

Ralphy has retry limits and detects rate-limit/quota failures so they can be deferred rather than blindly retried. Later versions also improved fatal authentication/configuration handling to prevent infinite retry loops.

This is valuable Harness evidence because retries are frequently treated as a generic loop when they should actually be **failure-class aware**.

Candidate model:

```text
failure
  ├─ transient → retry with bound/backoff
  ├─ quota/rate-limit → defer
  ├─ auth/config → fail fast
  ├─ deterministic task failure → inspect/remediate
  └─ unknown → stop/escalate
```

This should be investigated independently of Ralphy.

## 13. Execution limits are a governance primitive

Ralphy exposes maximum iterations, maximum retries, parallelism limits, dry-run, no-commit, no-merge, and browser enable/disable controls.

These are not merely convenience flags. They form an execution budget.

This corroborates our existing conclusion that autonomous loops need explicit bounds over:

- iterations;
- candidate mutations;
- concurrent agents;
- external calls;
- wall-clock time;
- context/token consumption;
- approver attention.

## 14. Project rules and protected paths

Ralphy's project configuration allows commands, rules, and `never_touch` path patterns. This is a useful example of keeping project-specific policy separate from generic execution mechanics.

Potential separation:

```text
Generic executor
       +
Project policy
       +
Task contract
       +
Execution profile
```

This is preferable to embedding every repository's rules into the executor itself.

## 15. Human behavior → reusable capability

Ras Mic's explanation that the factory emerged from repeated personal workflow is important evidence for a capability-improvement loop.

Observed pattern:

```text
repeated human behavior
        ↓
recognize recurring pattern
        ↓
encode as skill/workflow
        ↓
agent executes repeatedly
```

A governed Harness improvement system could extend this to:

```text
execution history
        ↓
pattern detection
        ↓
candidate capability proposal
        ↓
evidence/evaluation
        ↓
human approval
        ↓
versioned capability
```

This is a safer form of self-improvement than allowing the system to rewrite its own governance or canonical memory.

## 16. What should NOT be transferred directly

### Do not adopt Greptile 5/5 as a universal quality criterion

Useful as one external signal; insufficient as a general trusted evaluator.

### Do not adopt service-layer architecture as a Harness rule

It is a project architecture preference.

### Do not assume Git worktrees equal complete isolation

Shared runtime resources remain possible collision surfaces.

### Do not make autonomous indefinite execution a default

Execution must have explicit bounds and termination conditions.

### Do not copy the entire skill collection into PMB/ACR

The collection itself is evidence for small composable skills. Blindly importing it would recreate the skill-bloat problem it helps illustrate how to avoid.

### Do not make vendor tooling part of the abstract capability

`before-and-after`, Greptile, agent-browser, and specific AI CLIs are implementations behind capability boundaries.

## 17. Candidate capability backlog

| Capability | Priority | Likely scope | Rationale |
|---|---|---|---|
| Task isolation + collision detection | High | Harness | Concurrency safety |
| Claim → Evidence → Result | High | Harness | General evidence contract |
| Baseline → Change → Re-measure | High | Harness | Makes improvement measurable |
| Bounded evaluation/remediation loop | High | Harness / ACR | Feedback control |
| Failure-class-aware retry | High | Harness | Avoids runaway retries |
| Isolation implementation abstraction | High | Harness | Worktree/sandbox/container tradeoffs |
| Multi-engine execution adapter | High | Harness | Separates task from agent engine |
| Execution profile/model override | High | HE-001 | Model/runtime provenance |
| Capability negotiation | Medium–High | Harness | Tool availability vs authority |
| Graceful scale fallback | Medium | Harness | Preserve capability under limits |
| Architecture-specific skill pattern | Medium | Any project | Encode local architecture without globalizing it |
| Browser evidence capability | Medium | Evidence layer | Runtime proof for UI tasks |
| Webhook/notification layer | Low–Medium | Executor | Operational convenience |

## 18. Interesting findings worth preserving

These are deliberately recorded even where they are not immediate implementation candidates.

### A. A software factory can be a workflow contract rather than a framework

The `AGENTS.md` model suggests that the durable unit may be a compact lifecycle contract plus replaceable capabilities rather than a monolithic framework.

### B. Isolation itself can be polymorphic

A task may require a guarantee such as "no mutable filesystem collision" rather than specifically "Git worktree." Different implementations can satisfy that contract at different costs.

### C. Evidence can be portable across UI and non-UI work

The same evidence discipline can produce screenshots, measurements, output pairs, transcripts, or probes. This points toward an evidence schema rather than a screenshot feature.

### D. The evaluator is part of the control loop

A review/remediation loop is only as trustworthy as its evaluator and termination criteria. Loop mechanics and evaluator authority should remain separate.

### E. Runtime limits are part of architecture

Iteration, concurrency, retry, and resource budgets should be treated as execution policy, not merely CLI convenience.

### F. Repeated behavior is a source of architecture candidates

A system that can identify recurring human/agent behavior can propose reusable skills, but the transition from observation to durable capability should remain governed.

### G. "Harness agnostic" needs precision

The workflow can be portable across coding-agent harnesses, but it is not literally independent of infrastructure. Git, GitHub, browser tooling, review tooling, and specific CLIs remain execution dependencies. The accurate abstraction is **portable workflow contract with replaceable adapters**.

## 19. Deep-pass questions

The next evidence pass should answer these questions directly from Ralphy's implementation and related repositories:

1. What is the actual internal execution state machine for a task?
2. Where are task status and progress persisted?
3. How are agent outputs parsed and classified?
4. Exactly how are retryable vs fatal failures detected?
5. How are token/cost/duration metrics collected and surfaced?
6. How does parallel merge/conflict resolution work in practice?
7. What protections prevent agents from editing task-control files?
8. How are browser capabilities detected and injected into agent prompts/tools?
9. What does sandbox fallback guarantee, and where can it leak shared state?
10. How does the multi-engine adapter normalize materially different agent semantics?
11. What evidence is retained after a task finishes?
12. What happens after partial failure in a parallel group?
13. Which controls are hard enforcement versus prompt-level instruction?
14. Which ideas are actually implemented versus README promises?

## 20. Current disposition

**Adopt as research principles:**

- small composable capabilities;
- workflow orchestration separated from capability implementation;
- explicit task isolation;
- evidence as an artifact;
- baseline/before-state capture;
- bounded remediation loops;
- failure-class-aware retry;
- execution budgets;
- replaceable isolation and execution adapters;
- project policy separated from generic executor mechanics.

**Investigate before adoption:**

- generalized task-isolation capability;
- multi-engine execution;
- execution profiles;
- sandbox/worktree abstraction;
- evidence manifest schema;
- browser capability negotiation;
- scale fallback;
- Ralphy task state/provenance model.

**Do not adopt directly:**

- Greptile 5/5 as a universal gate;
- service-layer architecture as universal Harness architecture;
- indefinite autonomous execution;
- wholesale import of Ras Mic's skill collection.

## Conclusion

Ras Mic's work is useful because it demonstrates a practical way to turn repeated engineering behavior into small, composable agent capabilities. The deeper lesson is not "install these six skills." It is that a software factory can be modeled as a sequence of bounded capabilities with explicit isolation, evidence, evaluation, and shipping contracts.

Ralphy adds an execution-layer perspective that is particularly relevant to the Harness model: task intent, agent engine, model, isolation mechanism, tools, retries, concurrency, and verification can be treated as separate variables. That separation should be investigated against the existing Harness Engineering execution/provenance model.

The strongest emerging synthesis is:

```text
Workflow contract
      ↓
Task contract
      ↓
Execution profile
      ↓
Isolated execution
      ↓
Evidence
      ↓
Independent evaluation
      ↓
Bounded remediation
      ↓
Versioned artifact / PR
```

That is a research direction, not a proposed implementation architecture.