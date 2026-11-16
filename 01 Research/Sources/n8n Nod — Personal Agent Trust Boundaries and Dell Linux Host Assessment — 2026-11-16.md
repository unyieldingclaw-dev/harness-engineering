# n8n Nod — Personal Agent Trust Boundaries and Dell Linux Host Assessment — 2026-11-16

**Status:** ASSESS / research preserved. **No installation, purchasing decision, agent integration, PMB/ACR/HE runtime change, or Dashboard scope expansion authorized.** All personal server experiments require a separate go decision.

**User-supplied evidence (2026-11-16):** [Chase AI video, “n8n Just Released a FREE + Open Source Version of Claude Duet, Grokbot, Dots, & Muse”](https://www.youtube.com/watch?v=ynEEbqKQSaY), ~12-minute transcript and 19 screenshots. Video claims were treated as discovery; the author's demonstration was **not** an independent security audit or a successful deployment on the user's hardware. Additional primary evidence: Nod upstream documentation and README, checked on 2026-11-16 (URLs below). Nod is a research preview; pin a source commit and revalidate details for any later adoption test.

## Executive finding

Nod is a separate, Apache-2.0 open-source personal agent from the n8n team—not the n8n workflow engine. It combines a Node.js server, persisted thread/task/schedule state, Docker-contained workspace and browser, PostgreSQL, model-provider choice, a server-side action checker, Vault credential filling, and an audit/Usage surface. Self-hosting on Linux is documented, but the reported **Dell Inspiron N5010 with ~6 GB installed RAM is below the vendor's 8 GB starting point**. It has not been installed, benchmarked, or proven stable on this hardware. The laptop is already generally kept powered on; availability is not the observed blocker. RAM/CPU capacity and maintenance are.

**Potential value:** study narrow action authority, permission enforcement outside the planner's prompt, idempotency/ambiguous outcomes, untrusted-content boundaries, source-specific memory evidence and trace transparency. **Do not copy mechanisms into HE/PMB/ACR because they exist in another product.** The useful transfer is a question/evaluation pattern, not a new governance framework.

## Source ledger / confidence

| Claim or mechanism | Verified upstream surface and limitation |
| --- | --- |
| Product, license, architecture | [README](https://github.com/n8n-io/nod) and [architecture](https://nod.n8n.io/docs/how-it-works/architecture/): one Node/Fastify/React server with Pi agent runtime and PostgreSQL + workspace/browser containers; source available under Apache 2.0. Documentation and checked-in source do not establish mature reliability. |
| Self-host resource starting point | [Self-host guide](https://nod.n8n.io/docs/deploy/self-host/): Linux AMD64/ARM64, 2 CPU cores, **8 GB RAM, 50 GB disk**, Docker Engine 25+, Compose v2. These are **starting guidelines**, not evidence 6 GB will definitely fail or succeed. One-person server; server access token grants broad access. |
| Server-side authority | [Trust principles](https://nod.n8n.io/docs/trust/principles/) and [approvals](https://nod.n8n.io/docs/trust/approvals/): separate action checker sees user words, rules and proposed action, not external web/email contents. Reads/drafts can be permitted by default. Approved action/arguments are bound to that specific use. **Checker is model-based and can be wrong; matching permission is not validating factual correctness or recipient identity.** |
| Execute once / ambiguous outcome | [Trust principles](https://nod.n8n.io/docs/trust/principles/) and [security model](https://nod.n8n.io/docs/trust/security-model/): stored claim/receipt to avoid duplicate execution; no automatic retry of uncertain external effects. A known unresolved outcome must be verified at the destination before reissuing. Test the failure semantics rather than assuming exactly-once delivery across all third parties. |
| Vault/security | [Trust/Vault](https://nod.n8n.io/docs/trust/) and [security model](https://nod.n8n.io/docs/trust/security-model/): model does not directly receive stored passwords; server and browser provider can handle secret values. Isolation cannot prevent an **authorized but harmful** external action or compromise of the host. |
| Memory | [Memory implementation description](https://nod.n8n.io/docs/how-it-works/memory/): a private, bounded memory document in DB, evidence pointers/quotes from owner messages, `memory_find` keyword retrieval, archived replacements, background upkeep; excludes web/email/model text as authority for owner memories. This is a **concept to inspect**, not evidence PMB needs an AI memory daemon. |
| Provider and model boundary | [Local models](https://nod.n8n.io/docs/connect/local-models/): tool calls and **64k context** are needed; system/tool material uses ~20k tokens before chat; model and checker may use different connections. Local execution **does not imply no external transfer** if a hosted model or remote checker is chosen. |
| Data flows | [Privacy](https://nod.n8n.io/docs/trust/privacy/): model provider receives task content; Composio and an n8n-hosted connection service can process app calls. Composio ZDR has carve-outs and still stores tokens/metadata. Usage analytics **on by default**; conversation traces **off by default**. Turn off sharing **before** connecting sensitive material. |
| Costs and benchmarks | [README](https://github.com/n8n-io/nod): free source is distinct from paid model calls, third-party services and electricity. Vendor reports **98% success over 226 simulated tasks x3=678 runs, zero unsafe actions in that test**, not independent production performance. [Releases](https://github.com/n8n-io/nod/releases) show an actively changing preview, including a November 16 release. |
| Restore / operations | [Server operations](https://nod.n8n.io/docs/deploy/server-operations/): PostgreSQL + workspace/browser volumes, server config and the Vault encryption key must be backed up together; backups contain secrets and need encryption. Avoid `docker compose down --volumes`. Migration rollback requires a compatible backed-up revision. |

**Evidence classification:** (a) user-provided video/screenshots = illustrated feature claims; (b) upstream README/docs = documented contracts/design and vendor test reports; (c) local laptop output = directly observed inventory; (d) any projected Nod throughput/security/operational benefit on Dell = **unverified hypothesis**. No independent source-code security audit, installation, throughput benchmark or end-to-end integration test has been performed here.

## Dell Inspiron N5010 — verified local inventory, not a deployment result

The user supplied command output (2026-11-16; identifying machine/boot IDs intentionally omitted):

| Field | Observed |
| --- | --- |
| Distribution/kernel | Pop!_OS 24.04 LTS, Linux 7.2.15-76070215-generic, x86-64 |
| CPU | Intel Core i5 M 460 @ 2.53 GHz; **2 physical cores / 4 threads** (4 logical CPUs reported) |
| RAM | **5.6 GiB OS-visible (~6 GB installed)**; 2.6 GiB used, 3.0 GiB available at observation; 9.6 GiB swap reported (including zram) |
| GPU | AMD/ATI Mobility Radeon HD 4650/5165; not a realistic modern local-LLM accelerator |
| Storage | ~596.2 GB disk, encrypted LUKS/LVM Pop!_OS installation; **free space and HDD versus SSD not yet measured** |
| Virtualization | VT-x present; does **not** make a second VM advisable |
| Usage/decisions | User keeps computers powered on. SSD upgrade had already been contemplated for general responsiveness. User initially **declined buying RAM** for this old laptop, then asked for pricing; **no purchase approved**. |

**Host recommendation:** Do not add a guest VM. If later approved for testing, use the officially documented Docker self-host layout directly on Linux; use a strong remotely hosted model for first tests, private access via SSH tunnel, no public listener or personal-account connector at the outset. Browser container and database may run into memory pressure; swap/zram capacity is not equivalent to RAM. A SATA SSD may help I/O and swapping but cannot upgrade the CPU or make 6 GB meet the 8 GB vendor starting point. An 8 GB configuration requires two 4 GB DDR3 SODIMMs, likely replacing the existing 2 GB module; optional low-cost hardware economics should **not** become an implementation dependency. Local model inference on this Dell is not practical. A separate Ollama host (such as the stronger Windows Legion) may be technically connectable but needs separate 64k-context/tool-use reliability and networking tests, and is not part of a first proof.

**Unmeasured gates:** `df -h /` for free disk, `lsblk -d -o NAME,SIZE,ROTA,MODEL` for HDD/SSD, Docker version, thermal/power stability, uptime, baseline memory during a representative hour, host's network exposure and whether the self-host source build can be obtained without special release access. The official self-host page's *prebuilt bundle* instructions refer to private releases/workflow artifacts; do not claim a publicly downloadable bundle is guaranteed. Source-based build remains documented but may itself tax old hardware. Neither path was run.

## HE pattern-mining — classify instead of copying

| Candidate lesson | Incremental question worth testing | Disposition |
| --- | --- | --- |
| Server controls authority; planner only proposes actions | Can a deterministic permission check reject a proposed action without trusting the model's own claim to be authorized? Does the checker itself introduce mistakes? | **MINE** as a boundary/evaluation example; no new HE rule required unless a real failure is found. |
| Untrusted web/email/source text cannot add owner authority | Can injected content persuade the agent to request or execute an unauthorized action? Does the gate remain closed when the model obeys the injection? | **MINE** negative test pattern. Relevant to any future authorized agent, not current HE runtime. |
| Specific approval + one-use claim | Can one approved action be altered/replayed, and what happens after crash-after-send versus crash-before-send? | **ASSESS** mechanism and target-system reconciliation; do not assert universal exactly-once semantics. |
| Memory grounded in owner messages | Can corrections be traced to owner evidence and old versions invalidated without treating external content as canon? | **ALREADY OWNED / REINFORCE** PMB provenance, correction and memory-boundary research. No DB, scheduler, `MEMORY.md` replica, or promotion machinery justified. |
| Usage/action trace and costs | Is there a supported stable export contract, reliable provider attribution and consistent units, rather than only a Nod-specific UI? | **PARK** Dashboard adapter until Nod is actually used and export is verified; native multi-provider sources remain authoritative. |
| Proactive scheduling | Does a real repeated personal task need website interaction or notification that the existing scheduled digest cannot handle? | **ASSESS** only through an isolated personal pilot; no replacement of existing automations by default. |

**Ownership and boundaries:**
- **HE:** store this note as **source research**, not HE-001/HE-002 amendment or a new framework.
- **PMB:** no memory migration, background compaction, new storage/DB or release gate; keep release and outstanding issue decisions independent.
- **ACR:** no Nod/remote agent or third-party data transfer in its local deterministic reviewer/test runner.
- **Multi-Provider Engineering Dashboard:** Nod is a separate *agent*, not a multi-provider telemetry aggregator or a provider-native usage data source. No scheduler/permission broker/AI orchestrator in Dashboard MVP. Home/Work isolation and corporate approval remain required.
- **Work:** Do not install Nod or export work prompts, app credentials, logs or repository data to a personally hosted assistant without formal company authorization.

## Small optional personal pilot (not authorized or performed)

**Decision question:** On hardware already powered on, does a separate Nod service reliably perform **one bounded personal web-monitoring task** that is inconvenient in the user's current tools, without exposing accounts, changing unrelated projects or needing a hardware purchase?

1. **No-install feasibility:** capture disk free space, SSD/HDD status, Docker version and baseline memory; verify current source self-host path without relying on a private release artifact. Abort if install requires broad privileges or resources the host cannot spare.
2. **Isolation:** local-only self-host, non-sensitive model account, disabled optional usage/trace sharing, no Gmail/OAuth, payment cards, password manager, public port-forward, WhatsApp or Telegram until explicitly justified. One user/token; protect `.env` and backups.
3. **Three fixture experiments:** public website read/compare + scheduled digest; controlled browser action requiring approval using disposable synthetic data; intentional duplicate/cancel/crash-uncertain simulation with **no real messages or purchases**. Check actual external state rather than relying on the assistant's narration.
4. **Record:** repeatability, successful completion versus native/simple deterministic alternative, CPU/RAM/swap/load/disk, missed schedules, action logs, checker false allow/false block, permissions and observed data destinations, provider usage/cost. Don't call vendor benchmark numbers a local test.
5. **Stop/retire:** if OOM, persistent swap thrash, frequent browser failure, ambiguous unsafe actions, unreliable scheduling, uncontrolled external data flow or maintenance exceeds task savings. A successful installation alone is **not** pilot success.

## Decision record

- **2026-11-16 disposition:** Preserve verified design lessons and Dell capacity limits. **No installation, VM, RAM or SSD purchase, model configuration, personal-account access, GitHub action outside research docs, or Work use approved.**
- **Revisit only if:** user selects one concrete task, confirms acceptable exposure/cost, and independently authorizes a sandboxed deployment test.
- **Research freshness:** Recheck public upstream release, trust checker and local-model contract at the time of any pilot. Vendor preview details are actively changing.
