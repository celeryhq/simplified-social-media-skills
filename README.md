# Simplified Social Agent

A Claude Code plugin for scheduling and publishing social media posts across 10 platforms via [Simplified.com](https://simplified.com).

Compatible with **Claude Code**, **OpenClaw**, **Codex CLI**, and **Cursor**.

## What It Does

Connect your Simplified.com social accounts and manage posts directly from your AI coding tool:

- **Discover** connected social media accounts
- **Compose** posts with text, media, and platform-specific settings
- **Publish** — schedule at a specific time, add to your auto-schedule queue, or save as draft

## Supported Platforms

Facebook, Instagram, TikTok, YouTube, LinkedIn, Pinterest, Threads, Google Business Profile, Bluesky, TikTok Business

## Installation

### Claude Code

```bash
claude --plugin-dir /path/to/simplified-social-agent
```

Or via plugin marketplace:

```
/plugin install https://github.com/AISimplifed/simplified-social-agent
```

### OpenClaw / ClawHub

The root `SKILL.md` symlink makes this repo compatible with OpenClaw's skill discovery.

### Manual

Copy `.mcp.json` to your project root or merge its contents into your existing `.mcp.json`.

## Configuration

Set these environment variables:

| Variable             | Description                | Where to Find                   |
|----------------------|----------------------------|---------------------------------|
| `SIMPLIFIED_API_KEY` | Your Simplified API key    | simplified.com → Settings → API |

```bash
export SIMPLIFIED_API_KEY="your-api-key"
```

## Quick Start

Once installed and configured, just ask your AI tool to work with social media:

> "Post 'Just shipped v2.0!' to our Instagram and LinkedIn accounts"

> "Schedule a YouTube Short for tomorrow at 2pm with this video: https://cdn.example.com/demo.mp4"

> "Draft a LinkedIn post announcing our Series A"

The agent will:
1. Fetch your connected accounts
2. Select the right ones
3. Compose the post with appropriate platform settings
4. Publish using your preferred action (schedule/queue/draft)

## Tools

| Tool                      | Description                                        |
|---------------------------|---------------------------------------------------|
| `getSocialMediaAccounts`  | List connected accounts, optionally filter by network |
| `createSocialMediaPost`   | Schedule, queue, or draft a post                   |

See [SKILL.md](skills/simplified-social/SKILL.md) for the full workflow guide and [PLATFORM_GUIDE.md](skills/simplified-social/references/PLATFORM_GUIDE.md) for detailed platform reference.

## License

MIT — see [LICENSE](LICENSE)

---

Built by [Simplified](https://simplified.com)
