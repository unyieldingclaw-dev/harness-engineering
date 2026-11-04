# Intelligent Internet — Zenith Long-Horizon Harness — 2026-11-04

## Source, question and boundary

- Primary: https://github.com/Intelligent-Internet/zenith
- Technical report and source code: `technical_report/` and `zenith/` in that repository.
- Secondary discovery: Tom Bilyeu / Emad Mostaque interview, ~25:30–27:00 — https://www.youtube.com/watch?v=WsdcF7EEvhM
- Date reviewed: 2026-11-04.
- Question: **Can adaptive rechecking, independent verification and explicit stopping improve real long-horizon task completion enough to justify their orchestration cost?**

This is source-level research. It does **not** recommend installing Zenith, changing the HE architecture or treating author benchmarks as independent validation.

## Observed published implementation

Zenith is described as a multi-agent mission orchestrator for Claude Code, Codex and Hermes, connected via MCP/ACP. Its orchestrator reads task state and can allocate workers, testers, reusable skills, replanning or stopping decisions. Its declared target is long-running work that stops prematurely.

The published concepts are:

1. **Repeated gap-finding:** compare current artifacts with original requirements, not merely the last assistant reply.
2. **Revisable planning:** adjust plan when evidence shows the current path is wrong.
3. **Independent verification:** a test/review stage distinct from the producing worker.
4. **Adaptive orchestration:** select additional worker/tester effort only when useful.
5. **Stopping discipline:** cease when acceptance is evidenced or further work is not justified.

These are described methods, not separately verified outcome improvements for this user's projects.

## Reported evidence and its limits

The authors' eight-task ablation reports:

| Variant | Mean rank (lower is better) | Mean task cost | First-place wins |
| --- | ---: | ---: | ---: |
| One-session | 5.00 | $22.21 | 0 |
| Plan-RALPH | 4.00 | $161.53 | 0 |
| Milestone-RALPH | 2.88 | $209.47 | 0 |
| RALPH | 1.75 | $407.58 | 3 |
| Zenith | 1.38 | $175.68 | 5 |

Compared with RALPH in **this** test, Zenith reported about $232 lower mean spend per task. RALPH cost ~2.32× as much as Zenith, or equivalently Zenith was ~57% cheaper. Do not repeat the interview's **50× lower cost** as if this ablation validates it. Task choice, variance, model/runtime, evaluator independence, retries and differing orchestration complexity still constrain generalization.

The repository also cites a Frontier SWE leaderboard. Treat that as a separate comparison with potentially different model/harness pairs; it does not isolate the harness effect unless model and other execution conditions are controlled.

**Main limitation:** eight author-selected long-horizon tasks are a promising exploration, not a sufficiently broad independent result to generalize to HE, PMB or ACR.

## Mine the mechanism, not the framework

Potentially transferable with minimal new machinery:

- Reopen **original acceptance criteria** after each major work boundary.
- Use an independent check that can fail the producer's "done" declaration.
- Persist a compact task checkpoint: unmet requirement, evidence, owner, next action, and stop/escalation reason.
- Limit retries and parallel work by observed remaining gap, not a ritual fixed count.
- Require changed evidence/hypothesis before retrying the same failed step.
- Keep a deterministic/human acceptance gate for consequential actions.

These overlap established HE work in:

- `01 Research/Adaptive Autonomy, Diagnostic Gates & Fan-Out-Fan-In — 2026-09-25.md`
- `01 Research/Behavioral Invariants, Declarative Goals & Bounded Autonomous Loops — 2026-09-25.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Sources/Turing Post — Harness Minimalism, Long-Horizon State & Control Planes — 2026-10-19.md`

No evidence yet supports a new always-on multi-agent layer, worker registry, meta-controller, autonomous skills evolution, generalized agent-state database or mandatory full mission loop.

## Bounded evaluation question (only if a real failure warrants it)

1. Pick **one reproducible failure of premature completion** in an existing project. If none exists, do not build this test.
2. Preserve exact starting repo/fixtures, definition of done, model/runtime/effort, tooling and cost accounting.
3. Compare three conditions: minimal single-session continuation; small bounded gap-check/retry; Zenith or a faithful instrumented Zenith-like workflow.
4. Verify with an **external** acceptance check and known negative(s); record rate of truly complete outcomes, missed requirements, regressions, tool actions, elapsed time and cost per verified success.
5. Promote only the **smallest successful mechanism**, owned by the actual failing layer. If a one-page checklist closes the gap, do not install a scheduler.

## Disposition

- **REINFORCE:** acceptance separate from producer self-certification; current artifacts compared with original objective; budgeted retries; evidence-based stopping; durable task state rather than immortal context.
- **ASSESS:** whether independent gap checks materially improve a *specific* observed premature-completion failure.
- **PARK:** Zenith installation/integration, persistent orchestration service, automatic worker spawning and generalized skill evolution.
- **REJECT:** the interview's 50× number as proven by Zenith's ablation; multiple agents as an objective; treating author benchmarks as causally isolated HE evidence.

## Sources

- https://github.com/Intelligent-Internet/zenith
- https://github.com/Intelligent-Internet/zenith/tree/main/technical_report
- https://www.youtube.com/watch?v=WsdcF7EEvhM

**Research status only; HE-001/HE-002 and decision records are unchanged.**
