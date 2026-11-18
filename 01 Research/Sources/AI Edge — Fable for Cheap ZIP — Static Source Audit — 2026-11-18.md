# AI Edge — Fable for Cheap ZIP — Static Source Audit — 2026-11-18

**Disposition:** **DO NOT INSTALL UNCHANGED.** Source-level review of the user-supplied original archive; not an operational test or a security certification. Retain in [AI Edge PR #35](https://github.com/unyieldingclaw-dev/harness-engineering/pull/35), **draft / review only**. No changes to HE standards, runtime, PMB, ACR, user or work configuration.

## Evidence and method

- Supplied file: `fable-for-cheap.zip`, **SHA-256 `0b11dd696190a6120e3888b3433cdc9730a47f8619ef9884d820b2252eaeeb7f`**, 12,199 bytes, 10 ZIP entries (7 regular files plus 3 directories), 26,129 uncompressed bytes.
- Inspected **all seven** text files: `README.md`; `SKILL.md`; `scripts/install.sh` (144 lines); `scripts/spend.js` (274 lines); `files/Explore.md`; `files/fable-reviewer.md`; `files/claude-md-section.md`.
- ZIP CRC passed; no ZIP traversal filenames or ZIP symlink entries; shell and JavaScript **syntax parsing** passed (`bash -n`, `node --check` via stdin). These checks prove **only basic structure/syntax**, not correct execution.
- **Did not install, invoke the Claude skill, run `install.sh` or `spend.js`, open the user's Claude transcripts, connect to providers, alter any settings, or execute any downloaded code.**
- We have not verified file authorship, a release signature, upstream code history, a test suite or behavior under the user's installed Claude Code version. Absence of an obvious network call in these files is **not** an audit of the Claude agent or extension environment.

## Exact operation, by file (observed)

| File | Observed behavior |
| --- | --- |
| `SKILL.md` lines 1–7 | Manual slash skill (`disable-model-invocation: true`); grants `Read, Write, Edit, Grep, Glob` plus selected Bash commands using `allowed-tools`; invokes setup/report/audit/remove through instructions to the model |
| `SKILL.md` 15–29 | `setup` delegates to `bash .../scripts/install.sh install`, with a fallback instruction for the model to **modify the same files by hand** if Bash fails; may separately offer to edit `settings.json` when effort is `xhigh`/`max` (approval requested) |
| `install.sh` 6–10, 53–103 | Resolves `CLAUDE_CONFIG_DIR` or `~/.claude`; copies `Explore.md` and `fable-reviewer.md` into the **user-global** `agents/` directory; appends or replaces a marked rules block in **user-global** `CLAUDE.md`; backs up existing differing files before writes |
| `files/Explore.md` 1–11 | Named `Explore`, `model: haiku`, `effort: low`, tools `Read, Grep, Glob`. Prompt instructs read-only retrieval with paths/line numbers |
| `files/fable-reviewer.md` 1–11 | Named `fable-reviewer`, `model: fable`, `effort: medium`, tools `Read, Grep, Glob, Bash`; prompt *says* never write, but may run arbitrary project tests/checks through Bash |
| `files/claude-md-section.md` 1–10 | Global standing instructions: default main-session work, cheap subagents for separated/simple verification, code edits/reviews in main conversation; Fable reviewer by explicit request |
| `scripts/spend.js` 9–274 | Uses Node built-in `fs,path,os` only; recursively reads `~/.claude/projects/**/*.jsonl`; sums usage metadata and prints by-model/project/helper estimates and hypothetical single-model repricing. No network or file-write API observed in this script |
| `install.sh` 106–143 | `remove` deletes two agent files only if byte-identical to bundled versions; strips global rules block; makes a `CLAUDE.md` backup; leaves modified agent definitions untouched |

## Confirmed code-level findings

### F1 — HIGH: remove cannot restore preexisting agent definitions / skill remains installed

- `install.sh` 57–69 overwrites an existing `agents/Explore.md` or `agents/fable-reviewer.md` after placing a `.bak-<timestamp>` copy nearby.
- `install.sh` 106–118 removes the new file when it matches the ZIP's version; **it never restores the preexisting agent from the backup**. In the edited-file case, it leaves the changed file and likewise does not restore the original.
- `remove` does not remove `~/.claude/skills/fable-for-cheap/` itself. `README.md` line 23 says undo “everything”; that is not implemented. Global `CLAUDE.md` is stripped, not restored byte-for-byte.
- **Risk:** an existing user-owned `Explore` agent can be displaced indefinitely, despite the marketing promise of reversible setup. Original files remain in timestamped backups, but recovery requires manual work and proper conflict resolution.
- **Smallest correction:** do not overwrite any preexisting agent automatically; refuse on collision or use a unique agent name. For any explicit replacement, maintain a manifest with original existence/hash/path and test an exact-match guarded restoration. Uninstall should also document/manual-remove the skill folder. Avoid a sweeping installer rewrite unless adopting this package.

### F2 — HIGH: malformed rules markers can truncate unrelated CLAUDE.md content

- `install.sh` 31–38 checks **only for a start marker**. `strip_block` 43–50 drops every line from a start marker until an end marker; if the end marker is absent, it **continues dropping to EOF**.
- Both `install` (line 80) and `remove` (line 123) call `strip_block` without validating balanced unique markers. A stale/partially edited marker therefore removes all subsequent lines; backups reduce, but do not prevent, damage.
- `strip_block` also deletes any unmatched end markers and trims trailing blank lines from the complete document; it is not a precise textual inverse.
- **Smallest correction:** refuse to mutate unless marker cardinality/order are valid; allow zero blocks for first install, exactly one balanced pair for replacement/removal; write a new temp file and atomically replace only after validation. Test unmatched, duplicated, nested and modified-marker fixtures.

### F3 — HIGH: audit instruction contains conflicting mutation and approval sequence

- `SKILL.md` Audit line 47 says **“Don't change any files until the user approves.”** Line 57 then instructs **“Rewrite the three items that need it most”**, and line 58 only then says **“stop and ask which fixes to make.”**
- This is an instruction-level contradiction; a language model may interpret step 57 as permission to change files before step 58.
- `SKILL.md` line 6 pre-approves broad `Write` and `Edit`, so the “approval” language is not an enforceable restriction.
- **Smallest correction:** **propose** exact redlined drafts/diffs first; explicitly defer all disk writes until an affirmative user selection; keep audit mode read-only at the tool-authorization layer when practical.

### F4 — MEDIUM: “read-only Fable reviewer” is not tool-enforced

- `files/fable-reviewer.md` 4–10 grants `Bash` and tells the reviewer to run tests; it also says “never create, edit or delete files.”
- Bash tests can modify artifacts, invoke hooks, access network or carry side effects. The instruction does not restrict Bash permissions; the installed Claude permissions and project scripts determine actual effects.
- **Smallest correction:** use `Read, Grep, Glob` for a true read-only review; if tests are necessary, require specifically authorized bounded test commands or a disposable sandbox and surface test side effects.

### F5 — MEDIUM: global routing instruction is advice, not guaranteed behavior

- `files/claude-md-section.md` 4–9 says every helper should be cheap and Fable only by explicit request. The installer copies **two named subagents**; it does not set a universal enforceable model/effort policy for all built-ins, custom agents, plugins or direct invocations.
- A custom `Explore` can reduce search costs **only when chosen** and when higher-precedence project/managed definitions or invocation overrides do not change the effective model. Existing `Explore` is overwritten on user scope but may lose to a project-scoped version. The custom `Explore` may load additional `CLAUDE.md` context that the built-in Explore intentionally omits.
- **Smallest correction:** state this as opt-in heuristic with runtime verification; compare actual effective model and results on real tasks. Prefer project-scoped isolated trial to a user-global overwrite.

### F6 — MEDIUM: spend totals are approximate and have concrete counting edge cases

- `scripts/spend.js` lines 16–21 use case-insensitive **family regexes**, labeling every matched Opus/Sonnet/Haiku version as 5.5 and using one fixed price per family (not version/provider/long-context-aware). Unrecognized models are counted as **$0** (112–118; 265–267), which can bias totals.
- Transcript dedup is on `o.message.id` alone and takes the **first occurrence** (98–107), even where successive rows have the same ID and different usage snapshots; no final/maximum-usage reconciliation.
- A row with a missing/unparsable timestamp gets `when=0` and **passes** the 7-day cutoff (109–110).
- `repriceAll` (148–156; 255–260) applies **the same observed token counts** to a different model. It does not run that model, know its tokenization/tool behavior/quality, or include corrections and retries. Not a valid counterfactual savings estimate.
- `CLAUDE_CONFIG_DIR` changes the transcript scan root; sessions outside `projects/` or logged differently are not included. Reads whole JSONL files synchronously, which could be expensive for long histories.
- **Smallest correction:** label the output `estimated API-equivalent transcript cost`; map exact model IDs + pricing tiers; flag unknown and malformed data distinctly instead of $0; deduplicate carefully; report unpriced proportion; validate time-window logic; remove or strongly qualify the theoretical “same work” table until real A/B evidence exists. For subscriptions, neither the printed dollar values nor ratios directly measure weekly limits.

### F7 — LOW: cross-platform and symlink edge cases are undocumented

- The entrypoint is `bash` and installation’s fallback asks Claude to imitate shell mutations manually, which has different correctness and safety properties on Windows; neither is verified on the user's machine.
- `cp`, redirection and file deletion do not inspect symlink targets; a preexisting symlink at `CLAUDE.md` or either agent file changes which underlying data is touched. Source code is not currently written to handle or reject this explicitly.
- **Smallest correction:** make supported shells/platforms explicit, fail closed on unhandled filesystem types, and never silently substitute model-generated writes for failed deterministic installation.

## What is **not** evidenced as malicious by this inspection

- No obvious `curl`, `wget`, network client, credential-upload code, remote script fetch, `rm -rf`, or shell command substitution to obtain secrets exists in the bundled *executable* installer and reporter.
- The reporter reads local transcript JSONL and emits summarized stdout; no direct outbound request appears. **However**, invoking the skill inside Claude sends the report/audit results to that model session, and `audit` asks the remote model to read user, project and local Claude configuration. Review such contents for secrets or corporate information before use.
- We did not investigate the author or software supply chain, run functional security tests, or inspect the user's actual Claude configuration. “No obvious exfiltration in these scripts” **does not mean safe to install**.

## First-party behavior consulted (point-in-time)

- [Claude Code — custom subagents](https://code.claude.com/docs/en/sub-agents): project and managed subagent definitions have higher priority than a `~/.claude/agents/` user definition; effort/model invocation controls can override file defaults; built-in Explore model is provider-dependent.
- [Claude Code — skills](https://code.claude.com/docs/en/skills): `allowed-tools` **pre-approves** listed tools for the invoking turn; it is **not a deny-only sandbox** or an implementation of human review gates. `disable-model-invocation: true` limits automatic skill invocation, but not independent invocations or all agents' behavior.
- [Claude Help — Models, usage, and limits in Claude Code](https://support.claude.com/en/articles/14552983-models-usage-and-limits-in-claude-code): model selection and usage accounting depend on login/plan; API and subscription metering differ.

## HE decision and measured experiment gate

**Do not adopt the installer, the global `CLAUDE.md` rules or its report as a cost/usage source of truth.** The more defensible principle remains explicit model/effort choice for *bounded, verifiable tasks*, followed by real-task comparison that records success, time, quota/usage signals, fallback and rework.

No automatic skill or global rules are needed for a preliminary test. On a **personal non-work task only**, the user may manually trial one isolated lower-tier read-only subagent with unique name and explicit model/effort, while retaining existing rules and a baseline. Begin only if a repeated operational pain point is observed and model selection can be verified. Stop if the added agent/rules cost more to maintain than they demonstrably save.

**Status:** static source-reviewed, **no functional run performed**, installation **not approved**; HE source research only. No PMB, ACR, Dashboard or work-system changes.
