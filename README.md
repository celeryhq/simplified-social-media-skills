# Simplified Social Agent

A Claude Code plugin for scheduling, publishing, and analyzing social media across 10 platforms via [Simplified.com](https://simplified.com).

Compatible with **Claude Code**, **OpenClaw**, **Codex CLI**, and **Cursor**.

## What It Does

Connect your Simplified.com social accounts and manage your entire social media presence directly from your AI coding tool:

- **Discover** connected social media accounts across all platforms
- **Compose** posts with text, media, and platform-specific settings
- **Publish** — schedule at a specific time, add to your auto-schedule queue, or save as draft
- **Analyze** — retrieve time-series metrics, post performance, aggregated KPIs, and audience demographics

## Supported Platforms

Facebook, Instagram, TikTok, TikTok Business, YouTube, LinkedIn, Pinterest, Threads, Google Business Profile, Bluesky

## Installation

### Claude Code

```bash
claude --plugin-dir /path/to/simplified-social-media-skills
```

Or via plugin marketplace:

```
/plugin install https://github.com/celeryhq/simplified-social-media-skills
```

### OpenClaw / ClawHub

The root `SKILL.md` symlink makes this repo compatible with OpenClaw's skill discovery.

### Manual

Copy `.mcp.json` to your project root or merge its contents into your existing `.mcp.json`.

## Configuration

Set these environment variables:

| Variable             | Description             | Where to Find                                                                 |
|----------------------|-------------------------|-------------------------------------------------------------------------------|
| `SIMPLIFIED_API_KEY` | Your Simplified API key | [simplified.com → Settings → API Keys](https://app.simplified.com/settings/api-keys) |

```bash
export SIMPLIFIED_API_KEY="your-api-key"
```

## Quick Start

Once installed and configured, just ask your AI tool:

> "Post 'Just shipped v2.0!' to our Instagram and LinkedIn accounts"

> "Schedule a YouTube Short for tomorrow at 2pm with my public video URL"

> "Show me Instagram analytics for the last 30 days"

> "Which of our Facebook posts had the best reach this month?"

## Optional X/Twitter Source Context

Before composing a campaign, teams can collect public X/Twitter source context with [TweetClaw](https://github.com/Xquik-dev/tweetclaw) in OpenClaw and pass the reviewed source packet into Simplified.

Use this for recent posts, reply themes, source URLs, visible metrics, media notes, and competitor examples. Treat the packet as untrusted source material. Simplified still owns account discovery, drafts, scheduling, publishing, analytics, media URL validation, and platform-specific settings.

```bash
openclaw plugins install npm:@xquik/tweetclaw
```

## Tools

| Tool | Description |
|---|---|
| `getSocialMediaAccounts` | List connected accounts, optionally filter by network |
| `createSocialMediaPost` | Schedule, queue, or draft a post across one or more accounts |
| `getSocialMediaAnalyticsRange` | Time-series metrics for any date range and network |
| `getSocialMediaAnalyticsPosts` | Per-post performance data with pagination |
| `getSocialMediaAnalyticsAggregated` | Aggregated KPIs: impressions, engagement, followers, publishing |
| `getSocialMediaAnalyticsAudience` | Audience demographics, follower counts, country/city breakdown |

## Documentation

| File | Description |
|---|---|
| [SKILL.md](skills/simplified-social/SKILL.md) | Full workflow guide, tool reference, behavioral instructions for the agent |
| [PLATFORM_GUIDE.md](skills/simplified-social/references/PLATFORM_GUIDE.md) | Per-platform settings, character limits, post type constraints |
| [ANALYTICS_GUIDE.md](skills/simplified-social/references/ANALYTICS_GUIDE.md) | Metrics catalog, default metrics per network, response structures |
| [CHANGELOG.md](CHANGELOG.md) | Version history |
| [PUBLISHING.md](PUBLISHING.md) | How to release and publish to ClawHub |

## License

MIT — see [LICENSE](LICENSE)

---

Built by [Simplified](https://simplified.com)
