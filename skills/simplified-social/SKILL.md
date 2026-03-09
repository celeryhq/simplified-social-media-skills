---
name: simplified-social
description: Schedule and publish social media posts and retrieve analytics across 10 platforms via Simplified.com
version: 1.1.0
homepage: https://simplified.com
triggers:
  - social media
  - post to
  - schedule post
  - publish on
  - facebook
  - instagram
  - tiktok
  - youtube
  - linkedin
  - pinterest
  - threads
  - bluesky
  - social accounts
  - analytics
  - social media analytics
  - post analytics
  - audience analytics
  - engagement analytics
  - reach analytics
  - impressions
  - followers growth
  - social insights
  - social performance
metadata:
  openclaw:
    emoji: "📱"
    requires:
      env:
        - SIMPLIFIED_API_KEY
---

# Simplified Social Media

Schedule, queue, and draft social media posts, and retrieve analytics across 10 platforms using Simplified.com.

## IMPORTANT: Before Any Operation

**Always check if `SIMPLIFIED_API_KEY` is configured before attempting any tool calls.**

If the user tries to use any social media feature and the API key is missing or returns a 401/Unauthorized error:

1. **Stop immediately** — do not retry the failed call
2. **Inform the user** with this exact message:

   > **Simplified Social Media requires an API key to work.**
   >
   > Please follow these steps:
   > 1. Sign up or log in at [simplified.com](https://simplified.com)
   > 2. Go to **[Settings → API Keys](https://app.simplified.com/settings/api-keys)** and copy your API key
   > 3. Add to your shell config (`~/.zshrc` or `~/.bashrc`):
   >    ```bash
   >    export SIMPLIFIED_API_KEY="your-api-key"
   >    ```
   > 4. Reload your shell: `source ~/.zshrc`
   > 5. Restart Claude Code to pick up the new variable

3. **Do not proceed** with the original request until the user confirms the key is set

## Setup

1. Sign up at [simplified.com](https://simplified.com)
2. Connect your social media accounts in the Simplified dashboard
3. Get your API key from **[Settings → API Keys](https://app.simplified.com/settings/api-keys)**
4. Set environment variable:
   ```bash
   export SIMPLIFIED_API_KEY="your-api-key"
   ```

## Core Workflow

Always follow this sequence: **Discover → Select → Compose → Publish**

### Step 1: Discover Accounts

Call `getSocialMediaAccounts` to list connected accounts. Optionally filter by network.

```
getSocialMediaAccounts({ network: "instagram" })
```

Returns account objects with `id`, `name`, `network`, and connection status.

### Step 2: Select Target Accounts

Pick one or more `account_ids` from the results. You can post to multiple accounts in a single call.

### Step 3: Compose the Post

Build the post payload:
- `message` (required) — the post text, max 5000 chars
- `account_ids` (required) — array of target account IDs
- `action` (required) — `schedule`, `add_to_queue`, or `draft`
- `date` — required for `schedule`, format: `YYYY-MM-DD HH:MM`
- `media` — array of public URLs (images/videos), max 10
- `additional` — platform-specific settings (see below)

### Step 4: Publish

Call `createSocialMediaPost` with the composed payload.

## Tool Reference

### `getSocialMediaAccounts`

| Parameter | Type   | Required | Description                          |
|-----------|--------|----------|--------------------------------------|
| `network` | string | No       | Filter by platform (see networks)    |

**Networks:** `facebook`, `instagram`, `linkedin`, `tiktok`, `youtube`, `pinterest`, `threads`, `google`, `bluesky`, `tiktokBusiness`

### `createSocialMediaPost`

| Parameter     | Type     | Required | Description                              |
|---------------|----------|----------|------------------------------------------|
| `message`     | string   | Yes      | Post text (max 5000 chars)               |
| `account_ids` | string[] | Yes      | Target account IDs                       |
| `action`      | string   | Yes      | `schedule`, `add_to_queue`, or `draft`   |
| `date`        | string   | No       | Schedule datetime: `YYYY-MM-DD HH:MM`   |
| `media`       | string[] | No       | Public media URLs (max 10)               |
| `additional`  | object   | No       | Platform-specific settings               |

### `getSocialMediaAnalyticsRange`

Retrieves time-series data for selected metrics within a date range.

| Parameter    | Type     | Required | Description                                                  |
|--------------|----------|----------|--------------------------------------------------------------|
| `account_id` | integer  | Yes      | Social media account ID (from `getSocialMediaAccounts`)      |
| `metrics`    | string[] | Yes      | List of metrics to retrieve (see `references/ANALYTICS_GUIDE.md`) |
| `date_from`  | string   | Yes      | Start date: `YYYY-MM-DD`                                     |
| `date_to`    | string   | Yes      | End date: `YYYY-MM-DD`                                       |
| `tz`         | string   | No       | Timezone, e.g. `UTC`, `Europe/Warsaw` (default: `UTC`)       |

Returns a structured object:
- `data` — array of `{ date, metrics: AnalyticsMetric[] }` — per-day time-series
- `baseLine` — `{ [metricId]: AnalyticsMetric }` — aggregated totals for the full period, each with `value` (current) and `prevValue` (equivalent previous period)
- `additional` — `{ [metricId]: AnalyticsMetric[] }` — extra metrics computed over different windows (e.g., 28-day reach)

Unknown metrics are silently ignored. See `references/ANALYTICS_GUIDE.md` for full metric list and response examples.

### `getSocialMediaAnalyticsPosts`

Retrieves analytics for individual posts within a date range.

| Parameter    | Type    | Required | Description                                             |
|--------------|---------|----------|---------------------------------------------------------|
| `account_id` | integer | Yes      | Social media account ID                                 |
| `date_from`  | string  | Yes      | Start date: `YYYY-MM-DD`                                |
| `date_to`    | string  | Yes      | End date: `YYYY-MM-DD`                                  |

Returns paginated post list with per-post metrics (likes, impressions, etc.). Fields include `all_posts_count`, `current_page`, `pages_count`, and `posts` array with `id`, `message`, `publishedDate`, `postUrl`, `postType`, `media`, and `metrics`.

### `getSocialMediaAnalyticsAggregated`

Retrieves aggregated analytics (totals and averages) for an account within a date range.

| Parameter    | Type    | Required | Description                                             |
|--------------|---------|----------|---------------------------------------------------------|
| `account_id` | integer | Yes      | Social media account ID                                 |
| `date_from`  | string  | Yes      | Start date: `YYYY-MM-DD`                                |
| `date_to`    | string  | Yes      | End date: `YYYY-MM-DD`                                  |

Returns `data` (daily metrics array) and `baseLine` with four aggregated KPIs: `impressions_aggregated`, `engagement_aggregated`, `followers_aggregated`, `publishing_aggregated`. Each KPI includes `value` (current period) and `prevValue` (previous period for comparison).

### `getSocialMediaAnalyticsAudience`

Retrieves audience demographics and follower data for an account.

| Parameter    | Type    | Required | Description                                                  |
|--------------|---------|----------|--------------------------------------------------------------|
| `account_id` | integer | Yes      | Social media account ID                                      |
| `date_from`  | string  | Yes      | Start date: `YYYY-MM-DD`                                     |
| `date_to`    | string  | Yes      | End date: `YYYY-MM-DD`                                       |
| `tz`         | string  | No       | Timezone, e.g. `UTC`, `Europe/Warsaw`                        |

Returns audience breakdown: `audience_page_fans_gender_age` (age/gender split), `audience_page_fans_country` (followers by country code), `audience_page_fans_city` (followers by city). Not all fields are available for every network.

## Action Types

| Action         | When to Use                                          | `date` Required? |
|----------------|------------------------------------------------------|-------------------|
| `schedule`     | Post at a specific date/time                         | Yes               |
| `add_to_queue` | Add to the account's auto-schedule queue             | No                |
| `draft`        | Save for later editing in the Simplified dashboard   | No                |

**Default:** When the user doesn't specify timing, use `add_to_queue`. When they give a date/time, use `schedule`. When they say "save" or "draft", use `draft`.

## Platform Settings Quick Reference

All platform settings go inside the `additional` object, grouped by platform name. **Bold** = required. For full details see `references/PLATFORM_GUIDE.md`.

| Platform       | Required additionals              | Optional additionals               |
|----------------|-----------------------------------|------------------------------------|
| Facebook       | **`postType`**                    | —                                  |
| Instagram      | **`postType`**, **`channel`**     | `postReel` (reel only)             |
| TikTok         | **`postType`**, **`channel`**, **`post`** | `postPhoto` (photo only)  |
| TikTok Biz     | **`postType`**, **`post`**        | `postPhoto` (photo only)           |
| YouTube        | **`postType`**                    | `post`                             |
| LinkedIn       | **`audience`**                    | —                                  |
| Pinterest      | **`post`**                        | —                                  |
| Threads        | **`channel`**                     | —                                  |
| Google         | **`post`**                        | —                                  |
| Bluesky        | —                                 | —                                  |

Key enum values:

| Platform   | Field              | Values                              |
|------------|--------------------|-------------------------------------|
| Facebook   | `postType.value`   | `post`\*, `reel`, `story`           |
| Instagram  | `postType.value`   | `post`\*, `reel`, `story`           |
| Instagram  | `channel.value`    | `direct`\*, `reminder`              |
| TikTok     | `postType.value`   | `video`\*, `photo`                  |
| TikTok     | `channel.value`    | `direct`\*, `reminder`              |
| TikTok     | `post.privacyStatus` | `PUBLIC_TO_EVERYONE`\*, `MUTUAL_FOLLOW_FRIENDS`, `FOLLOWER_OF_CREATOR`, `SELF_ONLY` |
| YouTube    | `postType.value`   | `video`\*, `short`                  |
| YouTube    | `post.privacyStatus` | `""`, `public`, `private`, `unlisted` |
| LinkedIn   | `audience.value`   | `PUBLIC`\*, `CONNECTIONS`, `LOGGED_IN` |
| Threads    | `channel.value`    | `direct`\*, `reminder`              |
| Google     | `post.topicType`   | `STANDARD`\*, `EVENT`, `OFFER`      |

\* = default

## Example Workflows

### Simple Queue Post

```
1. getSocialMediaAccounts({ network: "instagram" })
2. createSocialMediaPost({
     message: "Check out our new feature! 🚀",
     account_ids: ["acc_123"],
     action: "add_to_queue",
     media: ["https://cdn.example.com/image.jpg"],
     additional: {
       instagram: {
         postType: { value: "post" },
         channel:  { value: "direct" }
       }
     }
   })
```

### Scheduled YouTube Short

```
1. getSocialMediaAccounts({ network: "youtube" })
2. createSocialMediaPost({
     message: "Quick tip: how to use our API",
     account_ids: ["acc_456"],
     action: "schedule",
     date: "2026-03-10 14:00",
     media: ["https://cdn.example.com/video.mp4"],
     additional: {
       youtube: {
         postType: { value: "short" },
         post: {
           title: "API Quick Tip",
           privacyStatus: "public",
           selfDeclaredMadeForKids: "no"
         }
       }
     }
   })
```

### Multi-Platform Campaign

```
1. getSocialMediaAccounts()
2. createSocialMediaPost({
     message: "Big announcement! We just launched v2.0 🎉",
     account_ids: ["ig_acc", "fb_acc", "li_acc"],
     action: "schedule",
     date: "2026-03-15 09:00",
     media: ["https://cdn.example.com/launch.jpg"],
     additional: {
       instagram: { postType: { value: "post" }, channel: { value: "direct" } },
       facebook:  { postType: { value: "post" } },
       linkedin:  { audience: { value: "PUBLIC" } }
     }
   })
```

### Analytics: Time-Series Metrics

```
1. getSocialMediaAccounts({ network: "instagram" })
2. getSocialMediaAnalyticsRange({
     account_id: 123,
     metrics: ["impressions", "reach", "follower_count"],
     date_from: "2026-02-01",
     date_to: "2026-02-28",
     tz: "Europe/Warsaw"
   })
```

### Analytics: Post Performance Report

```
1. getSocialMediaAccounts()
2. getSocialMediaAnalyticsPosts({
     account_id: 456,
     date_from: "2026-02-01",
     date_to: "2026-02-28"
   })
```

### Analytics: Account Overview (KPIs + Audience)

```
1. getSocialMediaAccounts({ network: "facebook" })
2. getSocialMediaAnalyticsAggregated({
     account_id: 789,
     date_from: "2026-02-01",
     date_to: "2026-02-28"
   })
3. getSocialMediaAnalyticsAudience({
     account_id: 789,
     date_from: "2026-02-01",
     date_to: "2026-02-28"
   })
```

## Gotchas

- **Analytics `account_id` is an integer**, not a string — use the numeric `id` from `getSocialMediaAccounts`
- **Analytics date format** is `YYYY-MM-DD` (no time component, unlike post scheduling)
- **Unknown metrics are silently ignored** by `getSocialMediaAnalyticsRange` — check `references/ANALYTICS_GUIDE.md` for per-network availability
- **Audience data availability varies** — `getSocialMediaAnalyticsAudience` may return partial or empty data depending on the network
- **Date format** must be `YYYY-MM-DD HH:MM` (24-hour, no seconds, no timezone — uses account timezone)
- **Media URLs** must be publicly accessible — pre-signed or CDN URLs work, localhost does not
- **`date` is required** when `action` is `schedule` — omit it for `add_to_queue` and `draft`
- **Platform character limits** — LinkedIn: 3000, Google: 1500, Bluesky: 300, Threads/Pinterest: 500, others: 2200
- **Instagram always requires `channel`** — include `channel: { value: "direct" }` for every Instagram post
- **TikTok `postType` values** are `video` and `photo` (not `image`)
- **TikTok channel values** are `direct` and `reminder` (not `business`)
- **LinkedIn audience** value is `LOGGED_IN` (not `LOGGED_IN_MEMBERS`)
- **Google `topicType`** only has `STANDARD`, `EVENT`, `OFFER` (no `PRODUCT`)
- **Instagram story** — message must be empty (`""`), max 1 photo
- **Reels and Shorts require video** — Instagram reel, Facebook reel, YouTube short all require a video file in `media`; images are not allowed (`photos.max: 0`)
