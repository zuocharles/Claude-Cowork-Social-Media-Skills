# Claude Cowork Social Media Skills

Browser automation skills for Claude (via [Claude in Chrome extension](https://chromewebstore.google.com/detail/claude-in-chrome/)) to browse and research content on social media platforms.

## What This Is

These are **site-specific knowledge skills** that teach Claude's browser agent how social media platforms work — their page structures, where data lives (and doesn't), URL patterns, and common pitfalls. Without these skills, the agent can still browse these sites, but it wastes significant time discovering things like "TikTok doesn't show view counts on search results pages" through trial and error.

Think of each skill as a cheat sheet that turns a 53-tool-call fumbling session into a focused 15-call efficient one.

## What This Is NOT

- **Not an API wrapper** — these skills control a real browser, not an API
- **Not a scraper** — no data extraction pipelines, just interactive browsing
- **Not a posting tool** — read-only browsing and monitoring (for now)

## Available Skills

| Skill | Platform | Status | Download |
|-------|----------|--------|----------|
| [tiktok-browser](./skills/tiktok-browser/) | TikTok | v0.1 | [⬇ .skill](https://github.com/zuocharles/Claude-Cowork-Social-Media-Skills/releases/latest/download/tiktok-browser.skill) |

## Requirements

- [Claude in Chrome extension](https://chromewebstore.google.com/detail/claude-in-chrome/) installed
- Logged into the target platform in your browser
- Claude Desktop, Claude Code, or Claude Cowork as the client

## Installation

### For Claude Cowork (Recommended)

**Option A: Download the .skill package (easiest)**

1. Go to the [latest release](https://github.com/zuocharles/Claude-Cowork-Social-Media-Skills/releases/latest)
2. Download the `.skill` file (e.g., `tiktok-browser.skill`)
3. In Claude Desktop, go to **Customize → Skills → click `+` → "Upload a skill"**
4. Select the downloaded `.skill` file
5. The skill is now available in all your Cowork sessions

**Option B: Copy-paste the skill instructions**

1. Open the [`SKILL.md`](./skills/tiktok-browser/SKILL.md) file in this repo
2. Copy the full contents
3. In Claude Desktop, go to **Customize → Skills → click `+` → "Write skill instructions"**
4. Paste the contents and save

### For Claude Code

Copy the skill folder into your skills directory:

```bash
cp -r skills/tiktok-browser ~/.claude/skills/
```

Or add the skill path to your Claude configuration.

### For Claude in Chrome Extension

The skill can be referenced in your Chrome extension shortcuts or workflows. Load the SKILL.md content into Claude's context when starting a browsing task.

## How Skills Work

Each skill is a `SKILL.md` file that Claude reads before performing tasks on a platform. It contains:

1. **Domain Knowledge** — what data is available where (saves the most tool calls)
2. **Page Structure Maps** — accessibility tree patterns for each page type
3. **URL Patterns** — direct navigation shortcuts to skip clicking through menus
4. **Common Pitfalls** — known issues and how to recover from them
5. **Workflow Guides** — step-by-step approaches for common tasks

## Packaging & Releases

Each skill is released as a `.skill` file (a zip archive of the skill directory containing `SKILL.md` and any reference files). Download from the [Releases](https://github.com/zuocharles/Claude-Cowork-Social-Media-Skills/releases) page.

- **Individual skills**: Download the `.skill` file for the platform you need
- **All skills bundle**: When multiple platform skills are available, a `.plugin` bundle will be provided for one-click installation of everything

## Roadmap

### Platforms
- [ ] Instagram browser skill
- [ ] X/Twitter browser skill
- [ ] YouTube browser skill
- [ ] Reddit browser skill
- [ ] LinkedIn browser skill

### Capabilities
- [ ] Content posting and scheduling
- [ ] Comment and engagement
- [ ] Analytics monitoring
- [ ] Cross-platform content research

## Contributing

Found a UI change that broke something? Know a trick for navigating a platform more efficiently? PRs welcome.

When contributing a new platform skill:
1. Actually browse the platform with Claude in Chrome first
2. Document what confused the agent or wasted tool calls
3. Map the accessibility tree patterns for key pages
4. Write the skill focusing on domain knowledge over step-by-step scripts

## License

MIT
