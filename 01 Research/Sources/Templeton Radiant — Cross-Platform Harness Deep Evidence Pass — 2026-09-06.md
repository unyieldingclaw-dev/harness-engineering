# Templeton Radiant — Cross-Platform Harness Deep Evidence Pass — 2026-09-06

## Source

- Repository: https://github.com/templetongroup/radiant
- README: https://github.com/templetongroup/radiant/blob/master/README.md
- Agent operating contract: https://github.com/templetongroup/radiant/blob/master/AGENTS.md
- Latest reviewed branch: `master`
- Review date: 2026-09-06
- Repository is primarily a macOS/Electron harness with a separate Capacitor/iOS surface.
- Current repository state was reviewed through the README, AGENTS.md, and recent commit history, with emphasis on harness behavior rather than UI.

## Corpus relationship

**SHARPENS / MERGES WITH existing harness evidence; ORTHOGONAL to platform-specific UI work.**

This entry should be read alongside:

- Matt Pocock deep evidence: sandbox/isolation, context boundaries, TDD, two-axis review, context/domain modeling, diagnosing, wayfinding.
- Ras Mic / Ralphy: execution profiles, task contracts, isolation, evidence contracts, bounded iteration, publication authority, capability negotiation, diagnostics.
- David Ondrej: decision review, goal/execution contracts, multidimensional isolation, capability/credential/cost/provenance/authority separation, publication serialization.
- Anthropic AI-Native SDLC: committed artifacts, intent/spec/plan, worktrees, independent verification, hooks, continuous evals.
- OpenMAIC: durable execution state separate from model context, capability force-off, persisted state, provenance/lineage, surgical state mutation, checkpoint/replay.
- Hindsight: retrospective memory admission, root-cause compression, proved vs suggested, user-controlled persistence, read-only retrospective.
- Rta-Smriti Brain: evidence taxonomy, deterministic context/evidence compilation, freshness, provenance, durable project state vs model context, continuity/checkpoints.
- PMB Filing Contract: canonical vs derived vs snapshot vs working state, startup context cost, value ownership, stale projections.

Radiant does **not** supersede these sources. Its strongest contribution is implementation evidence showing how a small local harness can make state ownership, task state, approvals, model selection, memory recall, test boundaries, and shipping state explicit in everyday engineering practice.

---

## Executive assessment

Radiant is worth studying despite being Mac-first.

The useful prior art is not Electron, Swift, Capacitor, or the visual cockpit. It is the **discipline embedded in the implementation loop**:

1. A task is an ordinary agent session with a goal attached, rather than a second execution system.
2. Task state is derived from the run's real events rather than manually maintained UI state.
3. Human intervention is represented as a first-class blocked state (`Needs you`) rather than as an informal message.
4. The task board is deliberately prevented from becoming a second source of truth.
5. The harness distinguishes model/provider identity from the neutral session transcript.
6. Local model access is treated as a provider capability, not a different agent architecture.
7. Tool activity is observable as a live event stream.
8. Approval is attached to consequential tool execution, while the project acknowledges where approval/workspace boundaries are still incomplete.
9. Tests are deliberately extracted into pure, cheap, deterministic seams when the real runtime is hard or impossible to exercise.
10. The test suite is expected to **bite**: vacuous assertions and guards that pass without exercising the intended failure are treated as defects.
11. Release/shipping state is treated as separate from repository state.
12. Human-authoritative external systems (for example App Store Connect) are explicitly represented as outside the agent's authority.
13. Memory retrieval is local, model-aware, project-scoped, and supports supersession rather than blind append-only accumulation.
14. The repo repeatedly records what was actually verified, what remains unverified, and what the environment makes impossible to verify.

This is a strong convergence point with the Harness Engineering corpus: **the harness is not primarily a prompt. It is a system of state, authority, evidence, execution, and feedback boundaries.**

---

# 1. Task state should be a projection of execution, not a parallel workflow

Radiant's task board is particularly relevant.

The implementation deliberately makes a task an ordinary session with a goal attached. The board creates the session and hands it to the same run engine used by chat. The task retains the existing agent checklist, approvals, model, and transcript rather than creating a second execution path.

The state machine is:

`Queued → Working → Needs you → Review → Done`

Crucially, only states that represent human disposition are directly mutable by the user. The middle states are produced from execution events. A person cannot simply drag an active run to `Done`; the server rejects that operation.

### Harness lesson

**Do not create a second state machine merely to visualize the first one.**

This sharpens an existing Harness principle:

> A control-plane projection should derive from authoritative execution state whenever practical.

Otherwise the system acquires two sources of truth:

- what the agent actually did;
- what the orchestration UI says happened.

Radiant's explicit refusal to let the board mutate execution state is a concrete implementation pattern for avoiding that drift.

### Relevant source evidence

Task-board implementation commit:
https://github.com/templetongroup/radiant/commit/f2a84b4254657b38ee92a98f77bf811608756968

The commit also documents a useful failure: `ask_user` emitted a question event followed by `done`, causing the board to classify a blocked interaction as Review. The fix was to make the question state blocking so the subsequent `done` event cannot overwrite it.

### Disposition

**ADOPT DESIGN PRINCIPLE — authoritative execution state should own lifecycle; projections should consume it.**

---

# 2. `Needs you` is a useful first-class execution state

Radiant does something simple but important with `Needs you`.

It is not merely a visual warning. It means the run has reached a point where the agent cannot continue until a person acts. The UI deliberately gives that state a single visual treatment and makes an empty `Needs you` column visually ordinary so the signal remains meaningful.

The implementation also asks the more fundamental question: **what event actually makes a task blocked?**

This matters for Harness Engineering because many systems have only:

- running;
- succeeded;
- failed.

That model loses the distinction between failure and legitimate human authority boundaries.

A better execution taxonomy can include:

- waiting for model/tool infrastructure;
- waiting for deterministic verification;
- waiting for human decision;
- waiting for human authorization;
- blocked by policy;
- failed;
- completed.

### Disposition

**HIGH-VALUE RESEARCH / SHARPENS execution-state taxonomy.**

Do not copy the five-column UI. The useful primitive is a semantically meaningful **blocked-on-human state** that is generated from execution semantics.

---

# 3. Steering a running task should reuse the existing delivery path

Radiant added `Steer` to running tasks, but deliberately did not create a second server-side message-delivery mechanism.

The board reuses the existing pending-prompt behavior from chat. A queued card does not expose steering because there is nothing running to steer. A running or human-blocked card can accept a message, and that message goes into that task's own chat.

This is a strong architecture heuristic:

> **When adding a control surface, prefer reusing the existing authoritative execution path over introducing a second delivery path.**

A second delivery path creates semantic drift:

- different authorization rules;
- different ordering;
- different persistence;
- different error behavior;
- different observability.

Radiant explicitly called this out as a design risk.

### Evidence boundary

The implementation verified routing and state retention against a throwaway server but explicitly did **not** claim end-to-end proof that a steer sent during live streaming lands after the current turn settles.

That last distinction is exactly the kind of evidence discipline we want in Harness work.

### Source

https://github.com/templetongroup/radiant/commit/4ad7f846b6027b38ee92a98f77bf811608756968

### Disposition

**ADOPT DESIGN PRINCIPLE — one authoritative delivery path per semantic operation.**

---

# 4. Model neutrality is useful when it means neutral session state, not universal abstraction

Radiant stores sessions in a neutral message representation and allows the model/provider to change mid-conversation. It supports Anthropic, OpenAI, OpenRouter, Ollama, LM Studio, and OpenAI-compatible custom endpoints.

The interesting architectural point is not the provider list. It is that **session identity is not fused to model identity**.

This reinforces the Harness model we have already been developing:

- model identity;
- provider identity;
- execution/runtime identity;
- task/session identity;
- tool/capability identity

should be separately representable.

Radiant's model picker also reveals a practical UI consequence: large model inventories need progressive disclosure, grouping, search, and pins rather than one giant flat list. Its first implementation exposed hundreds of models from OpenRouter and hundreds from other providers; the replacement reused the same picker for models and agents.

### Disposition

**SHARPENS existing Execution Profile / model-provider separation.**

Do not infer that every Harness needs Radiant's exact provider architecture.

---

# 5. Activity feeds are operational evidence, not just UX

Radiant exposes a live activity panel containing tool calls and outputs.

This has a deeper Harness implication: execution should produce an inspectable event stream that can support:

- operator visibility;
- debugging;
- task-state projection;
- provenance;
- retrospective analysis;
- incident reconstruction.

This converges strongly with OpenMAIC's event stream and Hindsight's retrospective model.

The important distinction is between **events** and **durable conclusions**. An activity feed is evidence of what happened; it should not automatically become memory or policy.

### Disposition

**ADOPT BOUNDARY — execution events are evidence; durable conclusions require a separate admission process.**

---

# 6. The repo is unusually good at separating “changed” from “shipped”

Radiant's AGENTS.md contains an explicit rule:

> Written is not shipped.

Its shipping contract requires three independently meaningful conditions:

1. Git state is committed and pushed.
2. The in-app Read Me reflects the user-visible change.
3. Linear reflects the shipped state.

It then runs an objective `ship-check` and can delegate the final synchronization to a cheap `ship-sync` agent.

This is very relevant to our corpus because it makes a distinction that coding-agent harnesses frequently miss:

**repository mutation is not equivalent to user-visible deployment.**

The same pattern appears in the iOS flow: a build can exist locally, a commit can exist on `master`, and Apple can still be serving a different binary. Radiant's instructions explicitly state that App Store Connect is a separate external authority and that the agent must not assume an upload happened.

### Harness lesson

A mature execution contract should model external publication state explicitly when the deployment system is not controlled by Git.

Potential state dimensions:

- source committed;
- source pushed;
- artifact built;
- artifact signed;
- artifact published;
- external platform accepted;
- user-visible artifact updated;
- publication verified.

### Disposition

**ADOPT DESIGN PRINCIPLE — “commit” and “ship” are different authority/state transitions.**

This sharpens Ras Mic's Publication Authority and David's execution/publication separation.

---

# 7. The AGENTS.md itself is a case study in living operational knowledge

Radiant's AGENTS.md is not a generic coding style file. It contains accumulated operational knowledge from actual failures:

- a review-state heading was wrong for nine days;
- a rejected App Store submission was mistakenly treated as still under review;
- a known model defect existed in a shipped binary;
- a release asset was not reaching users;
- an update progress event went to the wrong window;
- a board test once touched the real user data directory;
- a test could pass while testing the wrong CSS rule;
- a browser extension layer could not be verified because Chrome blocked the automated installation path;
- simulator behavior could not stand in for physical-device behavior.

This is exactly the kind of operational memory that can be valuable **if it is maintained as bounded, high-signal instructions rather than becoming an ever-growing transcript**.

But Radiant also demonstrates the danger: its AGENTS.md contains stale historical assertions that had to be explicitly corrected. The document itself says the heading was wrong for nine days.

### PMB connection

This reinforces the PMB Filing Contract work:

> **Don't state a value you don't own. Point at the owner, or carry a test that binds you to it. Measurements own what they stamp.**

Radiant's operational lessons are strongest when they point to an external owner or executable check rather than merely asserting a current value.

### Disposition

**SHARPENS PMB Filing Contract / durable operational memory.**

---

# 8. Radiant demonstrates “tests should bite” unusually well

Several recent commits explicitly document tests that initially passed for the wrong reason.

Examples include:

- two memory assertions that were vacuous;
- a guard that checked a dead CSS selector rather than the live selector;
- a catalogue guard that referenced files that were never checked into the repository;
- UI tests accidentally using the real server and real user data;
- a browser-extension test proving the Radiant side of a protocol while correctly admitting that Chrome itself remained unproven.

Radiant repeatedly uses a strong pattern:

1. identify the intended failure;
2. deliberately remove or revert the protection;
3. prove the test fails;
4. restore the protection;
5. record the environment boundary if full E2E proof is impossible.

This is stronger than merely increasing test counts.

### Harness lesson

**A verification artifact should demonstrate that it can distinguish the protected state from the failure state.**

This is directly compatible with Matt Pocock's evidence-driven testing and our existing verifier/evidence model.

### Disposition

**ADOPT DESIGN PRINCIPLE — require anti-vacuity evidence for high-value guards.**

Candidate verifier metadata:

- intended failure;
- failure injection method;
- observed pre-fix failure;
- post-fix result;
- environment boundary;
- unverified remainder.

---

# 9. Extracting pure seams from hard-to-test runtime is a major pattern

Radiant's iOS download path is a strong example.

The download arithmetic repeatedly failed in production, but the logic lived inside a plugin that could not initialize in the iOS Simulator. That forced the developer/user to become the test harness for simple arithmetic.

The response was not “test more.” It was to move the arithmetic into a pure function:

`values in → values out`

with no filesystem, network, UIKit, or MLX dependency.

The real runtime calls that function; the tests exercise the pure logic cheaply.

This is a general Harness Engineering pattern:

> **When an integration boundary prevents deterministic verification of a semantic unit, extract the semantic unit behind a narrow seam rather than weakening the verification standard.**

The same idea appears in Radiant's server/UI tests and in the model catalogue checks.

### Disposition

**ADOPT DESIGN PATTERN — verification seams before integration-heavy verification.**

This is consistent with Matt's tracer-bullet/TDD evidence and does not require another PMB workflow loop.

---

# 10. Test the environment boundary instead of pretending it does not exist

Radiant is especially good at explicitly recording environmental non-equivalence.

Examples:

- iOS Simulator cannot execute MLX model generation;
- browser previews do not reproduce native Capacitor contracts exactly;
- Chrome prevents automated command-line extension installation;
- App Store Connect is external to Git;
- physical-device behavior must be tested on a physical device;
- hidden browser panes can alter animation timing;
- a packaged app can differ from the dev server.

The correct response is not to label the test “E2E” and move on.

Instead Radiant records:

- what was tested;
- what was not tested;
- why it could not be tested;
- what substitute evidence is available;
- what remains human/manual.

### Harness lesson

This should become part of the Evidence Contract vocabulary:

**Verification scope is a first-class property of evidence.**

Possible fields:

- environment;
- execution mode;
- real vs simulated dependency;
- physical vs virtual target;
- packaged vs development artifact;
- external-system reachability;
- known non-equivalences.

### Disposition

**ADOPT DESIGN PRINCIPLE — evidence must carry verification scope and environment boundaries.**

---

# 11. Temporary test environments are an isolation contract, not a convenience

One Radiant task-board test accidentally used the real port and therefore the real `~/.radiant` data directory. The fix made the port configurable and ran the test against a throwaway server with a throwaway data directory.

That is directly relevant to our isolation research.

A test can be logically correct and still be unsafe if its environment points at real state.

Therefore isolation should cover at least:

- process/runtime identity;
- filesystem/data directory;
- network endpoint/port;
- credentials;
- external service identity;
- publication target.

This reinforces David Ondrej's multidimensional isolation and Matt's Sandcastle work.

### Disposition

**SHARPENS Isolation Contract — “throwaway” must mean data, endpoint, credentials, and external identity, not merely a temporary process.**

---

# 12. Security findings show why local ≠ trusted

One of Radiant's most instructive recent commits found a serious security issue: a browser page could reach the loopback API, obtain configuration, and drive the agent because the system treated loopback as implicitly trusted, reflected arbitrary CORS origins, and did not require approval for file reads/writes.

The fix tightened origin handling and redacted MCP credentials.

The commit explicitly left two behavior-changing questions unresolved: file tools still required no approval, and there was still no workspace jail.

This is valuable because it demonstrates a mature security decision boundary:

**fix the proven vulnerability without pretending unresolved authority questions have already been solved.**

It also reinforces a Harness principle:

> Network locality, process locality, and authorization are separate properties.

`127.0.0.1` is not equivalent to “trusted agent.”

### Disposition

**ADOPT DESIGN PRINCIPLE — locality is not authority.**

This sharpens our existing capability/credential/authority separation and Sandcastle/isolation work.

---

# 13. Memory recall adds a useful supersession pattern

Radiant's latest memory commit is particularly relevant to PMB.

The repo already stored durable facts separately from within-session compaction. It then added semantic recall using a local Ollama embedding model and added supersession when a new fact is sufficiently similar to an older fact in the same project.

The implementation deliberately includes several safety constraints:

- local-only embeddings;
- optional behavior with keyword fallback;
- model identity attached to vectors;
- vectors from another embedding model are ignored rather than compared as if compatible;
- supersession is project-scoped;
- old facts are embedded incrementally on read;
- pure-function tests validate ranking and supersession rules.

The deeper lesson is not “use vector search.”

It is:

**semantic retrieval makes contradiction handling more important, not less.**

Once retrieval becomes better, conflicting memories are more likely to surface together. Therefore retrieval quality and memory precedence cannot be designed independently.

This is a strong connection to Hindsight's root-cause compression and proved/suggested distinction, and to the PMB Filing Contract's canonical ownership model.

### Disposition

**HIGH-VALUE RESEARCH — retrieval + supersession + provenance should be designed together.**

Do not adopt Radiant's numeric similarity threshold as a PMB threshold; it is implementation-specific.

### Source

https://github.com/templetongroup/radiant/commit/a6982d274eedfd492b1f9a9c89caa668034b4559

---

# 14. Memory vectors demonstrate execution identity matters

Radiant refuses to compare vectors generated by different embedding models because cosine similarity between incompatible vector spaces is meaningless.

This is a small implementation detail with a big Harness implication:

**artifacts generated by a model/runtime carry execution provenance.**

For memory embeddings that means:

- embedding model;
- embedding version;
- vector space identity;
- generation date/version;
- project scope.

For other Harness artifacts the equivalent may be:

- model;
- provider;
- runtime;
- quantization;
- context settings;
- tool configuration;
- evaluator version.

This reinforces the Execution Profile concept from prior corpus work.

### Disposition

**SHARPENS Execution Profile / Provenance Contract.**

---

# 15. Supersession is preferable to silent contradiction

Radiant does not simply append a new preference when it conflicts with an older one. A sufficiently similar new fact replaces the old fact while preserving identity and recording what was superseded.

This is not enough for PMB's epistemic problem by itself: semantic similarity does not establish truth, and a new statement should not automatically become canonical merely because it is newer.

But the structural idea is useful:

`current claim ← supersedes ← prior claim`

rather than:

`claim A`
`claim B`

with no relationship.

This gives retrieval and review a chance to distinguish:

- current;
- superseded;
- contradicted;
- uncertain;
- historical.

### Disposition

**ADOPT STRUCTURAL PATTERN — explicit supersession links.**

Pair with Hindsight's proved/suggested gate before allowing a new claim to become authoritative.

---

# 16. “One writer” for sensitive configuration is a useful state-ownership rule

Radiant's AGENTS.md explicitly says `~/.radiant/config.json` has one writer, the server, while window geometry has a separate file to avoid racing that writer.

This is a concrete example of the PMB Filing Contract's value-ownership principle:

> Don't state a value you don't own.

For mutable state, that becomes:

> **One authoritative writer per state domain unless synchronization is itself an explicit contract.**

The alternative is distributed mutation with hidden race conditions.

### Disposition

**SHARPENS state ownership / canonical writer model.**

---

# 17. Dead-code and dead-guard detection is part of verification quality

Radiant repeatedly found cases where a correct-looking guard protected a dead selector, stale file, or obsolete path.

This is a different failure from a missing test.

The test exists. The assertion exists. The protection still fails because the test is attached to the wrong object.

This suggests a useful distinction for Harness verification:

- **assertion presence**;
- **assertion reachability**;
- **assertion bite**;
- **protected-object identity**.

A static pattern test can itself be misleading if it matches a dead representation.

### Disposition

**PRIORITY RESEARCH — guard reachability / target identity.**

This may belong in ACR as a quality-of-verification finding rather than PMB runtime behavior.

---

# 18. Radiant's release practice reinforces publication authority

The repo's release instructions are unusually explicit about the chain from source to artifact to user:

`commit → build → sign/notarize → tag → GitHub release → stable download asset → website version → live verification`

The instructions also distinguish a local archive from a user-available artifact.

This complements Ralphy's Publication Authority and the existing Harness distinction between execution authority and publication authority.

A general execution contract could model publication as a separate state machine:

`prepared → built → verified → published → externally accepted → user-visible → verified`

Not every project needs all states, but the distinction is useful whenever an external store or release channel exists.

### Disposition

**ADOPT DESIGN PRINCIPLE — publication is an authority-bearing workflow, not the last shell command.**

---

# 19. What Radiant does NOT justify

Do not over-read this repository.

### Do not adopt:

- Electron as a Harness architecture requirement.
- Capacitor/iOS as a cross-platform pattern for PMB.
- Radiant's UI/task board as a PMB interface requirement.
- A provider abstraction merely because many providers are supported.
- Vector memory as the PMB storage architecture.
- Radiant's numeric semantic similarity thresholds.
- Loopback networking as an acceptable trust boundary.
- “Approval before shell command” as a complete sandbox/security model.
- AGENTS.md as a substitute for canonical structured memory.
- The repo's accumulated operational notes as evidence that every rule generalizes.
- The Mac physical-device verification model outside analogous platform boundaries.

Radiant is a source of **patterns and failure evidence**, not a reference architecture to copy.

---

# 20. Cross-source synthesis

Radiant becomes substantially more valuable when compared with the other recent corpus entries.

## Radiant + Hindsight

Hindsight says:

`session → retrospective evidence → root cause → durability test → proved/suggested → candidate memory`

Radiant supplies concrete operational evidence and explicit supersession behavior.

Combined lesson:

**Execution events should feed retrospective analysis, but neither events nor semantic similarity should directly create canonical memory.**

## Radiant + Rta-Smriti

Rta-Smriti separates durable project state from bounded context compilation and emphasizes freshness/provenance.

Radiant demonstrates the complementary operational side:

- real execution events;
- task state projection;
- environment-specific verification;
- local memory recall;
- release state.

Combined lesson:

**Durable state, evidence selection, execution state, and model context should remain distinct but composable.**

## Radiant + OpenMAIC

OpenMAIC's event stream, checkpoints, leases, recovery, and durable runtime state provide the deeper execution-state model.

Radiant demonstrates a smaller practical implementation: task lifecycle projected from run events and an explicit blocked-on-human state.

Combined lesson:

**A durable execution state machine can be useful without making the agent itself the owner of the state machine.**

## Radiant + Matt Pocock

Matt's work emphasizes:

- deterministic seams;
- tests that bite;
- independent review;
- context boundaries;
- isolated execution.

Radiant supplies multiple real-world examples of those principles being discovered through failures.

Combined lesson:

**Verification quality is about causal discrimination, not test count.**

## Radiant + Ras Mic / Ralphy

Ralphy contributed:

- task contract;
- isolation contract;
- evidence contract;
- execution profile;
- bounded iteration;
- publication authority.

Radiant gives concrete support for nearly all of these, particularly task state, evidence boundaries, runtime identity, and publication state.

## Radiant + PMB Filing Contract

The PMB Filing Contract identified stale projections, canonical ownership, derived values, snapshots, and startup-context pressure.

Radiant provides a cautionary example: its AGENTS.md accumulated valuable operational knowledge but also contained stale status claims.

Combined lesson:

**Operational memory needs both high signal and explicit ownership/freshness mechanisms.**

---

# 21. Candidate Harness capabilities sharpened by Radiant

Radiant strengthens the following capability candidates already present in the corpus:

| Capability | Radiant contribution | Status |
|---|---|---|
| Task Contract | Goal attached to ordinary session; task remains tied to run | SHARPEN |
| Execution State Projection | Board state derived from execution events | ADOPT PRINCIPLE |
| Human Blocked State | `Needs you` is first-class | HIGH-VALUE RESEARCH |
| Delivery Path Reuse | Steering reuses existing prompt path | ADOPT PRINCIPLE |
| Execution Profile | Model/provider identity separated from session | SHARPEN |
| Evidence Contract | Explicit verification scope and unverified boundaries | ADOPT PRINCIPLE |
| Isolation Contract | Throwaway server + throwaway data + distinct port | SHARPEN |
| Verification Bite | Tests proved to fail when protection removed | ADOPT PRINCIPLE |
| Verification Seam | Pure arithmetic extracted from plugin runtime | ADOPT PATTERN |
| Publication Authority | Commit/build/release/external-store distinction | ADOPT PRINCIPLE |
| State Ownership | One writer per sensitive config domain | SHARPEN |
| Memory Supersession | Explicit replacement relationship | ADOPT STRUCTURE |
| Memory Provenance | Embedding model identity carried with vector | SHARPEN |
| Context Compiler | Not directly implemented; context remains in sessions | NO NEW ADOPTION |
| Sandboxing | Approval exists but workspace jail is absent | RESEARCH ONLY |
| Autonomous Loop | Not the core contribution | NO ADOPTION |

---

# 22. Proposed research experiments

These are deliberately experiments, not architecture changes.

## Experiment A — Execution-state projection

Build a tiny reference state machine where:

- execution emits events;
- lifecycle state is derived from those events;
- UI/control surfaces are read-only projections except for explicitly human-owned states.

Measure:

- duplicate state;
- event ordering defects;
- projection lag;
- impossible states;
- human override semantics.

## Experiment B — Evidence scope receipts

Extend the existing Evidence Contract research with:

- environment;
- dependency realism;
- packaged/development artifact;
- physical/virtual target;
- external-system reachability;
- known non-equivalence.

Require every high-value verifier to state what it does not prove.

## Experiment C — Anti-vacuity verifier

For selected Harness guards, record:

1. intended defect;
2. failure injection;
3. pre-fix failure;
4. post-fix pass;
5. protected-object identity;
6. environment scope.

The goal is not to require this for every unit test. It is to test whether this metadata improves confidence in high-value gates.

## Experiment D — Durable memory supersession

Compare three memory admission strategies:

1. append-only;
2. explicit supersession;
3. supersession plus Hindsight-style proved/suggested admission.

Measure:

- contradiction rate;
- duplicate retrieval;
- stale retrieval;
- false supersession;
- later-session correction rate;
- token cost.

## Experiment E — Publication state

Prototype a small publication ledger:

`source → build → verification → publication → external acceptance → user-visible verification`

Measure whether it catches real “committed but not shipped” failures without creating another heavyweight workflow.

---

# 23. Final disposition

### ADOPT DESIGN PRINCIPLES

- Execution projections should derive from authoritative execution state.
- `blocked on human` is semantically different from `failed`.
- Add control surfaces by reusing authoritative delivery paths.
- Evidence must carry verification scope and known non-equivalence.
- Tests should demonstrate that they can distinguish the failure state.
- Extract pure verification seams when runtime environments prevent deterministic testing.
- Locality is not authorization.
- Commit/build/publication/user-visible state are distinct transitions.
- One authoritative writer should own a mutable state domain.
- Supersession should be explicit rather than leaving contradictory current-looking records.

### HIGH-VALUE RESEARCH

- Execution-state projection and human-blocked state.
- Retrieval + supersession + provenance as one memory problem.
- Verification-scope receipts.
- Publication authority/state.
- Guard reachability and protected-object identity.

### SHARPENS EXISTING CORPUS

- Execution Profile.
- Task Contract.
- Isolation Contract.
- Evidence Contract.
- Publication Authority.
- PMB canonical/derived/snapshot model.
- Hindsight memory admission.
- Rta-Smriti evidence/context separation.
- OpenMAIC durable execution state.
- Matt's evidence-driven testing.
- Ras Mic/Ralphy execution boundaries.

### DO NOT ADOPT

- Radiant wholesale.
- A second PMB datastore.
- Mac/Electron-specific architecture.
- Vector memory as PMB's storage model.
- Numeric thresholds as policy.
- UI task board as a required Harness component.
- Approval prompts as a substitute for sandboxing.

---

## Bottom line

**Yes, there is a lot to learn from Radiant.**

The Mac-specific implementation is mostly incidental. The important part is that the repository repeatedly turns real agent failures into explicit boundaries:

- execution state instead of decorative state;
- human blocking instead of guessing;
- one delivery path instead of parallel mechanisms;
- throwaway environments instead of “trust me” tests;
- pure seams instead of runtime-dependent testing;
- evidence scope instead of fake E2E confidence;
- explicit publication state instead of “Git says done”;
- supersession instead of contradictory memory;
- model identity on derived artifacts instead of treating all vectors/results as interchangeable;
- operational instructions that point toward external truth rather than pretending the file itself is always current.

The strongest synthesis with the current Harness corpus is therefore:

> **A mature agent harness is a system for maintaining authoritative state, constraining authority, producing inspectable evidence, and projecting bounded state into the agent — not merely a collection of prompts, skills, or model calls.**

That is the part of Radiant worth carrying forward.
