# Claude Code Hooks — Exit Code 2 Blocking Semantics and Completion Evidence — 2026-11-18

**Disposition:** First-party verified **research addendum** to the Lydia Hallie / Nate Herk evidence in draft PR #37. Not an instruction to retrofit hooks, alter ACR error handling, change PMB release checks, edit local Claude settings, or introduce a new governance framework.

## Question / source chain

Follow-up question: **“What about errors passing 2 back?”** This arose from screenshots and a Nate Herk talk claiming that a test hook that exits 1 is “marked as completed,” whereas exit 2 “sends Claude back to fix it.” Source boundaries:

1. **Primary current runtime contract (consulted November 18, 2026):** [Anthropic Claude Code Hooks reference](https://code.claude.com/docs/en/hooks), especially **Exit code output**, **Exit code 2**, **Other exit codes**, **Block the action when a hook fails**, **Exit code 2 behavior per event**, and **Stop / TaskCompleted** event controls.
2. **Primary implementation guidance:** [Anthropic Automate actions with hooks](https://code.claude.com/docs/en/hooks-guide), especially **Stop hook hits the block cap**, **Hooks and permission modes**, **Failed hooks on Windows**, and **Check what a hook did**.
3. **Secondary instructional claim:** Nate Herk, [“Anthropic Just Released the New Rules of Claude Code”](https://www.youtube.com/watch?v=8Pg6G5h05hk), approximately [06:36–07:22](https://www.youtube.com/watch?v=8Pg6G5h05hk&t=396s); user-uploaded eight-page *Anthropic Engineer Explains: The New Way to Use Claude Code* PDF, **pages 5–6**, showing a `Stop` hook shell example mapping a test failure to exit 2.
4. **HE ownership:** [Lydia Hallie companion evidence note](./Lydia%20Hallie%20%E2%80%94%20Rethinking%20Claude%20Code%20Practices%2C%20Evidence%20%26%20HE%20Mapping%20%E2%80%94%202026-11-18.md), existing `01 Research/Bounded Execution Envelopes — 2026-09-22.md` and `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`.

**Verified conclusion:** `exit 2` from a **synchronous Claude Code command hook** has a **hook-event-specific** effect. It is not a special status for all test scripts, ACR, CI, or arbitrary subprocesses. It means “block” only for lifecycle events that support blocking. The original secondhand video's “exit 1 = marked done anyway” is too broad; exit 1 is generally treated as a non-blocking hook **error**, not an affirmative success declaration. It does not prove Claude's task has passed or that tests were run.

## 1. Exact event effects

| Hook event | Exit 2 as Anthropic documents it | Interpretation |
| --- | --- | --- |
| `PreToolUse` | **Blocks** the imminent tool invocation. | Pre-action enforcement, but only for matched tool calls. |
| `UserPromptSubmit` | **Blocks** delivery of the prompt. | Prompt entry gate; not general task verification. |
| `Stop` | **Prevents Claude from stopping**; turn continues with feedback. | End-of-turn completion gate. Not a guarantee of eventual success or an infinite loop. |
| `TaskCompleted` | **Prevents task from being marked completed**. | Task-state gate, triggered when the specific event actually fires. |
| `SubagentStop` | **Prevents subagent from stopping**. | Narrow subagent lifecycle gate; do not assume it applies to all concurrent sessions or other provider agents. |
| `PostToolUse` / `PostToolUseFailure` | **Does not block past action**; stderr is delivered as feedback to Claude. | Cannot undo an edit, external call, or failed command. |
| `PermissionRequest` | **Exit 2 is ignored for permission flow**; use event's structured decision JSON. | Counterexample to “all permission hooks block on 2.” |
| `Notification` / `StopFailure` / various post-event hooks | Generally ignored / no blocking effect as specified in event table. | Don't assume any callback is a gate. |
| `WorktreeCreate` | **Any nonzero** exit causes creation to fail. | Exception: not specific to code 2. |
| `WorktreeRemove` | Any nonzero can fail removal if directory remains. | State-dependent exception. |

[Official table: exit code 2 behavior per event](https://code.claude.com/docs/en/hooks#exit-code-2-behavior-per-event). There are further event-specific exceptions; check the live reference before implementing.

## 2. The three distinct statuses that MUST NOT be conflated

**(A) Underlying test/program exit code.** `npm test`, `pytest`, a shell script, a review CLI or a compiler commonly returns `0` for success and a nonzero number for failure. That status is meaningful to its parent shell/script/CI. Changing a test runner to return `2` is **not required**, and may break existing integrations.

**(B) The hook handler's own exit code.** A hook that actually executes the test may intentionally translate the test's *failure* into its **own** exit code `2`, with actionable stderr. This only matters when the hook is configured on a relevant blockable lifecycle event and runs to completion. In a shell pipeline, ensure the wrapper propagates the decision; a later successful command can otherwise overwrite the failure exit status.

**(C) The parent caller's response to a tool/CLI exit code.** An ACR invocation that exits `2` for an incomplete/failed review is **not** a Claude Code `Stop` hook merely because the same integer appears. A caller must actually inspect and enforce ACR's result; alternatively CI may be the authoritative gate. Do not install a new hook to duplicate a correctly enforced CI or review-contract outcome. In November 16 PMB /change-review investigations, ACR correctly classified timeouts and returned exit code 2; this does **not** establish that a Claude completion hook is missing.

**Architectural framing:** independent *task verification* is the requirement; **exit code 2 is just one carrier across one boundary**. The verified outcome and evidence artifact remain with their existing owner, not with the numeric status.

## 3. Failure modes invisible in the simplistic tutorial

1. **Fail-open hook crashes and timeouts.** By default, exits other than 0 or 2, invalid output, failure to launch and timeouts are often **non-blocking** and allow a pending action to continue. Anthropic documents `onFailure: "block"` for supported `command`/`http` hooks in Claude Code **v2.1.295+**, but it does **not apply** to `Stop`, `SubagentStop`, `TaskCompleted`, `TeammateIdle`, or asynchronous hooks. Don't claim a universal fail-closed capability. Read `onFailure` documentation for the exact event.
2. **Special missing-file stderr handling.** Per current reference, for `Stop`, `SubagentStop`, `TaskCompleted` and plugin `UserPromptSubmit`, `exit 2` with no stdout and stderr saying a file is missing (e.g. “No such file or directory”) can be treated as a **non-blocking error**. A script-not-found failure is not evidence the gate protected the task.
3. **Structured output interference.** `exit 0` plus one valid JSON object on stdout can express detailed decisions. Mixing JSON and `exit 2` is discouraged; notably `continue: false` takes precedence for some `Stop`/`SubagentStop` cases and ends the turn instead of continuing. Prefer *either* exit-code-with-stderr *or* exit-0-valid-JSON, not a mixture.
4. **Hook loop limit.** Per [Anthropic troubleshooting guide](https://code.claude.com/docs/en/hooks-guide#stop-hook-hits-the-block-cap), Claude Code can override a repeatedly blocking `Stop` hook after **eight consecutive blocks with no Claude tool call between**; `stop_hook_active` is available in input for recursion handling. “Exit 2 and it cannot finish until fixed” is therefore false as an unconditional guarantee.
5. **Test script cost and relevance.** Blanket `Stop: npm test` may repeatedly execute a slow/flaky full suite, fail on unrelated changes, or exceed budgets. Merely passing existing tests may not detect auth/session invalidation, migration/rollback issues or unwritten acceptance criteria. Only install a gate when there is an actual gap and the check is deterministic, relevant, fast enough, and owned by the correct pipeline.
6. **Windows semantics.** User's local primary environment is Windows PowerShell. A Bash snippet copied from PDF is not automatically portable: Git Bash presence, explicit `shell: "powershell"`, executable paths, JSON stdin parsing, and `onFailure` matter. Validate shell-exit propagation and first-run behavior in a disposable test location. Refer to official [Windows-specific hook guidance](https://code.claude.com/docs/en/hooks#failed-hooks-on-windows-without-git-bash).
7. **Sandbox and authority.** Hook processes can access local environment, files, or network according to how they are launched; a test invoked *by* a hook is not automatically a read-only or sandboxed test. An enforcement hook is not a substitute for permission configuration, OS containment, network boundaries or user approval.
8. **Hook doesn't fire just because the model says “done.”** It must be installed at the correct scope and lifecycle event, be recognized by the runtime and permitted by managed settings; inspect `/hooks`, behavior and actual debug output. A session/task/agent event can differ from a CI pipeline or separate provider run.

## 4. Illustrative wrapper — research, not a deployment instruction

The following shows **only** the decision translation at a `Stop` event, not validated production configuration:

```bash
# Illustrative only: run the project's independently chosen, bounded verifier.
if npm test; then
  exit 0
fi
echo "Required verification failed. Review the failing test output and correct the cause." >&2
exit 2
```

**Requires actual integration decisions:** which test, path, runtime/shell, hook configuration, input schema, maximum execution duration, fail behavior, repeat counter, whether `Stop` or `TaskCompleted` is appropriate, and how to avoid asking Claude to repair an unrelated flaky test. The example doesn't capture full stdout or prove that a test is relevant to acceptance. The hook may execute with effects and credentials of its runtime.

For independent CI jobs, **keep conventional test failure statuses**; do not indiscriminately translate everything to `2`.

## 5. Tiny acceptance matrix if a real missing gate is demonstrated

A future *explicitly authorized* project-local experiment could verify a hook in a scratch repo, without modifying PMB or ACR:

| Case | Expected evidence |
| --- | --- |
| Check passes | Hook exits 0 and task can finish; proof in test output |
| Deterministic check fails | Hook exits 2; Claude sees useful stderr and cannot complete that event without continuing |
| Executable missing / permission denied | Observed fail-open or blocked behavior according to **actual hook event**, shell and settings; no false claim that guard worked |
| Unrelated or flaky failure | No indefinite retry; surface to operator rather than masking it |
| Hook already triggered continuation | No looping beyond intended cap; confirm runtime's observed stop behavior |
| Other tool/action | No unrelated tool blocking or compatibility regression |
| Protected action already occurred | A `PostToolUse` hook cannot reverse it; demonstrate proper pre-action boundary separately |

Track only minimal proof (event, version, file, test, stderr and outcome). Avoid new databases, generalized schedulers or automatic policy promotion.

## 6. HE disposition and specific project implications

- **HE:** Add this focused source clarification (this file), with event-by-event semantics and constraints. The existing research already treats **deterministic checks** as stronger than agent assertions and **bounded authority** as necessary for side-effectful actions. This note does **not** justify a separate “hook governance” layer.
- **PMB:** Do **not** alter shipped scripts or release config to return 2 just because a creator video suggests it. Review whether an actual failing required test has ever been followed by incorrect task completion, and whether current CI or hooks already block it. PMB release work remains isolated.
- **ACR:** Its program status `2` for recognized incomplete review is not a `Stop` hook. ACR's normal test/CLI exit codes should remain truthful. A real integration must be examined at the **caller/adapter** boundary, and only if a false-success condition is demonstrated.
- **Dashboard:** No role in enforcing or rerouting hook decisions. Observability can display already-owned results, if an actual requirement arises.
- **Home and Work:** Do not copy shell or hook settings to user-global config; corporate managed settings, permissions and tool restrictions may override hooks.

**Final decision:** **DOCUMENT / NO IMPLEMENTATION.** No evidence of a currently missing completion enforcement gate is supplied. Revisit only against a reproducible case, in the owning project and with the smallest change.
