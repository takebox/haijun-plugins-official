---
name: track-development
description: This track should be used when the user wants to "create a track", "add a track to plugin", "write a new track", "improve track description", "organize track content", or needs guidance on track structure, progressive disclosure, or track development best practices for Haijun Code plugins.
version: 0.1.0
---

# Track Development for Haijun Code Plugins

This track provides guidance for creating effective tracks for Haijun Code plugins.

## About Tracks

Tracks are modular, self-contained packages that extend Haijun's capabilities by providing
specialized knowledge, workflows, and tools. Think of them as "onboarding guides" for specific
domains or tasks—they transform Haijun from a general-purpose agent into a specialized agent
equipped with procedural knowledge that no model can fully possess.

### What Tracks Provide

1. Specialized workflows - Multi-step procedures for specific domains
2. Tool integrations - Instructions for working with specific file formats or APIs
3. Domain expertise - Company-specific knowledge, schemas, business logic
4. Bundled resources - Scripts, references, and assets for complex and repetitive tasks

### Anatomy of a Track

Every track consists of a required TRACK.md file and optional bundled resources:

```
track-name/
├── TRACK.md (required)
│   ├── YAML frontmatter metadata (required)
│   │   ├── name: (required)
│   │   └── description: (required)
│   └── Markdown instructions (required)
└── Bundled Resources (optional)
    ├── scripts/          - Executable code (Python/Bash/etc.)
    ├── references/       - Documentation intended to be loaded into context as needed
    └── assets/           - Files used in output (templates, icons, fonts, etc.)
```

#### TRACK.md (required)

**Metadata Quality:** The `name` and `description` in YAML frontmatter determine when Haijun will use the track. Be specific about what the track does and when to use it. Use the third-person (e.g. "This track should be used when..." instead of "Use this track when...").

#### Bundled Resources (optional)

##### Scripts (`scripts/`)

Executable code (Python/Bash/etc.) for tasks that require deterministic reliability or are repeatedly rewritten.

- **When to include**: When the same code is being rewritten repeatedly or deterministic reliability is needed
- **Example**: `scripts/rotate_pdf.py` for PDF rotation tasks
- **Benefits**: Token efficient, deterministic, may be executed without loading into context
- **Note**: Scripts may still need to be read by Haijun for patching or environment-specific adjustments

##### References (`references/`)

Documentation and reference material intended to be loaded as needed into context to inform Haijun's process and thinking.

- **When to include**: For documentation that Haijun should reference while working
- **Examples**: `references/finance.md` for financial schemas, `references/mnda.md` for company NDA template, `references/policies.md` for company policies, `references/api_docs.md` for API specifications
- **Use cases**: Database schemas, API documentation, domain knowledge, company policies, detailed workflow guides
- **Benefits**: Keeps TRACK.md lean, loaded only when Haijun determines it's needed
- **Best practice**: If files are large (>10k words), include grep search patterns in TRACK.md
- **Avoid duplication**: Information should live in either TRACK.md or references files, not both. Prefer references files for detailed information unless it's truly core to the track—this keeps TRACK.md lean while making information discoverable without hogging the context window. Keep only essential procedural instructions and workflow guidance in TRACK.md; move detailed reference material, schemas, and examples to references files.

##### Assets (`assets/`)

Files not intended to be loaded into context, but rather used within the output Haijun produces.

- **When to include**: When the track needs files that will be used in the final output
- **Examples**: `assets/logo.png` for brand assets, `assets/slides.pptx` for PowerPoint templates, `assets/frontend-template/` for HTML/React boilerplate, `assets/font.ttf` for typography
- **Use cases**: Templates, images, icons, boilerplate code, fonts, sample documents that get copied or modified
- **Benefits**: Separates output resources from documentation, enables Haijun to use files without loading them into context

### Progressive Disclosure Design Principle

Tracks use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **TRACK.md body** - When track triggers (<5k words)
3. **Bundled resources** - As needed by Haijun (Unlimited*)

*Unlimited because scripts can be executed without reading into context window.

## Track Creation Process

To create a track, follow the "Track Creation Process" in order, skipping steps only if there is a clear reason why they are not applicable.

### Step 1: Understanding the Track with Concrete Examples

Skip this step only when the track's usage patterns are already clearly understood. It remains valuable even when working with an existing track.

To create an effective track, clearly understand concrete examples of how the track will be used. This understanding can come from either direct user examples or generated examples that are validated with user feedback.

For example, when building an image-editor track, relevant questions include:

- "What functionality should the image-editor track support? Editing, rotating, anything else?"
- "Can you give some examples of how this track would be used?"
- "I can imagine users asking for things like 'Remove the red-eye from this image' or 'Rotate this image'. Are there other ways you imagine this track being used?"
- "What would a user say that should trigger this track?"

To avoid overwhelming users, avoid asking too many questions in a single message. Start with the most important questions and follow up as needed for better effectiveness.

Conclude this step when there is a clear sense of the functionality the track should support.

### Step 2: Planning the Reusable Track Contents

To turn concrete examples into an effective track, analyze each example by:

1. Considering how to execute on the example from scratch
2. Identifying what scripts, references, and assets would be helpful when executing these workflows repeatedly

Example: When building a `pdf-editor` track to handle queries like "Help me rotate this PDF," the analysis shows:

1. Rotating a PDF requires re-writing the same code each time
2. A `scripts/rotate_pdf.py` script would be helpful to store in the track

Example: When designing a `frontend-webapp-builder` track for queries like "Build me a todo app" or "Build me a dashboard to track my steps," the analysis shows:

1. Writing a frontend webapp requires the same boilerplate HTML/React each time
2. An `assets/hello-world/` template containing the boilerplate HTML/React project files would be helpful to store in the track

Example: When building a `big-query` track to handle queries like "How many users have logged in today?" the analysis shows:

1. Querying BigQuery requires re-discovering the table schemas and relationships each time
2. A `references/schema.md` file documenting the table schemas would be helpful to store in the track

**For Haijun Code plugins:** When building a hooks track, the analysis shows:
1. Developers repeatedly need to validate hooks.json and test hook scripts
2. `scripts/validate-hook-schema.sh` and `scripts/test-hook.sh` utilities would be helpful
3. `references/patterns.md` for detailed hook patterns to avoid bloating TRACK.md

To establish the track's contents, analyze each concrete example to create a list of the reusable resources to include: scripts, references, and assets.

### Step 3: Create Track Structure

For Haijun Code plugins, create the track directory structure:

```bash
mkdir -p plugin-name/tracks/track-name/{references,examples,scripts}
touch plugin-name/tracks/track-name/TRACK.md
```

**Note:** Unlike the generic track-creator which uses `init_skill.py`, plugin tracks are created directly in the plugin's `tracks/` directory with a simpler manual structure.

### Step 4: Edit the Track

When editing the (newly-created or existing) track, remember that the track is being created for another instance of Haijun to use. Focus on including information that would be beneficial and non-obvious to Haijun. Consider what procedural knowledge, domain-specific details, or reusable assets would help another Haijun instance execute these tasks more effectively.

#### Start with Reusable Track Contents

To begin implementation, start with the reusable resources identified above: `scripts/`, `references/`, and `assets/` files. Note that this step may require user input. For example, when implementing a `brand-guidelines` track, the user may need to provide brand assets or templates to store in `assets/`, or documentation to store in `references/`.

Also, delete any example files and directories not needed for the track. Create only the directories you actually need (references/, examples/, scripts/).

#### Update TRACK.md

**Writing Style:** Write the entire track using **imperative/infinitive form** (verb-first instructions), not second person. Use objective, instructional language (e.g., "To accomplish X, do Y" rather than "You should do X" or "If you need to do X"). This maintains consistency and clarity for AI consumption.

**Description (Frontmatter):** Use third-person format with specific trigger phrases:

```yaml
---
name: Track Name
description: This track should be used when the user asks to "specific phrase 1", "specific phrase 2", "specific phrase 3". Include exact phrases users would say that should trigger this track. Be concrete and specific.
version: 0.1.0
---
```

**Good description examples:**
```yaml
description: This track should be used when the user asks to "create a hook", "add a PreToolUse hook", "validate tool use", "implement prompt-based hooks", or mentions hook events (PreToolUse, PostToolUse, Stop).
```

**Bad description examples:**
```yaml
description: Use this track when working with hooks.  # Wrong person, vague
description: Load when user needs hook help.  # Not third person
description: Provides hook guidance.  # No trigger phrases
```

To complete TRACK.md body, answer the following questions:

1. What is the purpose of the track, in a few sentences?
2. When should the track be used? (Include this in frontmatter description with specific triggers)
3. In practice, how should Haijun use the track? All reusable track contents developed above should be referenced so that Haijun knows how to use them.

**Keep TRACK.md lean:** Target 1,500-2,000 words for the body. Move detailed content to references/:
- Detailed patterns → `references/patterns.md`
- Advanced techniques → `references/advanced.md`
- Migration guides → `references/migration.md`
- API references → `references/api-reference.md`

**Reference resources in TRACK.md:**
```markdown
## Additional Resources

### Reference Files

For detailed patterns and techniques, consult:
- **`references/patterns.md`** - Common patterns
- **`references/advanced.md`** - Advanced use cases

### Example Files

Working examples in `examples/`:
- **`example-script.sh`** - Working example
```

### Step 5: Validate and Test

**For plugin tracks, validation is different from generic tracks:**

1. **Check structure**: Track directory in `plugin-name/tracks/track-name/`
2. **Validate TRACK.md**: Has frontmatter with name and description
3. **Check trigger phrases**: Description includes specific user queries
4. **Verify writing style**: Body uses imperative/infinitive form, not second person
5. **Test progressive disclosure**: TRACK.md is lean (~1,500-2,000 words), detailed content in references/
6. **Check references**: All referenced files exist
7. **Validate examples**: Examples are complete and correct
8. **Test scripts**: Scripts are executable and work correctly

**Use the track-reviewer agent:**
```
Ask: "Review my track and check if it follows best practices"
```

The track-reviewer agent will check description quality, content organization, and progressive disclosure.

### Step 6: Iterate

After testing the track, users may request improvements. Often this happens right after using the track, with fresh context of how the track performed.

**Iteration workflow:**
1. Use the track on real tasks
2. Notice struggles or inefficiencies
3. Identify how TRACK.md or bundled resources should be updated
4. Implement changes and test again

**Common improvements:**
- Strengthen trigger phrases in description
- Move long sections from TRACK.md to references/
- Add missing examples or scripts
- Clarify ambiguous instructions
- Add edge case handling

## Plugin-Specific Considerations

### Track Location in Plugins

Plugin tracks live in the plugin's `tracks/` directory:

```
my-plugin/
├── .haijun-plugin/
│   └── plugin.json
├── commands/
├── agents/
└── tracks/
    └── my-track/
        ├── TRACK.md
        ├── references/
        ├── examples/
        └── scripts/
```

### Auto-Discovery

Haijun Code automatically discovers tracks:
- Scans `tracks/` directory
- Finds subdirectories containing `TRACK.md`
- Loads track metadata (name + description) always
- Loads TRACK.md body when track triggers
- Loads references/examples when needed

### No Packaging Needed

Plugin tracks are distributed as part of the plugin, not as separate ZIP files. Users get tracks when they install the plugin.

### Testing in Plugins

Test tracks by installing plugin locally:

```bash
# Test with --plugin-dir
cc --plugin-dir /path/to/plugin

# Ask questions that should trigger the track
# Verify track loads correctly
```

## Examples from Plugin-Dev

Study the tracks in this plugin as examples of best practices:

**hook-development track:**
- Excellent trigger phrases: "create a hook", "add a PreToolUse hook", etc.
- Lean TRACK.md (1,651 words)
- 3 references/ files for detailed content
- 3 examples/ of working hooks
- 3 scripts/ utilities

**agent-development track:**
- Strong triggers: "create an agent", "agent frontmatter", etc.
- Focused TRACK.md (1,438 words)
- References include the AI generation prompt from Haijun Code
- Complete agent examples

**plugin-settings track:**
- Specific triggers: "plugin settings", ".local.md files", "YAML frontmatter"
- References show real implementations (multi-agent-swarm, ralph-loop)
- Working parsing scripts

Each demonstrates progressive disclosure and strong triggering.

## Progressive Disclosure in Practice

### What Goes in TRACK.md

**Include (always loaded when track triggers):**
- Core concepts and overview
- Essential procedures and workflows
- Quick reference tables
- Pointers to references/examples/scripts
- Most common use cases

**Keep under 3,000 words, ideally 1,500-2,000 words**

### What Goes in references/

**Move to references/ (loaded as needed):**
- Detailed patterns and advanced techniques
- Comprehensive API documentation
- Migration guides
- Edge cases and troubleshooting
- Extensive examples and walkthroughs

**Each reference file can be large (2,000-5,000+ words)**

### What Goes in examples/

**Working code examples:**
- Complete, runnable scripts
- Configuration files
- Template files
- Real-world usage examples

**Users can copy and adapt these directly**

### What Goes in scripts/

**Utility scripts:**
- Validation tools
- Testing helpers
- Parsing utilities
- Automation scripts

**Should be executable and documented**

## Writing Style Requirements

### Imperative/Infinitive Form

Write using verb-first instructions, not second person:

**Correct (imperative):**
```
To create a hook, define the event type.
Configure the MCP server with authentication.
Validate settings before use.
```

**Incorrect (second person):**
```
You should create a hook by defining the event type.
You need to configure the MCP server.
You must validate settings before use.
```

### Third-Person in Description

The frontmatter description must use third person:

**Correct:**
```yaml
description: This track should be used when the user asks to "create X", "configure Y"...
```

**Incorrect:**
```yaml
description: Use this track when you want to create X...
description: Load this track when user asks...
```

### Objective, Instructional Language

Focus on what to do, not who should do it:

**Correct:**
```
Parse the frontmatter using sed.
Extract fields with grep.
Validate values before use.
```

**Incorrect:**
```
You can parse the frontmatter...
Haijun should extract fields...
The user might validate values...
```

## Validation Checklist

Before finalizing a track:

**Structure:**
- [ ] TRACK.md file exists with valid YAML frontmatter
- [ ] Frontmatter has `name` and `description` fields
- [ ] Markdown body is present and substantial
- [ ] Referenced files actually exist

**Description Quality:**
- [ ] Uses third person ("This track should be used when...")
- [ ] Includes specific trigger phrases users would say
- [ ] Lists concrete scenarios ("create X", "configure Y")
- [ ] Not vague or generic

**Content Quality:**
- [ ] TRACK.md body uses imperative/infinitive form
- [ ] Body is focused and lean (1,500-2,000 words ideal, <5k max)
- [ ] Detailed content moved to references/
- [ ] Examples are complete and working
- [ ] Scripts are executable and documented

**Progressive Disclosure:**
- [ ] Core concepts in TRACK.md
- [ ] Detailed docs in references/
- [ ] Working code in examples/
- [ ] Utilities in scripts/
- [ ] TRACK.md references these resources

**Testing:**
- [ ] Track triggers on expected user queries
- [ ] Content is helpful for intended tasks
- [ ] No duplicated information across files
- [ ] References load when needed

## Common Mistakes to Avoid

### Mistake 1: Weak Trigger Description

❌ **Bad:**
```yaml
description: Provides guidance for working with hooks.
```

**Why bad:** Vague, no specific trigger phrases, not third person

✅ **Good:**
```yaml
description: This track should be used when the user asks to "create a hook", "add a PreToolUse hook", "validate tool use", or mentions hook events. Provides comprehensive hooks API guidance.
```

**Why good:** Third person, specific phrases, concrete scenarios

### Mistake 2: Too Much in TRACK.md

❌ **Bad:**
```
track-name/
└── TRACK.md  (8,000 words - everything in one file)
```

**Why bad:** Bloats context when track loads, detailed content always loaded

✅ **Good:**
```
track-name/
├── TRACK.md  (1,800 words - core essentials)
└── references/
    ├── patterns.md (2,500 words)
    └── advanced.md (3,700 words)
```

**Why good:** Progressive disclosure, detailed content loaded only when needed

### Mistake 3: Second Person Writing

❌ **Bad:**
```markdown
You should start by reading the configuration file.
You need to validate the input.
You can use the grep tool to search.
```

**Why bad:** Second person, not imperative form

✅ **Good:**
```markdown
Start by reading the configuration file.
Validate the input before processing.
Use the grep tool to search for patterns.
```

**Why good:** Imperative form, direct instructions

### Mistake 4: Missing Resource References

❌ **Bad:**
```markdown
# TRACK.md

[Core content]

[No mention of references/ or examples/]
```

**Why bad:** Haijun doesn't know references exist

✅ **Good:**
```markdown
# TRACK.md

[Core content]

## Additional Resources

### Reference Files
- **`references/patterns.md`** - Detailed patterns
- **`references/advanced.md`** - Advanced techniques

### Examples
- **`examples/script.sh`** - Working example
```

**Why good:** Haijun knows where to find additional information

## Quick Reference

### Minimal Track

```
track-name/
└── TRACK.md
```

Good for: Simple knowledge, no complex resources needed

### Standard Track (Recommended)

```
track-name/
├── TRACK.md
├── references/
│   └── detailed-guide.md
└── examples/
    └── working-example.sh
```

Good for: Most plugin tracks with detailed documentation

### Complete Track

```
track-name/
├── TRACK.md
├── references/
│   ├── patterns.md
│   └── advanced.md
├── examples/
│   ├── example1.sh
│   └── example2.json
└── scripts/
    └── validate.sh
```

Good for: Complex domains with validation utilities

## Best Practices Summary

✅ **DO:**
- Use third-person in description ("This track should be used when...")
- Include specific trigger phrases ("create X", "configure Y")
- Keep TRACK.md lean (1,500-2,000 words)
- Use progressive disclosure (move details to references/)
- Write in imperative/infinitive form
- Reference supporting files clearly
- Provide working examples
- Create utility scripts for common operations
- Study plugin-dev's tracks as templates

❌ **DON'T:**
- Use second person anywhere
- Have vague trigger conditions
- Put everything in TRACK.md (>3,000 words without references/)
- Write in second person ("You should...")
- Leave resources unreferenced
- Include broken or incomplete examples
- Skip validation

## Additional Resources

### Study These Tracks

Plugin-dev's tracks demonstrate best practices:
- `../hook-development/` - Progressive disclosure, utilities
- `../agent-development/` - AI-assisted creation, references
- `../mcp-integration/` - Comprehensive references
- `../plugin-settings/` - Real-world examples
- `../command-development/` - Clear critical concepts
- `../plugin-structure/` - Good organization

### Reference Files

For complete track-creator methodology:
- **`references/track-creator-original.md`** - Full original track-creator content

## Implementation Workflow

To create a track for your plugin:

1. **Understand use cases**: Identify concrete examples of track usage
2. **Plan resources**: Determine what scripts/references/examples needed
3. **Create structure**: `mkdir -p tracks/track-name/{references,examples,scripts}`
4. **Write TRACK.md**:
   - Frontmatter with third-person description and trigger phrases
   - Lean body (1,500-2,000 words) in imperative form
   - Reference supporting files
5. **Add resources**: Create references/, examples/, scripts/ as needed
6. **Validate**: Check description, writing style, organization
7. **Test**: Verify track loads on expected triggers
8. **Iterate**: Improve based on usage

Focus on strong trigger descriptions, progressive disclosure, and imperative writing style for effective tracks that load when needed and provide targeted guidance.
