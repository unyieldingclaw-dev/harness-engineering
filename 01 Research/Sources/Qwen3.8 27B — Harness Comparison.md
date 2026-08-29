# Qwen3.8 27B — Harness Comparison

## Source

**Video:** Qwen3.8 27B: Same Model, Three Harnesses, One Clear Winner  
**Creator:** James Layne  
**URL:** https://www.youtube.com/watch?v=sSySOPGNdjw  
**Date captured:** 2026-08-29

This is a research intake record. The video's ranking is an individual benchmark result, not evidence that one harness is universally superior. The useful question for Harness Engineering is what the comparison demonstrates about the effect of the harness on a fixed model.

## Core Observation

The video holds the model constant — Qwen3.8 27B — and compares three coding harnesses:

1. Pi Coding Agent / Pi Harness
2. Hermes Agent
3. DeepSeek Harness

The creator argues that the same local model can produce materially different results depending on the harness because the harness controls how the model is prompted and how it interacts with tools. The video describes a harness as a program that manipulates the model, including tool-calling functions intended to get more useful behavior from the model.

This is directly relevant to the central Harness Engineering premise: **model capability and harness capability are separate variables.**

## Tests Used

The comparison used two coding tasks against the same Qwen3.8 27B model:

### Test 1 — Minecraft Clone

A large, demanding prompt intended to produce a playable Minecraft-like implementation with many requested functions. The creator describes this as a long-standing stress test used with multiple models.

The creator notes that this is intentionally difficult and that only a much stronger model had previously one-shotted the prompt in his experience.

### Test 2 — Color Palette Website

A more conventional but visually demanding website-generation task. The goal was a functional palette generator with specified features and a particular visual style.

The creator attempted to keep the comparison reasonably consistent, including asking each harness to implement suggested improvements for the website test.

## Reported Results

The creator's final ranking for these tests was:

1. **DeepSeek Harness**
2. **Pi Coding Agent**
3. **Hermes Agent**

This should be treated as the creator's result for these particular prompts, settings, hardware, and evaluation method — not a general benchmark of the three harnesses.

The creator reported that DeepSeek Harness produced especially strong results with Qwen3.8 27B and claimed that tasks which previously required moving to a more powerful model were achievable without doing so when using this harness.

The video also reports that DeepSeek Harness used substantially more tokens in pursuit of those results. The creator explicitly connected the behavior to increased thinking time and said the improved result appeared worth the additional token use in his local setup.

## Harness Differences Worth Investigating

The most important research signal is **not the ranking**. It is that changing the harness changed the effective behavior of the same model.

Areas worth examining in the underlying repositories:

- system prompt structure;
- tool definitions and tool-call conventions;
- agent loop design;
- planning versus execution behavior;
- context management and compaction;
- retry/recovery behavior;
- verification loops;
- how much intermediate reasoning/tool output is retained;
- model-specific prompting or reasoning controls;
- extensibility/plugin boundaries;
- how the harness decides when to continue, retry, or stop.

The comparison is therefore useful as a **harness-design research lead**, even if the particular winner changes over time.

## Context / Token Signal

This source is especially relevant to the project's ongoing concern about context and token consumption.

The video explicitly reports that the DeepSeek Harness uses a lot of tokens and suggests that its performance comes partly from allowing more thinking time. That creates an important engineering tradeoff:

> Better harness behavior may come from spending more inference/context budget, rather than from making the system intrinsically more efficient.

For PMB and other harness work, we should distinguish:

- **token efficiency** — accomplishing the task with less model input/output;
- **context efficiency** — keeping useful information available while avoiding unnecessary context growth;
- **task efficiency** — requiring fewer agent/tool cycles to reach a correct result;
- **quality efficiency** — achieving a better result for the same token/context budget.

A harness that wins a task by spending dramatically more tokens is not automatically the better architecture.

## Relevance to PMB

### High-value research question

**How much of an agent's apparent model capability is actually produced by the harness?**

PMB should not assume that changing models is the only way to improve difficult tasks. Conversely, PMB should not assume that a sophisticated harness loop is worthwhile merely because it can make a smaller model appear stronger.

Useful areas to compare against PMB:

- context assembly;
- compaction strategy;
- tool-use loop;
- explicit planning;
- recovery from failed actions;
- verification before completion;
- model-specific prompting;
- token/context accounting.

### Important PMB guardrail

Do **not** copy a harness's complexity merely because it improves benchmark performance. Any candidate mechanism should first demonstrate a concrete PMB problem or measurable benefit.

## Relevance to ACR

ACR is a particularly interesting future test subject because code review is easier to evaluate than broad autonomous coding.

Potential experiment:

> Hold the review model constant and run the same fixed review corpus through multiple harnesses. Measure finding precision, recall, duplication, unsupported findings, context consumption, and total inference cost.

This would be substantially more informative for ACR than adopting a harness based on a YouTube coding benchmark.

## Potential Follow-Up Experiment

A useful controlled experiment would use a **fixed model + fixed task + fixed repository + fixed success criteria** while changing only the harness.

Record at minimum:

- model;
- quantization / inference backend;
- system prompt;
- task prompt;
- available tools;
- tool-call count;
- input tokens;
- output tokens;
- context size over time;
- compactions;
- retries;
- elapsed time;
- final correctness;
- verification result.

For PMB specifically, this would help determine whether a harness feature is genuinely improving reasoning/task completion or merely spending more context and tokens.

## What We Should Mine From the Repositories

If these harnesses are investigated later, prioritize implementation mechanics rather than UI:

### DeepSeek Harness

Investigate why its agent loop reportedly gets more out of Qwen3.8 27B. Pay particular attention to context construction, tool orchestration, retries, verification, and reasoning/control prompts.

### Pi Coding Agent

Investigate its comparatively lightweight coding-agent architecture and model configuration. It may provide a useful counterexample if it achieves strong results with less machinery.

### Hermes Agent

Investigate why the same model behaved differently and why the creator reported longer execution times. Look for architectural choices that trade latency for additional agent work.

## Relationship to Existing Research

This source reinforces several existing research threads:

- **Harness architecture:** alternative agent loops can materially change model behavior.
- **Context & memory:** long-running agent performance depends heavily on how context is assembled and compacted.
- **Model routing:** a harness may change the point at which a task requires escalation to a stronger model.
- **Independent evaluation:** harness comparisons need controlled tests rather than subjective impressions.

It also strengthens the case for maintaining explicit token/context measurements rather than treating model selection as the only performance variable.

## Disposition

**ASSESS / RESEARCH**

Do not adopt any of the three harnesses based on this video.

The video is useful enough to justify examining the underlying implementations because it provides a concrete example of **same model, materially different harness behavior**. The strongest potential lesson for PMB is methodological: when evaluating agent architecture, hold the model constant and measure the harness as an independent variable.

## Source Limitations

The video is not a controlled scientific benchmark. The tests were created and evaluated by the creator, the sample is small, and the reported winner may depend on prompt construction, configuration, inference backend, model version, and subjective evaluation — especially for visual quality.

The claims about token use, performance, and hardware are therefore useful observations to investigate, not established engineering facts.

## Links Mentioned by the Source

- Video: https://www.youtube.com/watch?v=sSySOPGNdjw
- Minecraft benchmark prompt: https://docs.google.com/document/d/1g...
- Website color-palette prompt: https://docs.google.com/document/d/15...

The source also references related harness/model comparison videos, but those are not treated as evidence here.
