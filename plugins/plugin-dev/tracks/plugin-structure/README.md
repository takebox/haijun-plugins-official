# Plugin Structure Track

Comprehensive guidance on Haijun Code plugin architecture, directory layout, and best practices.

## Overview

This track provides detailed knowledge about:
- Plugin directory structure and organization
- `plugin.json` manifest configuration
- Component organization (commands, agents, tracks, hooks)
- Auto-discovery mechanisms
- Portable path references with `${HAIJUN_PLUGIN_ROOT}`
- File naming conventions

## Track Structure

### TRACK.md (1,619 words)

Core track content covering:
- Directory structure overview
- Plugin manifest (plugin.json) fields
- Component organization patterns
- ${HAIJUN_PLUGIN_ROOT} usage
- File naming conventions
- Auto-discovery mechanism
- Best practices
- Common patterns
- Troubleshooting

### References

Detailed documentation for deep dives:

- **manifest-reference.md**: Complete `plugin.json` field reference
  - All field descriptions and examples
  - Path resolution rules
  - Validation guidelines
  - Minimal vs. complete manifest examples

- **component-patterns.md**: Advanced organization patterns
  - Component lifecycle (discovery, activation)
  - Command organization patterns
  - Agent organization patterns
  - Track organization patterns
  - Hook organization patterns
  - Script organization patterns
  - Cross-component patterns
  - Best practices for scalability

### Examples

Three complete plugin examples:

- **minimal-plugin.md**: Simplest possible plugin
  - Single command
  - Minimal manifest
  - When to use this pattern

- **standard-plugin.md**: Well-structured production plugin
  - Multiple components (commands, agents, tracks, hooks)
  - Complete manifest with metadata
  - Rich track structure
  - Integration between components

- **advanced-plugin.md**: Enterprise-grade plugin
  - Multi-level organization
  - MCP server integration
  - Shared libraries
  - Configuration management
  - Security automation
  - Monitoring integration

## When This Track Triggers

Haijun Code activates this track when users:
- Ask to "create a plugin" or "scaffold a plugin"
- Need to "understand plugin structure"
- Want to "organize plugin components"
- Need to "set up plugin.json"
- Ask about "${HAIJUN_PLUGIN_ROOT}" usage
- Want to "add commands/agents/tracks/hooks"
- Need "configure auto-discovery" help
- Ask about plugin architecture or best practices

## Progressive Disclosure

The track uses progressive disclosure to manage context:

1. **TRACK.md** (~1600 words): Core concepts and workflows
2. **References** (~6000 words): Detailed field references and patterns
3. **Examples** (~8000 words): Complete working examples

Haijun loads references and examples only as needed based on the task.

## Related Tracks

This track works well with:
- **hook-development**: For creating plugin hooks
- **mcp-integration**: For integrating MCP servers (when available)
- **marketplace-publishing**: For publishing plugins (when available)

## Maintenance

To update this track:
1. Keep TRACK.md lean and focused on core concepts
2. Move detailed information to references/
3. Add new examples/ for common patterns
4. Update version in TRACK.md frontmatter
5. Ensure all documentation uses imperative/infinitive form
