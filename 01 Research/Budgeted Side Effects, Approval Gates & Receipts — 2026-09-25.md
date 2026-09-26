# Budgeted Side Effects, Approval Gates & Receipts — 2026-09-25

## Purpose

Synthesize the durable Harness Engineering lessons from Pav Rusovs' `studio-skill` without adopting the media-generation product itself.

Source evidence:

- `01 Research/Sources/Pav Rusovs — Public GitHub Portfolio Follow-Up — 2026-09-25.md`

Related HE themes:

- bounded authority;
- monotonic policy;
- tool success versus verified effect;
- human acceptance authority;
- source-owned telemetry/provenance.

---

## Core finding

Consequential agent actions benefit from a control loop richer than `permission=true`.

A stronger shape is:

```text
1. PREVIEW / QUOTE
   describe exact intended effect + bounded cost/scope
             ↓
2. APPROVAL
   human authorizes that specific operation
             ↓
3. RESERVATION / POLICY CHECK
   deterministic code confirms remaining authority/budget
             ↓
4. EXECUTION
   external side effect occurs
             ↓
5. SETTLEMENT / VERIFICATION
   determine what actually happened and what was actually consumed
             ↓
6. RECEIPT / PROVENANCE
   durable evidence links request, approval, effect and result
```

The useful HE lesson is not about image generation. It is about **how an agent spends scarce or consequential authority**.

---

# 1. Preview and execution are separate capabilities

A safe system should make it possible to inspect an operation without performing it.

Examples:

```text
cost quote          → paid API call
change plan         → repository mutation
recipient preview   → send
resource estimate   → expensive batch
impact set          → deletion
release plan        → production deployment
```

### Principle candidate

> **Consequential actions should expose a side-effect-free preview whenever the effect can be bounded before execution.**

This helps the human make an informed decision and gives deterministic policy code something concrete to evaluate.

---

# 2. Approval should bind to an exact operation

A generic “yes” or `--confirm` flag is weaker than an authorization bound to:

- action type;
- target;
- parameters;
- count/scope;
- resource/cost ceiling;
- expiry / one-shot semantics.

If any approval-relevant parameter changes, the original authorization should not silently carry forward.

### Principle candidate

> **Approval is authority over a described effect, not a reusable mood or session state.**

Standing approval should be the exception and should itself have an explicit bounded policy.

---

# 3. Execution must bind price/authority-affecting parameters explicitly

A quote is meaningless if execution inherits a different provider default.

The same applies outside cost:

- a deployment approved for staging must not inherit production as a default;
- a deletion preview for 10 files must not execute against a wildcard that now matches 100;
- a notification approved for one channel must not inherit “all subscribers” from configuration.

### Principle candidate

> **Any parameter that affects authority, cost, blast radius or acceptance must be explicit in both preview and execution.**

This is a general anti-drift requirement between plan and action.

---

# 4. Policy should constrain the actor from outside its writable authority

A runtime with no `--override-budget` flag is safer than one that can raise its own limit with an option.

But a budget/policy file is not a hard boundary if the same agent can simply edit that file.

### Principle candidate

> **The actor being constrained should not be able to silently widen the policy that constrains it.**

For high-impact controls, widening authority should require a higher-trust path than ordinary execution.

This reinforces monotonic-policy thinking:

```text
lower-trust layer may tighten
lower-trust layer may not silently loosen
```

---

# 5. Hard ceilings require reservation, not check-then-act

The common pattern:

```text
read usage
check budget
perform effect
record usage
```

is unsafe under concurrency and failure.

Two workers can both pass the same check before either records the spend. A crash after the external side effect can also leave local accounting incomplete.

A stronger conceptual model is:

```text
atomic reservation
      ↓
execute
      ↓
settle actual consumption
      ↓
release/refund unused reservation
      ↓
reconcile discrepancies
```

### Principle candidate

> **A control advertised as a hard resource ceiling needs atomic authority reservation or an equivalent serialized owner.**

Otherwise it is a best-effort local gate, not a true ceiling.

---

# 6. Local accounting and external truth are different authorities

A locally calculated estimate can support planning, but it does not become authoritative billing merely because the tool wrote it to a ledger.

Distinguish:

```text
quoted_estimate
reserved_amount
provider_reported_actual
local_observed_actual
unknown/unreconciled
```

The same pattern applies to:

- token cost;
- cloud spend;
- API quota;
- deployment resources;
- compute runtime.

### Principle candidate

> **Derived resource estimates must not masquerade as source-owned actual consumption.**

Expose provenance and degraded/unknown states.

---

# 7. Corrupt/missing accounting evidence should fail visibly

Skipping malformed ledger records is convenient for analytics but dangerous for a safety ceiling. If a record that might represent consumed budget cannot be parsed, treating it as zero can widen authority accidentally.

### Principle candidate

> **For safety decisions, unparseable evidence should degrade to UNKNOWN/unsafe, not silently to zero.**

This is the same epistemic rule HE has found repeatedly in orchestration, telemetry and review evidence.

---

# 8. Artifact atomicity is not transaction atomicity

Writing an output via temporary file + rename prevents partial local artifacts.

It does not make this sequence atomic:

```text
external charge
→ download
→ file write
→ receipt
→ local ledger
```

A system can successfully spend money and then fail to record the spend.

### Principle candidate

> **Verify atomicity at the level of the real-world effect, not only the local file operation.**

When true distributed transactions are impossible, design explicit reconciliation and uncertain states.

---

# 9. Receipts are valuable when they preserve the right evidence

A useful receipt links:

- intended operation;
- approval reference;
- executed parameters;
- provider/tool identity;
- timestamps;
- result/artifact;
- actual or estimated resource usage;
- verification status.

But receipts should not automatically copy sensitive prompts/data merely because more logging feels safer.

### Principle candidate

> **Preserve enough provenance to audit an effect; minimize payload that does not improve verification.**

This aligns with HE's source-preservation rule: preserve the evidence trail, not necessarily the entire artifact.

---

# 10. Safety preflight should test refusal paths

A capability is not safely installed merely because the happy path works.

For critical guards, setup should test that the forbidden action is actually refused.

Examples:

```text
unapproved paid call → refused
public bind          → refused
write outside scope  → refused
unauthorized send    → refused
budget overrun       → refused
```

### Principle candidate

> **Preflight critical safety properties by proving refusal, not just successful operation.**

This is especially useful for hooks, permission profiles, unattended agents and external-action tools.

---

# 11. Routing policy should explain both capability fit and escalation cost

`studio-skill` routes work by requirement rather than by prestige/model size alone, and keeps higher-cost options as explicit escalation tiers.

Generalized:

```text
task requirement
    ↓
minimum capability tier that reliably clears requirement
    ↓
explicit escalation when additional quality/authority/cost is justified
```

### Principle candidate

> **Route to the least expensive/least powerful capability that reliably satisfies the acceptance requirement, while making escalation criteria explicit.**

This complements recent Jev, model-tiering and fan-out/fan-in research.

---

# Cross-project implications

## Harness Miner

Harness Miner could eventually observe:

- repeated approval prompts;
- repeated denied actions;
- estimates versus actuals where authoritative actuals exist;
- cap/refusal events;
- operations repeatedly escalated to a higher tier;
- safety controls that users routinely work around.

It should **not** become the policy owner or ledger owner merely because it analyzes those events.

## Dashboard / Cockpit

A future Cockpit may display:

- remaining bounded authority/budget;
- pending approval intents;
- executed effects and receipts;
- reconciliation/degraded states.

It should not own the budget itself.

## PMB / work MB

Memory should record durable policy/decisions only when MB is the correct owner. It should not become an operational spend ledger.

## ACR

If ACR ever gains automatic fixer/merge/deploy authority, these patterns become highly relevant:

```text
proposed fix
→ bounded preview/diff
→ explicit authorization
→ isolated mutation
→ tests/review
→ effect evidence
→ receipt/provenance
```

No such authority change is implied by this note.

---

## Disposition summary

- **REINFORCE:** side-effect-free preview before consequential action.
- **REINFORCE:** approval should bind to exact operation parameters and scope.
- **REINFORCE:** approval-sensitive parameters must be explicit at execution.
- **REINFORCE:** constrained actors should not silently widen their own policy.
- **REINFORCE:** provenance/receipt after consequential effects.
- **REINFORCE:** negative/refusal paths belong in safety preflight.
- **REINFORCE:** routing should use explicit capability/cost escalation criteria.
- **ASSESS:** atomic reservation + settlement/reconciliation for real hard caps.
- **ASSESS:** machine-readable approval objects for high-impact tools.
- **CAUTION:** local estimates/ledgers are not authoritative provider truth.
- **CAUTION:** check-then-act controls race under concurrency.
- **CAUTION:** post-effect logging can fail after the external effect has already occurred.
- **CAUTION:** ordinary writable policy files are not strong security boundaries.

## Status

Research synthesis only. No implementation changes authorized.
