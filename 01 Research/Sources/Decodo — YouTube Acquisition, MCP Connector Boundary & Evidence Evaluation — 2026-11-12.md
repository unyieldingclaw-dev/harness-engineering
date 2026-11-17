# Decodo — YouTube Acquisition, MCP Connector Boundary & Evidence Evaluation — 2026-11-12

**Status:** ASSESS — research-only candidate for externally sourced, public YouTube evidence. **Not installed, not tested against live YouTube, not adopted as HE/PMB/ACR policy or a Dashboard dependency.**

**User source:** Rob The AI Guy, [“This AI Agent Can Search & Scrape the Entire Web (crazy results)”](https://www.youtube.com/watch?v=rd-jo8fENfw), plus user-supplied screenshots and description (2026-11-11). This video demonstrates a Decodo-to-Claude scraping workflow and advertises search, scrape, map and crawl plus a free tier. The **provided material does not demonstrate a controlled YouTube-subtitle extraction experiment**, success rate, end-to-end fidelity or low operating cost. Treat creator and vendor claims as discovery prompts, not acceptance evidence.

**Source verification date:** 2026-11-12. Official [Decodo MCP source](https://github.com/Decodo/mcp-server/tree/c447c89580f1a8038b7ff82678e8f003417361fe) (pinned public source snapshot); [README](https://github.com/Decodo/mcp-server/blob/c447c89580f1a8038b7ff82678e8f003417361fe/README.md); [YouTube subtitles API docs](https://help.decodo.com/docs/web-scraping-api-youtube-subtitles); [Decodo billing details](https://help.decodo.com/docs/web-data-api-billing); [Web Data API offer](https://decodo.com/web-data-api); [yt-dlp official options](https://github.com/yt-dlp/yt-dlp/blob/master/README.md); [yt-dlp YouTube subtitle 429 discussion](https://github.com/yt-dlp/yt-dlp/issues/13831). Source and pricing may change; recheck when testing.

## Executive disposition

There is **a plausible, specific use case**: video-transcript and metadata acquisition for source-grounded HE research when native YouTube captions, human-supplied transcripts or a small local `yt-dlp` workflow are unavailable/incomplete. This could reduce the friction of retrieving a video’s spoken claims, **not** automatically improve technical verification or provide visual evidence.

The demonstrated pain point is occasional difficulty accessing complete video transcripts during research; the comparative reliability and cost benefit of Decodo **has not been measured**. Do not add a standing agent integration, tool router, transcript archive, crawler, scheduled task or knowledge graph for this.

## Verified API and MCP facts

### Four dedicated YouTube tools (implemented in open-source MCP server)

| MCP tool | Official source contract | Boundary |
| --- | --- | --- |
| [`youtube_subtitles`](https://github.com/Decodo/mcp-server/blob/c447c89580f1a8038b7ff82678e8f003417361fe/src/tools/youtube-subtitles/youtube-subtitles-tool.ts) | Accepts YouTube **video ID** as `query`, optional `language_code`, invokes the `YoutubeSubtitles` API target and returns JSON text | Provides captions, not video frames or verified speech transcription from all videos |
| [`youtube_metadata`](https://github.com/Decodo/mcp-server/blob/c447c89580f1a8038b7ff82678e8f003417361fe/src/tools/youtube-metadata/youtube-metadata-tool.ts) | Accepts video ID, calls `YoutubeMetadata`, returns JSON | Metadata/description cannot establish the accuracy of technical claims |
| [`youtube_channel`](https://github.com/Decodo/mcp-server/blob/c447c89580f1a8038b7ff82678e8f003417361fe/src/tools/youtube-channel/youtube-channel-tool.ts) | Accepts channel handle or ID and optional result `limit`, calls `YoutubeChannel`, returns parsed JSON | Channel listing is discovery; publication dates/completeness need field-level validation |
| [`youtube_search`](https://github.com/Decodo/mcp-server/blob/c447c89580f1a8038b7ff82678e8f003417361fe/src/tools/youtube-search/youtube-search-tool.ts) | Accepts search query, calls `YoutubeSearch`, returns JSON | Search results aren't a complete corpus, nor evidence a video is correct |

All four are in the MCP server's `social_media` toolset and are annotated `readOnlyHint`; that hint expresses intent and is **not a security boundary**.

### Important connector distinction — correction to earlier assumptions

The **official Decodo connector in Claude’s connector directory exposes only** `web` (`scrape_as_markdown`, `screenshot`, `map`, and crawl operations) and `search` (`search`). It **does not expose the four dedicated YouTube tools** merely because the connector appears connected. The **general MCP server** exposes `social_media` (YouTube and TikTok). Some MCP clients can select toolsets (e.g. `?toolsets=social_media` on hosted MCP, or client-specific `TOOLSETS` for the local server). **Confirm the exact client, permissions, and supported connector mode before claiming Claude integration works.** Do not confuse Decodo's general `/scrape` tool with the dedicated `youtube_subtitles` target.

The MCP wrapper currently exposes only `query` and `language_code` for subtitles, whereas the **direct YouTube subtitles API** documents `subtitle_origin=uploader_provided|auto_generated` with `auto_generated` default. That difference matters when measuring caption provenance and choosing authoritative captions. A future test may use the direct API for that case, not assume the MCP wrapper permits all target parameters.

### Caption and evidence shape

The official subtitles API accepts an 11-character video ID, optional language code, and caption origin; it returns JSON with subtitle segments. The published example includes `events[].tStartMs`, `dDurationMs` and `segs[].utf8`. Preserve this timing structure for quote validation, navigation and visual-frame lookup. A language mismatch can return HTTP 404. The published sample illustrates a possible output **shape**, not a demonstrated result for any user-supplied video.

A transcript is **spoken or captioned content only**. It cannot verify demonstrations, UI state, printed code, benchmark settings, architecture diagrams, timestamps of visual evidence or the truth of a presenter’s claim. Visually important videos still require actual frames/screenshots and, where appropriate, source-code and documentation checks. Automated/translated captions may mis-hear technical names and version strings.

### Pricing, free tier and limitations

Decodo's product page advertises a free Web Data API tier with **up to 10,000 requests per month** and **$5 free credit**, without a card, plus paid tiers. This is an **advertised ceiling**, not proof that every API mode or request consumes exactly one unit at the cheapest rate. The official detailed billing guide says `/scrape` price varies with `proxy_pool` and `headless`, and defaults to premium proxy if omitted; `/map` may charge two retrievals and `/crawl` charges pages, not just one job. Vendor landing-page “no multipliers” language must not override the actual billing table. Record real credits consumed by the YouTube target before estimating how many videos the free tier supports. Success/failure charges and retries should be confirmed from dashboard records, not assumed. **The 99.99% success figure is promotional, not independently verified on YouTube subtitle retrieval.**

The MCP README identifies a `tokenLimit` / return-size parameter (described as a maximum number of **characters**, default 45,000), but the dedicated YouTube tools return JSON text directly. Do not assume generic Markdown truncation and the YouTube JSON pathway behave identically; check for missing tail segments and compare to native captions.

## Minimum realistic alternatives

| Path | Benefit | Limitation / next step |
| --- | --- | --- |
| YouTube's own “Show transcript” / supplied transcript | No additional vendor credentials; direct human check | May be absent, incomplete, awkward to extract or poorly timestamped |
| Local [`yt-dlp`](https://github.com/yt-dlp/yt-dlp) | Free/local tool with `--list-subs`, `--write-subs`, `--write-auto-subs`, `--skip-download`, `--sub-langs` | YouTube failures/429 reported; may require maintenance; do not assume all videos permit captions |
| Decodo YouTube API/MCP | Vendor-hosted extraction, structured metadata/subtitles, potential reliability for difficult cases | Third-party processing, credentials, billing, unknown success rate, toolset/client mismatch; must benchmark |
| Audio transcription + visual evidence | May cover videos lacking usable subtitles | Separate permission, media-access, compute/cost and technical QA path; not a capability proven by the Decodo subtitle tool |

**Preference:** Use existing native/manual captions or a local read-only tool first. Test Decodo only when the baseline fails often enough to justify another remote service.

## Bounded evaluation plan — **NOT RUN**

**Decision hypothesis:** For an explicitly chosen set of public technical videos that matter to HE, Decodo can retrieve **more complete, timestamped, correctly attributed caption evidence** than the simplest available alternative, at acceptable per-video cost and without exporting private/project information.

1. Select **3–5 public videos**: at least one with uploader captions, one auto-captioned, one with captions unavailable or a language mismatch, one long technical video with diagrams/code visible, and one representative “previously hard to retrieve” case. Freeze video URLs/IDs and retrieval date before testing.
2. Record baseline from YouTube's own transcript display and local `yt-dlp` where allowed. No hidden account automation or personal cookies by default.
3. Independently call `youtube_metadata` and `youtube_subtitles` for supported cases. If provenance selection is required, try the direct API with explicit `subtitle_origin`, and **record that this is a different interface**. Do not quietly substitute generic `scrape_as_markdown` for a transcript target.
4. Compare **completeness** (beginning/middle/end versus native captions; truncation; missing sections), caption source/language, correctness of video ID/title/channel, preserved timing, handling of missing captions/404, elapsed time, number of charged calls/credits, and manual repairs. Include a deliberately visually-dependent question that must be labelled **“not established by transcript.”**
5. Check one or two important claims in each transcript against original video frames plus upstream official documentation/code. Mark each observation **stated / visually observed / upstream verified / disputed / unknown**. Never call a summary a source-verified fact.
6. Test redaction and configuration hygiene: no secrets, private URLs, workplace identifiers, prompts or logs sent to Decodo; no ambient global MCP registration, unnecessary toolsets, autonomous schedules or persistent transcript collection.
7. Produce a small decision record: per-video outcomes; failure taxonomy; cost and time versus baseline; whether the provider solves **a repeated observed problem**; keep or discard recommendation.

**Pass criteria (set before running):** measurable improvement over baseline on at least the cases that prompted evaluation, correct metadata/caption provenance, timed and complete transcripts or explicit failure, no unsupported visual claims, no sensitive content transfer, costs auditable and within a user-chosen limit. **Stop** if native/local retrieval works well enough, 404/caption absence dominates, output integrity is unprovable, or another integration creates more maintenance than saved research time.

## Ownership / explicit non-goals

- **HE:** source-derived research and possible research-acquisition experiment; document it as a tool-selection question only.
- **PMB:** no memory architecture change, retrieval implementation, hook, context loader, prompt rule, template or release dependency. Keep #83/#95 archival/release decisions independent.
- **ACR:** do not inject external web scraping into its local deterministic review/evaluation pathway.
- **Multi-Provider Dashboard:** native provider telemetry remains authoritative; YouTube/Decodo is **not an adapter or MVP requirement**. Home/Work data boundary remains absolute; corporate use needs independent approval.
- **Human review:** a research agent may gather public captions, but technical conclusions still require claim-specific upstream verification, not vendor-provided summary scores.

**Disposition: preserve as a bounded experiment candidate; no integration, purchase, credential creation, configuration, automation or source-code changes are authorized by this note.**
