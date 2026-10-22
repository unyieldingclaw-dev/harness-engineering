# Cole Medin — Model Mixing, Archon & Software Factory — 2026-09-24

## Purpose

Mine the durable Harness Engineering mechanisms from:

- Cole Medin video: `AI Coding Rate Limits are RIDICULOUS Now - Here's How You Keep Scaling Anyway`
- YouTube: `https://www.youtube.com/watch?v=NZq88JAJSag`
- user-provided transcript/screenshots reviewed 2026-09-24;
- `coleam00/ai-software-factory` inspected at current main `e167ddc480d2f5c95d436f9d1c5cdcf37b092281`;
- `coleam00/Archon` inspected at current dev `7b3eb317d2246c306264db7915c48840d3a8070f`;
- `coleam00/dark-factory-experiment` inspected at current main `eaf804b4cad7606496d54298b9c42d47731119d0`;
- `coleam00/kimi-k3-reliability-benchmark` inspected at current main `d0376c58df5a30b97d2fe274034460477be8d714`;
- selected adjacent Cole Medin repositories: `skills`, `harness-engineering-demo`, `adversarial-dev`, `claude-memory-compiler`, `Linear-Coding-Agent-Harness`, `ai-native-starter-pack`;
- Pi Agent Harness (`earendil-works/pi`) as the provider-neutral coding harness used in the demonstrated open-model path.

This is source research. It does not authorize adoption of Archon, Pi, Neon AI Gateway, a software-factory control plane, dynamic model routing, or changes to PMB/ACR.

Related HE research:

- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Risk-Directed Review & Release Proof — 2026-09-24.md`
- `01 Research/Supervised Agent Orchestration & Effect Verification — 2026-09-21.md`

---

# 1. Video claim: allocate expensive reasoning where the workflow needs judgment

The video presents a repeated workflow shape:

```text
issue
  ↓
stronger model: plan
  ↓
cheaper/faster model: implement
  ↓
stronger model: independent review
  ↓ findings
cheaper/faster model: fix
  ↓
deterministic test/build checks
  ↓
merge
```

Cole reports that implementation is commonly the most token-heavy stage and that his experiments often preserved acceptable output when a cheaper model implemented a plan produced and reviewed by a stronger model. He demonstrates one preferred combination using GPT-6 Astra for planning/review and GLM 5.3 Flash for implementation.

He also gives an important caveat: unfamiliar, difficult, or poorly understood work can justify a stronger model throughout the workflow.

### HE interpretation

The durable idea is **not**:

> always use a frontier model for planning/review and a small model for coding.

A safer formulation is:

> **Allocate model capability by workload role, uncertainty, risk, and measured task performance rather than using the most expensive model for every stage.**

The role allocation is a hypothesis to evaluate, not a static taxonomy.

**Disposition: STRONGLY REINFORCE evaluated model/effort configuration.**

---

# 2. Planning may be leverage-heavy, but it is not universally the most important stage

Cole argues that planning is the most important step because a sufficiently explicit plan can let a less-capable implementer execute successfully.

That is credible for bounded implementation work, but should not become HE doctrine.

Counterexamples include tasks where:

- implementation itself requires discovery;
- repository state contradicts the plan;
- hidden invariants emerge only during execution;
- debugging depends on runtime evidence;
- security/correctness depends on subtle implementation choices;
- the requested work is underspecified enough that builder judgment remains load-bearing.

### HE interpretation

A strong plan can **move reasoning cost earlier** and reduce implementation ambiguity. It does not eliminate the need for implementation judgment.

Useful question:

> How much unresolved judgment remains after the accepted planning artifact?

That is a better routing input than the stage name `implement` by itself.

**Disposition: REINFORCE artifact quality; REJECT universal stage-based capability assumptions.**

---

# 3. Artifact handoff is the important multi-model seam

The video describes both manual Markdown handoff and Archon-managed artifact passing between models/providers.

The useful mechanism is that model substitution occurs at an explicit work boundary with a durable artifact:

```text
accepted plan
   → builder

current diff + accepted requirements + evidence
   → independent reviewer

review findings
   → fixer
```

This is preferable to relying on one model to inherit another model's full conversational transcript.

### HE implication

> **Cross-model work should transfer through explicit, inspectable task artifacts rather than assuming conversational state survives provider/model changes.**

This strongly reinforces HE's existing model-transition-as-state-transfer and focused-continuation research.

**Disposition: STRONGLY REINFORCE.**

---

# 4. Archon is useful prior art for explicit workflow semantics

Archon is substantially more than a model router. Its current workflow system supports:

- DAG execution;
- deterministic shell/script nodes alongside AI nodes;
- per-workflow and per-node provider/model/effort selection;
- fresh/shared/resumed context modes;
- typed/validated output contracts;
- artifact passing;
- worktree isolation;
- conditional branching and join semantics;
- retries;
- loop/loop-group primitives with explicit termination controls;
- approval nodes;
- durable waits;
- cancellation;
- child governed runs;
- tool allow/deny controls where the provider supports them;
- explicit output artifacts and run state.

### HE interpretation

Useful implementation evidence:

1. model/provider choice can remain node-local rather than becoming a global router;
2. deterministic and probabilistic nodes can coexist in one workflow without pretending every step needs an LLM;
3. fresh context can be selected deliberately at verification boundaries;
4. output schemas/artifacts can make stage transitions machine-observable;
5. retries/loops/approvals are lifecycle semantics, not prompt prose.

### Disposition

**MINE as prior art. Do not adopt Archon as HE architecture by default.**

HE already has many of these concepts in lighter forms, and adopting a workflow engine would add operational ownership that current evidence does not require.

---

# 5. The AI Software Factory is stronger on trust boundaries than the video headline suggests

The current `ai-software-factory` implementation is notably conservative in several places.

It separates project-owned policy/context from shared workflow machinery and states that project guidance does not grant permission to publish, comment, merge, deploy, or schedule work.

Its current workflow policy requires runtime/holdout evidence to:

- exercise the delivered candidate;
- match current source identity;
- use fresh state for each attempt;
- preserve typed evidence;
- remain independent of the implementer for review acceptance.

Stale, ambiguous, or self-attested evidence is not considered sufficient.

The factory's holdout example also explicitly warns that a folder named `holdout` does not itself create isolation; the producer must establish real runtime isolation.

### HE implications

> **A label or directory name does not establish an authority/isolation property. The runtime must actually enforce the property being claimed.**

> **Evidence should bind to the exact candidate/version it is asserting facts about.**

These are strong corroborations of existing HE authority/effect-verification principles.

**Disposition: STRONGLY REINFORCE.**

---

# 6. The dark-factory experiment contains more useful HE material than the model-combination demo

The `dark-factory-experiment` is valuable because it makes several controls explicit:

- an immutable/human-owned governance perimeter that the autonomous factory cannot modify;
- a deterministic/stateless scheduler rather than an LLM deciding what should run;
- GitHub state/labels as durable lifecycle truth;
- bounded repair/fix attempts with escalation;
- per-node budget ceilings;
- independent holdout/evaluation outside the builder's optimization loop;
- worktree/isolation boundaries;
- deliberate human authority for judgment values and protected floors;
- regression evidence and mutation checks;
- scheduling only after manual observation of the workflow.

Its protected quality-floor mechanism is particularly interesting: a floor is raised only after the value has actually been observed, and an autonomous worker cannot lower the floor to make its own result pass.

### Durable HE mechanism

> **A verifier is only independent to the extent that the producer cannot rewrite the verifier's acceptance boundary.**

This is stronger than simply using a separate reviewer agent.

### Disposition

**HIGH-VALUE MINE.** Preserve the authority/perimeter mechanism; do not copy the factory product.

---

# 7. Cole's own benchmark material weakens any simplistic reading of the video

The mixed-provider benchmark in `dark-factory-experiment` deliberately varies planning and implementation models while holding other stages more constant.

More importantly, its playbook documents a known prompt-parity confound: the premium baseline cell has materially richer prompts than most comparison cells. The repository explicitly warns against quoting affected cross-cell results as clean measurements until prompt parity is restored.

That methodological honesty matters more to HE than the video's visual comparison of three builds.

### HE implication

A model-routing experiment must control more than model name:

```text
same task / starting state
same harness
same tools
same artifact contract
same prompt semantics
same verification
same retry policy
verified served-model identity
multiple runs
```

If those differ, the experiment is measuring the whole changed harness, not only the model.

**Disposition: STRONGLY REINFORCE behavioral-eval rigor.**

---

# 8. The Kimi K3 benchmark adds a reliability dimension public leaderboards do not answer

`kimi-k3-reliability-benchmark` uses:

- real/trap-laden tasks rather than only clean benchmark prompts;
- the same harness for compared models;
- multiple runs and `pass^k`-style reliability rather than one successful run;
- transcript/diff inspection;
- explicit served-model identity verification;
- hidden-invariant / false-premise / destructive-authority tasks that distinguish models which tie on easy work.

The repository's own finding is that easy well-specified work often fails to discriminate strong models, while reliability under ambiguity/invariants separates them.

### HE implication

> **Routing should optimize for required reliability under the actual failure modes of a workload, not leaderboard capability alone.**

A cheaper model that matches a frontier model on easy implementation tasks may still be a poor substitute on high-consequence hidden-invariant work.

**Disposition: STRONGLY REINFORCE workload-specific behavioral evals.**

---

# 9. Pi is a provider-neutral execution option, not a security boundary

Pi exposes a multi-provider LLM API and agent runtime, which makes it useful in Cole's open-model workflow.

Its own documentation explicitly states that Pi does **not** provide a built-in permission system for filesystem, process, network, or credential access; by default it runs with the permissions of the launching process. Stronger boundaries require containerization/sandboxing.

### HE implication

> **Model/provider portability and execution isolation are separate properties.**

Do not infer safety from a provider-neutral harness.

**Disposition: REINFORCE; PARK adoption.**

---

# 10. Selected Cole Medin repository scan

The following adjacent repositories were inspected for incremental value rather than assumed useful because they share an author.

| Repository | Useful mechanism | HE disposition |
|---|---|---|
| `ai-software-factory` | evidence-bound lifecycle, source identity, holdouts, publication/merge authority separation | **MINE** |
| `Archon` | explicit workflow semantics, node-local model/provider selection, artifacts, bounded loops, approvals | **MINE / PARK adoption** |
| `dark-factory-experiment` | immutable governance perimeter, deterministic scheduler, independent judge, budgets/ratchets | **HIGH-VALUE MINE** |
| `kimi-k3-reliability-benchmark` | same-harness model evaluation, repeated-run reliability, trap tasks, identity verification | **HIGH-VALUE MINE** |
| `skills` | progressive skill loading, rule drift checks, AI-layer ablation, system-evolution review | **SELECTIVE MINE** |
| `harness-engineering-demo` | minimal hooks + fresh review + bounded fresh-session loop | **CORROBORATION** |
| `adversarial-dev` | independent planner/builder/evaluator contexts, predeclared completion contracts, bounded retry | **SELECTIVE MINE**; reject opaque model score as sole authority |
| `claude-memory-compiler` | capture → compile → index lifecycle; offline memory maintenance | **ASSESS against PMB after pilot**, no migration assumption |
| `Linear-Coding-Agent-Harness` | external tracker can own task lifecycle/session handoff | **CORROBORATE one-source ownership**; do not copy forced issue decomposition |
| `ai-native-starter-pack` | derive project-specific context from real code; hooks as deterministic layer | **MOSTLY OVERLAP** |
| `context-engineering-intro` | context-rich planning/validation pattern | **LOW MARGINAL VALUE** relative to current HE research |

---

# 11. The `skills` repo contains one particularly useful eval idea: ablation

Cole's current skills collection includes an `ablate-ai-layer` concept: remove rules/context, run the same task again, and compare outcomes.

That maps directly to HE's behavioral-eval question:

> Does this rule/skill/context actually earn its permanent context and governance cost?

### HE implication

Behavioral harness evaluation should include **ablation** when practical:

```text
baseline harness
vs
harness without candidate rule/skill/context
```

Measure whether the component changes outcomes that matter before treating it as permanent infrastructure.

**Disposition: ASSESS as a lightweight behavioral-eval technique.**

---

# 12. What not to infer from the video

Do not promote these claims to HE doctrine from this evidence:

- `planning is always the most important stage`;
- `small/medium models should never plan/review`;
- `large models should never implement well-specified work`;
- `GLM 5.3 Flash is the correct default builder`;
- `Astra is the correct planner/reviewer`;
- `4x cheaper with equal quality` as a general cross-project result;
- public leaderboard rank as sufficient model-selection evidence;
- rate-limit pressure as justification for automatic opaque routing;
- Archon/Neon/Pi as required HE dependencies.

The demonstrated model names and economics will age quickly. The durable part is the evaluation method and role/capability boundary.

---

# Overall disposition

## STRONGLY REINFORCE

- Treat model/provider/effort as evaluated harness configuration.
- Use explicit artifacts at model/session boundaries.
- Keep deterministic checks outside expensive semantic reasoning.
- Bind evidence to exact source/candidate identity.
- Bound retry/fix loops and resource use.
- Keep verifier authority outside the producer when the producer could otherwise rewrite its own judge.
- Evaluate reliability on real workload failure modes, not leaderboard score alone.

## ASSESS

- A local role-based model matrix using real tasks and existing behavioral-eval discipline.
- Instruction/context ablation as a way to measure whether harness components earn their cost.
- Whether a cheap-builder / strong-planner-reviewer policy works for specific project workload classes.
- Whether Pi or another provider-neutral runtime offers useful portability without duplicating existing clients/gateways.

## PARK

- Archon adoption as HE's workflow engine.
- Neon or another gateway as an HE requirement.
- Dynamic/AI-selected model routing.
- Full software-factory automation for PMB/HE.
- Automatic scheduling before a supervised/manual workflow has been proven.

## REJECT

- Model selection by public leaderboard alone.
- Unbounded reviewer/fixer loops.
- Treating holdout naming as actual isolation.
- Allowing the producer to tune/disable its own independent acceptance boundary.
- Hard-coding current model names into HE architecture.

---

# Bottom line

The source set strengthens a specific HE direction:

> **Use expensive reasoning where the task still contains consequential judgment; use cheaper execution where the accepted artifact makes the work genuinely bounded; preserve model changes as explicit artifact handoffs; and prove the allocation with repeated real-task evaluations before turning it into routing policy.**

The most valuable material is not the current Astra/GLM combination. It is the surrounding discipline: explicit role boundaries, controlled experiments, source-bound evidence, independent verification, bounded loops, and a governance perimeter the autonomous worker cannot rewrite.