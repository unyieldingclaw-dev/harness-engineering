# Pav Rusovs — Public GitHub Portfolio Follow-Up — 2026-09-25

## Purpose

Follow up the MAPS video/repository review by inspecting **all public repositories currently returned for `pavrus117`** and mining additional Harness Engineering mechanisms rather than assuming the MAPS guide repository contains the implementation shown in the video.

This note complements:

- `01 Research/Sources/Pav Rusovs — MAPS AI OS, Navigable Memory & Derived Screens — 2026-09-25.md`
- `01 Research/Navigable Truth, Routine Manifests & Derived Screens — 2026-09-25.md`

Research only. No third-party code is vendored into HE and no external architecture is authorized by this note.

---

## Public repository inventory observed

GitHub repository search for owner `pavrus117` returned three public repositories:

1. `pavrus117/ai-os-maps-guide`
2. `pavrus117/studio-skill`
3. `pavrus117/cursor`

### `ai-os-maps-guide`

Already mined in the MAPS source note. At review time it contains the guide README and PDF, not the implementation behind the dashboard/routines shown in the video.

### `studio-skill`

This is the only additional non-empty implementation repository and contains substantial HE-relevant code/policy.

Pinned revision reviewed:

`360be943a394b8ed14e5d79f7f4b978567cabe1e`

Repository contents at that revision include:

- `SKILL.md`
- `scripts/gen.py`
- `scripts/gen_video.py`
- `INSTALL-WITH-CLAUDE.md`
- `spend-cap.txt`
- README / guide / license

License: MIT according to the repository README/LICENSE.

### `cursor`

The repository is empty; GitHub returns no commit history. There is nothing substantive to mine.

### Important conclusion

No additional public repository in this account currently exposes the actual MAPS dashboard, brain builder, scheduler, sync layer, Telegram interface, or queue implementation shown in the video.

Therefore HE should distinguish:

- **MAPS implementation claims/specification** — available in the guide/video;
- **public implementation evidence** — not currently available for most MAPS internals;
- **studio-skill implementation evidence** — available and independently inspectable.

---

# Deep dive: `studio-skill`

## 1. Policy text plus deterministic enforcement

The skill separates human-readable policy from machine-enforced guards.

`SKILL.md` says paid generation must follow:

```text
quote
→ explicit yes
→ confirmed execution
→ receipt
```

The scripts independently refuse execution without `--confirm`, perform local cap checks, validate inputs, and reject malformed provider responses.

### HE translation

This is stronger than relying on prompt instructions alone:

> **Use prose to explain intent and deterministic code to enforce invariants that can be checked mechanically.**

However, the implementation also exposes an important limit: `--confirm` proves only that a flag was supplied. It does **not** prove that a human actually approved the exact operation in the conversation.

**Disposition: REINFORCE hybrid policy; do not confuse execution flag with approval evidence.**

---

## 2. Quote mode is deliberately side-effect free

The scripts expose a quote path that computes expected cost without making the paid provider call.

This creates a useful separation:

```text
PREVIEW / QUOTE
  no paid side effect
       ↓
HUMAN DECISION
       ↓
EXECUTE
```

### HE translation

For consequential operations, prefer an explicit **preview/plan/quote phase** that can be inspected before authority is consumed.

Examples beyond money:

- deployment plan before deploy;
- affected files before mutation;
- external recipients before send;
- deletion set before delete;
- estimated resource use before expensive batch execution.

**Disposition: REINFORCE.**

---

## 3. Price-affecting inputs are bound explicitly

The video generator deliberately refuses ambiguous/autonomous duration because cost cannot be known before the provider chooses the duration.

It also sends price-sensitive provider parameters explicitly rather than relying on server defaults. Example: Seedance resolution is included in the request because a 480p quote paired with a provider default of 720p would silently underquote the real operation.

The implementation similarly refuses an `--audio` option for providers where enabling audio changes the billed rate without a separately updated quote.

### HE translation

This is a strong general pattern:

> **If approval depends on a parameter, execution must bind that parameter explicitly rather than inherit an uncontrolled default.**

The approved plan and executed operation should describe the same side effect.

**Disposition: REINFORCE strongly.**

---

## 4. One-operation approval scope

The written policy says one approval covers one run; batches are quoted as batches and capped at the approved count.

This is better than a broad standing approval because it keeps authority bounded to a concrete operation.

### HE translation

A useful authorization object should ideally bind:

- operation/type;
- parameters;
- expected cost/resource bound;
- count/scope;
- expiry or one-shot semantics.

A plain `--confirm` flag does not encode all of that, but the policy points in the right direction.

**Disposition: REINFORCE concept; ASSESS stronger machine-binding for consequential tools.**

---

## 5. Hard-cap configuration has a single edit point and no runtime override

The skill stores the monthly cap in `spend-cap.txt`. The CLI provides no flag or environment-variable override to raise it.

This is a good instinct: runtime execution should not be able to casually widen its own budget merely by passing another option.

### Important limitation

The cap file itself is ordinary writable repository/skill state. If the same agent being constrained can edit `spend-cap.txt`, the control is not a security boundary.

### HE translation

This reinforces the recent monotonic-policy finding:

> **Configuration that widens authority should be owned/protected outside the authority of the actor it constrains.**

“No command-line override” is useful but insufficient if the constrained process can rewrite the policy file.

**Disposition: REINFORCE principle; reject the file alone as a hard trust boundary.**

---

## 6. Receipts and spend ledger create provenance

Each successful generation writes a sidecar receipt containing model, dimensions/duration, approximate cost and prompt, and appends a spend record to `spend-ledger.jsonl`.

This gives a human-readable local evidence trail.

### HE translation

For external/consequential effects, useful provenance often includes:

```text
what was requested
what was approved
what actually executed
which provider/model/tool performed it
when
with what bounded cost/authority
what artifact/result was produced
```

This is stronger than a chat statement saying “done.”

**Disposition: REINFORCE.**

### Privacy caution

The receipt stores the **full prompt**. In corporate/sensitive environments, provenance requirements must be balanced against data minimization and retention policy.

**Disposition: ASSESS per environment; do not universally log raw prompts.**

---

## 7. Atomic artifact writes are good, but the whole transaction is not atomic

The output artifact is written through a temporary file and renamed only after basic content validation. This avoids leaving a partial image/video as if it were complete.

That is good local filesystem hygiene.

But the broader paid transaction is not atomic:

```text
cap check
→ paid provider call
→ download
→ output write
→ receipt write
→ ledger append
```

A crash or filesystem failure after the paid call but before the ledger append can leave real spend missing from the local cap accounting.

Likewise, a receipt-write failure occurs **after** the external charge has already happened.

### HE translation

> **Local artifact atomicity is not external side-effect atomicity.**

For bounded-budget systems, consider reservation/settlement/reconciliation rather than only check-then-record.

**Disposition: MINE the positive atomic-write pattern; ASSESS stronger transaction semantics.**

---

## 8. The “hard cap” has concurrency and corruption gaps

Both generation scripts compute current spend by reading the ledger, then check whether the proposed operation would exceed the cap, then perform the paid call, and only later append the spend.

This creates a check-then-act race:

```text
Process A reads $8 spent, plans $2
Process B reads $8 spent, plans $2
A passes
B passes
both spend
```

Two concurrent runs can therefore exceed the nominal cap.

The ledger reader also skips corrupt JSON lines. A corrupted spend entry therefore disappears from the computed monthly total.

### HE translation

For a control advertised as a hard resource ceiling:

- serialize/reserve budget atomically;
- treat ledger corruption as degraded/unsafe rather than silently zeroing evidence;
- reconcile against the provider when possible;
- expose UNKNOWN when authoritative spend cannot be established.

This parallels HE's wider rule that missing/unparseable evidence should not silently become a successful or zero state.

**Disposition: IMPORTANT CAUTION.**

---

## 9. Local ledger cost is estimated, not authoritative provider billing

The scripts use locally maintained approximate prices. The repository explicitly warns that provider prices and billing dimensions drift and that larger images can exceed the flat local quote assumptions.

The commit history shows the author actively updates model IDs, prices, resolution behavior and model defaults when the provider changes.

### HE translation

A locally calculated cost is **derived planning data**, not provider-owned billing truth.

Display it as:

```text
estimated / quoted
```

not:

```text
actual charged
```

unless the provider returns authoritative billing data.

**Disposition: REINFORCE provenance/authority classification.**

---

## 10. Capability routing is explicit and evidence-oriented

The Skill contains model-selection rules based on job characteristics rather than simply selecting the largest/newest model. Examples include:

- text fidelity;
- real-product preservation;
- physics/motion quality;
- duration requirements;
- price tiers.

The user's explicit model choice overrides the automatic routing table.

The commit history also shows routing defaults being changed when new evidence/models arrive rather than preserving them indefinitely.

### HE translation

This is a compact example of routing policy:

```text
task property
→ capability requirement
→ cheapest/appropriate tier that clears the requirement
```

The useful part is not these media-model choices; it is the principle that routing policy should expose **why** a capability was selected and remain revisable as evidence changes.

**Disposition: REINFORCE.**

---

## 11. Backward-compatible aliases reduce migration friction

The video script retains old aliases such as `seedance` and maps them to current implementations instead of immediately breaking old commands/docs.

### HE translation

Capability evolution can preserve thin compatibility aliases while moving the canonical implementation forward, provided aliases remain explicit and do not mask semantic incompatibility.

**Disposition: MINE lightly.**

---

## 12. The installer verifies refusal behavior, not just happy-path setup

`INSTALL-WITH-CLAUDE.md` asks the installer to run two tests:

1. quote mode succeeds without spending;
2. an unconfirmed generation **must refuse**.

Testing the negative safety property is important.

### HE translation

> **Installation/preflight should verify critical refusal behavior, not merely that the tool launches.**

For safety-sensitive capabilities, a successful install includes proof that forbidden execution is still forbidden.

**Disposition: REINFORCE strongly.**

---

## 13. No automated test suite was observed in the public tree

At the pinned revision, the repository root contains no visible test directory/suite. The installer performs smoke/refusal checks, but implementation-level regression coverage is not evident from the public tree reviewed.

### HE translation

This limits how strongly HE should treat the implementation as validated prior art, especially around cap accounting and provider changes.

**Disposition: EVIDENCE LIMITATION.**

---

# Cross-repository conclusion

Pav Rusovs' public GitHub account currently provides two substantive HE sources with different strengths:

## MAPS guide

Strongest for:

- source-owned truth;
- navigable memory/signposts;
- derived screens;
- bounded unattended agents;
- structured routines;
- dashboard/data ownership separation.

## Studio Skill

Strongest for:

- preview/quote before side effect;
- explicit one-run approval;
- deterministic refusal gates;
- budget ceilings;
- receipts/provenance;
- binding cost-affecting parameters;
- negative safety preflight;
- capability routing under explicit cost/quality tradeoffs.

It also reveals important limitations that HE should preserve:

- local accounting is not provider billing truth;
- check-then-act caps race under concurrency;
- corrupt ledger evidence must not be silently skipped for safety decisions;
- a policy file is not a hard boundary if the constrained actor can edit it;
- post-effect receipt/ledger writes can fail after the real-world charge already occurred.

`cursor` contributes no evidence because the repository is empty.

---

## Overall disposition

- **REINFORCE:** preview/quote before consequential external action.
- **REINFORCE:** approval should be narrowly scoped to the operation being authorized.
- **REINFORCE:** cost/authority-affecting parameters must be explicit at execution time.
- **REINFORCE:** deterministic refusal gates + human-readable policy are complementary.
- **REINFORCE:** successful external actions should leave provenance/evidence.
- **REINFORCE:** safety installation tests should include expected refusal paths.
- **REINFORCE:** provider routing should be reasoned from task requirements and revisable evidence.
- **ASSESS:** reservation/settlement/reconciliation for hard resource caps.
- **ASSESS:** stronger machine binding between approval and exact execution parameters.
- **CAUTION:** local ledger/estimated cost is not authoritative provider billing.
- **CAUTION:** ordinary writable policy files do not constrain an agent that can edit them.
- **CAUTION:** concurrency makes check-then-act budget gates unsafe as hard ceilings.
- **CAUTION:** receipt/ledger creation after the external side effect creates accounting gaps on failure.
- **PARK:** adopting Studio itself; media generation is not HE's mission.
- **REJECT:** treating the MAPS guide repository as the public implementation of the dashboard/routines shown in the video.
