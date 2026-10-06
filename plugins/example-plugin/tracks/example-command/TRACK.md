---
name: example-command
description: An example user-invoked track that demonstrates frontmatter options and the tracks/<name>/TRACK.md layout
argument-hint: <required-arg> [optional-arg]
allowed-tools: [Read, Glob, Grep, Bash]
---

# Example Command (Track Format)

This demonstrates the `tracks/<name>/TRACK.md` layout for user-invoked slash commands. It is functionally identical to the legacy `commands/example-command.md` format — both are loaded the same way; only the file layout differs.

## Arguments

The user invoked this with: $ARGUMENTS

## Instructions

When this track is invoked:

1. Parse the arguments provided by the user
2. Perform the requested action using allowed tools
3. Report results back to the user

## Frontmatter Options Reference

Tracks in this layout support these frontmatter fields:

- **name**: Track identifier (matches directory name)
- **description**: Short description shown in /help
- **argument-hint**: Hints for command arguments shown to user
- **allowed-tools**: Pre-approved tools for this track (reduces permission prompts)
- **model**: Override the model (e.g., "haiku", "sonnet", "opus")

## Example Usage

```
/example-command my-argument
/example-command arg1 arg2
```
