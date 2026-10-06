---
description: Update HAIJUN.md with learnings from this session
allowed-tools: Read, Edit, Glob
---

Review this session for learnings about working with Haijun Code in this codebase. Update HAIJUN.md with context that would help future Haijun sessions be more effective.

## Step 1: Reflect

What context was missing that would have helped Haijun work more effectively?
- Bash commands that were used or discovered
- Code style patterns followed
- Testing approaches that worked
- Environment/configuration quirks
- Warnings or gotchas encountered

## Step 2: Find HAIJUN.md Files

```bash
find . -name "HAIJUN.md" -o -name ".haijun.local.md" 2>/dev/null | head -20
```

Decide where each addition belongs:
- `HAIJUN.md` - Team-shared (checked into git)
- `.haijun.local.md` - Personal/local only (gitignored)

## Step 3: Draft Additions

**Keep it concise** - one line per concept. HAIJUN.md is part of the prompt, so brevity matters.

Format: `<command or pattern>` - `<brief description>`

Avoid:
- Verbose explanations
- Obvious information
- One-off fixes unlikely to recur

## Step 4: Show Proposed Changes

For each addition:

```
### Update: ./HAIJUN.md

**Why:** [one-line reason]

\`\`\`diff
+ [the addition - keep it brief]
\`\`\`
```

## Step 5: Apply with Approval

Ask if the user wants to apply the changes. Only edit files they approve.
