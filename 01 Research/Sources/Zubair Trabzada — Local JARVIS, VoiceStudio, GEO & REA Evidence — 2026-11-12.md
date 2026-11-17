# Zubair Trabzada — Local JARVIS, VoiceStudio, GEO & REA Evidence — 2026-11-12

**Disposition: MINE SELECTIVELY / DO NOT ADOPT THE PLATFORM.** Research note only. No implementation approval, installer execution, environment changes, model downloads, external service connection, or PMB/ACR/Dashboard modification.

## Source ledger and scope

- Creator's video, [“I Stopped Paying for AI APIs… and Built This Jarvis Instead”](https://www.youtube.com/watch?v=BgpRjk4Yoqk), 24:55, plus user-provided description, chapter list and many annotated screenshots (2026-11-12). These show interfaces and demonstrations, **not independent evidence of runtime architecture, cost, security, reproducibility, or completion**.
- User-provided **6-page PDF** *Build Your Own Jarvis — The Free Prompt Pack* (Zubair Trabzada). This is the **cloud-assisted starter**: Python file-scan/graph, optional Anthropic Messages API, browser voice, personality, and markdown `/remember`. It is not the finished JARVIS shown in the video. Its text explicitly requests an Anthropic API key and offers a CLI-call workaround.
- Video screenshots of a separate **18-page “Run Jarvis Fully Local” prompt pack**, including pages 1–5 and a selection of install instructions, **not the actual downloadable 18-page file**. These screenshots describe Ollama + Whisper + VoiceStudio, a Mac/Apple-Silicon voice path, model hot-swap and a local-only ambition. We have not inspected the remaining pages nor run its installers. Do not treat the two prompt packs as interchangeable.
- [VoiceStudio](https://github.com/debpalash/VoiceStudio/tree/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7), pinned public source commit `ac4ad4295b265b9a39f2f684827bd3fe8f5417c7`: [README](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/README.md), [Windows install](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/install/windows.md), [engine matrix](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/engines/README.md), [GGUF engine](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/engines/omnivoice-gguf.md), [API authentication](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/api-auth.md), [license notice](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/LICENSE-NOTICE.md), [model licenses](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/backend/config/model_licenses.json), [MCP contract](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/mcp.md).
- [geo-seo-claude](https://github.com/zubair-trabzada/geo-seo-claude/tree/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c), pinned `989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c`: [README](https://github.com/zubair-trabzada/geo-seo-claude/blob/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c/README.md), [architecture](https://github.com/zubair-trabzada/geo-seo-claude/blob/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c/docs/architecture.md), [score methodology](https://github.com/zubair-trabzada/geo-seo-claude/blob/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c/docs/scoring-methodology.md), [citability scorer](https://github.com/zubair-trabzada/geo-seo-claude/blob/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c/scripts/citability_scorer.py), [Windows installer](https://github.com/zubair-trabzada/geo-seo-claude/blob/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c/install-win.sh).
- [REA](https://github.com/morluto/rea/tree/edc93046692c120567553fb68390db6ec5bc98c7), pinned `edc93046692c120567553fb68390db6ec5bc98c7`: [README](https://github.com/morluto/rea/blob/edc93046692c120567553fb68390db6ec5bc98c7/README.md), [CDP/browser observation](https://github.com/morluto/rea/blob/edc93046692c120567553fb68390db6ec5bc98c7/docs/guides/web/browser-observation.md), [installer approvals](https://github.com/morluto/rea/blob/edc93046692c120567553fb68390db6ec5bc98c7/docs/getting-started/installation.md). Its earlier evidence/permission/cost boundaries were **already covered** in [HE PR #30](https://github.com/unyieldingclaw-dev/harness-engineering/pull/30) and its [Nov 8 source note](./David%20Ondrej%20and%20Maitis%20%E2%80%94%20Linux%20Agent%20Hosting%2C%20DeepSeek%20Harness%2C%20REA%20and%20Free-Model%20Claims%20%E2%80%94%202026-11-08.md). This entry only records incremental visual/source-specific details.
- Screenshot links to Star History are **popularity charts**, not runtime evaluation. The Skool “finished JARVIS,” “brain-jarvis-skill-v8-windows.zip” and associated installation guides were not supplied as source bytes; their code and authority boundaries are **unverified**. No verified public GitHub source for the finished JARVIS was established from the supplied links.

Source inspection: first-party repository files at the pinned commits. **Not** an independent execution/test of any product, YouTube video transcript, user machine, downloaded ZIP, voice model, or website teardown.

## Claims vs established evidence

| Claim / image | Supported observation | Limit or correction |
| --- | --- | --- |
| “100% local,” “no paid APIs,” “nothing leaves the machine” | A local model path via Ollama can avoid model API requests; a separate screenshot pack describes Whisper and VoiceStudio as local engines | The **six-page PDF** uses Anthropic API and browser Web Speech; video shows Google Calendar, Gmail, Google Docs, GitHub and other connected tools. Cloud-enabled connectors are not offline. Claude Code is used to **build** even the local-only version. Need an actual egress audit before saying “nothing leaves.” |
| 3D second-brain graph and fly-to-source | Six-page pack specifies simple Python scan of markdown, label/folder grouping, ~700-character excerpt, keyword/wikilink edges, positional node IDs, `3d-force-graph` viewer, and top-six keyword-ranked notes in chat | The graph is mostly a visual **projection of filenames and links**. Position-based IDs can be unstable when file ordering changes. Keyword overlap is not evidence-based semantic retrieval, and illuminating answer-associated nodes is **not claim-level citation correctness**. |
| Natural voice + snark | Browser speech synthesis and Chrome speech recognition are requested in the first PDF. The video shows a personality layer, voice UI and local-model labels | A witty response style can be added without audio capture, a knowledge graph, or tool autonomy. Browser recognition is not guaranteed to remain on-device; streaming/wake word/barge-in are separate runtime problems. |
| Fully local voice | Screenshot pack specifies MLX/Apple-native CSM path; VoiceStudio itself has Windows releases with CUDA and CPU engines | The pack’s *specific engine* being Apple-only does **not** make the current VoiceStudio *application* Apple-only. On Windows, choose a supported backend and model explicitly, then test with the user’s hardware. |
| Reverse engineer igloo.inc | Screenshot shows JARVIS reporting DOM counts, captured scripts, bundle fingerprints and an inferred Svelte/Three.js/GSAP stack | REA does implement CDP observation, saved bundle analysis, source-evidence/limitations, origin checking, and optional screenshot capture. **An observed fingerprint is not proof that a particular animation algorithm or proprietary implementation was correctly reconstructed.** Browser inspection has explicit scope, consent and privacy/security requirements. |
| Tool armory, connect/reconnect, calendar use | UI shows state of Gmail, Calendar, GitHub and other tools and a calendar-read example | Connected ≠ permissions bounded; an agent can still be misled by untrusted tool responses. Distinguish read from write, present real source/tool receipts, require confirmation for consequential operations. |
| JARVIS uses `Qwen3.5:9b`, `Gemma4:12b`, etc. | Labels and local-model instructions visible in screenshots | Model names, speed, backend behavior, full offline execution, long-context reliability, and concurrency were **not independently measured**. Changing model is an execution identity/configuration change, not equivalent reasoning for free. |
| “Free” engineering | Local inference can reduce cloud API charges | Hardware, electricity, downloads, model maintenance, user time, voice licensing, and external integrations remain costs. Subscription-rate versus API-billed claims must be separated. |

## Deep pass — three referenced GitHub repositories

### 1. VoiceStudio — independent tool candidate; likely the highest practical value

**Evidence in current source**
- Native Electron desktop released for **Windows 10/11 x64**, macOS Apple Silicon and Linux; the Windows guide specifies NVIDIA CUDA acceleration or CPU alternatives, and approximately 10 GB disk capacity at baseline. Current repo is substantially broader than the video pack's Apple-MLX path.
- [Engine matrix](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/engines/README.md) lists distinct TTS/ASR backends with **different** OS/accelerator and voice-cloning support. “646 languages” is an app-level marketing feature count; do not infer uniform voice, transcription, accent and model quality across every engine.
- [OmniVoice GGUF](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/engines/omnivoice-gguf.md) gives nominal VRAM footprints around **945 MB Q8_0**, **659 MB Q4_K_M**, rather than requiring a 6+ GB full precision model. This might support a bounded voice trial alongside small LLMs; performance and total process memory require measurement.
- Local REST API plus MCP exposed on the running server, including `generate_speech`, `clone_voice`, `transcribe`. [MCP docs](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/mcp.md) warn that returning base64 WAV through an LLM tool context can be wasteful; **file-path-based output** is a useful optimization for media work.
- [Local API](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/docs/api-auth.md): loopback-only and **unauthenticated by default**. Non-loopback modes have separate PIN/API-key options. Even localhost is accessible to other local processes; never expose port 3900 publicly by accident. Select a restricted workspace/client before adding MCP.
- [License notice](https://github.com/debpalash/VoiceStudio/blob/ac4ad4295b265b9a39f2f684827bd3fe8f5417c7/LICENSE-NOTICE.md): application AGPL-3.0-only; `omnivoice/` code separately Apache-2.0; downloaded **default model weights CC-BY-NC** and audio-tokenizer assets have other terms. Do **not** infer unlimited commercial rights from “open source.” AGPL obligations matter if embedding a modified service; model/user voice rights are independent.
- The current Windows guide warns some TTS backends can out-of-memory during compilation at **less than 16 GB VRAM**. Test a lightweight engine rather than expecting any backend to coexist with a 9B LLM on an 8 GB GPU.

**Disposition:** **OPTIONAL EXPERIMENT IN LOCAL AI LAB**, not integration into HE, PMB, ACR, Dashboard or an always-on assistant. Test one voice path only if voice output has personal utility. Choose a voice you own or have permission to use.

### 2. geo-seo-claude — specialized marketing toolkit, not a harness pattern to transplant

**Evidence in current source**
- MIT-licensed Claude Code skill bundle: main `geo/SKILL.md`, **13 subskills, five parallel agents**, Python scripts, JSON-LD/schema templates, reports/PDF and install/uninstall scripts. It is built for **public website GEO/SEO audits**, not building or inspecting JARVIS itself.
- The composite GEO 0–100 score weights six categories (25/20/20/15/10/10). [Implementation](https://github.com/zubair-trabzada/geo-seo-claude/blob/989cae01e8ebbc42a9ec798eb9e7cb423f5ec89c/scripts/citability_scorer.py) gives points for regex-based writing patterns, word counts and formatting: e.g., **134–167 words** receives a favored length score. These are **developer-defined heuristics**, not observed end-to-end AI citation probabilities. Subagent interpretations can vary, and composite scores can conceal missing or unreliable inputs.
- The Windows installer copies skills/scripts/agent files into **user-global `~/.claude`**, creates a dedicated venv and installs dependencies. The advertised `curl | bash` shortcut executes unreviewed moving code if followed literally. Installing globally would change unrelated Claude Code sessions.
- External crawling can involve third-party page access and content. Do not claim “AI-citability readiness” proves actual ChatGPT/Gemini/Perplexity inclusion or sales lift.

**Disposition:** **NOT FOR HE/PMB/ACR**. At most, a separate one-off analysis of the user's own public website if a real search-discoverability question arises. Prefer conventional Search Console/crawlability/structured-data evidence rather than an unsupported GEO score.

### 3. REA — legitimate inspection tool with bounded evidence, not magical app reconstruction

**Evidence in current source**
- MIT, `rea-agents`, JavaScript/TypeScript MCP/CLI plus adapters for web, Electron, native, mobile, archives and several binary formats; some require external programs and OS-specific support. Current `package.json` reports version **6.9.0** at the inspected commit.
- [Browser observation](https://github.com/morluto/rea/blob/edc93046692c120567553fb68390db6ec5bc98c7/docs/guides/web/browser-observation.md) attaches to an explicitly user-owned Chrome-family browser via CDP. Default observation retains metadata rather than raw credentials/body text; separate script capture, dynamic scenarios, and exports have different authority and data implications.
- REA enforces loopback target endpoints and selected browser origins, and does not expose generic arbitrary CDP execution in its passive browser provider. But attaching a debugger is **not a harmless security sandbox**; it can reveal sensitive site information, and some capture options include URL query parameters/console text.
- [Installation](https://github.com/morluto/rea/blob/edc93046692c120567553fb68390db6ec5bc98c7/docs/getting-started/installation.md) previews MCP/client configuration changes, separates npm acquisition approval from setup approval and backs up old configuration.
- The video shows one claimed igloo.inc teardown; no independent reproducibility or side-by-side source validation was supplied. Previous HE REA source research already states the correct flow: **specific question → bounded capture → evidence + limitations → permitted implementation → independent test**.

**Disposition:** **RETAIN AS OPTIONAL INSPECTION TOOL** when ordinary public docs/code/browser DevTools do not answer a concrete question. No new HE harness layer, no sweeping “clone any site” task, and no installation merely to replicate the JARVIS demo.

## What is actually new or worth mining into HE

1. **Source-linked response UX, carefully labelled.** The fly-to-source interaction is a useful exploratory UI cue **only** if it links to actual source paths and bounded evidence excerpts. It must not imply answer correctness or per-claim proof from a graph edge.
2. **Model/engine state is not enough.** UI that shows `online`, `speaking`, model name and tool state can improve user orientation, but operational claims need **real runtime readback** (loaded model, endpoint, selected accelerator, response shape, elapsed time, refusals). Already covered broadly by HE source-authority and instrumentation work.
3. **Stable personality outside authority.** Dry humor is a presentation preference, not an agent's evidence, tool permissions, write authority, or reason to route extra inference. Turn off jokes for errors, ambiguous claims and irreversible actions.
4. **Separation of media bytes from model context.** VoiceStudio's documented file-backed audio return is a concrete example of keeping bulky binary media out of language-model messages. Apply only if a real audio workflow emerges.
5. **Evidence-enriched bounded investigation.** REA's distinction among page structure, script fingerprint, actual source, capture completeness, and inferred mechanism is a real source-level lesson; HE already covers similar boundaries. Record as supporting example rather than create more governance.
6. **Global installation side effects and local auth.** Skill packs, MCP tools, browser debugger access and local APIs can broaden privileges even when inference is offline. Preserve opt-in bounded setup / disable-uninstall tests for any later trial.

## Explicitly reject or park

- Entire “AI Workshop OS” and paid Skool JARVIS distribution as a dependency.
- A 3D graph, node-type taxonomy, universal memory capture endpoint, automatic memory promotion, voice activation, always-on mic, spontaneous proactive scheduling, all-app tool armory, unrestricted REA browser inspection, and multi-agent orchestration for PMB, ACR or the Dashboard.
- Treating `remember that...` as automatic trusted memory: the six-prompt design writes directly into `captures/` without evidence of consent scope, correction, injection resistance, duplicates, sensitivity, expiry or approval. Do not silently make it authoritative knowledge.
- Copying `geo-seo-claude`'s five-agent scoring framework or inventing a “context health” number.
- Presenting saved source snapshots, code fingerprints, popularity charts or vendor latency examples as proof of correct end-to-end behavior.
- Treating “free, offline, same endpoint shape” as same model quality or identical tool/safety semantics.

## Minimal if/when a genuine need emerges — no experiment was executed

**Option A — personality only:** Add a short style instruction to an existing assistant, without changing tools/memory/runtime. Success = responses stay factual and snark does not appear in security/correctness decisions.

**Option B — local voice demo:** On the Windows AI Lab (32 GB host RAM, RTX 5060 Laptop ~8 GB VRAM, Ollama, Qwen3.5:9b already available), first try ordinary system TTS or one lightweight VoiceStudio backend. Collect **end-to-end microphone-to-response latency, TTS start delay, peak GPU VRAM, RAM, speech intelligibility, audio device reliability, and evidence of network egress**. Do not run a model hot-swapper, wake-word daemon or full second brain. If 9B + voice cannot coexist in 8 GB VRAM, offload speech to CPU or stop; don't hide spillover.

**Option C — evidence tool:** On one **authorized, public** site with a bounded question, compare DevTools/public source to REA passive CDP capture. Require specific supported findings, known gaps, no uncontrolled credential/data access and no license/asset copying. This is already an HE REA experiment candidate, not a new project.

**Option D — website GEO:** Only for a **specific own-site question**, run one isolated, read-only conventional audit; compare objective crawl/structured-data results before deciding whether the specialized heuristics add any value.

## Ownership and final decision

- **HE** owns this **research record** and existing evidence/investigation patterns. No new required standards or runtime.
- **PMB** already has recall/index/detail experiments and archival research. A beautiful graph is not proof of retrieval benefit or a reason to change the release.
- **ACR** stays deterministic/local; do not inject JARVIS, VoiceStudio, web-teardown output or autonomous repair.
- **Multi-Provider Dashboard** stays separately scoped to native usage/reliability telemetry and read-only source-owned facts. No mic, agent armory, voice skin, graph, scraping, transcript store or knowledge engine in MVP.
- **Local lab** is the only reasonable place to try a narrow voice proof later, **if** desired.
- **No code changes, purchases, installations, model changes, data collection or hosted accounts initiated** by this evidence pass.

**Overall:** Interesting demo, especially the user-facing personality; a legitimate voice application and inspection tool in the source links; virtually no case for recreating the aggregate platform. A few standalone, measurable ideas; preserve the evidence and leave implementation optional.
