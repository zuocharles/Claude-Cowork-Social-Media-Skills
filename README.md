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

| Skill | Platform | Status |
|-------|----------|--------|
| [tiktok-browser](./skills/tiktok-browser/) | TikTok | v0.1 |

## Requirements

- [Claude in Chrome extension](https://chromewebstore.google.com/detail/claude-in-chrome/) installed
- Logged into the target platform in your browser
- Claude Desktop, Claude Code, or Claude Cowork as the client

## Installation

### For Claude Code / Cowork

Copy the skill folder into your skills directory:

```bash
cp -r skills/tiktok-browser ~/.claude/skills/
```

Or add the skill path to your Claude configuration.

### For Claude in Chrome

The skill works automatically when loaded into Claude's context. You can reference it in your Chrome extension shortcuts or workflows.

## How Skills Work

Each skill is a `SKILL.md` file that Claude reads before performing tasks on a platform. It contains:

1. **Domain Knowledge** — what data is available where (saves the most tool calls)
2. **Page Structure Maps** — accessibility tree patterns for each page type
3. **URL Patterns** — direct navigation shortcuts to skip clicking through menus
4. **Common Pitfalls** — known issues and how to recover from them
5. **Workflow Guides** — step-by-step approaches for common tasks

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
