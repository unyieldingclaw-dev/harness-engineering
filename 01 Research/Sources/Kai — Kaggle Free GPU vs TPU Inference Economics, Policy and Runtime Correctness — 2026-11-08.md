# Kai — Kaggle Free GPU vs TPU Inference Economics, Policy and Runtime Correctness — 2026-11-08

**Research only.** This is a source-specific review, **not** permission to create a Kaggle account, run a cloud server, open a tunnel, install/execute third-party code, pass project files or tokens to external services, add a router, change PMB/ACR/HE configuration, or buy hardware.

**Primary user-provided source:** Kai (@kaiexplainsYT), [*Don't Buy a GPU Instead Build This 32GB Free AI Server Instead*](https://www.youtube.com/watch?v=IMVjEsKX5UQ). User supplied full ~15-minute transcript and video description, which cite October 11–12 2026 measurements. Assessed November 8, 2026. The attached transcript is not a complete reproducible lab record. Video includes affiliate marketing and secondary claims; treat its performance observations as illustrative, not an independently verified purchase or deployment recommendation.

## Sources checked independently (as of November 8, 2026)

- [Kaggle Notebooks documentation](https://www.kaggle.com/docs/notebooks): remote environment, T4x2 hardware, 12-hour GPU/CPU notebook sessions, ~9-hour TPU cap, 20-minute idle editor behavior. UI/quotas can differ by user and availability.
- [Kaggle Efficient GPU Usage](https://www.kaggle.com/docs/efficient-gpu-usage): quota nominally 30 GPU hours/week, variable with supply/demand; account-specific quota is source of truth.
- [Kaggle AUP](https://www.kaggle.com/aup) (effective June 22, 2025): prohibits abusive use of resources including **server farming** and **activity unrelated to ML data science**. Running a continuous public inference API/keepalive as a generic coding server plausibly falls into restricted activity; **no specific violation decision or ban was independently verified**. Purpose-built notebook model evaluations are more aligned with Kaggle than an always-on hosted service, but are not blanket permission to circumvent limits.
- [Cloudflare Error 524](https://developers.cloudflare.com/support/troubleshooting/http-status-codes/cloudflare-5xx-errors/error-524/) (updated July 23, 2026): default Proxy Read Timeout **125 seconds**, **not 100 seconds** as stated in Kai's whiteboard argument. Request path and progress/streaming still materially affect viability.
- [Cloudflare Quick Tunnels](https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/) (updated September 30, 2026): non-production, random URL per start, 200 concurrent request cap, **officially no SSE support**, no uptime guarantee. Working POST streaming through some clients is possible, but real compatible tool/SSE behavior must be tested, not assumed.
- [OpenRouter Qwen3.8-27B](https://openrouter.ai/qwen/qwen3.8-27b): listed provider rates and weighted average are **dynamic**. Not a fixed $2.46 or $1.49 output/M guarantee; input tokens, caching and provider reliability matter.
- [Ollama new model scheduling](https://ollama.com/blog/new-model-scheduling): multi-GPU throughput is runtime/partitioning dependent; 2x capacity is not 2x single-stream speed but “exactly one GPU's speed” is also too categorical.
- [ARahim3/kaggle-tpu-lab](https://github.com/ARahim3/kaggle-tpu-lab): **live GitHub source checked**, README, Qwen-specific README, kernel `qwen38-27b/kernel/serve_qwen38.py`, and `launch.py`. This is an implementation claim plus inspectable source, **not a benchmark run or a full security audit**. At time of review repository offers **Qwen3.8-27B** and newer **GLM-5.3-Flash**, which is a delta from Kai's description.

## Executive decision

**The headline is misleading but the main economics are plausible.** Two 16 GB Tesla T4 devices do not behave like one high-bandwidth 32 GB VRAM card. A public Kaggle notebook reached through Cloudflare isn't private local computing, and the startup/quota/reliability/reconfiguration burden can dominate simple token-cost comparisons. The TPU recipe is a genuinely useful example of open model deployment and benchmarking, but **policy/security/reproducibility boundaries make it inappropriate as an HE/ACR production execution dependency**.

**Most valuable HE-specific new evidence is not token speed:** the upstream TPU project documents **a concrete correctness bug in speculative decoding**, a conditional fail-closed patch, and separate structured-output/vision path runtime failures. That strongly reinforces HE's existing idea that inference runtime configuration is part of the **verified execution artifact**, not an invisible provider detail.

## Claim audit

| Video claim | Current evidence and qualification |
| --- | --- |
| Free 32 GB via 2×T4 | Kaggle docs list GPU T4x2. 16 GB each is not pooled as one fast contiguous device; weight partition and memory headroom matter. |
| Layer split gives speed exactly equal to one T4 | **Overstated**. Serial stages / inter-device coordination may restrict single-stream throughput, but aggregate bandwidth usage and concurrency vary by runtime and model. Video cites **16.23 tokens/sec from DevZoneX**, not our measurement. |
| 30 GPU hours, 12-hour session, 20-minute idle | Nominally grounded in Kaggle, but avoid treating editor idle timer, kernel session limit, and time quota as the *same* property. TPU quota is separately ~20h/week and ~9h/session. |
| 30k prompt at 325 tok/s gives 92s prefill and only 8s Cloudflare margin | **Back-of-envelope extrapolation** from theoretical speed; effective prefill varies with attention architecture, cache, batch, runtime, warmup and input path. Cloudflare's official current general proxy timeout is **125s**, not the video's universal 100s. The general “time to first token matters” lesson stands. |
| 1.75M output tokens/week, therefore ~$4.31 | Arithmetic: 30×3600×16.23 ≈ 1.753M tokens, × $2.46/M ≈ $4.31 **only under unrealistic 100% nonstop decode**, with the video's historical token price and zero startup/prefill/idling. It is **not** an actual week consumption or full request billing comparison. |
| Qwen TPU v5e-8 bf16 130 decode, 10,300 prefill | Reported by upstream maintainer for **that pinned vllm-tpu 0.28.0 + patch + speculative MTP + 8-chip setup**. 130 decode was reported **with MTP**; ~78 without. Don't quote as default on every host, load and prompt. |
| Supports 262k context | Upstream says it ran 225k test and supports 262,144 config. Large contexts still consume runtime resources and their quality needs separate testing. |
| Cloudflare API key means secure private inference | Actual `vllm --api-key` Bearer auth is better than open Ollama. Nevertheless public URL, Kaggle, Cloudflare, `ntfy.sh` notification path and local/remote stored tokens add trust surfaces. |
| TPU recipe is free and better for coding | Can be effective **for bounded, eligible ML studies**, but public long-lived server may violate Kaggle's AUP. Startup time and reauthentication often make paid hosted inference operationally cheaper. No universal model-quality parity assumed. |

## Most material implementation findings from upstream read

### 1. **Correctness bug in speculative decoding with Gated DeltaNet recurrence**

In [Qwen3.8-27B README, “Good to know / limits”](https://github.com/ARahim3/kaggle-tpu-lab/blob/main/qwen38-27b/README.md), the maintainer says **stock vllm-tpu 0.28.0 corrupts output when MTP speculative decoding is enabled**: rejected draft tokens advance recurrent Gated DeltaNet state without proper rollback. In 12 developer greedy exact-match comparisons, **0/12 stock outputs matched** non-speculative output. A bundled port of upstream [tpu-inference PR #3178](https://github.com/vllm-project/tpu-inference/pull/3178) restores rollback, reaching **12/12 exact matches** in that tiny sample; project reports 104 vs 78 tok/s in an A/B test and ~130 tok/s in the shipped config.

The kernel code [serve_qwen38.py](https://github.com/ARahim3/kaggle-tpu-lab/blob/main/qwen38-27b/kernel/serve_qwen38.py) checks patch application and **disables MTP if patching fails**, avoiding a known unsafe mode. Good pattern: feature activation is contingent on proven runtime correctness and explicit failure. **But 12 tests are not an exhaustive correctness proof**; this code is not validated by our ACR or HE test corpora.

**HE relevance:** pin exact backend/model/patch/effort/speculative/cache configuration and benchmark **functional output**, especially when comparing agents, rather than only throughput. If a future local agent yields garbage, root cause can be backend state-handling, not agent prompt/harness rules. Existing owner: `Local Inference Capacity, Context Headroom & Runtime Fitness`; **no new subsystem**.

### 2. Separate structured-output and image crash paths

Same README documents a bug when `JSON mode / structured outputs` and MTP async scheduler coincide: server exits with `AttributeError: __delitem__`; workaround is disable async scheduling or MTP. First image resolution may require **~1 minute** of TPU graph compilation and can timeout via tunnel. **Prefix caching off** on the selected vllm-tpu version means repeated turns re-prefill context. A steady-state decode benchmark therefore fails to characterize agent runtimes using tools, JSON schemas, screenshots and repeated context.

**HE relevance:** workloads must match real **tool paths**, not prompt-only self tests. This is a genuine runtime correctness/reliability case; it does **not** establish any defect in existing PMB/ACR deployments.

### 3. Source-inspected secret/notification boundary, not an alleged exploit

In [launch.py](https://github.com/ARahim3/kaggle-tpu-lab/blob/main/launch.py) the launcher creates a random API key and an approximately 80-bit entropy `ntfy` topic, writes `{"kernel", "topic", "api_key"}` as JSON to `~/.kaggle-tpu-lab.json` using normal file writing, and embeds the key in a pushed **private Kaggle script kernel**. In [serve_qwen38.py](https://github.com/ARahim3/kaggle-tpu-lab/blob/main/qwen38-27b/kernel/serve_qwen38.py), `publish("ready", ..., api_key=CFG["api_key"], ...)` sends a notification **containing that key to third-party `https://ntfy.sh`** when the launcher config enables its topic. The topic is randomly generated, reducing casual enumeration. But **this is a concrete third-party credential-handling and retention dependency**; no breach, unintended disclosure or weak randomness was verified. The public serving endpoint additionally requires Bearer auth.

**Security consequence for *any* candidate experiment:** inspect local token persistence/file permissions, Kaggle script visibility, external progress notifications/logs, dataset sources and public endpoints before providing any credentials; only synthetic/public test data. Neither blanket approval nor assuming “we inspected open source, therefore safe” is justified.

### 4. No free daily production platform

The repository spins up ephemeral Google-owned TPU hardware, relying on public third-party compile-cache/weight mirror datasets. A new run can change URL/runtime availability. The actual output may be fast while the system's **time to acceptable complete task** remains poor due to startup, retries, tool-call fidelity, provider limits and in-flight state reset.

Cloudflare's current [Quick Tunnels docs](https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/) say SSE is not supported. The source claims Claude Code and OpenAI-compatible agent clients work: **resolve this with actual requests** (especially streamed tool deltas) rather than declaring all streaming broken or all clients compatible. POST streaming may work despite limitations for browser GET EventSource; it is not guaranteed.

## Fit to the user's AI workstation and current work

- Existing laptop has an **RTX 5060 laptop GPU** and a 32GB host RAM machine, already running local Ollama/Open WebUI. Kaggle does **not replace that local, privacy-contained capability**; it trades hardware constraint for cloud dependence and quota.
- It can be a **separate model-performance learning sandbox** for public, synthetic ML experimentation on accelerated hardware the laptop cannot fit. Keep **work code, private repos, keys, memory content and sensitive ACR data** off the free Kaggle/tunnel route absent explicit data/security permission.
- Don't buy a GPU **because of this video**. Buy/upgrade only after an observed local workload cannot be served acceptably at realistic owned hardware costs, and requires privacy, offline use or predictable sustained access that hosted models cannot satisfy.
- Managed hosted Qwen API is generally simpler for daily coding, but its pricing/rate limits, provider retention policy, reliability and tool compliance must be evaluated independently; it is not automatically appropriate for employer code.

## HE ownership / disposition

**Already owned by**
- `01 Research/Local Inference Capacity, Context Headroom & Runtime Fitness — 2026-09-24.md`
- `01 Research/Sources/Kai — Local Coding AI, KV Cache & Runtime Envelope — 2026-09-26.md`
- `01 Research/Sources/Coding Horizon — Local AI Total Cost, Ownership & Hybrid Routing — 2026-09-28.md`
- `01 Research/Model-Tiered Workflows & Independent Factory Assurance — 2026-09-24.md`
- `01 Research/Bounded Execution Envelopes — 2026-09-22.md`
- `01 Research/Harness Portability, Exit Cost & Inspectability — 2026-09-24.md`

**Retain:** MTP rollback/check-and-disable example; prefill/steady-state/end-to-end split; *model+backend+patched features* as execution identity; quota/policy/security in economics; public-endpoint auth and third-party notification route; source-specific known structured-output failure.

**Reject:** “free 32GB local server,” “T4 pair exactly one GPU speed,” “100s Cloudflare hard stop,” 130 tok/s as general performance guarantee, $4.31 as universal hosting cost, routine server farming, permanent tunnels, wholesale infrastructure imports.

**Potential experiment only if actually needed:** public synthetic evaluation of one bounded Qwen workload, comparing (a) existing local Ollama configuration, (b) hosted provider, and (c) Kaggle in-notebook batch inference **within provider permitted use**, keeping identical task fixtures, outputs, tool schemas and verifier. Include setup-to-accepted-result time, first-token latency, tool correctness, quota loss and crash/retry costs. Do not stand up a public Kaggle coding server or supply project secrets to test this video. **No experiment run, architecture implementation or settings changed.**
