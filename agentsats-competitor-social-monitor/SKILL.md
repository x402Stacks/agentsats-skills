---
name: agentsats-competitor-social-monitor
description: Monitor public Twitter/X and TikTok activity from a company's competitors using AgentSats CLI JSON output. Use when asked to spy on competitors, build competitive social intelligence, require the company name and a description of what the app, product, or service does, ask for additional context such as website, industry, geography, target customer, price segment, and known competitors, infer at least 20 competitor brands, collect the latest public posts, produce a first-run baseline from the last 10 posts per brand, and create daily competitor marketing briefs.
---

# AgentSats Competitor Social Monitor

## Mandatory AgentSats Usage

Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

## Overview

Build a public competitive-intelligence workflow for a company. Infer competitor brands, find their public Twitter/X and TikTok accounts, collect recent posts with `npx agentsats` in JSON mode, and summarize their social marketing patterns.

This skill uses public social data only. Do not bypass access controls, impersonate users, scrape private data, or automate posting, following, liking, commenting, or messaging.

## Inputs

Ask only for missing inputs:

- Company name.
- Description of what the app, product, or service does.
- Optional website, industry, product category, geography, target customer, or price segment.
- Optional known competitors to include or exclude.
- Optional maximum AgentSats API request budget.
- Whether this is the first run or a daily run.
- Optional `--api-url <url>` for a custom bitcoinagent API.

If no company is provided, ask:

```text
What company should I monitor competitors for?
```

If no app, product, or service description is provided, ask:

```text
What does your app, product, or service do, and who is it for?
```

Always ask for additional context before inferring competitors. The user may skip any unknown fields:

```text
Can you share any extra context: website, industry/category, geography, target customer, price segment, and known competitors to include or exclude?
```

If the run type is unclear, ask:

```text
Is this the first baseline run, or a daily monitoring run?
```

## Competitor Discovery

Infer at least 20 competitor brands before collecting posts. Use the company name and the app, product, or service description as required grounding, plus any supplied website, market, geography, and customer segment.

Build the competitor set from:

- Direct competitors: brands selling the same product or service.
- Adjacent competitors: brands solving the same customer job with a different approach.
- Category leaders: large brands shaping social expectations in the category.
- Emerging challengers: newer brands with active social marketing.
- User-provided competitors, always included unless excluded.

For each candidate, record:

- `brand`
- `why_competitor`
- `confidence`: high, medium, or low
- `twitter_username` if known or discovered
- `twitter_user_id` if available
- `tiktok_username` if known or discovered
- `tiktok_sec_uid` if available
- `notes`

Ask the user to approve or adjust the competitor list when confidence is low, the industry is ambiguous, or fewer than 20 credible brands can be inferred.

## Account Resolution

Use public profiles where possible. Do not invent account IDs or `secUid` values.

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats tiktok-profile --username <tiktok_username> --json
```

If a profile response contains a usable Twitter/X user ID or TikTok `secUid`, use it for post collection. If account resolution fails, mark the brand as a data gap and continue.

If the user supplied a custom API URL, add `--api-url <url>` to every command.

## First Baseline Run

On the first run, collect the latest 10 posts per available platform for each competitor brand.

```bash
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 10 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 10 --json
npx agentsats tiktok-videos --sec-uid <tiktok_sec_uid> --count 10 --json
```

For each brand, summarize:

- Main content pillars.
- Typical post formats.
- Product and offer messaging.
- Visual or creative patterns.
- Calls to action.
- Posting cadence signals when timestamps are available.
- Notable high-signal posts.
- What the brand appears to be testing.

Use the baseline to create a competitor marketing map across all brands.

## Daily Monitoring Run

On daily runs, collect recent posts from the approved competitor list. Use the latest available content and compare against the baseline when available.

```bash
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 10 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 10 --json
npx agentsats tiktok-videos --sec-uid <tiktok_sec_uid> --count 10 --json
```

Filter to today's posts only when timestamps are present. If timestamps are missing, label the output as "latest posts returned by the API" instead of claiming they were posted today.

Track:

- New campaigns, launches, promotions, announcements, or content series.
- Repeated hooks or creative formats.
- Offers, pricing messages, waitlists, product claims, or social proof.
- Platform differences between Twitter/X and TikTok.
- Brands increasing or decreasing activity.
- Posts worth reacting to, only as draft opportunities.

## Request Budget

If the user provides a max request budget, treat it as a hard cap. If no cap is provided, prioritize coverage of at least 20 brands and use enough requests to collect the baseline or daily posts from both platforms where available.

Recommended planning:

- 20 to 40 requests for resolving accounts across brands.
- 40 to 80 requests for first-run baseline collection across 20+ brands.
- 20 to 60 requests for daily monitoring, depending on available accounts.

Continue only while requests add distinct brand or platform coverage. Stop when additional calls are repetitive, paid-blocked, or the approved cap is reached.

## Payment and Error Handling

If an API call returns `PAYMENT_REQUIRED`:

- Record the endpoint, brand, and platform.
- Summarize the decoded payment challenge when available.
- Continue with available data.
- Do not set up wallets, fund accounts, or change payment config unless the user explicitly asks.

If account IDs or `secUid` values are missing, ask for them only if they block important brands. Otherwise mark the gap and continue.

## Output

For a first baseline run, return:

1. Company monitored.
2. Competitor list with confidence and discovered accounts.
3. Request budget and requests used.
4. Brand-by-brand post summary from the latest 10 posts.
5. Competitor marketing map.
6. Common hooks, formats, offers, and CTAs.
7. TikTok versus Twitter/X differences.
8. Opportunities for the user's company.
9. Data gaps and next setup steps.

For a daily run, return:

1. Company monitored and run date.
2. Competitors checked.
3. New posts and notable changes.
4. Campaigns or marketing moves detected.
5. Brand-by-brand daily highlights.
6. Cross-competitor trends.
7. Suggested response ideas or content opportunities, as drafts only.
8. Data gaps and paid-blocked endpoints.

For automation output, include a compact JSON-style object with `company`, `runType`, `competitors`, `requestsUsed`, `brandSummaries`, `trends`, `opportunities`, and `dataGaps`.
