# Nate B. Jones & Low Level — REA Behavioral Discovery, Reconstruction Limits & Obligation Gates — 2026-10-23

**Disposition: MINE research techniques and gate definitions; no installation, external-target reconstruction or host configuration change.** Follow-up source to [Fabian Markl's 15-skill triage](Fabian%20Markl%20%E2%80%94%20Popular%20Claude%20Skills%2C%20Privilege%20Surfaces%20%26%20Trial%20Dispositions%20%E2%80%94%202026-10-23.md) in **draft HE PR #23**. These videos reuse the *same* REA project; only Low Level's distinct hands-on calculator run is potentially independent experience. Do not treat three video mentions as three independent product validations.

## Source/provenance ledger

| Source | Scope reviewed | Limits |
| --- | --- | --- |
| [Nate B. Jones — Two Ways To Use Reverse Engineer Anything](https://www.youtube.com/watch?v=PseF3LyXQ04) (~22:30) | Full user-provided auto-transcript, `Pasted text(20261023-203810).txt`, SHA-256 `89a3db27e20b8160f285cb2596d7cc3f38b7bce20d2408264e900b71fd8ad259`. User's chapter locations and full supplied text. | Mostly interpretation of **REA's own published cases**; no independently reproduced REA runs, original binaries or standalone test artifacts in interview. |
| [Low Level — I Tested The Viral Reverse Engineering Repo](https://www.youtube.com/watch?v=qFEuYUfauNY) (~16:20) | Full user-provided auto-transcript, `Pasted text (2)(4).txt`, SHA-256 `e598d1f3f2d550f4939dce3a02cd3398e30577f5e6d78593f9e9868350b040ea`. | First-person single demonstration on GNOME Calculator; no independent before/after baseline, logs, test matrix, output repo or ability to reproduce configuration. Sponsorship/CodeRabbit segment is not evidence of REA or ACR effectiveness. |
| [REA source @ `f2db494bfca6`](https://github.com/morluto/rea/tree/f2db494bfca62528a48fc219038a2bd883dbb9f3) | README, canonical Skill, security statement, five case evidence notes, reconstruction-obligation-ledger documentation and relevant checked-in tests. Commit dated Oct 23, 2026. | **Source inspected; tests not executed.** Source docs can describe contracts but are not host-observed enforcement or independently reproduced functionality. |
| [REA casebook @ `6514c626860f`](https://github.com/aliaihub/rea-casebook/tree/6514c626860fabfabf84686bbfced1b3329a3704) | Four focused case summaries, especially Ledger Reborn and DX-Ball; last repo revision Oct 17, 2026. | Curated secondary project summaries. Its Ledger Reborn review says README reviewed, no application run/test; do not turn into verified functional parity. |

**Terminology:** `OBSERVED-IN-VIDEO` = visibly reported in a demo, **not repeated by HE**; `PROJECT-EVIDENCE` = REA checked-in detailed record with a pinned target/build and stated limits; `SOURCE-CONTRACT` = docs/tests appear in source but no local runtime verification; `HE-INFERENCE` = candidate reusable method. Native decompiler pseudocode is a **reconstruction / inferred representation**, not recovered original C. User-provided transcripts can have proper-name, product and numerical recognition errors.

## Major interview findings — two distinct uses

### 1. Product / requirements discovery (Nate: 03:38–13:19)

Nate's clearest argument is that a requirement like “copy/paste should work,” “percentage button,” “record a payment,” “send reminder,” or “undo” is **under-specified** until context, state, persistence, exceptions and interoperability are addressed.

- **Nate 04:28–08:48:** Research-tool example: quote is text or an object? Does its source reference travel on copy? Does editing a derived copy mutate the original? Does the receiving app have the same stored references? These are *requirements decisions* discovered by investigating a comparable implementation, not automatically “the best architecture.”
- **Nate 05:43–10:59:** Notion clipboard provides a useful design fork between text/HTML compatibility and structured block data; identifier-mediated local state may not survive foreign machines or deletion. Public REA [Notion evidence](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/website/evidence/notion-clipboard.md) **narrows** the claim: a 24,579-byte preload was analyzed; its traced graph had partial coverage and one seed produced no match. Separate extracted web-editor **stubbed/lifted Node/JSDOM probes**, *not REA's live desktop capture*, support a matching-ID rich clipboard recovery policy. No complete in-app clipboard execution, Notion server implementation or comprehensive model is established.
- **Nate 11:01–13:19:** Windows Calculator's percent behaves differently depending on prior operation (e.g. `200 + 10% → 220` vs `200 × 10% → 20`). [REA calculator evidence](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/website/evidence/calculator-and-dino.md) describes a particular installed DLL's branch and references Microsoft source/tests. It says the upstream tests **were inspected but not executed**. This is an illustrative context-dependent behavior, not proof all calculator modes work.
- **Nate 13:20–16:29:** Ledger Reborn is portrayed as an independent Python+SQLite implementation informed by focused QuickBooks Desktop for Mac investigations into inventory, reconciliation, reporting and balances. [REA casebook](https://github.com/aliaihub/rea-casebook/blob/6514c626860fabfabf84686bbfced1b3329a3704/cases/ledger-reborn.md) calls it a **preview**, flags omitted workflows and distinguishes public synthetic checks from private comparisons; explicitly **did not execute application or tests**. This is implementation-existence evidence, **not** a verified QuickBooks-equivalence result.
- **Nate 16:30–18:45:** Chromium-derived dinosaur game's acceleration is a bounded recovered rule, not proof of recreating the entire game. REA evidence labels newly authored controls/jump physics and replay as distinct from the specific extracted speed behavior. DX-Ball pan mapping has strong **function-scoped** developer-reported evidence (next section).

**HE-INFERENCE:** a *behavioral question* can be made into a small design-choice and acceptance matrix: initial state, action/inputs, hidden context, output, persistence, portability, exceptional cases, and any source/data identity that must survive. This enriches spec elicitation and behavioral evals. It does not imply **use REA before every spec** or copy somebody else's code/UX.

### 2. Focused reverse-engineering / reconstruction (Low Level: 02:57–11:30)

- At **02:57–06:26**, Low Level explains that compiled binaries lose source names/comments/types and decompilers infer C-like pseudocode from machine code; the pseudocode is not original source. REA's distinctive contribution is arranging **agent → tool evidence → narrower question → deeper probe** around traditional Ghidra/IDA/Hopper machinery, *not* a novel standalone decompiler.
- At **06:27–08:58**, Low Level reports an agent using REA/Claude Code to inspect GNOME Calculator's **local Linux ELF binary** (rather than its available public source), then generate a new **Python** recreation in about **28 minutes**. The camera demonstrates a similar-looking window and a limited arithmetic calculation. It also **visibly reveals a misplaced mode selector and broken resize layout**. His estimate that a person would need days/weeks and his approximations of target size and function count are informal, not a controlled timing comparison.
- At **09:00–11:30**, he describes an iterative question/evidence loop and notes similar-looking results have been reported **without** REA. This is a **counterpoint to any claims that REA alone caused the speedup**. He explicitly does not test the Electron/browser path in this demo.
- At **11:30–16:18**, he raises maintenance, open-source supply-chain trust, redistribution/incentive and potential vendor cloud-only responses. These are **hypotheses and normative concerns**, not observed adoption forecasts. The CodeRabbit-sponsored PR triage segment at **01:19–02:56** does not validate REA or recommend replacement of ACR.

**HE-INFERENCE:** one convincing visible behavior check cannot establish **application equivalence**. The demo itself supplies a negative fixture: a visually similar application with happy-path arithmetic but incorrect resizing/layout. Acceptance requires independently defined behavior/edge/accessibility/runtime/system contracts; appearance is a different axis from semantic correctness.

## Strongest concrete source finding: REA's *reconstruction obligation ledgers*

REA now has a [documented obligation-ledger CLI/MCP](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/docs/reconstruction-obligation-ledgers.md), `build_reconstruction_obligation_ledger` / `rea build-reconstruction-obligation-ledger`, plus source [closure/negative tests](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/src/application/ReconstructionObligationLedger.closure.test.ts). This mechanism is **more interesting to HE than the demo UI**.

The document describes conservative obligations derived from authenticated Evidence + reviewed claims. It requires a unique owner, relevant parser/schema/domain type, required original/candidate case fixtures, and a verifier of **comparable authority**. It binds fixture/verifier proof to the **specific owner implementation digest** and **obligation revision**; an old passing result cannot close a changed implementation. Missing evidence yields `unknown` (zero obligations do not produce a green/ready system). Explicit failure remains failure; skip/unavailable/weak authority do not silently become passes. The source contains tests covering incomplete/unknown coverage, weak authority, missing cases, mismatched proofs, and closure conditions. **These tests were read, not run.**

**HE owner:** Existing HE work already owns evidence durability, authority, artifact-gated acceptance, stale proof, unknown-not-green and blast-radius checks. The source sharpens that with a **single explicit relation:** *claim → owner/digest → original-case authority → fixture → verifier → current proof applicability*. This is an **assessment/falsifier** for HE-001 and downstream ACR, not a proposal to import REA's ledger schema or create a second canonical source store.

Suggested HE negative fixtures, using **synthetic/owned** code only:
1. Required behavior `copy quote with source` passes locally but fails when pasted without shared local state → **UNKNOWN/FAIL**, not complete.
2. Calculator `200+10%` passes, `200×10%` fails → **FAIL**, not “percent works.”
3. Target supports right-sizing? only initial visual screenshot tested, resizing breaks → **FAIL** for layout requirement and **UNMEASURED** for keyboard, accessibility, advanced modes.
4. A reconstructed library passes unit tests but its requirement demands packaged-process equivalence → **INSUFFICIENT EVIDENCE**.
5. Verified candidate implementation digest changes after a one-line edit → old verifier receipt becomes **STALE**, no green carryover.
6. A function-scoped native binary replay passes thousands of cases → allow **function-specific** pass; **no** application-wide correctness promotion.
7. Partial code graph or failed seed query returns no result → **UNKNOWN outside search boundary**, not “no such behavior.”
8. An agent claims a private backend implementation based solely on browser-visible requests → **INFERRED/UNKNOWN**, not source-recovered.

**Test contract:** include source artifact license and immutable digest, observed environment/version, question/scope, observation method/evidence IDs, authority strength, *explicit exclusions*, candidate owner/digest, independent test oracle, result, post-edit applicability and human review of side effects.

## Case studies: published evidence ceilings (do not overclaim)

| Case | Strongest check documented in pinned REA source | What it **does not** prove |
| --- | --- | --- |
| **Aegis** Android authenticator | [Detailed evidence](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/website/evidence/aegis-login-code.md) records target APK digest, selected analysis Evidence IDs, static method traversal and JVM-level reconstruction checks. | Original released APK was **not run** in that test. No verified complete auth app, Android UI, vault/key-storage security or deployment correctness. |
| **Notion** clipboard | Saved REA preloader inspection + separate extracted-code/probe checks for rich clipboard identifier behavior, with explicit partial-graph coverage. | No complete captured OS clipboard workflow or private Notion server code. |
| **Windows Calculator** percentage | A checked interpretation of an installed DLL function and public source/tests, against focused percent scenarios. | Microsoft tests were **read, not executed** in the case. No universal UX/functionality coverage. |
| **Dinosaur** game | One measured/replayed speed rule and independently authored teaching game. | The newly authored dinosaur/game physics are not the original implementation. |
| **DX-Ball** sound panning | [Evidence note](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/website/evidence/dx-ball-sound-pan.md): recorded **3,205 differential original-x86 cases** and **63 matching compiled bytes** for one function at [pinned project checkpoint](https://github.com/N0zoM1z0/dx-ball/tree/a55dca27ec0a07018c1b2c95ae2be027f7d8c3c4); the website update reused recorded results. | Not independently re-executed in this HE review, and not evidence of complete game equivalence. |
| **Ledger Reborn** | Public independent application preview and maintainer-reported use of REA, supported by a [casebook entry](https://github.com/aliaihub/rea-casebook/blob/6514c626860fabfabf84686bbfced1b3329a3704/cases/ledger-reborn.md). | Casebook reviewed README, not app/test execution; omitted workflows, unknown equivalence with QuickBooks and unverified private comparative tests. |
| **GNOME Calculator** (Low Level) | First-person video of binary inspection leading to Python UI and one arithmetic check (~28 min), including observable layout failures. | No full end-to-end reconstruction, no controlled REA-vs-no-REA uplift, no accessibility/security testing, no repeatability data. |

**Source correction:** Nate's broad line that REA can show how *all* software is built is a metaphor, not a capability claim. Only accessible browser/packaged/client artifacts and **authorized** runtime observations are in scope; a private service backend is not recoverable by looking at its website. Even accessible binary pseudocode is interpretive and lossy. Low Level's “complete recreate any application” introduction similarly overstates what his own evaluation shows.

## Security/permissions and practical HE scope

- [REA `SECURITY.md`](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/SECURITY.md) explicitly states **not a sandbox** and cannot defend against a malicious same-user process. An untrusted binary is parsed by a selected local engine under the host user's privileges; authenticated local bridge tokens do not change that boundary.
- [REA Skill](https://github.com/morluto/rea/blob/f2db494bfca62528a48fc219038a2bd883dbb9f3/.agents/skills/reverse-engineer-anything/SKILL.md) explicitly says **skip REA for ordinary complete source-repository analysis**; use normal source tooling. Installing the `SKILL.md` is not the same as registering MCP, and connected tool lists depend on actual host/release.
- Local static analysis does **not** mean confidentiality from the AI model provider: agent prompt/tool contents are governed by that separate data route.
- Do not inspect employer-private/proprietary apps or other products absent authorization, disclosure, licensing and acceptable-use review; third-party licenses and independent implementation IP can matter. No circumvention, credential extraction or backend probing follows from this source note.
- Typical HE / PMB / ACR source-visible code has direct code and test access. REA is primarily justified when **the required fact resides in a distributed artifact or observable behavior missing from available source**, not as an always-on prerequisite for planning.

## Final dispositions

**MINE (principles/test contracts):** a behavior-to-requirement elicitation checklist; scoped source-vs-inference labels; owner/digest/obligation-specific independent acceptance; failing demo cases must be preserved, not hidden by polished screenshots. These **sharpen existing canonical HE owners**, including `Artifact-Gated SDLC, Behavioral Evals & Metrics`, `Source Preservation & Evidence Durability`, `Skill Routing, Gate-Bound Evidence & Orthogonal Review`, `Risk-Directed Review & Release Proof`, and the HE-001 assessment work. Existing HE PR #23 already contains the initial REA pilot suggestion.

**ASSESS (future only, separate approval):** one *controlled owned-source-vs-binary* REA test in disposable environment: freeze source ground truth, compile a small project with hidden corner cases, restrict the investigator to binary + documented APIs, compare **without-REA** versus **with-REA** agent on identical tasks under comparable budget/model/settings, run independent behavioral fixtures and inspect evidence ledger. Report cost, missed behaviors, ambiguous inferences, privilege footprint, false-green cases, repeatability and cleanup. Establish actual host compatibility before enabling MCP.

**PARK:** large app cloning, paid toolchain acquisition, broad REA installation, general-purpose codebase analysis through REA, and adopting entire REA obligation-ledger framework. No broader system design choice follows from a viral video.

**REJECT:** “28 minutes = complete recreation,” “REA sees private backend code,” “pseudocode = exact source,” “100k GitHub stars prove it works,” “two videos + earlier #23 are three independent successes,” “a sophisticated source-level ledger proves security,” and “a model/provider's own judgment can approve its unverified output.”

## What was not done

No REA setup, provider engine installation, app reverse engineering, packaged target execution, licensed software copying, ACR integration, PMB changes, external-model calls on private artifacts or REA test execution. Source files were *read via GitHub*; all findings that depend on private runtime or independently replayed tests remain **not verified**. This PR stays **research only** until an owner approves a strictly bounded test.
