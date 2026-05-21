---
name: agentsats-influencer-lead-scoring
description: Score and rank creator or influencer leads for campaigns from Twitter/X and TikTok data gathered with AgentSats CLI. Use when asked to evaluate creator lists, prioritize outreach, compare brand fit, or produce campaign lead scores.
---

# AgentSats Influencer Lead Scoring

## Mandatory AgentSats Usage

Prerequisite: install the base AgentSats CLI skill first with `npx skills add x402Stacks/bitcoinagent-cli`. Use `npx agentsats` in JSON mode as the mandatory data source for Twitter/X and TikTok profile, post, video, watchlist, competitor, creator, trend, and topic data. Parse usable endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. If the user provides pasted text, screenshots, or transcripts, use them only as fallback or supplemental context after AgentSats cannot resolve the source item from the available identifiers.

For paid AgentSats endpoints, first check `npx agentsats wallet --json`. If no compatible wallet is configured, or an AgentSats command returns `PAYMENT_REQUIRED` because wallet setup is missing, ask the user before configuring payments and use OWS setup: `npx agentsats wallet setup --provider ows --preview-stacks --wallet agentsats-mainnet --network mainnet --json`. After setup, surface `data.address` and tell the user to send STX to that address before using paid x402 endpoints. Do not lead with private-key setup unless the user explicitly asks for it.

## Overview

Use `npx agentsats` in JSON mode to gather read-only Twitter/X and TikTok data for candidate creators, then score them against a campaign brief or outreach goal.

## Inputs

Ask for:

- A list of candidate creators with Twitter/X usernames, Twitter/X user IDs, TikTok usernames, or TikTok `secUid` values.
- Campaign goal, niche, product, target audience, geography, exclusions, and any must-have criteria.
- Optional weighting preferences.
- Optional `--api-url <url>` for a custom bitcoinagent API.

If the user provides only handles, collect profile data first and use returned IDs when available for post/video lookups.

## Collect Data

Run the relevant commands for each creator. Keep command output separate by creator so scoring evidence remains auditable.

```bash
npx agentsats twitter-profile --username <twitter_username> --json
npx agentsats twitter-tweets --user-id <twitter_user_id> --count 20 --json
npx agentsats twitter-highlights --user-id <twitter_user_id> --count 20 --json
npx agentsats tiktok-profile --username <tiktok_username> --json
npx agentsats tiktok-videos --sec-uid <tiktok_sec_uid> --count 20 --json
```

If a command returns `PAYMENT_REQUIRED`, score only from available data and mark the missing endpoint as a data gap.

## Score

Default to a 100-point score unless the user provides a rubric:

- Audience fit, 25 points: niche, content topics, visible community fit.
- Activity and consistency, 20 points: recency, posting volume, platform coverage.
- Content quality, 20 points: clarity, repeatable formats, originality, production fit.
- Campaign fit, 20 points: alignment with product, tone, constraints, likely pitch angle.
- Cross-platform leverage, 10 points: useful presence on both Twitter/X and TikTok.
- Risk and uncertainty, 5 points: data gaps, mismatch, brand-safety concerns from visible content.

Do not score sensitive personal traits. Penalize uncertainty only when missing data affects the campaign decision.

## Output

Return:

- Ranked table with creator, platforms found, score, tier, and one-line rationale.
- Evidence bullets for each high-priority creator.
- Recommended outreach angle for each high-priority creator.
- Data gaps for creators that need another lookup.
- JSON-friendly summary when the user needs automation output.

Use tiers:

- `A`: strong fit, contact first.
- `B`: plausible fit, contact if capacity remains.
- `C`: weak fit or major uncertainty.
- `Reject`: clear mismatch for the stated campaign.
