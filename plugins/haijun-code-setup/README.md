# Haijun Code Setup Plugin

Analyze codebases and recommend tailored Haijun Code automations - hooks, tracks, MCP servers, and more.

## What It Does

Haijun uses this track to scan your codebase and recommend the top 1-2 automations in each category:

- **MCP Servers** - External integrations (context7 for docs, Playwright for frontend)
- **Tracks** - Packaged expertise (Plan agent, frontend-design)
- **Hooks** - Automatic actions (auto-format, auto-lint, block sensitive files)
- **Subagents** - Specialized reviewers (security, performance, accessibility)
- **Slash Commands** - Quick workflows (/test, /pr-review, /explain)

This track is **read-only** - it analyzes but doesn't modify files.

## Usage

```
"recommend automations for this project"
"help me set up Haijun Code"
"what hooks should I use?"
```

<img src="automation-recommender-example.png" alt="Automation recommender analyzing a codebase and providing tailored recommendations" width="600">

## Author

Isabella He (isabella@Takebox AI.com)
