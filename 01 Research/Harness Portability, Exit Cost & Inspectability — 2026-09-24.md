# Harness Portability, Exit Cost & Inspectability — 2026-09-24

## Purpose

Synthesize a durable Harness Engineering boundary from the 2026-09-24 review of Manolo Remiddi's harness-lock-in discussion and direct inspection of DeepSeek Harness.

Primary research note:

- `01 Research/Sources/Manolo Remiddi & DeepSeek Harness — Lock-In, Inspectability & Exit Cost — 2026-09-24.md`

Related HE research:

- `01 Research/Context Engineering.md.md`
- `01 Research/AI Engineering Observability & Dashboard Boundary — 2026-09-18.md`
- `01 Research/Session Rollover, Handoff & Verification — 2026-09-16.md`
- `01 Research/FreeLLMAPI — Gateway Boundaries & Runtime Routing — 2026-09-14.md`

This note does not require an open-source runtime, a model router, a dashboard, or replacement of Claude/Codex.

---

# 1. Portability is broader than model swapping

A harness can claim model flexibility while still imposing high migration cost through session state, instruction formats, plugins, memory layout, workflow semantics, or hidden runtime behavior.

Therefore evaluate portability across several dimensions:

```text
model/provider
runtime/inference
instructions + skills
project knowledge
session/continuation state
tool protocol
permissions/authority
plugin/API surface
deployment assumptions
verification/eval artifacts
```

A component is more replaceable when its owned state and contracts remain understandable without that component.

### Candidate principle

> **Portability should be evaluated by replacement cost, not merely by model compatibility.**

**Disposition: ASSESS / REINFORCE.**

---

# 1A. Model portability and harness portability are separate

The 2026-10-19 Ollama/Claude Code and HarnessTax pass reinforces that "the coding agent" is not one
indivisible product.

At minimum, distinguish:

```text
model
+ model/provider adapter
+ harness
+ execution/runtime configuration
```

A model may be usable through more than one harness, and the same harness may be able to drive more
than one model. That separation creates both an exit path and an experimental control.

Two useful portability tests are:

1. **Hold the model constant; swap the harness.**
   - measures harness contribution, cost, tool behavior, and dependency coupling.
2. **Hold the harness constant; swap the model.**
   - measures how much observed behavior belongs to the backbone versus surrounding machinery.

This does not require HE to adopt a universal router. In fact, automatic model/provider failover can
make attribution harder when reproducibility is the objective.

### Candidate principle

> **Own durable workflow/state/evidence above the model-harness pairing where practical; treat both model and harness as replaceable execution dependencies unless a measured requirement says otherwise.**

> **Portability is proven by a controlled swap, not by an abstraction layer existing on paper.**

---

# 1B. Canonical semantics plus generated host adapters can reduce workflow drift

Current upstream pstack provides a useful portability pattern.

The canonical skill/playbook source lives in the Cursor plugin tree, while the same repository generates a Claude Code representation under `harness/pstack/claude-code/`. Upstream documentation states that the generated tree is regenerated from the canonical source and CI checks that it has not drifted.

Conceptually:

```text
canonical workflow semantics
          ↓
host-specific adapter generation
          ↓
Claude Code / other harness representation
          ↓
drift check
```

This is stronger than maintaining unrelated copies by hand because semantic ownership remains explicit.

It does not prove behavioral equivalence across hosts. A host may differ in:

- tool semantics;
- subagent capability;
- hooks;
- permissions;
- model identifiers;
- invocation lifecycle.

Those differences belong in the adapter and should remain visible/testable.

### Candidate principle

> **When one workflow must span harnesses with different primitives, prefer one canonical semantic owner plus explicit adapters and drift checks over independently edited copies.**

**Disposition: REINFORCE portability mechanism; ASSESS behavioral equivalence per host.**

---


# 1C. Cross-agent skill portability needs a managed canonical source, not manual copies

The 2026-10-21 Rob Shocks / MoltenBase demonstration makes the operational form of this portability problem concrete.

MoltenBase was presented as a skill manager for reusing skills across several coding agents. The transferable HE mechanism is not the specific product. It is the need to prevent reusable capability definitions from becoming unrelated copies spread across host-specific folders.

A bounded capability manager should preserve:

- canonical source and version provenance;
- owner and review state;
- supported-agent / host metadata;
- project-local vs user/global scope;
- controlled deploy/sync;
- diff-before-update;
- refusal to silently overwrite unmanaged content;
- drift detection and rollback;
- explicit host adapters when semantics differ.

Conceptually:

```text
canonical skill / capability
          ↓
source + version + owner
          ↓
host-specific adapter / deployment
          ↓
Claude Code / Cursor / Codex / ...
          ↓
drift + behavior verification
```

The important boundary is that a capability registry should remain a deployment/provenance layer.

It should not become:

- project memory;
- task orchestration authority;
- a secret store;
- an autonomous self-modification loop;
- evidence that a capability behaves equivalently across hosts merely because the same text was copied.

### Candidate principle

> **Manage reusable agent capabilities like versioned engineering artifacts: one canonical owner, explicit host deployment, provenance, drift detection and behavioral verification where host semantics differ.**

**Disposition: ASSESS / PILOT where multiple coding agents share the same capability set.**

---


# 2. Exit cost should be explicit during dependency selection

Before making a harness/runtime/tool a durable dependency, ask:

1. What facts does it own?
2. In what format are they stored?
3. Can those facts be exported or reconstructed?
4. Which behavior exists only in proprietary configuration or conversation state?
5. What must be rewritten to replace the component?
6. Does leaving it require losing history, permissions, workflows, or verification evidence?
7. Is the replacement boundary documented and testable?

This is not an argument to avoid dependencies. It is a way to identify **irreversible coupling** before it becomes invisible.

### Candidate principle

> **Prefer reversible dependencies when equivalent choices exist; justify irreversible coupling with demonstrated value.**

**Disposition: ASSESS.**

---

# 3. Durable project truth lowers exit cost

Files, Git history, explicit task artifacts, portable test/eval fixtures, and documented decisions reduce dependence on any one session or model provider.

This reinforces HE's existing direction:

- historical conversation may inform;
- current project state governs;
- focused handoff carries only needed successor state;
- durable knowledge should be reachable outside transient model context.

This does **not** imply that every runtime event belongs in Git or Markdown.

Store only durable facts whose survival materially improves recovery, portability, reproducibility, or verification.

---

# 4. Inspectability is a portability aid

A harness is easier to debug or replace when users can determine what actually shaped execution.

Useful inspectable state may include:

- active model/provider/effort;
- loaded instruction sources;
- active skills/capabilities;
- available tools;
- authority/sandbox boundaries;
- context size and material compaction events;
- current lifecycle/run state;
- verification status;
- configuration/profile identity.

The requirement is not “show everything.” It is:

> **Expose enough effective state to explain a material behavioral difference or migration dependency.**

This remains compatible with HE's minimal-provenance rule and with the parked Dashboard/Cockpit boundary.

---

# 5. Composability is useful only when boundaries are real

DeepSeek Harness demonstrates a strong form of plugin composition: model adapter, tool registry, session log, agent loop, sandbox/policy, and telemetry are explicit replaceable seams, and plugin effects are designed to unwind on unload.

HE should mine the underlying mechanism, not copy the framework.

A capability boundary earns its complexity when it enables one or more of:

- independent ownership;
- independent evaluation;
- replacement without unrelated rewrites;
- isolation;
- deterministic enforcement;
- explicit lifecycle/unload behavior.

If the abstraction merely adds indirection without one of those benefits, it is architectural overhead.

### Candidate principle

> **Composability is valuable when it makes ownership, replacement, isolation, or enforcement cheaper than the coordination complexity it introduces.**

**Disposition: REINFORCE.**

---

# 6. PMB implication

PMB already reduces some harness lock-in by keeping project truth outside the conversational runtime.

Do not add a provider abstraction or export subsystem merely because portability is desirable.

During and after the pilot, observe whether any important state is trapped in:

- Claude-specific conversation behavior;
- provider-only memory;
- opaque handoff/compaction state;
- one client's instruction semantics;
- non-portable session metadata.

Only then propose the smallest correction in the component that owns the trapped state.

**Current PMB disposition: OBSERVE, no new implementation.**

---

# 7. Dashboard/Cockpit implication

If the parked Cockpit is revisited, inspectability should be an input requirement:

```text
source-owned runtime facts
        ↓
read-only effective-state view
        ↓
user diagnosis / decision
```

Do not let the Dashboard become the canonical owner of model choice, task truth, permissions, or project state simply because it displays them.

---

# Research dispositions

## REINFORCE

- Harness behavior is determined by the whole execution system, not model name alone.
- Durable truth should outlive transient conversation when recovery/portability matters.
- Minimal but discriminating runtime provenance is useful.
- Composability must justify its own complexity.

## ASSESS

- Exit cost for candidate runtime/harness dependencies.
- Which HE/PMB facts are currently portable vs trapped in provider/client state.
- Whether selected DeepSeek Harness lifecycle/seam mechanisms offer useful comparison evidence.

## PARK

- Universal model-agnostic runtime requirement.
- New provider router solely for portability.
- New Cockpit implementation.
- DeepSeek Harness adoption.

## REJECT

- Open-source license as proof of low lock-in.
- Model swapping as a complete portability test.
- Duplicating durable state into multiple systems “for portability.”

---

# Bottom line

Harness lock-in is best treated as an **ownership and replacement-cost problem**.

The practical HE posture is:

> **Keep important project truth portable, make material runtime state inspectable, define real capability boundaries where they reduce coupling, and measure the cost of replacing dependencies before allowing them to become invisible infrastructure.**
