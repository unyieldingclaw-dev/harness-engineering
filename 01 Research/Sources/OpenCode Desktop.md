# OpenCode Desktop

## Source

- OpenCode project: https://github.com/anomalyco/opencode
- Desktop: https://dev.opencode.ai/

## Why It Is In The Research Queue

Standalone desktop AI coding-agent workspace that may provide a Claude Desktop / Cursor Agent-view-like experience without requiring VS Code.

## Initial Disposition

**ASSESS**

Do not adopt or install solely from this research entry. First compare the desktop workflow and architecture against the current Claude Code workflow and Traycer.

## Questions To Investigate

- How does the desktop UI manage agent sessions and concurrent work?
- How are plan/read-only and implementation capabilities separated?
- How are diffs, approvals, tools, and permissions surfaced?
- How are multiple model providers supported?
- What project context is loaded automatically versus discovered on demand?
- How does the desktop application interact with the underlying OpenCode agent/runtime?
- What persistent state exists outside the conversation?
- Does the desktop layer add meaningful capability or primarily provide a better interface to an existing agent runtime?
- How does its context handling compare with Claude Code and Traycer?
- What are the operational costs and tradeoffs of adding another agent harness to the local workflow?

## Relevance To Our Work

This is primarily a **workflow/UI and agent-harness comparison**, not a recommendation to replace Claude Code.

The useful architectural question is whether a standalone agent workspace can provide the desired agent-oriented experience while preserving explicit project state, bounded authority, inspectable work, and independent verification.

The research should distinguish:

- UI convenience;
- agent runtime capability;
- model/provider routing;
- project context management;
- execution/permission controls;
- verification/review;
- durable project state.

A better UI alone is not sufficient justification for changing the underlying harness.

## Related Research

- Traycer — multi-agent workspace and coordination
- Unlazy — completion verification / assertion vs evidence
- OpenViking — hierarchical context and memory
- SwitchYard — model routing and escalation
- Matt Pocock — modular agent capabilities

## Status

Research candidate. No implementation decision.
