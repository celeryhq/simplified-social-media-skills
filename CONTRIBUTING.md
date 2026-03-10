# Contributing

## Skill Content Conventions

### SKILL.md — behavioral instructions only

SKILL.md is what the agent reads to decide how to act. Keep it focused on:
- **Workflows** — step-by-step sequences the agent should follow
- **Decision trees** — when to call which tool
- **Gotchas** — common mistakes and how to avoid them
- **Pointers** to reference files for detailed data

Do NOT add large data tables (metrics lists, enum values) to SKILL.md — put those in `references/`.

### references/ANALYTICS_GUIDE.md

Owns all analytics detail:
- Default metrics per network
- Full metric catalog per network
- Date/timezone rules
- Response structure examples

### references/PLATFORM_GUIDE.md

Owns all platform detail:
- Per-platform `additional` object settings
- Character limits
- Post type constraints (photo/video requirements, dimensions, durations)

## Adding a New Platform

1. Add the platform to the networks list in `SKILL.md` (`getSocialMediaAccounts`)
2. Add the `type` value mapping in the accounts response table
3. Add platform settings to `references/PLATFORM_GUIDE.md`
4. Add default metrics to `references/ANALYTICS_GUIDE.md`
5. Add character limit to the limits table in `references/PLATFORM_GUIDE.md`
6. Add trigger keywords to `SKILL.md` frontmatter if needed
7. Bump minor version

## Adding a New Tool

1. Add tool to the Tool Reference section in `SKILL.md` with parameter table
2. Add an example workflow to the Example Workflows section
3. Add relevant gotchas if applicable
4. Update `README.md` tools table
5. Bump minor version, update `CHANGELOG.md`

## Versioning

See [PUBLISHING.md](PUBLISHING.md) for versioning rules and release checklist.
