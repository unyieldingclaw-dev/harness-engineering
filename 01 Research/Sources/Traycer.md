# Traycer

**Source:** WorldAI — Claude Code + Codex = AI GOD MODE! (Open source + Free)
**Video:** https://www.youtube.com/watch?v=oBuWXUy7stw
**Repository:** https://github.com/traycerai/traycer
**Website / download:** https://traycer.ai/download
**Disposition:** ASSESS
**Research thread:** Agent Harness Architecture / Multi-Agent Coordination

## Why it is interesting

Traycer is an open-source orchestration workspace intended to coordinate existing coding agents rather than replace them. The source video demonstrates Claude Code and Codex working in the same project with shared artifacts/context, explicit responsibilities, agent-to-agent communication, review handoffs, task history, and a visible orchestration workspace.

The repository describes Traycer as a "Nerve Center for Agentic Coding" with Bring Your Own Agent support, shared context across providers, agent-to-agent communication, regular and Epic modes, and collaboration features.

## Questions to investigate

1. What exactly is shared between agents: files, task state, artifacts, conversation context, history, or all of these?
2. How is agent-to-agent communication implemented? Is it structured messaging, files/events, MCP, or another mechanism?
3. What authority does the coordinator have versus the underlying coding harnesses?
4. How are conflicting edits handled when multiple agents work in the same project?
5. Does one-writer-per-artifact become necessary to avoid context churn or race conditions?
6. How does Traycer verify that one agent actually completed work before another agent proceeds?
7. Does a review agent provide independent evidence, or merely another model opinion?
8. Does shared context reduce duplicated context transmission, or introduce another context layer and associated token cost?
9. Which responsibilities belong in a coordination layer versus Claude Code, Codex, LiteLLM, or the project harness itself?
10. What parts of Traycer overlap with our existing harness direction, and what is genuinely different?

## Important observed issue

Traycer's public GitHub issue #1298 describes a multi-agent context-cost problem: peer artifact edits can trigger full-file re-injection into another agent's harness, increasing token cost. The proposed mitigations include artifact ownership, diff-based delivery, and distinguishing message delivery from message processing. This is particularly relevant to our existing context-cost and bounded-authority research.

## Current assessment

Do **not** adopt Traycer based on the video. Investigate the underlying coordination mechanisms and compare them with our existing architecture.

The strongest research value is not "Claude + Codex together." It is the architectural question of whether a small coordination layer can let independent agents perform bounded work, exchange useful state, and verify results without creating another expensive context-management layer.

## Related research

- Unlazy — completion verification / assertion vs evidence
- OpenViking — durable context and hierarchical retrieval
- SwitchYard — model routing and escalation
- 5-Persona Advisory Board — independent analytical perspectives
- Context Engineering — shared context, evidence, and orchestration
