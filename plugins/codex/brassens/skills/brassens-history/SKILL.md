---
name: brassens-history
description: Use when the user asks about something they dictated, said in a meeting, or imported as media (video, podcast, captions) in Brassens - e.g. "what did we decide in Monday's meeting", "find my note about X", "summarize that YouTube talk I imported". Explains how to use the read-only Brassens MCP tools (search then fetch) and handle access errors.
---

# Brassens history

The `brassens` MCP server gives read-only access to the signed-in user's own Brassens history. Nothing can be created, edited or deleted through it.

## Tools (always search, then fetch)

| Content | Search tool | Fetch tool |
| --- | --- | --- |
| Dictations / transcriptions | `search_transcriptions` | `fetch_transcription` |
| Meetings (notes, speakers, segments) | `search_meetings` | `fetch_meeting` |
| Media imports (videos, podcasts, captions) | `search_content` | `fetch_content` |

1. Call the search tool that matches the content type. If the type is unclear, search several.
2. Pick the relevant results from their `title`, `createdAt` and `snippet`.
3. Call the paired fetch tool with the result `id` to read the full text. Never answer from a snippet alone when details matter.

The generic `search` and `fetch` tools search and read across all content types at once. Prefer the kind-specific tools when you know the type.

## Search inputs

- `query`: full-text keywords, max 200 characters. Supports quoted phrases, `or` and `-exclude`. An empty query lists the most recent items.
- `from` / `to`: ISO 8601 dates. `from` is inclusive and `to` is exclusive. For "last week", compute concrete dates first.
- `limit`: 1 to 25, default 10.
- `cursor`: pass `nextCursor` from the previous page to continue. Keep query and filters unchanged when paging.

## Fetching and truncation

- `fetch_meeting` returns notes, speaker names, `transcriptText` and paged segments. For long meetings, page with `segmentOffset` and `segmentLimit` (default 500, max 2000).
- `fetch_content` returns the media text (capped at 200,000 characters) and paged segments. Page with `segmentOffset` and `segmentLimit`, continuing from `nextSegmentOffset` until it is absent.
- `fetch_transcription` returns the dictation text.
- If any fetch result has `truncated: true`, tell the user the text was cut and that your answer may miss the end.

## Citing

Cite what you used by title, date and `id` (and `url` when present), so the user can open it in Brassens.

## Errors

Errors come back as tool results with `error.code`. Report them plainly and do not retry the same call in a loop, except where a recovery is given below.

- `invalid_cursor`: the cursor is stale or does not match the query. Run the search again without `cursor`, keeping the same query and filters.
- `not_found`: the `id` does not exist or is not the user's. Search again rather than guessing ids.
- `temporarily_unavailable`: retry once after a short pause; if it fails again, tell the user to try later.
- `internal_error`: tell the user the Brassens server failed; do not retry.

- `cloud_retention_disabled`: the user turned cloud data retention off, so their content stays on their devices. Tell them to enable it in Brassens Settings > Data & Privacy if they want AI tools to read it. Content from before that is not available either.
- `subscription_required`: the connector needs an active paid Brassens Pro subscription; free trials are not included. The app keeps read access to history, but agent access does not.
- `feature_disabled`: agent access is not enabled for this account yet.
- `account_not_found`: the login does not match a Brassens account; ask the user to sign in with the account they use in the app.
- HTTP 401 / authentication prompts: the user needs to (re)authenticate the `brassens` MCP server.
