# Publishing Guide

How to release a new version and publish to ClawHub.

## Release Checklist

Before every release:

- [ ] All changes are tested manually (post, analytics, accounts)
- [ ] `SKILL.md` version bumped
- [ ] `.claude-plugin/plugin.json` version bumped
- [ ] `.claude-plugin/marketplace.json` version bumped
- [ ] `CHANGELOG.md` updated with new version entry
- [ ] `README.md` reflects any new tools or features

## Bumping the Version

Update version in three files:

```
skills/simplified-social/SKILL.md       → version: X.Y.Z
.claude-plugin/plugin.json              → "version": "X.Y.Z"
.claude-plugin/marketplace.json         → "version": "X.Y.Z" (two occurrences)
```

Follow semantic versioning:
- **Patch** `1.0.x` — bug fixes, wording corrections, gotcha additions
- **Minor** `1.x.0` — new tools, new reference content, new guidance sections
- **Major** `x.0.0` — breaking changes to skill structure or tool interface

## Publishing to ClawHub

Install the ClawHub CLI if not already installed:

```bash
npm install -g clawhub
```

Publish the skill:

```bash
clawhub publish skills/simplified-social \
  --slug simplified-social \
  --name "Simplified Social Media" \
  --version <version>
```

Requirements:
- GitHub account must be at least one week old
- Must be logged in to ClawHub CLI (`clawhub login`)

## File Structure

```
.
├── SKILL.md                          # Symlink → skills/simplified-social/SKILL.md
├── README.md                         # Public-facing project overview
├── CHANGELOG.md                      # Version history
├── PUBLISHING.md                     # This file
├── LICENSE
├── .mcp.json                         # MCP server config (HTTP, external)
├── .claude-plugin/
│   ├── plugin.json                   # Claude Code plugin manifest
│   └── marketplace.json              # Marketplace listing metadata
└── skills/
    └── simplified-social/
        ├── SKILL.md                  # Agent instructions (behavioral)
        └── references/
            ├── ANALYTICS_GUIDE.md   # Metrics, defaults, response structures
            └── PLATFORM_GUIDE.md    # Platform settings, character limits
```

## What Goes Where

| Content | File |
|---|---|
| Agent workflow, decision trees, gotchas | `skills/simplified-social/SKILL.md` |
| Default metrics, date/timezone rules | `skills/simplified-social/references/ANALYTICS_GUIDE.md` |
| Platform settings, character limits, constraints | `skills/simplified-social/references/PLATFORM_GUIDE.md` |
| User-facing overview, installation, examples | `README.md` |
| Version history | `CHANGELOG.md` |
