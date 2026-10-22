# Source Preservation & Evidence Durability — 2026-09-16

## Purpose

Harness Engineering depends on external evidence that may later disappear, move, change, require authentication, or become difficult to rediscover. Research is only durable if a future reader can still understand what was reviewed and why a conclusion was reached after the original link stops working.

This document defines a lightweight preservation approach for HE research. It is intentionally narrower than building a general archival system.

---

# Principle

> **Preserve the evidence trail, not necessarily the entire external artifact.**

A source note should remain useful even if the original URL disappears.

For every source that materially affects HE research, preserve enough locally to answer:

- What was reviewed?
- When was it reviewed?
- Where did it come from?
- Which exact artifact/version was inspected when that matters?
- What claims or mechanisms were actually observed?
- Which HE conclusions depended on it?
- Can a future reviewer distinguish source evidence from HE interpretation?

Do not assume that a URL is durable evidence by itself.

---

# Minimum preservation record

For material external research, record in `01 Research/Sources/`:

1. **Source identity**
   - title;
   - author/project;
   - canonical URL when known;
   - relevant repository URL when applicable.

2. **Retrieval/review date**
   - use an explicit date rather than `today` or `recently`.

3. **Artifact identity when the exact bytes matter**
   - original filename;
   - SHA-256 or equivalent content hash;
   - version/tag/commit when available;
   - archive contents or important filenames for ZIP/bundle sources.

4. **Observed evidence**
   - concise description of the implementation, behavior, claim, issue, or experiment actually inspected;
   - file/function/issue pointers when source code is involved;
   - short excerpts only when wording itself is material.

5. **HE interpretation**
   - clearly separate inference/recommendation from source-derived fact;
   - record MINE / ASSESS / PARK / REJECT disposition where useful.

6. **Preservation status**
   - live URL only;
   - source-derived note preserved;
   - exact version pinned by Git commit/tag;
   - local user-held artifact fingerprinted;
   - vendored/archived copy permitted and retained;
   - redistribution/license unknown.

This is enough for most HE research. Do not create an archival subsystem merely because some links may disappear.

---

# GitHub sources

GitHub sources are comparatively easy to make durable.

Prefer, in order:

1. repository + exact commit SHA;
2. file path + commit/tag;
3. issue/PR number + retrieval date;
4. default-branch URL only when version identity is unimportant.

When implementation details materially support an HE conclusion, record the exact file/function or issue and, when practical, the commit/tag observed.

Do not copy entire third-party repositories into HE simply to protect against link rot.

---

# Web pages, Notion pages, videos, and transient documentation

These sources are more vulnerable to deletion or silent editing.

For material findings:

- preserve a source note in HE containing the claims/mechanisms actually used;
- record the URL and review date;
- preserve short excerpts only when exact wording matters;
- keep screenshots only when a visual detail is evidentiary and cannot be described reliably in text;
- prefer first-party documentation over a creator's paraphrase when verifying product behavior.

A copied marketing page is not automatically better evidence than a concise researched note with first-party corroboration.

---

# Downloaded ZIPs, skills, and other external bundles

Downloaded artifacts create two separate problems:

1. **future rediscovery** — the user may forget the filename/source;
2. **redistribution rights** — the artifact may not be licensed for republication in the HE repository.

Therefore:

## Default handling

For a third-party ZIP or bundle with no clear redistribution license:

- do **not** commit the binary into HE;
- record its original filename;
- record where it came from;
- record the retrieval date;
- compute and record SHA-256;
- record an inventory of important contained files;
- mine the relevant behavior into an HE source note;
- mark the original as **user-held external artifact — not vendored; redistribution/license unclear**.

This lets a future reviewer identify a surviving copy unambiguously and preserves the research even if the download link dies.

## When vendoring is appropriate

A full copy may be retained in-repo only when there is a concrete reason and redistribution is clearly permitted by the source license or explicit permission.

If vendored:

- preserve the original license/notice;
- keep the artifact under a clearly identified third-party/archive path rather than mixing it with HE implementation;
- record the original source and hash;
- do not silently modify the archived copy.

## Why not commit every ZIP

Blindly retaining every downloaded artifact creates:

- repository bloat;
- supply-chain ambiguity;
- accidental execution risk;
- unclear provenance;
- copyright/license problems;
- a second unmanaged software collection inside a research repository.

The useful default is **fingerprint + inventory + mined evidence**, not binary hoarding.

---

# External Artifact Manifest

When HE receives downloadable source bundles, add them to:

`01 Research/Sources/External Artifact Manifest.md`

The manifest is an index, not an archive. It exists so a future session can answer questions such as:

- "What were those three Ben AI ZIP files?"
- "Did we actually inspect them?"
- "Which one contained the self-editing behavior?"
- "Is this ZIP I found later the same artifact we reviewed?"

The source-specific research note remains the authoritative interpretation.

---

# Dead-link behavior

If a previously cited URL stops working:

1. do not delete the research finding solely because the link is dead;
2. mark the source as unavailable/dead and record the date observed;
3. retain the prior retrieval date, artifact hash, and mined evidence;
4. look for an authoritative replacement, repository copy, release/tag, Internet Archive snapshot, or first-party equivalent when needed;
5. distinguish replacement evidence from the originally reviewed artifact.

A dead link lowers reproducibility, but it does not erase evidence that was previously inspected and fingerprinted.

---

# Claims that need stronger preservation

Use stronger source identity when a finding is:

- load-bearing for an HE principle or decision;
- likely to be contested;
- based on code behavior rather than general guidance;
- security/governance related;
- version-sensitive;
- contradicted by another source;
- based on a downloadable artifact that may disappear.

Low-impact background reading does not require the same preservation effort.

---

# Relationship to verification

Source preservation is an evidence problem, not a documentation-completeness ritual.

The goal is not to collect everything. The goal is to make important claims auditable later.

This aligns with existing HE research:

- verification should produce relevant evidence;
- provenance should be minimal but discriminating;
- context should be intentional;
- more stored material is not automatically more useful;
- deterministic identifiers such as commit SHAs and file hashes are stronger than memory or filenames alone.

---

# Research disposition

- **REINFORCE:** Source notes must remain useful after link rot.
- **REINFORCE:** Fingerprint downloaded artifacts when exact identity matters.
- **REINFORCE:** Preserve concise evidence and provenance rather than entire external corpora by default.
- **REINFORCE:** Pin GitHub implementation evidence to commits/tags when version sensitivity matters.
- **ASSESS:** Whether future high-value sources justify a dedicated archival location or private artifact store.
- **PARK:** Automated web archiving, mirror infrastructure, or a full research database until source loss becomes an observed operational problem.
- **REJECT:** Treating a live URL as sufficient durable evidence.
- **REJECT:** Committing unlicensed third-party ZIPs into HE merely so they are not forgotten.
