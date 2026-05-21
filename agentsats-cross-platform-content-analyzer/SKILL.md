---
name: agentsats-cross-platform-content-analyzer
description: Analyze and repurpose creator content across Twitter/X and TikTok using AgentSats CLI JSON data. Use when asked to compare social content, find content pillars, convert tweets to short-video ideas, turn TikToks into tweets or threads, or identify cross-platform content gaps.
---

# AgentSats Cross-Platform Content Analyzer

## Mandatory AgentSats Usage

Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

## Overview

Use `npx agentsats` in JSON mode to compare a creator's Twitter/X and TikTok content. Produce practical recommendations for repurposing, content strategy, and platform-specific positioning.

## Inputs

Collect the identifiers needed for both platforms:

- Twitter/X username and, when needed, Twitter/X user ID.
- TikTok username and, when needed, TikTok `secUid`.
- Optional goal: repurpose content, compare performance, build a content calendar, or find content gaps.
- Optional `--api-url <url>` for a custom bitcoinagent API.

If only one platform is available, produce a single-platform analysis and list what is needed for the cross-platform pass.

## Collect Data

Use JSON mode so downstream analysis can parse one stable object per command. Read useful endpoint payloads from `data.response`.

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 20 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 20 --json
npx agentsats tiktok-profile --username <tiktok_username> --json
npx agentsats tiktok-videos --sec-uid <tiktok_sec_uid> --count 20 --json
```

Use `--api-url <url>` on every command when the user supplies an API URL.

## Compare Content

Analyze the platforms side by side:

- Topics: themes that appear on Twitter/X, TikTok, or both.
- Formats: threads, short observations, clips, tutorials, reactions, trends, demos.
- Hooks: opening lines, video intros, questions, claims, numbers, controversy, novelty.
- Depth: which ideas need thread depth versus short-video demonstration.
- Timing: recent activity and repeatable cadence patterns when timestamps are present.
- Gaps: strong topics on one platform that are missing on the other.

Avoid claiming causality from thin data. Say "likely", "appears", or "not enough data" when the API payload does not include strong performance evidence.

## Produce Recommendations

Deliver one or more of these outputs based on the request:

- Cross-platform summary: what the creator is known for and where each platform differs.
- Repurposing map: TikTok videos that could become tweets or threads, and tweets that could become video scripts.
- Content pillar table: pillar, evidence, Twitter/X angle, TikTok angle.
- Next 10 content ideas: each with platform, hook, format, and source evidence.
- Gaps and experiments: specific tests to run next.

Keep recommendations tied to observed content. Do not suggest automated posting or engagement unless the user explicitly asks; this skill is for analysis and drafting.
