---
name: agentsats-high-coverage-twitter-intelligence-brief
description: Create a high-coverage Twitter/X intelligence brief for any user-selected topic using AgentSats CLI JSON output, targeting at least 50 API requests by default unless the user provides a lower max. Use when asked to get updated on a topic, maximize request coverage, optionally set a max request budget, ask whether to run the brief daily, find top tweets, track important people, companies, organizations, projects, or communities, and summarize what matters now.
---

# AgentSats High-Coverage Twitter Intelligence Brief

## Mandatory AgentSats Usage

Prerequisite: install the base AgentSats CLI skill first with `npx skills add x402Stacks/bitcoinagent-cli`. Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

For paid AgentSats endpoints, first check `npx agentsats wallet --json`. If no compatible wallet is configured, or an AgentSats command returns `PAYMENT_REQUIRED` because wallet setup is missing, ask the user before configuring payments and use OWS setup: `npx agentsats wallet setup --provider ows --preview-stacks --wallet agentsats-mainnet --network mainnet --json`. After setup, surface `data.address` and tell the user to send STX to that address before using paid x402 endpoints. Do not lead with private-key setup unless the user explicitly asks for it.

## Overview

Produce a current Twitter/X intelligence brief for any topic by combining broad topic discovery with focused monitoring of important people, organizations, companies, projects, and communities. Use `npx agentsats` in JSON mode and spend the approved request budget aggressively while keeping a request ledger.

This skill is for research and briefing. Do not post, reply, follow, like, or automate engagement. For financial, medical, legal, political, or safety-sensitive topics, label uncertainty and separate claims from verified facts.

## Inputs

Ask only for missing inputs:

- Topic or scope, such as crypto, AI, a company, a product launch, a public figure, a sports team, a city, a policy issue, a market, or a niche community.
- Optional maximum AgentSats API requests for this run.
- Whether the user wants this brief to run every day.
- Region, language, date window, excluded keywords, or must-include accounts when relevant.
- Important people, companies, organizations, projects, or communities the user specifically wants included.
- Optional `--api-url <url>` for a custom bitcoinagent API.

If no topic is provided, ask:

```text
What Twitter/X topic should I brief today?
```

Then ask:

```text
Do you want this brief to run every day?
```

If the user does not provide a max request budget, do not block on it. Use the default high-coverage target of at least 50 API requests. If the user wants to set a cap, ask:

```text
What is the maximum number of AgentSats API requests I can use for this high-coverage brief? I recommend at least 50.
```

If the user provides a maximum below 50, ask whether to proceed with the smaller run or raise the budget to at least 50. Do not exceed a user-provided maximum.

## Request Strategy

Target at least 50 API requests by default. If the user provides a maximum request count, treat it as a hard cap and use it as the budget. If no maximum is provided, continue past 50 when additional diverse sources remain useful, but stop when endpoints are unavailable, paid access blocks useful collection, or repeated calls are clearly redundant.

Allocate the budget roughly as follows:

- 10 percent: discover Twitter endpoints and test search/trend/query routes.
- 30 percent: broad topic search, top tweets, latest tweets, trend, or hashtag endpoints when available.
- 20 percent: key people and individual accounts.
- 20 percent: companies, organizations, projects, official accounts, or communities.
- 15 percent: second-pass validation on claims, accounts, narratives, or related queries.
- 5 percent: retries, alternate parameter names, and data-gap checks.

For budgets over 100 requests, widen source diversity before deepening the same account. For budgets near 50, prioritize broad search, top tweets, and the most central accounts. For uncapped runs, prefer a practical 50 to 75 request pass unless the topic is broad and fresh enough to justify more.

## Daily Run Preference

Ask whether the user wants this brief to run every day. If yes, include the daily preference in the output and ask what delivery or scheduling surface should handle it only when the user asks you to set up automation. Do not create cron jobs, calendar events, background processes, or external automations unless the user explicitly asks.

## Discover Twitter Endpoints

Start each run by discovering the Twitter API surface:

```bash
npx agentsats service-endpoints --service twitter --json
```

Prefer endpoints that can find top or recent posts by query. Look for endpoint names or parameters like `search`, `query`, `q`, `keyword`, `top`, `latest`, `tweets`, `hashtag`, `trends`, or `timeline`.

Use only discovered or documented endpoints. Do not invent paths.

Examples for discovered service-scoped endpoints:

```bash
npx agentsats twitter --endpoint <discovered-endpoint> --query q=<topic> --json
npx agentsats twitter --endpoint <discovered-endpoint> --query query=<topic> --json
npx agentsats twitter --endpoint <discovered-endpoint> --query keyword=<topic> --json
```

If the user supplied a custom API URL, add `--api-url <url>` to every command.

## Broad Topic Discovery

Run broad searches first when supported. Build query variants from:

- The exact user topic.
- Synonyms, ticker symbols, product names, aliases, hashtags, or common abbreviations.
- Related people, companies, organizations, projects, or communities.
- Event terms such as launch, outage, announcement, lawsuit, earnings, hack, regulation, debate, rumor, or update when relevant.

Prefer top or high-engagement results when an endpoint exposes ranking. Otherwise use recent/latest results and state the limitation.

## Key Account Coverage

Build account coverage from three sources:

1. User-provided accounts.
2. Accounts returned by broad search or top-tweet endpoints.
3. Topic-relevant candidates inferred from the scope, labeled as candidates rather than permanent authorities.

Categories to consider:

- Individuals: founders, executives, researchers, creators, analysts, journalists, athletes, politicians, or builders relevant to the topic.
- Official sources: companies, projects, agencies, teams, communities, or institutions.
- Amplifiers: media accounts, newsletters, analysts, commentators, or fan/community accounts.
- Critics and skeptics: accounts driving disagreement or risk narratives.

Resolve profiles and collect recent tweets or highlights:

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 50 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 50 --json
```

If the approved budget is large, increase account count before increasing per-account depth. If an endpoint supports `--count 100` or `--count 200`, use larger counts for the highest-signal accounts only.

## Analyze

Extract:

- Top narratives: what Twitter/X is focused on now.
- Top tweets: central posts, announcements, high-signal commentary, or widely referenced claims.
- Important accounts: who is driving the discussion and why.
- Important organizations: official announcements, incidents, launches, disputes, integrations, outages, statements, or policy changes.
- Sentiment split: supportive, critical, skeptical, excited, confused, angry, risk-focused, technical, speculative, or mixed.
- Urgency: what needs attention today versus background context.
- Verification needs: claims that require official sources, filings, data, external reporting, or direct confirmation.

Keep rumor, speculation, satire, opinion, and confirmed news clearly separated.

## Output

Return:

1. Topic, run date, daily-run preference, request cap if provided, target minimum, and requests used.
2. Executive summary.
3. Top Twitter/X narratives.
4. Top tweets or posts, with source identifiers when available.
5. Important people and accounts.
6. Important organizations, companies, projects, or official accounts.
7. Sentiment and disagreement map.
8. Confirmed items versus speculation.
9. Risks, missing data, paid-blocked endpoints, and low-confidence areas.
10. Follow-up queries and accounts to monitor next.

If the user wants automation output, include a compact JSON-style summary with `topic`, `dailyRun`, `requestCap`, `requestsUsed`, `targetMinimum`, `narratives`, `topPosts`, `accounts`, `organizations`, `risks`, and `nextQueries`.
