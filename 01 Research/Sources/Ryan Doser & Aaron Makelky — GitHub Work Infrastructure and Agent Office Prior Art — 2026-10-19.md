# Ryan Doser & Aaron Makelky — GitHub as AI Work Infrastructure & Agent Office Prior Art — 2026-10-19

## Source set

User-supplied videos/transcripts:

- Ryan Doser and Aaron Makelky — **GitHub for Non-Coders: How to Actually Use It**
  - https://www.youtube.com/watch?v=SxjvrqgL1DU
- Ryan Doser and Aaron Makelky — **Stop Using Lovable! The Alternative You Already Pay For**
  - https://www.youtube.com/watch?v=sRBn2YNd_nk

Primary repository inspected directly:

- dkyazzentwatwa/agent-office
  - https://github.com/dkyazzentwatwa/agent-office
  - inspected at main commit `07c25747d68c08949bd91f3efcb4f38bd1890f2f`

Supplemental first-party GitHub documentation:

- Custom agents:
  - https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-custom-agents
- Copilot customization surfaces:
  - https://docs.github.com/en/copilot/reference/customization-cheat-sheet
- GitHub Projects layouts:
  - https://docs.github.com/en/issues/planning-and-tracking-with-projects/customizing-views-in-your-project/changing-the-layout-of-a-view
- Repository rulesets / branch protection:
  - https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/creating-rulesets-for-a-repository

## Evidence independence and scope

Both supplied videos are attributed to the same presenters and share an advocacy perspective. Treat them as **dependent practitioner evidence**, not two independent demonstrations that repository-first workflows outperform alternatives. The `agent-office` repository is independent implementation prior art, but its README/role roster does not establish operational efficacy, measured productivity, actual autonomous utilization, or permission safety.

All outside repositories are **untrusted research inputs**: do not run setup commands, install prompts/agents or copy rules into HE without a separate supply-chain and policy assessment.

## Why this source set matters to HE

The videos are not primarily about Harness Engineering, but they surface two questions that are directly relevant:

1. What should GitHub own in an AI-assisted engineering workflow?
2. When does a role-specific agent provide enough value to justify another context, router, handoff, or maintenance surface?

The source material supports a useful distinction:

> **GitHub can act as durable, inspectable work infrastructure across AI clients, while agent roles should be admitted only when specialization or independence buys measurable value.**

This does not imply that a repository should become a dump of all chat history or that a large role catalog should become an architecture.

---

# 1. GitHub as durable work infrastructure

The first video frames GitHub as a portable home for code, Markdown, reusable playbooks, AI configuration, and project history. The useful part is not the "Google Drive on steroids" analogy. The useful part is the ownership boundary.

Source-derived claims include:

- repositories preserve work independently of a particular AI assistant;
- version history supports rollback and recovery;
- repository-backed playbooks can be reused across clients or projects;
- AI tools can consume repository content more naturally than ad-hoc cloud folders in many coding workflows;
- backups can preserve local AI configuration and allow a new machine or client to resume from durable files;
- public repositories should be treated as untrusted input rather than copied blindly.

The source also acknowledges a failure mode that matters to HE: storing ever-growing daily logs and assistant context can create context rot instead of useful memory.

### HE interpretation

> **Versioned durable state is valuable; indiscriminate accumulated context is not.**

GitHub should own artifacts that need one or more of:

- provenance;
- review;
- diffability;
- rollback;
- portability;
- collaboration;
- stable identity across tools.

It should not become the automatic owner of every transient observation, transcript, tool output, or model thought.

---

# 2. Repository ownership reduces AI vendor exit cost

The second video makes the same point through website tooling: keeping the implementation in a repository makes the AI coding client replaceable.

This generalizes beyond websites.

A useful HE portability model is:

```text
durable repository state
        ↑        ↓
Claude / Codex / other client
        ↓
runtime / deployment / tools
```

Changing the client should not require reconstructing project truth from chat history.

This reinforces existing HE work on harness portability, exit cost, inspectability, source preservation, and durable handoff.

### Boundary

The videos contain broad marketing claims about specific website builders, SEO performance, and products. Those claims are not required for the HE conclusion and are not promoted here as verified evidence.

---

# 3. Public GitHub repositories are capability-supply inputs

The first video explicitly warns that public repositories may contain malicious instructions or code and recommends using them as inspiration rather than blindly adopting them.

This is consistent with HE's existing supply-chain work.

A repository can supply:

- executable code;
- prompt/instruction text;
- skills;
- hooks;
- workflow definitions;
- MCP/connectors;
- CI configuration;
- agent profiles.

Those surfaces may carry authority far beyond ordinary reference documentation.

### Transferable principle

> **Treat external repositories as untrusted capability supply until the exact mechanism, authority, and execution path are understood.**

"Fork it" is not a security review.

---

# 4. Agent Office: what it actually is

At the inspected commit, Agent Office contains:

- 44 role-specific Markdown "employee" prompts;
- an `AGENTS.md` repository guidance file;
- a small `OFFICE_OPS/` control-plane directory;
- a roster;
- a coordination matrix;
- daily-routine SOPs;
- local operational logs;
- reusable skill files;
- a single-file dashboard.

The repository describes each role using a repeatable shape:

```text
role / purpose
tone / style
core capabilities
response rules
output formats
coordination relationships
escalation triggers
```

The Operations Manager is the routing default for broad or cross-functional work.

Its router skill maps request classes to role owners and suggests common cross-role pairings.

This is materially different from 44 independent autonomous processes. Much of the system is a library of role contracts plus routing instructions.

---

# 5. What Agent Office gets right

## 5.1 Explicit ownership

Roles name responsibilities and escalation boundaries.

This can reduce the failure mode where one general-purpose agent implicitly owns research, architecture, mutation, review, and publication at the same time.

## 5.2 Bounded output contracts

Role files provide expected output formats rather than only persona language.

That is more useful than cosmetic roleplay because output contracts can be evaluated.

## 5.3 Escalation conditions

Roles describe when a decision should leave the specialist.

This is consistent with HE's authority model: delegation does not transfer acceptance authority.

## 5.4 Unified fan-in

The router tells multi-role work to produce one unified response rather than disconnected persona transcripts.

That matches HE's preference for evidence/result fan-in instead of transcript fan-in.

## 5.5 Small shared control plane in concept

Agent Office explicitly says the operations hub should stay small and should exist only for routing, delegation, logging, and review.

That design intent is directionally good even though the concrete repository still carries substantial duplication.

---

# 6. What HE should not copy

## 6.1 Forty-four roles

The repository does not provide evidence that 44 roles outperform a smaller set or a strong general model.

Each additional role creates:

- another instruction surface;
- another routing boundary;
- another maintenance owner;
- another potential overlap;
- another source of context/tool cost.

HE should not treat role count as capability.

## 6.2 Static "utilization" as if it were observed runtime state

The roster contains utilization percentages and health thresholds, but the inspected repository does not demonstrate a runtime measurement system producing those values.

For HE, synthetic capacity should not be presented as operational fact.

## 6.3 Duplicated routing truth

Routing knowledge appears in multiple places:

- router skill;
- roster;
- coordination matrix;
- operations guide;
- daily SOP;
- individual role files.

That can become a drift problem.

### HE implication

> **One owner should define a routing boundary; other surfaces should reference or derive from it rather than restate it.**

## 6.4 Scheduled ceremony

The daily SOP includes repeated triage, logs, capacity checks, and review cadence.

Some of those may be useful for a real office workflow. They should not be imported into HE merely because a multi-agent system appears more complete with them.

## 6.5 Persona as false independence

Two role prompts using the same model, same context, and same evidence path are not automatically independent reviewers.

Independence must be designed through evidence separation, task isolation, model/runtime separation where material, or at minimum fresh-session re-derivation.

---

# 7. GitHub custom agents are a host capability, not HE architecture

GitHub currently supports repository-level custom agent profiles under `.github/agents/` and distinguishes always-on instructions, reusable prompts, and custom agents.

That is relevant as an execution host, but HE should not bind its role architecture to GitHub Copilot.

A role contract should remain conceptually portable across:

- Claude Code;
- Codex;
- GitHub Copilot;
- other compatible agent runtimes.

Host-specific files may be adapters.

### Transferable principle

> **Define the role contract before the host adapter.**

---

# 8. Candidate HE Specialist Bench

The strongest Agent Office idea worth testing is not an office. It is a small set of specialists with explicit authority boundaries.

Candidate bench:

## Research Scout

Purpose:
- discover candidate sources;
- establish source identity and provenance;
- find primary evidence;
- return a bounded source pack.

Authority:
- read/search only;
- no HE mutation;
- no architectural decision.

## Evidence Miner

Purpose:
- inspect approved sources deeply;
- separate source claims from inference;
- identify mechanisms, boundaries, and relationships to existing HE work.

Authority:
- may draft findings;
- does not promote findings to decisions.

## Opposition Reviewer

Purpose:
- independently attempt to disprove or weaken proposed findings;
- identify unsupported claims, duplicate evidence, hidden assumptions, and governance creep.

Authority:
- read-only;
- cannot repair the artifact it is reviewing in the same pass;
- does not inherit acceptance authority.

## Documentation Integrator

Purpose:
- integrate accepted findings into the correct existing owner;
- prefer updating an existing canonical artifact over creating a new one;
- preserve provenance and disposition.

Authority:
- may propose branch/PR changes;
- cannot merge or approve its own work.

### Important difference from Agent Office

There is no permanent AI "manager" in the initial design.

The human/main session remains the coordinator until evidence shows that automated routing itself purchases enough value to justify another component.

---

# 9. Specialist admission rule

A new specialist should exist only if at least one of these is demonstrated:

1. **Specialization:** task-specific instructions materially improve output.
2. **Isolation:** separating context materially reduces contamination or context cost.
3. **Independence:** a separate review path catches failures the producer misses.
4. **Authority:** a separate role prevents a component from both proposing and accepting the same consequence.
5. **Parallel information value:** independent work shortens the critical path or increases useful evidence.

A role should not be created merely because a job title is imaginable.

---

# 10. Relationship to existing HE research

## MERGES WITH / SHARPENS

- **Adaptive Autonomy, Diagnostic Gates & Fan-Out/Fan-In**
  - reinforces fan-out only for independent information value;
  - reinforces skepticism toward large agent operating-system bundles.
- **Skill Routing, Gate-Bound Evidence & Orthogonal Review**
  - sharpens the difference between routing metadata and actual independent verification.
- **Role-Based Model Allocation & Software Factory Boundaries**
  - role specialization is useful only when execution boundaries and contribution can be measured.
- **Turing Post — Harness Minimalism, Long-Horizon State & Control Planes**
  - supports smallest-credible-baseline evaluation;
  - rejects multi-agent orchestration by default.
- **Harness Portability, Exit Cost & Inspectability**
  - GitHub/repository ownership reduces client-specific exit cost.
- **Source Preservation & Evidence Durability**
  - repository-backed durable evidence is preferable to link-only or chat-only memory.

## DOES NOT AUTHORIZE

- Agent Office adoption;
- 44-agent role inventory;
- a new orchestration framework;
- a persistent HE agent control plane;
- GitHub Copilot as a required HE dependency;
- automatic routing;
- automatic publication;
- storing full chat history as repository memory.

---

# 11. Disposition

## MINE

- explicit specialist responsibility;
- output contract;
- coordination boundary;
- escalation trigger;
- unified fan-in;
- repository-owned portable state;
- external-repository supply-chain caution.

## ASSESS

- a small HE Specialist Bench;
- repository-level agent adapters after role contracts prove useful;
- a cross-repository GitHub Project for work visibility;
- short repository guidance where repeated errors justify it.

## PARK

- automatic coordinator/router;
- synthetic team capacity;
- autonomous daily office routines;
- persistent background agent office;
- dashboard for role utilization.

## REJECT

- role proliferation as capability;
- persona-based "independence";
- duplicated routing truth;
- automatically promoting accumulated assistant logs into durable memory;
- copying external agent systems wholesale.

## Bottom line

The transferable lesson is:

> **Use GitHub as durable, reviewable work infrastructure and use specialist agents only where a role boundary buys measurable specialization, isolation, independence, authority separation, or parallel information value.**

Agent Office is useful prior art for role contracts. It is not a target HE architecture.
