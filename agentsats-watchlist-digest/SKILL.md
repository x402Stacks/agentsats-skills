---
name: agentsats-watchlist-digest
description: Build daily or weekly watchlist digests for selected Twitter/X and TikTok accounts using AgentSats CLI JSON output. Use when asked to monitor creators, summarize recent activity, identify notable posts or videos, or flag outreach and content opportunities.
---

# AgentSats Watchlist Digest

## Mandatory AgentSats Usage

Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

## Overview

Use `npx agentsats` in JSON mode to monitor a curated list of Twitter/X and TikTok accounts and produce a concise digest for a day, week, campaign, or niche.

## Inputs

Ask for:

- Watchlist accounts: Twitter/X user IDs for tweets and highlights, TikTok `secUid` values for videos, plus handles/usernames for labels.
- Digest period: daily, weekly, since last run, or a custom date range.
- Digest goal: opportunities, competitor monitoring, creator research, campaign tracking, or content ideas.
- Optional `--api-url <url>` for a custom bitcoinagent API.

If the user gives only usernames, run profile lookups first and use returned IDs when available.

## Collect Data

Run the relevant recent-content commands for each account:

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 20 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 20 --json
npx agentsats tiktok-profile --username <tiktok_username> --json
npx agentsats tiktok-videos --sec-uid <tiktok_sec_uid> --count 20 --json
```

Filter by timestamp only when the endpoint payload includes usable timestamps. Otherwise, clearly label the digest as "recent items returned by the API" instead of claiming an exact time window.

## Build the Digest

Prioritize signal over completeness:

- Account-level changes: profile updates, follower changes when available, new platform activity.
- Notable posts or videos: strong hooks, unusual topics, high visible engagement, campaign relevance.
- Repeated themes: topics multiple accounts mentioned.
- Opportunities: outreach moments, reply/draft opportunities, partnership openings, content ideas.
- Risks: visible controversy, off-brand topics, stale accounts, missing data.

Do not perform engagement actions. Provide suggested replies or outreach notes only as drafts.

## Output

Use this structure:

1. Executive Summary
2. Account Highlights
3. Top Posts or Videos
4. Themes Across the Watchlist
5. Opportunities
6. Risks and Data Gaps
7. Suggested Next Actions

When the user asks for automation-ready output, return a compact JSON-style object with `accounts`, `highlights`, `themes`, `opportunities`, and `dataGaps`.
