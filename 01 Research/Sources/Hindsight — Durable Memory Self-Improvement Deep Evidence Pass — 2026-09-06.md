# Hindsight — Durable Memory Self-Improvement Deep Evidence Pass — 2026-09-06

## Source

- Repository: [EfficientStreet/hindsight](https://github.com/EfficientStreet/hindsight)
- README: [README.md](https://github.com/EfficientStreet/hindsight/blob/main/README.md)
- Full walkthrough: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)
- Skill: [SKILL.md](https://github.com/EfficientStreet/hindsight/blob/main/SKILL.md)

## Corpus relationship

- **SHARPENS** PMB Dream / retrospective memory consolidation research.
- **SHARPENS** PMB durable-state, authority, and context-cost research.
- **MERGES WITH** David Ondrej's decision/discipline findings around explicit human control and narrow concern skills.
- **MERGES WITH** Anthropic's continuous feedback/eval direction, but Hindsight is retrospective memory consolidation rather than continuous evaluation.
- **ORTHOGONAL TO** OpenMAIC's durable execution runtime: Hindsight addresses what durable lessons should enter memory after work; OpenMAIC addresses durable execution state and recovery while work is running.
- **NO EXISTING MATCH** for the exact proved-vs-suggested memory-admission distinction observed here.

## Executive finding

Hindsight is not primarily a memory store. It is a **retrospective memory-admission procedure**.

Its central loop is:

1. inspect the complete session;
2. identify process failures and their upstream causes;
3. discard one-off content noise;
4. verify session-specific claims against what actually happened;
5. separately judge whether the lesson was **proved** or merely **suggested**;
6. merge into an existing memory topic rather than creating duplicates;
7. write only durable lessons;
8. report what was retained and what remains only a watch-item.

The strongest idea for PMB is the separation of **factual verification** from **evidentiary strength of the generalization**. A session can accurately establish that an event occurred without proving that the event represents a durable rule.

## 1. Memory is treated as a filter, not a transcript

The README explicitly rejects session journaling. Hindsight keeps only lessons that would change how a future task is handled and discards one-off task content. A clean session is allowed to produce "nothing to report."

Source: [README.md](https://github.com/EfficientStreet/hindsight/blob/main/README.md)

### Harness implication

This strongly supports a PMB distinction between:

- **session evidence/history** — what happened;
- **candidate lesson** — an interpretation of what should change;
- **durable memory** — an admitted rule that future work should use.

A retrospective should therefore not automatically become memory merely because it can summarize a session.

**Disposition: SHARPENS PMB Dream consolidation.**

## 2. Root-cause compression is the admission unit

For repeated attempts, Hindsight asks for the single upstream change that would have prevented the chain rather than recording every failed attempt as a separate lesson. It distinguishes missing information from process failures such as skipping a step, assuming instead of asking, or verifying the wrong thing.

Source: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)

### Harness implication

The useful durable object is not "failure X happened." It is closer to:

> **Failure pattern → root cause → preventive behavior.**

This is potentially valuable for PMB because raw retrospective history is expensive context while a validated preventive rule can amortize that cost over future sessions.

**Disposition: HIGH-VALUE RESEARCH.**

## 3. The proved-vs-suggested distinction is unusually strong

Hindsight explicitly separates two questions:

1. **Was the session claim accurately established?**
2. **Does the session establish the broader lesson as a rule?**

It defines **proved** as recurrence, confirmed root cause, or a fix observed to work. **Suggested** means a single plausible occurrence without independent confirmation. Suggested lessons are either held out of memory as watch-items or persisted only with explicit uncertainty.

Source: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)

### Why this matters

This is stronger than ordinary "confidence" metadata. Confidence describes an agent's belief. Hindsight instead asks what the **evidence establishes**.

That is directly compatible with our evidence-first corpus discipline:

> Accurate observation does not automatically justify generalization.

Potential PMB admission vocabulary:

- observed;
- supported;
- corroborated/repeated;
- verified fix;
- suggested pattern;
- admitted durable rule.

Do not adopt those labels as schema yet; this deserves testing against actual PMB retrospective sessions.

**Disposition: PRIORITY RESEARCH.**

## 4. Hindsight does not re-open the work

The skill is explicitly read-only against the session's actual work. It does not re-run tests, redo fixes, or second-guess completed decisions. Its question is different: "what should be done differently next time?"

Source: [SKILL.md](https://github.com/EfficientStreet/hindsight/blob/main/SKILL.md)

### Important boundary

This creates a useful separation:

- **retrospective learning** asks what process lesson the session suggests;
- **verification** asks whether the artifact/change is correct;
- **review** asks whether the work satisfies standards/specification;
- **admission** decides what becomes durable memory.

Hindsight should not become a disguised second code review.

**Disposition: MERGES WITH existing review/evidence separation.**

## 5. It distinguishes environment findings from process lessons

The guide says real environment discoveries — e.g. a tool behaving differently than documented, permissions being more restrictive, or a connector failing — should be recorded as factual findings separately from process lessons.

Source: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)

### Harness implication

This is important for PMB because environment facts and behavioral guidance have different authorities and lifecycles.

Possible model:

| Object | Meaning | Example |
|---|---|---|
| Environment finding | observed fact about the system | connector rejected operation |
| Process lesson | recommended future behavior | check connector capability before planning around it |
| Decision | human-selected choice | use alternate path |
| Durable memory | admitted future guidance | always verify connector availability before relying on it |

Do not collapse these into one "memory" class.

**Disposition: SHARPENS durable-state/authority model.**

## 6. Existing-topic merge is a deliberate anti-duplication mechanism

Before writing, Hindsight checks whether an existing memory already covers the topic and extends that entry instead of creating a near-duplicate. It also requires the memory index to reflect new or changed entries.

Source: [SKILL.md](https://github.com/EfficientStreet/hindsight/blob/main/SKILL.md)

### PMB connection

This directly overlaps the filing-contract problem recently identified in PMB: one fact/topic should not proliferate into multiple competing homes.

However, Hindsight's implementation is based on an atomic per-fact memory system. PMB's five startup memory files are a materially different storage model. The useful principle is **topic-aware admission**, not copying the file layout.

**Disposition: MERGES WITH PMB Durable State / Authority / Projection research.**

## 7. Index maintenance is part of definition-of-done

Hindsight says a memory that is written but not represented in the index is effectively orphaned. The index must be updated whenever a new or newly relevant memory file is created.

Source: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)

### PMB connection

This supports treating discoverability as part of persistence correctness rather than an administrative afterthought.

But PMB should not automatically adopt an index layer. Our recent filing audit found that additional index/projection layers can themselves become stale and increase startup context. The rule should be tested against PMB's existing five-file model first.

**Disposition: SHARPENS; adoption blocked on PMB context-cost evidence.**

## 8. Hindsight explicitly frames memory as a token-saving investment

The README and GUIDE argue that a durable lesson prevents future sessions from spending tokens rediscovering a failed approach, correcting a repeated mistake, or re-litigating a settled question.

Source: [README.md](https://github.com/EfficientStreet/hindsight/blob/main/README.md)

### Important PMB refinement

This is a **hypothesis about amortized context cost**, not evidence that more memory is always cheaper.

For PMB, the correct question remains:

> Does the token cost of carrying and retrieving this durable lesson produce enough future savings to justify its startup/retrieval cost?

This aligns with our existing net-context-cost gate and argues against indiscriminate memory accumulation.

**Disposition: SHARPENS PMB context-cost research.**

## 9. User control is explicit because memory writes are consequential

The skill is deliberately `disable-model-invocation: true`. The user triggers the retrospective. The README explains that timing remains the user's call because the operation writes persistent memory.

Source: [SKILL.md](https://github.com/EfficientStreet/hindsight/blob/main/SKILL.md)

### Harness implication

This is a concrete example of **mutation authority being separated from analytical capability**.

The agent can be capable of identifying a lesson without being authorized to persist it whenever it wants.

That maps strongly to our broader principle:

> **Capability does not imply authority.**

**Disposition: SHARPENS capability/authority separation.**

## 10. It has an explicit memory mutation boundary

The scope boundary says Hindsight does not edit project code or content. Its only writes are to memory and the memory index.

Source: [SKILL.md](https://github.com/EfficientStreet/hindsight/blob/main/SKILL.md)

This is a useful example of a bounded skill contract:

- reads session history;
- reasons about lessons;
- writes only designated memory surfaces;
- does not silently expand into implementation.

**Disposition: SUPPORTING EVIDENCE for bounded skill authority.**

## 11. The research/fact-checking separation is excellent prior art

Hindsight's Step 3 verifies whether a lesson's concrete claims match the session evidence. Step 4 separately asks whether the event establishes a broader pattern. The guide calls out that these questions are easy to conflate.

This mirrors a broader Harness distinction:

```text
Observation → Claim verification → Pattern inference → Admission
```

rather than:

```text
Observation → Agent confidence → Memory
```

This should be preserved if PMB develops retrospective consolidation.

**Disposition: HIGH-VALUE.**

## 12. It has a deliberately small output contract

The final report is supposed to contain only retained lessons and practical effects, plus explicit watch-items. It should not reproduce the session.

Source: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)

This is valuable because retrospective output itself can become context pollution. The useful artifact is the admitted delta, not a second transcript.

**Disposition: SHARPENS context-cost discipline.**

## 13. Hindsight's seven-step process

Normalized from the source:

```text
1. Find genuine process failures
       ↓
2. Separate durable lessons from one-off noise
       ↓
3. Verify concrete claims against the session
       ↓
4. Classify proved vs suggested
       ↓
5. Merge into existing memory / create atomic topic
       ↓
6. Keep scope honest
       ↓
7. Report lessons + watch-items
```

Source: [GUIDE.md](https://github.com/EfficientStreet/hindsight/blob/main/GUIDE.md)

## 14. What Hindsight does NOT establish

The repository does not by itself establish that:

- retrospective LLM judgment is reliable enough to autonomously mutate a production memory bank;
- one recurrence is statistically sufficient to establish a durable pattern;
- atomic per-fact files are superior to PMB's structured memory files;
- automatic memory consolidation improves overall task outcomes after accounting for retrieval/startup cost;
- Hindsight's process is safe for unattended execution;
- a single model can reliably distinguish process lesson from domain-specific fact.

These are evaluation questions, not conclusions from the repository.

## 15. Candidate experiment for PMB

Do **not** install Hindsight wholesale.

Run a retrospective-consolidation experiment over a controlled sample of completed PMB sessions:

### Input

- full session transcript/history;
- existing PMB memory;
- durable decisions already recorded;
- known corrections/failed attempts;
- existing evidence/provenance where available.

### Candidate output

For each proposed lesson:

```text
candidate lesson
source event(s)
root cause
specific preventive behavior
proved | suggested
why
existing PMB topic match
startup/retrieval cost estimate
```

### Human admission

Require the user to approve any candidate that would alter canonical PMB memory until the experiment establishes a safer admission mechanism.

### Measure

- precision of retained lessons;
- duplicate rate;
- false generalization rate;
- useful lesson recall;
- later-session error reduction;
- retrieval frequency;
- startup-context cost;
- maintenance burden;
- contradiction rate;
- stale-memory rate.

This would turn Hindsight from an appealing skill into evidence about whether retrospective consolidation actually pays for itself in PMB.

## 16. Potential PMB architecture refinement

The most useful abstraction suggested by Hindsight is not "add a hindsight skill." It is:

```text
Session evidence
      ↓
Retrospective analysis
      ↓
Candidate lesson
      ↓
Evidence-strength classification
      ↓
Duplicate/topic resolution
      ↓
Human/governed admission
      ↓
Canonical PMB memory
      ↓
Future-session retrieval
```

The **candidate lesson** should remain separate from canonical memory until admission.

That directly reinforces the PMB Dream conclusion that automatic canonical mutation should not yet be adopted.

## Disposition summary

| Finding | Disposition |
|---|---|
| Retrospective memory as filter, not log | **ADOPT DESIGN PRINCIPLE** |
| Root-cause compression | **HIGH-VALUE CANDIDATE** |
| Proved vs suggested | **PRIORITY RESEARCH** |
| Concrete fact verification vs pattern inference | **ADOPT DESIGN PRINCIPLE** |
| Existing-topic merge | **SHARPENS existing filing/authority work** |
| User-triggered persistent mutation | **ADOPT BOUNDARY PRINCIPLE** |
| Read-only against actual work | **ADOPT BOUNDARY PRINCIPLE** |
| Environment finding vs process lesson | **SHARPENS information taxonomy** |
| Index maintenance as persistence correctness | **RESEARCH; context-cost constrained** |
| Token-saving claim | **RESEARCH HYPOTHESIS** |
| Automatic canonical memory mutation | **DO NOT ADOPT YET** |
| Hindsight skill itself | **EXPERIMENT, not core PMB architecture** |

## Bottom line

Hindsight is a **strong prior-art contribution to PMB retrospective consolidation**.

Its most valuable idea is not the skill file or Claude Code integration. It is the epistemic gate between **"this happened"** and **"this should become a rule."**

The key lesson for Harness Engineering is:

> **Memory admission should distinguish verified observations from justified generalizations. A fact can be true without the broader lesson being proved.**

That gives PMB a potentially rigorous path from session history to durable learning without turning every retrospective into automatic self-modification.
