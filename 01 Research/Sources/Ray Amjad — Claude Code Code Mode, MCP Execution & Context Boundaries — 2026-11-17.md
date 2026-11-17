# Ray Amjad — Claude Code Code Mode, MCP Execution & Context Boundaries — 2026-11-17

**Status:** HE source research / evidence preservation. No code/config changes, feature enablement, new platform, implementation authority, or change to any unrelated project. **Primary decision: investigate with bounded A/B tests only if an observed tool/context-cost problem warrants it.**

**Input:** [Ray Amjad, *Code Mode Is Claude Code's BIGGEST Harness Change Yet*](https://www.youtube.com/watch?v=DRNbxPk9Ab8) (Nov 2026), user-supplied **screenshots, video description with timestamps (14:54), and YouTube AI-generated summary**. A complete verbatim video transcript, raw benchmark logs and executable demo were **not provided or independently reviewed**. This note distinguishes screenshots and presenter claims from product documentation and independently reproduced measurements.

## Sources and confidence

| Source | Evidence | Confidence / boundary |
| --- | --- | --- |
| [Anthropic Claude Code: Run tool calls from code with Code Mode](https://code.claude.com/docs/en/code-mode) | Product's experimental behavior, supported versions/surfaces, excluded modes/providers, permission and hook implications. | **Primary product docs, verified 2026-11-17.** Feature explicitly experimental/off by default; docs can change. |
| [Cloudflare: Code Mode—the better way to use MCP](https://blog.cloudflare.com/code-mode/) (2025-09-26) | Code-as-interface pattern: generated code invokes typed tool APIs. | Primary design argument, not evidence of Claude Code-specific results. |
| [Cloudflare: Give agents an entire API in ~1,000 tokens](https://blog.cloudflare.com/code-mode-mcp/) (2026-02-20) and [Cloudflare API MCP repository](https://github.com/cloudflare/mcp) | Server-side search/execute, narrow typed API discovery, sandbox isolation; reports tool-schema footprint comparison (1,170,523 vs ~1,069 tokens) for Cloudflare API. | **Vendor measurement of tool-description overhead**, not a 99.9% reduction in end-to-end session cost, task duration, or failure rates. |
| [Datadog: Code Execution with the MCP Server](https://docs.datadoghq.com/mcp_server/code_execution/) | Vendor-operated sandbox where JavaScript joins and filters telemetry via Datadog APIs before returning compact values. | Primary description, does not establish that all Datadog tools are inaccessible by conventional means. |
| [Executor docs](https://executor.sh/docs), [MCP endpoint](https://executor.sh/docs/mcp), [self-host](https://executor.sh/docs/run/self-host) | A separate, optional MCP hub exposing search/describe/call/execute/resume; connects accounts to a scope, with a self-host mode. | A gateway adds credential/capability concentration and ops costs. **Not required** for Claude Code's built-in Code Mode. |
| [Pydantic: Code mode vs tool calling, 5,276-run benchmark](https://pydantic.dev/articles/why-code-mode-is-the-future) | External comparative result showing workload dependence (including cases where code mode costs more). | Different environment and task family; not direct validation of Anthropic Claude Code's release. |
| [Recursive Language Models](https://arxiv.org/abs/2512.24601) | Research strategy for treating prompts as external environment and using programmatic decomposition / model recursion. | Separate research architecture; **not a shipped Claude Code capability**. |
| [Meta Muse Model API: media understanding](https://dev.meta.ai/docs/media-understanding) and [TubeText for Muse](https://tubetext.app/muse) | Official API can transcribe uploaded audio and supported video; third party offers YouTube transcripts over MCP. | Not proof that Muse consumer app reliably extracts every YouTube URL; TubeText is third-party and unverified. |

## Mechanism: precise boundaries

**Ordinary execution:** model selects one tool → tool output comes back into conversation context → model selects next tool. Tool definitions and large results can consume context and additional model turns.

**Server-side Code Mode (Cloudflare, Datadog, Executor example):** one MCP endpoint exposes a narrow discovery/execution surface; generated JS operates on **that server's** available API/tool set; intermediate responses remain in its execution runtime; an explicit return value enters model context. Isolation and access checks are implemented by the service, not implied by the word “sandbox.”

**Claude Code Code Mode (client-side):** Claude writes JavaScript for a session-scoped REPL runner that composes built-in tools (including read/edit/bash) and accessible integrations. The code can parallelize, chain tool outputs, filter and return selected values without inserting each intermediate result into the model's main prompt. Underlying actions still happen. Tool-call batching is an execution strategy **not** an authorization mechanism, a new memory system, or generalized remote autonomy.

**Implication:** moving intermediary data out of model context can reduce repeated reading, but can also reduce what the model and reviewer observe. Preserve enough durable receipts (commands, read sources, actual effects, diffs, tests, failures) to validate claims and support investigation. **Important nuance:** Anthropic documents that ordinary conversation view exposes nested tool calls, including edit diffs; the JavaScript is accessible via the transcript viewer (`Ctrl+O`). This makes execution *inspectable in principle*, but whether complete nested traces are exported to existing operational telemetry is a separate question—especially because Code Mode currently conflicts with OpenTelemetry export.

**Different degrees of authority:** Datadog's documented `code-exec` runs against **read-only Datadog APIs** under the caller's role, and cannot access local files or arbitrary network destinations. Cloudflare's server-side search/execute can invoke permitted **mutating Cloudflare API endpoints**, depending on credential scope. Claude Code's local REPL can invoke normal authorized filesystem and shell tools. Code Mode is therefore not a single interchangeable sandbox/security design.

**Screenshot-specific Executor example (creator demonstration, not verified execution):** the `21-dreams-mcps` connection exposes `skills`, `search`, `describe`, `call`, `execute` and `resume`. The video displays `execute` marked `destructive · open-world` and text warning that external side effects are **not rolled back by cancellation**; approval-required or input-required flows must **resume the pending operation rather than rerun the program**. That is a useful illustration of the pre-existing HE requirement to track partial results and uncertain external effects, **not proof** that Executor enforces the stated behavior in every failure case.

## Verified Claude Code requirements / limitations (official, 2026-11-17)

- v2.1.323+; **native** Claude Code; interactive terminal session only, **off by default**; supported models Opus 5.5, Sonnet 5.5, Haiku 5.5 and Fable 5.2.
- Enable only for one test session: `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1 claude` (Linux/macOS/WSL). Verify startup message and `/status`. Code Mode sessions require Code Mode enabled to resume; no mixing with a standard session.
- **Not supported with model gateways or custom ANTHROPIC_BASE_URL**, Bedrock and other listed hosted-provider paths. Code Mode does not operate through headless `-p`, the Agent SDK, desktop, cloud sessions, Remote Control, Chrome, or computer use.
- **OpenTelemetry export is incompatible.** Some `PreToolUse`-inspecting mods, plugin configurations, auth helpers and `DISABLE_TELEMETRY` prevent activation. Do not disable an existing production hook or telemetry path casually to enable the feature.
- Agent teams require v2.1.326+ for in-process display; older versions and split-pane teammates may not use Code Mode. The creator's unreleased spawn/list/release-context functions are **speculative**; not evidence of a shipping recursive-agent feature.
- **Permissions:** nested calls still pass through their normal deny/ask rules and `PreToolUse` hooks. However, in auto mode the classifier evaluates **generated code**, not every runtime-resolved tool call. Hooks targeting only old syntactic patterns must be tested. For code-level interception an explicit `REPL` hook can inspect `tool_input.code`; command-text filtering is not a reliable OS isolation boundary.
- Waiting code times out after ten minutes; supported background code can continue up to two hours under specific permission-mode requirements. This is not general background scheduling.
- Failure to start with OTel/mods/gateway is an **intentional compatibility boundary** documented by Anthropic, not evidence that the repo's configuration is defective or should be disabled.

## Claim audit

| Video claim / graphic | Disposition |
| --- | --- |
| “Harness swallowed by model” | **Metaphor, not architecture fact.** Code chooses composition, while runtime, tools, permissions, I/O, credentials and OS effects still live in the harness and connected systems. |
| “99.9% fewer tokens” | **True within Cloudflare's illustrated API *schema* benchmark only.** It contrasts ~2,594 exposed endpoint descriptions against search/execute. Real sessions load task context and tool returns and may see much smaller savings. |
| “My results: ~30% cheaper” (video description, 8:37 chapter) | **Creator-reported experiment**, not independently reproducible from the supplied screenshots/description. No complete raw task list, repeated runs, confidence intervals, exact price accounting, baseline parity or independently verified acceptance checks were provided. Avoid making it an HE ROI claim. |
| “Code Mode is smarter” | **Hypothesis**. Better results may follow cleaner intermediate reasoning, but correctness, test coverage, wrong edits, hidden errors and human approval must be compared. |
| “Future contexts / RLM make compaction unnecessary” | **Speculative roadmap extrapolation**. The RLM paper describes a separate inference design; code mode does not currently expose prompt/history as arbitrary mutable variables or prove cache-safe user-authorized deletion. |
| “Just use a shell script” equivalence | **Incomplete.** Deterministic scripts are already attractive for bounded work; Code Mode's distinction is model-generated programmatic composition across harness tools/MCP. Do not replace known deterministic tasks with generated scripts. |
| "Code mode is safe because sandboxed" | **Not generally true.** Server-side and client-side runtimes have different isolation; a sandbox does not undo API effects or prevent use of a broadly scoped token. |

## HE implications and existing owners

### 1. Smallest concrete value: tools as functions, controlled result projection (REINFORCE)

For genuine repeated multi-tool investigations, programmatic projection can avoid huge intermediary outputs. First ask whether fixed SQL, jq, CLI pipelines or purpose-built read-only scripts already solve the problem more reliably. Only add model-generated composition if task diversity demands it.

**Owned by:** [Derived Index Economics, Tool Steering & Structural Retrieval (Sep 25)](../Derived%20Index%20Economics%2C%20Tool%20Steering%20%26%20Structural%20Retrieval%20%E2%80%94%202026-09-25.md) and [AI Engineering Observability & Dashboard Boundary (Sep 18)](../AI%20Engineering%20Observability%20%26%20Dashboard%20Boundary%20%E2%80%94%202026-09-18.md). No new broker/index.

### 2. Batching doesn't replace failure accounting or bounded authority (REINFORCE)

An execution block may read, mutate and send several things before returning. Partial success, retry, timeout and approval semantics become more important—not less. The host/server must be able to say which effects occurred, what is uncertain, and what can safely repeat. Treat permissions/hooks as enforced checks per effect; test a generated nested call and one blocked path.

**Owned by:** [Budgeted Side Effects, Approval Gates & Receipts (Sep 25)](../Budgeted%20Side%20Effects%2C%20Approval%20Gates%20%26%20Receipts%20%E2%80%94%202026-09-25.md) and [Bounded Execution Envelopes (Sep 22)](../Bounded%20Execution%20Envelopes%20%E2%80%94%202026-09-22.md). No new universal policy.

### 3. Keep runtime hooks and observability above claimed efficiency (REINFORCE)

Disabling OTel or known working hooks to adopt an experimental runtime can erase evidence needed for debugging, replay or institutional controls. This is a **documented compatibility issue**, not a justification to rewrite the HE harness. For work environments, consult organizational settings and approved providers.

**Owned by:** [Harness Portability, Exit Cost & Inspectability (Sep 24)](../Harness%20Portability%2C%20Exit%20Cost%20%26%20Inspectability%20%E2%80%94%202026-09-24.md) and [AI Engineering Observability & Dashboard Boundary (Sep 18)](../AI%20Engineering%20Observability%20%26%20Dashboard%20Boundary%20%E2%80%94%202026-09-18.md).

### 4. Avoid broadening PMB, ACR or Dashboard scopes (PARK)

- **PMB:** no new context or memory mechanism; code mode does not supersede provenance or source ownership.
- **ACR:** current bounded local-model reviewer does not need Claude Code REPL, agent coordination or provider gateway. Preserve deterministic input/output and tests.
- **Separate Dashboard:** read-only provider-native collection remains its boundary; do not add MCP Executor, 9Router, telemetry relaying or remote mutations in response to this video.
- **HE:** source research only. Not a request to install Executor or enable experimental Code Mode globally.

## Proposed lightweight A/B experiment — conditional, not authorized or scheduled

**Precondition:** observe a costly, repetitive pattern in an actual personal Claude Code session where multiple tool outputs swamp context or cause substantial repeated model turns. No test is necessary without that observation.

1. Capture versions, model, effort, prompt, task fixtures, tools/MCPs, permissions, hooks and OTel state. If Code Mode requires disabling critical controls, **stop; do not weaken them for a benchmark**.
2. Select a disposable, non-sensitive repository and one **read-only** task with meaningful multi-file or multi-tool filtering; if supported, a separate verified edit/test task.
3. Run standard vs Code Mode in **fresh sessions** multiple times with same fixtures and comparable cache/warmup state. Publish full successful and failed runs.
4. Compare task correctness, output artifacts, input/cached/output tokens, wall clock, approval count, tool failures, raw calls, context size, dollar costs as actually billed and provenance of any edits. Confirm that selected-output projection still leaves enough accessible audit evidence (`Ctrl+O`/transcript, task receipts, tests); distinguish user-visible traces from machine-exportable telemetry.
5. Run a blocked-action negative control in a disposable test environment, including nested/reformatted commands and a hook timeout, without authorizing real external side effects.
6. Evaluate whether the gain remains after risk/maintenance cost. Then choose enable-per-session, leave off, or defer; all reversible.

**Reject as optimization evidence:** same-task before/after in a live changing repository, one lucky run, changed model or hooks, schema-footprint percentage applied to cost, hidden denied actions, or an agent-authored success summary without independently checked output.

**Decision:** Preserve architectural pattern and concrete compatibility warnings. **No repo changes beyond this note.** Do not enable Code Mode in personal/work default settings or add a new service without an observed, reproducible need.

## Adjacent question: Muse and video/transcription

Meta's [Model API media documentation](https://dev.meta.ai/docs/media-understanding) supports video/audio understanding and transcription when appropriate media is supplied. This is **not proof** the consumer Muse UI can retrieve complete audio/transcripts solely by a YouTube page URL. [TubeText for Muse](https://tubetext.app/muse) advertises timestamped transcript retrieval via a third-party MCP; its data handling, accuracy and availability were not independently audited. For HE: prefer supplied transcripts, official captions or source video; verify timestamps, omissions and quotations before research promotion. No connector adoption authorized.
