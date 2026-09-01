# Austin Marchese — Claude Loop Engineering & Skills — 2026-09-01

## Source

- Creator: Austin Marchese
- Videos reviewed:
  - [Stop Prompting Claude. Start Loop Engineering](https://www.youtube.com/watch?v=YAS4ojuhbW4)
  - [The ONLY 6 Skills You Need to 10x Your Claude Projects](https://www.youtube.com/watch?v=AfKoqFwC7Ew)
- Additional source material: screenshots and transcript excerpts supplied in the research session on 2026-09-01.
- Related site mentioned by the creator: BuildPartner.ai.

## Source-derived concepts captured

### Loop engineering

A loop is presented as a recurring execution structure in which an AI performs a task, output is evaluated against a goal/done rule, and the agent iterates until the output passes. The creator emphasizes that the hard part is not the loop mechanism; it is creating a meaningful verification process.

The four blocks explicitly described are:

1. Trigger — when the loop should run.
2. Execution skills — the reusable capability used to perform the work.
3. Goal and verification — what done means and how the output is judged.
4. Output and memory — the artifact produced plus durable information about what happened.

The creator also describes a fifth practical component in the broader loop-engineering workflow: separate verification, preferably using a different agent/context from the agent that produced the work.

### Verification

The source repeatedly distinguishes a task from a verifiable task. Suggested verifier forms include:

- approved / not approved;
- a numeric score such as 1–10;
- a concrete done-rule;
- a separate subagent evaluating the produced output.

For less-quantifiable work, the creator recommends smaller goals and human verification checkpoints at key decision points where choosing the wrong direction would invalidate later work.

### Output and memory

The creator's central memory claim is that each loop starts from scratch unless results are recorded. The suggested implementation is deliberately simple: write the loop's output and a memory/run-history artifact to disk. The memory should capture what happened, what worked, what failed, and what should be remembered next run.

### Loop Training Mode

The screenshots show a proposed `Loop Training Mode` with two states:

- ON: pause at each step for human approval; skip steps that already pass their done-rule; retry only failures; cap retries.
- OFF: run autonomously without approval pauses while retaining done-rule checks and retry caps.

The suggested workflow is to begin with training mode ON, observe repeated successful runs, then turn it OFF while retaining verification and retry boundaries.

### Skill discovery / saved skills

The creator recommends auditing repeated work and identifying tasks that would benefit from a saved skill. The proposed skill file records the task, preferences, goals, steps, and definition of done so a loop can invoke the capability consistently.

### `improve-system`

The proposed `improve-system` skill is a routed system-improvement capability with five modes:

1. Audit — find stale, conflicting, or duplicate notes.
2. Skill Review — improve a skill based on recent back-and-forth and experience.
3. Experience — capture a story, win, failure, or lesson just shared.
4. Historical Review — mine recent Claude Code sessions for missed learning.
5. Foundation — identify missing foundational information needed by the system.

The source describes using recent experience and historical sessions to sharpen the knowledge/skills available to future runs.

### `/ask-the-board`

The proposed board-of-advisors pattern is:

1. Identify experts relevant to the domain.
2. Ingest their public material into a project knowledge base.
3. Represent each expert as a separate perspective/agent.
4. Ask one expert or the whole panel a question.
5. Return each perspective and synthesize agreements/disagreements into a recommendation.

The creator explicitly biases toward experts with extensive public material because that provides more source material for the perspective.

### `/internal-focus-group`

The proposed focus-group skill creates separate personas/agents with their own voice, lens, and history. Members can be queried individually or in parallel as a panel. Their responses are synthesized with disagreements preserved rather than averaged away.

The screenshots show the intended implementation pattern as a directory of person-specific source material that the skill automatically discovers.

### Web and ingestion skills

The six-skill workflow includes:

- `/web-scraping` — semantic search / web retrieval, with Firecrawl suggested for JavaScript-heavy pages.
- `/ingest-source` / `/ingest-resource` — normalize articles, YouTube links/transcripts, PDFs, notes, and other sources into a knowledge/project structure; add source/date/key-concept metadata; create folders; cross-link related notes.
- `/improve-system` — compound learning into the system.
- `/ask-the-board` — expert perspective and synthesis.
- `/internal-focus-group` — audience/expert pressure test before shipping.
- Compound Engineering — planning, execution, review, debugging, and codifying lessons so future work becomes easier.

### Compound engineering

The source presents a simple operating philosophy: each unit of engineering work should make subsequent units easier rather than harder. The demonstrated skills are:

- `/ce-brainstorm`
- `/ce-plan`
- `/ce-work`
- `/ce-code-review`
- `/ce-debug`

The important idea is not the names or plugin. It is the explicit sequence of planning, defining done, execution, review/debugging, completion, and capturing reusable knowledge.

## Important source limitations

- The material is a creator's methodology and demonstrations, not controlled comparative evidence.
- Persona/board outputs are model-generated interpretations of source material, not the actual experts.
- A second model or subagent is not automatically an independent oracle; common-model bias remains possible.
- A successful loop requires a trustworthy done-rule. A loop without a meaningful verifier can automate repetition rather than correctness.
- The screenshots demonstrate proposed mechanisms but do not establish their effectiveness for the user's PMB or ACR systems.
