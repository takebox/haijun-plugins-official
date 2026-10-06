# Tracks Recommendations

Tracks are packaged expertise with workflows, reference materials, and best practices. Create them in `.haijun/tracks/<name>/TRACK.md`. Tracks can be invoked by Haijun automatically when relevant, or by users directly with `/track-name`.

Some pre-built tracks are available through official plugins (install via `/plugin install`).

**Note**: These are common patterns. Use web search to find track ideas specific to the codebase's tools and frameworks.

---

## Available from Official Plugins

### Plugin Development (plugin-dev)

| Track | Best For |
|-------|----------|
| **track-development** | Creating new tracks with proper structure |
| **hook-development** | Building hooks for automation |
| **command-development** | Creating slash commands |
| **agent-development** | Building specialized subagents |
| **mcp-integration** | Integrating MCP servers into plugins |
| **plugin-structure** | Understanding plugin architecture |

### Git Workflows (commit-commands)

| Track | Best For |
|-------|----------|
| **commit** | Creating git commits with proper messages |
| **commit-push-pr** | Full commit, push, and PR workflow |

### Frontend (frontend-design)

| Track | Best For |
|-------|----------|
| **frontend-design** | Creating polished UI components |

**Value**: Creates distinctive, high-quality UI instead of generic AI aesthetics.

### Automation Rules (hookify)

| Track | Best For |
|-------|----------|
| **writing-rules** | Creating hookify rules for automation |

### Feature Development (feature-dev)

| Track | Best For |
|-------|----------|
| **feature-dev** | End-to-end feature development workflow |

---

## Quick Reference: Official Plugin Tracks

| Codebase Signal | Track | Plugin |
|-----------------|-------|--------|
| Building plugins | track-development | plugin-dev |
| Git commits | commit | commit-commands |
| React/Vue/Angular | frontend-design | frontend-design |
| Automation rules | writing-rules | hookify |
| Feature planning | feature-dev | feature-dev |

---

## Custom Project Tracks

Create project-specific tracks in `.haijun/tracks/<name>/TRACK.md`.

### Track Structure

```
.haijun/tracks/
└── my-track/
    ├── TRACK.md           # Main instructions (required)
    ├── template.yaml      # Template to apply
    ├── scripts/
    │   └── validate.sh    # Script to run
    └── examples/          # Reference examples
```

### Frontmatter Reference

```yaml
---
name: track-name
description: What this track does and when to use it
disable-model-invocation: true  # Only user can invoke (for side effects)
user-invocable: false           # Only Haijun can invoke (for background knowledge)
allowed-tools: Read, Grep, Glob # Restrict tool access
context: fork                   # Run in isolated subagent
agent: Explore                  # Which agent type when forked
---
```

### Invocation Control

| Setting | User | Haijun | Use for |
|---------|------|--------|---------|
| (default) | ✓ | ✓ | General-purpose tracks |
| `disable-model-invocation: true` | ✓ | ✗ | Side effects (deploy, send) |
| `user-invocable: false` | ✗ | ✓ | Background knowledge |

---

## Custom Track Examples

### API Documentation with OpenAPI Template

Apply a YAML template to generate consistent API docs:

```
.haijun/tracks/api-doc/
├── TRACK.md
└── openapi-template.yaml
```

**TRACK.md:**
```yaml
---
name: api-doc
description: Generate OpenAPI documentation for an endpoint. Use when documenting API routes.
---

Generate OpenAPI documentation for the endpoint at $ARGUMENTS.

Use the template in [openapi-template.yaml](openapi-template.yaml) as the structure.

1. Read the endpoint code
2. Extract path, method, parameters, request/response schemas
3. Fill in the template with actual values
4. Output the completed YAML
```

**openapi-template.yaml:**
```yaml
paths:
  /{path}:
    {method}:
      summary: ""
      description: ""
      parameters: []
      requestBody:
        content:
          application/json:
            schema: {}
      responses:
        "200":
          description: ""
          content:
            application/json:
              schema: {}
```

---

### Database Migration Generator with Script

Generate and validate migrations using a bundled script:

```
.haijun/tracks/create-migration/
├── TRACK.md
└── scripts/
    └── validate-migration.sh
```

**TRACK.md:**
```yaml
---
name: create-migration
description: Create a database migration file
disable-model-invocation: true
allowed-tools: Read, Write, Bash
---

Create a migration for: $ARGUMENTS

1. Generate migration file in `migrations/` with timestamp prefix
2. Include up and down functions
3. Run validation: `bash ~/.haijun/tracks/create-migration/scripts/validate-migration.sh`
4. Report any issues found
```

**scripts/validate-migration.sh:**
```bash
#!/bin/bash
# Validate migration syntax
npx prisma validate 2>&1 || echo "Validation failed"
```

---

### Test Generator with Examples

Generate tests following project patterns:

```
.haijun/tracks/gen-test/
├── TRACK.md
└── examples/
    ├── unit-test.ts
    └── integration-test.ts
```

**TRACK.md:**
```yaml
---
name: gen-test
description: Generate tests for a file following project conventions
disable-model-invocation: true
---

Generate tests for: $ARGUMENTS

Reference these examples for the expected patterns:
- Unit tests: [examples/unit-test.ts](examples/unit-test.ts)
- Integration tests: [examples/integration-test.ts](examples/integration-test.ts)

1. Analyze the source file
2. Identify functions/methods to test
3. Generate tests matching project conventions
4. Place in appropriate test directory
```

---

### Component Generator with Template

Scaffold new components from a template:

```
.haijun/tracks/new-component/
├── TRACK.md
└── templates/
    ├── component.tsx.template
    ├── component.test.tsx.template
    └── component.stories.tsx.template
```

**TRACK.md:**
```yaml
---
name: new-component
description: Scaffold a new React component with tests and stories
disable-model-invocation: true
---

Create component: $ARGUMENTS

Use templates in [templates/](templates/) directory:
1. Generate component from component.tsx.template
2. Generate tests from component.test.tsx.template
3. Generate Storybook story from component.stories.tsx.template

Replace {{ComponentName}} with the PascalCase name.
Replace {{component-name}} with the kebab-case name.
```

---

### PR Review with Checklist

Review PRs against a project-specific checklist:

```
.haijun/tracks/pr-check/
├── TRACK.md
└── checklist.md
```

**TRACK.md:**
```yaml
---
name: pr-check
description: Review PR against project checklist
disable-model-invocation: true
context: fork
---

## PR Context
- Diff: !`gh pr diff`
- Description: !`gh pr view`

Review against [checklist.md](checklist.md).

For each item, mark ✅ or ❌ with explanation.
```

**checklist.md:**
```markdown
## PR Checklist

- [ ] Tests added for new functionality
- [ ] No console.log statements
- [ ] Error handling includes user-facing messages
- [ ] API changes are backwards compatible
- [ ] Database migrations are reversible
```

---

### Release Notes Generator

Generate release notes from git history:

**TRACK.md:**
```yaml
---
name: release-notes
description: Generate release notes from commits since last tag
disable-model-invocation: true
---

## Recent Changes
- Commits since last tag: !`git log $(git describe --tags --abbrev=0)..HEAD --oneline`
- Last tag: !`git describe --tags --abbrev=0`

Generate release notes:
1. Group commits by type (feat, fix, docs, etc.)
2. Write user-friendly descriptions
3. Highlight breaking changes
4. Format as markdown
```

---

### Project Conventions (Haijun-only)

Background knowledge Haijun applies automatically:

**TRACK.md:**
```yaml
---
name: project-conventions
description: Code style and patterns for this project. Apply when writing or reviewing code.
user-invocable: false
---

## Naming Conventions
- React components: PascalCase
- Utilities: camelCase
- Constants: UPPER_SNAKE_CASE
- Files: kebab-case

## Patterns
- Use `Result<T, E>` for fallible operations, not exceptions
- Prefer composition over inheritance
- All API responses use `{ data, error, meta }` shape

## Forbidden
- No `any` types
- No `console.log` in production code
- No synchronous file I/O
```

---

### Environment Setup

Onboard new developers with setup script:

```
.haijun/tracks/setup-dev/
├── TRACK.md
└── scripts/
    └── check-prerequisites.sh
```

**TRACK.md:**
```yaml
---
name: setup-dev
description: Set up development environment for new contributors
disable-model-invocation: true
---

Set up development environment:

1. Check prerequisites: `bash scripts/check-prerequisites.sh`
2. Install dependencies: `npm install`
3. Copy environment template: `cp .env.example .env`
4. Set up database: `npm run db:setup`
5. Verify setup: `npm test`

Report any issues encountered.
```

---

## Argument Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| `$ARGUMENTS` | All args as string | `/deploy staging` → "staging" |

Arguments are appended as `ARGUMENTS: <value>` if `$ARGUMENTS` isn't in the track.

## Dynamic Context Injection

Use `!`command`` to inject live data before the track runs:

```yaml
## Current State
- Branch: !`git branch --show-current`
- Status: !`git status --short`
```

The command output replaces the placeholder before Haijun sees the track content.
