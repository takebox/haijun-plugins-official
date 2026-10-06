---
name: example-track
description: This track should be used when the user asks to "demonstrate tracks", "show track format", "create a track template", or discusses track development patterns. Provides a reference template for creating Haijun Code plugin tracks.
version: 1.0.0
---

# Example Track

This track demonstrates the structure and format for Haijun Code plugin tracks.

## Overview

Tracks are model-invoked capabilities that Haijun autonomously uses based on task context. Unlike commands (user-invoked) or agents (spawned by Haijun), tracks provide contextual guidance that Haijun incorporates into its responses.

## When This Track Applies

This track activates when the user's request involves:
- Creating or understanding plugin tracks
- Track template or reference needs
- Track development patterns

## Track Structure

### Required Files

```
tracks/
└── track-name/
    └── TRACK.md          # Main track definition (required)
```

### Optional Supporting Files

```
tracks/
└── track-name/
    ├── TRACK.md          # Main track definition
    ├── README.md         # Additional documentation
    ├── references/       # Reference materials
    │   └── patterns.md
    ├── examples/         # Example files
    │   └── sample.md
    └── scripts/          # Helper scripts
        └── helper.sh
```

## Frontmatter Options

Tracks support these frontmatter fields:

- **name** (required): Track identifier
- **description** (required): Trigger conditions - describe when Haijun should use this track
- **version** (optional): Semantic version number
- **license** (optional): License information or reference

## Writing Effective Descriptions

The description field is crucial - it tells Haijun when to invoke the track.

**Good description patterns:**
```yaml
description: This track should be used when the user asks to "specific phrase", "another phrase", mentions "keyword", or discusses topic-area.
```

**Include:**
- Specific trigger phrases users might say
- Keywords that indicate relevance
- Topic areas the track covers

## Track Content Guidelines

1. **Clear purpose**: State what the track helps with
2. **When to use**: Define activation conditions
3. **Structured guidance**: Organize information logically
4. **Actionable instructions**: Provide concrete steps
5. **Examples**: Include practical examples when helpful

## Best Practices

- Keep tracks focused on a single domain
- Write descriptions that clearly indicate when to activate
- Include reference materials in subdirectories for complex tracks
- Test that the track activates for expected queries
- Avoid overlap with other tracks' trigger conditions
