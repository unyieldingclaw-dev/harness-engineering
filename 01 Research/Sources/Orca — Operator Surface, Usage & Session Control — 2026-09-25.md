# Orca — Operator Surface, Usage & Session Control — 2026-09-25

## Purpose

Follow up the earlier Orca research note with the operator-interface findings surfaced by **The Next New Thing — Top Repos, Fame, Traffic & Agents** and a fresh pass over the current Orca source.

This note does **not** replace:

- `01 Research/Sources/Orca — Orchestration, Handoff & Verified Computer Use — 2026-09-21.md`

That note remains the evidence record for orchestration, focused handoff, runtime-versioned Skills, effect verification and automation semantics. This follow-up records the separate question:

> What does Orca teach HE about the operator/control surface around heterogeneous coding agents, and how should that affect the parked Dashboard / AI Engineering Cockpit idea?

## Source identity

- Repository: https://github.com/stablyai/orca
- Inspected revision: `ff74506c0bde1530fc079a6cf5b592a7a3127860`
- Review date: 2026-09-25 local / 2026-09-26 UTC
- Relevant docs/source:
  - `docs/site/content/docs/agents/claude-code.mdx`
  - `docs/site/content/docs/agents/supported.mdx`
  - `src/preload/index.ts`
  - `src/renderer/src/store/types.ts`
  - `src/cli/specs/core.ts`
  - `src/cli/specs/orchestration-worker-specs.ts`
  - Claude statusline/rate-limit tests

No Orca code is vendored into HE.

---

## Executive finding

Orca is more than a multi-pane terminal wrapper. Its useful HE value is that it treats coding-agent operation as a **control-surface problem** with provider-specific adapters underneath a shared operator model.

The product can launch essentially any CLI agent while providing deeper integrations where the host exposes better signals. Claude Code, for example, can be launched in a selected worktree with Orca-managed status hooks, usage/rate-limit visibility, account switching, hooks/memory visibility and child rows for background subagents / Agent Teams.

For HE, the important pattern is:

```text
provider/runtime-specific facts
          ↓
thin acquisition adapters
          ↓
shared operator concepts
  session / worktree / status
  usage / limits / children
          ↓
operator UI
```

This is strong prior art for the parked Cockpit idea and a reason **not** to build common runtime plumbing from scratch before evaluating Orca as a baseline.

---

# 1. Worktree is a first-class session boundary

Orca launches Claude Code with the selected worktree as its working directory and associates the resulting session with that worktree.

That makes the worktree more than a Git convenience. It becomes a useful join key for:

- filesystem isolation;
- branch/SHA provenance;
- session identity;
- operator navigation;
- parallel work ownership.

### HE translation

A cross-agent cockpit should prefer stable repository/worktree identity over chat titles or model-generated task labels.

**REINFORCE:** concurrency needs isolation + provenance, not merely more agent tabs.

---

# 2. Provider usage belongs above the model conversation

Orca reads host/provider usage state and surfaces rate-limit proximity instead of asking the model to estimate its own remaining capacity.

Current source contains separate bridges/slices for multiple providers, including Claude and Codex.

### HE translation

Provider capacity is **source-owned operational state**.

A cockpit should obtain:

- quota/rate-limit windows;
- reset times;
- provider/account identity;
- host-reported usage state;

from the provider/host adapter when available.

Do not make PMB, the agent, or a transcript parser recreate a fact the host already exposes.

**REINFORCE:** structured source data outranks model inference.

---

# 3. Subagents should be visible as children of owned work

For supported Claude sessions, Orca can surface background subagents and Agent Teams teammates as child rows beneath the lead session.

This is a better mental model than presenting every spawned process as an unrelated top-level session.

### HE translation

If HE later needs operator visibility into fan-out work, retain:

- parent/lead identity;
- child attempt identity;
- provider/model;
- lifecycle state;
- worktree/ownership relationship.

This complements HE's recent fan-out/fan-in work: **fan-out should preserve identity through fan-in and through operator observability.**

**REINFORCE.**

---

# 4. Status should come from runtime signals, not prose

Orca injects/uses host hooks and status-line signals to derive session state rather than trusting a model statement such as “I am still working.”

Claude statusline payloads also expose fields such as context-window usage, even when a particular Orca parser is currently focused on rate-limit information.

### HE translation

A future cockpit should distinguish:

- facts the host reports;
- facts Orca/another adapter derives deterministically;
- inferred attention state;
- model-generated narrative.

The existence of a field in a host payload does **not** prove the operator UI currently exposes it. Verify the rendered feature separately.

**REINFORCE:** acquisition capability and product presentation are separate layers.

---

# 5. Orca is a strong baseline for the parked Dashboard/Cockpit

Orca already covers a large portion of the generic plumbing a custom Cockpit would otherwise need:

- multiple agent runtimes;
- worktrees;
- session navigation;
- runtime status;
- provider usage / limits;
- subagent visibility;
- notifications;
- provider-specific adapters.

Therefore HE should change the Dashboard question from:

> “Can we build a useful AI engineering dashboard?”

into:

> “What repeated operational questions remain unanswered after using an existing operator surface such as Orca?”

Potential HE/PMB/ACR-specific gaps that could still justify a separate tool include:

- PMB health / active-context / handoff state;
- explicit context-occupancy visualization across hosts;
- ACR run/calibration state;
- cross-project evidence/history;
- grounded “since you left” deltas;
- provenance/authority classification across multiple source systems.

### Disposition

**ASSESS DEEPLY / TRY LOCALLY before starting a custom Dashboard repository.**

No installation is authorized by this research note.

---

# 6. Safety warning: Orca's launch defaults are not HE's desired default

Current Orca documentation states that new supported-agent launches prefill each CLI's permission-bypass mode unless the user changes the global Agent Permissions setting or customizes the agent.

Examples include:

- Claude `--dangerously-skip-permissions`;
- Codex `--dangerously-bypass-approvals-and-sandbox`;
- equivalent YOLO/bypass flags for other supported agents.

Orca's own documentation correctly warns that a worktree is an isolated checkout, **not a security sandbox**.

### HE translation

Do not conflate:

```text
filesystem isolation
```

with:

```text
security containment / bounded authority
```

If Orca is evaluated locally, **Manual permissions should be the baseline** unless a specific task and containment boundary justify otherwise.

**REJECT:** broad permission bypass as HE's default runtime policy.

---

# 7. Cross-provider support should preserve provider-specific semantics

Orca works with many CLI agents but has deeper hooks/status/usage support for only some of them.

That is the correct architectural shape.

### HE translation

A shared cockpit schema should normalize only genuinely shared concepts. Do not flatten provider-specific semantics merely to make every adapter look identical.

For example:

```text
session_id
repo/worktree
provider/model
lifecycle state
last activity
```

may be common, while:

```text
quota windows
context fields
subagent hierarchy
compaction events
```

may remain host-specific or optional.

**REINFORCE:** common interface, non-lossy adapters.

---

## Overall disposition

- **REINFORCE:** worktree as first-class runtime/session identity.
- **REINFORCE:** provider-owned usage/limit facts should be collected above the model conversation.
- **REINFORCE:** child-agent relationships should remain visible and attributable.
- **REINFORCE:** host/runtime signals outrank prose for live state.
- **REINFORCE:** shared operator concepts should sit over provider-specific adapters.
- **ASSESS DEEPLY:** Orca as an immediately usable operator surface and competitive baseline for the parked Cockpit.
- **PARK:** custom Dashboard implementation until repeated HE/PMB/ACR-specific gaps remain after evaluating existing surfaces.
- **REJECT:** permission-bypass / YOLO launch as a default HE safety posture.

## Short HE principle set from the follow-up

1. **Observe runtime state from the runtime that owns it.**
2. **Use worktree/repository identity as a stable anchor for concurrent coding sessions.**
3. **Preserve child-work identity instead of flattening fan-out into anonymous activity.**
4. **Normalize common operator concepts without erasing provider-specific semantics.**
5. **Evaluate existing operator surfaces before building another one.**
