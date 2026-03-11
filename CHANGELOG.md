# Changelog

## [1.2.2] - 2026-03-11

### Fixed
- Add MCP server configuration section to SKILL.md body so OpenClaw/mcporter knows to connect to `https://mcp.simplified.com/social-media/mcp`
- Remove non-standard `requires.mcp` from frontmatter (not supported by OpenClaw)

## [1.2.1] - 2026-03-10

### Fixed
- Add `license: MIT` to SKILL.md frontmatter for ClawHub publishing compatibility

## [1.2.0] - 2026-03-10

### Added
- Decision tree for choosing the right analytics tool (Range vs Posts vs Aggregated vs Audience)
- Default metrics per network for `getSocialMediaAnalyticsRange` — recommended metric sets for all 10 platforms
- Relative date range guide — how to translate "last 30 days", "this month" etc. to concrete dates
- Timezone guidance for analytics — when to pass `tz`, when to ask the user
- Pagination guidance for `getSocialMediaAnalyticsPosts` — use `per_page: 100`, loop until `current_page >= pages_count`
- Empty accounts message — agent now shows onboarding prompt when no accounts are connected
- `page` and `per_page` parameters for `getSocialMediaAnalyticsPosts`
- Character limits table per platform in `PLATFORM_GUIDE.md`
- New triggers: `google my business`, `gmb`, `content calendar`, `social media manager`, `post scheduling`, `social media automation`, `social media campaign`

### Fixed
- `getSocialMediaAccounts` response documented correctly: `type` field (not `network`), wrapped in `{ accounts: [] }`, no connection status
- Deprecated `impressions` metric removed from Instagram examples — replaced with `saves`
- YouTube `post` marked as required additional (includes mandatory `title` field)
- LinkedIn Company vs Personal distinction based on `type` field value
- Example `per_page: 25` corrected to `per_page: 100`

### Changed
- Default metrics and character limits moved to reference files (`ANALYTICS_GUIDE.md`, `PLATFORM_GUIDE.md`) — SKILL.md now contains only behavioral instructions

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
