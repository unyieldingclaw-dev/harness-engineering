# Ras Mic — Ralphy Deep Evidence Pass

**Date:** 2026-09-02  
**Status:** Evidence pass / research; no implementation commitment  
**Repository examined:** `michaelshimeles/ralphy`  
**Evidence revision examined:** `506eea0e7d72c8eeb96dd2f697363bef396add34`

## Scope

This pass moves below README-level claims and inspects implementation files where available. It focuses on execution state, boundaries, retries, metrics, task sources, isolation, prompt construction, and multi-engine behavior.

## 1. Execution options are explicit and composable

`cli/src/cli/args.ts` exposes independent controls for engine, model, dry-run, maximum iterations, retries, retry delay, parallelism, sandbox/worktree selection, branch/PR behavior, task source, tests/lint, browser capability, commit behavior, merge behavior, and engine-specific arguments. The parser then normalizes these into a `RuntimeOptions` object.

This is significant because the executor is not treating "the agent" as one indivisible thing. Model selection, engine, isolation, verification, concurrency, and publication behavior are separate runtime variables.

### Harness implication

An execution profile can reasonably be treated as a first-class artifact:

```text
Execution Profile
├── agent engine
├── model override
├── tool/capability settings
├── isolation mode
├── concurrency
├── retry policy
├── verification policy
└── publication/merge policy
```

This strongly corroborates the existing HE-001 direction to separate model identity from execution identity.

## 2. Important finding: Ralphy permits unlimited iteration by default

The CLI defines `--max-iterations <n>` with `0 = unlimited` and defaults it to `0`. The README describes the same behavior. Therefore, the existence of an iteration flag does **not** by itself mean the default autonomous loop is bounded.

This is an important correction to any earlier interpretation that Ralphy's mere presence of a maximum constitutes a safety boundary.

### Disposition

**Do not copy the default.**

For governed autonomous execution, a finite default should be considered a policy choice, and unlimited mode should require explicit authorization if supported at all.

## 3. Retry controls are explicit, but failure classification matters

The runtime exposes maximum retries and retry delay. The engine layer contains a dedicated `isRetryableError` path, while authentication errors are separately detected and formatted. The README/changelog reports explicit handling for rate-limit/quota failures and fatal authentication/configuration errors.

The implementation therefore distinguishes at least two classes:

```text
engine failure
├── retryable → retry policy
└── authentication/configuration → surface failure
```

This is better than blindly retrying every non-zero exit.

### Harness implication

A future retry capability should classify failures before consuming retry budget. Retry budget should be attached to the failure class, not merely the task.

## 4. Authentication failure handling is deliberately normalized

`cli/src/engines/base.ts` parses structured stream output and checks error records for authentication-related messages such as invalid API key, authentication failure, unauthorized, and login requirements. It returns a clean authentication error instead of allowing a large raw output dump to become the primary diagnostic.

This is a small but useful operational pattern:

> Normalize known failure classes into actionable diagnostics before generic error reporting.

It is relevant to Harness observability and failure classification, but should not be mistaken for a comprehensive error taxonomy.

## 5. Output parsing captures usage metrics for supported stream formats

The base engine implementation parses `result` records from stream-json output and extracts `input_tokens` and `output_tokens`. It also detects structured error records.

This means Ralphy has an actual measurement path for token usage, at least for engines emitting the expected structured format.

### Important limitation

The README's engine table shows that different engines report different quantities: some expose tokens + cost, some tokens, some duration. Therefore the common result model does not imply identical measurement fidelity across engines.

### Harness implication

Execution provenance should record both:

```text
measurement available
vs.
measurement unavailable / incomparable
```

A missing cost metric should not silently become zero.

## 6. Prompt construction is where some enforcement currently lives

`cli/src/execution/prompt.ts` constructs prompts from:

- project context;
- project rules;
- system boundaries;
- user boundaries;
- discovered skill roots;
- browser instructions;
- task text;
- test/lint instructions;
- commit instructions.

The prompt explicitly tells the agent not to modify the PRD, `.ralphy/progress.txt`, `.ralphy-worktrees`, and `.ralphy-sandboxes`.

This is useful policy communication, but it is **prompt-level enforcement**.

The executor itself separately owns task completion/progress behavior, worktree creation, and sandbox management.

### Harness implication

This is another concrete example of the rule:

> Instructions can communicate an authority boundary; they do not prove that the boundary is structurally enforced.

For critical boundaries, we should inspect the executor/tool layer and not count the prompt alone as enforcement.

## 7. There is a notable progress-file boundary inconsistency worth investigating

The parallel prompt receives `.ralphy/progress.txt` as a progress file and includes an instruction to update it, while the system boundaries also identify `.ralphy/progress.txt` as a file the agent must not modify. Separately, the executor contains `logTaskProgress`, which appends completion/failure entries itself.

This indicates the intended architecture is that **the orchestrator owns progress state**, not the agent. The prompt wording is therefore at least potentially confusing and deserves closer testing.

### Important lesson

A durable control artifact should have one clear owner. If the orchestrator owns task state, the agent should report outcome rather than edit the control ledger directly.

## 8. Progress logging is asynchronous and intentionally non-critical

`cli/src/config/writer.ts` implements a debounced write queue for progress logging. Entries are buffered and appended within a 100ms window. The flush path catches write errors and ignores them. A separate `flushAllProgressWrites()` exists for process-exit durability.

This is an interesting tradeoff:

```text
progress log
   ↓
performance optimized
   ↓
non-blocking / batched
   ↓
write failure does not fail task
```

### Harness implication

This is appropriate if progress logging is informational telemetry. It would be inappropriate if progress state were the authoritative source for task completion or auditability.

That distinction should be explicit in any Harness provenance design.

## 9. Markdown task identity is line-number based

`MarkdownTaskSource` parses `- [ ]` tasks and uses the source line number as the task ID. It caches file content and task counts, invalidating the cache when modification time changes. `markComplete` then replaces the checkbox on the identified line.

This is simple and efficient, but line-number IDs are inherently unstable when lines are inserted or removed.

### Harness implication

For durable task identity across edits, a stable task identifier is preferable to a positional identifier.

This is a candidate lesson for any future task-contract implementation.

## 10. Parallel execution has a clear orchestration boundary

`cli/src/execution/parallel.ts` creates an agent result record containing task, agent number, worktree directory, branch name, result, error, and sandbox usage. It creates the isolation environment, copies the task source, builds the prompt, executes with retry, and returns structured results.

The orchestrator therefore owns:

- task assignment;
- isolation;
- prompt construction;
- retry;
- result collection;
- merge-phase inputs.

The agent engine owns the actual agent invocation.

This is a strong architectural separation.

## 11. Parallelism is bounded by `maxParallel`

The executor batches tasks and limits each batch to `maxParallel`. The CLI default is three. Parallel task groups can be represented in YAML/JSON, and execution waits between groups where the task source supplies group semantics.

This is a useful example of **bounded concurrency as a first-class execution policy**.

It should be distinguished from worktree isolation: one protects concurrent mutable state; the other controls resource pressure.

## 12. Ralphy has multiple isolation implementations

The parallel executor chooses between Git worktrees and lightweight sandboxes. It checks whether worktrees can be used and can fall back to sandbox mode. Sandbox mode uses copied mutable source plus symlinked read-only dependencies.

This is strong evidence for the abstraction:

```text
Required isolation guarantee
          ↓
implementation selected by environment/cost
          ├── worktree
          └── sandbox
```

The exact guarantee of each implementation still needs a deeper filesystem/process experiment before either should be considered equivalent.

## 13. Sandbox mode is not equivalent to full isolation

The README states that dependencies and `.git` can be symlinked read-only while source files are copied. This improves startup cost for large repositories, but shared dependencies, caches, external services, ports, credentials, and other resources can remain outside the copied source boundary.

### Harness implication

Isolation should be described in terms of guarantees, for example:

- mutable source isolation;
- Git metadata isolation;
- dependency isolation;
- process isolation;
- network isolation;
- credential isolation;
- database isolation.

A single `isolated=true` boolean would be misleading.

## 14. Browser support is capability negotiation, not a mandatory engine property

Prompt construction checks whether browser capability is available and can inject browser instructions. The CLI supports `--browser`, `--no-browser`, and auto mode. This lets the same task executor adapt to the runtime's capabilities.

This reinforces the distinction:

```text
Task requirement
      +
Tool availability
      +
Execution authority
```

A browser being installed is not equivalent to permission to use it, and permission is not equivalent to the task actually requiring it.

## 15. Skills are discovered at execution time

The prompt builder looks for skill/playbook roots under `.opencode/skills`, `.claude/skills`, `.github/skills`, and `.skills`. If present, it tells the agent to read relevant skills and, where supported, use the engine's skill tool.

This is a concrete progressive-disclosure pattern:

```text
execution
   ↓
detect local capabilities
   ↓
load only relevant guidance
   ↓
execute task
```

This supports our existing anti-bloat direction better than injecting every skill into every prompt.

## 16. Engine-specific arguments preserve escape hatches

The CLI supports a `--` separator and passes all following arguments directly to the selected engine. This avoids forcing the generic executor to understand every engine-specific feature.

This is a useful adapter principle:

> Normalize common capabilities, but preserve an explicit escape hatch for engine-specific controls.

The risk is that engine-specific flags can bypass generic policy, so governance needs to distinguish safe extension points from authority escalation.

## 17. Task-source normalization is a useful boundary

The same executor can consume Markdown, Markdown folders, YAML, JSON, or GitHub Issues. This indicates that task acquisition and task execution are intentionally separated.

Potential Harness model:

```text
Source adapter
   ↓
normalized Task
   ↓
execution planner
   ↓
agent execution
```

This is a strong reusable architecture pattern independent of Ralphy.

## 18. Dry-run is a valuable planning capability

Ralphy's dry-run path identifies the next batch, reports what would run, tracks processed task IDs locally, and deliberately avoids mutating the task source.

This is more than a convenience flag. It provides a **zero-mutation planning phase** before execution.

Potential generalized Harness capability:

```text
plan / inspect
      ↓
show intended mutations
      ↓
human or policy approval
      ↓
execute
```

This is especially attractive for high-authority operations.

## 19. Merge authority is explicitly configurable

Parallel mode can auto-merge, create PRs, or keep branches without merging. This separates execution from publication/merge authority.

This is highly relevant to our authority model:

```text
agent may modify task branch
        ≠
agent may merge to base
```

The same separation should be maintained in other autonomous workflows.

## 20. Ralphy's model override strengthens the execution-identity distinction

The same engine can run with a selected model override. The engine itself is therefore not sufficient to identify the model that produced an artifact.

For provenance, a run should distinguish at least:

```text
engine
model
model variant/version where known
runtime/CLI version
execution mode
tool configuration
task
commit/base
verification result
```

This is directly relevant to HE-001.

## 21. The current implementation suggests a stronger generalized state machine

From the observed orchestration, a useful abstract state model is:

```text
DISCOVER TASK
      ↓
PLAN / DRY RUN
      ↓
ESTABLISH ISOLATION
      ↓
BUILD EXECUTION CONTEXT
      ↓
RUN AGENT
      ↓
CLASSIFY RESULT
   ┌──┴───────────┐
   │              │
retryable      terminal
   │              │
retry           verify
   │              │
   └──────┬───────┘
          ↓
COLLECT EVIDENCE
          ↓
PUBLISH / MERGE / PR
          ↓
RECORD PROVENANCE
```

This is a synthesis from implementation evidence, not a claim that Ralphy literally implements this exact state machine.

## 22. Deep-pass findings by confidence

### Strongly evidenced

- explicit multi-engine adapter;
- model override independent of engine selection;
- configurable retry limits and delay;
- unlimited iteration default (`0`);
- bounded parallelism;
- worktree/sandbox alternatives;
- task-source adapters;
- project rules and never-touch boundaries;
- prompt-time skill discovery;
- structured token parsing for supported engines;
- dry-run mode;
- configurable commit/merge/PR behavior;
- orchestrator-owned progress logging;
- structured parallel agent result objects.

### Strong inference

- execution profile is a useful abstraction;
- task isolation should be modeled as a guarantee rather than a mechanism;
- progress/control artifacts should have a single owner;
- task representation should be separated from execution;
- capability negotiation should distinguish availability from authority;
- failure classification should precede retry consumption.

### Still requiring direct experiment

- exact process/file isolation guarantees of sandbox mode;
- whether shared dependency symlinks can be mutated indirectly;
- exact merge conflict strategy and whether AI conflict resolution can introduce unreviewed semantic changes;
- complete provenance retained after a run;
- cost accounting parity across every engine;
- behavior after partial parallel-group failure;
- hard versus prompt-only enforcement for every boundary;
- security implications of engine-specific argument passthrough;
- behavior of browser sessions/credentials across isolated agents.

## 23. New capability candidates from the deep pass

### Priority 1 — Investigate

1. **Execution Profile** — canonicalize engine/model/runtime/tool/isolation/retry/verification configuration.
2. **Task Contract** — normalize tasks from multiple sources before execution.
3. **Isolation Contract** — specify guarantees and select worktree/sandbox/container implementations.
4. **Evidence Contract** — claim/evidence/result with manifests and provenance.
5. **Failure Taxonomy** — retry/defer/fail/escalate based on classified failure.
6. **Bounded Concurrency** — explicit agent/resource budget.
7. **Dry-Run / Mutation Preview** — inspect intended changes before high-authority execution.
8. **Publication Authority** — explicitly separate edit, commit, PR, merge, and deploy permissions.

### Priority 2 — Investigate

9. **Capability Negotiation** — determine tool availability and task requirements before execution.
10. **Skill Discovery / Progressive Disclosure** — load only relevant guidance.
11. **Scale Fallback** — switch execution mechanisms while preserving the same capability contract.
12. **Stable Task Identity** — use durable IDs rather than line positions.
13. **Operational Diagnostics** — normalize common fatal failures into actionable categories.

## 24. Rejected or constrained transfers

- Do not adopt unlimited iterations as a default autonomous behavior.
- Do not equate worktree or sandbox with complete environment isolation.
- Do not make prompt boundaries the sole enforcement mechanism for correctness-critical paths.
- Do not make third-party reviewer scores the universal termination condition.
- Do not import Ralphy's entire CLI/configuration surface into another project; its breadth is evidence for separation of concerns, not evidence that every project needs every flag.

## Conclusion

The deep evidence pass makes Ralphy more interesting than the skills repository alone. It is a concrete example of an execution substrate that separates task source, agent engine, model override, isolation, concurrency, browser capability, retry policy, and publication behavior.

The most important new conclusion is that our Harness model should probably treat **execution profile, task contract, isolation contract, evidence contract, and publication authority** as separate objects/capabilities rather than burying them inside a single agent-runner abstraction.

The second important conclusion is negative: configurable limits are not automatically governance. Ralphy explicitly permits unlimited iterations by default, and several important boundaries are communicated through prompts rather than guaranteed structurally. The research value is therefore in the separation patterns and operational mechanics, not in assuming the system is itself a reference governance model.
