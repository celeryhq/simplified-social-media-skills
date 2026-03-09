# Changelog

## [1.1.0] - 2026-03-09

### Added
- Analytics support: four new tools for retrieving social media insights
  - `getSocialMediaAnalyticsRange` — time-series metrics for any date range and network
  - `getSocialMediaAnalyticsPosts` — per-post performance data
  - `getSocialMediaAnalyticsAggregated` — aggregated KPIs (impressions, engagement, followers, publishing)
  - `getSocialMediaAnalyticsAudience` — audience demographics, follower counts, and country/city breakdown
- `references/ANALYTICS_GUIDE.md` — full metric catalog with per-network availability, response shapes, and examples
- New analytics triggers in SKILL.md (analytics, impressions, followers growth, etc.)
- Analytics workflow examples in SKILL.md (time-series, post performance, account overview)
- Analytics gotchas section in SKILL.md

## [1.0.0] - 2026-03-06

### Added
- Initial release
- `getSocialMediaAccounts` — list connected social media accounts with optional network filter
- `createSocialMediaPost` — schedule, queue, or draft posts across 10 platforms
- Platform support: Facebook, Instagram, TikTok, YouTube, LinkedIn, Pinterest, Threads, Google Business, Bluesky, TikTok Business
- SKILL.md with full workflow guide, tool reference, and example workflows
- PLATFORM_GUIDE.md with detailed per-platform parameter reference
- Claude Code plugin manifest (`.claude-plugin/plugin.json`)
- MCP server configuration (`.mcp.json`)
