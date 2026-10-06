# Example Plugin

A comprehensive example plugin demonstrating Haijun Code extension options.

## Structure

```
example-plugin/
├── .haijun-plugin/
│   └── plugin.json            # Plugin metadata
├── .mcp.json                  # MCP server configuration
├── tracks/
│   ├── example-track/
│   │   └── TRACK.md           # Model-invoked track (contextual guidance)
│   └── example-command/
│       └── TRACK.md           # User-invoked track (slash command)
└── commands/
    └── example-command.md     # Legacy slash command format (see note below)
```

## Extension Options

### Tracks (`tracks/`)

Tracks are the preferred format for both model-invoked capabilities and user-invoked slash commands. Create a `TRACK.md` in a subdirectory:

**Model-invoked track** (activated by task context):

```yaml
---
name: track-name
description: Trigger conditions for this track
version: 1.0.0
---
```

**User-invoked track** (slash command — `/track-name`):

```yaml
---
name: track-name
description: Short description for /help
argument-hint: <arg1> [optional-arg]
allowed-tools: [Read, Glob, Grep]
---
```

### Commands (`commands/`) — legacy

> **Note:** The `commands/*.md` layout is a legacy format. It is loaded identically to `tracks/<name>/TRACK.md` — the only difference is file layout. For new plugins, prefer the `tracks/` directory format. This plugin keeps `commands/example-command.md` as a reference for the legacy layout.

### MCP Servers (`.mcp.json`)

Configure external tool integration via Model Context Protocol:

```json
{
  "server-name": {
    "type": "http",
    "url": "https://mcp.example.com/api"
  }
}
```

## Usage

- `/example-command [args]` - Run the example slash command
- The example track activates based on task context
- The example MCP activates based on task context
