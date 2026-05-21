# AgentSats Skills

Agent skills for Twitter/X and TikTok workflows powered by the AgentSats CLI. This repository is intended to be used as a multi-skill source for `npx skills`.

## Install

Install the base AgentSats CLI skill first:

```bash
npx skills add x402Stacks/bitcoinagent-cli
```

Install all skills from the repo:

```bash
npx skills add <owner/repo>
```

Install one skill:

```bash
npx skills add <owner/repo> --skill agentsats-competitor-social-monitor
```

Local install while developing:

```bash
npx skills add . --list
npx skills add . --skill agentsats-competitor-social-monitor
```

## Repository Layout

Each top-level folder is one skill:

```text
skill-name/
├── SKILL.md
└── agents/
    └── openai.yaml
```

Do not add a root `SKILL.md`; this repo contains multiple skills.

## Skills

- `agentsats-competitor-social-monitor`: monitor public Twitter/X and TikTok competitor activity.
- `agentsats-creator-intelligence-brief`: create creator briefs from Twitter/X and TikTok data.
- `agentsats-cross-platform-content-analyzer`: compare and repurpose creator content across platforms.
- `agentsats-high-coverage-twitter-intelligence-brief`: produce high-coverage Twitter/X topic briefs.
- `agentsats-influencer-lead-scoring`: score creator leads for campaigns.
- `agentsats-tiktok-carousel-image-generator`: create TikTok carousel image plans/prompts from social posts.
- `agentsats-trend-mining`: mine trends from curated Twitter/X and TikTok creators.
- `agentsats-twitter-topic-status-brief`: create daily Twitter/X topic status briefs.
- `agentsats-twitter-tweet-idea-engine`: analyze an account and suggest new tweets.
- `agentsats-watchlist-digest`: create watchlist digests for selected accounts.

## AgentSats Requirement

All skills expect the base AgentSats CLI skill to be installed first:

```bash
npx skills add x402Stacks/bitcoinagent-cli
```

All skills must then use AgentSats as the mandatory source for Twitter/X and TikTok data:

```bash
npx agentsats ... --json
```

Parse useful endpoint payloads from `data.response`. Do not replace AgentSats with direct scraping, browser-only collection, platform APIs, or guessed social data. Pasted text, screenshots, or transcripts may only be used as fallback or supplemental context when AgentSats cannot resolve the source item.

For paid AgentSats endpoints, verify wallet setup first:

```bash
npx agentsats wallet --json
```

If no compatible wallet is configured, set up OWS before paid x402 requests:

```bash
npx agentsats wallet setup --provider ows --preview-stacks --wallet agentsats-mainnet --network mainnet --json
```

After setup, surface the returned `data.address` and tell the user to send STX to that address before using paid endpoints. Do not lead with private-key setup unless the user explicitly asks for it.

## Validation

Confirm the repository is discoverable as a multi-skill source:

```bash
npx skills add . --list
```
