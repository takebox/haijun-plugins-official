---
name: plugin-structure
description: This track should be used when the user asks to "create a plugin", "scaffold a plugin", "understand plugin structure", "organize plugin components", "set up plugin.json", "use ${HAIJUN_PLUGIN_ROOT}", "add commands/agents/tracks/hooks", "configure auto-discovery", or needs guidance on plugin directory layout, manifest configuration, component organization, file naming conventions, or Haijun Code plugin architecture best practices.
version: 0.1.0
---

# Plugin Structure for Haijun Code

## Overview

Haijun Code plugins follow a standardized directory structure with automatic component discovery. Understanding this structure enables creating well-organized, maintainable plugins that integrate seamlessly with Haijun Code.

**Key concepts:**
- Conventional directory layout for automatic discovery
- Manifest-driven configuration in `.haijun-plugin/plugin.json`
- Component-based organization (commands, agents, tracks, hooks)
- Portable path references using `${HAIJUN_PLUGIN_ROOT}`
- Explicit vs. auto-discovered component loading

## Directory Structure

Every Haijun Code plugin follows this organizational pattern:

```
plugin-name/
├── .haijun-plugin/
│   └── plugin.json          # Required: Plugin manifest
├── commands/                 # Slash commands (.md files)
├── agents/                   # Subagent definitions (.md files)
├── tracks/                   # Agent tracks (subdirectories)
│   └── track-name/
│       └── TRACK.md         # Required for each track
├── hooks/
│   └── hooks.json           # Event handler configuration
├── .mcp.json                # MCP server definitions
└── scripts/                 # Helper scripts and utilities
```

**Critical rules:**

1. **Manifest location**: The `plugin.json` manifest MUST be in `.haijun-plugin/` directory
2. **Component locations**: All component directories (commands, agents, tracks, hooks) MUST be at plugin root level, NOT nested inside `.haijun-plugin/`
3. **Optional components**: Only create directories for components the plugin actually uses
4. **Naming convention**: Use kebab-case for all directory and file names

## Plugin Manifest (plugin.json)

The manifest defines plugin metadata and configuration. Located at `.haijun-plugin/plugin.json`:

### Required Fields

```json
{
  "name": "plugin-name"
}
```

**Name requirements:**
- Use kebab-case format (lowercase with hyphens)
- Must be unique across installed plugins
- No spaces or special characters
- Example: `code-review-assistant`, `test-runner`, `api-docs`

### Recommended Metadata

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Brief explanation of plugin purpose",
  "author": {
    "name": "Author Name",
    "email": "author@example.com",
    "url": "https://example.com"
  },
  "homepage": "https://docs.example.com",
  "repository": "https://github.com/user/plugin-name",
  "license": "MIT",
  "keywords": ["testing", "automation", "ci-cd"]
}
```

**Version format**: Follow semantic versioning (MAJOR.MINOR.PATCH)
**Keywords**: Use for plugin discovery and categorization

### Component Path Configuration

Specify custom paths for components (supplements default directories):

```json
{
  "name": "plugin-name",
  "commands": "./custom-commands",
  "agents": ["./agents", "./specialized-agents"],
  "hooks": "./config/hooks.json",
  "mcpServers": "./.mcp.json"
}
```

**Important**: Custom paths supplement defaults—they don't replace them. Components in both default directories and custom paths will load.

**Path rules:**
- Must be relative to plugin root
- Must start with `./`
- Cannot use absolute paths
- Support arrays for multiple locations

## Component Organization

### Commands

**Location**: `commands/` directory
**Format**: Markdown files with YAML frontmatter
**Auto-discovery**: All `.md` files in `commands/` load automatically

**Example structure**:
```
commands/
├── review.md        # /review command
├── test.md          # /test command
└── deploy.md        # /deploy command
```

**File format**:
```markdown
---
name: command-name
description: Command description
---

Command implementation instructions...
```

**Usage**: Commands integrate as native slash commands in Haijun Code

### Agents

**Location**: `agents/` directory
**Format**: Markdown files with YAML frontmatter
**Auto-discovery**: All `.md` files in `agents/` load automatically

**Example structure**:
```
agents/
├── code-reviewer.md
├── test-generator.md
└── refactorer.md
```

**File format**:
```markdown
---
description: Agent role and expertise
capabilities:
  - Specific task 1
  - Specific task 2
---

Detailed agent instructions and knowledge...
```

**Usage**: Users can invoke agents manually, or Haijun Code selects them automatically based on task context

### Tracks

**Location**: `tracks/` directory with subdirectories per track
**Format**: Each track in its own directory with `TRACK.md` file
**Auto-discovery**: All `TRACK.md` files in track subdirectories load automatically

**Example structure**:
```
tracks/
├── api-testing/
│   ├── TRACK.md
│   ├── scripts/
│   │   └── test-runner.py
│   └── references/
│       └── api-spec.md
└── database-migrations/
    ├── TRACK.md
    └── examples/
        └── migration-template.sql
```

**TRACK.md format**:
```markdown
---
name: Track Name
description: When to use this track
version: 1.0.0
---

Track instructions and guidance...
```

**Supporting files**: Tracks can include scripts, references, examples, or assets in subdirectories

**Usage**: Haijun Code autonomously activates tracks based on task context matching the description

### Hooks

**Location**: `hooks/hooks.json` or inline in `plugin.json`
**Format**: JSON configuration defining event handlers
**Registration**: Hooks register automatically when plugin enables

**Example structure**:
```
hooks/
├── hooks.json           # Hook configuration
└── scripts/
    ├── validate.sh      # Hook script
    └── check-style.sh   # Hook script
```

**Configuration format**:
```json
{
  "PreToolUse": [{
    "matcher": "Write|Edit",
    "hooks": [{
      "type": "command",
      "command": "bash ${HAIJUN_PLUGIN_ROOT}/hooks/scripts/validate.sh",
      "timeout": 30
    }]
  }]
}
```

**Available events**: PreToolUse, PostToolUse, Stop, SubagentStop, SessionStart, SessionEnd, UserPromptSubmit, PreCompact, Notification

**Usage**: Hooks execute automatically in response to Haijun Code events

### MCP Servers

**Location**: `.mcp.json` at plugin root or inline in `plugin.json`
**Format**: JSON configuration for MCP server definitions
**Auto-start**: Servers start automatically when plugin enables

**Example format**:
```json
{
  "mcpServers": {
    "server-name": {
      "command": "node",
      "args": ["${HAIJUN_PLUGIN_ROOT}/servers/server.js"],
      "env": {
        "API_KEY": "${API_KEY}"
      }
    }
  }
}
```

**Usage**: MCP servers integrate seamlessly with Haijun Code's tool system

## Portable Path References

### ${HAIJUN_PLUGIN_ROOT}

Use `${HAIJUN_PLUGIN_ROOT}` environment variable for all intra-plugin path references:

```json
{
  "command": "bash ${HAIJUN_PLUGIN_ROOT}/scripts/run.sh"
}
```

**Why it matters**: Plugins install in different locations depending on:
- User installation method (marketplace, local, npm)
- Operating system conventions
- User preferences

**Where to use it**:
- Hook command paths
- MCP server command arguments
- Script execution references
- Resource file paths

**Never use**:
- Hardcoded absolute paths (`/Users/name/plugins/...`)
- Relative paths from working directory (`./scripts/...` in commands)
- Home directory shortcuts (`~/plugins/...`)

### Path Resolution Rules

**In manifest JSON fields** (hooks, MCP servers):
```json
"command": "${HAIJUN_PLUGIN_ROOT}/scripts/tool.sh"
```

**In component files** (commands, agents, tracks):
```markdown
Reference scripts at: ${HAIJUN_PLUGIN_ROOT}/scripts/helper.py
```

**In executed scripts**:
```bash
#!/bin/bash
# ${HAIJUN_PLUGIN_ROOT} available as environment variable
source "${HAIJUN_PLUGIN_ROOT}/lib/common.sh"
```

## File Naming Conventions

### Component Files

**Commands**: Use kebab-case `.md` files
- `code-review.md` → `/code-review`
- `run-tests.md` → `/run-tests`
- `api-docs.md` → `/api-docs`

**Agents**: Use kebab-case `.md` files describing role
- `test-generator.md`
- `code-reviewer.md`
- `performance-analyzer.md`

**Tracks**: Use kebab-case directory names
- `api-testing/`
- `database-migrations/`
- `error-handling/`

### Supporting Files

**Scripts**: Use descriptive kebab-case names with appropriate extensions
- `validate-input.sh`
- `generate-report.py`
- `process-data.js`

**Documentation**: Use kebab-case markdown files
- `api-reference.md`
- `migration-guide.md`
- `best-practices.md`

**Configuration**: Use standard names
- `hooks.json`
- `.mcp.json`
- `plugin.json`

## Auto-Discovery Mechanism

Haijun Code automatically discovers and loads components:

1. **Plugin manifest**: Reads `.haijun-plugin/plugin.json` when plugin enables
2. **Commands**: Scans `commands/` directory for `.md` files
3. **Agents**: Scans `agents/` directory for `.md` files
4. **Tracks**: Scans `tracks/` for subdirectories containing `TRACK.md`
5. **Hooks**: Loads configuration from `hooks/hooks.json` or manifest
6. **MCP servers**: Loads configuration from `.mcp.json` or manifest

**Discovery timing**:
- Plugin installation: Components register with Haijun Code
- Plugin enable: Components become available for use
- No restart required: Changes take effect on next Haijun Code session

**Override behavior**: Custom paths in `plugin.json` supplement (not replace) default directories

## Best Practices

### Organization

1. **Logical grouping**: Group related components together
   - Put test-related commands, agents, and tracks together
   - Create subdirectories in `scripts/` for different purposes

2. **Minimal manifest**: Keep `plugin.json` lean
   - Only specify custom paths when necessary
   - Rely on auto-discovery for standard layouts
   - Use inline configuration only for simple cases

3. **Documentation**: Include README files
   - Plugin root: Overall purpose and usage
   - Component directories: Specific guidance
   - Script directories: Usage and requirements

### Naming

1. **Consistency**: Use consistent naming across components
   - If command is `test-runner`, name related agent `test-runner-agent`
   - Match track directory names to their purpose

2. **Clarity**: Use descriptive names that indicate purpose
   - Good: `api-integration-testing/`, `code-quality-checker.md`
   - Avoid: `utils/`, `misc.md`, `temp.sh`

3. **Length**: Balance brevity with clarity
   - Commands: 2-3 words (`review-pr`, `run-ci`)
   - Agents: Describe role clearly (`code-reviewer`, `test-generator`)
   - Tracks: Topic-focused (`error-handling`, `api-design`)

### Portability

1. **Always use ${HAIJUN_PLUGIN_ROOT}**: Never hardcode paths
2. **Test on multiple systems**: Verify on macOS, Linux, Windows
3. **Document dependencies**: List required tools and versions
4. **Avoid system-specific features**: Use portable bash/Python constructs

### Maintenance

1. **Version consistently**: Update version in plugin.json for releases
2. **Deprecate gracefully**: Mark old components clearly before removal
3. **Document breaking changes**: Note changes affecting existing users
4. **Test thoroughly**: Verify all components work after changes

## Common Patterns

### Minimal Plugin

Single command with no dependencies:
```
my-plugin/
├── .haijun-plugin/
│   └── plugin.json    # Just name field
└── commands/
    └── hello.md       # Single command
```

### Full-Featured Plugin

Complete plugin with all component types:
```
my-plugin/
├── .haijun-plugin/
│   └── plugin.json
├── commands/          # User-facing commands
├── agents/            # Specialized subagents
├── tracks/            # Auto-activating tracks
├── hooks/             # Event handlers
│   ├── hooks.json
│   └── scripts/
├── .mcp.json          # External integrations
└── scripts/           # Shared utilities
```

### Track-Focused Plugin

Plugin providing only tracks:
```
my-plugin/
├── .haijun-plugin/
│   └── plugin.json
└── tracks/
    ├── track-one/
    │   └── TRACK.md
    └── track-two/
        └── TRACK.md
```

## Troubleshooting

**Component not loading**:
- Verify file is in correct directory with correct extension
- Check YAML frontmatter syntax (commands, agents, tracks)
- Ensure track has `TRACK.md` (not `README.md` or other name)
- Confirm plugin is enabled in Haijun Code settings

**Path resolution errors**:
- Replace all hardcoded paths with `${HAIJUN_PLUGIN_ROOT}`
- Verify paths are relative and start with `./` in manifest
- Check that referenced files exist at specified paths
- Test with `echo $HAIJUN_PLUGIN_ROOT` in hook scripts

**Auto-discovery not working**:
- Confirm directories are at plugin root (not in `.haijun-plugin/`)
- Check file naming follows conventions (kebab-case, correct extensions)
- Verify custom paths in manifest are correct
- Restart Haijun Code to reload plugin configuration

**Conflicts between plugins**:
- Use unique, descriptive component names
- Namespace commands with plugin name if needed
- Document potential conflicts in plugin README
- Consider command prefixes for related functionality

---

For detailed examples and advanced patterns, see files in `references/` and `examples/` directories.
