---
name: agentsats-twitter-tweet-idea-engine
description: Analyze a Twitter/X account's latest tweets with AgentSats CLI, compare against a niche and similar accounts, and prepare new tweet suggestions. Use when asked to study an account, fetch up to 200 recent tweets, infer voice and content pillars, analyze peer accounts, or generate ready-to-post tweet ideas.
---

# AgentSats Twitter Tweet Idea Engine

## Mandatory AgentSats Usage

Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

## Overview

Use `npx agentsats` in JSON mode to collect recent Twitter/X account data, analyze the account's voice and niche, compare similar accounts when available, and generate new tweet suggestions that fit the account.

## Inputs

Ask for missing identifiers and constraints:

- Target Twitter/X username.
- Target Twitter/X user ID, if known.
- Niche or desired audience.
- Similar account usernames or user IDs, if known.
- Desired number and style of suggestions.
- Optional constraints: no threads, thread ideas only, educational, contrarian, founder voice, meme-light, launch-focused, etc.
- Optional `--api-url <url>` for a custom bitcoinagent API.

If only the username is provided, run a profile lookup first and use a returned user ID when available.

## Collect Target Data

Always use `--json`. Read endpoint payloads from `data.response`.

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 200 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 200 --json
npx agentsats twitter-followings --user-id <twitter_user_id> --count 200 --json
```

If the API returns fewer than 200 tweets, use the returned count and state the actual sample size. If `--count 200` is rejected or capped, retry with the largest supported count only when the error tells you the limit.

When endpoint coverage is uncertain, discover Twitter endpoints:

```bash
npx agentsats service-endpoints --service twitter --json
```

Use `api-call` or the `twitter` service command only for discovered endpoints. Keep endpoint paths validated and use `--query key=value` for query strings.

## Collect Similar Account Data

Use user-provided similar accounts first. If none are provided, infer candidates from followings, profile context, or the niche, but label them as candidates and ask before spending paid calls on many accounts.

For each approved similar account:

```bash
npx agentsats twitter-profile --username <similar_username> --json
npx agentsats twitter-tweets --user-id <similar_user_id> --count 100 --json
npx agentsats twitter-highlights --user-id <similar_user_id> --count 100 --json
```

Keep peer examples separate from the target account so the final suggestions preserve the target voice instead of copying peers.

## Analyze

Build a working model of the account:

- Voice: sentence length, tone, confidence level, humor, vocabulary, formatting.
- Content pillars: repeated topics, audience pains, beliefs, product/category ideas.
- Formats: one-liners, lists, mini-threads, frameworks, hot takes, questions, stories, proof posts.
- Hooks: opening patterns that recur in strong posts.
- Engagement hints: visible likes/reposts/replies when present, highlights, repeated ideas.
- Niche gaps: important topics peers discuss that the target account does not.
- Avoid list: stale themes, off-brand language, overused hooks, claims without evidence.

Do not plagiarize target or peer tweets. Use patterns and topics to create original drafts.

## Suggest Tweets

Create suggestions in batches:

- Quick posts: short standalone tweets.
- Authority posts: practical frameworks or lessons.
- Conversation starters: questions or polarizing but defensible takes.
- Proof or story prompts: tweet drafts that need the user's real details.
- Thread starters: hooks plus 3 to 5 bullet outline when requested.

For each suggestion include:

- Draft tweet.
- Why it fits the account.
- Source signal from target or similar accounts.
- Niche pillar.
- Risk or required fact check, if any.

Respect current platform length constraints by keeping standalone drafts concise. If a draft needs external facts, mark it as requiring verification before posting.

## Output

Return:

1. Data collected and sample sizes.
2. Account voice summary.
3. Content pillars and gaps.
4. Similar-account signals.
5. Ready-to-use tweet suggestions.
6. Optional thread ideas.
7. Fact-check and personalization notes.

When the user wants an ongoing workflow, propose a repeatable cadence: refresh the latest 200 tweets, refresh selected peer accounts, update the voice model, then generate a new batch.
