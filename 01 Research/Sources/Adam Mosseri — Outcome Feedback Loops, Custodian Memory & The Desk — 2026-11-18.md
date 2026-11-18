# Adam Mosseri — Outcome Feedback Loops, Custodian Memory & The Desk — 2026-11-18

**Disposition:** Preserve as HE **research / observation**, not a specification, implementation request, endorsement of cross-tool memory sharing, or proof of measured productivity. **Draft review only.** No modifications to HE runtime, Personal Memory Bank (PMB), AI Code Review Agent (ACR), the independent Home/Work AI Engineering Dashboard, or workplace systems.

## Source and provenance

- **Primary:** [Silicon Valley Girl / Marina Mogilko, “Head of Instagram Reveals How the Top 1% Actually Work With AI” — interview with Adam Mosseri](https://www.youtube.com/watch?v=CN8c1edTCew), published **November 17, 2026** (episode date independently corroborated by [PodSized](https://podsized.io/podcasts/silicon-valley-girl-ai-tech-and-career-growth/episodes/head-of-instagram-reveals-how-the-top-1-actually-work-with-a-421721e5-1da7-4f9f-9b21-8fd9c1b5a29e)).
- **Material inspected:** user-supplied complete approximately 44-minute, 401-line time-aligned transcript, uploaded November 18, 2026 as *Pasted text(20261118-064834).txt*, and user-supplied YouTube description/chapter timestamps. The raw copyrighted transcript is **not** copied into this repository.
- **Evidence quality:** attributed first-person descriptions of Mosseri's personal/workflow systems. The supplied text appears to be automatic transcription: noisy spelling, chapter boundaries and timestamp rendering, and a sponsor segment at about 10:40–11:50. Times below are **approximate video positions**, not audited runtime logs.
- **Not inspected:** Mosseri's app source, network requests, actual database, API and access configuration, memory schema, prompt revisions, model/evaluation logs, Meta's employee governance rules, or a quantitative productivity study. This is **not** a software/security audit. “Top 1%” is the video's editorial positioning, **not a measured ranking**.
- The HubSpot CRM segment is sponsored and does not establish how Mosseri's internal tools work. Claims about Muse, Duet, and competing providers are examples of **his usage**, not independent benchmarks.

## 1. What Mosseri actually described

| Interview segment | First-person/source-reported mechanism | What is **not** established |
| --- | --- | --- |
| [02:08–03:10](https://www.youtube.com/watch?v=CN8c1edTCew&t=128s) — **Roger** | Tracks commitments made to him through meetings, email and Slack, including overdue requests; shows items inside his larger task list and supports human follow-up | No demonstrated extraction precision, agreement validation, calendar/identity model, provenance UI, escalation policy, or automatic sending approval |
| [03:10–05:06](https://www.youtube.com/watch?v=CN8c1edTCew&t=190s) — **the Desk** | A custom web app displaying the highest-priority item plus the rest, with snooze/edit actions; includes Roger, another named component “Caddy”, meeting history and briefings. Built using an internal Meta harness/CLI and internal APIs; described as a **simple SQLite-backed web app** fed by overnight automations | No public code or configuration; “Caddy” is mentioned without a sufficient description to specify its behavior. A single app does not imply a universal architecture suitable for home, work or PMB |
| [05:33–07:59](https://www.youtube.com/watch?v=CN8c1edTCew&t=333s) — **email draft corrections** | An assistant prepares drafts; Mosseri reviews, edits and sends them himself. Later the assistant compares its draft with his sent message and uses the differences to refine future responses | No evidence that underlying model weights are trained. Likely application-level feedback/context, but exact mechanism unspecified. No quantified acceptance rate, learning speed, privacy implementation, or guaranteed sender approval |
| [08:09–09:50](https://www.youtube.com/watch?v=CN8c1edTCew&t=489s) — **meeting briefings** | Initial advance summaries were unhelpful. The system now compares predicted priorities against post-meeting action emails, AI notes, personal notes and Slack discussion; over 1–2 months he reports material improvement | No blinded scoring, recorded accuracy, holdout examples, baseline time studies, or safeguard against hindsight leakage and self-grading bias |
| [09:52–10:40; 11:57–14:09](https://www.youtube.com/watch?v=CN8c1edTCew&t=592s) — **small personal apps** | A children's podcast generator selects topic, listener age, length and friendly voice; sports answers use external current scores and time anchoring. He says a simple message can substitute for a UI for many needs | Does not establish a broadly reusable children's content platform, podcast quality, or that every agent requires a new app |
| [14:27–15:54](https://www.youtube.com/watch?v=CN8c1edTCew&t=867s) — **Custodian** | His **personal** shared memory/activity/skills service: text files indexed for semantic retrieval; other agent tools receive read or read/write access, send factual additions under guidelines, and are plugged in or removed as he experiments. He explicitly says **work uses a different system** | No source code, canonical fact schema, trust/ACL enforcement, attribution/conflict logic, aging/deletion procedure, encryption, prompt injection controls, cost/latency measurements, or portability test results |
| [16:00–17:15](https://www.youtube.com/watch?v=CN8c1edTCew&t=960s) — **decisions** | Uses AI to stress-test decisions while trying to delegate business decision authority transparently to qualified **people** closest to the work | Does not advocate autonomous AI decision delegation or human accountability replacement |
| [20:30–22:07](https://www.youtube.com/watch?v=CN8c1edTCew&t=1230s) — **AI productivity** | He explicitly rejects token use, diffs, lines of code, and experiment counts as goals; calls them possible indicators, with real value/outcome as the goal | No rigorous causal proof that his setup improves output, nor evidence for a universal productivity metric |
| [23:22–29:52](https://www.youtube.com/watch?v=CN8c1edTCew&t=1402s) — **proxies, tradeoffs, product focus** | Easier-to-measure short-term reactions may diverge from longer-term user value. He prefers fewer higher-quality projects, anchored in mission, relevant users and future importance | Does not specify quantitative investment weights, formal prioritization algorithm, or prove broad headcount/AI deployment economics |
| [30:15–34:58](https://www.youtube.com/watch?v=CN8c1edTCew&t=1815s) — **signals from feedback** | Uses qualitative interviews, comments, DMs, and assistant-generated surfacing of notable requests; emphasizes distinguishing signal from noise | No claim that volume or sentiment clustering automatically defines product priorities |

## 2. High-value HE interpretation (distinct from source facts)

### A. Outcome-reconciled feedback is better than “agent remembers the answer”

**Observed:** the original meeting briefing had low utility; comparing *advance priorities* with the *later actual meeting* gave Mosseri a useful way to improve it. The email workflow compares *initial AI output* against *human-approved final output*.

**HE interpretation:** the useful loop is **candidate output → independently observed human/operational result → discrepancy → proposed smallest owned correction → fresh verification**. The human-approved result is evidence, not an automatic instruction to generalize. This directly **reinforces**, rather than replaces, existing HE sources:

- [Experience-Derived Harness Evolution — 2026-09-25](https://github.com/unyieldingclaw-dev/harness-engineering/blob/main/01%20Research/Experience-Derived%20Harness%20Evolution%20%E2%80%94%202026-09-25.md): history/telemetry helps locate weaknesses; only evaluated evidence becomes a durable rule.
- [Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22](https://github.com/unyieldingclaw-dev/harness-engineering/blob/main/01%20Research/Artifact-Gated%20SDLC%2C%20Behavioral%20Evals%20%26%20Metrics%20%E2%80%94%202026-09-22.md): feedback is not equivalent to a verifier; acceptance belongs with artifact and owner.
- Existing HE investigation-integrity guidance: identify the incorrect owning component, propose the smallest durable fix, require reproducibility or clear evidence, rerun the original task in a fresh session, verify no routing regressions.

**Do not infer:** that every edit delta should be promoted to a global skill; that one person's email rewrite indicates a lasting preference; that autonomous self-grading is independent verification; or that a nightly learning process is justified. A necessary first test is whether the current manual review/notes process already solves the problem.

### B. The Desk is evidence **against** prematurely designing a giant dashboard

**Observed:** despite access to Meta internal services, Mosseri describes a simple web app + SQLite + narrow nighttime collection. He also states that most users can obtain similar utility from a much lighter proactive assistant and messages rather than a custom UI (about 05:30–06:22 and 09:52–10:40).

**HE interpretation:** choose the smallest decision interface only if a demonstrated information overload/retrieval need warrants one. For the independently scoped Home/Work AI Engineering Dashboard (existing [draft PR #28](https://github.com/unyieldingclaw-dev/harness-engineering/pull/28)), this is **context** for decision-oriented presentation, **not** evidence for ingesting corporate email, meetings or Slack, implementing Roger, building nightly jobs, introducing SQLite, or creating a shared memory service.

The dashboard should remain **independent of PMB, ACR and HE**, **read-only/local-first where authorized**, and Home/Work instances must stay separated. A generalized cross-provider “command center” is not automatically valuable just because a leader with different information density uses one.

### C. Custodian is a useful contrast to PMB, not a PMB replacement

**Observed:** one personal memory/service exposes reusable factual/skill context to multiple AI tools; he describes text files, semantic search, read/write access, and a **separate work system**.

**HE interpretation:** compare that approach with PMB's current purpose: durable project context, provenance, source ownership, scoped instructions, security boundaries and cold starts. Multi-provider access does not resolve canonical-owner, contradictory claim, provenance, version, consent or revocation questions.

The interview **does not supply enough design evidence** to justify:
- a central writable universal memory store;
- automatic cross-provider write-through and conflict reconciliation;
- text embeddings or a vector database just for portability;
- exchanging Home and Work memories; or
- making a dashboard the owner of project memory.

**Potentially borrow:** explicit user-approved read/read-write scopes; clear separation between canonical stored facts and provider-specific retrieval; ability to unplug a provider without rewriting the canonical source. All need concrete requirements before implementation.

### D. Roger exposes a separate high-risk category: claims about people

**Observed:** Roger infers promised work from communications and helps prepare nudges.

**HE interpretation:** tracking a commitment made by a colleague is not the same as extracting an unambiguous task from a ticket. False positives, missing context, private conversations, consent, retention and social consequences matter. The safest candidate behavior is **suggested, source-linked reminders reviewed by a human**. No automatic reminders, escalation, employee scoring, or surveillance is supported by this interview. Corporate approval and available data permissions are prerequisites; not an HE/PMB feature.

### E. Outcome quality and human judgment beat activity counters

**Observed:** Mosseri treats tokens/lines/diffs as indicators, not targets, and worries that high throughput encourages too many lower-quality projects.

**HE interpretation:** a model router, agent telemetry product, or HE “velocity” metric is useful only insofar as it changes a decision or predicts an operational outcome. Favor acceptance, defect escape/rework, resolution time and repeatability over token throughput. Evaluation requires task/outcome provenance and tradeoffs, not a universal productivity score.

## 3. Candidate applications — decision table

| Candidate | Observed need in user's current work? | Smallest credible next action | Decision |
| --- | --- | --- | --- |
| **HE feedback loop for agent failure** | Existing HE research and postmortem prompt already address this | Cross-reference as independent qualitative corroboration; **do not** add a second loop/policy | **Retain / reinforce** |
| **PMB cross-provider Custodian-style memory** | Portability is a known interest, but no newly documented failure requires universal sync | Compare source authority and write permissions only when a real provider handoff fails | **Research only / no PMB change** |
| **Dashboard ingestion of meetings/email/Slack** | No demonstrated approved Home/Work data/access or demonstrated need | Keep separate dashboard requirements and no corporate-data ingestion assumption | **Do not scope in** |
| **User-specific draft improvement from edits** | Plausible general-purpose productivity use, **not established as a repeated pain point** | Optional personal, manual, privacy-reviewed comparison of initial draft against final sent version; no background data collection | **Optional micro-experiment** |
| **Pre/post briefing assessment** | Applicable as an evaluation pattern, but no evidence that a new service is required | Optional single bounded example with a known meeting summary and human-confirmed outcome; manually score relevance/misses | **Optional micro-experiment** |
| **Roger-like automated commitments monitor** | No verified workplace permissions, precision or operational need | No action beyond documenting sensitivity and manual review requirements | **Do not build from this evidence** |
| **Multimodel orchestration and a central agents UI** | Video provides no comparable model cost/benefit tests | Leave current provider choice and independent projects alone | **No implementation** |

## 4. If a very small experiment is warranted

1. **Use personal/non-sensitive material only.** Pick **one** type of brief or draft where the current process repeatedly wastes time. Do not connect corporate mail, meetings, Slack/Teams, credentials or private third-party records.
2. Save the **initial** result, source inputs, task goal and timestamp. Decide **before** seeing the outcome what “helpful” means (for example: whether the three priority items matched the human assessment, omissions, edit time and accept/reject).
3. Record the human-approved final artifact or actual outcome. Create a short **difference report**, classifying substantive omissions/errors versus style preferences; don't turn every stylistic change into a permanent instruction.
4. If a recurring and attributable failure is visible, modify **only the owning prompt, retrieval source or process**; preserve source evidence. Try again on a **fresh, different example** and check that the change helps without harming other cases.
5. Compare time saved and decision quality with the existing manual approach. If no benefit, **stop**. Avoid a database, scheduler, agent registry or orchestration framework unless real work justifies the complexity.

This is an **evaluation idea**, not an authorization to deploy anything.

## 5. Limitations, tensions and negative evidence

- The interview is a self-report of workflows, not an empirical benchmark or a source inspection.
- “Learn from the delta” is a colloquial description of behavior; it does **not prove** model retraining or that a particular memory/indexing design is responsible.
- Nightly automations can silently codify wrong assumptions and spread them among multiple assistants; correctness of feedback signals is the actual bottleneck.
- Combining task lists, email, Slack, meetings and agent memory increases sensitivity and dependency on organizational approvals. The interview discloses **Meta's internal CLI and APIs** but not a reproducible integration design.
- Personal/work separation is explicit and meaningful, not incidental.
- Human gate on emails is an observed control. For meetings, the comparison is an asserted self-assessment without independently demonstrated scoring.
- The app's SQLite choice may be sensible for his single-user interface but is not an argument for database adoption elsewhere.
- The source never establishes a 1%-level performance baseline or cost savings from this stack.
- Choosing **not to build** is itself a prominent theme in the interview (about 28:31–29:52).

## HE disposition

**Preserve and link, do not build.** Highest-confidence contribution: outcome-based feedback and deliberate attention to proxy-metric failure, consistent with existing HE research. Second: a concrete example of **separate personal and work memory** and intentionally lightweight interfaces. Research should inform PMB and Dashboard comparison **without creating new requirements** for them.

**No follow-up automatically authorized.** No edits to policy, skills, agents, automations, model routing, data-access permissions, PMB, ACR, Home/Work dashboards, Meta services or workplace integrations.
