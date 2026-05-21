---
name: agentsats-twitter-topic-status-brief
description: Create a daily current-status brief for a user-selected topic on Twitter/X using AgentSats CLI JSON output within a user-approved maximum API request budget. Use when asked to run a daily Twitter topic monitor, ask the user what topic to brief and how many requests may be used, gather current Twitter/X signals, summarize narratives, accounts, notable posts, risks, and what changed.
---

# AgentSats Twitter Topic Status Brief

## Mandatory AgentSats Usage

Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

## Overview

Run a daily Twitter/X topic-monitoring workflow. Ask the user for the topic and maximum request budget when they are not already provided, gather current Twitter/X data through `npx agentsats` in JSON mode, and produce a concise status brief.

This skill is for analysis and briefing. It does not post, reply, follow, like, or automate engagement.

## Daily Run Behavior

At the start of each daily run:

1. If the user already provided a topic, use it.
2. If no topic is provided, ask one direct question: `What Twitter/X topic should I brief today?`
3. If the user already provided a max request budget, use it.
4. If no max request budget is provided, ask one direct question: `What is the maximum number of AgentSats API requests I can use for this brief?`
5. Ask for optional constraints only when necessary: geography, language, niche, competitor names, excluded keywords, or custom `--api-url`.
6. Stop earlier than the approved budget when the brief has enough evidence.

Treat the user's max request budget as a cap, not a requirement. Use 50 requests only when the user explicitly chooses 50 or says to use the default.

## Request Budget

Track requests as you run them. Adapt the plan to the approved maximum:

- 1 request for `service-endpoints --service twitter`.
- About 10 percent for topic discovery or search endpoints, if available.
- About 35 percent for notable tweets, timelines, or related result pages.
- About 35 percent for profiles or recent tweets from key accounts.
- About 15 percent for follow-up validation on claims, account context, or similar terms.
- Reserve the remaining budget for retries, endpoint alternatives, or data gaps.

Do not hide failed or paid-blocked requests. Include them in the data-gaps section.

## Discover Twitter Endpoints

Start by discovering the available Twitter API surface unless the user already gave a known endpoint plan:

```bash
npx agentsats service-endpoints --service twitter --json
```

Prefer discovered endpoints that support topic/search/query behavior. Look for names or parameters such as `search`, `query`, `q`, `keyword`, `tweets`, `latest`, `timeline`, `trends`, `top`, or `hashtag`.

When using service-scoped endpoints, pass query strings as repeated `--query key=value` flags:

```bash
npx agentsats twitter --endpoint <discovered-endpoint> --query q=<topic> --json
npx agentsats twitter --endpoint <discovered-endpoint> --query query=<topic> --json
npx agentsats twitter --endpoint <discovered-endpoint> --query keyword=<topic> --json
```

Use only endpoints discovered from `service-endpoints` or already documented by the AgentSats CLI. Do not invent paths.

## Fallback Collection

If topic search is not available, explain the limitation and switch to a seed-account workflow:

1. Ask the user for 3 to 10 Twitter/X accounts relevant to the topic, or use accounts the user already provided.
2. Resolve profiles and recent tweets:

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 50 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 50 --json
```

3. Build the topic brief from the returned posts, highlights, and account context.

If a profile response contains a usable user ID, use it for follow-up tweet commands. Do not invent user IDs.

## Payment and Error Handling

If an API call returns `PAYMENT_REQUIRED`:

- Record the endpoint and what it was trying to collect.
- Summarize the decoded payment challenge when available.
- Continue with unpaid or already-collected data when possible.
- Do not set up a wallet, clone OWS, or fund anything unless the user explicitly asks.

If an endpoint returns validation errors, inspect the discovered endpoint shape and retry once with the corrected parameter name only when the fix is clear.

## Analyze the Topic

Extract signals from returned tweets, profiles, highlights, and search results:

- Current narratives: what people are saying now.
- Key accounts: who is driving or reacting to the conversation.
- Notable posts: posts that appear central, high-signal, or widely referenced.
- Sentiment split: supportive, critical, skeptical, excited, confused, or mixed.
- Claims and open questions: factual claims that need verification.
- Freshness: timestamps or recency markers when available.
- Shifts since prior runs: only compare if previous daily notes are available in the conversation or user-provided files.

Avoid overclaiming from small samples. Label low-confidence conclusions.

## Output

Return a brief with:

1. Topic and run date.
2. Approved request budget and requests used, including failed or paid-blocked requests.
3. Executive summary.
4. Current narratives.
5. Key accounts and why they matter.
6. Notable tweets or examples, with source identifiers when available.
7. Sentiment and disagreement map.
8. What changed or appears new.
9. Risks, uncertainty, and data gaps.
10. Suggested follow-up queries for tomorrow.

Keep the brief concise and decision-oriented. Include raw JSON only when the user asks for automation-ready output.
