# External Artifact Manifest

## Purpose

Index downloadable third-party research artifacts that were actually supplied to and inspected for Harness Engineering.

This file is **not** an artifact archive. It records enough provenance to identify a future copy and points to the HE source note that contains the durable research findings.

Default policy: if redistribution rights are unclear, preserve **filename + source + retrieval date + SHA-256 + contents inventory + mined findings**, but do not commit the original binary into HE.

See:

- `01 Research/Source Preservation & Evidence Durability — 2026-09-16.md`

---

# 2026-09-16 — Ben AI Token Optimization skills

Source page:

https://benai.notion.site/Token-Optimization-3d71124570fe8096bef6e530c782a10d

Associated video:

https://www.youtube.com/watch?v=Jr-jyTL2MYI

Primary HE evidence record:

`01 Research/Sources/Ben AI — Token Optimization, Refresh & Prompt Master — 2026-09-16.md`

Preservation status for all three bundles:

- supplied directly by the user after downloading from the source page;
- inspected statically; not installed or executed;
- no LICENSE / LICENCE / COPYING / NOTICE file was present inside the supplied ZIPs;
- original third-party ZIPs are therefore **not vendored into HE**;
- redistribution/license status is treated as unclear;
- hashes below identify the exact copies reviewed.

## `refresh-skill.zip`

SHA-256:

`fbfa45e6bb0e47c3eaffa4088d80d58e7fef2ef86d9be7dc5ac2624008fff958`

Archive inventory:

```text
refresh/
refresh/SKILL.md       9347 bytes
```

Material findings mined:

- next-session-goal-oriented handoff;
- scans current conversation and touched files;
- `lite`, `full`, and `ultra` transfer modes;
- checks whether referenced files/data will be reachable in the successor session;
- distinguishes local/persistent/ephemeral state;
- can preserve dead ends in higher-fidelity transfer;
- contains write/copy behavior that should not be imported into PMB without explicit authority.

## `interview-me.zip`

SHA-256:

`c242c4df3d3113ea877bc322279711b6fde790068a43cd0aa83460a2f0edad1c`

Archive inventory:

```text
interview-me/
interview-me/SKILL.md  2863 bytes
```

Material findings mined:

- read existing context before asking questions;
- ask one question at a time;
- bounded interview length;
- prioritize questions that materially change scope, architecture, or proof of done;
- useful distinction between user-owned decisions and routine implementation details;
- delegation language grants more authority than HE should adopt by default.

## `prompt-master.zip`

SHA-256:

`746cfc9693abc419a3219f8fa715e06e4acaf11bbedad6ad668858ccaf46b000`

Archive inventory:

```text
prompt-master/
prompt-master/SKILL.md                                      5378 bytes
prompt-master/EVAL.md                                       1259 bytes
prompt-master/references/rules-to-reasons.md                 2025 bytes
prompt-master/references/file-audit.md                       2169 bytes
prompt-master/references/job-brief.md                        3229 bytes
prompt-master/references/interview.md                        2500 bytes
prompt-master/references/voice-and-format.md                 2464 bytes
prompt-master/references/retired-instructions.md             3102 bytes
prompt-master/references/rulebook.md                         3762 bytes
prompt-master/references/examples/client-report-rewrite.md   2905 bytes
```

Material findings mined:

- converts rough requests into bounded job/why/guardrails/done-means briefs;
- favors rules with reasons over unexplained emphasis;
- contains useful scope/deliverable/report-back caps;
- retains strong rules when the consequence of failure is expensive;
- contains evidence-oriented progress/reporting guidance;
- overgeneralizes model-specific advice about redundant self-verification;
- contains self-modification behavior based on user correction/praise that HE should not adopt without stronger governance;
- provided eval is useful for instruction-conformance checks but is not independent evidence of downstream task quality.

---

# Recovery / rediscovery procedure

If one of these files is found later under a different name or location:

1. compute SHA-256;
2. compare against this manifest;
3. if the hash matches, it is byte-for-byte the artifact reviewed on 2026-09-16;
4. if it does not match, treat it as a new version and review the delta before replacing prior conclusions.

If the original download page disappears, the HE research remains usable through the source note and this manifest even though the original binary is not redistributed here.
