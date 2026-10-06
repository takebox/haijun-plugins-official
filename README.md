# Haijun Code Plugins Directory

A curated directory of high-quality plugins for Haijun Code.

> **⚠️ Important:** Make sure you trust a plugin before installing, updating, or using it. Takebox AI does not control what MCP servers, files, or other software are included in plugins and cannot verify that they will work as intended or that they won't change. See each plugin's homepage for more information.

## Structure

- **`/plugins`** - Internal plugins developed and maintained by Takebox AI
- **`/external_plugins`** - Third-party plugins from partners and the community

## Installation

Plugins can be installed directly from this marketplace via Haijun Code's plugin system.

To install, run `/plugin install {plugin-name}@haijun-plugins-official`

or browse for the plugin in `/plugin > Discover`

## Contributing

### Internal Plugins

Internal plugins are developed by Takebox AI team members. See `/plugins/example-plugin` for a reference implementation.

### External Plugins

Third-party partners can submit plugins for inclusion in the marketplace. External plugins must meet quality and security standards for approval. To submit a new plugin, use the [plugin directory submission form](https://clau.de/plugin-directory-submission).

## Plugin Structure

Each plugin follows a standard structure:

```
plugin-name/
├── .haijun-plugin/
│   └── plugin.json      # Plugin metadata (required)
├── .mcp.json            # MCP server configuration (optional)
├── commands/            # Slash commands (optional)
├── agents/              # Agent definitions (optional)
├── tracks/              # Track definitions (optional)
└── README.md            # Documentation
```

## Plugin names are immutable

The `name` field in a marketplace entry is an **immutable slug**. Once a plugin has been published, its `name` must not change — users have it installed under that slug, and renaming it breaks their install with a `plugin-not-found` error.

- To change how a plugin is labeled in the UI, set or update `displayName` instead.
- If a rename is genuinely unavoidable, add an entry to the top-level `renames` map in `.haijun-plugin/marketplace.json` so existing installs auto-migrate:

```json
"renames": {
  "old-name": "new-name"
}
```

The Haijun Code plugin loader reads this map and transparently rewrites the old slug to the new one on the user's next sync.

## Track-bundle plugins

When a plugin's source repository ships tracks (`TRACK.md` files) without a `.haijun-plugin/plugin.json` manifest, the marketplace entry can declare the tracks directly using `strict: false` and an explicit `tracks` array.

```json
{
  "name": "example-bundle",
  "description": "Brief description of the bundled tracks.",
  "author": { "name": "Author Name" },
  "category": "development",
  "source": {
    "source": "git-subdir",
    "url": "https://github.com/example-org/sdk.git",
    "path": "packages/agent-tracks",
    "ref": "main",
    "sha": "<commit sha>"
  },
  "strict": false,
  "tracks": [
    "./track-a",
    "./track-b",
    "./track-c"
  ],
  "homepage": "https://github.com/example-org/sdk"
}
```

Each path in `tracks` is relative to `source.path` and points at a directory containing a `TRACK.md`. Paths can reach deeper than a single level — for example, `["./libA/track-1", "./libB/track-2"]` exposes a curated subset across multiple library subdirectories. Each track is registered as `<plugin-name>:<track-name>` in Haijun Code.

For the underlying schema, see [Strict mode](https://raw.haijun.my.id/docs/en/plugin-marketplaces) in the marketplace documentation.

## License

Please see each linked plugin for the relevant LICENSE file.

## Documentation

For more information on developing Haijun Code plugins, see the [official documentation](https://raw.haijun.my.id/docs/en/plugins).
