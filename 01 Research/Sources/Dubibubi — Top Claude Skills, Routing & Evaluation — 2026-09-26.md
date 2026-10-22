# Dubibubi — Top Claude Skills, Routing & Evaluation — 2026-09-26

## Purpose

Deep evidence pass on the video **Top 9 Most Powerful Claude Skills (No Bullsh*t List)** by Dubibubi, using the supplied transcript as the claim set and the current public GitHub implementations as the implementation evidence.

Video:
- https://www.youtube.com/watch?v=LCwT00LrPZg

This note does **not** treat the creator's ranking, install counts, claimed savings, or personal tests as independent benchmark evidence. The useful question for HE is which mechanisms survive inspection below the README and what ownership/routing/evaluation lessons they expose.

---

## Executive disposition

The video is more useful as a **taxonomy of skill interventions** than as a shopping list.

The strongest HE findings are not "install these nine skills." They are:

1. different skills intervene at fundamentally different layers and should not all be governed the same way;
2. progressive skill routing is better than loading a whole methodology all the time;
3. empirically answerable forks should often be tested rather than escalated to the human as preference questions;
4. orthogonal review objectives should remain separate through fan-in;
5. evidence of completion should be bound to the exact gate/check definition that produced it;
6. minimality is safer when deliberate shortcuts carry explicit ceilings and upgrade triggers;
7. output compression needs safety/clarity escape hatches;
8. a popular skill or an impressive anecdote is not evidence that it improves this repo's work;
9. skill behavior itself should be evaluated with representative tasks, not accepted on description quality.

---

# Source pins

## Caveman

Repository:
- https://github.com/JuliusBrussee/caveman/tree/2fd153c67988e980fb0b2455c90832159a6a5a25

Inspected:
- `skills/caveman/SKILL.md`

## Pstack / Poteto Mode

Current public implementation is shipped inside Cursor's plugin repository:
- https://github.com/cursor/plugins/tree/ecc249f1e306fc64ddf83c7bed16cacf7c2239db/pstack

Inspected:
- `pstack/skills/poteto-mode/SKILL.md`
- pstack skill inventory

## Vibe Security

Repository:
- https://github.com/raroque/vibe-security-skill/tree/850938f20f6915e7c3688d85c0a838f7909c87bb

Inspected:
- `vibe-security/SKILL.md`
- reference/agent structure

## No AI Slop

Repository:
- https://github.com/petergyang/no-ai-slop/tree/000650b156983f5159695b441477f4e63b25dc85

Inspected:
- `skills/no-ai-slop/SKILL.md`

## Ponytail

Repository:
- https://github.com/DietrichGebert/ponytail/tree/e3ba2aa6f1e6f0bc4d69eb09c9f0d0a93af56156

Inspected:
- `skills/ponytail/SKILL.md`
- `benchmarks/results/2026-06-12-v4-hardening-vs-caveman.md`
- cross-host/plugin surfaces in the repository tree

## Matt Pocock Skills

Repository:
- https://github.com/mattpocock/skills/tree/c55ee46073ed923f86ce59a5eb3b6d895095d1b7

Inspected:
- `skills/engineering/code-review/SKILL.md`
- skill/router references found through code search
- repository context/changeset structure

## HyperFrames

Repository:
- https://github.com/heygen-com/hyperframes/tree/984ad28c7cbaae37db26e0c8eeea9b92e7b457ac

Inspected:
- skill documentation/router references
- `AGENTS.md` / `CLAUDE.md` skill guidance
- core-skill installation/update references
- skill-manifest tests

## Taste

Repository:
- https://github.com/Leonxlnx/taste-skill/tree/ce26fc25c0e5e8cab638f883de62d9a86ee5e45b

Inspected:
- current repository metadata and README skill-version structure

## Impeccable

Repository:
- https://github.com/pbakaus/impeccable/tree/9d715cc4f5564a990ca8345abfdd5df6dc9b41c8

Inspected:
- `.claude/skills/impeccable/SKILL.md`
- cross-host skill copies/surfaces

## Unlazy — honorable mention, but high HE signal

Repository:
- https://github.com/Leonxlnx/unlazy/tree/16671491f6679ad9378f52604d3bc2415b4120c7

Inspected:
- `SKILL.md`

## Autoresearch

Already deeply mined in HE from Karpathy's actual repository:
- https://github.com/karpathy/autoresearch/tree/228791fb499afffb54b46200aca536f79142f117

Disposition here: **REINFORCE**, do not duplicate prior source mining.

---

# 1. Caveman — presentation compression is a different intervention class

The video presents Caveman mainly as a token-saving communication style. The current implementation is more careful than "talk shorter."

Mechanisms worth mining:

- explicit persistence/intensity state;
- removes filler and tool-call narration without touching code/error text;
- refuses made-up abbreviations that cost clarity without actually saving tokens;
- carries an `Auto-Clarity` escape hatch for security warnings, irreversible-action confirmations, ambiguous multi-step sequences, user confusion, and other cases where compression could change meaning;
- persisted artifacts such as docs, comments, commits, issues and third-party messages remain normal prose by default.

### HE interpretation

**MINE:** treat output/presentation compression as downstream policy, not reasoning compression.

A terse rendering layer may reduce cost/noise while leaving the underlying reasoning, evidence acquisition and durable artifacts intact.

### Caution

Do not infer that fewer visible output tokens means the model reasoned more efficiently. Do not compress safety-critical semantics merely to win a token metric.

Disposition: **ASSESS** as a presentation policy; do not promote as a global HE rule without local measurement.

---

# 2. Pstack / Poteto Mode — empirical forks, task-class routing and trigger sprawl

Poteto Mode is not one small Skill. It is a large routing/orchestration layer with specialist skills, playbooks, automation and subagent conventions.

High-value mechanisms:

### Empirical-decision gate

Before asking a human a "which approach?" question, Poteto Mode distinguishes between:

- an observable question that can be settled by running/prototyping/measuring; and
- a genuine product/preference/authority decision only the operator can make.

If the fork is empirical, its default is to build a cheap prototype or otherwise obtain evidence rather than outsourcing an answerable engineering question to the human.

**MINE.** This is a strong expression of Evidence Before Architecture.

### Task-class playbook routing

The skill routes investigations, bug fixes, performance work, prototypes, features, refactors, visual parity, skill authoring and other task classes into different playbooks.

This is better than one giant universal workflow **if** routing remains accurate and the loaded procedure is proportional to the task.

### Parallel work is specialized

Poteto Mode distinguishes design/code bakeoffs, coverage fan-out, adversarial review and other parallel shapes rather than treating "spawn agents" as one primitive.

### Lessons move into structure

One explicit principle is to encode repeated lessons as lint, metadata, runtime checks or scripts rather than continuing to append prose instructions.

This aligns strongly with HE's ownership discipline.

### Parent ownership of subagent work

The parent is instructed to review the actual diff/output and write its own summary instead of blindly forwarding a subagent's "done" message.

**MINE:** delegation does not transfer acceptance authority.

### Decision trail for long/unattended work

Long or autonomous work can create an auditable decision trail rather than relying on a final summary to reconstruct why the run made choices.

### Caution: orchestration surface can become the problem

Poteto Mode also demonstrates the opposite risk: a very large trigger/routing surface can create persistent cognitive/routing cost, hidden dependencies, overlapping playbooks and harder debugging.

HE should mine mechanisms, not adopt the whole "operating system." This is exactly the kind of system that should be justified by behavioral evals against a simpler baseline.

Disposition: **MINE** empirical-decision gate and delegation/ownership patterns; **ASSESS** task-class routing; **REJECT** wholesale adoption without local evidence.

---

# 3. Vibe Security — progressive disclosure by detected technology

The current skill keeps a relatively compact audit process and loads technology-specific references only when the codebase actually uses the corresponding technology/pattern.

Reference domains include:

- secrets/environment;
- database access controls;
- auth;
- rate limiting;
- payments;
- mobile;
- AI/LLM integration;
- deployment;
- data access/input validation.

The Skill explicitly says to skip irrelevant technology sections and to report genuine security findings rather than style concerns.

### HE interpretation

This is a clean example of:

```text
small router / audit skeleton
        ↓
detect task + technology
        ↓
load only relevant specialist knowledge
```

**MINE:** progressive disclosure should follow task topology and observed technology, not merely file hierarchy.

### Caution

A security Skill is model-mediated judgment, not security acceptance authority. Deterministic scanners, runtime tests, policy checks, independent review and human authority remain separate evidence sources.

Disposition: **MINE** reference routing pattern; **ASSESS** security-domain coverage for ACR only if ACR evidence shows a gap.

---

# 4. No AI Slop — detect and mutate are separate jobs

The current skill defines two explicit modes:

- **Detect:** name the pattern, quote the line, state the fix direction; do not rewrite or score.
- **Edit:** make the minimum effective change and return a short `What changed` section.

It also explicitly rejects guessing whether AI authored the text; it grounds findings in named observable patterns.

### HE interpretation

Two useful patterns:

1. **Diagnosis-only and mutation should be separate modes.**
2. **When transformation occurs, surface a concise change ledger so the operator can challenge individual edits.**

This is a small but useful analogue of diagnosis-before-mutation and receipt/provenance patterns elsewhere in HE.

Disposition: **MINE** mode separation / change transparency. Low value for broader HE architecture beyond that.

---

# 5. Ponytail — minimum sufficient mechanism, with explicit ceilings

Ponytail's implementation is more rigorous than the video's "write less code" framing.

Its ladder is:

1. does this need to exist;
2. reuse what is already in the codebase;
3. use standard library;
4. use native platform capability;
5. use an already-installed dependency;
6. one line if sufficient;
7. only then write the minimum custom code.

Crucially, it says the ladder runs **after understanding the problem**, not instead of understanding it.

Other high-value details:

- root-cause fix over per-symptom patch;
- no speculative abstractions/config/scaffolding;
- deliberate simplifications with known ceilings can carry an explicit ceiling and upgrade path;
- security, validation, data-loss protection, accessibility and explicit requirements cannot be optimized away;
- non-trivial logic leaves one minimal runnable check.

### HE interpretation

**MINE:**

> Prefer the minimum sufficient mechanism after understanding the real flow; when a deliberate shortcut has a known ceiling, record the ceiling and the evidence/condition that would justify upgrading it.

This is stronger than "always write less code."

### The benchmark is itself useful HE material

Ponytail ships a same-model benchmark against Caveman and a no-skill control using six build tasks, extension requests, independent security/concurrency probes and recorded token/time data.

Reported same-model totals in that repo's benchmark:

- control2 build LOC: 3,629;
- Caveman: 1,440;
- Ponytail v4: 490;
- agent tokens: 430,697 / 290,546 / 229,370;
- wall time: 2,749s / 1,596s / 821s;
- security/concurrency probes pass in all arms.

The repo itself notes important caveats, including n=1 cells and wall-time scheduling noise.

The value to HE is **not** the headline percentage. It is the evaluation shape:

```text
same model
+ same tasks
+ control/treatment
+ correctness/safety probes
+ extension cost
+ token/time measurements
```

That is much better evidence than "this Skill feels good."

Disposition: **MINE** minimum-sufficient/ceiling pattern and behavioral-eval shape. Treat repository benchmark as project-local evidence, not universal proof.

---

# 6. Matt Pocock Skills — preserve orthogonal review axes through fan-in

The current `code-review` Skill is one of the strongest findings in this pass.

It pins a fixed comparison point, identifies the originating spec and repository standards, then launches two parallel review axes:

- **Standards:** does the diff follow the repository's documented standards?
- **Spec:** does the diff implement what the originating issue/spec asked for?

The two reviewers run separately so their contexts do not contaminate one another. The final step deliberately **does not merge or rerank** their findings into one score because a change can pass one axis and fail the other.

### HE interpretation

This directly strengthens the fan-out/fan-in work already in HE:

> **Orthogonal acceptance objectives should remain distinguishable through fan-in. A synthesizer may present them together without collapsing them into one truth score.**

This is especially relevant to ACR. Security, correctness, architecture, policy, spec compliance and evidence quality should not automatically be blended into one opaque severity/rank if doing so hides what actually failed.

### Another strong repository lesson: docs as caches

The repository's current change history explicitly treats environment-owned facts such as package scripts, config, directory layout and `--help` output as source-owned truth. A document that restates those facts is a **cache** and should only earn persistence/load when the underlying lookup is expensive. The durable positive target is unwritten conventions, reasons, constraints and gotchas that the environment cannot reveal directly.

This reinforces Single Ownership and Progressive Disclosure.

Disposition: **MINE** orthogonal review preservation and source-vs-cache distinction.

---

# 7. HyperFrames — core router + on-demand workflow installation

HyperFrames currently ships many domain/workflow Skills, but its own documentation recommends a **core set**. The `/hyperframes` entry Skill serves as a capability map/router and installs creation workflows on demand; installing the entire bundle is an explicit opt-in rather than the default.

The repository also tests skill-manifest/core-skill behavior rather than treating Skills as unversioned text snippets.

### HE interpretation

This is a concrete example of progressive capability disclosure:

```text
small stable core
      ↓
capability map / router
      ↓
load or install task-specific workflow only when required
```

That architecture is more interesting to HE than the motion-graphics domain itself.

### Caution

Dynamic installation is a capability/security boundary. A router should not silently fetch/enable arbitrary code or instructions merely because a task resembles a trigger. Source, version, trust and authorization still matter.

Disposition: **MINE** core-router/on-demand leaf pattern; **PARK** domain-specific HyperFrames workflow content.

---

# 8. Taste — strong domain priors, limited general HE signal

Taste is an opinionated design-generation Skill. Its current repository preserves an older v1 behavior while actively iterating the newer design Skill, which is itself a useful acknowledgement that changing prompt/skill behavior is a versioned behavioral change rather than a transparent implementation detail.

### HE interpretation

The generalizable lesson is modest:

- domain-specific priors can be valuable when deliberately invoked;
- behavioral Skill versions can matter to reproducibility;
- a strong generator is not automatically a good evaluator.

Disposition: **PARK** most design content; **REINFORCE** behavioral-version provenance.

---

# 9. Impeccable — one capability entry point, many explicit subcommands

Impeccable's current Skill is a large design capability surface, but it presents one explicit entry point with named subcommands/reference files for planning, critique, audit, polish, hardening, adaptation, performance, etc.

Notable mechanisms:

- one entry skill routes to leaf references by explicit/implied command;
- setup loads product/design/surface context once per session;
- evaluation commands are distinct from mutation/refinement commands;
- verification is deliberately bounded to a small number of inspection/fix passes rather than an open-ended self-polish loop;
- a `doctor` path reports artifact/config drift, while normal design work does not silently repair unrelated drift as a side effect;
- cross-host copies/adapters expose the same underlying capability to multiple agent environments.

### HE interpretation

**MINE:**

- one coherent capability can expose a small router instead of dozens of equally visible top-level entry points;
- health/drift repair should be explicit, not hidden side effects;
- bounded self-QA can prevent infinite "one more polish pass" loops;
- producer/evaluator modes should remain distinguishable even inside one capability family.

Disposition: **MINE** routing, bounded verification and explicit doctor/drift semantics; **PARK** design-specific policy.

---

# 10. Unlazy — evidence must bind to the current gate definition

Although only an honorable mention in the video, Unlazy is one of the strongest HE sources in the batch.

Its completion model writes acceptance gates before substantial work. More importantly, it treats runnable checks as executable code and binds approval/evidence to the exact check definition and environment.

The current implementation binds approvals/evidence to details including:

- ledger/gate;
- command;
- expectation;
- working directory;
- shell;
- timeout/output constraints;
- platform;
- inherited `PATH`;
- current definition digest.

If the gate definition changes, old evidence no longer silently proves the new gate.

Other strong mechanisms:

- inherited gate text/output is untrusted data;
- checks do not execute merely because a ledger was loaded;
- impossible gates are explicitly abandoned with a reason and require handoff rather than disappearing;
- parent re-verification is distinct from leaf self-check;
- parallel leaves carry narrow ownership/dependencies;
- final completion report is reconciled against the original request immediately before claiming done;
- optional stop hook is a structural backstop and does not itself execute the checks.

### HE interpretation

Strong principle candidate:

> **Verification evidence is valid only for the acceptance definition and execution envelope it actually measured. Materially change the gate or environment and the evidence must be reacquired.**

This is directly relevant to ACR calibration/evidence provenance and Harness Miner experiments.

### Caution

Unlazy is heavy. The Depth Tree / ledger machinery would be unacceptable overhead for ordinary edits. Mine the gate-binding, explicit abandonment and final-claim audit mechanisms without adopting the whole workflow.

Disposition: **MINE STRONGLY** gate-bound evidence. **REJECT** universal use.

---

# 11. Autoresearch — already mined; video framing is too broad

The video correctly notices that repeated experiment → measure → keep/revert is broadly reusable. HE already mined Karpathy's actual implementation in depth.

HE retains the stronger version:

- fixed evaluator/runtime;
- narrow mutable surface;
- baseline first;
- objective metric;
- bounded experiment budget;
- isolated/reversible state;
- keep/revert loop;
- producer cannot redefine acceptance.

Do not generalize "run 100 experiments" into domains where the objective function is weak, confounded or unsafe. The power comes from a **good evaluator and bounded experiment envelope**, not the number 100.

Disposition: **REINFORCE** existing HE research.

---

# 12. Honorable mentions not deeply mined

The transcript also names AI Job Search, Agent Reach and Open Design but does not provide unambiguous repository URLs in the supplied material.

Several similarly named public repositories exist. Rather than guess at attribution, this pass does **not** claim a canonical implementation for those three.

Disposition: **PARK pending direct source URL** unless a future HE question makes them material.

---

# Cross-source findings

## A. "Skill" is too broad a category

The sources cover fundamentally different intervention classes:

| Class | Example | Primary intervention |
|---|---|---|
| Presentation policy | Caveman | response surface / output cost |
| Implementation heuristic | Ponytail | solution size/complexity |
| Domain reference router | Vibe Security | selective specialist knowledge |
| Workflow/router | Pstack, HyperFrames, Impeccable, Matt | task orchestration/procedure |
| Diagnostic/editor | No AI Slop | detect vs mutate |
| Evaluator/reviewer | Matt code-review, Impeccable critique/audit | independent objectives / quality |
| Completion/evidence | Unlazy | acceptance gates and proof |
| Experiment harness | Autoresearch | iterative optimization |

HE should evaluate each class against its actual owner/cost/failure mode rather than asking generically whether "Skills are good."

## B. Popularity is discovery evidence, not effectiveness evidence

Stars, installs and YouTube rankings can identify candidates to inspect. They do not prove:

- routing accuracy;
- reduced defects;
- lower token cost;
- better maintainability;
- portability;
- security;
- value in HE's actual workload.

Ponytail's same-model benchmark is notable precisely because it attempts behavioral comparison instead of relying only on popularity.

## C. Router quality matters as much as leaf quality

A great leaf Skill that rarely triggers when needed, triggers constantly when irrelevant, or loads too much context can still be a net-negative harness intervention.

Evaluate:

- discovery/trigger recall;
- false triggers;
- context/load cost;
- routing ambiguity;
- overlap/competition with other Skills;
- fallback behavior when no Skill fits.

## D. Skill drift is executable-behavior drift

These repositories increasingly version their skill behavior, routers, manifests and cross-host adapters. A Skill should not be treated as harmless prose when it materially changes the execution policy of an agent.

Pin source/version in experiments and durable evidence.

---

# Candidate implications

## ACR

Worth a separate ACR-native inspection later:

1. **Orthogonal review axes:** Matt Pocock's separate Standards vs Spec reviewers are a strong analogue for preserving ACR reviewer provenance/objectives through fan-in.
2. **Gate-bound evidence:** Unlazy's definition-digest model is relevant to ensuring calibration/evidence results are tied to the exact evaluator/configuration that produced them.
3. **Skill/evaluator A-B testing:** Ponytail's benchmark structure provides another concrete model for same-model control/treatment testing, but ACR should use its own representative fixtures/real-PR evidence rather than copying those tasks.

No ACR changes authorized by this note.

## Harness Miner

Harness Miner could eventually measure whether candidate Skills earn their existence:

- recurring user corrections matching a Skill's target failure;
- skill invocation frequency;
- false routing/irrelevant invocations;
- token/tool/time deltas with and without the Skill;
- outcome/evidence differences;
- obsolete Skills whose behavior no longer changes results.

History proposes the Skill intervention. Behavioral evaluation earns it.

---

# Final disposition

### MINE STRONGLY

- empirical forks should be tested when the answer is observable;
- orthogonal review axes should remain separate through fan-in;
- gate-bound evidence / evidence expiration when acceptance definition changes;
- minimum sufficient mechanism + explicit ceiling/upgrade trigger;
- progressive reference/workflow disclosure based on task/technology;
- delegation does not transfer acceptance authority;
- change transparency for mutation skills;
- version/provenance for behavior-changing Skills.

### ASSESS

- presentation compression;
- task-class routers;
- dynamic/on-demand Skill installation;
- producer/critic pairings;
- broad "agent operating system" bundles.

### PARK

- design-domain content from Taste/Impeccable/HyperFrames;
- unresolved honorable-mention repos until source identity is unambiguous.

### REJECT AS DOCTRINE

- "top Skill" rankings as architecture evidence;
- install counts/stars as effectiveness evidence;
- same-model self-review as independent verification;
- loading every useful Skill all the time;
- applying heavy completion/orchestration machinery to trivial work;
- "100 experiments" without a trustworthy objective/evaluator.

## Status

Research only. No PMB, work-MB, ACR, Dashboard, Harness Miner or other implementation change is authorized by this note.
