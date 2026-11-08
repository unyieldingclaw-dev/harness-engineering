# World of AI — Message Threads, Agent Instruction Injection and DeepSeek Creator Mode — 2026-11-08

**Status:** source research and security implications only; no runtime change, model adoption, new mandatory HE gate or PMB/ACR implementation.  
**Video:** [*Anthropic's RSI Breakthrough?! NEW Fable 5.5 Checkpoint, NEW GPT, DeepSeek Harness, & More!*](https://www.youtube.com/watch?v=i-wYjUriSzo), World of AI, Nov 8 2026. **Evidence:** user-supplied full timestamped ~33-minute transcript (602 lines), description and chapter list. The 6:27–7:37 CodeRabbit Secure segment is **paid sponsorship**, not a benchmark or independent validation. The author advertises their own woaibench.ai platform; demos and head-to-head claims are not independent evaluations.
**Review method:** matched source claims to official Anthropic API docs/SEC filing, Forever Security report, DeepSeek Harness website + source code, MIT researcher publication, OpenAI's first-party account, and Meituan LongCat notes. Static research only: no proof replay, model installation, running plugins, security tests, or benchmark reproduction.

## Decision

**Three technical mechanisms matter to HE; one is an actual security failure class.**

1. **New evidence:** server-assisted *message transport* continuity can lower API request overhead while the **client remains the session source of truth**. Not PMB memory/compaction.
2. **Real-world security evidence:** attacker-origin `AGENTS.md`/`CLAUDE.md` instructions can cause global config mutations, unsafe package install, API endpoint redirection and token compromise. The missing boundary is **provenance + authority**, not “how good is the agent at obeying rules.”
3. **New product-specific extension details:** DeepSeek's Creator Mode exposes runtime API discovery (read-only) alongside a separate plugin installer that can persist executable extensions. Worth examining for *inspection-before-creation* and effective-effect verification; no reason to adopt another harness just for this.
4. **Supporting evaluation warning:** MIT's small-model weight edits repaired a highly measurable repetition/truncation defect and made existing ability observable; this is not a general “AI trained another AI without data” result or an HE runtime feature.

**Exclude as architecture decisions:** Fable 5.5 unverified anonymous checkpoint, new GPT/Google routing leaks, Qwen open-weight strategy rumor, impressive voxel animations, robotics demo, standalone market predictions.

## 1. Claude Messages API threads — transport, not memory (26:28)

Official Anthropic sources:
- [Nov 2 announcement](https://claude.com/resources/articles/message-threads)
- [Messages API message threads](https://platform.claude.com/docs/en/build-with-claude/message-threads)

**Verified:**
- API public beta; `thread: create` sends initial history + system + tools, then `thread: continue` sends only **new** content and the previous *response ID*. For supported long-running coding sessions, Anthropic reports **~35% median time-to-first-token reduction** at **500k–1M tokens**. A reported bytes-on-wire decrease is **not token-billing savings**.
- Model still sees/reads the full logical conversation. Token usage, standard token prices and rate limits **unchanged**. Distinguish **network serialization latency, model processing, and end-to-end verified task latency**.
- **Expiry:** five minutes of inactivity; deletion at most an hour after last use. Continue after expiry produces 404; client must recreate a thread from **its own authoritative full history**. Client/SDK handles replay only while it retains that history.
- Thread create locks many request fields: model, tool choice, thinking, MCP servers, context management and more. Modifying them during continuation can cause fingerprint mismatch. Changing model during fallback requires the documented fallback/rethread procedure.
- Memory encrypted in process/server-managed memory, not durable storage; **not Zero Data Retention eligible and not HIPAA eligible**. This matters for confidential corporate/customer inputs.
- **No list/read/delete API** for active threads. A thread reference alone is not a durable session checkpoint; the host cannot reconstruct a lost history solely from its ID.

**HE interpretation:** keep source-owned task/conversation events/decisions outside transient API threading. Only consider native provider API threading if repeated full-history uploads are a *measured bottleneck*, and the supported host/model/provider/data-handling contract is verified. If Claude Code already enables it by default in the user's plan/runtime, do **not** create a second thread manager. PMB should not read or maintain provider-internal thread IDs as project memory.

**Conditional acceptance:** long/short session A/B with same task, model, outputs, available tools, streaming/prefill/cache settings; verify 404 replay, no lost tools/results after a pause, fallback after changing model, request-size/TTFT, full task duration and permitted retention terms. No implementation warranted from the video alone.

## 2. Forever Security's poisoned instructions — concrete attack (5:25)

[Gal Weizman and Omer Samet, Forever Security, Oct 22 2026](https://forever.security/blog/hijacking-ai-agents-via-downloads-folder). Their first-party report documents detection of a real malicious `AGENTS.md` in a customer's Downloads folder, then isolated reproduction of the exploit paths.

**Codex path observed/reproduced:**
- Instructions made the agent register a malicious npm MCP “helper” in **`~/.codex/config.toml`**, a globally loaded configuration.
- It fetched and executed the known malicious package `function-color`. The researchers say their controlled test of the actual payload executed malware **in over 90% of attempts**; the initial live incident was **stopped before code ran** by the defender.
- Injected guidance sought to run a child Codex process while bypassing approvals/sandbox, and used an external launcher to survive startup timing limits.

**Claude Code adaptation reproduced by researchers:**
- A planted `CLAUDE.md` induced writes to project `.claude/settings.json` setting `ANTHROPIC_BASE_URL` to an attacker-controlled “company gateway.”
- The first connection on a reloaded/subagent session transmitted the agent's OAuth token to the hostile endpoint, which could impersonate the model and return unsafe tool calls.
- The report says **~50% success for its Claude Code attack tests**, and the same-session code-execution chain worked in about **one-fifth** of trials. These are **controlled test rates**, **not** prevalence of compromised users.
- Separately, the researchers report ~half their monitored **customers** have used agent sessions inside Downloads. That's an **exposure statistic**, not a compromise rate. Do not mix the denominators.

**What actually transfers to HE/PMB/ACR:**
- Trust of an instructions file must not be inferred solely from its filename or apparent format. A browser-downloaded file, recently cloned repo, generated doc, unreviewed memory, or tool result can carry instructions that should be treated as **source data**, not an authorized policy.
- Keep agent current working directory out of Downloads/untrusted files. Inspect unrecognized `AGENTS.md`/`CLAUDE.md`, especially before allowing them to influence tool execution. A policy cannot solely rely on an LLM noticing malicious prose.
- Apply provenance review to **config writes** targeting endpoints/proxies, MCP servers, permission bypass and local/global agent settings: `~/.codex/config.toml`, `.claude/settings.json`, `.claude/settings.local.json`, and applicable workspace configuration. Do not blanket-ban legitimate settings or assume all files are controlled by one repository.
- Native approvals/sandbox, least-privilege local OS user, pinned dependencies and explicit diff review help limit damage. Path/regex inspection **alone** cannot fully validate shell commands, symlinks, nested configs or benign-looking tool payloads.
- If real indicators exist, treat credentials as potentially compromised and use documented response/rotation practices. Do **not** infer this user's devices or repositories are affected.
- Cross-reference separate [Apiiro Claude Code allow-rule leak](https://www.apiiro.com/blog/claude-code-secret-leak/) discussed in earlier HE/PMB/ACR security reviews: **plaintext approval persistence** and **instruction-origin prompt injection** are separate causes, though both involve config files.
- Useful **conditional review case:** in an authorized ACR security pass, inspect instructions/config files changed in a PR and any unexpected MCP/API endpoint, without triggering new permanent scans, global rule sets, or falsely classifying all legitimate overrides as malware. No new scanner installed.

## 3. DeepSeek Harness Creator Mode — inspectability vs installation (27:34)

Official [DeepSeek Harness product page](https://www.deepseek.com/harness/en/) confirms public preview and chat-built plugins, and the actual [`@deepseek-ai/dsh-tool-cordis` source README](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/extensions/tool-cordis/README.md) confirms:
- `cordis_inspect_list` and `cordis_inspect_query` return **read-only API/schema discovery**, supplied via a host registry. Client-side queries require a responding browser/page.
- A distinct **Plugin Manager** owns persistent installed bundles, including executable code and MCP configuration. Inspection alone cannot install or run generated plugins.
- The inspected tool schema and live provider configuration can be retrieved narrowly, avoiding an everything-in-context host dump. Not all provider state is automatically visible.
- This is an **extensible desktop harness with user-created plugins**, **not** independently demonstrated superiority in coding quality, enterprise safety, or portability.

**HE:** retain *read authoritative runtime interface → produce smallest extension → explicit install authorization → verify actual effect → rollback*. Those mechanisms overlap existing HE source-specific research on inspectability, extension/supply-chain authority, and bounded effects. **No new generic plugin registry** without a current product need. The viewer's “with vs without harness” 3D castle example gives one candidate more retries/tools/tokens; it is **not** a controlled model-equivalence evaluation.

## 4. Other claims: true, exaggerated, or unverified

| Video time | Claim | Primary-source check and HE disposition |
| --- | --- | --- |
| 2:31–5:25 | Anthropic's Nov 2 amended S-1 proves RSI might *already* have been crossed | **Incorrect reading.** [Actual SEC S-1/A filed Nov 2](https://www.sec.gov/Archives/edgar/data/2130357/000162828026068676/anthropicpbc-sx1ax1.htm) says **future-model expectations**, rather than current models, may *imminently* meet its accelerated-R&D threshold, and explicitly **no current model has been designated** as exceeding it. Prior [Nov 5 METR](https://metr.org/blog/2026-11-05-anthropic-ai-rd-acceleration-assessment/) finding (~1.9× capability-improvement trend; wide uncertainty/redactions) is **not proof of recursive autonomous R&D**. Prior HE METR source already owns analysis. |
| 7:37–17:35 | “Fable 5.5” anonymous checkpoint dominates | **UNVERIFIED identity/release/attribution**, visually impressive examples but no independent model identity or controlled conditions. Release Nov 10 is **speculation**, not announcement. Ignore for model-tier/ACR policy. |
| 18:12–20:15 | 10k AI agents solved Navier–Stokes recently | **Previously published**, not a new Nov 8 preview. [OpenAI Sept 8 account](https://openai.com/index/navier-stokes-solution/) describes ~10k concurrent agents, ~88h to a result, 17h Lean formalization, a public writeup/Lean artifact and large compute budget. Those are **OpenAI's assertions**; scholarly/Clay acceptance differs from formalization. Not a recommendation for 10,000 coding agents. |
| 20:15–21:33 | “Lifeval” new benchmark | [OpenAI LifeVal](https://openai.com/index/lifeval/) launched Nov 2. Practical consumer tasks and rubric-centric assessment are real; a provider's published scores are not independent validation. **Useful research direction**: evaluate end-to-end user task and actual successful action, not raw text or tool activity. Does not require an HE consumer benchmark. |
| 21:33–23:01 | GPT-6 Astra improved Qwen 0.8B math ~6× by touching .016% weights | **TRUE in an early researcher evaluation, narrowly.** [MIT researcher original Nov 2 post](https://rikiyat.github.io/blog/weight-space-programming/) reports MATH-500 **7.9%→48.1%**, GSM8K **13.8→67.1%**, IFEval **21.0→62.3%**, with 50 allowed forward-pass experiments, 131,072 weights modified. Critically **93%→33.1% of answers hitting 32,768-token cap**. The researcher explicitly says the edit mostly repaired a **looping/truncation failure**, not lack of general mathematical capability; no equivalent gain on a non-looping Gemma small model. **HE lesson:** diagnose actual failure mode before crediting an alleged capability upgrade. Not a model-tuning action item. |
| 23:50–25:36 | Antigravity/Gemini art demo, stealth model “Kentia” | Demos and anonymized attribution cannot establish general coding correctness, cost, reproducibility or a supported model ID. **MONITOR only.** |
| 25:36 | Pulse Alpha free stealth model | Provider/model identity speculative; a free endpoint needs cost, data-policy, reliability and tool-capability checks. No automatic route change. |
| 29:10–30:06 | Qwen abandoning smaller open weights | Creator explicitly calls it **unconfirmed rumor**; not a migration or purchase signal. |
| 30:06–31:12 | Google smart AI routing | Attributed to leaked discussion, not verified first-party deployment. HE already studies tiering and fallback semantics; do not encode guessed model-selection heuristics. |
| 31:12–31:56 | LongCat 2.5 is 1.6T/48B active multimodal agent model | [Meituan's Nov 2 technical report](https://tech.meituan.com/2026/11/02/Meituan-LongCat-2.5.html) confirms model characteristics and training with long-run trajectories, cross-harness mixtures and replays of **actual agent failure histories** to create verifiable training tasks. **Reinforces** existing HE “failure history → candidate eval fixture” research; training-system claims are vendor-authored and not a reason to deploy or replicate long-horizon RL. |
| 31:56–32:54 | Traffic-gesture humanoid robot | News / robotics demonstration; no specific HE engineering mechanism established. |

## Portable decisions and boundaries

**Immediate research classification (no deployment):**
1. **HIGH**: review entry-point instruction provenance/config write authority during existing PMB/ACR security reviews; test only if a real threat surface is present.
2. **MEDIUM**: retain API threading as a potential provider-native latency optimization, with proper replay/error/retention semantics; no generic session-memory rewrite.
3. **MEDIUM**: runtime introspection and separate plugin installation as a concrete extension contract; optional study, not platform migration.
4. **LOW / informational**: MIT targeted failure-mode edit and LifeVal outcome-oriented eval examples.
5. **DISMISS as roadmap signals**: Fable 5.5, stealth-model identities, rumored Qwen/Google changes, proof of full RSI, model leaderboard claims from demo videos.

**Existing HE owners:** 
- `01 Research/Sources/METR — Anthropic AI R&D Acceleration, Redactions and Verification — 2026-11-08.md` (same draft PR #29).
- `01 Research/Sources/Manolo Remiddi & DeepSeek Harness — Lock-In, Inspectability & Exit Cost — 2026-09-24.md`.
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`.
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`.
- `01 Research/Experience-Derived Harness Evolution — 2026-09-25.md`.
- `01 Research/Budgeted Side Effects, Approval Gates & Receipts — 2026-09-25.md`.
- `01 Research/Artifact-Gated SDLC, Behavioral Evals & Metrics — 2026-09-22.md`.
- Prior security-only research PR #24.

**Not done:** no install, benchmark, proof of exploit, code mutation, threat detector, PMB/ACR security rule or new product subsystem. A demonstrated recurring operational gap—not a news headline—would justify a future implementation experiment.
